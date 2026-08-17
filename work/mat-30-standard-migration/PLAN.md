# skills → AE standard migration — plan

<!-- Steps with executable acceptance. Executed via relay (controller +
     fresh implementer subagent per step); commit per step on this
     branch, conventional commits, English. -->

- [ ] S1: Canonical AGENTS.md instantiated from
  `../Agent-Engineering/templates/repo/AGENTS.md.template` per the SPEC
  move map (stamp AE/1.1.0, tier one-liner, four blocks) + CLAUDE.md
  reduced to the ≤3-line pointer — accept:
  `node ../Agent-Engineering/scripts/agent-lint.mjs .` exits 0
- [ ] S2: Dead-name fixes in README.md per the SPEC list — accept:
  `git grep -iE "context-engineering|context-lint|context-init|context-audit" -- README.md CLAUDE.md AGENTS.md`
  exits 1 (no matches)
- [ ] S3: `docs/tiers.md` copied from
  `../Agent-Engineering/templates/repo/docs/tiers.md` — accept: file
  exists AND `node ../Agent-Engineering/scripts/agent-lint.mjs .`
  exits 0
- [ ] S4 (controller, post-relay): agent-audit final gate + work-verify
  M DoD — accept: audit reports compliant at AE/1.1.0; PASS block in
  PROGRESS
- [ ] S5 (controller): work-handoff close + PR — accept: PR opens green
  with `Closes MAT-30`
