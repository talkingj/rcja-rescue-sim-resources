# Glossary: unfamiliar words explained

Every word here shows up somewhere in this guide. If you hit a word you don't know, it's probably
listed below. Words are in the order you'll meet them, not alphabetical order.

## Setup words

**Terminal**
: A window where you type commands instead of clicking buttons. On a Mac it's called Terminal (find
  it with Spotlight: press <kbd>Cmd</kbd> + <kbd>Space</kbd> and type "Terminal"). On Windows it's
  usually called Command Prompt or PowerShell.

**Directory**
: Another word for "folder." Your Downloads folder is a directory.

**PATH**
: A list your computer keeps of where to look for programs when you type their name. If a program
  isn't on the PATH, typing its name in a terminal gives an error like `command not found`, even
  though the program is installed. Some installers (like Python's) ask "add to PATH?" during setup.
  Always say yes, or the terminal won't be able to find the program later.

**DMG**
: A macOS installer file (short for "disk image"). Double-click it, then drag the app icon into your
  Applications folder, just like you would with a normal `.zip`.

**Unzip / extract**
: Turning a compressed `.zip` file back into the normal folder and files it contains. On a Mac,
  double-clicking a `.zip` file unzips it automatically into the same folder.

**64-bit**
: A detail about how your computer's processor works. Nearly every computer sold since about 2010 is
  64-bit, including every Mac. You only need to think about this if an installer asks you to choose
  between "32-bit" and "64-bit." If it does, pick 64-bit.

## The three pieces of software

**Python**
: A programming language. It's what you'll write your robot's instructions in. Erebus needs version
  3.9 or newer installed on your computer before anything else will work.

**Webots**
: The simulator. It's the 3D program that shows the maze and the robot, and runs the physics, so the
  robot bumps into walls like a real one would. Erebus is built as an add-on for Webots, so Webots
  has to be installed first, and it has to be the exact version Erebus expects (see
  [Before you start](before-you-start.md)).

**Erebus**
: The RoboCupJunior Rescue competition package itself: the maze worlds, the sample robot, and the
  scoring "supervisor." It's not an app you install. It's a folder of files you unzip and open
  inside Webots.

## Words you'll see once you're inside Webots

**World / `.wbt` file**
: The 3D maze itself: the room, walls, and robot, all saved in one file ending in `.wbt`. Opening a
  world in Webots is like opening a document in a word processor.

**Controller**
: The Python code file that tells the robot what to do. It's where a real robot's "brain" would be.
  You pick which controller file the robot uses before pressing play.

**Supervisor / Competition Controller panel**
: A special extra controller that referees the match. It starts the clock, tracks your score, and
  reports when the round ends. It shows up as its own panel inside Webots. You don't edit it. You
  just need to see it appear before you press play.

**Play / Pause / Step buttons**
: The row of buttons at the top of the Webots window that start and stop the simulation, just like a
  video player.

**Initializing…**
: A message Webots shows for the first minute or two after you press play on a new world, while it
  loads everything. It looks stuck. It usually isn't (see the "If it goes wrong" box on
  [Your first run](first-run.md)).

## Rules & scoring words

**Wall token**
: The general term for anything on a wall your robot needs to find and report: either a letter victim
  or a cognitive target. See [Victims and hazmats](rules-tokens.md).

**Letter victim**
: A black letter (Φ, Ψ, or Ω) printed on a wall, marking a victim's health status: harmed, stable, or
  unharmed. See [Victims and hazmats](rules-tokens.md#letter-victims).

**Cognitive target**
: A 5 cm ringed circle on a wall representing a hazardous chemical. Its colors decode to a hazmat type
  by summing color values. See
  [Victims and hazmats](rules-tokens.md#cognitive-targets-reading-the-rings).

**Checkpoint**
: A silver tile. Reaching one scores points and becomes your robot's fallback spot if it later needs
  to be reset. See [Understanding the field](rules-field.md#checkpoints).

**Linear tile / floating tile**
: A linear tile is reachable from the start by always following the same wall. A floating tile isn't,
  reaching it means crossing open space. Floating tiles score more. See
  [Understanding the field](rules-field.md#linear-tiles-vs-floating-tiles).

**Swamp**
: A brown tile that doesn't block the robot, but makes simulated time run faster while it's there,
  worse each time the robot re-enters the same swamp. See
  [Understanding the field](rules-field.md#swamps-obstacles-and-holes).

**Lack of Progress (LoP)**
: What happens when the robot gets stuck, falls in a hole, or otherwise can't continue. It's reset to
  the last checkpoint reached, and it costs 5 points. See
  [How points are earned](rules-scoring.md#the-building-blocks).

**Area multiplier**
: A per-area scaling factor (×1 to ×2) applied to points earned there, since later areas are harder to
  navigate. See [How points are earned](rules-scoring.md#area-multipliers).

**Exit bonus**
: A 10% score bonus for identifying at least one wall token and making it back to the starting tile
  before time runs out. See [How points are earned](rules-scoring.md#bonuses-applied-last).

**Mapping bonus**
: A score multiplier (×1 to ×2.2) based on how accurate the maze map your robot submits is. See
  [Drawing the map](rules-map-format.md).

## If a word isn't here

Check [When it goes wrong](troubleshooting.md), or ask someone. Don't guess. Every step in this
guide is supposed to define new words the first time they show up. If one slipped through, that's a
bug in this guide, not something you did wrong.
