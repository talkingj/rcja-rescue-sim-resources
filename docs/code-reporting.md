# Reporting a victim and earning your first points

To turn the sign your robot can already spot ([last page](code-victim-detection.md)) into actual
score, this page shows you how to report it to the supervisor. It takes about 20 minutes and is
the first page in this series where the number in the top corner of the Competition Controller
moves because of something your code did.

!!! note "Not adapted from a sample this time"
    There's no official `report_victim.py` sample to start from. The message format below was read
    directly out of the Erebus supervisor's own source (`Robot.py`'s `set_message`, `MainSupervisor.py`'s
    `_detect_victim`), the same way you'd have to if you were writing this for real. The game-info
    query is the one part that does come from an official sample, `GetGameInfo.py`.

---

## Step 1: Two message shapes, one emitter

Your robot only has one way to talk to the supervisor: `emitter.send(bytes)`. What the supervisor
*does* with those bytes depends entirely on how many bytes you send.

- **1 byte, the letter `G`**, a game-info request. The supervisor replies (via your `receiver`)
  with 16 bytes: `struct.pack("c f i i", b'G', score, time_left, real_time_left)`.
- **9 bytes: two ints and a char**, a victim or hazmat report. `struct.pack('i i c', x_cm, z_cm,
  type_char)`. The two ints are your **estimated position in whole centimetres**, not metres — that's
  the detail most likely to trip you up. The char is the type: `'H'`/`'U'`/`'S'` for a harmed,
  unharmed, or stable victim, or one of the hazmat letters for a cognitive target.

There's a third shape (map data, covered in a later page) and single control bytes like `'E'`
(exit) and `'L'` (relocate me), but those aren't this page's concern.

The supervisor only acts on a 9-byte report if your robot has been **stationary for at least one
full second** when it arrives. Send it while still moving and it's silently ignored, not queued,
not erroring, just dropped.

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

sent_correct = False
sent_wrong = False
step = 0

while robot.step(timeStep) != -1:
	step += 1
	t = step * timeStep / 1000

	if 1.0 < t < 1.5:
		emitter.send(struct.pack('c', 'G'.encode()))

	if 2.0 < t < 2.05 and not sent_correct:
		emitter.send(struct.pack('i i c', 6, 13, 'U'.encode()))
		sent_correct = True
		print("SENT identification x=6 z=13 type=U")

	if 3.0 < t < 3.5:
		emitter.send(struct.pack('c', 'G'.encode()))

	if 4.0 < t < 4.05 and not sent_wrong:
		emitter.send(struct.pack('i i c', 200, 200, 'U'.encode()))
		sent_wrong = True
		print("SENT misidentification x=200 z=200 type=U")

	if 5.0 < t < 5.5:
		emitter.send(struct.pack('c', 'G'.encode()))

	if receiver.getQueueLength() > 0:
		data = receiver.getBytes()
		if len(data) == 16:
			tup = struct.unpack('c f i i', data)
			if tup[0].decode() == 'G':
				print(f"t={t:.1f}s SCORE={tup[1]}")
		receiver.nextPacket()
```

This controller doesn't drive anywhere, it starts already stopped next to a known victim (more on
that in a moment) and does three things on a timer: asks for the score, reports a victim correctly,
asks again, reports a deliberately wrong location, asks a third time.

---

## Step 3: A real run, score before and after

To see the score change in real time, the controller queries the game info three times — once before
any report, once after a correct one, and once after a deliberate misidentification.

!!! success "You should now see"
    Starting score, queried before any report:

    ```
    t=1.1s SCORE=0.0
    ```

    After the correct report (`x=6 z=13 type=U`, matching a real `unharmed` victim worth 5 points at
    this exact spot):

    ```
    SENT identification x=6 z=13 type=U
    t=3.0s SCORE=22.5
    ```

    After the deliberately wrong report (`x=200 z=200`, nowhere near any real victim):

    ```
    SENT misidentification x=200 z=200 type=U
    t=5.1s SCORE=17.5
    ```

    `0.0 → 22.5 → 17.5`. Full record: `trials/20260730-082558-C7.json`.

### Where 22.5 comes from

Not a round number, and that's worth unpacking rather than skipping past. Per
[the scoring page](rules-scoring.md#point-values), a correctly-identified linear-tile victim is
worth **5 points** (TI) plus **10 points** (TT) for getting the type right too, 15 raw points.
The robot scored 22.5, which is `15 × 1.5`, as this particular victim sits in a spot the supervisor
scores with a **×1.5 area multiplier** — the same multiplier
[the scoring page](rules-scoring.md#area-multipliers) lists for Area 3. Nothing about `world1`
announces which room counts as which area. We only found this by reading the actual score change.
Therefore, *where* you find a victim can matter as much as which one it is.

The misidentification afterwards cost a flat **−5**, no multiplier, matching the scoring page's note
that TMI doesn't scale with area.

### How the robot got next to a real victim

Same limitation as [the last page](code-victim-detection.md): reliable navigation to a specific
victim isn't solved yet on this site. Additionally, this controller starts already in position,
staged there by the trial harness's supervisor access (an environment variable the harness sets
before the match starts), not by anything the controller itself does. The code above is exactly what ran and sent
the reports, the positioning step is a testing convenience, not a technique your controller has
access to in a real round.

### About that identification range

To understand how close your reported coordinate needs to be, you need to know the supervisor
checks two things before accepting a report as correct: your robot has to be
physically near the real victim, *and* your reported coordinate has to be near it too. Both checks
Both checks use the same fixed radius, read straight from the source: **0.09 m**.
[The scoring page](rules-scoring.md#identifying-a-token-correctly) describes this in the rules'
own words as "half a tile." However, this page's number is the one actually enforced in code —
use `0.09` if you're writing detection logic and want to know exactly how close is close enough.

---

## Now make it your own

- Try sending a report with the right position but the wrong type letter (`'H'` instead of `'U'`,
  say). You should still get the 5-point TI bonus, just not the 10-point TT bonus, misidentification
  is about *location*, not type.
- Try sending two correct reports for the same victim in a row. The scoring page's "no duplicate
  rewards" rule means the second one shouldn't add anything, worth confirming for yourself rather
  than taking that on faith.
- Combine this with [victim detection](code-victim-detection.md): once `detectVisualSimple` returns
  a coordinate, what turns that pixel position into the centimetre estimate this page's message
  needs? That's real dead-reckoning work, and a future page's job, not this one's.

---

## If it goes wrong

- **The score never changes, no error either.** Almost always means the report arrived while the
  robot was still notionally "moving." Double check your wheels are actually at `setVelocity(0)`,
  not just slow, and that you've waited a full second past that before sending.
- **You get 0 points instead of the full type bonus.** Check your type character against the exact
  letters this page uses (`H`/`U`/`S`), a lowercase or a typo silently just won't match.
- **`struct.pack('i i c', ...)` raises an error about too many/few arguments.** The format string
  needs exactly two ints and one single-character bytes object, in that order, a Python `str` where
  it wants `bytes` (missing `.encode()`) is the usual cause.

---

Next: [reading the clock and the score](code-game-info.md).
