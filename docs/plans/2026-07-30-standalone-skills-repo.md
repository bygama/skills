# Standalone Skills Repo Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the public `bygama/skills` repo holding the personal Agent Skills library, move `reviewing-plans` and `tracing-root-causes` into it from Context-Engineering, and repoint the workstation junction installer — without ever leaving `~/.claude/skills` junctions dangling.

**Architecture:** Three repos touched in a fixed order (create+publish new repo → repoint installer → delete from Context-Engineering). The installer gains a source-root list with name-level dedupe (later source wins), which makes the overlap window — when both repos contain the two skills — idempotent.

**Tech Stack:** Git/GitHub (`gh` CLI), PowerShell 7 (workstation installer), Node (context-lint), NTFS junctions.

## Global Constraints

- All artifacts in English; chat with the user in rioplatense Spanish.
- Conventional commits (`feat:`, `fix:`, `docs:`, `chore:`); every commit ends with `Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>`.
- On existing repos (Context-Engineering, workstation): branch from `main`, ff-merge back, delete the branch. The new repo's bootstrap commits go straight to `main`.
- Repo CLAUDE.md ≤60 lines; SKILL.md <500 lines; references one level deep.
- Every skill ships 3 evals; evals change BEFORE skill content.
- Never claim a step done without running its verification command.
- Absolute paths: repos live under `C:\Briar\repos\mine\`.

---

### Task 1: skills repo skeleton

**Files:**
- Create: `C:\Briar\repos\mine\skills\CLAUDE.md`
- Create: `C:\Briar\repos\mine\skills\AGENTS.md`
- Create: `C:\Briar\repos\mine\skills\README.md`
- Create: `C:\Briar\repos\mine\skills\LICENSE`
- Create: `C:\Briar\repos\mine\skills\SECURITY.md`
- Create: `C:\Briar\repos\mine\skills\CODE_OF_CONDUCT.md`
- Create: `C:\Briar\repos\mine\skills\CONTRIBUTING.md`
- Create: `C:\Briar\repos\mine\skills\.github\PULL_REQUEST_TEMPLATE.md`
- Create: `C:\Briar\repos\mine\skills\.github\ISSUE_TEMPLATE\bug_report.md`
- Create: `C:\Briar\repos\mine\skills\.github\ISSUE_TEMPLATE\feature_request.md`
- Create: `C:\Briar\repos\mine\skills\docs\README.md`
- Create: `C:\Briar\repos\mine\skills\docs\adrs\ADR-001-standalone-skills-repo.md`

**Interfaces:**
- Consumes: repo already exists with `main` branch and one commit (founding spec).
- Produces: a repo skeleton that later tasks publish and link against; `skills/` subdirectory is created by Task 2.

The repo (`git init -b main`) and `docs/specs/SPEC-skills-repo.md` already exist. Community-file set follows Context-Engineering `templates/community/MATRIX.md`, Public OSS column: README, LICENSE (MIT), SECURITY, CONTRIBUTING, CODE_OF_CONDUCT, issue + PR templates.

- [ ] **Step 1: Write CLAUDE.md** (exactly this content):

```markdown
# skills

Personal Agent Skills library for Claude Code: methodology skills usable from
any repo. The context-engineering standard and its enforcement skills
(context-init, context-audit) live in the Context-Engineering repo; this repo
holds everything else.

## Commands

- `node ../Context-Engineering/scripts/context-lint.mjs .` — mechanical
  compliance check (needs the Context-Engineering repo cloned alongside)

## Gotchas

- Skills here are junction-linked into `~/.claude/skills` by the workstation
  installer: edits go live immediately, no copy step; a NEW skill needs one
  installer run to create its junction.
- The frontmatter `description` is the discovery interface — Claude picks
  skills by description alone; keep what + when (triggers) sharp.

## Hard constraints

- Every skill ships with 3 evals (`evals/`); evals change BEFORE skill
  content.
- Nothing here may violate the context-engineering standard (SKILL.md <500
  lines, references one level deep, third-person descriptions).
```

- [ ] **Step 2: Write AGENTS.md** (exactly this content):

```markdown
# skills

