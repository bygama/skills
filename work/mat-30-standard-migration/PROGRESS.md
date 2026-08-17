# skills → AE standard migration — progress

## Done

- 2026-08-17 — S1 — Instantiated canonical AGENTS.md from
  `../Agent-Engineering/templates/repo/AGENTS.md.template` per the SPEC
  move map (stamp `Standard: AE/1.1.0`, tier one-liner, Commands/
  Gotchas/Hard constraints blocks carried over from the old canonical
  CLAUDE.md); reduced CLAUDE.md to the `@AGENTS.md` pointer — accept:
  `node ../Agent-Engineering/scripts/agent-lint.mjs .` (run from
  C:\Briar\repos\mine\skills) → exit 0 (0 high, 0 medium, 0 low).
- 2026-08-17 — S2 — Dead-name fixes in README.md per the SPEC list
  (context-engineering → agent-engineering, context-lint.mjs →
  agent-lint.mjs, context-init/context-audit → agent-init/agent-audit,
  "context-engineering standard" → "agent-engineering standard"; AGENTS.md
  and CLAUDE.md already cleaned in S1) — accept: `git grep -iE
  "context-engineering|context-lint|context-init|context-audit" -- README.md
  CLAUDE.md AGENTS.md` (run from C:\Briar\repos\mine\skills) → exit 1 (no
  matches).
- 2026-08-17 — S3 — Copied `docs/tiers.md` verbatim from
  `../Agent-Engineering/templates/repo/docs/tiers.md` (verified: zero
  {{PLACEHOLDER}} markers in source) — accept: file exists AND
  `node ../Agent-Engineering/scripts/agent-lint.mjs .` (run from
  C:\Briar\repos\mine\skills) → exit 0 (0 high, 0 medium, 0 low).

## In progress

- 2026-08-17 — S5: handoff close + PR.

## Tried and failed

## Next

- S1 dispatch (implementer subagent, mid tier — template instantiation
  with content moves needs judgment).

## Verification

<!-- PASS evidence only, written by work-verify (newest on top); the close
     handoff refuses to close a lane without a current PASS block here. -->

### 2026-08-17 — M DoD — PASS
- L1 static: `node ../Agent-Engineering/scripts/agent-lint.mjs .` → exit 0 (0 high, 0 medium, 0 low)
- L2 behavioral: n/a — markdown-only repo, no runtime; agent-lint is the repo's only executable check (recorded decision)
- L3 end-to-end: PLAN acceptance re-run independently by the fresh reviewer — dead-name grep → exit 1 (no matches); docs/tiers.md byte-identical to the template
- Fresh-context review: PASS — move map verified line-by-line against `5545a85:CLAUDE.md`; 1 low finding (uncommitted S3 tick), fixed in the finalize commit
- Adversarial review: n/a — M tier, not requested
- agent-audit (agent-init final gate): 10/10, no findings, stamp AE/1.1.0 current
- Relay run record: S1 (sonnet) d2ac209, S2 (haiku) bd933be, S3 (haiku) 19f7627 — each step per-step reviewed (SPEC PASS / QUALITY APPROVED ×3), fix loop never triggered
