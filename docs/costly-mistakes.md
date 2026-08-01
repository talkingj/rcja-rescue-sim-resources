# The rule violations that quietly cost the most

To understand the trades you didn't choose to make, review these four ways teams lose points (or
time, which [spending your eight minutes](strategy-run-budget.md) already established is the same
thing) without meaning to. Every one here cites its exact rule and its real cost — nothing
invented.

!!! note "Where this fits"
    Last of six Strategy pages. This is the flip side of [scoring maths for strategy
    decisions](strategy-scoring-maths.md): trades you didn't choose to make.

---

## 1. Misidentification: the flat cost that never scales down

Reporting a token that isn't there, isn't where you said it was, or has already been identified
costs a flat **−5**, confirmed in code and [demonstrated with a real score
change](code-reporting.md) (`22.5 → 17.5`). Unlike almost every other number on [the scoring
page](rules-scoring.md), this one **does not scale with the area multiplier** — a misidentification
in Area 4 costs exactly the same as one in Area 1. That makes misidentification quietly worse the
better your run is going otherwise, as it's the one penalty that never gets relatively cheaper as
your score climbs.

## 2. Decoy tokens: designed to look identical from a distance

[Confirmed on the tokens page](rules-tokens.md): some wall tokens are rendered with visible **3D
depth** instead of flat print, and are decoys. Reporting one costs exactly the same **−5**
misidentification as reporting a real token in the wrong place. The rules page's own words are
explicit that this isn't a softer or different penalty — it's the same one. A detector tuned only
on 2D contour shape, [like this site's own victim-detection
code](code-victim-detection.md), has no way to tell a decoy from the real thing
without specifically checking for depth. Therefore, it is worth testing your own detector against a
decoy deliberately rather than assuming flat-print logic generalises.

## 3. Reporting distance: a fixed radius, not a felt one

[Confirmed directly in the supervisor's source](code-reporting.md#about-that-identification-range):
the identification check uses a **fixed 0.09 m radius** on both your robot's real position and your
reported estimate. [Track C's own dead-reckoning attempt](code-complete-run.md) drifted about 12cm
in just 5 seconds of real movement, comfortably outside that radius. This is real evidence that
"close enough by eye" and "close enough for the supervisor" are not the same thing. Additionally,
this isn't really a rule violation so much as a rule most teams underestimate the strictness of,
until a position estimate that looks fine on screen scores a misidentification anyway.

## 4. Re-entering swamps: a time cost with no score line to warn you

[Confirmed in the official rules](official-rules-2026.md) and [the field
page](rules-field.md#swamps-obstacles-and-holes): a swamp runs the simulator's clock at **5×
speed** the first time your robot enters it, and each *repeat* entry into that *same* swamp adds
another full multiple, `6×`, `7×`, up to a cap of `10×`. There's no score penalty and no message
telling you this happened, [unlike misidentification or LoP, which at least show up in the match
history](code-lack-of-progress.md). A swamp re-entry just quietly burns your clock 6, 7, up to 10
times faster than normal while you're in it. Given [how tightly Track C's own trials found time
gets spent](strategy-run-budget.md), a wall-follower or exploration strategy that loops back through
the same swamp repeatedly is losing real playing time with nothing on screen announcing it.

---

## Now check your own controller against these four

- Does your detector distinguish flat tokens from 3D decoys, or would it report either the same
  way?
- Does your position estimate get *better* the longer you've been moving, or *worse*, like [this
  site's own dead-reckoning did](code-complete-run.md)?
- Does your exploration strategy ever double back through the same tile? If it does, and that tile
  is a swamp, you're paying for it whether or not anything on screen tells you so. Therefore,
  check your path logic against swamp positions before competition day.

---

Next: [an 8-week club curriculum](club-curriculum.md).
