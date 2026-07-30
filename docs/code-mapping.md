# Building and submitting the map matrix

The hardest page in this series, not because the code is complex, but because the matrix it sends
is the most detailed data structure anywhere on this site. This page adapts the official
`MapScorerExample.py` sample, which ships with something invaluable: a complete, ready-made answer
matrix for `world1`, the same world every page in this series has used. About 20 minutes, most of it
reading, not typing.

!!! note "Credit where it's due"
    The submission code and the entire map matrix on this page are the official Erebus
    `MapScorerExample.py` sample's own commented-out `world1.wbt` test array, used as shipped. This
    page's job is explaining what it means and what happens when you send it, per [the map format
    page](rules-map-format.md).

---

## Step 1: What you're actually sending

[The map format page](rules-map-format.md) already covers the quarter-tile encoding in detail, one
character per cell, `0` empty, `1` wall, `5` starting tile, and so on. This page is about the
mechanics of sending it: three separate emitter messages, in order.

1. **The matrix itself**, as `shape_bytes + flattened_data_bytes`. The shape is two ints (rows,
   columns) packed with `struct.pack('2i', *s)`. The data is every cell joined with commas into one
   string, then UTF-8 encoded, no shape information inside it, that's what the first 8 bytes are for.
2. **A single byte, `'M'`**, telling the supervisor "score the map I just sent."
3. **Later, `'E'`** (exit, [from the last page](code-exit.md)). The map bonus isn't applied when you
   send `'M'`, it's applied when you exit, as a multiplier on your score at that moment, not as
   points added on the spot.

---

## Step 2: Write the controller

```python
from controller import Robot
import numpy as np
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

# The official MapScorerExample.py's own ready-made answer matrix for world1.wbt.
subMatrix = np.array([
    ['1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1'],
    ['1', '5', '0', '5', '0', '0', '0', '0', '0', '0', '0', '0', '0', '2', '0', '2', '1', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '3', '0', '3', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '5', '0', '5', '0', '0', '0', '0', '0', '0', '0', '0', '0', '2', '0', '2', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '3', '0', '3', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', 'S'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '1', '1', '1', '1', '1', '0', '0', '0', '1', '1', '1', '1', '1', '0', '0', '0', '0', '0', '0', '0', '1', '1', '1', '0', '0', '0', '0', '0', '1'],
    ['F', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', 'b', '0', 'b', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', 'b', '0', 'b', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '1', '1', '1', '1', '1', '1', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', 'p', '0', 'p', '1', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', 'p', '0', 'p', '1', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '1', '1', '1', '1', '1', '1', '1', '1', '0', '0', '0', '1', '1', '1', '1', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '1', '1', '1', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', 'U', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '1', '1', '1', 'H', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '1', '1', '1', '0', '0', '0', '0', '1'],
    ['P', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '4', '0', '4', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '4', '0', '4', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '1'],
    ['1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1', '1']
])

last_request_time = robot.getTime()
sent_report = False
sent_map = False
sent_eval = False
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

	if now > 9.0 and not sent_map:
		s = subMatrix.shape
		s_bytes = struct.pack('2i', *s)
		flatMap = ','.join(subMatrix.flatten())
		sub_bytes = flatMap.encode('utf-8')
		emitter.send(s_bytes + sub_bytes)
		sent_map = True
		print(f"t={now:.1f}s SENT map data, shape={s}")

	if now > 10.0 and not sent_eval:
		emitter.send(struct.pack('c', 'M'.encode()))
		sent_eval = True
		print(f"t={now:.1f}s SENT map evaluate request")

	if now > 14.0 and not sent_lop:
		emitter.send(struct.pack('c', 'L'.encode()))
		sent_lop = True
		print(f"t={now:.1f}s SENT LoP request (to get back onto the start tile)")

	if now > 18.0 and not sent_exit:
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

The full 33×33 matrix above, all 1089 cells, is exactly what the trial ran, copied unedited from the
official sample. This controller reports one victim first (so there's a base score for the map
bonus to multiply), then the map, then uses [the LoP relocation](code-lack-of-progress.md) to get
back onto the start tile before exiting, [same trick as the last page](code-exit.md).

---

## Step 3: A real run, mapping bonus included

!!! success "You should now see"
    ```
    t=2.6s score=0.0
    t=5.0s SENT identification x=6 z=13 type=U
    t=5.7s score=22.5
    t=9.0s SENT map data, shape=(33, 33)
    t=10.0s SENT map evaluate request
    t=14.0s SENT LoP request (to get back onto the start tile)
    t=14.0s RECEIVED LoP acknowledgement
    t=14.9s score=17.5
    t=18.0s SENT exit
    ```

    Controller output stops there, same as [the last page](code-exit.md), the match ends at exit.
    The trial harness's independent score feed confirms the final total: **`41.65`**. Working
    backwards: the exit bonus takes `17.5` to `19.25` (`× 1.1`, [as before](code-exit.md)), then the
    map bonus takes `19.25` to `41.65`. That's a *lot* more than the exit bonus alone, solving the
    algebra (`41.65 = 19.25 + 19.25 × correctness × 1.2`) says this exact official matrix scored
    about **97% correctness** against `world1`'s real internal answer, not a perfect 100%. The
    official sample array is a close match for this world, not a guaranteed-exact one, worth
    remembering if you're tempted to copy a "known good" map wholesale into a real competition
    world, small differences between what ships as a test aid and the actual scoring key are real,
    not hypothetical. Full record: `trials/20260730-085222-C11.json`.

---

## Now make it your own

- Submit an empty matrix (every cell `'0'`) and confirm the map bonus really is zero, not negative,
  [the map format page](rules-map-format.md) and [the scoring page](rules-scoring.md) both describe
  it as "no penalty for trying."
- Change one correct wall cell (`'1'`) to `'0'` in a copy of this matrix, resend, and see how much
  the score drops. That tells you roughly how much a single mapping mistake costs on a matrix this
  size.
- Cross-reference a few cells of this matrix against [the map format page's](rules-map-format.md)
  encoding table by hand. Building your own matrix during a real round means reading your robot's
  own sensor history into this exact shape, this page only covers submitting one that's already
  built.

---

## If it goes wrong

- **`'M'` seems to do nothing.** The map bonus is applied at exit, not immediately, [same lag in
  spirit as the score-update delay from a few pages ago](code-game-info.md). Query the score after
  exiting, not right after `'M'`.
- **You send the matrix and get a supervisor-side error instead of a score.** Almost always a shape
  mismatch, the two ints in `s_bytes` have to exactly match the actual dimensions of the flattened
  data that follows, or an encoding issue if you've edited the array by hand and introduced a
  non-string cell.
- **Your own hand-built matrix scores much lower than this page's.** That's normal for a first
  attempt, not a bug, this page's matrix is the official shipped answer key for this exact world,
  not a bar to expect to clear immediately.

---

Next: [putting it together, a complete scored run](code-complete-run.md).
