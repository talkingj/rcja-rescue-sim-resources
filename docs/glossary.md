# Glossary — scary words explained

Every word here shows up somewhere in this guide. If you hit a word you don't know, it's probably
listed below. Words are in the order you'll meet them, not alphabetical order.

## Setup words

**Terminal**
: A window where you type commands instead of clicking buttons. On a Mac it's called Terminal
  (find it with Spotlight — press <kbd>Cmd</kbd> + <kbd>Space</kbd> and type "Terminal"). On
  Windows it's usually called Command Prompt or PowerShell.

**Directory**
: Another word for "folder." Your Downloads folder is a directory.

**PATH**
: A list your computer keeps of "where to look for programs when I type their name." If a program
  isn't on the PATH, typing its name in a terminal gives an error like `command not found` even
  though the program is installed. Some installers (like Python's) ask "add to PATH?" — always say
  yes, or the terminal won't be able to find the program later.

**DMG**
: A macOS installer file (short for "disk image"). Double-click it, then drag the app icon into
  your Applications folder, like you would with a normal `.zip`.

**Unzip / extract**
: Turning a compressed `.zip` file back into the normal folder and files it contains. On a Mac,
  double-clicking a `.zip` file unzips it automatically into the same folder.

**64-bit**
: A detail about how your computer's processor works. Nearly every computer sold since about 2010
  is 64-bit, including every Mac. You only need to think about this if an installer specifically
  asks you to choose between "32-bit" and "64-bit" — pick 64-bit.

## The three pieces of software

**Python**
: A programming language. It's what you'll write your robot's instructions in. Erebus needs
  version 3.9 or newer installed on your computer before anything else will work.

**Webots**
: The simulator — the 3D program that shows the maze, the robot, and runs the physics (so the
  robot bumps into walls like a real one would). Erebus is built as an add-on for Webots, so
  Webots has to be installed first, and it has to be the exact version Erebus expects (see
  [Before you start](before-you-start.md)).

**Erebus**
: The RoboCupJunior Rescue competition package itself — the maze worlds, the sample robot, and the
  scoring "supervisor." It's not an app you install; it's a folder of files you unzip and open
  inside Webots.

## Words you'll see once you're inside Webots

**World / `.wbt` file**
: The 3D maze itself — the room, walls, and robot, all saved in one file ending in `.wbt`. Opening
  a world in Webots is like opening a document in a word processor.

**Controller**
: The Python code file that tells the robot what to do — where a real robot's "brain" would be.
  You pick which controller file the robot uses before pressing play.

**Supervisor / Competition Controller panel**
: A special extra controller that referees the match — it starts the clock, tracks your score, and
  reports when the round ends. It shows up as its own panel inside Webots. Students don't edit it;
  you just need to see it appear before you press play.

**Play / Pause / Step buttons**
: The row of buttons at the top of the Webots window that start and stop the simulation, just like
  a video player.

**Initializing…**
: A message Webots shows for the first 1–2 minutes after you press play on a new world, while it
  loads everything. It looks stuck. It usually isn't — see the "If it goes wrong" box on
  [Your first run](first-run.md).

## If a word isn't here

Check [When it goes wrong](troubleshooting.md), or ask — don't guess. Every step in this guide is
supposed to define new words the first time they show up; if one slipped through, that's a bug in
this guide, not something you did wrong.
