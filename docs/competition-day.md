# Pre-run and competition-day checklist

To catch the failures that most often sink an otherwise solid run, walk through this checklist
before your first real match. Every item here traces to either the official rules or a real failure
this site actually hit while building [Track C](code-sensors.md), meaning none of it is generic
advice.

!!! note "Where this fits"
    Fifth of six Strategy pages. Read [the debugging playbook](debugging-playbook.md) first, several
    items below are the same findings turned into a pre-run check instead of an in-the-moment fix.

---

## The checklist

- [ ] **Debug mode is off.** Confirmed straight from the supervisor's own source
      (`MainSupervisor.py`): if debug mode is left on when a match starts, it logs "WARNING: Debug
      mode is on. This should not be on during competitions" into the match history, visible to
      judges. Check this before every real match.
- [ ] **Every device name in your controller matches the robot's proto file exactly.** [The single
      most common silent failure this site hit](debugging-playbook.md#1-a-device-call-fails-with-no-visible-error-at-all):
      a misspelled name produces no visible error at all, just a controller that quietly does
      nothing.
- [ ] **Your identification code waits meaningfully longer than the bare 1-second stillness
      minimum before reporting.** [Confirmed necessary, more than once, while building this
      site](code-exit.md#getting-this-run-clean-took-more-patience-than-it-looked-like-it-should):
      reporting too soon after stopping can silently fail to register at all.
- [ ] **You know both clocks, and which one you're actually racing.** [Confirmed on the run-budget
      page](strategy-run-budget.md#two-clocks): an 8-minute game clock, and a separate,
      longer real-world clock that's slack, not extra playing time.
- [ ] **If you're submitting a map, you send it with real time to spare.** The map bonus is only banked once `'M'` has actually been received, [confirmed on
      the mapping page](code-mapping.md). Additionally, sending it and then immediately running out
      of time before the supervisor processes it is a real, avoidable risk.
- [ ] **You know exactly what your exit message needs to be true to pay off.** [Two conditions,
      confirmed on the exit page](code-exit.md#step-1-one-byte-two-conditions): on the start tile,
      and at least one prior identification. Miss either and the message still ends the match — it
      just doesn't pay the 10% bonus. Therefore, double-check both conditions in testing and
      in the rules.
- [ ] **You've tested what your controller does immediately after an LoP relocate.** Position and
      orientation both change abruptly [confirmed on the LoP page](code-lack-of-progress.md).
      Therefore, any code that assumes smooth, continuous movement should be checked against a
      sudden jump.
- [ ] **You've run your actual competition controller against more than one practice world.** [The
      eight practice worlds page](practice-worlds.md) found real, structural differences between
      them — trap counts, area spread, token density. A controller tuned against only one world may
      behave differently on the one you're actually given.

---

## The morning-of version, in one sentence each

1. Debug mode off.
2. Device names checked.
3. Report timing has margin.
4. You know which clock you're racing.
5. Map submitted early if you're submitting one at all.
6. Exit conditions understood.
7. LoP recovery tested.
8. Tested on more than one world.

---

## If it goes wrong on the day

To troubleshoot something that worked in practice but fails in the real round, start with [the
debugging playbook](debugging-playbook.md). Most of what's there is exactly this kind of "worked
before, mysteriously doesn't now" symptom.
- **You're unsure whether to keep exploring or exit with what you have.** [Scoring maths for
  strategy decisions](strategy-scoring-maths.md) works through that exact trade-off with real
  numbers. Review this the morning of, rather than figuring it out live.

---

Next: [the rule violations that quietly cost the most](costly-mistakes.md).
