# Assessment rubric and progress tracker

A rubric for grading a student's or team's real progress through [Track C](code-sensors.md), plus a
simple per-student tracker. Every criterion below maps to one specific skill taught on one specific
page, nothing here is a generic "participation" or "effort" score.

!!! note "Who this page is for"
    A teacher or mentor who needs to grade progress, formally or informally, rather than just run
    sessions. Pairs well with [the 8-week curriculum](club-curriculum.md), whose weeks map directly
    onto this rubric's rows.

## The rubric

Score each row **0** (not yet attempted), **1** (attempted, doesn't work reliably), or **2**
(works, verified live).

| # | Skill | Taught on | What "2" looks like |
|---|---|---|---|
| 1 | Read a distance sensor and find its near/far threshold | [Reading a distance sensor](code-sensors.md) | Prints real `ps0`-`ps7` values and can state which direction means "close" on their own robot |
| 2 | Drive straight and turn a measured amount | [Driving straight and turning](code-driving.md) | Robot turns a consistent, known angle, verified by repeating the turn several times and checking it lines back up |
| 3 | Read the floor colour sensor | [Reading the floor with the colour sensor](code-colour.md) | Can explain what their own robot's sensor reads on plain floor, and knows not to assume every tile type reads differently |
| 4 | Combine sensors into a working wall-follower | [A wall-follower that survives world1](code-wall-follower.md) | Robot runs at least a few minutes without permanently getting stuck; full maze coverage is not required, [it's still an open problem on this site too](code-wall-follower.md) |
| 5 | Enable a camera and read its real resolution | [Turning on the camera](code-camera.md) | Prints the camera's actual width, height, and buffer length, not assumed values |
| 6 | Detect a victim sign and tune the threshold for their own camera | [Spotting a victim sign](code-victim-detection.md) | Detects a real sign at a real distance, with a threshold value they found themselves, not copied blind from a sample |
| 7 | Report a victim and see a real score change | [Reporting a victim and earning your first points](code-reporting.md) | Score visibly increases in the Competition Controller after a real report |
| 8 | Read game info (score, both clocks) | [Reading the clock and the score](code-game-info.md) | Can explain the difference between the game clock and the real-world clock in their own words |
| 9 | Trigger and recover from Lack of Progress deliberately | [Getting stuck, and recovering from it](code-lack-of-progress.md) | Can trigger `'L'` on purpose and explain why it costs the same 5 points as being caught by the passive timeout |
| 10 | Send a correct exit message and explain its two conditions | [Sending the exit message](code-exit.md) | Can state both conditions for the exit bonus without looking them up |
| 11 | Submit a map matrix | [Building and submitting the map matrix](code-mapping.md) | Successfully submits a matrix and can explain that the bonus is a multiplier, not a flat amount |
| 12 | Combine detection, reporting, and exit into one autonomous controller | [Putting it together: a complete scored run](code-complete-run.md) | Controller runs unassisted and attempts a real report; a non-zero score is a bonus, not a requirement, [this site's own combined controller didn't reliably get one either](code-complete-run.md) |
| 13 | Apply the point table to a real scenario | [Scoring maths for strategy decisions](strategy-scoring-maths.md) | Can work through one of the page's comparisons using their own team's real numbers |
| 14 | Identify a real rule violation before it happens | [The rule violations that quietly cost the most](costly-mistakes.md) | Can name at least two of the four violations and their real cost without looking them up |

## Per-student progress tracker

A simple table to copy per student or team. Fill in a score (0/1/2) per skill as they progress.

| Student/team | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | Total (/28) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | | | | | | |
| | | | | | | | | | | | | | | | |
| | | | | | | | | | | | | | | | |

---

## Using this alongside the curriculum

[The 8-week curriculum's](club-curriculum.md) weeks map onto this rubric roughly as: Week 2 → skills
1-2, Week 3 → skills 3-4, Week 4 → skills 5-7, Week 5 → skills 8-10, Week 6 → skills 11-12, Week 7 →
skills 13-14. A team scoring mostly 2s through skill 12 by week 6 is on pace; a team stuck at 1s on
skill 4 (the wall-follower) going into week 4 is worth extra attention before piling on camera work,
[skill 6 assumes the robot can already move around reliably](code-victim-detection.md).

---

Next: [running a mock competition in class](mock-competition.md).
