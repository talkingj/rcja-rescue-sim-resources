# Quick-start (one page)

A condensed, printable version of this whole site for anyone who's already read it once and just
wants a checklist. If any step doesn't make sense, it's explained in full on the linked page.

!!! tip "Printing this page"
    Use your browser's print (<kbd>Ctrl</kbd> or <kbd>Cmd</kbd> + <kbd>P</kbd>) and it'll fit on
    one or two sheets. The sidebar and search box won't print.

## The three installs, in order

| # | Install | Exact version | Guide |
|---|---|---|---|
| 1 | Python | **3.10.11**, 64-bit, install fresh even if you already have one | [Windows](install-windows.md) · [macOS](install-mac.md) · [Linux](install-linux.md) |
| 2 | Webots | **R2023b** exactly — not the newest | same pages as above |
| 3 | Erebus | latest release, **"Source code (zip)"** (there is no "Release Build") | [releases page](https://github.com/robocup-junior/erebus/releases/latest) |

Windows: tick **"Add python.exe to PATH"** during the Python install. Skipping this causes most
Windows problems — see [When it goes wrong](troubleshooting.md#7-typing-python-does-nothing-windows).

## First run

1. Open **`game/worlds/world1.wbt`** from your unzipped Erebus folder.
2. Wait out **"Initializing…"** the first time (1–3 minutes, normal).
3. In the Competition Controller panel, press **LOAD**, pick
   `player_controllers/ExamplePlayerController_updated.py`.
4. Press **start**. ✅ The robot should drive itself around the maze.

Full walkthrough with screenshots: [Your first run](first-run.md).

## Make it move

1. Open `ExamplePlayerController_updated.py` in a text editor.
2. Line 4: change `max_velocity = 6.28` to `max_velocity = 3.14`. Save.
3. In Webots: **reset** → **LOAD** the file again → **start**. ✅ Robot moves noticeably slower.

Full walkthrough: [Make it move](make-it-move.md).

## If something breaks

Jump straight to [When it goes wrong](troubleshooting.md) and search (<kbd>Ctrl</kbd>/<kbd>Cmd</kbd>
+ <kbd>F</kbd>) for what you're seeing. The two most common snags:

- **Windows PATH** not ticked during Python install.
- **`ModuleNotFoundError: No module named 'cv2'`** → run
  `python -m pip install numpy termcolor requests opencv-python pillow overrides` (drop the `-m` /
  use `python3` on macOS or Linux, see [troubleshooting §3](troubleshooting.md#3-its-stuck-on-initializing)).

## Unfamiliar word?

Every term here (PATH, controller, DMG, unzip, and more) is in the [Glossary](glossary.md).
