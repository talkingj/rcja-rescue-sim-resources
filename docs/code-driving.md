# Driving straight and turning a known amount

[Reading a distance sensor](code-sensors.md) watched the robot drive into a wall with no steering
at all. This page fixes that: you'll make the robot stop before it hits the wall, then turn a
measured 90 degrees using its wheel sensors, not a guess. It takes about 20 minutes.

!!! note "What you're building"
    A controller that drives straight, stops when a wall is close, turns a measured quarter turn,
    and stops again.

---

## Step 1: How do you measure a turn without a protractor?

Each wheel motor has a matching **position sensor** (an encoder), named `wheel1 sensor` and
`wheel2 sensor`. It reports how far that wheel has spun, in radians, since the controller started.
It only ever counts up (or down), it never resets on its own.

To turn the robot in place, spin the wheels in opposite directions: one forward, one backward. The
robot pivots roughly around its own centre instead of driving forward. How far it turns depends on
how far the wheels have spun, which is exactly what the position sensor measures. So the plan is:

1. Remember what the wheel sensor reads right before the turn starts.
2. Spin the wheels in opposite directions.
3. Stop once the sensor has moved a target amount away from where it started.

That target amount is a real number you have to find out empirically, it depends on the robot's
wheel size and how far apart the wheels are. Step 3 shows how we found it.

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

# Position sensors report how far a wheel has spun, in radians, since the controller started.
# That's how we measure "how much did I actually turn" instead of guessing from a timer.
wheel1_sensor = robot.getDevice("wheel1 sensor")
wheel2_sensor = robot.getDevice("wheel2 sensor")
wheel1_sensor.enable(timeStep)
wheel2_sensor.enable(timeStep)

ps0 = robot.getDevice("ps0")
ps7 = robot.getDevice("ps7")
ps0.enable(timeStep)
ps7.enable(timeStep)

FORWARD_SPEED = 3.0
TURN_SPEED = 3.0
TURN_TARGET_RADIANS = 2.28   # measured on this build to turn the robot about 90 degrees

state = "straight"
wheel1_at_turn_start = None

while robot.step(timeStep) != -1:
    if state == "straight":
        wheel1.setVelocity(FORWARD_SPEED)
        wheel2.setVelocity(FORWARD_SPEED)
        if ps0.getValue() < 0.1 or ps7.getValue() < 0.1:
            print(f"REACHED WALL ps0={ps0.getValue():.3f} ps7={ps7.getValue():.3f}")
            wheel1_at_turn_start = wheel1_sensor.getValue()
            state = "turning"

    elif state == "turning":
        wheel1.setVelocity(-TURN_SPEED)
        wheel2.setVelocity(TURN_SPEED)
        turned_so_far = abs(wheel1_sensor.getValue() - wheel1_at_turn_start)
        if turned_so_far >= TURN_TARGET_RADIANS:
            print(f"TURN DONE wheel1 turned {turned_so_far:.4f} rad")
            state = "done"

    elif state == "done":
        wheel1.setVelocity(0)
        wheel2.setVelocity(0)
        print(f"AFTER TURN ps0={ps0.getValue():.3f} ps7={ps7.getValue():.3f}")
        state = "stopped"

    elif state == "stopped":
        pass
```

`TURN_TARGET_RADIANS = 2.28` is not a textbook number. It's what we measured on this exact robot,
in this exact build, using the check in Step 3. Save the file.

---

## Step 3: Load it and watch it stop, turn, and stop again

1. In Webots, press **reset**, then **LOAD** your saved file.
2. Press **start**.

!!! success "You should now see"
    The robot drives forward, then stops as it nears a wall. On a real run, the console printed:

    ```
    REACHED WALL ps0=0.052 ps7=0.052
    ```

    Then it pivots in place (one wheel spins each way) and stops again a moment later:

    ```
    TURN DONE wheel1 turned 2.3048 rad
    AFTER TURN ps0=0.047 ps7=0.064
    ```

### How we found `2.28`, and how we know it's really 90 degrees

We didn't measure an angle directly, there's no protractor in Webots. Instead we used a check that
doesn't depend on knowing anything about the maze around the robot: if one turn is really 90
degrees, four turns in a row should bring the robot back to face the exact wall it started against.

!!! success "You should now see"
    Running four turns back to back, with `TURN_TARGET_RADIANS = 2.28` each time, on a real run:

    ```
    REFERENCE (before any turn) ps0=0.052 ps7=0.052
    AFTER TURN 1/4 wheel1 turned 2.3048 rad this turn  ps0=0.048 ps7=0.059
    AFTER TURN 2/4 wheel1 turned 2.3048 rad this turn  ps0=0.800 ps7=0.438
    AFTER TURN 3/4 wheel1 turned 2.3048 rad this turn  ps0=0.800 ps7=0.202
    AFTER TURN 4/4 wheel1 turned 2.2985 rad this turn  ps0=0.041 ps7=0.066
    ```

    After turn 4, `ps0` and `ps7` read `0.041` and `0.066`, close to the `0.052`/`0.052` we started
    with, and nothing like turns 2 and 3, which faced open space. Four quarter turns brought the
    robot back to the wall it started against. That's our evidence `2.28` is a real 90 degree turn
    on this robot, not just a number that happened to work once.

---

## Now make it your own

- Change `TURN_TARGET_RADIANS` to `1.14`, about half of `2.28`, and see the robot make a smaller,
  roughly 45 degree turn instead.
- Chain two turns in a row (reset `wheel1_at_turn_start` and go back to `"turning"` instead of
  `"done"`) to make a 180 degree turn, then check the front sensors read something completely
  different from the wall you started against.
- Try `TURN_SPEED = 6.0`. The turn finishes faster, but check whether `wheel1 turned` still lands
  close to the same number, if it overshoots noticeably, that tells you something about how the
  simulation handles fast spins.

Next in this series, the floor colour sensor tells the robot what kind of tile it's driving over,
which matters as much as not hitting walls.

---

## If it goes wrong

- **The robot spins forever and never stops.** Check the comparison is `>=`, not `==`. Floating
  point sensor readings essentially never land on an exact target value, so `==` will never be true
  and the turn never ends.
- **The turn happens but the numbers afterward look nothing like this page's.** That's expected if
  your robot reaches the wall from a different angle or a different spot in the maze than this run
  did, the exact sensor values depend on exactly where the robot is standing. What should carry
  over is the shape of the result: turn 1 and turn 4 read similarly, turns 2 and 3 don't.
- **`wheel1_sensor.getValue()` returns `nan` or throws an error.** You likely forgot the
  `.enable(timeStep)` call for the position sensor, it's easy to enable the motor and the distance
  sensors and forget the encoder, since it doesn't look like a normal sensor.

---

Next: the floor colour sensor (coming soon).
