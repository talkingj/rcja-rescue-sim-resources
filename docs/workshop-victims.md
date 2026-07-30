# Workshop 3: victim detection lab (60 min)

A facilitator guide for a hands-on lab built on [turning on the camera](code-camera.md),
[spotting a victim sign](code-victim-detection.md), and [reporting a
victim](code-reporting.md). Same format as [the sensors lab](workshop-sensors.md).

!!! note "Who this page is for"
    The teacher or mentor running the session. Students should have finished [the sensors
    lab](workshop-sensors.md) already, this workshop assumes comfort with `getDevice`, `enable`, and
    the timestep loop.

!!! note "On this page's timing"
    Same approach as [the sensors lab](workshop-sensors.md): every code sample and observed value
    below is exactly what [the three source pages](code-camera.md) verified for real. Minute
    allocations are a facilitator estimate, not a literal live-tested session.

## Learning objectives

By the end of the session, students should be able to:

- Enable a camera and read its resolution and buffer format.
- Adapt a contour-based detector and explain why its threshold has to match the camera's actual
  resolution, not a number copied from a sample.
- Explain the emitter message format well enough to report a token and see a real score change.

## Before you run it

**Time:** 60 minutes, flexible ±10.

**Group size:** pairs, one laptop with Webots per pair.

**Materials checklist:**

- [ ] Every laptop has completed [the sensors lab](workshop-sensors.md) material already.
- [ ] A way to project code and the Competition Controller's score display.

## Agenda at a glance

| Time | Activity | Format |
|---|---|---|
| 0–5 min | Intro: a camera is just another sensor | You narrate |
| 5–20 min | Activity 1 — Turn on the camera | Pairs, hands-on |
| 20–40 min | Activity 2 — Find the real threshold | Pairs, hands-on |
| 40–55 min | Activity 3 — Report it and watch the score | Pairs, hands-on |
| 55–60 min | Wrap-up | You narrate |

## Intro: a camera is just another sensor (5 min)

A camera is a device like any other, `getDevice`, `enable`, then read a value, except the value is a
big flat buffer of bytes instead of one number. The interesting part isn't the camera, it's what you
do with the numbers it returns.

## Activity 1 — Turn on the camera (15 min)

**Objective:** enable `camera_centre` and confirm its actual resolution rather than assuming one.

Have each pair adapt [the camera page's controller](code-camera.md#step-2-write-the-controller):
enable the camera and print its width, height, and the length of one `getImage()` buffer.

??? question "What should they see"
    [Confirmed on the camera page](code-camera.md), this robot's `camera_centre` is a small
    **64×40** image, `10240` bytes per frame (`64 × 40 × 4`, confirming BGRA). That's much lower
    resolution than students usually expect from "a camera", worth pausing on before Activity 2,
    it's the reason the next activity's threshold number matters so much.

## Activity 2 — Find the real threshold (20 min)

**Objective:** adapt the official victim-detection sample and discover why its default threshold
doesn't work here.

Give pairs [the victim-detection page's controller](code-victim-detection.md#step-2-write-the-controller)
already adapted with the *official* `1000` threshold first. Have them confirm it detects nothing,
even close to a sign.

??? question "Why, and what to change"
    [Confirmed on that page](code-victim-detection.md#step-3-why-the-official-threshold-didnt-work-here):
    a `1000`-pixel contour would need to cover 39% of this camera's 64×40 frame, which a sign never
    does. Lowering the threshold to **150** is what worked reliably in this site's own trial. Have
    pairs re-test at `150` and confirm real detections appear.

## Activity 3 — Report it and watch the score (15 min)

**Objective:** turn a detection into a real score change using the emitter.

Adapt [the reporting page's controller](code-reporting.md#step-2-write-the-controller): once a sign
is detected, stop, wait a few real seconds (not the bare 1-second minimum, [confirmed necessary on
the exit page](code-exit.md#getting-this-run-clean-took-more-patience-than-it-looked-like-it-should)),
then send the 9-byte identification.

??? question "What a real run looked like"
    [Confirmed on the reporting page](code-reporting.md#step-3-a-real-run-score-before-and-after):
    score went `0.0 → 22.5` on a correct report. The exact number depends on which token and area a
    given team's robot happens to be near, the shape of the result (a real, visible jump in the
    Competition Controller's score display) is what to look for, not that exact number.

## Wrap-up (5 min)

Recap: a low-resolution camera means official sample constants may not transfer as-is, always
re-tune against what your own sensor actually reports; and the identification message needs real
stillness margin, not the bare rules minimum. Point ahead to [the complete-run
page](code-complete-run.md) for what happens when detection and reporting are combined with
autonomous exploration, and why that's still an open problem worth working on.

## If you're running short on time

Cut in this order:

1. **Skip testing the official `1000` threshold in Activity 2.** Tell pairs it doesn't work here
   and why, then have them start straight from `150`.
2. **In Activity 3, use a fixed, made-up position instead of a real detected one.** The point is
   seeing the score change, not solving position estimation live, that's [a much harder, still-open
   problem this site found the hard way](code-complete-run.md).
3. **Shorten Activity 1 to just printing the resolution**, skip having pairs confirm the exact byte
   count matches `width × height × 4`.

Don't cut the real-threshold discovery in Activity 2, it's the single most concrete "official
samples aren't always right for your build" lesson in this whole lab.

---

Next: [assessment rubric and progress tracker](assessment-rubric.md).
