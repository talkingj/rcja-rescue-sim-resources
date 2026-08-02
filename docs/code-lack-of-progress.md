# Getting stuck, and recovering from it

To answer a question every earlier page in this series has been quietly ignoring — what actually
happens when your robot gets stuck? — this page adapts the official `LackOfProgress.py` sample. The
answer costs points either way; this page is about choosing *when* you pay that cost rather than
avoiding it entirely. About 15 minutes.

!!! note "Credit where it's due"
    The request/acknowledge pattern on this page is the official Erebus `LackOfProgress.py` sample,
    unchanged. The identification report used to give the penalty something to subtract from is
    [from two pages ago](code-reporting.md).

---

## Step 1: Lack of Progress isn't a bug report — it's a relocation

"LoP" happens in exactly three situations, and only one of them is something your controller
triggers on purpose:

- **You send the single byte `'L'`** on your emitter, on purpose, because your own code has
  detected it's stuck (wheels spinning, position not changing, whatever check you write).
- **The supervisor decides you've been stationary for 20 real seconds**, whether you meant to be or
  not.
- **Your robot falls into a hole** (a `trap` tile, [covered on the rules pages](rules-tokens.md)).

All three do exactly the same thing: your robot teleports back to the last checkpoint tile it
crossed (the start tile, if it hasn't crossed any), and [the scoring page](rules-scoring.md) is
correct that it costs a flat **−5**, every time, with no exception for having asked for it yourself.
Sending `'L'` isn't a way to dodge the penalty; it's a way to control *when* you take it, rather than
losing time first and then being relocated involuntarily anyway.

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
last_score = None

while robot.step(timeStep) != -1:
	now = robot.getTime()

	if now - last_request_time > 1:
		emitter.send(struct.pack('c', 'G'.encode()))
		last_request_time = now

	if now > 2.0 and not sent_report:
		emitter.send(struct.pack('i i c', 6, 13, 'U'.encode()))
		sent_report = True
		print(f"t={now:.1f}s SENT identification x=6 z=13 type=U (to give LoP something to take from)")

	if now > 6.0 and not sent_lop:
		emitter.send(struct.pack('c', 'L'.encode()))
		sent_lop = True
		print(f"t={now:.1f}s SENT LoP request")

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

The identification at the top isn't the point of this page; it's there so there's a non-zero score
for the LoP penalty to visibly subtract from. [The scoring page](rules-scoring.md) also notes total
score can never go below zero, so testing this against a starting score of exactly `0.0` would have
hidden the whole effect.

---

## Step 3: A real run, deliberate LoP

!!! success "You should now see"
    ```
    t=2.0s SENT identification x=6 z=13 type=U (to give LoP something to take from)
    t=2.7s score=22.5
    t=6.0s SENT LoP request
    t=6.0s RECEIVED LoP acknowledgement
    t=6.8s score=17.5
    ```

    `22.5 → 17.5`, exactly `−5`, right after the acknowledgement. Full record:
    `trials/20260730-083834-C9-retry.json`.

    Identification right after a spawn or relocate is timing-sensitive on this build — in 10
    consecutive test runs the same code and spawn landed the identification only half the time
    (5 of 10 scored `22.5`; the other 5 scored `0.0` despite sending an identical report).
    If the report scores `0.0` on the first try, reset and try again rather than assuming the code
    is wrong.

### The 20-second timeout is real, and it doesn't care why you're stationary

A separate run, identical setup but **never sending `'L'` at all**, left the robot sitting still
(this page's trial harness starts it already stopped, [same technique as the last two
pages](code-reporting.md)) with no further action from the controller. At **t=21.6s**, with nothing
sent from the robot side, the receiver still got:

```
t=21.6s RECEIVED LoP acknowledgement (nobody asked for this)
```

The supervisor decided on its own that 20 seconds of stillness was enough. A second run of the same
setup confirmed the same thing from the other direction: our own position telemetry (a verification
tool this site's trial harness uses, not something available to student code) showed the robot's
position jump from its stationary spot back to the start tile's real-world coordinates between the
`t=20` and `t=22` position samples, unprompted. Therefore, if your controller ever pauses for 20
seconds for any reason — debugging, waiting on a sensor, anything — expect this to fire whether you
wanted it to or not.

---

## Now make it your own

To avoid waiting for the passive 20-second timeout, write an actual stuck-detector: compare a wheel's
position-sensor reading now against its reading a few seconds ago, and send `'L'` yourself once it
stops changing. You lose the same 5 points either way, but you get back to trying sooner.
- Try triggering LoP with a starting score of exactly `0`, confirm for yourself that the score
  really does floor at zero rather than going negative.
- Combine this with [the wall-follower](code-wall-follower.md), which the wall-follower's own page
  admits settles into a small loop rather than exploring, at what point would you rather your
  controller detect that and self-relocate, instead of quietly burning the clock?

---

## If it goes wrong

- **You send `'L'` and nothing happens.** Confirm you're sending exactly the single byte
  `struct.pack('c', 'L'.encode())`, not a longer message, a 9-byte identification-shaped message
  starting with an `'L'`-like value won't be read as a relocation request.
- **Your score doesn't visibly change after LoP.** If your score was already `0`, this is expected,
  not a bug, per [the scoring page](rules-scoring.md#point-values) the total floors at zero.
- **LoP fires and you didn't ask for it.** Almost always the 20-second passive timeout, see above.
  Additionally, if your controller is doing real work but the robot itself isn't physically moving
  (waiting on a slow calculation, for instance), that still counts as stationary to the supervisor.

---

Next: [sending the exit message](code-exit.md).
