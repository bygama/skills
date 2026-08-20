# Contributing to skills

## Workflow

- Branch from `main`: `feat/<topic>` or `fix/<topic>`.
- Conventional commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`.
- Keep commits atomic — one "why" per commit.

## Before opening a PR

Walk the changed skill's 3 evals (`skills/<name>/evals/`) — expected behavior
must still hold; if behavior changes, update the evals FIRST, then the skill.
Run `node ../Agent-Engineering/scripts/agent-lint.mjs .` — must pass.

## Merging

- Merge strategy: rebase only (linear history); branches auto-delete on merge.
- PRs need review before merge; keep them small and focused.
