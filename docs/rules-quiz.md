# Rules self-check

A mixed set of questions covering the last four pages: field, tokens, scoring, and the map format.
Not graded, not timed, just a way to find out what stuck before you start writing code that depends on
it. Work out each answer yourself before opening it.

!!! note "Where this fits"
    Last of five pages building on the [official rules](official-rules-2026.md). If any question here
    stumps you, the linked page has the full explanation.

## The field

??? question "1. Which area uses quarter-tiles *and* can round a 90° corner into a curve?"
    **Area 3.** Area 2 also uses quarter-tiles, but only Area 3 is allowed to round corners.
    ([Understanding the field](rules-field.md#the-four-areas))

??? question "2. Your robot's floor-color sensor reads green. Which two areas did it just cross between?"
    **Area 1 and Area 4.** Check the passage-color table: green connects Areas 1 and 4.
    ([Understanding the field](rules-field.md#the-four-areas))

??? question "3. What's the actual difference between a linear tile and a floating tile?"
    A linear tile can be reached from the starting tile by consistently following the same wall
    (left or right). A floating tile can't; reaching it means crossing open space at some point.
    ([Understanding the field](rules-field.md#linear-tiles-vs-floating-tiles))

??? question "4. True or false: a tile can be both a checkpoint and have an obstacle on it."
    **False.** A tile is never two of: swamp, hole, checkpoint, starting tile, obstacle tile, or
    passage, at the same time. ([Understanding the field](rules-field.md#swamps-obstacles-and-holes))

??? question "5. A robot enters the same swamp tile for the third time. How much faster is simulated
    time running there now?"
    **×7.** First entry is ×5, then it climbs by one each time the robot re-enters that *same* swamp:
    ×5, ×6, ×7, up to a cap of ×10.
    ([Understanding the field](rules-field.md#swamps-obstacles-and-holes))

## Victims and hazmats

??? question "6. What does the symbol Ψ mean?"
    **Stable victim** (health status code S). Φ is harmed, Ω is unharmed.
    ([Victims and hazmats](rules-tokens.md#letter-victims))

??? question "7. A cognitive target's rings, center to outward, are: Blue, Blue, Yellow, Yellow, Black.
    What hazmat is this?"
    2 + 2 + 0 + 0 − 2 = **2 → Corrosive [C]**.
    ([Victims and hazmats](rules-tokens.md#cognitive-targets-reading-the-rings))

??? question "8. Why must a robot never report the 3D/textured wall tokens?"
    They're decoys, fake tokens included specifically to test whether your robot is actually sensing
    depth rather than just pattern-matching a shape. Reporting one counts as a misidentification, a
    real point deduction. ([Victims and hazmats](rules-tokens.md#letter-victims))

## Scoring

??? question "9. A robot correctly identifies a letter victim's type on a floating tile. How many raw
    points is that, before the area multiplier?"
    **25.** 15 for TI (floating-tile letter victim) + 10 for TT (correct type) = 25, before that
    area's multiplier is applied. ([How points are earned](rules-scoring.md#point-values))

??? question "10. Why are TMI and LoP always a flat −5, regardless of which area they happen in?"
    The area multiplier only applies to TI, TT, and CN (the rewards). Penalties are flat everywhere,
    so a mistake in the hardest area doesn't cost proportionally more than the same mistake in
    Area 1. ([How points are earned](rules-scoring.md#area-multipliers))

??? question "11. What two things must both be true for a team to earn the exit bonus?"
    The robot must have identified **at least one** wall token, **and** it must return to the
    starting tile while sending an `exit` command before time runs out.
    ([How points are earned](rules-scoring.md#bonuses-applied-last))

??? question "12. A team's score is 500 points before bonuses. They earn the exit bonus and a mapping
    bonus multiplier of 1.6. What's their final score?"
    500 × 1.1 (exit bonus) = 550, then 550 × 1.6 (mapping bonus) = **880**.
    ([How points are earned](rules-scoring.md#bonuses-applied-last))

## The map format

??? question "13. What value represents a hole in the map matrix?"
    **`2`.** (`1` = wall, `2` = hole, `3` = swamp, `4` = checkpoint, `5` = starting tile.)
    ([Drawing the map](rules-map-format.md#the-legend))

??? question "14. Why does a single 1×1 tile end up needing more than one matrix cell?"
    The matrix works at quarter-tile resolution, and it also gives a separate cell to every edge and
    vertex between quarter-tiles (where a wall might sit). One tile is a 2×2 block of quarter-tiles
    plus all the edge/vertex cells around them.
    ([Drawing the map](rules-map-format.md#the-resolution-quarter-tiles-not-tiles))

??? question "15. What symbol fills in all of Area 4 on the map matrix, and why not the usual codes?"
    **`*`.** Area 4 isn't built on a tile grid, its layout is arbitrary, so there's no quarter-tile
    system to encode it against, and the rules just mark the whole area with `*` instead.
    ([Drawing the map](rules-map-format.md#the-legend))

## How did you do?

- **13–15 right:** you're ready to start turning this into strategy. Go build.
- **8–12 right:** re-read whichever page(s) tripped you up, then come back to just those questions.
- **Under 8:** that's fine, this is a lot of rules at once. Start over from
  [Understanding the field](rules-field.md) and take it one page at a time.

Want a printable quick-reference instead of re-reading full pages? See the
[scoring cheat sheet](scoring-cheat-sheet.md).
