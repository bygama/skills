# skills

Personal [Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
library for Claude Code — methodology skills usable from any repo, installed
globally as junction links by the `workstation` repo's installer.

Built on the agent-engineering standard defined in
[Agent-Engineering](https://github.com/bygama/Agent-Engineering):
SKILL.md under 500 lines, references one level deep, third-person
descriptions with triggers, and 3 evals per skill that change before the
skill's content does.

These skills encode personal judgment — opinionated guidance over rule
lists, per the Claude 5-era shift the standard is built on.

## Skills

| Skill | What it does |
|---|---|
| [`tracing-root-causes/`](skills/tracing-root-causes/) | Owns debugging end to end: reproduce, isolate, competing hypotheses, evidence ranked by strength, active disconfirmation, fix at the source |
| [`designing-consistently/`](skills/designing-consistently/) | Keeps UI work consistent with an app's DESIGN.md: read before building, consume tokens, record decisions as a gated step |
| [`extracting-design-md/`](skills/extracting-design-md/) | Reverse-engineers a DESIGN.md from an existing project: evidenced drift report, collapsed tokens, backfilled decisions, migration plan with a convergence metric |
| [`testing-first/`](skills/testing-first/) | Test-first implementation: the Iron Law, RED-GREEN-REFACTOR with both verification beats mandatory, cycle evidence into the lane, completion handed to `work-verify` |

## Authoring method

Every skill here is authored and verified with two complementary lenses:

- [`writing-skills`](https://github.com/obra/superpowers) — from the
  superpowers set by Jesse Vincent: TDD applied to documentation. No skill
  (or edit to one) without a failing test first; baseline subagent runs are
  observed before any content is written.
- [`skill-creator`](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
  — by Anthropic: paired baseline/with-skill runs, objectively graded
  assertions, and aggregated benchmarks (e.g. `extracting-design-md`
  shipped at 8/8 assertions vs a 4/8 baseline, delta +0.50).

## Used alongside (not in this repo)

Day to day this library is complemented by two plugin skill sets, installed
from the official marketplace by the `workstation` installer:

- [`frontend-design`](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/frontend-design)
  (Anthropic) — UI/UX implementation guidance.
- [`superpowers`](https://github.com/obra/superpowers) by Jesse Vincent —
  collaboration workflows (vendored in the
  [official marketplace](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/superpowers)).

## Provenance

Both initial skills were salvaged from oh-my-claudecode's agent pack (see
Agent-Engineering ADR-002) and first lived in that repo; they moved here
when the library split from the standard
([ADR-001](docs/adrs/ADR-001-standalone-skills-repo.md)).

[`testing-first/`](skills/testing-first/) is derived-with-notice from
`test-driven-development` and its `writing-good-tests` reference in
[superpowers](https://github.com/obra/superpowers) v6.3.0, MIT ©
2025 Jesse Vincent — first ported under MAT-47, classified substantial
(MAT-94); evidence:
[DECISIONS.md](https://github.com/bygama/skills/blob/002ef05fd8b9aceab6f7ed9f14cb8b8fa076441b/work/mat-94-attribution-skills/DECISIONS.md), full
permission text in [`NOTICE`](NOTICE).

[`tracing-root-causes/`](skills/tracing-root-causes/) is
derived-with-notice, in part, from `systematic-debugging` and its
technique references in
[superpowers](https://github.com/obra/superpowers) v6.3.0, MIT ©
2025 Jesse Vincent — the base is owner-original (Context-Engineering
salvage); classified substantial in five named `SKILL.md` sections plus
`references/techniques.md` in full (MAT-94); evidence:
[DECISIONS.md](https://github.com/bygama/skills/blob/002ef05fd8b9aceab6f7ed9f14cb8b8fa076441b/work/mat-94-attribution-skills/DECISIONS.md), full
permission text in [`NOTICE`](NOTICE).
