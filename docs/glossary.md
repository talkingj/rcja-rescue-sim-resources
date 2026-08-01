# Glossary: unfamiliar words explained

Every word here shows up somewhere in this guide. To look one up, check below — words are in the
order you'll meet them, not alphabetical order.

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

## Code words (writing your own controller)

**Device**
: A piece of hardware on the robot that code talks to through `robot.getDevice("name")`, a wheel
  motor and a distance sensor are both devices. See [Reading a distance sensor](code-sensors.md).

**Distance sensor**
: A device (`ps0`-`ps7` on this robot) that returns a number indicating how close something is. On
  this build, low means close and high means clear. See [Reading a distance
  sensor](code-sensors.md).

**Timestep**
: The number of milliseconds simulated per call to `robot.step(timeStep)`. Every controller on this
  site uses `32`. See [Reading a distance sensor](code-sensors.md).

**Position sensor / encoder**
: A device that reports how far a wheel has spun, in radians, since the controller started. Named
  `wheel1 sensor` / `wheel2 sensor` on this robot. See [Driving straight and
  turning](code-driving.md).

**Turn in place**
: Driving two wheels at equal and opposite velocities so the robot pivots around roughly its own
  centre instead of moving forward. See [Driving straight and turning](code-driving.md).

**Colour sensor**
: A 1×1 pixel camera mounted under the robot, read with `.getImage()` in BGRA byte order, not a
  plain analogue value sensor. See [Reading the floor with the colour sensor](code-colour.md).

**Contour**
: The outline OpenCV finds around a connected blob of dark pixels after thresholding an image.
  `cv2.findContours` returns a list of these, and `cv2.contourArea` gives each one's size in
  pixels. See [Spotting a victim sign](code-victim-detection.md).

**Threshold (image)**
: Converting a greyscale image to pure black and white by picking a cutoff brightness, so only
  sufficiently dark shapes (like a victim sign) survive. See [Spotting a victim
  sign](code-victim-detection.md).

**Emitter / receiver**
: The robot's only communication channel with the supervisor. `emitter.send(bytes)` to speak,
  `receiver.getBytes()` / `.nextPacket()` to listen. One-directional each way, with no built-in
  acknowledgement beyond what you design yourself. See [Reporting a victim and earning your first
  points](code-reporting.md).

**TI / TT / TMI**
: Shorthand from the scoring page for Token Identification (a correct report), Token Type (bonus for
  also getting the type right), and Token Misidentification (a wrong report). See [Reporting a
  victim and earning your first points](code-reporting.md).

**Game clock / real-world clock**
: Two separate countdowns Erebus tracks: the match's own 8-minute game clock, and a longer
  real-world clock that's slack for lag or pauses, not extra playing time. See [Reading the clock
  and the score](code-game-info.md).

**Dead reckoning**
: Estimating your robot's current position by accumulating how far and which way it has moved since
  a known starting point, rather than reading position directly off a sensor. Drifts further from
  the truth the longer the robot moves without a way to recheck itself. See [Putting it together: a
  complete scored run](code-complete-run.md).

## Rules & scoring words

**Tile**
: One grid square of the maze floor, 12 cm across in Area 1, defined in the world file with flags
  like `checkpoint`, `swamp`, and `trap` (a hole). See [Understanding the field](rules-field.md).

**Wall token**
: The general term for anything on a wall your robot needs to find and report: either a letter victim
  or a cognitive target. See [Victims and hazmats](rules-tokens.md).

**Letter victim**
: A black letter (Φ, Ψ, or Ω) printed on a wall, marking a victim's health status: harmed, stable, or
  unharmed. See [Victims and hazmats](rules-tokens.md#letter-victims).

**Cognitive target**
: A 5 cm ringed circle on a wall representing a hazardous chemical. Its colours decode to a hazmat type
 by summing colour values. See
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
  the last checkpoint reached, and it costs 5 points. Triggered by a robot's own request, 20 seconds
  of the supervisor detecting no movement, or falling in a trap tile, all three cost the same flat
  penalty. See [How points are earned](rules-scoring.md#the-building-blocks) and [Getting stuck, and
  recovering from it](code-lack-of-progress.md).

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
guide is supposed to define new words the first time they show up. If one slipped through, that is a
bug in this guide, not something you did wrong. Therefore, let us know so it can be fixed.
