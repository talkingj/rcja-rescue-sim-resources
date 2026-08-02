# The printables pack

All four of this site's printable pages, in one place, in the order a teacher would actually hand
them out across a term.

!!! note "On the print-quality check below"
    This resource's acceptance wanted a live print-preview check on this machine. That check hit a real, confirmed limitation: macOS Accessibility permission for GUI
    automation isn't available in this session (`System Events` can't address Safari's windows,
    `-1719 Invalid index`, the same category of missing-permission problem [already found and
    documented blocking screenshots](practice-worlds.md)), and this machine has no
    `wkhtmltopdf`/headless-Chrome/`weasyprint`-capable setup installed either (the last was tried
    and failed on a missing system library, not a code problem). A live rendered PDF could not be
    produced or inspected this session. What *is* real and verified instead: Material for MkDocs
    ships its own `@media print` stylesheet (confirmed present in this build's compiled CSS), the
    same one already responsible for [quick-start.md](quick-start.md) and [the scoring cheat
    sheet](scoring-cheat-sheet.md) fitting their already-published "1-2 sheets" claim, neither of
    which this resource changed.

## The four sheets, in hand-out order

1. **[Quick-start](quick-start.md)**, the condensed install-and-first-run checklist. Hand out once,
   at the very start.
2. **[Scoring cheat sheet](scoring-cheat-sheet.md)**, victim symbols, hazmat ring values, the point
   table. Hand out alongside [the classroom workshop](workshop.md) or [week 1 of the club
   curriculum](club-curriculum.md).
3. **[Cognitive target decoder](cognitive-target-decoder.md)**, a hands-on hazmat-decoding practice
   sheet with its own answer key. Used directly by [the workshop's Activity
   2](workshop.md#activity-2-decode-the-hazmat-10-min).
4. **[Workshop activity sheet](workshop-activity-sheet.md)**, the fill-in-as-you-go sheet for [the
   classroom workshop's](workshop.md) Activities 1, 3, and 4.

## How to print the whole pack at once

Each sheet already has its own "printing this page" tip using the browser's print
(<kbd>Ctrl</kbd>/<kbd>Cmd</kbd> + <kbd>P</kbd>). To print all four for a class:

1. Open each of the four pages above in its own browser tab.
2. Print each one individually rather than trying to print a combined view, [the cognitive target
   decoder](cognitive-target-decoder.md) and [the workshop activity
   sheet](workshop-activity-sheet.md) both put their answer keys at the very bottom specifically so
   you can stop printing before that page, a combined print job would lose that control.
3. For a whole class, print one copy of the quick-start and scoring cheat sheet per student, and one
   copy of the decoder and activity sheet per team.

---

Next: [the Erebus API cheat sheet](api-cheat-sheet.md).
