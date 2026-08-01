# An 8-week club curriculum

A term's worth of weekly club sessions, each week pointing at pages that already exist on this site.
No week references material that doesn't exist yet. Additionally, every link below points to a real,
already-verified page.

!!! note "Who this page is for"
    The teacher or mentor running a term-long club, not a single class period. For a single 45-60
    minute session instead, see [the classroom workshop](workshop.md).

## How to use this page

To keep club time free for the hands-on parts, assign each week's pages as homework reading — every
Track C page is written to be followed along with a laptop and Webots open. Each week below lists
roughly one 60-90 minute session's worth, plus a short note on what to check before moving on.

## Week 1: Setup

- [Before you start](before-you-start.md)
- Install: [Windows](install-windows.md), [macOS](install-mac.md), or [Linux](install-linux.md)
- [Your first run](first-run.md)
- [Make it move](make-it-move.md)

**Check before moving on:** every student has Webots and Erebus running, and has personally changed
`max_velocity` and seen the robot move differently, [as that page walks through](make-it-move.md).

## Week 2: Sensors and driving

- [Reading a distance sensor](code-sensors.md)
- [Driving straight and turning a known amount](code-driving.md)

**Check before moving on:** every student's controller prints real `ps0`-`ps7` values and can turn a
measured amount, not just "some amount that looked like 90 degrees."

## Week 3: The colour sensor and a real wall-follower

- [Reading the floor with the colour sensor](code-colour.md)
- [A wall-follower that survives world1](code-wall-follower.md)

**Check before moving on:** every student's robot can run for a full 3 minutes in `world1` without
permanently getting stuck. [This site's own wall-follower still settles into a small loop rather
than touring the maze](code-wall-follower.md), meaning full coverage isn't realistic yet. However,
that's a real, unsolved problem, not a bar this week's work needs to clear.

## Week 4: Seeing and reporting

- [Turning on the camera](code-camera.md)
- [Spotting a victim sign](code-victim-detection.md)
- [Reporting a victim and earning your first points](code-reporting.md)

**Check before moving on:** every student has seen their own score change in the Competition
Controller after a real report, not just read about it.

## Week 5: The game's other messages

- [Reading the clock and the score](code-game-info.md)
- [Getting stuck, and recovering from it](code-lack-of-progress.md)
- [Sending the exit message](code-exit.md)

**Check before moving on:** every student can explain, in their own words, the two conditions for
the exit bonus, [confirmed on that page](code-exit.md#step-1-one-byte-two-conditions), and why
running out the clock forfeits it even with a submitted map, [confirmed on the run-budget
page](strategy-run-budget.md#what-running-out-the-clock-actually-costs-you).

## Week 6: Mapping and the complete pipeline

- [Building and submitting the map matrix](code-mapping.md)
- [Putting it together: a complete scored run](code-complete-run.md)

**Check before moving on:** every student has combined at least detection and reporting into one
controller and watched it run without a supervisor's help — [the same honest test this site ran on
itself](code-complete-run.md). It's fine, expected even, if it doesn't score every time.

## Week 7: Strategy

- [Spending your eight minutes](strategy-run-budget.md)
- [Scoring maths for strategy decisions](strategy-scoring-maths.md)
- [The eight practice worlds and what each is for](practice-worlds.md)
- [The rule violations that quietly cost the most](costly-mistakes.md)

**Check before moving on:** each team can work through at least one of [the worked score
comparisons](strategy-scoring-maths.md) using their own team's real numbers instead of the page's
example ones.

## Week 8: Mock competition

- [The debugging playbook](debugging-playbook.md)
- [Pre-run and competition-day checklist](competition-day.md)
- A scored mock round in class, see [running a mock competition in class](mock-competition.md)

**Check before moving on:** every team has run their controller through a full timed match at least
once, start to finish, with a real final score, whatever that score turns out to be.

---

## If a week runs long

To account for the site's hardest, still-unsolved problem (reliable maze coverage), plan for two
sessions on weeks 3 and 6 if a team needs them — that's normal, not a sign anything's wrong.
Therefore, don't compress the schedule by skipping the "check before moving on" item: later weeks
assume it.

---

Next: [workshop 2, sensors lab](workshop-sensors.md).
