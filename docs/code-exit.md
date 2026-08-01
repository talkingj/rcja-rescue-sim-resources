# Sending the exit message

To see for yourself what happens when you meet the exit bonus conditions — and what doesn't when
you don't — this page adapts the official `exit_test.py` sample, the shortest one in the whole
official sample set, and pairs it with [the scoring page's](rules-scoring.md) two conditions. About 10
minutes.

!!! note "Credit where it's due"
    The exit message itself is the official Erebus `exit_test.py` sample, three lines, unchanged.
    Getting the robot back onto the start tile to send it reuses [the LoP
    relocation](code-lack-of-progress.md) from the last page, since that's a real game mechanic that
    happens to land you exactly there.

---

## Step 1: One byte, two conditions

`struct.pack('c', 'E'.encode())` is the entire message. What it does depends on where your robot is
and what it's done so far, per [the scoring page](rules-scoring.md#bonuses-applied-last):

- Your robot's position has to be on the **starting tile** at the moment the message arrives.
- You have to have **identified at least one token** ([a victim or a
  target](code-reporting.md)) already, anywhere, anytime before this message.

Meet both and you get a flat **+10%** on your entire score so far. Miss either one and nothing
happens — no bonus, no penalty, the match still ends. Therefore, sending `'E'` is a one-way action:
the supervisor removes your robot from the simulation and there's no going back to keep scoring
afterward.

---

## Step 2: Write the controller

```python
from controller import Robot
import struct

robot = Robot()
timeStep = 32

wheel1 = robot.getDevice("wheel1 motor")
wheel2 = robot.getDevice("wheel2 motor")
wheel1.setPosition(float("inf"))
wheel2.setPosition(float("inf"))
wheel1.setVelocity(0)
wheel2.setVelocity(0)

emitter = robot.getDevice("emitter")
receiver = robot.getDevice("receiver")
receiver.enable(timeStep)

last_request_time = robot.getTime()
sent_report = False
sent_lop = False
sent_exit = False
last_score = None

while robot.step(timeStep) != -1:
	now = robot.getTime()

	if now - last_request_time > 1:
		emitter.send(struct.pack('c', 'G'.encode()))
		last_request_time = now

	if now > 5.0 and not sent_report:
		emitter.send(struct.pack('i i c', 6, 13, 'U'.encode()))
		sent_report = True
		print(f"t={now:.1f}s SENT identification x=6 z=13 type=U")

	if now > 9.0 and not sent_lop:
		emitter.send(struct.pack('c', 'L'.encode()))
		sent_lop = True
		print(f"t={now:.1f}s SENT LoP request (to get back onto the start tile)")

	if now > 13.0 and not sent_exit:
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
```

This controller does three things in sequence, on a timer: identify a real victim (so there's a
score, and so the "has identified something" condition is met), send an LoP request (which happens
to relocate the robot back onto the start tile as a side effect), then exit. Additionally, the
identification must register before the exit is sent — the two conditions are checked at the moment
the supervisor receives `'E'`.

---

## Step 3: A real run, exit bonus included

!!! success "You should now see"
    ```
    t=2.4s score=0.0
    t=5.0s SENT identification x=6 z=13 type=U
    t=5.5s score=22.5
    t=9.0s SENT LoP request (to get back onto the start tile)
    t=9.0s RECEIVED LoP acknowledgement
    t=9.6s score=17.5
    t=13.0s SENT exit
    ```

    The controller's own polling stops right after the exit is sent (the match ends, there's nothing
    left to poll), but the supervisor's own score feed, which this site's trial harness listens to
    independently of anything the controller prints, confirms the final score: **`19.25`**. That's
    `17.5 × 1.1`, exactly the 10% exit bonus [the scoring page](rules-scoring.md#bonuses-applied-last)
    describes, applied on top of the `17.5` this run had after the identification and the LoP
    penalty. Full record: `trials/20260730-084823-C10-v2.json`.

### Getting this run clean took more patience than it looked like it should

The very first few attempts at this exact controller silently produced a score that never left
`0.0`, meaning the identification wasn't registering at all, not a display problem, an actual
non-event. Pushing every step's timing back (identification at `5.0s` instead of an earlier
attempt's `2.0s`, everything else shifted to match) fixed it consistently. The most likely
explanation: the supervisor requires a *full, uninterrupted* second of stillness before it will act
on an identification, and tiny physics settling noise right after a simulation starts can look like
motion for a moment. Building in a few extra seconds of margin before your first message, especially
right after your robot starts or gets relocated, costs you a little simulated time but avoids
exactly this kind of intermittent nothing-happened result.

---

## Now make it your own

- Send `'E'` from a position that is deliberately *not* the start tile. [The scoring
  page](rules-scoring.md#bonuses-applied-last) says nothing should happen, confirm that for
  yourself rather than taking it on faith.
- Send `'E'` before ever identifying anything. Same prediction, same exercise.
- Combine this with [game info](code-game-info.md): once `game_time_left` is running low, is exiting
  with whatever bonus you've earned better than risking the clock running out with none? That's a
  real strategic question, not a coding one, [Track S](rules-scoring.md) territory once it exists.

---

## If it goes wrong

- **The exit bonus never appears, even from the start tile.** Confirm you've actually identified
  something first, in this exact match, a plan to identify something later doesn't count.
- **You send `'E'` from what you believe is the start tile, and get nothing.** Position checks in
  this engine are exact, not approximate to the eye; if you drove there yourself rather than being
  relocated, you may simply not be as centred on the tile as you think.
- **Nothing seems to happen at all after sending `'E'`.** That's actually correct, once the match
  ends there's no further message coming back, your controller's next `robot.step()` call will start
  returning `-1` as the simulation winds the robot down.

---

Next: [building and submitting the map matrix](code-mapping.md).
