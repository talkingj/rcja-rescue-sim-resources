# Provenance & attribution — official Erebus screenshots

Every image in this `official/` folder was taken from the **RoboCupJunior Erebus documentation**
project and is reused here under its licence. Keep this file with the images.

- **Source repository:** https://github.com/robocup-junior/erebus-document
- **Licence:** Apache License 2.0 (see the repo's `LICENSE`). Apache-2.0 permits redistribution
  provided attribution and the licence notice are retained — that is what this file does.
- **Copyright:** © RoboCupJunior Erebus contributors.
- **Retrieved:** 2026-07-20, from the `master` branch.

## Why these are reused (and Mac shots are not)
These are either the **installer dialog** (`windows/python-path.png` — the "Add Python to PATH"
checkbox) or **in-app UI that is identical on every operating system** (the Competition Controller,
the LOAD/start/pause buttons, etc.). Reusing them lets the Windows and Linux pages show the exact
screen a student sees without us owning a Windows/Linux machine.

**macOS install steps are verified and screenshotted for real on this Mac** (see `PLAN.md` §6);
the few `mac/` images here (`preferences*`, `initializing`) are staged only as a fallback/reference
and will be replaced by our own captures during `M1`/`R1` where the page shows a real step.

## When embedding one in a page
Add a short caption crediting the source, e.g.:

> *Screenshot: RoboCupJunior Erebus documentation, Apache-2.0.*

## File index
| File | Shows | Best used on |
|---|---|---|
| `installation/components.png` | The 3 pieces: Webots + Supervisor + Python | before-you-start |
| `windows/python-path.png` | Python installer "Add to PATH" checkbox | install-windows |
| `windows/download_erebus.png` | The Erebus release download | install-windows / -linux |
| `getting-started/main_screen_unloaded.png` | Competition Controller before LOAD | first-run |
| `getting-started/main_screen_loaded.png` | LOAD button turned orange (success check) | first-run |
| `getting-started/controller.png` | Controller panel anatomy | first-run |
| `getting-started/start_button.png` | Start button | first-run |
| `getting-started/pause_button.png` | Pause button | first-run |
| `getting-started/lop_button.png` | Restart / lack-of-progress button | first-run |
| `getting-started/reset_buttons.png` | Reset + settings (debug) buttons | first-run |
| `getting-started/manual.png` | Drag-arrows to move the robot by hand | first-run |
| `getting-started/showrobot.png` | Scene Tree → Show Robot Window fix | troubleshooting |
| `mac/preferences.png`, `mac/preferences_open.png` | Webots ▸ Preferences ▸ Python command | troubleshooting (fallback) |
| `mac/initializing.png` | "Initializing…" first-run library install | first-run (fallback) |
