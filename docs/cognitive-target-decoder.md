# Cognitive target decoder (practice sheet)

A printable practice sheet for decoding cognitive targets by hand, no simulator required. Full
explanation: [Victims and hazmats](rules-tokens.md#cognitive-targets-reading-the-rings).

!!! tip "Printing this page"
    Use your browser's print (<kbd>Ctrl</kbd> or <kbd>Cmd</kbd> + <kbd>P</kbd>). The answer key is
    at the very bottom, on its own, so you can stop printing before it if you're handing this out.

## The rule

Read a cognitive target's colours from the **centre outward** (centre circle, ring 1, ring 2, ring 3,
ring 4). Convert each colour to a number and add all five together.

| Colour | Value |
|---|---|
| Black | −2 |
| Red | −1 |
| Yellow | 0 |
| Green | 1 |
| Blue | 2 |

| Sum | Hazmat |
|---|---|
| 0 | Flammable Gas [F] |
| 1 | Poison [P] |
| 2 | Corrosive [C] |
| 3 | Organic Peroxide [O] |
| anything else | Fake target |

Adjacent rings of the same colour still count separately, never merge them.

## Decode these six

For each one, write out the running total, then the final hazmat (or "fake").

**1.** Yellow, Yellow, Yellow, Yellow, Yellow

`____ + ____ + ____ + ____ + ____ = ____  →  ____________`

**2.** Blue, Black, Yellow, Yellow, Green

`____ + ____ + ____ + ____ + ____ = ____  →  ____________`

**3.** Green, Blue, Yellow, Red, Yellow

`____ + ____ + ____ + ____ + ____ = ____  →  ____________`

**4.** Blue, Blue, Red, Yellow, Yellow

`____ + ____ + ____ + ____ + ____ = ____  →  ____________`

**5.** Red, Red, Black, Yellow, Blue

`____ + ____ + ____ + ____ + ____ = ____  →  ____________`

**6.** Green, Green, Green, Green, Green

`____ + ____ + ____ + ____ + ____ = ____  →  ____________`

---

## Answer key

1. 0+0+0+0+0 = 0 → **Flammable Gas [F]**
2. 2−2+0+0+1 = 1 → **Poison [P]**
3. 1+2+0−1+0 = 2 → **Corrosive [C]**
4. 2+2−1+0+0 = 3 → **Organic Peroxide [O]**
5. −1−1−2+0+2 = −2 → **Fake target**
6. 1+1+1+1+1 = 5 → **Fake target**
