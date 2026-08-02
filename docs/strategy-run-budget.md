# Spending your eight minutes

To find out where a team's eight minutes actually goes, this page picks up after Track C proved
every scoring mechanism works in isolation. It's about the resource every team actually runs out of
first: time. Every claim on this page traces back to either the official rules or a real
number this site measured while building Track C, no invented pacing advice.

!!! note "Where this fits"
    First of six Strategy pages, building on everything in [Rules & Scoring](briefing.md) and
    [Track C](code-sensors.md). This page assumes you've read [reading the clock and the
    score](code-game-info.md) and [the exit message](code-exit.md).

---

## Two clocks

[Reading the clock and the score](code-game-info.md) measured this directly: a match runs an
**8-minute game clock** (`game_time_left` started at `479` seconds into a query fired 2 seconds in,
consistent with 480), alongside a separate, longer **real-world clock** (`real_time_left` started
around `599`, about 10 minutes). The second clock is slack for lag or a paused match, not extra
playing time. Therefore, don't plan around it.

## What running out the clock actually costs you

This is the one most teams get wrong, and it's a real behaviour confirmed by reading the supervisor's
own source, not a guess: **if the game clock reaches zero before you send an exit message, your map
bonus is still applied automatically, but the 10% exit bonus is not.** [The exit
page](code-exit.md#step-1-one-byte-two-conditions) already covers the exit bonus's two conditions;
the third, unwritten condition is that you have to ask for it. Running out the clock with an
unsubmitted map costs you both. However, running out the clock *with* a submitted map costs you only
the 10%, [worth weighing against `rules-scoring.md`'s numbers](rules-scoring.md#bonuses-applied-last)
for your own team's typical score.

## Where the time actually goes, per Track C's own measurements

- **To get an identification to register, budget real margin.** [Sending the
  exit message](code-exit.md#getting-this-run-clean-took-more-patience-than-it-looked-like-it-should)
  found that a report sent too soon after the robot last stopped moving can silently fail to
  register at all, seemingly because of physics-settling noise near the 1-second stillness
  threshold. Budget a few real seconds of stillness before every report, treating that `1.0`
  as a floor, not a target.
- **Exploration is where a real run's time actually disappears, and it can vanish entirely.**
  [The complete run page](code-complete-run.md) ran a fully autonomous controller for 90 real
  seconds and it spent the last 70-plus of them stuck in a small repeating loop, never finding a
  single victim. Time spent stuck is time spent scoring nothing, and [the wall-follower
  page](code-wall-follower.md) already showed this is what a reasonable,
  working wall-follower actually does on this exact maze.
- **A misidentification is instant, a correct one takes several seconds.** Reporting a wrong
  position or type costs a flat 5 points ([confirmed on the reporting
  page](code-reporting.md)) but the message itself is one packet, near-instant. Getting a *correct*
  report costs real time: stopping, waiting out the stillness threshold, estimating position,
  sending. [The complete run page](code-complete-run.md#what-this-means) measured about 5 seconds of
  movement being enough to drift a dead-reckoned estimate outside the identification radius, time
  spent moving before you stop to report is time spent accumulating error.
- **Lack of Progress costs you the same 5 points whether you ask for it or not**, [confirmed on the
  LoP page](code-lack-of-progress.md#step-1-lack-of-progress-isnt-a-bug-report-its-a-relocation),
  plus whatever time the relocate itself takes. The strategic question isn't "how do I avoid ever
  triggering LoP" — it's "given I'll pay 5 points either way if I'm stuck, how quickly can I notice
  and ask for it myself", rather than waiting out the passive 20-second timeout and losing that time
  too.

---

## A rough budget, built from the numbers above

Not a script, a shape. Every real team's split will differ, but the categories are fixed by how the
game actually works:

1. **A settling margin at the very start**, before your first message of any kind, [confirmed
   necessary above](#where-the-time-actually-goes-per-track-cs-own-measurements).
2. **Exploration**, the largest and least predictable share, and the one most likely to eat the
   whole clock if your navigation isn't better than [the wall-follower this site
   built](code-wall-follower.md).
3. **Stop-and-report time**, several seconds each, every time you find something real.
4. **A deliberate buffer before the clock runs out**, long enough to submit a map (if you have one)
   and send an exit, rather than letting the clock end the match for you and losing the 10% bonus
   automatically.

---

## Now think this through for your own team

- If your controller can reliably identify one victim, is it worth the risk of also submitting a
  map, given [the mapping bonus is a multiplier, not a flat amount](rules-scoring.md#bonuses-applied-last)
  that's worth more the more you've already scored, not a fixed payoff?
- Given [misidentification only costs 5 points, flat](code-reporting.md), is a fast, less accurate
  reporting strategy ever better than a slow, careful one? [Scoring maths for strategy
  decisions](strategy-scoring-maths.md) works through exactly this kind of comparison with real
  numbers.
- How much of your own team's practice time goes into exploration coverage versus reporting
  accuracy? Track C's own experience says coverage was the harder problem here. Is that true for
  your robot too?

---

Next: [scoring maths for strategy decisions](strategy-scoring-maths.md).
