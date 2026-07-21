# Erebus Rescue Sim: Beginner Guide

A friendly, screenshot-driven guide that takes a student from *"I have a laptop"* to *"my robot is
driving around inside the Erebus rescue simulator"*, on Windows, macOS, or Linux.

It fills the gaps that trip up beginners in the official docs: what "PATH" means, exactly which
Webots version to install, which file to actually download, and a "you should now see…" check after
every step.

## Read the guide

**<https://talkingj.github.io/rcja-rescue-sim-resources/>**

## What's inside

- **Before you start:** the three pieces (Python, Webots, Erebus) in plain English.
- **Install:** step-by-step for Windows, macOS, and Linux, with screenshots.
- **Your first run:** open the maze, load the sample robot, and watch it drive.
- **Make it move:** change one line of code and see the robot behave differently.
- **When it goes wrong:** an error-message-indexed troubleshooting hub.
- **Glossary:** every scary word explained.

## Build it locally

```bash
python3 -m venv .venv
.venv/bin/pip install mkdocs-material
.venv/bin/mkdocs serve        # live preview at http://127.0.0.1:8000/
.venv/bin/mkdocs build        # static site into ./site
```

## Credits

Built with [MkDocs](https://www.mkdocs.org/) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).
Some interface screenshots are reused from the official
[RoboCupJunior Erebus documentation](https://github.com/robocup-junior/erebus-document) under the
Apache-2.0 licence. See `docs/assets/official/NOTICE.md` for attribution.
