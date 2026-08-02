# Putting it together: a complete scored run

To see every piece of Track C wired into one controller and run for real, with no supervisor-side
positioning help, this page brings together [sensing](code-sensors.md),
[driving](code-driving.md), [the wall-follower](code-wall-follower.md),
[seeing](code-victim-detection.md), [reporting](code-reporting.md),
[recovering](code-lack-of-progress.md), and [exiting](code-exit.md).
This is the payoff page, and it comes with an honest result, not a clean one.

---

## Step 1: What "complete" actually means here

One controller that, on its own: explores using [the wall-follower](code-wall-follower.md), watches
its camera for a victim sign using [the same detection code as
before](code-victim-detection.md), stops and reports it using
[dead-reckoned position](code-reporting.md) (tracked from wheel encoders, since nothing gives a
robot its own absolute position for free), then uses [an LoP relocate](code-lack-of-progress.md) to
get back to the start tile and [exits](code-exit.md). No step teleports the robot anywhere, unlike
every earlier page in this series, which used the trial harness's supervisor access to start
already positioned next to something. This page is where that crutch comes off.

---

## Step 2: Write the controller

```python
from controller import Robot
import cv2
import numpy as np
import struct
import math

robot = Robot()
timeStep = 32

wheel1 = robot.getDevice("wheel1 motor")
wheel2 = robot.getDevice("wheel2 motor")
wheel1.setPosition(float("inf"))
wheel2.setPosition(float("inf"))

ps0 = robot.getDevice("ps0")
ps7 = robot.getDevice("ps7")
ps2 = robot.getDevice("ps2")
for s in (ps0, ps7, ps2):
	s.enable(timeStep)

wheel1_sensor = robot.getDevice("wheel1 sensor")
wheel2_sensor = robot.getDevice("wheel2 sensor")
wheel1_sensor.enable(timeStep)
wheel2_sensor.enable(timeStep)

camera = robot.getDevice("camera_centre")
camera.enable(timeStep)

emitter = robot.getDevice("emitter")
receiver = robot.getDevice("receiver")
receiver.enable(timeStep)

MAX_VELOCITY = 6.28
FRONT_BLOCKED = 0.15
WALL_TOO_CLOSE = 0.05
WALL_LOST = 0.5
WHEEL_RADIUS = 0.02
TRACK = 0.052

# This world's real start-tile coordinate. Measured once, the same way you'd read your own
# wheel geometry off the robot's proto file, not something this code discovers on its own.
START_X, START_Z = -0.48, -0.48


def detectVisualSimple(image_data, camera):
	coords_list = []
	img = np.array(np.frombuffer(image_data, np.uint8).reshape((camera.getHeight(), camera.getWidth(), 4)))
	img[:, :, 2] = np.zeros([img.shape[0], img.shape[1]])
	gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
	thresh = cv2.threshold(gray, 140, 255, cv2.THRESH_BINARY)[1]
	contours, h = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
	for c in contours:
		if cv2.contourArea(c) > 150:
			coords_list.append(list(c[0][0]))
	return coords_list


step = 0
stuck_steps = 0
escape_steps_left = 0
escape_phase = "reverse"
prev_l = None
prev_r = None
odom_x, odom_z, theta = 0.0, 0.0, 0.0

state = "explore"       # explore -> stopped_for_report -> heading_home
stop_start_time = None
reported = False
sent_lop = False
sent_exit = False
last_score = None
home_time = None
last_g_time = 0.0

while robot.step(timeStep) != -1:
	step += 1
	now = robot.getTime()

	l = wheel1_sensor.getValue()
	r = wheel2_sensor.getValue()
	if prev_l is not None:
		dl = WHEEL_RADIUS * (l - prev_l)
		dr = WHEEL_RADIUS * (r - prev_r)
		dcenter = (dl + dr) / 2
		dtheta = (dr - dl) / TRACK
		theta += dtheta
		odom_x += dcenter * math.cos(theta)
		odom_z += dcenter * math.sin(theta)
	prev_l, prev_r = l, r

	front_blocked = ps0.getValue() < FRONT_BLOCKED or ps7.getValue() < FRONT_BLOCKED

	if state == "explore":
		detections = detectVisualSimple(camera.getImage(), camera)
		if detections and stuck_steps == 0 and escape_steps_left == 0:
			wheel1.setVelocity(0)
			wheel2.setVelocity(0)
			state = "stopped_for_report"
			stop_start_time = now
			print(f"t={now:.1f}s SIGN DETECTED, stopping (odom est so far: "
			      f"x={START_X + odom_x:.3f} z={START_Z + odom_z:.3f})")
		elif escape_steps_left > 0:
			if escape_phase == "reverse":
				wheel1.setVelocity(-MAX_VELOCITY / 2)
				wheel2.setVelocity(-MAX_VELOCITY / 2)
			else:
				wheel1.setVelocity(MAX_VELOCITY)
				wheel2.setVelocity(-MAX_VELOCITY)
			escape_steps_left -= 1
			if escape_steps_left == 0 and escape_phase == "reverse":
				escape_phase = "turn"
				escape_steps_left = 35
			elif escape_steps_left == 0:
				escape_phase = "reverse"
				stuck_steps = 0
		else:
			if front_blocked:
				stuck_steps += 1
			else:
				stuck_steps = 0
			if stuck_steps > 12:
				escape_steps_left = 20
				escape_phase = "reverse"
			elif front_blocked:
				wheel1.setVelocity(-MAX_VELOCITY / 2)
				wheel2.setVelocity(MAX_VELOCITY / 2)
			else:
				right = ps2.getValue()
				if right < WALL_TOO_CLOSE:
					wheel1.setVelocity(MAX_VELOCITY / 3)
					wheel2.setVelocity(MAX_VELOCITY)
				elif right > WALL_LOST:
					wheel1.setVelocity(MAX_VELOCITY)
					wheel2.setVelocity(MAX_VELOCITY / 3)
				else:
					wheel1.setVelocity(MAX_VELOCITY)
					wheel2.setVelocity(MAX_VELOCITY)

	elif state == "stopped_for_report":
		wheel1.setVelocity(0)
		wheel2.setVelocity(0)
		if now - stop_start_time > 2.0 and not reported:
			est_x_cm = int(round((START_X + odom_x) * 100))
			est_z_cm = int(round((START_Z + odom_z) * 100))
			emitter.send(struct.pack('i i c', est_x_cm, est_z_cm, 'U'.encode()))
			reported = True
			print(f"t={now:.1f}s SENT identification x={est_x_cm} z={est_z_cm} type=U "
			      f"(odom estimate)")
		if reported and now - stop_start_time > 3.0:
			state = "heading_home"
			home_time = now

	elif state == "heading_home":
		if not sent_lop:
			emitter.send(struct.pack('c', 'L'.encode()))
			sent_lop = True
			print(f"t={now:.1f}s SENT LoP request (to get back to the start tile before exiting)")
		elif now - home_time > 2.0 and not sent_exit:
			emitter.send(struct.pack('c', 'E'.encode()))
			sent_exit = True
			print(f"t={now:.1f}s SENT exit")

	if receiver.getQueueLength() > 0:
		data = receiver.getBytes()
		if len(data) == 1:
			tup = struct.unpack('c', data)
			if tup[0].decode() == 'L':
				print(f"t={now:.1f}s RECEIVED LoP acknowledgement")
		elif len(data) == 16:
			tup = struct.unpack('c f i i', data)
			if tup[0].decode() == 'G' and tup[1] != last_score:
				print(f"t={now:.1f}s score={tup[1]}")
				last_score = tup[1]
		receiver.nextPacket()

	if now - last_g_time > 1:
		emitter.send(struct.pack('c', 'G'.encode()))
		last_g_time = now
```

