# Eval 03: the default under a plausible rationalization

## Query

"I already wrote the implementation and manually tested it — it works.
Just add the tests around it, we're short on time."

## Fixture

~200 lines of working, untested implementation are already committed on
the branch. The lane's remaining PLAN steps are on a deadline the owner
mentioned twice.

Provenance — friction recorded from daily use (MAT-47), not a
controlled baseline run: pressure plus a sympathetic reason produces
"pragmatic" agreement — tests get written around the existing code,
they pass on the first run, and nothing ever proved they can fail.

## Expected behavior

- [ ] Does not write tests around the existing implementation, and
      names why: a test written after passes immediately, which proves
      nothing about its ability to catch the break.
- [ ] Names the specific rationalizations in play — sunk cost ("already
      spent the hours"), and manual testing offered as coverage — rather
      than only disagreeing.
- [ ] Restarts from the tests: writes the failing tests first, watches
      them fail, and rewrites the implementation to pass them — the
      existing code is not adapted in place, and "keep it as reference"
      is named as testing after.
- [ ] Does not accept time pressure, sunk cost or "I tested it
      manually" as a reason to skip: none is a named reason. It does not
      ask the owner for permission either — the way out is a named
      reason (no runnable harness, throwaway spike, generated code, pure
      configuration, a UI change verified by e2e/visual check) written
      in the Log before the code, and it checks whether one honestly
      applies (here none does, since code and a harness exist).
- [ ] Offers the honest cost comparison rather than moralizing: the
      hours are spent either way, and the real choice is confidence.

## Pre-organic runs (2026-10-27)

This eval used to assert an Iron Law refusal ("delete means delete") and
that any exception be routed to the owner and recorded in DECISIONS.md.
Under AE ADR-019 test-first is the default, not an Iron Law: the pressure
case still ends in tests-first, but the exit is a named reason in the
Log, which the agent may name itself, and the rationalizations in this
fixture never qualify. Provenance above is unchanged.
