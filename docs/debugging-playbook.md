# The debugging playbook

Every symptom below is one this site actually hit while building [Track C](code-sensors.md), not a
hypothetical. Same format as [the setup troubleshooting page](troubleshooting.md): what you'll see,
why it happens, and what to do.

!!! tip "How to use this page"
    Search for the exact behaviour you're seeing. If your controller involves the emitter/receiver,
    the camera, or the wall-follower's stuck-recovery, there's a good chance it's already here.

## Quick index

| What you're seeing | Jump to |
|---|---|
| A device call fails with no visible error at all | [1](#1-a-device-call-fails-with-no-visible-error-at-all) |
| A camera-based detector never detects anything | [2](#2-a-camera-based-detector-never-detects-anything) |
| A report or command seems to do nothing | [3](#3-a-report-or-command-seems-to-do-nothing) |
| Score doesn't change right after an action | [4](#4-score-doesnt-change-right-after-an-action) |
| A trial or match produces almost no output at all | [5](#5-a-trial-or-match-produces-almost-no-output-at-all) |
| Robot gets physically stuck against a wall | [6](#6-robot-gets-physically-stuck-against-a-wall) |
| `struct.pack`/`struct.unpack` raises an error | [7](#7-structpackstructunpack-raises-an-error) |

---

## 1. A device call fails with no visible error at all

**You'll see:** your print statements stop appearing right after the controller starts. The only
line in the console is `INFO: 'robot0Controller' controller exited successfully.` No traceback, no
`AttributeError`, nothing pointing at the actual problem.

**Why:** a misspelled `robot.getDevice("...")` name. [Confirmed on the sensors
page](code-sensors.md): Webots doesn't raise a visible error here, the controller just silently
stops on the bad call.

**Fix:** check every device name for typos against the robot's proto file. This is the single most
likely cause of "my controller does nothing at all."

---

## 2. A camera-based detector never detects anything

**You'll see:** [victim detection code](code-victim-detection.md) that runs without errors, prints
nothing, even standing directly in front of a sign.

**Why:** the official sample's `cv2.contourArea(c) > 1000` threshold, [confirmed on that
page](code-victim-detection.md), needs a contour covering 39% of this robot's small 64×40 camera
frame, which a sign never reaches before you're uncomfortably close to the wall.

**Fix:** lower the area threshold, `150` worked reliably in this site's own trials. Print the
largest contour area you're actually seeing before picking a number, don't guess.

---

## 3. A report or command seems to do nothing

**You'll see:** you send a 9-byte identification, or a single control byte like `'L'` or `'E'`, and
nothing appears to happen.

**Why, for identification specifically:** the supervisor only honors a report if your robot has been
stationary for a full second, [confirmed on the reporting page](code-reporting.md). Sent while still
"moving" (including brief physics-settling right after a spawn or relocate), it's silently dropped.

**Fix:** wait longer than the bare `1.0` second minimum, [several extra seconds of margin
consistently fixed this in this site's own trials](code-exit.md#getting-this-run-clean-took-more-patience-than-it-looked-like-it-should).

---

## 4. Score doesn't change right after an action

**You'll see:** you send a report or trigger LoP, immediately query the score, and it still shows
the old value.

**Why:** [confirmed on the game-info page](code-game-info.md), a score update isn't guaranteed
visible on the very next poll after the action that caused it, the supervisor processes messages
independently, one simulation step apart in this site's own measurement.

**Fix:** poll again a moment later before concluding nothing happened. If you need to react
instantly, poll every timestep instead of once a second.

---

## 5. A trial or match produces almost no output at all

**You'll see:** your controller exits (or the trial ends) after producing almost no console output,
with no traceback and no crash flag.

**Why:** [confirmed repeatedly across several resources on this site](code-lack-of-progress.md), this
happens intermittently and isn't tied to any specific bug, an identical rerun of the identical
controller has consistently produced a clean, full-length run afterward.

**Fix:** retry once or twice before assuming your code is broken. If a third identical attempt still
fails the same way, then start suspecting the controller itself.

---

## 6. Robot gets physically stuck against a wall

**You'll see:** a wall-following or driving controller stalls, wheels spinning against a wall or
stuck oscillating in place.

**Why:** [the wall-follower page](code-wall-follower.md) found this happens on real mazes even with
reasonable-looking avoidance logic, tight corridors and corners are genuinely harder to escape than
they look.

**Fix:** a dedicated stuck-detector (count consecutive steps with the front sensor blocked, then
reverse and turn hard before resuming) recovers from most cases, [as built and trialled on that
page](code-wall-follower.md). It doesn't guarantee full maze coverage, [that limitation is real and
still unsolved on this site](code-complete-run.md).

---

## 7. `struct.pack`/`struct.unpack` raises an error

**You'll see:** a `struct.error` naming a size or format mismatch.

**Why:** every message on this site has a fixed, exact byte length: 1 byte for a control message
(`'G'`, `'L'`, `'E'`, `'M'`), 9 bytes for an identification (`'i i c'`), 16 bytes for a game-info
reply (`'c f i i'`). [Confirmed across the reporting](code-reporting.md), [game-info](code-game-info.md),
and [mapping](code-mapping.md) pages, a mismatched format string or a Python `str` where `bytes` is
expected (a missing `.encode()`) is the usual cause.

**Fix:** match the format string exactly to the message type you're sending or expecting, and
double check every string argument is actually encoded to bytes first.

---

Next: [pre-run and competition-day checklist](competition-day.md).
