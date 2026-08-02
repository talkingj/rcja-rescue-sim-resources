# What's next: sensors

You've got a robot driving itself and you've changed how fast it goes.

To build on that, everything past this point uses the same **edit code → reload → watch what
changes** loop, just with more interesting code.

!!! success "The full code tutorial series"
    **[Code: build a scoring robot](code-sensors.md)** is a twelve-page series, sensors and driving
    through a complete scored run, and every page quotes real numbers from a real run on this exact
    robot. Read this page for the quick map, then go straight there for the real thing.

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
avoid walls. That's *why* the robot turns before it crashes: it checks a sensor first.

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

The complete competition requires the robot to execute three concurrent tasks, each extending
functionality already introduced.

- **Maze navigation without wall collision.** [A wall-follower that survives
  world1](code-wall-follower.md) implements this and documents its actual maze coverage,
  including limitations.
- **Victim detection** via the robot's camera, identifying coloured or lettered markers on walls.
  Marker semantics and scoring values are specified in [Victims and hazmats](rules-tokens.md) and
  [How points are earned](rules-scoring.md). Detection implementation is covered in [Spotting a
  victim sign](code-victim-detection.md), and [Reporting a victim](code-reporting.md) converts
  detection into scored points.
- **Map submission** to the Competition Supervisor before time expiration. Format specification is
  in [Drawing the map](rules-map-format.md); [Building and submitting the map
  matrix](code-mapping.md) provides the implementation and demonstrates the resulting score change.

Upon completing Track C, [Spending your eight minutes](strategy-run-budget.md) begins a second
series covering strategy: time budgeting, the quantitative basis for run decisions, and common
high-cost errors.

## Additional resources

- The official [Erebus tutorials](https://erebus.rcj.cloud/docs/tutorials/) provide in-depth
  coverage of sensors, camera use, and mapping, using the same controller file referenced
  throughout this site.
- The [RoboCupJunior Rescue Simulation forum](https://junior.forum.robocup.org/c/rescue-simulation)
  handles setup and rules questions.
- The Erebus Discord server handles most technical support and provides faster turnaround than the
  forum for troubleshooting. The invite link is available on the
  [Erebus GitHub page](https://github.com/robocup-junior/erebus).

At this point you have a working simulator installation, a functioning robot and direct experience
modifying controller code. Sensor integration is the next incremental step.
