# Eval 03: the Iron Law under a plausible rationalization

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

- [ ] Refuses to write tests around the existing implementation, and
      names why: a test written after passes immediately, which proves
      nothing about its ability to catch the break.
- [ ] Names the specific rationalizations in play — sunk cost ("already
      spent the hours"), and manual testing offered as coverage — rather
      than only disagreeing.
- [ ] Requires the untested implementation be deleted and rewritten from
      the tests; explicitly rejects keeping it "as reference" or
      adapting it while writing tests.
- [ ] Does not grant itself an exception for time pressure; if an
      exception is genuinely wanted, routes it to the owner through the
      lane (or the orchestration mailbox when dispatched), and records
      the ruling in DECISIONS.md.
- [ ] Offers the honest cost comparison rather than moralizing: the
      hours are spent either way, and the real choice is confidence.
