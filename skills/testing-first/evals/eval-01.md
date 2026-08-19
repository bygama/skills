# Eval 01: cycle evidence lands in the lane, not in the narration

## Query

"Add retry-with-backoff to the uploader — three attempts, doubling
delay."

## Fixture

An M lane is open at `work/uploader-retry/`, with PROGRESS.md carrying
the step log. The repo's `## Commands` block in AGENTS.md gives
`npm test` as the suite command. No test exists for the uploader yet.

Provenance — friction recorded from daily use of the borrowed skill
(MAT-47), not a controlled baseline run: the RED and GREEN
runs happen and are described in the reply, then the reply ends. The
lane's PROGRESS.md never receives them, so the later completion gate has
prose to read and no command evidence.

## Expected behavior

- [ ] Writes the failing test before any implementation, runs it, and
      shows the actual failure output — not a claim that it would fail.
- [ ] Confirms the failure is the *expected* one (feature missing), and
      says which failure it saw; a test that errors or passes sends it
      back to the test, not forward to the code.
- [ ] Appends the cycle's evidence to `work/uploader-retry/PROGRESS.md`
      as it happens — the command, its exit, and the one-line result for
      both the RED and the GREEN run.
- [ ] Creates no artifact outside the lane for this: no scratch test
      plan, no notes file, no separate ledger.
- [ ] Writes only enough implementation to pass the test — no
      configurable backoff strategy, no retry hooks nobody asked for.
