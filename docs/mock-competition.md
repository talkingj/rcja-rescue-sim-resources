# Running a mock competition in class

A guide for running a scored mock round in a single class period, with a scoring sheet, walked
through once against this site's own real [complete-run page](code-complete-run.md) data so the
procedure is proven before you use it live.

!!! note "Who this page is for"
    The teacher or mentor running week 8 of [the club curriculum](club-curriculum.md), or any single
    session where teams run their controllers under real match conditions and get a real score.

## Before you run it

**Time:** one class period, roughly 60-90 minutes depending on team count.

**Materials checklist:**

- [ ] Every team's controller loads and runs without crashing, confirmed once beforehand, [per the
      debugging playbook](debugging-playbook.md), a controller that fails silently can eat a whole
      team's turn with nothing to show for it.
- [ ] One printed scoring sheet (below) per team.
- [ ] The Competition Controller visible to the room, or announced aloud after each run.

## Format

Each team gets one timed run (use [the eight practice worlds page](practice-worlds.md) to pick a
world matching what your teams have practised on). Run the match for real, watch the Competition
Controller's score, and fill in the scoring sheet as it happens, not from memory afterward.

## The scoring sheet

| Event | When it happened | Points |
|---|---|---|
| Starting score | 0:00 | 0 |
| Identification 1 (correct/incorrect, area) | | |
| Identification 2 | | |
| ... | | |
| Lack of Progress events | | |
| Checkpoints crossed | | |
| Map submitted? (Y/N, correctness if known) | | |
| Exit sent? (Y/N) | | |
| **Final score** | | |

## Walked through once against a real run

Using [the complete-run page's](code-complete-run.md) own two real trials as the worked example,
exactly as this site ran them:

**Run 1 (fully autonomous, 90 seconds):**

| Event | When it happened | Points |
|---|---|---|
| Starting score | 0:00 | 0 |
| No sign ever detected | entire run | +0 |
| **Final score** | 1:30 | **0.0** |

A real, complete scoring sheet, and a legitimate result, [confirmed on that
page](code-complete-run.md#what-this-means): the wall-follower never got close enough to a sign to
attempt a report. Zero is a real score, not a broken sheet.

**Run 2 (staged next to a known sign, same controller):**

| Event | When it happened | Points |
|---|---|---|
| Starting score | 0:00 | 0 |
| Sign detected, stopped | 0:05 | 0 |
| Identification sent (estimate `x=18 z=11`, true position `x=6 z=13`) | 0:07 | 0 (misidentification, estimate ~12cm off, outside the 0.09m radius) |
| **Final score** | 0:10 | **0.0** |

Also a real, legitimate zero, for a different reason: the report itself was well-formed and sent at
the right moment, but the dead-reckoned position estimate drifted outside [the identification
radius](code-reporting.md#about-that-identification-range). Worth debriefing both runs with your
class, they show two different, real failure modes that both end in the same score.

**What a higher-scoring sheet looks like:** [the mapping page's real
run](code-mapping.md#step-3-a-real-run-mapping-bonus-included) is a good example to show
afterward, `0.0 → 22.5 → 17.5 → 19.25 → 41.65` across an identification, an LoP, an exit, and a map
bonus, all on one sheet.

## Debrief

- Compare each team's sheet against [the point-value table](rules-scoring.md#point-values) and
  confirm the arithmetic, not just the final number.
- Ask each team to name one thing their sheet shows they should practise next, [the debugging
  playbook](debugging-playbook.md) and [costly mistakes page](costly-mistakes.md) are good places to
  point them.

---

Next: [the printables pack](printables-pack.md).
