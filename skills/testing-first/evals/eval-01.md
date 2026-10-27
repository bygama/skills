# Eval 01: cycle evidence lands in the lane's Log, not in the narration

## Query

"Add retry-with-backoff to the uploader — three attempts, doubling
delay."

## Fixture

A lane is open at `work/uploader-retry/`, with `LANE.md` carrying
`## Specs` (S1: «three attempts, doubling delay»), `## Tasks` and an
append-only `## Log`. The repo's `## Commands` block in AGENTS.md gives
`npm test` as the suite command. No test exists for the uploader yet.

Provenance — friction recorded from daily use of the borrowed skill
(MAT-47), not a controlled baseline run: the RED and GREEN
runs happen and are described in the reply, then the reply ends. The
lane's log never receives them, so the later completion gate has
prose to read and no command evidence.

## Expected behavior

- [ ] Writes the failing test before any implementation, runs it, and
      shows the actual failure output — not a claim that it would fail.
- [ ] Confirms the failure is the *expected* one (feature missing), and
      says which failure it saw; a test that errors or passes sends it
      back to the test, not forward to the code.
- [ ] Appends the cycle's evidence to the `## Log` of
      `work/uploader-retry/LANE.md` as it happens — the command, its
      exit, and the one-line result for both the RED and the GREEN run.
- [ ] Ticks the task in `## Tasks` only after its proof is in the Log,
      and does not create a PROGRESS.md for a lane that has a LANE.md.
- [ ] On a v1 lane (no LANE.md, a PROGRESS.md present), appends the same
      entries to PROGRESS.md instead.
- [ ] Creates no artifact outside the lane for this: no scratch test
      plan, no notes file, no separate ledger.
- [ ] Writes only enough implementation to pass the test — no
      configurable backoff strategy, no retry hooks nobody asked for.

## Pre-organic runs (2026-10-27)

Before AE ADR-019 this eval asserted the evidence went to
`work/uploader-retry/PROGRESS.md` for an "M lane". The provenance above
is unchanged; only the destination moved (LANE.md `## Log`, PROGRESS.md
kept for v1 lanes) and the "M and above" scoping became "inside a lane".
