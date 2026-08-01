# What's next: sensors

You've got a robot driving itself and you've changed how fast it goes.

To build on that, everything past this point uses the same **edit code → reload → watch what
changes** loop, just with more interesting code.

!!! success "The full code tutorial series now exists on this site"
    This page used to be a teaser with no tutorial behind it. That's no longer true: **[Code: build
    a scoring robot](code-sensors.md)** is a twelve-page series, sensors and driving through a
    complete scored run, and every page quotes real numbers from a real run on this exact robot, not
    invented ones. Read this page for the quick map, then go straight there for the real thing.

    | You want to... | Go to |
    |---|---|
    | Read a distance sensor for real | [Reading a distance sensor](code-sensors.md) |
    | Drive a known distance and turn a known angle | [Driving straight and turning](code-driving.md) |
    | Build a robot that survives the maze | [A wall-follower that survives world1](code-wall-follower.md) |
    | See and report a victim | [Spotting a victim sign](code-victim-detection.md), [Reporting a victim](code-reporting.md) |
    | Submit a map and see the mapping bonus | [Building and submitting the map matrix](code-mapping.md) |
    | See it all combined, including what still doesn't work | [Putting it together: a complete scored run](code-complete-run.md) |

---

## The robot can already feel its surroundings

The e-puck robot (the round one you've been driving) has **eight distance sensors**. They're named
`ps0` through `ps7`, spaced evenly around its edge. The sample code already reads some of them to
avoid walls. That's *why* the robot turns before it crashes. It isn't guessing. It's checking a
sensor first.

A distance sensor is a **device**, a piece of hardware code can talk to. It returns a number, and on
this build it works the opposite way you might expect: a high number means open space, a low number
means something's close, meaning the robot already has the data it needs to avoid walls.
[Reading a distance sensor](code-sensors.md) has the real values from a real run. Reading one in
code looks like this:

```python
distance_sensor = robot.getDevice("ps0")
distance_sensor.enable(timeStep)
value = distance_sensor.getValue()
```

Try this next: open the controller file again and find where the sample code reads `ps0`–`ps7`.
Add a `print(value)` next to one of them, reload, and watch the numbers change in the console as
you drag the robot near a wall.

## After that: mapping and victims

The full competition asks the robot to do three things at once, each building on what you've
already touched. Each now has both a rules explanation and a real, trialled code tutorial:

- **Navigate the maze** without bumping into walls. [A wall-follower that survives
  world1](code-wall-follower.md) builds one, and reports honestly on how much of the maze it does
  and doesn't cover.
- **Find "victims"** using the robot's camera, little coloured or lettered markers on the walls. What
  those markers mean and what they're worth is covered in [Victims and hazmats](rules-tokens.md) and
  [How points are earned](rules-scoring.md); the code that actually sees one is [Spotting a victim
  sign](code-victim-detection.md), and [Reporting a victim](code-reporting.md) turns that into real
  points.
- **Report a map** of the maze back to the Competition Supervisor before time runs out. The format is
  covered in [Drawing the map](rules-map-format.md); [Building and submitting the map
  matrix](code-mapping.md) is the code that sends one and shows the real score change.

Once you've been through all of Track C, [Spending your eight minutes](strategy-run-budget.md)
starts a second series on strategy, budgeting your time, the maths behind real decisions, and the
mistakes that quietly cost the most.

## Where to go for the real thing

- The official
  [Erebus tutorials](https://erebus.rcj.cloud/docs/tutorials/) cover sensors, the camera, and
  mapping in depth, using the same controller file you've already been editing.
- The [RoboCupJunior Rescue Simulation forum](https://junior.forum.robocup.org/c/rescue-simulation)
  is where teams ask setup and rules questions.
- The Erebus Discord server is where most **technical support** happens. It's faster than the forum
  for "my thing is broken" questions. Look for the invite link on the
  [Erebus GitHub page](https://github.com/robocup-junior/erebus).

You've installed a real simulator, run a real robot, and changed real code. Therefore, sensors are
just the next number to change.
