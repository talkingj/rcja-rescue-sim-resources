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
- **Code: build a scoring robot:** a twelve-page series, sensors and driving through a complete
  scored run (detection, reporting, Lack of Progress, the exit message, the map matrix), every page
  verified with a real trial on a real robot, no invented numbers.
- **Rules & Scoring:** the official 2026 rules turned into readable, worked-example lessons, victims
  and hazmats, how points are earned, the map-matrix format, plus a self-check quiz and printable
  cheat sheets.
- **Strategy and competition readiness:** run budgeting, scoring maths for real decisions, a tour of
  the eight practice worlds, a debugging playbook, a competition-day checklist, and the rule
  violations that quietly cost the most.
- **Teacher and club material:** an 8-week club curriculum, two hands-on workshop labs, an assessment
  rubric, a mock-competition guide, and a printables pack.
- **When it goes wrong / the debugging playbook:** error-message-indexed troubleshooting for setup
  and for your own code.
- **Glossary** and **Erebus API cheat sheet:** every scary word explained, and every confirmed
  device, method, and message format in one reference page.

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
