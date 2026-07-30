# Scoring cheat sheet (one page)

A condensed, printable reference for everything on the [Victims and hazmats](rules-tokens.md),
[How points are earned](rules-scoring.md), and [Drawing the map](rules-map-format.md) pages. If
anything here doesn't make sense, those pages explain it in full.

!!! tip "Printing this page"
    Use your browser's print (<kbd>Ctrl</kbd> or <kbd>Cmd</kbd> + <kbd>P</kbd>) and it'll fit on one
    or two sheets. The sidebar and search box won't print.

## Letter victims

| Symbol | Status | Code |
|---|---|---|
| Φ | Harmed | H |
| Ψ | Stable | S |
| Ω | Unharmed | U |

## Cognitive targets: ring colors → values

| Color | Value |
|---|---|
| Black | −2 |
| Red | −1 |
| Yellow | 0 |
| Green | 1 |
| Blue | 2 |

Sum all 5 (center + 4 rings), center to outward. Adjacent same-color rings still count separately.

| Sum | Hazmat | Code |
|---|---|---|
| 0 | Flammable Gas | F |
| 1 | Poison | P |
| 2 | Corrosive | C |
| 3 | Organic Peroxide | O |
| anything else | Fake — don't report | — |

## Point values

| | Linear tile, Areas 1–3 | Floating tile, Areas 1–3 / anywhere in Area 4 |
|---|---|---|
| Letter victim TI | 5 | 15 |
| Letter victim TT (correct type) | +10 | +10 |
| Cognitive target TI | 10 | 30 |
| Cognitive target TT (correct type) | +20 | +20 |

- **CN** (checkpoint): +10, anywhere.
- **TMI** (misidentification): −5, anywhere.
- **LoP** (lack of progress): −5, anywhere.
- Identification only counts if your robot's center is within **half a tile** of the token.
- TI, TT, and CN get scaled by the area multiplier below. TMI and LoP never do.

## Area multipliers

| Area | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| Multiplier | ×1 | ×1.25 | ×1.5 | ×2 |

## Bonuses (applied last, in order)

1. **Exit bonus:** +10%, if at least one token was identified **and** the robot returns to the start
   and sends `exit`.
2. **Mapping bonus:** ×(correctness × 1.2 + 1), so between ×1 (map ignored/all wrong) and ×2.2
   (perfect map).

```
final score = ((TI + TT + CN, per area × area multiplier, summed)
               − (5 × TMI) − (5 × LoP))
              × exit bonus (1 or 1.1)
              × mapping bonus (1 to 2.2)
```

## Map matrix legend

| Value | Means | | Value | Means |
|---|---|---|---|---|
| `0` | Nothing | | `4` | Checkpoint |
| `1` | Wall | | `5` | Starting tile |
| `2` | Hole | | `x` | Obstacle |
| `3` | Swamp | | `*` | Area 4 (whole area) |

Passages: `b` 1↔2 · `y` 1↔3 · `g` 1↔4 · `p` 2↔3 · `o` 2↔4 · `r` 3↔4

Wall tokens use their own code (`H S U F P C O`); multiple on one wall segment get concatenated in one
cell. Matrix resolution is quarter-tiles plus a cell for every edge and vertex between them, so a
single 1×1 tile is more than one cell wide. Full walkthrough: [Drawing the map](rules-map-format.md).

## Unfamiliar word?

Every term here is in the [glossary](glossary.md#rules-scoring-words).
