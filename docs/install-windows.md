# Install on Windows

This page sets up everything on a Windows 10 or 11 computer. Do the three steps **in order** —
each one needs the one before it. Give yourself about 30 minutes the first time.

!!! tip "Before you begin"
    Read [Before you start](before-you-start.md) first if you haven't — it explains what these
    three pieces (Python, Webots, Erebus) are. Every unfamiliar word is in the
    [Glossary](glossary.md).

---

## Step 1 — Install Python

Python is the language your robot's instructions are written in.

1. Go to the official Python download page: **<https://www.python.org/downloads/windows/>**.
2. Download **Python 3.10.11** (the last 3.10 version with a Windows installer). Click the
   **"Windows installer (64-bit)"** link. *(Python 3.9.x also works, but this guide uses 3.10.)*
3. Open the file you just downloaded (it's called something like `python-3.10.11-amd64.exe`).
4. On the very first screen of the installer, **tick the box at the bottom that says
   "Add python.exe to PATH"**. This is the most important click on this whole page — if you miss
   it, Webots won't be able to find Python later.

    ![The Python installer with the "Add python.exe to PATH" box ticked at the bottom](assets/official/windows/python-path.png)

    *Tick **Add python.exe to PATH** before clicking Install. Screenshot: RoboCupJunior Erebus
    documentation, Apache-2.0.*

5. Click **"Install Now"** and wait for it to finish. Click **Close** at the end.

!!! info "What is PATH?"
    PATH is the list your computer checks to find a program when something asks for it by name.
    Ticking that box adds Python to the list, so Webots can start Python for you. More in the
    [glossary](glossary.md#setup-words).

!!! success "You should now have"
    Python installed. To check: press <kbd>Win</kbd>, type **cmd**, open **Command Prompt**, type
    `python --version` and press <kbd>Enter</kbd>. You should see `Python 3.10.11`. If you instead
    see an error or the Microsoft Store opens, PATH wasn't set — see
    [When it goes wrong](troubleshooting.md).

---

## Step 2 — Install Webots

Webots is the simulator — the 3D app where the maze and robot live.

1. Download the **exact** version Erebus needs, **Webots R2023b**, straight from here:
   **[webots-R2023b_setup.exe](https://github.com/cyberbotics/webots/releases/download/R2023b/webots-R2023b_setup.exe)**
   *(about 1 GB — this download takes a while, that's normal).*

    !!! warning "Use R2023b, not the newest Webots"
        A newer Webots will give confusing errors with Erebus. Install **exactly R2023b**. The link
        above already points to the right one.

2. Open the downloaded file and click through the installer, leaving every option at its default.
3. Wait for it to finish, then click **Finish**.

!!! success "You should now have"
    Webots installed. Open it once from the Start menu — it should open to a 3D window. You can
    close it again; the next step opens it properly.

---

## Step 3 — Download the Erebus files

Erebus is the competition package: the maze worlds, the sample robot, and the scoring referee.
It isn't an installer — it's a folder of files you unzip.

1. Go to the latest release page: **<https://github.com/robocup-junior/erebus/releases/latest>**.
2. Under **"Assets"**, click **"Source code (zip)"** to download it.

    !!! warning "There is no file called 'Release Build' — use 'Source code (zip)'"
        Older guides say to download a "Release Build". Newer Erebus releases don't include one —
        **"Source code (zip)" is the real, complete package.** (We confirmed this against the
        current v26.1 release.)

    ![The Erebus release download](assets/official/windows/download_erebus.png)

    *Screenshot: RoboCupJunior Erebus documentation, Apache-2.0.*

3. Find the downloaded `.zip` in your Downloads folder. **Right-click it → "Extract All…" →
   Extract.** Pick somewhere easy to find, like `Documents\Erebus`.

    !!! warning "Actually extract it — don't just open the zip"
        If you double-click a `.zip`, Windows only *previews* it. Running Erebus from inside that
        preview causes a **blank/black Webots screen**. Use **Extract All** so you get a real
        folder first.

!!! success "You should now have"
    A folder (for example `Documents\Erebus\erebus-26.1`) containing folders named `game`,
    `player_controllers`, and others.

---

## Step 4 — Open it and check it works

1. In your extracted Erebus folder, go into **`game\worlds`**.
2. **Double-click `world1.wbt`.** It opens in Webots and the Competition Supervisor panel appears
   on the left.
3. The first time only, Webots installs some Python libraries automatically. You'll see
   **"Initializing…"**.

    !!! note "\"Initializing…\" can take a few minutes — this is normal"
        It looks frozen but it isn't; it's downloading `numpy`, `requests`, and `termcolor` in the
        background. Wait a couple of minutes. If it truly never finishes, see
        [When it goes wrong](troubleshooting.md).

!!! success "You're installed! ✅"
    When the maze is on screen and the Competition Controller panel shows a time limit, Windows
    setup is done. Continue to **[Your first run](first-run.md)** to load the robot and watch it
    move.

---

## If it goes wrong

- **Typed `python` and nothing happened, or the Store opened** → PATH wasn't set in Step 1.
- **Webots screen is blank/black** → you opened Erebus from inside the zip; Extract All first.
- **"Initializing…" never ends**, or `No module named 'cv2'` → open Command Prompt and run
  `python -m pip install numpy termcolor requests opencv-python pillow overrides`, then reopen the
  world. (The simulator needs more than the three libraries the official docs list.)
- **The Supervisor panel didn't appear** → the fix is in [When it goes wrong](troubleshooting.md).

Full, error-message-indexed help is on the **[When it goes wrong](troubleshooting.md)** page.
