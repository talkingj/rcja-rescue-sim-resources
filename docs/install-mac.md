# Install on macOS

This page sets everything up on a Mac. Do the three steps **in order**, since each one needs the one
before it. Give yourself about 30 minutes the first time.

!!! tip "Before you begin"
    If you haven't read [Before you start](before-you-start.md), do that first. It explains what
    these three pieces (Python, Webots, Erebus) are. Every unfamiliar word is in the
    [Glossary](glossary.md).

!!! note "Apple Silicon (M1/M2/M3) or Intel: same steps either way"
    Both use the exact same downloads below. The Webots R2023b installer is *universal* (one file
    works on both), and so is the Python installer. You don't need to know which chip you have.

---

## Step 1: Install Python

Python is the language your robot's instructions are written in.

1. Go to **<https://www.python.org/downloads/macos/>**.
2. Download **Python 3.10.11**, the "macOS 64-bit universal2 installer." *(It's the last 3.10 with a
   ready-made Mac installer, and that's the version this guide uses.)*
3. Open the downloaded file (`python-3.10.11-macos11.pkg`) and click through the installer:
   **Continue, Continue, Agree, Install**. Enter your Mac password if it asks. Then click **Close**.

!!! warning "Install this even if you think you already have Python"
    Macs come with a Python, and if you've used Homebrew or Anaconda you have more. **Install this
    fresh python.org version anyway.** Mixing the others with Webots is the most common reason the
    robot won't run on a Mac even though everything looks installed. A fresh python.org 3.10 avoids
    the whole mess.

!!! success "You should now have"
    Python installed. To check, open **Terminal** (press <kbd>Cmd</kbd> + <kbd>Space</kbd>, type
    *Terminal*, and press <kbd>Enter</kbd>). Type `python3.10 --version` and press <kbd>Enter</kbd>.
    You should see `Python 3.10.11`.

---

## Step 2: Install Webots

Webots is the simulator, the 3D app where the maze and robot live.

1. Download the **exact** version Erebus needs, **Webots R2023b**, straight from here:
   **[webots-R2023b.dmg](https://github.com/cyberbotics/webots/releases/download/R2023b/webots-R2023b.dmg)**.
   It's about 100 MB to download and expands to roughly 1 GB once installed, so this takes a few
   minutes. That's normal.

    !!! warning "Use R2023b, not the newest Webots"
        A newer Webots will give confusing errors with Erebus. Install **exactly R2023b**. The link
        above already points to the right one.

2. Open the downloaded **`webots-R2023b.dmg`**. (A DMG is a Mac installer file, explained in the
   [glossary](glossary.md#setup-words).) A window opens showing the **Webots** icon and an
   **Applications** folder.
3. **Drag the Webots icon onto the Applications folder** in that window. Wait for it to finish
   copying.
4. Eject the installer: in Finder's sidebar, click the ⏏ next to "Webots."

!!! success "You should now have"
    Webots in your Applications. Open it once (from Launchpad or Applications) and it should open to
    a 3D window. The first time, macOS may ask whether you're sure about an app downloaded from the
    internet. Click **Open**. You can close Webots again. The next step opens it properly.

---

## Step 3: Download the Erebus files

Erebus is the competition package: the maze worlds, the sample robot, and the scoring referee. It
isn't an app you install. It's a folder of files you unzip.

1. Go to the latest release page: **<https://github.com/robocup-junior/erebus/releases/latest>**.
2. Under **"Assets"**, click **"Source code (zip)"** to download it.

    !!! warning "There's no file called 'Release Build.' Use 'Source code (zip)'"
        Older guides say to download a "Release Build." Newer Erebus releases don't include one, so
        **"Source code (zip)" is the real, complete package** (confirmed against the current v26.1).

3. Find the download in your **Downloads** folder. Safari usually unzips it for you into a folder
   like `erebus-26.1`. If you see a `.zip` instead, **double-click it** to unzip.
4. **Drag that `erebus-26.1` folder somewhere you'll find again**, such as your Documents folder.

!!! success "You should now have"
    A folder (for example `Documents/erebus-26.1`) containing folders named `game`,
    `player_controllers`, and others.

---

## Step 4: Open it and check it works

1. In your Erebus folder, go into **`game/worlds`**.
2. **Double-click `world1.wbt`.** It opens in Webots, and the Competition Supervisor panel appears
   on the left.
3. The first time only, Webots installs some Python libraries automatically. You'll see
   **"Initializing…"**.

    !!! note "\"Initializing…\" can take a few minutes"
        It's installing Python libraries in the background, not stuck. If it never finishes, or you
        see `WARNING: Python was not found`, head to
        [When it goes wrong](troubleshooting.md#5-warning-python-was-not-found).

!!! success "You should now have"
    When the maze is on screen and the Competition Controller panel shows a time limit, macOS setup
    is done. Continue to **[Your first run](first-run.md)** to load the robot and watch it move.

---

## If it goes wrong

- **`WARNING: Python was not found` in the Webots console.** Tell Webots where Python is: in
  Terminal run `which python3.10`, copy the path, then in Webots open **Webots → Settings** (or
  **Preferences**) **→ Python command** and paste it in. Full steps:
  [When it goes wrong](troubleshooting.md#5-warning-python-was-not-found).
- **The robot won't run even though Python is installed.** You're probably hitting a Homebrew or
  Anaconda Python clash. Point Webots at the python.org one, as above.
- **"Initializing…" never ends, or you see `No module named 'cv2'`.** Open Terminal and run
  `python3.10 -m pip install numpy termcolor requests opencv-python pillow overrides`, then reopen
  the world. (The simulator needs more than the three libraries the official docs list.)
- **The Webots screen is blank or black.** Open the world from the unzipped folder, not from inside
  the `.zip`.
- **The Supervisor panel didn't appear.** See [When it goes wrong](troubleshooting.md#4-the-competition-supervisor-panel-doesnt-appear).

Full, searchable help is on the **[When it goes wrong](troubleshooting.md)** page.
