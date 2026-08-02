# Workshop 2: sensors lab (60 min)

A facilitator guide for a hands-on lab built on [reading a distance sensor](code-sensors.md),
[driving straight and turning](code-driving.md), and [the colour sensor](code-colour.md). Same
format as [the Rules & Scoring workshop](workshop.md): you narrate and time each activity, students
work hands-on at laptops with Webots open.

!!! note "Who this page is for"
    The teacher or mentor running the session. Students should have finished [make it
    move](make-it-move.md) already, this workshop assumes Webots and Erebus are both already
    installed and working.

!!! note "On this page's timing"
    Every code sample and every observed value below is exactly what [the three source
    pages](code-sensors.md) verified for real, on this Mac, nothing here is invented. The minute
    allocations are a facilitator's estimate built around that real content and real trial
    durations (C1's 30s trial, C2's 40s trial, C3's 15s trial), not a literal classroom session
    timed with a room of students, this site doesn't have a classroom to test against. Treat the
    agenda as a solid starting plan, and adjust live based on your own group's pace.

## Learning objectives

By the end of the session, students should be able to:

- Read a distance sensor's value and know which direction (low or high) means "close."
- Write a controller that drives straight until close to a wall, then turns a known amount.
- Read the floor colour sensor and recognise that on this build, several tile types read
  suspiciously similar.

## Before you run it

**Time:** 60 minutes, flexible ±10.

**Group size:** pairs, one laptop with Webots per pair.

**Materials checklist:**

- [ ] Every laptop has Webots and Erebus already installed and `world1.wbt` loading successfully.
- [ ] A way to project code, this session is copy-adapt-run, not type-from-scratch.

## Agenda at a glance

| Time | Activity | Format |
|---|---|---|
| 0–5 min | Intro: what a sensor actually returns | You narrate |
| 5–20 min | Activity 1 — Read the distance sensors | Pairs, hands-on |
| 20–40 min | Activity 2 — Drive straight and turn | Pairs, hands-on |
| 40–55 min | Activity 3 — The colour sensor surprise | Pairs, hands-on |
| 55–60 min | Wrap-up | You narrate |

## Intro: what a sensor actually returns (5 min)

A distance sensor doesn't return "metres to the wall" — it returns a raw number whose scale you have
to discover for yourself. On this exact build, [confirmed on the sensors page](code-sensors.md), low
means close and high means clear. However, that's the opposite of what most students guess first,
which is the whole point of Activity 1.

## Activity 1 — Read the distance sensors (15 min)

**Objective:** get real `ps0`-`ps7` readings and find the near/far threshold for yourself.

Have each pair adapt [the sensors page's controller](code-sensors.md#step-2-write-the-controller):
read all eight sensors every step, print them, and drive the robot into a wall (`wheel1.setVelocity`
/ `wheel2.setVelocity` at a small positive value is enough, steering isn't the point here).

??? question "What should they see"
    In open space, most values sit around `0.8` (clear). Once the robot is pressed against a wall,
    the sensors facing that wall drop toward `0.01`-`0.05`, [exactly what this site's own C1 trial
    found](code-sensors.md#step-3-load-it-and-read-the-console). If a pair sees the opposite
    (high near a wall), the most likely cause is [a misspelled device
    name](debugging-playbook.md#1-a-device-call-fails-with-no-visible-error-at-all), not a
    different sensor scale.

## Activity 2 — Drive straight and turn (20 min)

**Objective:** turn a *measured* amount instead of guessing a timer value.

Adapt [the driving page's controller](code-driving.md#step-2-write-the-controller): drive forward
until the front sensors cross the near threshold from Activity 1, then turn using the wheel position
sensors (`wheel1 sensor`/`wheel2 sensor`) until a fixed radian value is reached.

To give pairs a real number to aim for instead of a guess, tell them the measured value: [confirmed
on the driving page](code-driving.md), roughly **2.28 radians** of wheel rotation turned this robot
90 degrees, verified by turning four times in a row and checking the sensors read the same as the
reference.

??? question "A good check for pairs that finish early"
    Have them turn four times in a row, like [the source page
    did](code-driving.md#step-3-load-it-and-watch-it-stop-turn-and-stop-again), and confirm the sensor reading
    after four turns matches the reading before the first one. Therefore, if it doesn't match closely,
    their radian value needs adjusting.

## Activity 3 — The colour sensor surprise (15 min)

**Objective:** read the floor colour sensor, and see a real, slightly surprising result rather than
a clean textbook one.

Adapt [the colour sensor page's controller](code-colour.md#step-2-write-the-controller): print the
decoded RGB whenever it changes while driving around.

To prepare them for the interesting part, tell students what to expect: [confirmed on that
page](code-colour.md#step-3-load-it-and-watch-the-readings), on this exact build, plain
floor, a checkpoint tile, and a swamp tile all read close to the same near-white value. That's a
real, verified finding, not a mistake in their code if they see it too. Ask pairs: if the colour
sensor alone can't tell these tiles apart, what else would your robot need to know which tile it's
actually on? (Position tracking, most likely, [a real unsolved problem this site ran
into later too](code-complete-run.md).)

## Wrap-up (5 min)

Recap: sensor scale isn't obvious and has to be measured; a "known amount" turn means
measuring it once and reusing the number; and real sensor data sometimes contradicts what the rules
description implies, [confirmed real more than once building this site's own
material](debugging-playbook.md).

To see all three sensors combined into one continuously-running controller, point students ahead to
[the wall-follower page](code-wall-follower.md).

## If you're running short on time

Cut in this order:

1. **Drop the four-turns-in-a-row check in Activity 2.** Confirm the single turn looks roughly
   square and move on.
2. **Trim Activity 3 to just the plain-floor reading.** Skip driving to a checkpoint or swamp tile,
   tell students the surprising result verbally instead of having them find it live.
3. **Shorten Activity 1** to just the two readings (open space, against a wall) rather than having
   pairs drive around exploring further.

Additionally, don't cut Activity 2's real turn-angle number, as guessing a fresh one live wastes far
more time than it saves.

---

Next: [workshop 3, victim detection lab](workshop-victims.md).
