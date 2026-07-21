# Before you start

This page tells you what you're about to install and why, so none of it is a surprise. It doesn't
install anything yet — the actual steps are on the install page for your computer.

## The three pieces you'll install

You're going to install three separate things, in this order. Each one depends on the one before
it, so the order matters.

1. **Python** — a programming language. This is what makes your robot's code run. Think of it as
   the language your robot understands.
2. **Webots** — the simulator (a 3D app — the app the robot lives in). This is the app that draws
   the maze, the robot, and makes the robot bump into walls like a real one. Erebus needs one
   *exact* version of Webots — not "whatever's newest" — see the box below.
3. **Erebus** — the competition files themselves: the maze worlds, the sample robot, and the
   scoring referee. This isn't an app you install with a wizard — it's a `.zip` file (a compressed
   folder — see [glossary](glossary.md#setup-words)) that you download and unzip into a folder you
   choose.

If any of those words are new, check the [glossary](glossary.md) — every one is explained there
too.

!!! success "The exact version that works, confirmed for real"
    Erebus **v26.1** (the current release, with the 2026 competition rules) needs
    **Webots R2023b** — not the newest Webots. This isn't a guess: we checked it against the
    official Erebus code (its README and its version-history file) on 2026-07-20, and neither has
    changed the required Webots version since 2024. Installing a newer Webots will likely cause
    confusing errors, so install *exactly* R2023b. The install pages below link straight to it.

## What you need before you begin

- [ ] **A computer** running Windows, macOS, or Linux (Ubuntu). This guide covers all three.
- [ ] **About 2 GB of free disk space** (Webots and Erebus together are roughly that size).
- [ ] **An internet connection** for the downloads — Webots alone is a large download (several
      hundred MB), so a fast connection helps but isn't required, just patience.
- [ ] **About 30 minutes**, uninterrupted if possible, for the first-time setup. After that,
      opening Erebus again takes seconds.
- [ ] **Nothing else.** You do not need to already know how to program, use a terminal, or know
      what any of the words above mean — that's what this guide is for.

## What "done" looks like

By the end of the install pages, you'll have:

- Python installed and working from a terminal.
- Webots R2023b installed and opening to an empty 3D window.
- The Erebus files unzipped into a folder you can find again.

Then [Your first run](first-run.md) takes you from that point to an actual robot driving around a
maze on your screen.

## If it goes wrong

If any install step fails, don't guess — check [When it goes wrong](troubleshooting.md) for the
exact error message, or re-read the step; the most common cause is skipping the "add to PATH"
checkbox during the Python install (explained on the install page for your OS).
