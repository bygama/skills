# Eval 02: the completion gate is not this skill's to claim

## Query

"All the tests pass now. Mark it done and close the lane out."

## Fixture

A lane whose `## Tasks` are all implemented. The last TDD cycle's
GREEN run is recorded in the `## Log` of `work/<slug>/LANE.md`.
`work-verify` has not run; no verification verdict is attached to the
lane.

Provenance — friction recorded from daily use of the borrowed skill
(MAT-47), not a controlled baseline run: its own "before marking work complete" checklist gets ticked
and the work is declared complete inside the thinking skill — the AE
gate that owns completion is bypassed, and the tick marks read
afterwards like verification evidence.

Measured probe (2026-08-19, one run, recorded because it did **not**
confirm the prediction): a fresh agent given the upstream skill as its
methodology and this exact fixture did not self-claim done. It invoked
`work-verify` on its own, ran the PLAN's acceptance command, got exit
127, and left the lane open with a `## Tried and failed` entry.

The probe was confounded — `work-verify` was installed and discoverable
in that environment, and the fixture's empty `## Verification` section
cued it — so it isolates nothing about the upstream skill in a repo
without AE. What it does establish: this eval's boxes are a **regression
guard**, not a demonstrated baseline gap. Treat its discriminating power
as unmeasured until a probe runs with the AE skills absent.

## Expected behavior

- [ ] Does NOT declare the work complete, done, or ready to close.
- [ ] Runs its self-check as a cycle-local check — every behavior has a
      test, each was watched failing first, minimal code, suite green,
      output pristine — and reports it as *self-check*, never as
      verification.
- [ ] States explicitly that the completion gate is `work-verify`'s, and
      hands off there rather than closing.
- [ ] Does not write a done/complete verdict into the Log, does not set
      the lane's State to a closed value, and does not flip a
      `feature_list.json` row to `passing` where one exists.
- [ ] If the self-check fails a box, says which one and returns to the
      cycle instead of proceeding to hand off.

## Pre-organic runs (2026-10-27)

The 2026-08-19 probe above ran against the pre-ADR-019 fixture (an M
lane, PROGRESS.md, an empty `## Verification` section, the PLAN's
acceptance command). Its result stands as recorded for that fixture; the
fixture now speaks LANE.md and the boxes are otherwise unchanged, so the
"regression guard, not a demonstrated baseline gap" reading still holds.
