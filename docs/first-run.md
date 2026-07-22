# Your first run

You've installed everything ([Windows](install-windows.md), [macOS](install-mac.md), or
[Linux](install-linux.md)). Now you'll load the sample robot brain and watch the robot drive around
the maze by itself. This takes about 5 minutes.

!!! note "What you'll see by the end"
    The e-puck robot appears in the maze and drives forward on its own, slowing and turning when it
    gets near a wall. That's the sample code thinking for it, and it's code you'll edit yourself on
    the next page.

---

## Step 1: Open the maze world

1. In your Erebus folder, open **`game/worlds`** and **double-click `world1.wbt`**. It opens in
   Webots.
2. On the **left** you'll see the **Competition Controller** panel. That's the referee, and it runs
   the clock and the score.

!!! success "You should now see"
    The maze in the middle of the screen and the Competition Controller panel on the left. If the
    left panel is missing, see [When it goes wrong](troubleshooting.md#4-the-competition-supervisor-panel-doesnt-appear).

---

## Step 2: Load the sample robot code

The robot needs a **controller**, the Python file that acts as its brain. Erebus comes with a sample
one.

1. In the Competition Controller panel, click the **LOAD** button (top band).

    ![The Competition Controller before loading, with the LOAD button](assets/official/getting-started/main_screen_unloaded.png)

    *Screenshot: RoboCupJunior Erebus documentation, Apache-2.0.*

2. A file picker opens. Go into the **`player_controllers`** folder and choose
   **`ExamplePlayerController_updated.py`**.

!!! success "You should now see"
    The **LOAD button turns orange**, which means the code is loaded and ready.

    ![The LOAD button turned orange after loading](assets/official/getting-started/main_screen_loaded.png)

    *Screenshot: RoboCupJunior Erebus documentation, Apache-2.0.*

---

## Step 3: Press play and watch it drive

The Competition Controller has three round buttons. The left one starts the match.

![The three run buttons in the Competition Controller](assets/official/getting-started/controller.png)

*Screenshot: RoboCupJunior Erebus documentation, Apache-2.0.*

1. Press the **start** (play) button.

    ![The start button](assets/official/getting-started/start_button.png)

    *Screenshot: RoboCupJunior Erebus documentation, Apache-2.0.*

2. The robot appears in the maze and **starts driving forward on its own**. When it gets close to a
   wall, the sample code slows one wheel so the robot turns away.

!!! note "First time only: a short wait"
    The very first time, Webots may show **"Initializing…"** while it sets up the Python libraries
    the simulator needs. Give it a minute or two. If it never finishes, or you see an error about a
    missing module, see [When it goes wrong](troubleshooting.md#3-its-stuck-on-initializing).

!!! success "You should now see"
    The robot driving around the start area by itself, turning away from walls. We ran this ourselves
    to be sure: with the sample controller loaded, the robot really does spawn and drive around,
    avoiding obstacles.

![The robot in the world1 maze during a real run](assets/real/world1-first-run.png)

*A real screenshot from our run. The robot is the round object in the upper area of the `world1`
maze.*

---

## The other buttons

- **Pause** freezes the match so you can look around. Press start again to continue.

    ![The pause button](assets/official/getting-started/pause_button.png)

- **Reset** (and the gear next to it for debug options) clears the run so you can start over.

    ![The reset and settings buttons](assets/official/getting-started/reset_buttons.png)

    *Screenshots: RoboCupJunior Erebus documentation, Apache-2.0.*

## Move the robot by hand

Click the robot in the maze and coloured arrows appear. **Drag an arrow** to slide the robot to a
new spot. (The robot only shows up once you've pressed start.)

![Dragging the arrows to move the robot](assets/official/getting-started/manual.png)

*Screenshot: RoboCupJunior Erebus documentation, Apache-2.0.*

---

## If it goes wrong

- **No left-side panel.** See [Supervisor didn't appear](troubleshooting.md#4-the-competition-supervisor-panel-doesnt-appear).
- **You pressed LOAD but the robot won't move.** The controller may not have loaded, or Python isn't
  set up. See [Can't load a controller](troubleshooting.md#6-cant-load-a-controller).
- **Stuck on "Initializing…"** See [It's stuck on Initializing](troubleshooting.md#3-its-stuck-on-initializing).

---

Next up: **[Make it move](make-it-move.md)**. You'll change one number in the robot's code and watch
it drive differently. It's your first real taste of programming the robot.
