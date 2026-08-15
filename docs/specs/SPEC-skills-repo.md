# SPEC: Standalone skills repo

Date: 2026-07-30
Status: Approved (design validated interactively)

## Purpose

A standalone public repo (`bygama/skills`) holding mateo's personal Agent
Skills library for Claude Code. It splits the *skill library* (opinions and
methodologies) from the *context-engineering standard*: the
Context-Engineering repo keeps only the skills that replicate and enforce the
standard (`context-init`, `context-audit`), and this repo takes everything
else, starting with the two methodology skills salvaged from OMC.

## Decisions (fixed)

- Name `skills`, public, MIT, personal-public community profile per
  Context-Engineering `templates/community/MATRIX.md`.
- Layout mirrors Context-Engineering: skills live under `skills/<name>/`; the
  repo root holds CLAUDE.md, AGENTS.md, README, LICENSE, and `docs/`.
- `reviewing-plans` and `tracing-root-causes` move here whole (SKILL.md,
  `references/`, `evals/`). Git history stays in Context-Engineering;
  provenance is recorded in ADR-001 here and in the move commits.
- Hard constraint carried over from Context-Engineering: every skill ships
  3 evals; evals change BEFORE skill content.
- The repo must itself pass `context-audit` (dogfooding), instantiated per
  the standard with `context-audit` as the final gate.
- workstation's `claude/install.ps1` junction-links skills from a LIST of
  source roots (`Context-Engineering/skills` + `skills/skills`) instead of a
  single one.
- New row in workstation `dev/repos/mine.md`: `skills | bygama/skills`.
- GitHub settings per repo conventions: rebase-only merges, auto-delete
  branches on merge.

## Migration order (junction safety)

The live `~/.claude/skills` junctions for the two moved skills point into
Context-Engineering; deleting there before repointing would break skill
discovery in new sessions. Order is therefore:

1. Create this repo with skeleton + moved skills; commit.
2. Publish to GitHub; apply repo settings.
3. Update the workstation installer (multi-source) and `dev/repos/mine.md`;
   run `tests/run.ps1` and `install.ps1 -WhatIfOnly`; then run the installer
   for real so the junctions repoint here.
4. Only then delete the two skills from Context-Engineering and update its
   docs (CLAUDE.md Map line, `reference/agents.md` pointer); re-run its
   self-lint and lint tests.

## Verification

- `context-lint` passes against this repo; `context-audit` reports no
  findings.
- workstation `tests/run.ps1` passes; `-WhatIfOnly` reports only the intended
  junction changes; the real run leaves all four skill junctions in
  `~/.claude/skills` pointing at existing targets.
- Context-Engineering self-lint and `tests/run-lint-tests.mjs` pass after the
  removal.
- Discovery check: the moved skills' SKILL.md files resolve through their
  junctions.