`START_X`/`START_Z` is this world's real starting coordinate, found once by checking where the
robot actually ends up (the same kind of one-time measurement as the wheel geometry Track C has
used since [driving](code-driving.md)), not something the controller works out for itself. The
`odom_x`/`odom_z` accumulation is standard differential-drive dead reckoning: every timestep, how
far each wheel turned, converted to how far the robot's centre moved and how much it rotated.

---

## Step 3: Two real runs, and an honest gap between them

!!! success "Run 1: fully autonomous, 90 seconds, no help at all"
    ```
    t=1.6s score=0.0
    ```
    That's the *entire* controller output over 90 real simulated seconds. Position telemetry (our
    own verification tooling, not visible to a real controller) tells the rest of the story: the
    robot left the start tile, made real progress for about 14 seconds, then settled into a tight
    repeating loop, bouncing between the same few positions for the remaining 70-plus seconds,
    never getting close enough to any victim sign for the camera to see one. Final score: **`0.0`**.
    This is the same limitation [the wall-follower's own page](code-wall-follower.md) already
    admitted to, confirmed again here with the full pipeline attached: exploration is this robot's
    weakest link.

!!! success "Run 2: started next to a known sign, everything else identical"
    ```
    t=1.7s score=0.0
    t=5.0s SIGN DETECTED, stopping (odom est so far: x=0.182 z=0.107)
    t=7.0s SENT identification x=18 z=11 type=U (odom estimate)
    t=8.1s SENT LoP request (to get back to the start tile before exiting)
    t=8.1s RECEIVED LoP acknowledgement
    t=10.0s SENT exit
    ```
    Here, staged next to the same sign [the victim detection page](code-victim-detection.md) used
    (a testing convenience, not something this controller can do on its own), the full pipeline runs
    end to end exactly as designed: it sees the sign, stops, dead-reckons a position, reports,
    relocates, exits. But the reported estimate, `x=18 z=11`, is off by about **12 cm** from the
    sign's real position (`x=6 z=13`, [confirmed back on the reporting page](code-reporting.md)),
    outside [the 0.09 m identification radius](code-reporting.md#about-that-identification-range).
    Final score: **`0.0`** again — a misidentification (the robot sent the wrong position, so it scored nothing, but it didn't crash or hang).
    Record: `trials/20260730-095138-C12-boosted-v2.json`.

### What this means

Every individual piece of Track C works, verified on its own page with real, non-zero score
changes: [reporting](code-reporting.md) (`0.0 → 22.5`), [LoP](code-lack-of-progress.md)
(`22.5 → 17.5`), [the exit bonus](code-exit.md) (`17.5 → 19.25`), [the map bonus](code-mapping.md)
(`19.25 → 41.65`).

Wired together into one autonomous controller, however, this robot still doesn't score,
for two compounding reasons, in order: it can't reliably explore enough of the maze to find a sign
in the first place, and even when handed a sign for free, five seconds of real wheel movement is
already enough dead-reckoning drift to miss the identification radius. Therefore, both are real,
unsolved problems on this site. Getting past them is exactly what [Track
S](rules-scoring.md) exists to think through; better exploration and better position estimates are
strategy questions.

---

## Now make it your own

- To improve your dead-reckoning accuracy, try recalibrating position more often than "never" (this
  controller only ever dead-reckons from one fixed reference), for instance resetting to a known
  value every time [a checkpoint tile is crossed](rules-scoring.md), if you can detect that reliably.
- Additionally, try a smarter exploration strategy than [the existing wall-follower](code-wall-follower.md). Even
  a modest improvement in how much of the maze gets covered directly improves how often a sign is
  ever seen at all.
- Measure your own dead-reckoning drift the way this page did: report a position you know is wrong
  by a deliberately introduced amount, and see exactly how far off is still "close enough."

---

## If it goes wrong

- **Your combined controller finds nothing over a long run, like Run 1 here.** Not necessarily a
  bug, confirm via your own printed status lines (or accept the same limitation this page found)
  before assuming something is broken.
- **A sign is detected but the report never scores, like Run 2 here.** Check your dead-reckoning
  math before assuming the report format itself is wrong, [the last page](code-reporting.md)
  already confirmed the format and radius both work correctly given an accurate position.
- **The two runs give very different amounts of exploration before getting stuck.** That's
  consistent with what [the wall-follower page](code-wall-follower.md) found, small differences in
  exactly when a stuck-recovery kicks in can compound into meaningfully different coverage.

---

Next: [spending your eight minutes](strategy-run-budget.md).
