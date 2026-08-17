# skills

Standard: AE/1.3.0

Personal Agent Skills library for Claude Code — methodology skills usable
from any repo. The agent-engineering standard and its enforcement skills
live in the Agent-Engineering repo; this repo holds the personal
methodology skills.

Tiers: S direct+verify · M lane+plan · L four files+feature list · XL fan-out — doubt → higher (docs/tiers.md).

## Commands

- `node ../Agent-Engineering/scripts/agent-lint.mjs .` — mechanical
  compliance check (needs the Agent-Engineering repo cloned alongside)

## Gotchas

- Skills here are junction-linked into `~/.claude/skills` by the
  workstation installer: edits go live immediately, no copy step; a NEW
  skill needs one installer run to create its junction.
- The frontmatter `description` is the discovery interface — Claude
  picks skills by description alone; keep what + when (triggers) sharp.

## Hard constraints

- Every skill ships with ≥3 evals (`evals/`); evals change BEFORE skill
  content.
- Nothing here may violate the agent-engineering standard (authoring
  rules: `../Agent-Engineering/reference/skills.md` — SKILL.md <500
  lines, references one level deep, third-person descriptions).
