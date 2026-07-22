# What's next: sensors

You've got a robot driving itself and you've changed how fast it goes. That's the whole loop this
guide teaches: **edit code → reload → watch what changes.** Everything past this point uses that
same loop, just with more interesting code.

!!! note "This page is a teaser, not a tutorial"
    We don't walk through these steps with screenshots the way the rest of the site does. Think of
    it as a map of where to go next, once you're ready.

---

## The robot can already feel its surroundings

The e-puck robot (the round one you've been driving) has **eight distance sensors**. They're named
`ps0` through `ps7`, spaced evenly around its edge. The sample code already reads some of them to
avoid walls. That's *why* the robot turns before it crashes. It isn't guessing. It's checking a
sensor first.

A distance sensor is a **device**, a piece of hardware code can talk to. It returns a number. A low
number means open space. A high number means something's close. Reading one in code looks like
this:

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
already touched:

- **Navigate the maze** without bumping into walls (you've seen the start of this).
- **Find "victims"** using the robot's camera, little coloured or lettered markers on the walls.
- **Report a map** of the maze back to the Competition Supervisor before time runs out.

None of that is covered here. It's a lot more code, and it deserves real tutorials, not a rushed
add-on to a beginner guide.

## Where to go for the real thing

- The official
  [Erebus tutorials](https://erebus.rcj.cloud/docs/tutorials/) cover sensors, the camera, and
  mapping in depth, using the same controller file you've already been editing.
- The [RoboCupJunior Rescue Simulation forum](https://junior.forum.robocup.org/c/rescue-simulation)
  is where teams ask setup and rules questions.
- The Erebus Discord server is where most **technical support** happens. It's faster than the forum
  for "my thing is broken" questions. Look for the invite link on the
  [Erebus GitHub page](https://github.com/robocup-junior/erebus).

You've installed a real simulator, run a real robot, and changed real code. Sensors are just the
next number to change.
