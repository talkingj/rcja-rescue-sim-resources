# Install on macOS

This page sets everything up on a Mac. Do the three steps **in order** — each one needs the one
before it. Give yourself about 30 minutes the first time.

!!! tip "Before you begin"
    Read [Before you start](before-you-start.md) first if you haven't — it explains what these
    three pieces (Python, Webots, Erebus) are. Every unfamiliar word is in the
    [Glossary](glossary.md).

!!! note "Apple Silicon (M1/M2/M3) or Intel — same steps"
    Both use the exact same downloads below. The Webots R2023b installer is *universal* (one file
    works on both), and the Python installer is too. You don't need to know which chip you have.

---

## Step 1 — Install Python

Python is the language your robot's instructions are written in.

1. Go to **<https://www.python.org/downloads/macos/>**.
2. Download **Python 3.10.11** — the "macOS 64-bit universal2 installer". *(It's the last 3.10 with
   a ready-made Mac installer; that's the version this guide uses.)*
3. Open the downloaded file (`python-3.10.11-macos11.pkg`) and click through the installer:
   **Continue → Continue → Agree → Install**. Enter your Mac password if it asks. Then **Close**.

!!! warning "Install this even if you think you already have Python"
    Macs come with a Python, and if you've used Homebrew or Anaconda you have more. **Install this
    fresh python.org version anyway.** Mixing the others with Webots is the most common cause of
    "it installed but the robot won't run" on a Mac — a fresh python.org 3.10 avoids it.

!!! success "You should now have"
    Python installed. To check: open **Terminal** (press <kbd>Cmd</kbd> + <kbd>Space</kbd>, type
    *Terminal*, press <kbd>Enter</kbd>), then type `python3.10 --version` and press
    <kbd>Enter</kbd>. You should see `Python 3.10.11`.

---

## Step 2 — Install Webots

Webots is the simulator — the 3D app where the maze and robot live.

1. Download the **exact** version Erebus needs, **Webots R2023b**, straight from here:
   **[webots-R2023b.dmg](https://github.com/cyberbotics/webots/releases/download/R2023b/webots-R2023b.dmg)**
   *(about 100 MB to download; it expands to roughly 1 GB once installed — this takes a few minutes,
   that's normal).*

    !!! warning "Use R2023b, not the newest Webots"
        A newer Webots will give confusing errors with Erebus. Install **exactly R2023b**. The link
        above already points to the right one.

2. Open the downloaded **`webots-R2023b.dmg`** (a DMG is a Mac installer file — see the
   [glossary](glossary.md#setup-words)). A window opens showing the **Webots** icon and an
   **Applications** folder.
3. **Drag the Webots icon onto the Applications folder** in that window. Wait for it to finish
   copying.
4. Eject the installer: in Finder's sidebar, click the ⏏ next to "Webots".

!!! success "You should now have"
    Webots in your Applications. Open it once (from Launchpad or Applications) — it should open to a
    3D window. macOS may first ask *"Webots is an app downloaded from the Internet — are you sure?"*;
    click **Open**. You can close Webots again; the next step opens it properly.

---

## Step 3 — Download the Erebus files

Erebus is the competition package: the maze worlds, the sample robot, and the scoring referee.
It isn't an app you install — it's a folder of files you unzip.

1. Go to the latest release page: **<https://github.com/robocup-junior/erebus/releases/latest>**.
2. Under **"Assets"**, click **"Source code (zip)"** to download it.

    !!! warning "There is no file called 'Release Build' — use 'Source code (zip)'"
        Older guides say to download a "Release Build". Newer Erebus releases don't include one —
        **"Source code (zip)" is the real, complete package** (confirmed against the current v26.1).

3. Find the download in your **Downloads** folder. Safari usually unzips it for you into a folder
   like `erebus-26.1`. If you see a `.zip` instead, **double-click it** to unzip.
4. **Drag that `erebus-26.1` folder somewhere you'll find again**, such as your Documents folder.

!!! success "You should now have"
    A folder (for example `Documents/erebus-26.1`) containing folders named `game`,
    `player_controllers`, and others.

---

## Step 4 — Open it and check it works

1. In your Erebus folder, go into **`game/worlds`**.
2. **Double-click `world1.wbt`.** It opens in Webots and the Competition Supervisor panel appears
   on the left.
3. The first time only, Webots installs some Python libraries automatically. You'll see
   **"Initializing…"**.

    !!! note "\"Initializing…\" can take a few minutes — this is normal"
        It looks frozen but isn't; it's downloading `numpy`, `requests`, and `termcolor` in the
        background. Wait a couple of minutes. If it truly never finishes, or you see
        `WARNING: Python was not found`, see [When it goes wrong](troubleshooting.md#5-warning-python-was-not-found).

!!! success "You're installed! ✅"
    When the maze is on screen and the Competition Controller panel shows a time limit, macOS setup
    is done. Continue to **[Your first run](first-run.md)** to load the robot and watch it move.

---

## If it goes wrong

- **`WARNING: Python was not found`** in the Webots console → tell Webots where Python is:
  in Terminal run `which python3.10`, copy the path, then in Webots open
  **Webots → Settings/Preferences → Python command** and paste it in. Full steps:
  [When it goes wrong](troubleshooting.md#5-warning-python-was-not-found).
- **The robot won't run even though Python is installed** → you're likely hitting a Homebrew/Anaconda
  Python clash; point Webots at the python.org one as above.
- **"Initializing…" never ends, or `No module named 'cv2'`** → open Terminal and run
  `python3.10 -m pip install numpy termcolor requests opencv-python pillow overrides`, then reopen
  the world. (The simulator needs more than the three libraries the official docs list.)
- **Webots screen is blank/black** → open the world from the unzipped folder, not from inside the
  `.zip`.
- **The Supervisor panel didn't appear** → see [When it goes wrong](troubleshooting.md#4-the-competition-supervisor-panel-doesnt-appear).

Full, error-message-indexed help is on the **[When it goes wrong](troubleshooting.md)** page.
