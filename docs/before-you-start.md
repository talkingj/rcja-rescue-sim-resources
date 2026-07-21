# Before you start

This page tells you what you're about to install and why, so none of it is a surprise. It doesn't
install anything yet. The actual steps are on the install page for your computer.

## The three pieces you'll install

You're going to install three separate things, in this order. Each one needs the one before it, so
the order matters.

1. **Python** is a programming language. It's what makes your robot's code run. Think of it as the
   language your robot understands.
2. **Webots** is the simulator, a 3D app that the robot lives inside. It draws the maze and the
   robot, and it makes the robot bump into walls like a real one would. Erebus needs one *exact*
   version of Webots, not "whatever's newest," so check the box below.
3. **Erebus** is the competition itself: the maze worlds, the sample robot, and the scoring referee.
   This isn't an app you install with a wizard. It's a `.zip` file (a compressed folder, explained
   in the [glossary](glossary.md#setup-words)) that you download and unzip into a folder you choose.

If any of those words are new, the [glossary](glossary.md) explains every one of them.

!!! success "The exact version that works"
    Erebus **v26.1** (the current release, with the 2026 competition rules) needs **Webots R2023b**,
    not the newest Webots. This isn't a guess. We checked it against the official Erebus code, and it
    hasn't changed the required Webots version since 2024. A newer Webots will likely cause confusing
    errors, so install *exactly* R2023b. The install pages below link straight to it.

## What you need before you begin

- [ ] **A computer** running Windows, macOS, or Linux (Ubuntu). This guide covers all three.
- [ ] **About 2 GB of free disk space** (Webots and Erebus together are roughly that size).
- [ ] **An internet connection** for the downloads. Webots alone is a large download (several
      hundred MB), so a fast connection helps, but patience works too.
- [ ] **About 30 minutes**, uninterrupted if possible, for the first-time setup. After that,
      opening Erebus again takes seconds.
- [ ] **Nothing else.** You don't need to already know how to program, use a terminal, or know what
      any of the words above mean. That's what this guide is for.

## What "done" looks like

By the end of the install pages, you'll have:

- Python installed and working from a terminal.
- Webots R2023b installed and opening to an empty 3D window.
- The Erebus files unzipped into a folder you can find again.

Then [Your first run](first-run.md) takes you from there to an actual robot driving around a maze on
your screen.

## If it goes wrong

If an install step fails, don't guess. Check [When it goes wrong](troubleshooting.md) for the exact
error message, or re-read the step. The most common cause is skipping the "add to PATH" checkbox
during the Python install, which the install page for your computer walks you through.
