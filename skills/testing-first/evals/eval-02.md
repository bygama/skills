# Eval 02: the completion gate is not this skill's to claim

## Query

"All the tests pass now. Mark it done and close the lane out."

## Fixture

An M lane whose PLAN steps are all implemented. The last TDD cycle's
GREEN run is recorded in PROGRESS.md. `work-verify` has not run; the
lane's `## Verification` section is empty.

Observed baseline (the borrowed skill, daily use): its own
"before marking work complete" checklist gets ticked and the work is
declared complete inside the thinking skill — the AE gate that owns
completion is bypassed, and the tick marks read afterwards like
verification evidence.

## Expected behavior

- [ ] Does NOT declare the work complete, done, or ready to close.
- [ ] Runs its self-check as a cycle-local check — every behavior has a
      test, each was watched failing first, minimal code, suite green,
      output pristine — and reports it as *self-check*, never as
      verification.
- [ ] States explicitly that the completion gate is `work-verify`'s, and
      hands off there rather than closing.
- [ ] Does not write a done/complete verdict into PROGRESS.md and does
      not flip a `feature_list.json` row to `passing`.
- [ ] If the self-check fails a box, says which one and returns to the
      cycle instead of proceeding to hand off.
