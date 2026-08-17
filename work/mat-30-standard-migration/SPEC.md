---
issue: MAT-30
---
# skills → AE standard migration — spec

<!-- Owner-written. The agent never edits this file. -->

Done looks like: this v1 repo (canonical CLAUDE.md + stub AGENTS.md, no
stamp) carries the standard's v2 shape at AE/1.1.0, with zero dead
pre-rename references, and `node ../Agent-Engineering/scripts/agent-lint.mjs .`
exits 0. Owner approved this plan in chat, 2026-08-17.

## Move map (CLAUDE.md → canonical AGENTS.md)

Instantiate from `../Agent-Engineering/templates/repo/AGENTS.md.template`
(stamp `Standard: AE/1.1.0`, tier one-liner, four blocks):

- Summary (CLAUDE.md L3-6): personal Agent Skills library for Claude
  Code — methodology skills usable from any repo. Renamed: the
  agent-engineering standard and its enforcement skills live in the
  **Agent-Engineering** repo; this repo holds the personal methodology
  skills.
- Commands (L10-11): `node ../Agent-Engineering/scripts/agent-lint.mjs .`
  — mechanical compliance check (needs Agent-Engineering cloned
  alongside). Verified 2026-08-17.
- Gotchas (L15-19), verbatim: junction-linked into `~/.claude/skills`
  by the workstation installer (edits live immediately; a NEW skill
  needs one installer run); frontmatter `description` is the discovery
  interface — keep what + when sharp.
- Hard constraints (L23-27): every skill ships with ≥3 evals, evals
  change BEFORE skill content; nothing here may violate the
  agent-engineering standard (authoring rules:
  `Agent-Engineering/reference/skills.md` — SKILL.md <500 lines,
  references one level deep, third-person descriptions).

CLAUDE.md becomes the ≤3-line pointer (`@AGENTS.md`), per
`../Agent-Engineering/templates/repo/CLAUDE.md.template` if present, else
the house pointer shape.

## Dead-name fixes (README.md)

`Context-Engineering` → `Agent-Engineering` (prose AND GitHub links),
`context-lint.mjs` → `agent-lint.mjs`, `context-init`/`context-audit` →
`agent-init`/`agent-audit`, "context-engineering standard" →
"agent-engineering standard". Content meaning unchanged; ADR/spec/plan
files under docs/ are historical records — do NOT rewrite them.

## Additions

- `docs/tiers.md` copied from
  `../Agent-Engineering/templates/repo/docs/tiers.md` (no placeholders
  in that file; copy is the correct instantiation).

## Keep untouched

`skills/` (all four skills and their evals), `docs/` existing content,
LICENSE, CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, `.github/`, git
history. Layout `skills/<name>/` must not change (workstation junctions
depend on it).
