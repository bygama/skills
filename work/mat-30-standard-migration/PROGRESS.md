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

## In progress

- 2026-08-17 — Lane opened; migration plan approved by owner in chat.
  Executing S1-S3 via relay (first production relay run).

## Tried and failed

## Next

- S1 dispatch (implementer subagent, mid tier — template instantiation
  with content moves needs judgment).

## Verification

<!-- PASS evidence only, written by work-verify (newest on top); the close
     handoff refuses to close a lane without a current PASS block here. -->
