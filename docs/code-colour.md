# Reading the floor with the colour sensor

The robot has a second sensor pointing straight down: a colour sensor that tells you what kind of
tile it's standing on. [Understanding the field](rules-field.md) already covers what each floor
colour means in the rules. This page is about reading that colour from code, and what we actually
saw when we did. It takes about 15 minutes.

!!! note "What you're building"
    A wall-avoiding controller (borrowed from the very first sample you ran) with one line added:
    print the floor colour every time it changes.

---

## Step 1: The colour sensor is a tiny camera, not a simple number

Unlike the distance sensors, `colour_sensor` is a **camera** with a 1x1 pixel image, not a plain
value sensor. Call `.getImage()` on it and you get raw bytes back, one pixel's worth: blue, green,
red, alpha, in that order. To get red, green, and blue as separate numbers:

```python
image = colour_sensor.getImage()
b, g, r = image[0], image[1], image[2]
```

That byte order (BGRA, not RGB) is easy to get backwards. We checked it against the real result in
Step 3 before trusting it.

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

ps5 = robot.getDevice("ps5")
ps7 = robot.getDevice("ps7")
ps0 = robot.getDevice("ps0")
ps2 = robot.getDevice("ps2")
for s in (ps5, ps7, ps0, ps2):
    s.enable(timeStep)

# The floor colour sensor is a 1x1 camera mounted underneath the robot, not a simple analogue
# sensor. getImage() returns raw bytes in BGRA order, one pixel's worth: blue, green, red, alpha.
colour_sensor = robot.getDevice("colour_sensor")
colour_sensor.enable(timeStep)

MAX_VELOCITY = 6.28
last_rgb = None

while robot.step(timeStep) != -1:
    speed1 = MAX_VELOCITY
    speed2 = MAX_VELOCITY
    if ps5.getValue() < 0.1:
        speed2 = MAX_VELOCITY / 2
    if ps2.getValue() < 0.1:
        speed1 = MAX_VELOCITY / 2
    if ps0.getValue() < 0.1 or ps7.getValue() < 0.1:
        speed1 = MAX_VELOCITY
        speed2 = -MAX_VELOCITY
    wheel1.setVelocity(speed1)
    wheel2.setVelocity(speed2)

    image = colour_sensor.getImage()
    b, g, r = image[0], image[1], image[2]
    rgb = (r, g, b)
    if rgb != last_rgb:
        print(f"COLOUR CHANGED to r={r} g={g} b={b}  at t={robot.getTime():.1f}s")
        last_rgb = rgb
```

We reused the wall-avoidance from `ExamplePlayerController_updated.py` so the robot actually
covers ground instead of sitting still, only printing when the colour changes keeps the console
readable.

---

## Step 3: Load it and watch the readings

1. In Webots, press **reset**, then **LOAD** your saved file.
2. Press **start**.

!!! success "You should now see"
    On a real run, starting on the practice map's marked starting tile, the very first reading was:

    ```
    COLOUR CHANGED to r=247 g=247 b=247  at t=1.4s
    ```

    As the robot drove around on ordinary floor, later readings looked like this:

    ```
    COLOUR CHANGED to r=253 g=253 b=253  at t=2.4s
    COLOUR CHANGED to r=252 g=252 b=252  at t=2.4s
    COLOUR CHANGED to r=251 g=251 b=251  at t=3.3s
    ```

    All of these are shades of the same near-white grey, `r`, `g`, and `b` always equal to each
    other. That's expected: plain floor has no colour tint, just brightness that flickers a little
    with the simulated lighting as the robot moves.

### A genuinely surprising result: checkpoints and swamps don't look different

We expected the starting tile (which the rules mark as a checkpoint) and a swamp tile to read a
different colour from plain floor, since [Understanding the field](rules-field.md) describes them
as visually distinct. We tested this directly: the starting tile's reading above, `247/247/247`,
and a swamp tile reached by a short deliberate drive both landed in the exact same `247`–`255`
near-white band as ordinary floor tiles.

**On this build, the colour sensor alone cannot tell a checkpoint or a swamp apart from plain
floor.** If your strategy depends on knowing you've reached a checkpoint, you need the game info
the supervisor reports (a later page in this series), not this sensor.

### What we couldn't verify this session

We didn't manage to safely and repeatably drive the robot onto a hole tile or a coloured passage
tile to quote a real reading from either, the maze routes to reach them from the practice worlds
we tried needed more precise navigation than this page's simple wall-avoidance can manage.
[Understanding the field](rules-field.md) and [Victims and hazmats](rules-tokens.md) describe what
to expect (a black-edged gap for a hole, a specific solid colour per passage). Until you've measured
it yourself, don't hard-code an exact number for either: watch for a **large, sudden** change away
from the near-white range this page confirmed, and treat that as a reason to stop and look, rather
than trusting a guessed threshold.

---

## Now make it your own

- Print `image` itself (the raw 4 bytes) before decoding it, and check the fourth byte, alpha, it
  should stay constant even while r/g/b change.
- Add a `print()` that shows all three channels divided by 255, so you're looking at numbers from
  0 to 1 instead of 0 to 255, useful for comparing against the tile colours described in
  [Understanding the field](rules-field.md).
- Try slowing the robot down. The near-white readings above jitter by a few points frame to frame,
  slower driving makes it easier to tell real colour changes from that noise.

Next in this series: a wall-follower that combines this page and the last two into a robot that can
actually survive a maze.

---

## If it goes wrong

- **`image[0]` raises an `IndexError` or looks like garbage.** The colour sensor needs
  `.enable(timeStep)` just like any other sensor, and needs at least one `robot.step()` to have run
  before `getImage()` returns real data. If you call it before the first step, you'll get nothing
  useful.
- **Every reading looks identical, `r`, `g`, and `b` never seem to change at all.** That may be
  correct, not a bug: as this page found, ordinary floor, checkpoints, and swamps all read close to
  the same near-white value on this build. Don't assume your sensor is broken just because the
  number isn't moving.
- **You expected a specific colour (like the black hole or a coloured passage) and got white
  instead.** You're most likely still on plain floor. We ran into exactly this trying to reach
  those tiles this session, see the note above.

---

Next: a wall-follower that survives world1 (coming soon).
