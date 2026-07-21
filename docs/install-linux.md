# Install on Linux (Ubuntu)

This page sets everything up on **Ubuntu 20.04 or 22.04**. Most steps are typed into a
**terminal** (the window where you type commands instead of clicking — see the
[glossary](glossary.md#setup-words)). Copy each command exactly. Give yourself about 30 minutes.

!!! tip "Before you begin"
    Read [Before you start](before-you-start.md) if you haven't — it explains the three pieces
    (Python, Webots, Erebus). On Ubuntu, Python 3 is already installed, so you only add a couple of
    pieces to it.

!!! note "Other Linux distributions"
    These exact commands are for **Ubuntu** (and Ubuntu-based systems like Linux Mint). On Fedora,
    Arch, etc. the idea is the same but the package commands differ — install Webots R2023b from the
    [Webots releases page](https://github.com/cyberbotics/webots/releases/tag/R2023b) and use your
    own package manager for `pip`.

---

## Step 1 — Set up Python's libraries

Open a terminal (<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>T</kbd>) and run these one at a time.

1. Install `pip`, the tool that adds libraries to Python:

    ```bash
    sudo apt update
    sudo apt install python3-pip
    ```

2. Install the libraries Erebus needs:

    ```bash
    python3 -m pip install numpy termcolor requests opencv-python pillow overrides
    ```

    !!! note "Why more than three?"
        The official docs list only `numpy termcolor requests`, but the Competition Supervisor also
        needs `opencv-python`, `pillow`, and `overrides` — and `opencv-python` isn't installed
        automatically. Installing the full list now avoids a `No module named 'cv2'` crash later.
        (Verified on a real run, 2026-07-21.)

!!! success "You should now have"
    `pip` and the three libraries installed. Check Python itself is present with
    `python3 --version` — Ubuntu 20.04/22.04 ships 3.8–3.10, all of which work.

---

## Step 2 — Install Webots R2023b

Erebus needs the **exact** version R2023b, not the newest Webots.

1. Download the R2023b package (about 1 GB — this takes a while, that's normal):

    ```bash
    wget https://github.com/cyberbotics/webots/releases/download/R2023b/webots_2023b_amd64.deb
    ```

2. Install it (the `./` in front matters — it tells `apt` to install the file you just downloaded):

    ```bash
    sudo apt install ./webots_2023b_amd64.deb
    ```

    !!! warning "Install exactly R2023b"
        A newer Webots will give confusing errors with Erebus. The command above installs the
        correct version.

!!! success "You should now have"
    Webots installed. Test it with `webots` in the terminal — it should open to a 3D window. Close
    it again; the next steps open it properly.

---

## Step 3 — Download the Erebus files

Erebus is the competition package (maze worlds, sample robot, scoring referee) — a folder of files
you unzip, not an installer.

1. Go to the latest release page: **<https://github.com/robocup-junior/erebus/releases/latest>**.
2. Under **"Assets"**, download **"Source code (zip)"**.

    !!! warning "There is no 'Release Build' file — use 'Source code (zip)'"
        Older guides mention a "Release Build". Newer Erebus releases don't include one —
        **"Source code (zip)" is the real, complete package** (confirmed against the current v26.1).

3. Unzip it into a folder you'll remember, for example your home folder:

    ```bash
    unzip ~/Downloads/erebus-*.zip -d ~/Erebus
    ```

!!! success "You should now have"
    A folder such as `~/Erebus/erebus-26.1` containing `game`, `player_controllers`, and others.

---

## Step 4 — Open it and check it works

1. Launch the world file with Webots. Replace the path with wherever you unzipped it:

    ```bash
    webots ~/Erebus/erebus-26.1/game/worlds/world1.wbt
    ```

2. Webots opens and the Competition Supervisor panel appears on the left.
3. The first time only, Webots installs Python libraries automatically — you'll see
   **"Initializing…"**.

    !!! note "\"Initializing…\" can take a few minutes — this is normal"
        It looks frozen but isn't; it's setting up in the background. Wait a couple of minutes. If
        it truly never finishes, you already installed the libraries in Step 1, so check the
        console for other errors — see [When it goes wrong](troubleshooting.md).

!!! success "You're installed! ✅"
    When the maze is on screen and the Competition Controller panel shows a time limit, Linux setup
    is done. Continue to **[Your first run](first-run.md)** to load the robot and watch it move.

---

## If it goes wrong

- **`webots: command not found`** → the `.deb` install didn't finish; re-run Step 2.
- **Webots screen is blank/black** → unzip Erebus to its own folder (Step 3) and open the world
  from there, not from inside the archive viewer.
- **"Initializing…" never ends** → you already ran Step 1, so open the console and read the actual
  error; details on [When it goes wrong](troubleshooting.md).
- **The Supervisor panel didn't appear** → the fix is on [When it goes wrong](troubleshooting.md).

Full, error-message-indexed help is on the **[When it goes wrong](troubleshooting.md)** page.
