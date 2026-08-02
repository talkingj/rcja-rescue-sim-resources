# Before you start

To eliminate ambiguity before installation begins, this page documents the required components
and their purpose. No installation is performed on this page; the actual installation steps are
provided on the platform-specific install page for your operating system.

## Components required for installation

Three components must be installed, in the following order. Each component depends on the one
preceding it, so sequence is important.

1. **Python** is the programming language in which the robot's control code is written and executed.
2. **Webots** is the simulation environment. It renders the maze and robot models and handles
   physics interactions, such as collisions with walls. Erebus requires a specific Webots version,
   not the most recent release, so verify the version requirement below before installing.
3. **Erebus** is the competition package itself, comprising the maze environments, a sample robot
   controller, and the scoring engine. It's distributed as a `.zip` archive (a compressed folder,
   defined in the [glossary](glossary.md#setup-words)), which you download and extract to a
   directory of your choice.

Unfamiliar terms are defined in the [glossary](glossary.md).

!!! success "The exact version that works"
    Erebus **v26.1** (the current release, 2026 competition rules) requires **Webots R2023b**
    specifically. Do not use any newer releases.

## Expected end state

Upon completion of the install pages, the following should be true:

- Python is installed and accessible from a terminal.
- Webots R2023b is installed and launches to an empty 3D window.
- The Erebus files are extracted to a known, locatable directory.

From this state, [Your first run](first-run.md) proceeds to a functioning robot navigating a
maze on screen.

## If it goes wrong

If an installation step fails, consult [When it goes wrong](troubleshooting.md) for the specific
error message rather than guessing at a fix, or re-verify the step against the instructions. The
most common failure cause is omitting the "add to PATH" option during Python installation; the
platform-specific install page addresses this explicitly.
