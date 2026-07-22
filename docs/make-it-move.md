# Make it move

You've watched the sample robot drive itself in [Your first run](first-run.md). Now you'll change
**one number** in its code and see the robot behave differently. This is your first real taste of
programming the robot, and it takes about 5 minutes.

!!! note "You don't need to know how to code yet"
    You're going to change a single number and save. That's it. Everything else in the file can stay
    exactly as it is.

---

## Step 1: Open the robot's code

The robot's brain is a plain text file. Open it in any code or text editor:

1. Go to your Erebus folder and into **`player_controllers`**.
2. Open **`ExamplePlayerController_updated.py`**. On Windows, right-click it and choose *Open with*,
   then *IDLE* or *Notepad*. On a Mac, right-click and choose *Open With*, then *TextEdit*. On Linux,
   open it in *gedit* or *nano*.

!!! success "You should now see"
    A page of Python code. Near the very top, on **line 4**, is the line you'll change:

    ```python
    max_velocity = 6.28      # Set a maximum velocity time constant
    ```

---

## Step 2: Understand the one number

`max_velocity` is how fast the robot's wheels are allowed to spin.

- The number is in **radians per second**, which is a way of measuring turning speed.
- **`6.28`** is about **2 × π**, which works out to one full wheel-turn every second. That's the
  robot's top speed in the sample code.

To make the robot slower, you'll cut that number in half.

---

## Step 3: Change it and save

1. Change the number from `6.28` to **`3.14`** (about half):

    ```python
    max_velocity = 3.14      # Set a maximum velocity time constant
    ```

2. **Save the file** (<kbd>Ctrl</kbd> or <kbd>Cmd</kbd> + <kbd>S</kbd>).

---

## Step 4: Reload and watch the difference

Webots is still running your *old* code until you load the new version.

1. Back in Webots, in the Competition Controller panel, press **reset**, then **LOAD** and pick
   `ExamplePlayerController_updated.py` again.
2. Press **start**.

!!! success "You should now see"
    The robot drives the same way, but **noticeably slower**. It covers roughly **half the ground**
    in the same amount of time. We measured this on a real run: at `6.28` the robot travelled about
    twice as far in ten seconds as it did at `3.14`. Cutting the number in half really does roughly
    halve the speed.

---

## Now make it your own

Try these one at a time. Change the number, save, then reset, LOAD, and start:

- Set `max_velocity = 1.0` and the robot crawls.
- Set `max_velocity = 6.28` again to go back to full speed.
- Try a number **bigger** than 6.28, like `10`. The sample maze is small, though, so a very fast
  robot mostly bumps around the walls.

You just changed how the robot behaves by editing its code. Everything a competition robot does,
from turning to sensing walls to finding victims, is built from more lines like this one.

---

## If it goes wrong

- **The robot drives exactly as before.** You didn't reload the new code. Press **reset**, then
  **LOAD** the file again, then **start**.
- **The robot won't move at all after editing.** You may have changed more than the number, like a
  typo somewhere in the code. Re-open the file and check that line 4 reads `max_velocity = 3.14`
  with nothing else on the line changed. If you're still stuck, download a fresh copy of the
  controller.
- **An error appears in the console.** See [When it goes wrong](troubleshooting.md).

---

Next: [What's next: sensors](next-steps.md) is a short teaser on reading the robot's distance
sensors, the next natural thing to try. The official
[Erebus tutorials](https://erebus.rcj.cloud/docs/tutorials/) go further still, covering the
cameras and mapping the maze.
