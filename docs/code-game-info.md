# Reading the clock and the score

Every page so far has had you guess at your score by reasoning about the rules. This page adapts
the official `GetGameInfo.py` sample so your own controller can ask the supervisor directly: what's
my score right now, and how much time is actually left. It takes about 10 minutes.

!!! note "Credit where it's due"
    This page's polling loop is the official Erebus `GetGameInfo.py` sample, essentially unchanged.
    The only additions are the identification report ([from the last page](code-reporting.md)) used
    to give the score field something to do, and only printing when a value actually changes.

---

## Step 1: One request, three numbers back

Send the single byte `'G'` on your emitter, and the supervisor replies on your receiver with 16
bytes: `struct.pack("c f i i", b'G', score, game_time_left, real_time_left)`. Three numbers, all
live:

- **`score`**, your current total, the exact number the Competition Controller shows.
- **`game_time_left`**, whole seconds left in the match clock.
- **`real_time_left`**, whole seconds left on a *separate*, longer real-world clock. This page's
  trial found it starts around **600s (10 minutes)** against a match clock starting around **480s
  (8 minutes)**, the extra buffer is presumably there to absorb lag or a paused match, not something
  a normal run should ever hit.

There's no fourth field for "have I called exit yet" or similar, whatever this page's title implies
about "exit state", the message itself only ever carries these three numbers. If you need to know
whether you've already sent an exit, that's state your own controller has to track.

---

## Step 2: Write the controller

```python
from controller import Robot
import struct

robot = Robot()
timeStep = 32

emitter = robot.getDevice("emitter")
receiver = robot.getDevice("receiver")
receiver.enable(timeStep)

wheel1 = robot.getDevice("wheel1 motor")
wheel2 = robot.getDevice("wheel2 motor")
wheel1.setPosition(float("inf"))
wheel2.setPosition(float("inf"))
wheel1.setVelocity(0)
wheel2.setVelocity(0)

last_request_time = robot.getTime()
sent_report = False
last_printed = None

while robot.step(timeStep) != -1:
	now = robot.getTime()

	if now - last_request_time > 1:
		emitter.send(struct.pack('c', 'G'.encode()))
		last_request_time = now

	if now > 4.0 and not sent_report:
		emitter.send(struct.pack('i i c', 6, 13, 'U'.encode()))
		sent_report = True
		print(f"t={now:.1f}s SENT identification x=6 z=13 type=U")

	if receiver.getQueueLength() > 0:
		data = receiver.getBytes()
		if len(data) == 16:
			tup = struct.unpack('c f i i', data)
			if tup[0].decode() == 'G':
				reading = (tup[1], tup[2], tup[3])
				if reading != last_printed:
					print(f"t={now:.1f}s score={tup[1]} game_time_left={tup[2]} "
					      f"real_time_left={tup[3]}")
					last_printed = reading
		receiver.nextPacket()
```

Like [the last page](code-reporting.md), this controller starts already stopped next to a known
victim (staged by the trial harness, not the controller) so there's something real for the score
field to report on partway through the run.

---

## Step 3: A real run, start to mid-run

!!! success "You should now see"
    ```
    t=2.0s score=0.0 game_time_left=479 real_time_left=599
    t=3.0s score=0.0 game_time_left=478 real_time_left=598
    t=4.0s SENT identification x=6 z=13 type=U
    t=4.1s score=0.0 game_time_left=477 real_time_left=597
    t=5.1s score=22.5 game_time_left=476 real_time_left=596
    ```

    Two things worth noticing. First, `game_time_left` counts down by roughly 1 each second, exactly
    as you'd expect from an 8-minute match clock. Second, `score` does **not** update instantly: the
    poll at `t=4.1s`, one tenth of a second after the identification was sent, still reads the old
    `0.0`. It's the *next* poll, at `t=5.1s`, that shows `22.5`. The supervisor processes your report
    and your game-info request as separate messages in its own loop, and there's no guarantee they
    land in the same simulation step. If your code ever needs to react to a score change the instant
    it happens, polling once a second like this sample does isn't fast enough, you'd need to poll
    every timestep instead.

---

## Now make it your own

- Poll every timestep instead of once a second (drop the `now - last_request_time > 1` check
  entirely) and see how much sooner the updated score shows up after a report.
- Print `real_time_left - game_time_left` once at the start. It won't be zero, now you know by how
  much the two clocks actually differ on this build.
- Combine this with the exit message (a later page's topic): once `game_time_left` gets close to
  zero, what should your controller do differently?

---

## If it goes wrong

- **You only ever see `score=0.0`, even after a report you know should score.** Check the report
  itself first ([the last page's](code-reporting.md) "if it goes wrong" section), a `'G'` reply
  can only ever repeat back a score the supervisor has already accepted.
- **`struct.unpack('c f i i', data)` raises a size error.** The reply is fixed at 16 bytes
  (1 + 4 + 4 + 4, with 3 bytes of struct padding after the leading char). If you see a different
  length, you're very likely reading a reply to a different request type, not this one.
- **The printed time never seems to change.** This page's `last_printed` check only prints when a
  value is different from the previous poll, if your game clock is somehow frozen (paused match,
  for instance) that's expected, not a bug in your code.

---

Next: [getting stuck, and recovering from it](code-lack-of-progress.md).
