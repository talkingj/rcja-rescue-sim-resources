# A wall-follower that survives world1

To build the first genuinely useful robot on this site, this page combines the last three pages into
one that follows a wall instead of just avoiding the nearest thing in front of it. It takes about 20
minutes, and it's the most honest page in this series about what a simple approach can and can't
do.

!!! note "What you're building"
    A controller that keeps a wall on its right and drives alongside it, with a fallback manoeuvre
    for when it gets wedged.

---

## Step 1: Wall avoidance isn't wall following

Every controller so far has reacted to the front sensors only: stop or turn when something's close.
That's enough to not crash. However, it isn't enough to explore a maze, as a robot that only reacts
to what's directly in front of it has no sense of direction. To give it one, **wall following**
picks one wall (say, the one on the right) and steers to keep it at a roughly constant distance:

- Wall too close on the right → **veer left**.
- Wall too far away (or lost) on the right → **curve right** to find it again.
- Wall at a good distance → **drive straight**.
- Anything blocking the front → **turn away first**, wall-following logic waits.

This is the same left/right-hand rule you might have heard of for solving a maze by hand: keep one
hand on the wall and eventually you cover the connected maze.

---

## Step 2: Write the controller

```python
from controller import Robot

timeStep = 32
robot = Robot()

wheel1 = robot.getDevice("wheel1 motor")
wheel2 = robot.getDevice("wheel2 motor")
wheel1.setPosition(float("inf"))
wheel2.setPosition(float("inf"))

ps0 = robot.getDevice("ps0")   # front-right-ish
ps7 = robot.getDevice("ps7")   # front-left-ish
ps2 = robot.getDevice("ps2")   # right side, the wall we're following
for s in (ps0, ps7, ps2):
    s.enable(timeStep)

MAX_VELOCITY = 6.28
FRONT_BLOCKED = 0.15
WALL_TOO_CLOSE = 0.05
WALL_LOST = 0.5

step = 0
stuck_steps = 0
escape_steps_left = 0
escape_phase = "reverse"

while robot.step(timeStep) != -1:
    step += 1
    front_blocked = ps0.getValue() < FRONT_BLOCKED or ps7.getValue() < FRONT_BLOCKED

    if escape_steps_left > 0:
        # Stuck-recovery: back away from whatever we're wedged against, then turn hard, before
        # handing control back to the wall-follower.
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

    if step % 470 == 0:   # roughly every 15 simulated seconds
        print(f"t={robot.getTime():.1f}s ps0={ps0.getValue():.3f} ps7={ps7.getValue():.3f} "
              f"ps2={ps2.getValue():.3f} stuck_steps={stuck_steps} "
              f"escaping={'yes' if escape_steps_left > 0 else 'no'}")
```

To keep the robot from staying physically wedged in world1's tighter corners, which plain
wall-following does on its own, the `stuck_steps` / `escape_steps_left` machinery backs it out and
turns before handing control back. Step 3 shows you exactly how well (and how badly) this actually
worked.

---

## Step 3: Run it for the full 3 minutes

1. In Webots, press **reset**, then **LOAD** your saved file.
2. Press **start**, and this time let it run for a full 3 minutes.

!!! success "You should now see"
    Status lines every 15 seconds or so, all reporting `escaping=no`, meaning the recovery
    manoeuvre never had to trigger for more than a few steps at a time:

    ```
    t=16.3s ps0=0.183 ps7=0.310 ps2=0.637 stuck_steps=0 escaping=no
    t=31.3s ps0=0.152 ps7=0.304 ps2=0.636 stuck_steps=0 escaping=no
    t=46.4s ps0=0.178 ps7=0.309 ps2=0.637 stuck_steps=0 escaping=no
    ```

### The honest result: it never freezes, but it doesn't tour the maze either

We measured the robot's real position from outside its own code for this trial — our verification
rig can do this, your controller can't, as that's a supervisor-only capability covered in a later
page on reading the clock and the score — to answer the acceptance question properly: how much of
world1 did this robot actually cover in 3 minutes?

**Result: not much, and it repeats itself.** The sensor readings above are a strong hint: `ps0`,
`ps7`, and `ps2` return to almost the exact same handful of values over and over. Every 15-second
sample looks like one of two snapshots. Additionally, position data confirms it: over the full 180
seconds, the robot's position stayed within a box roughly 0.21 m by 0.48 m, and it never stopped
moving (no `escaping=yes` lasting more than a couple of steps), but it was tracing the same short
loop repeatedly rather than pushing further into the maze.

**Why:** world1 has some tight corners (you may recognise this from [Driving straight and turning a
known amount](code-driving.md), which found a similar dead-end near this same starting corridor).
A simple three-zone rule (too close / too far / just right) reacts fast enough to avoid the wall,
but it doesn't have enough memory to notice "I've been here before" and try something different.
Therefore, that's the real, if unglamorous, limit of this approach: it survives, and it doesn't get
properly lost, but it isn't yet a maze-solving algorithm.

---

## Now make it your own

- Switch to following the **left**-hand wall instead (`ps5` instead of `ps2`, mirror the turn
  directions). We tried this on this same maze and it got stuck in an even smaller loop, a good
  demonstration that which wall you choose matters given a specific maze layout.
- Widen `WALL_LOST` even further, or narrow `WALL_TOO_CLOSE`, and see whether the loop this page
  found gets bigger or smaller.
- To address this, the real fix for a repeating loop is usually to add memory: some way to detect
  "I'm covering the same ground" and deliberately do something different. That's a bigger project
  than this page, but it's exactly the kind of thing a stronger competition robot needs.

---

## If it goes wrong

- **The robot doesn't move at all.** Check `stuck_steps > 12` isn't true from the very first step.
  If `front_blocked` is `True` at spawn (unlikely in world1, but possible elsewhere), the robot will
  go straight into recovery mode and might look like it's doing nothing if the reverse speed is too
  low to notice.
- **The robot spins in place forever without ever going straight.** Check `WALL_TOO_CLOSE` is
  smaller than `WALL_LOST`. If you accidentally swap them, every reading counts as both "too close"
  and "too far" at once, and the two branches fight each other.
- **It looks like it's exploring more than this page describes.** Good, that means you changed
  something that helped, most likely the thresholds or which wall you're following. Try quoting your
  own real numbers rather than assuming this page's result still applies once you've changed the
  code.

---

Next: [Turning on the camera](code-camera.md).
