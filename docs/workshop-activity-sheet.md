# Workshop activity sheet (printable)

One page per team for the [45-minute classroom workshop](workshop.md). Fill in your answers as you go
through Activities 1, 3, and 4. Activity 2 uses its own sheet, the
[cognitive target decoder](cognitive-target-decoder.md).

!!! tip "Printing this page"
    Use your browser's print (<kbd>Ctrl</kbd> or <kbd>Cmd</kbd> + <kbd>P</kbd>). The answer key is at
    the very bottom, on its own, so you can stop printing before it.

Team name: ______________________

## Activity 1 — Read the field

**1.** Your robot's floor-colour sensor reads green. Which two areas did it just cross between?

`________________________________`

**2.** True or false: a tile can be both a checkpoint and have an obstacle on it.

`________________________________`

**3.** A robot enters the same swamp tile for the third time. How much faster is simulated time
running there now?

`________________________________`

## Activity 2 — Decode the hazmat

Use the separate [cognitive target decoder](cognitive-target-decoder.md) sheet for this one.

## Activity 3 — Score the run

**Together:** *A robot identifies a letter victim on a floating tile in Area 2, gets the type right,
and visits one checkpoint. No penalties.*

```
TI (floating letter victim)      15
TT (correct type)               +____
CN (one checkpoint)             +____
                                  --
raw total                        ____
× Area 2 multiplier (1.25)       ____
```

**Your turn:** *A robot identifies a cognitive target on a linear tile in Area 3, gets the type right,
visits two checkpoints, and has one Lack of Progress.*

```
TI (linear cognitive target)     ____
TT (correct type)               +____
CN (two checkpoints)            +____
                                  --
raw total                        ____
× Area 3 multiplier (1.5)        ____
− LoP (flat, not multiplied)     −____
                                  --
final                            ____
```

## Activity 4 — Read the map

Looking at the projected map figure:

**1.** How many matrix cells represent the starting tile, and what value do they hold?

`________________________________`

**2.** What do the cells holding wall-token codes (like `F` or `H`) tell you?

`________________________________`

---

## Answer key

**Activity 1:** (1) Area 1 and Area 4 &nbsp;·&nbsp; (2) False &nbsp;·&nbsp; (3) ×7

**Activity 3, together:** TT +10, CN +10, raw 35, final **43.75**

**Activity 3, your turn:** TI 10, TT +20, CN +20, raw 50, ×1.5 = 75, − 5 = final **70**

**Activity 4:** (1) Four cells, each holding `5` &nbsp;·&nbsp; (2) They mark exactly which wall a
wall token sits on, using that token's own code
