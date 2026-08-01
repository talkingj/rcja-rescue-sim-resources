# Classroom workshop (45–60 min)

To deliver the Rules & Scoring material as one live class period instead of solo reading, use this facilitator guide. You narrate, time each activity, and debrief as a group;
the lesson pages become the follow-up reading, not the in-class material.

!!! note "Who this page is for"
    This page is written for the teacher or mentor running the session. If you're a student looking
    for the material itself, start at the [quick briefing](briefing.md) instead.

## Learning objectives

By the end of the session, students should be able to:

- Name the four field areas and read a passage colour to know which two areas it connects.
- Decode a cognitive target's rings into a hazmat type.
- Compute a simple score from the point-value table, including an area multiplier.
- Read the basic shape of a map-matrix legend.

## Before you run it

**Time:** 50 minutes, flexible ±10. Every activity below has a fallback cut if you're running long,
see [If you're running short on time](#if-youre-running-short-on-time).

**Group size:** works in pairs or small teams of 3–4. However, bigger groups tend to let one student do all
the thinking.

**Materials checklist:**

- [ ] One printed [cognitive target decoder](cognitive-target-decoder.md) per team (print only the
      first page, up to the "Answer key" divider, keep the key for yourself).
- [ ] One printed [workshop activity sheet](workshop-activity-sheet.md) per team.
- [ ] A way to project images: [Understanding the field](rules-field.md) and
      [Drawing the map](rules-map-format.md) both have the figures you'll need, or open
      [scoring-cheat-sheet.md](scoring-cheat-sheet.md) for the point-value table.
- [ ] A visible timer.

To cover the essential rules and scoring without a full lecture, the session runs four short
activities against a timer, each targeting a different skill from the lesson pages.

## Agenda at a glance

| Time | Activity | Format |
|---|---|---|
| 0–5 min | Intro: the scenario | You narrate |
| 5–15 min | Activity 1 — Read the Field | Rapid-fire Q&A |
| 15–25 min | Activity 2 — Decode the Hazmat | Team race |
| 25–40 min | Activity 3 — Score the Run | Together, then pairs |
| 40–50 min | Activity 4 — Read the Map | Team Q&A |
| 50–55 min | Wrap-up + homework | You narrate |

## Intro: the scenario (5 min)

Read or paraphrase the top of the [quick briefing](briefing.md#the-scenario): the robot explores
alone, searches walls for tokens, maps as it goes, and isn't eliminated for getting stuck — it's just
sent back to the last checkpoint. This is the only slide-style narration in the session; everything
after this is hands-on.

## Activity 1 — Read the Field (10 min)

**Objective:** recognise the four areas, passage colours, tile types, and hazards.

Project the two field figures from [Understanding the field](rules-field.md#the-four-areas) (the area
overview and the passage-colour table). Then run these five questions rapid-fire, cold-call or show of
hands, no writing needed — they're pulled directly from [Rules self-check](rules-quiz.md):

1. Which area uses quarter-tiles *and* can round a 90° corner into a curve?
2. Your robot's floor-colour sensor reads green. Which two areas did it just cross between?
3. What's the actual difference between a linear tile and a floating tile?
4. True or false: a tile can be both a checkpoint and have an obstacle on it.
5. A robot enters the same swamp tile for the third time. How much faster is simulated time running
   there now?

??? question "Answer key"
    1. **Area 3.**
    2. **Area 1 and Area 4** (green connects them).
    3. A linear tile is reachable by always following one wall; a floating tile isn't.
    4. **False** — a tile is never two of those things at once.
    5. **×7** (×5 first entry, then +1 per re-entry, up to ×10).

## Activity 2 — Decode the Hazmat (10 min)

**Objective:** practice the cognitive-target ring-decoding formula under a bit of time pressure.

Hand out the printed [cognitive target decoder](cognitive-target-decoder.md) sheets, one per team.
Give teams **6 minutes** to decode all six targets (colour → value → sum → hazmat). First team with all
six correct can call it out, but let everyone finish before debriefing.

Debrief using the answer key already printed at the bottom of that page. If a team gets one wrong,
the most common mistake is merging two adjacent same-colour rings into one — remind them every ring
counts separately.

## Activity 3 — Score the Run (15 min)

**Objective:** apply the point-value table and an area multiplier to compute an actual score.

The full official worked example (on [How points are earned](rules-scoring.md)) covers all four areas
at once and is too dense to compute live in a few minutes. Therefore, use these two smaller scenarios instead.
Have the [scoring cheat sheet](scoring-cheat-sheet.md) point-value table visible.

**Together, on the board (5 min):** *A robot identifies a letter victim on a floating tile in Area 2,
gets the type right, and visits one checkpoint. No penalties.*

Walk it as a class:

```
TI (floating letter victim)      15
TT (correct type)               +10
CN (one checkpoint)             +10
                                  --
raw total                        35
× Area 2 multiplier (1.25)     43.75
```

**In pairs (8 min), then reveal:** *A robot identifies a cognitive target on a linear tile in Area 3,
gets the type right, visits two checkpoints, and has one Lack of Progress.*

??? question "Answer"
    ```
    TI (linear cognitive target)     10
    TT (correct type)               +20
    CN (two checkpoints, 2×10)      +20
                                      --
    raw total                        50
    × Area 3 multiplier (1.5)        75
    − LoP (flat, not multiplied)     −5
                                      --
    final                            70
    ```
    The LoP penalty is flat and applied *after* the multiplier — that's the detail most pairs miss.

## Activity 4 — Read the Map (10 min)

**Objective:** recognise the shape of a map-matrix legend without doing a full encoding from scratch.

Project the simplest map figure from
[Drawing the map](rules-map-format.md#worked-example-the-smallest-possible-map). In teams, have
students answer just two things out loud or on their activity sheet:

- Which cells in the matrix represent the starting tile, and how many of them are there?
- Which cells hold the wall tokens, and what do their values mean?

Confirm as a class: four `5` cells (one per quarter-tile of the starting tile), and the token cells
read `F` and `H` exactly where those tokens sit on the real wall. Additionally, full encoding practice is homework
material, not something to start from scratch with 10 minutes left.

## Wrap-up (5 min)

Recap the strategy takeaways from the
[quick briefing](briefing.md#strategy-what-actually-moves-the-score): checkpoints are a safety net,
floating tiles pay more, don't re-enter the same swamp, the exit bonus is free points if you budget
time for it, and the map is worth up to ×2.2.

Assign as follow-up for teams that want to go deeper on their own: the four full lesson pages
([field](rules-field.md), [tokens](rules-tokens.md), [scoring](rules-scoring.md),
[map format](rules-map-format.md)), the longer [self-check quiz](rules-quiz.md), and the printable
[scoring cheat sheet](scoring-cheat-sheet.md) to keep.

## If you're running short on time

Cut in this order:

1. **Drop Activity 4 entirely.** It's the most skippable, point students to
   [Drawing the map](rules-map-format.md) as homework instead.
2. **Trim Activity 3 to just the "together" scenario.** Skip the paired one, reveal the answer
   verbally instead of having pairs work it.
3. **Trim Activity 1 to 3 questions** instead of 5, questions 2 and 4 are the fastest to cut.

Don't cut Activity 2 — it's the most engaging part of the session and needs the least setup.
