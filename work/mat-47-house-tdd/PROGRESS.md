# PROGRESS — MAT-47 house TDD skill

Lane opened 2026-08-19. Tier M. Branch `bygama/mat-47-house-tdd`.

## Verification command (read this first)

AGENTS.md cites `node ../Agent-Engineering/scripts/agent-lint.mjs .`. That
sibling path does **not** resolve from an Orca worktree — this lane runs
the lint through the absolute path instead:

```
node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .
```

Same script, same rules; only the invocation differs. See DECISIONS.md
D-002 for why AGENTS.md is not edited to match.

## Baseline — the one finding is pre-existing on clean main

Run against a pristine export of `origin/main` (0e6faad), in a directory
with no `Agent-Engineering` sibling, so nothing from this lane is present:

```
$ git archive origin/main | tar -x -C <scratch>/main-baseline
$ node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs <scratch>/main-baseline
agent-lint ...\main-baseline
  MEDIUM AGENTS.md:15  file not found: ../Agent-Engineering/scripts/agent-lint.mjs  [cmd-drift]
0 high, 1 medium, 0 low — FAIL
EXIT=1
```

Per D-002 this lane's acceptance is therefore: the summary line stays
`0 high, 1 medium, 0 low — FAIL` and the sole finding stays the
AGENTS.md:15 cmd-drift. Any second finding is attributable to this lane
and must be fixed. CI `standard` on the PR is the authoritative gate — in
CI the sibling checkout exists, so it lints clean.

## Step log

| Step | State | Evidence |
|---|---|---|
| 1. Lane opened | done | this file; `git show --stat` touches nothing under `skills/` |
| 2. Evals (≥3) | pending | |
| 3. `references/writing-good-tests.md` | pending | |
| 4. `SKILL.md` | pending | |
| 5. README index surfaces | pending | |
