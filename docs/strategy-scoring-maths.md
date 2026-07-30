# Scoring maths for strategy decisions

[The scoring page](rules-scoring.md) defines every point value. This page uses them to work three
real decisions all the way through, by hand, the way you'd actually reason about them mid-season.
Every number below either comes straight from [the scoring page's own
table](rules-scoring.md#point-values) or from a real measurement earlier on this site.

!!! note "Where this fits"
    Second of six Strategy pages. Read [spending your eight minutes](strategy-run-budget.md) first,
    this page assumes its two-clock and exit-bonus findings.

---

## Comparison 1: is the map bonus worth the code?

[The mapping bonus](rules-scoring.md#bonuses-applied-last) is `correctness × 1.2 + 1` applied as a
**multiplier on your entire score**, not a flat amount. That single fact changes the answer
depending on how much you've already scored.

Say your robot has scored `20` points from victims and hazmats so far, and you're deciding whether
to spend remaining time building and submitting a map instead of looking for one more token.

| Map correctness | Multiplier | Final score | Gain over no map |
|---|---|---|---|
| 0% (submitted anyway) | `0 × 1.2 + 1 = 1.0` | `20 × 1.0 = 20` | `+0` |
| 50% | `0.5 × 1.2 + 1 = 1.6` | `20 × 1.6 = 32` | `+12` |
| 97% (Track C's own real result) | `0.97 × 1.2 + 1 ≈ 2.164` | `20 × 2.164 ≈ 43.3` | `+23.3` |

[The complete run page](code-complete-run.md) measured almost exactly this multiplier for real: a
score of `19.25` became `41.65` after submitting a matrix that worked out to about 97%
correctness, a gain of `22.4`, consistent with the table above.

**The maths says:** a mediocre map is nearly worthless (`+0` to `+12` isn't much for the coding
effort a matrix takes, see [building the map matrix](code-mapping.md) for how much work that is).
A strong map is worth more than almost anything else on the field, because it multiplies work
you've *already* banked. The map bonus rewards teams that have already scored well more than teams
that haven't, it is not a flat consolation prize.

---

## Comparison 2: is a floating tile worth the detour?

[The point table](rules-scoring.md#point-values) pays floating tiles and Area 4 tiles **3× the
linear-tile rate for TI** (`15` vs `5` for a letter victim, `30` vs `10` for a cognitive target),
with the TT bonus staying flat either way (`+10` letter, `+20` target).

Take a letter victim, type correctly identified, on a tile in Area 2 (`×1.25` multiplier, [per the
scoring page](rules-scoring.md#area-multipliers)):

| Tile type | Raw (TI + TT) | With Area 2 multiplier |
|---|---|---|
| Linear tile | `5 + 10 = 15` | `15 × 1.25 = 18.75` |
| Floating tile | `15 + 10 = 25` | `25 × 1.25 = 31.25` |

The floating tile is worth `12.5` more, before even asking how much longer it takes to reach one.
[Track C's own wall-follower](code-wall-follower.md) found that reaching *any* specific tile
reliably, floating or not, is still an open problem here, so "worth it" has to be weighed against a
real risk: time spent detouring toward a floating tile is time that might instead be spent finding
two more linear-tile victims, or exiting before the clock forfeits your bonus (see [spending your
eight minutes](strategy-run-budget.md)). The maths favours the floating tile only if your team's
navigation is good enough to actually reach it without burning disproportionate time.

---

## Comparison 3: keep exploring, or exit now?

This is the sharpest trade this site has real numbers for. Say your robot has scored `20` points
and has correctly identified at least one token (satisfying [the exit bonus's second
condition](code-exit.md#step-1-one-byte-two-conditions)), and is standing on the start tile with
two minutes of game clock left. Two options:

- **Exit now.** Score becomes `20 × 1.1 = 22`, guaranteed, [confirmed as a real mechanism on the
  exit page](code-exit.md).
- **Keep exploring.** Best case, you find one more linear-tile victim, type correct, no area
  multiplier: `+15`, then exit, `(20 + 15) × 1.1 = 38.5`. Worst case, per [spending your eight
  minutes](strategy-run-budget.md#what-running-out-the-clock-actually-costs-you), the clock runs
  out before you get back to the start tile to exit: you keep your `20` (and your map bonus, if you
  submitted one) but **forfeit the `2.2`-point exit bonus entirely**, landing at `20`, worse than
  exiting immediately, before even counting the real risk of a `-5` misidentification or LoP along
  the way.

**The maths says:** exiting now is a guaranteed `22`. Exploring further has a strictly higher
ceiling (`38.5`) but a floor *below* the guaranteed option (`20`, or less with a misidentification).
Whether that trade is worth it depends entirely on how confident your team is in finding another
token in the time left, this page can't answer that for you, but it can tell you the exact number
you're risking to try.

---

## Now work through your own numbers

- Redo Comparison 1 with your own team's typical real score instead of `20`. The break-even point
  where a map becomes "worth it" moves depending on how much you usually score first.
- Redo Comparison 3 with your own team's real identification success rate instead of "best case /
  worst case", if you're only right half the time, what does the expected value actually favour?
- The costly mistakes page (later in this series) covers the flip side of this page: the ways teams
  lose points they didn't need to, rather than the trades they chose to make.

---

Next: [the eight practice worlds and what each one is for](practice-worlds.md).
