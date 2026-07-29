# Reading a distance sensor

[What's next: sensors](next-steps.md) showed you the four lines that read `ps0`. This page is the
real version: you'll write a controller that reads all eight distance sensors and watch the numbers
change as the robot drives into a wall. It takes about 15 minutes and is the first page in a series
that ends with a robot that can score points.

!!! note "What you're building"
    A controller that prints all eight sensor readings, twice a second, while the robot drives
    straight ahead. No steering logic yet, that's the next page.

---

## Step 1: What a distance sensor actually returns

The e-puck robot (the round one) has eight distance sensors spaced evenly around its edge, named
`ps0` through `ps7`. Each one is a **device**, a piece of hardware your code talks to through
`robot.getDevice("name")`. Once you have the device, you must call `.enable(timeStep)` on it before
it reports anything. This is easy to forget, an un-enabled sensor silently returns `0.0` forever
instead of an error.

A sensor's `.getValue()` is **not a distance in centimetres**. It's a raw number, and on this build
it runs from about `0.8` (nothing nearby) down toward `0.0` (something right against the sensor).
That's the opposite of what you might expect: **low means close, high means clear.** You'll see the
real numbers in Step 3.

---

## Step 2: Write the controller

Open a fresh copy of `ExamplePlayerController_updated.py` and replace its contents with this:

```python
from controller import Robot

timeStep = 32                # milliseconds between updates, same as the sample controller
robot = Robot()

# Every device on the robot is reached through robot.getDevice("name"). The wheels are
# devices too, but for this page we only need to prove the robot is doing something while
# we watch the sensors, so we drive it straight ahead with no logic at all.
wheel1 = robot.getDevice("wheel1 motor")
wheel2 = robot.getDevice("wheel2 motor")
wheel1.setPosition(float("inf"))
wheel2.setPosition(float("inf"))

# The e-puck has eight distance sensors spaced evenly around its ring, named ps0 to ps7.
# Each one is a separate device and each one needs its own enable() call before it will
# report anything.
sensor_names = ["ps0", "ps1", "ps2", "ps3", "ps4", "ps5", "ps6", "ps7"]
sensors = []
for name in sensor_names:
    sensor = robot.getDevice(name)
    sensor.enable(timeStep)
    sensors.append(sensor)

step_count = 0
while robot.step(timeStep) != -1:
    # Drive straight ahead at a gentle speed. This controller does not look at the
    # sensors yet, on purpose, so we can watch what they report both in open space and
    # once the robot has driven into a wall.
    wheel1.setVelocity(3.0)
    wheel2.setVelocity(3.0)

    step_count += 1
    if step_count % 16 == 0:            # print roughly twice a second, not every 32ms
        readings = {name: round(sensor.getValue(), 3)
                    for name, sensor in zip(sensor_names, sensors)}
        print(readings)
```

Save it. Every device this page uses (`wheel1 motor`, `wheel2 motor`, `ps0`–`ps7`) is already
named on the robot, you don't need to change anything in Webots itself.

---

## Step 3: Load it and read the console

1. In Webots, in the Competition Controller panel, press **reset**, then **LOAD** and pick your
   saved file.
2. Press **start**.

!!! success "You should now see"
    A new line printed roughly twice a second, each one a dictionary of all eight readings. Right
    after start, on a real run, the first line was:

    ```
    {'ps0': 0.8, 'ps1': 0.461, 'ps2': 0.8, 'ps3': 0.185, 'ps4': 0.171, 'ps5': 0.055, 'ps6': 0.116, 'ps7': 0.45}
    ```

The robot drives forward with no steering at all, so it will eventually meet a wall head-on. Watch
`ps0` and `ps7` in particular, they're the sensors that ended up facing the wall on this run. Their
numbers should drop toward zero as the robot presses into it.

!!! success "You should now see"
    On the same run, once the robot had driven into a wall and stopped making progress, the reading
    settled into a steady pattern and stayed there:

    ```
    {'ps0': 0.011, 'ps1': 0.06, 'ps2': 0.8, 'ps3': 0.52, 'ps4': 0.173, 'ps5': 0.056, 'ps6': 0.064, 'ps7': 0.011}
    ```

    `ps0` and `ps7` fell from `0.8`/`0.45` at the start to `0.011`, and stayed at `0.011` for the
    rest of the run, that's the wall. `ps2`, which never faced anything, stayed pegged at `0.8` for
    the entire 30 seconds. On this build, readings above about `0.4` mean nothing is close, and
    readings below about `0.1` mean the sensor is pressed against something.

---

## Now make it your own

- Change `% 16` to `% 4` to print four times as often, then reload. More data, same idea.
- Add a `print()` for just `ps0` on its own, so you can watch one number instead of eight at once.
- Try changing the forward speed from `3.0` to `6.0` and see how much less warning you get before
  the front sensors drop toward zero, since the robot covers the same distance in less time.

The next page in this series,
[Driving straight and turning a known amount](code-driving.md), uses these same numbers to make the
robot turn before it hits the wall, instead of just watching it happen.

---

## If it goes wrong

- **Nothing prints in the console.** Check you saved the file before pressing LOAD, and that you
  pressed LOAD again after saving, an old version stays loaded until you do.
- **Nothing prints, and the console just says the controller exited successfully, with no error at
  all.** We tested this by misspelling `ps0` as `pso` on purpose: the controller doesn't crash and
  doesn't print anything, it just quietly stops on the first bad `getDevice()` call. There's no
  traceback to read. If your printing stops right after start with no error text, re-check every
  sensor name is spelled exactly `ps0` through `ps7`, lowercase, no typos.
- **The console fills up faster than you can read it.** That's expected at twice a second. Use the
  console's scrollback, or lower how often it prints as described above.

---

Next: [Driving straight and turning a known amount](code-driving.md).