Personal Agent Skills library for Claude Code: methodology skills
(kebab-case verb phrases) under `skills/`, each with references and 3 evals.

Claude Code is the primary tool for this repo; the maintained context file is
`CLAUDE.md`. This file exists as a minimal entry point for other AI tools.
```

- [ ] **Step 3: Write README.md** (exactly this content):

```markdown
# skills

Personal [Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
library for Claude Code — methodology skills usable from any repo, installed
globally as junction links by the `workstation` repo's installer.

Built on the context-engineering standard defined in
[Context-Engineering](https://github.com/bygama/Context-Engineering):
SKILL.md under 500 lines, references one level deep, third-person
descriptions with triggers, and 3 evals per skill that change before the
skill's content does.

## Skills

| Skill | What it does |
|---|---|
| [`reviewing-plans/`](skills/reviewing-plans/) | Adversarial review of plans, specs, and proposals before execution |
| [`tracing-root-causes/`](skills/tracing-root-causes/) | Disciplined causal analysis: competing hypotheses, evidence ranked by strength, active disconfirmation |

## Provenance

Both initial skills were salvaged from oh-my-claudecode's agent pack (see
Context-Engineering ADR-002) and first lived in that repo; they moved here
when the library split from the standard
([ADR-001](docs/adrs/ADR-001-standalone-skills-repo.md)).
```

- [ ] **Step 4: Write LICENSE** — Context-Engineering `templates/community/LICENSE-MIT.template` with `{{YEAR}}` → `2026`, `{{OWNER}}` → `Mateo García (bygama)` (same owner string as Context-Engineering's LICENSE).

- [ ] **Step 5: Write SECURITY.md** (exactly this content):

```markdown
# Security Policy

## Reporting a vulnerability

Use [GitHub private vulnerability reporting](https://github.com/bygama/skills/security/advisories/new).
Do not open public issues for security problems.

You can expect an acknowledgment within a week.

## Scope

Supported: the `main` branch. This repo is markdown-only (skills); the main
risk class is a skill instructing an agent to do something unsafe — report
exactly that.
```

- [ ] **Step 6: Write CODE_OF_CONDUCT.md** (exactly this content):

```markdown
# Code of Conduct

This project adopts the [Contributor Covenant v2.1](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).

Report unacceptable behavior via a GitHub issue mentioning @bygama, or
privately through GitHub's report abuse tools.
```

- [ ] **Step 7: Write CONTRIBUTING.md** (template instantiated — exactly this content):

```markdown
# Contributing to skills

## Workflow

- Branch from `main`: `feat/<topic>` or `fix/<topic>`.
- Conventional commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`.
- Keep commits atomic — one "why" per commit.

## Before opening a PR

Walk the changed skill's 3 evals (`skills/<name>/evals/`) — expected behavior
must still hold; if behavior changes, update the evals FIRST, then the skill.
Run `node ../Context-Engineering/scripts/context-lint.mjs .` — must pass.

## Merging

- Merge strategy: rebase only (linear history); branches auto-delete on merge.
- PRs need review before merge; keep them small and focused.
```

- [ ] **Step 8: Copy the three .github templates verbatim** from `C:\Briar\repos\mine\Context-Engineering\templates\community\.github\` (PULL_REQUEST_TEMPLATE.md, ISSUE_TEMPLATE/bug_report.md, ISSUE_TEMPLATE/feature_request.md) — no placeholders inside them.

- [ ] **Step 9: Write docs/README.md** (exactly this content):

```markdown
# Docs

| Area | What lives there |
|---|---|
| [`adrs/`](adrs/) | Architecture decision records (`ADR-NNN-<topic>.md`) |
| [`specs/`](specs/) | Feature specs (`SPEC-<feature>.md`) |
| [`plans/`](plans/) | Implementation plans (`YYYY-MM-DD-<topic>.md`) |
```

- [ ] **Step 10: Write docs/adrs/ADR-001-standalone-skills-repo.md** (exactly this content):

```markdown
# ADR-001: Standalone skills repo

Date: 2026-07-30
Status: Accepted

## Context

The methodology skills `reviewing-plans` and `tracing-root-causes` (salvaged
from OMC — Context-Engineering ADR-002) lived in the Context-Engineering
repo, whose purpose is the context-engineering standard and its enforcement
tooling. A growing personal skill library inside the standard's repo blurs
both identities: the standard repo bloats, and skills unrelated to context
engineering inherit its release rhythm.

## Decision

Split the library from the standard. This repo (`bygama/skills`, public)
holds the personal Agent Skills library; Context-Engineering keeps only
`context-init` and `context-audit`, which ARE the standard's tooling. The
workstation installer junction-links skills from both repos. Git history of
the moved skills stays in Context-Engineering; this ADR and the move commits
record provenance.

## Consequences

- New skills default to this repo; Context-Engineering only gains a skill if
  it enforces the standard itself.
- The workstation installer maintains a skill source-root list (two today).
- The 3-evals rule and the standard's budgets carry over as hard constraints.
```

- [ ] **Step 11: Verify budgets and commit**

Run: `cd C:\Briar\repos\mine\skills; (Get-Content CLAUDE.md).Count` → expect ≤60.
Run: `git add -A; git commit -m "feat: instantiate repo skeleton per the context-engineering standard"` (with the Co-Authored-By trailer).

---

### Task 2: Move the two skills in (copy — do NOT delete from Context-Engineering yet)

**Files:**
- Create: `C:\Briar\repos\mine\skills\skills\reviewing-plans\` (SKILL.md, `references\gap-taxonomy.md`, `evals\eval-01.md..eval-03.md`)
- Create: `C:\Briar\repos\mine\skills\skills\tracing-root-causes\` (SKILL.md, `evals\eval-01.md..eval-03.md`)

**Interfaces:**
- Consumes: Task 1 skeleton.
- Produces: `C:\Briar\repos\mine\skills\skills\<name>` directories that Task 4's installer links and Task 5's deletion assumes present.

- [ ] **Step 1: Copy both skill directories** from `C:\Briar\repos\mine\Context-Engineering\skills\`:

```powershell
Copy-Item -Recurse C:\Briar\repos\mine\Context-Engineering\skills\reviewing-plans C:\Briar\repos\mine\skills\skills\reviewing-plans
Copy-Item -Recurse C:\Briar\repos\mine\Context-Engineering\skills\tracing-root-causes C:\Briar\repos\mine\skills\skills\tracing-root-causes
```

- [ ] **Step 2: Verify the copy is complete** — file count and content identical:

```powershell
(Get-ChildItem -Recurse -File C:\Briar\repos\mine\skills\skills).Count   # expect 9 (5 + 4)
git -C C:\Briar\repos\mine\Context-Engineering status --porcelain        # expect empty (source untouched)
```

- [ ] **Step 3: Lint gate**

Run: `cd C:\Briar\repos\mine\skills; node ..\Context-Engineering\scripts\context-lint.mjs .`
Expected: `0 high, 0 medium, 0 low — PASS` (this also validates the CLAUDE.md `node` command path).

- [ ] **Step 4: Commit**

```powershell
git add skills; git commit -m "feat: adopt reviewing-plans and tracing-root-causes from Context-Engineering"
```

---

### Task 3: Publish to GitHub with repo conventions

**Files:** none (remote state only).

**Interfaces:**
- Consumes: local `main` with Tasks 1-2 committed.
- Produces: `https://github.com/bygama/skills` (public), which `dev/repos/mine.md` (Task 4) references.

- [ ] **Step 1: Create and push**

```powershell
cd C:\Briar\repos\mine\skills
gh repo create bygama/skills --public --source . --push --description "Personal Agent Skills library for Claude Code"
```

- [ ] **Step 2: Apply merge conventions** (rebase-only, auto-delete branches):

```powershell
gh api -X PATCH repos/bygama/skills -F allow_merge_commit=false -F allow_squash_merge=false -F allow_rebase_merge=true -F delete_branch_on_merge=true
```

- [ ] **Step 3: Verify**

Run: `gh repo view bygama/skills --json visibility,deleteBranchOnMerge,rebaseMergeAllowed,mergeCommitAllowed,squashMergeAllowed`
Expected: `"visibility": "PUBLIC"`, `deleteBranchOnMerge: true`, `rebaseMergeAllowed: true`, the other two `false`.

---

### Task 4: workstation installer — multi-source junctions

**Files:**
- Modify: `C:\Briar\repos\mine\workstation\claude\install.ps1` (skills section, currently `Write-Step 'Skills (Context-Engineering junctions)'` near line 227, and the stale comment near line 71)
- Modify: `C:\Briar\repos\mine\workstation\dev\repos\mine.md` (add row)
- Modify: `C:\Briar\repos\mine\workstation\CLAUDE.md` (gotcha wording)

**Interfaces:**
- Consumes: `C:\Briar\repos\mine\skills\skills\` populated (Task 2).
- Produces: `~/.claude/skills` junctions for the two moved skills pointing at the NEW repo; Task 5 relies on this before deleting from Context-Engineering.

- [ ] **Step 1: Branch**

```powershell
cd C:\Briar\repos\mine\workstation; git switch -c feat/multi-source-skill-junctions
```

- [ ] **Step 2: Replace the skills section** of `claude/install.ps1`. Current block:

```powershell
Write-Step 'Skills (Context-Engineering junctions)'
$ceSkillsSrc = Join-Path (Get-LayoutPath 'repos') 'mine\Context-Engineering\skills'
$skillsDst = Join-Path $claudeHome 'skills'
if (-not (Test-Path $ceSkillsSrc)) {
    Write-Warn2 "Context-Engineering repo not found at $ceSkillsSrc; clone it (dev/repos), then re-run"
    $failed.Add('skills: Context-Engineering repo missing')
}
else {
    if (-not (Test-Path $skillsDst) -and -not $script:DryRun) {
        New-Item -ItemType Directory $skillsDst -Force | Out-Null
    }
    foreach ($src in Get-ChildItem $ceSkillsSrc -Directory | Sort-Object Name) {
        $link = Join-Path $skillsDst $src.Name
        $existing = Get-Item $link -ErrorAction SilentlyContinue
        if ($existing -and $existing.LinkType -eq 'Junction' -and $existing.LinkTarget -eq $src.FullName) {
            Write-Ok "$($src.Name) junction already in place"
            continue
        }
        if ($script:DryRun) { Write-Would "link skill $($src.Name) -> $($src.FullName)"; continue }
        if ($existing) {
            Backup-ExistingFile $link | Out-Null
            Remove-Item -LiteralPath $link -Recurse -Force
        }
        New-Item -ItemType Junction -Path $link -Target $src.FullName | Out-Null
        Write-Ok "$($src.Name) junction created"
        $configChanged = $true
    }
}
```

New block (name-level dedupe, later source wins, so the overlap window while both repos hold a skill is idempotent):

```powershell
Write-Step 'Skills (repo junctions)'
$skillSources = @(
    (Join-Path (Get-LayoutPath 'repos') 'mine\Context-Engineering\skills'),
    (Join-Path (Get-LayoutPath 'repos') 'mine\skills\skills')
)
$skillsDst = Join-Path $claudeHome 'skills'
if (-not (Test-Path $skillsDst) -and -not $script:DryRun) {
    New-Item -ItemType Directory $skillsDst -Force | Out-Null
}
# One junction per skill NAME. When two sources ship the same name, the later
# source in $skillSources wins, so a skill migrating between repos never
# flip-flops its junction within one run.
$skillDirs = [ordered]@{}
foreach ($skillsSrc in $skillSources) {
    if (-not (Test-Path $skillsSrc)) {
        Write-Warn2 "skills source not found at $skillsSrc; clone it (dev/repos), then re-run"
        $failed.Add("skills: source missing: $skillsSrc")
        continue
    }
    foreach ($src in Get-ChildItem $skillsSrc -Directory) { $skillDirs[$src.Name] = $src }
}
foreach ($name in @($skillDirs.Keys) | Sort-Object) {
    $src = $skillDirs[$name]
    $link = Join-Path $skillsDst $name
    $existing = Get-Item $link -ErrorAction SilentlyContinue
    if ($existing -and $existing.LinkType -eq 'Junction' -and $existing.LinkTarget -eq $src.FullName) {
        Write-Ok "$name junction already in place"
        continue
    }
    if ($script:DryRun) { Write-Would "link skill $name -> $($src.FullName)"; continue }
    if ($existing) {
        Backup-ExistingFile $link | Out-Null
        Remove-Item -LiteralPath $link -Recurse -Force
    }
    New-Item -ItemType Junction -Path $link -Target $src.FullName | Out-Null
    Write-Ok "$name junction created"
    $configChanged = $true
}
```

- [ ] **Step 3: Update the stale comment** near line 71 of `claude/install.ps1`. Replace:

```powershell
# User skills are junction links into the Context-Engineering repo, so editing that repo
# updates the live skills with no copy step. Linking every directory under its skills/
# keeps this declarative: a skill added there appears on the next run.
```

with:

```powershell
# User skills are junction links into the repos listed in $skillSources, so editing those
# repos updates the live skills with no copy step. Linking every directory under each
# source keeps this declarative: a skill added there appears on the next run.
```

- [ ] **Step 4: Add the mine.md row** — in `dev/repos/mine.md`'s `## The list` table append (match the table's column padding):

```markdown
| skills                   | `bygama/skills`                   |
```

- [ ] **Step 5: Update the workstation CLAUDE.md gotcha.** Replace:

```markdown
- `claude/CLAUDE.md` is a SYNCED COPY; the canonical source is
  `Context-Engineering/global/CLAUDE.md`. Skills are junction-linked from
  that repo by `claude/install.ps1`.
```

with:

```markdown
- `claude/CLAUDE.md` is a SYNCED COPY; the canonical source is
  `Context-Engineering/global/CLAUDE.md`. Skills are junction-linked from
  the `Context-Engineering` and `skills` repos by `claude/install.ps1`.
```

- [ ] **Step 6: Run the workstation test suite**

Run: `pwsh C:\Briar\repos\mine\workstation\tests\run.ps1`
Expected: all tests pass (exit 0). If a test asserts the old single-source shape, update THAT test to the new `$skillSources` contract — tests change with the contract, and say so in the commit.

- [ ] **Step 7: Dry-run the installer**

Run: `pwsh C:\Briar\repos\mine\workstation\claude\install.ps1 -WhatIfOnly`
Expected: `Would link skill reviewing-plans -> C:\Briar\repos\mine\skills\skills\reviewing-plans` and the same for `tracing-root-causes`; every other skill "already in place"; zero tracked or user-config writes reported.

- [ ] **Step 8: Real installer run (repoints the junctions)**

Run: `pwsh C:\Briar\repos\mine\workstation\claude\install.ps1`
Expected: both junctions re-created pointing at `C:\Briar\repos\mine\skills\skills\...`; exit 0.

- [ ] **Step 9: Verify junction targets resolve**

```powershell
Get-Item $HOME\.claude\skills\* | Select-Object Name, LinkTarget
Test-Path $HOME\.claude\skills\reviewing-plans\SKILL.md   # expect True
Test-Path $HOME\.claude\skills\tracing-root-causes\SKILL.md  # expect True
```

Expected: 4 junctions; the two moved ones target `...\repos\mine\skills\skills\...`.

- [ ] **Step 10: Commit and ff-merge**

```powershell
git add claude/install.ps1 dev/repos/mine.md CLAUDE.md
git commit -m "feat: junction-link skills from a source-root list (Context-Engineering + skills)"
git switch main; git merge --ff-only feat/multi-source-skill-junctions; git branch -d feat/multi-source-skill-junctions
```

(with the Co-Authored-By trailer.)

---

### Task 5: Delete the moved skills from Context-Engineering

**Files:**
- Delete: `C:\Briar\repos\mine\Context-Engineering\skills\reviewing-plans\`, `...\skills\tracing-root-causes\`
- Modify: `C:\Briar\repos\mine\Context-Engineering\CLAUDE.md` (Map line)
- Modify: `C:\Briar\repos\mine\Context-Engineering\reference\agents.md` (History pointer)

**Interfaces:**
- Consumes: Task 4 verified — junctions already point at the new repo, so this deletion breaks nothing live.
- Produces: Context-Engineering containing only `context-init` and `context-audit` under `skills/`.

- [ ] **Step 1: Branch**

```powershell
cd C:\Briar\repos\mine\Context-Engineering; git switch -c chore/move-methodology-skills-out
```

- [ ] **Step 2: Delete the two skill directories**

```powershell
git rm -r skills/reviewing-plans skills/tracing-root-causes
```

- [ ] **Step 3: Remove the Map line from CLAUDE.md.** Delete exactly this line:

```markdown
- Methodology skills (globally installable): `skills/tracing-root-causes/`, `skills/reviewing-plans/`
```

- [ ] **Step 4: Point reference/agents.md at the new home.** In the History section, replace:

```markdown
genuinely useful methodologies were salvaged as skills (`tracing-root-causes`,
`reviewing-plans`). Full matrix: `docs/adrs/ADR-003-omc-removal.md`. The
```

with:

```markdown
genuinely useful methodologies were salvaged as skills (`tracing-root-causes`,
`reviewing-plans`), now maintained in the standalone `skills` repo. Full
matrix: `docs/adrs/ADR-003-omc-removal.md`. The
```

- [ ] **Step 5: Check for other live references** (docs/specs and docs/plans are dated records — leave them):

Run: `git grep -n "reviewing-plans\|tracing-root-causes" -- . ':!docs/specs' ':!docs/plans' ':!docs/adrs'`
Expected: no hits outside `skills/context-*` (if any appear, fix them in this task).

- [ ] **Step 6: Self-lint + lint tests**

```powershell
node scripts/context-lint.mjs . --ignore examples,tests   # expect PASS
node tests/run-lint-tests.mjs                             # expect all 5 cases passed
```

- [ ] **Step 7: Commit and ff-merge**

```powershell
git add -A
git commit -m "chore: move reviewing-plans and tracing-root-causes to the skills repo"
git switch main; git merge --ff-only chore/move-methodology-skills-out; git branch -d chore/move-methodology-skills-out
```

(with the Co-Authored-By trailer; commit body notes the destination `bygama/skills` and ADR-001 there.)

---

### Task 6: End-to-end verification and sync

**Files:** none.

**Interfaces:**
- Consumes: all prior tasks.
- Produces: the spec's Verification section satisfied; remotes in sync.

- [ ] **Step 1: Junctions still resolve after the deletion**

```powershell
Test-Path $HOME\.claude\skills\reviewing-plans\SKILL.md      # expect True
Test-Path $HOME\.claude\skills\tracing-root-causes\SKILL.md  # expect True
Test-Path $HOME\.claude\skills\context-audit\SKILL.md        # expect True
Test-Path $HOME\.claude\skills\context-init\SKILL.md         # expect True
```

- [ ] **Step 2: Installer idempotence** — a second `-WhatIfOnly` reports nothing to do for skills:

Run: `pwsh C:\Briar\repos\mine\workstation\claude\install.ps1 -WhatIfOnly`
Expected: all four skills "already in place"; no "Would link".

- [ ] **Step 3: Clean trees everywhere**

Run: `git -C C:\Briar\repos\mine\skills status --porcelain; git -C C:\Briar\repos\mine\Context-Engineering status --porcelain; git -C C:\Briar\repos\mine\workstation status --porcelain`
Expected: all empty.

- [ ] **Step 4: Push all three repos** (skills was pushed at creation; push the new plan-doc commit plus Context-Engineering and workstation `main`):

```powershell
git -C C:\Briar\repos\mine\skills push
git -C C:\Briar\repos\mine\Context-Engineering push
git -C C:\Briar\repos\mine\workstation push
```

Expected: all fast-forward pushes succeed.

- [ ] **Step 5: Final report** — audit score of the new repo (`context-audit` checklist pass), junction table, and before/after `skills/` listing of Context-Engineering.
