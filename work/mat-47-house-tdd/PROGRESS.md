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

## Verification

### 2026-08-19 — M DoD — PASS

- **L1 static:** `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  → exit 1, `0 high, 1 medium, 0 low — FAIL`, sole finding
  `AGENTS.md:15 ... [cmd-drift]` — byte-identical to the clean-main
  baseline above, so zero findings are attributable to this lane (D-002).
- **L2 structural:** `node <scratch>/verify.mjs` → exit 0, 39/39 checks.
  Frontmatter parses, name matches directory, description 711 chars and
  third person, SKILL.md 249 lines < 500, all six KEPT elements present,
  all four ADAPTED assertions, all three DROPPED assertions, the single
  relative link resolves one level deep, the reference adds no second
  hop and opens with a table of contents at 220 lines, four evals each
  with Query + Expected behavior + ≥3 checklist items, README lists the
  skill, AGENTS.md does not.
  *Named structural, not behavioral, per the reviewer's Minor 1: it is a
  static string-matcher and several assertions grep for phrases written
  in the same session. It guards wording regressions; it does not prove
  agent behavior.*
- **L3 end-to-end:** n/a — the change is one skill directory plus its
  README row. The skill's real behavioral contract is its evals, and
  grading those is what the fresh-context review and the probe below do.
- **Ordering (repo hard constraint):**
  `git log --oneline --diff-filter=A --reverse -- skills/testing-first`
  → `5b0ac3a test(testing-first): evals before any skill content` first,
  then `d752693` (reference), then `d02c9cc` (SKILL.md). The reviewer
  confirmed by `git show --stat` that 5b0ac3a adds only the four eval
  files.
- **Fences:** `git diff --name-only main..HEAD` → no
  `skills/tracing-root-causes/**`, no `AGENTS.md`, nothing outside the
  repo.
- **Fresh-context review:** PASS. Reviewer ran every layer itself,
  including reproducing the lint baseline independently from its own
  `git archive origin/main` export. No Critical findings; four Important,
  all fixed this session (triage below).
- **Adversarial review:** n/a at M from this lane — the cross-model seat
  is the parent's, dispatched after `worker_done` (D-004).

### CI — the authoritative gate

`gh pr checks 10` → `standard  pass  5s`
(https://github.com/bygama/skills/actions/runs/32222589054). In CI the
Agent-Engineering sibling checkout exists, so agent-lint resolves
AGENTS.md's cited command and the repo lints clean — which is the direct
confirmation that the local finding is a worktree artifact and not a
defect in the tree (D-002).

PR: https://github.com/bygama/skills/pull/10 — carries `Closes MAT-47`.
Not merged by this lane, by design: the parent merges after its own
adversarial review.

### Reviewer findings — triage

| # | Finding | Disposition |
|---|---|---|
| Important 1 | Step log still said `pending` while `git log` showed all steps shipped | Fixed — step log below now matches history, and this block is the evidence it lacked |
| Important 2 | Evals claimed an "Observed baseline" that was never observed | Fixed — one real probe run, blocks relabelled to daily-use friction, probe result recorded including that it contradicted the prediction (D-005) |
| Important 3 | The port's headline adaptation — the PROGRESS evidence format — was described in prose and never shown; upstream's bugfix example was the one worth keeping | Fixed — `SKILL.md` now carries a worked bugfix example modelling the exact RED/GREEN evidence lines |
| Important 4 | Attribution: the reference is a substantial MIT derivative and README's Provenance did not name superpowers | Fixed — README Provenance now names superpowers v6.3.0, MIT © 2025 Jesse Vincent |
| Minor 1 | The harness proves less than it looks | Accepted — layer relabelled structural; harness hardened (frontmatter-parse guard, first-person check widened past the first token with quoted triggers excluded, TOC check now asserts >100 lines) |
| Minor 2 | PLAN step 2 mislabels eval-03 as testing an adapted behavior | Accepted, PLAN left as written — a plan is a record of what was planned, not edited after the fact. Correct reading: 01 and 02 are strongly adapted; 03 and 04 restate upstream with one adapted hook each |
| Minor 3 | eval-04 had no provenance block, breaking the shape | Fixed |
| Minor 5 | The description did not surface the skill for skill/eval authoring, which the body claims jurisdiction over | Fixed — trigger added to the description |
| Minor 4, 8 | The reference is nearer to copy-paste than SKILL.md; the RED example is verbatim upstream | Accepted — attribution now carries it (Important 4). Almost no AE surface touches that material, so rewriting for its own sake would cost clarity |
| Minor 6 | No `~/.claude/skills/testing-first` junction exists yet | Handoff note, not a lane defect — per AGENTS.md a new skill needs one workstation-installer run. Nothing has loaded this skill for real yet |
| Minor 7 | Agent-Engineering's `reference/skills.md` still says TDD is untouched | Out of scope by fence — an AE sibling lane owns that row. The two repos disagree until it lands |

### One thing worth stating plainly

L2 first reported `1 FAILED`, on the assertion that the skill never flips
a `feature_list.json` row. That was my checker's regex failing to span a
line wrap, not a content defect — confirmed by `grep` at SKILL.md:153-155
**before** the regex was touched. Recorded because "fixed the test until
it passed" is exactly the failure this skill exists to prevent, and a
lane about TDD does not get to do it silently.

The same care applies to the second harness FAIL, on the hardened
first-person check: the description contains `"I already manually tested
it"` as a quoted trigger phrase. Verified as a genuine false positive —
the skill's own voice is third person throughout — then the check was
narrowed to exclude quoted spans.

## Step log

| Step | State | Evidence |
|---|---|---|
| 1. Lane opened | done | `f7ebb80` — SPEC, PLAN, DECISIONS, PROGRESS; touches nothing under `skills/` |
| 2. Evals (≥3) | done | `5b0ac3a` — four eval files, and nothing else, as the first commit under `skills/testing-first` |
| 3. `references/writing-good-tests.md` | done | `d752693` — 220 lines, TOC, no second hop; landed before SKILL.md so no commit carried a dangling link |
| 4. `SKILL.md` | done | `d02c9cc` — 249 lines after the review fixes, one relative link, one level deep |
| 5. README index surfaces | done | `cf5ff43` — Skills-table row added, TDD dropped from the superpowers credit; `grep -c` → README 1, AGENTS.md 0 |
| 6. Review fixes | done | this commit — Important 1-4 and Minors 1, 3, 5 |

## Handoff notes

- The `~/.claude/skills/testing-first` junction does not exist yet: one
  workstation-installer run is needed before this skill loads anywhere.
- ~~README's "used alongside" line is also MAT-46's target (for
  debugging), so expect one rebase conflict there.~~ **Prediction was
  wrong** — see the rebase note below.
- A properly isolated baseline run for the evals — AE skills absent — is
  the honest follow-up this lane did not do (D-005).
- **Open for the owner, deliberately not decided here:** the repo's root
  `LICENSE` is MIT © 2026 Mateo García with no mention of the upstream
  copyright. This lane added the MIT notice (© 2025 Jesse Vincent) to the
  header of both derived files and a Provenance line to README, which is
  the file-level convention. Whether the root LICENSE should also carry
  the upstream notice is a repo-wide call, not a lane's.

### Fix round 1 — re-review

Scoped re-review of the fix diff: all four Important findings
**ADDRESSED**, round verdict **PASS**. It raised one wording
inconsistency (eval-02's Provenance lacked the "not a controlled baseline
run" qualifier its siblings carry) — fixed — and the LICENSE observation
above, logged rather than acted on.

## 2026-08-19 — rebase onto merged main (cf47cc3)

MAT-46 merged and main advanced. Rebased; **zero conflicts**, because the
predicted collision never existed: `git diff --name-only 0e6faad..origin/main`
shows MAT-46 touched only `skills/tracing-root-causes/**` and its own lane
folder. It never edited README.md.

That left a semantic problem git could not have flagged, and it is what
the rebase actually had to fix — both README lines were wrong on the
merged tree:

- **The superpowers credit.** It read "debugging and collaboration
  workflows" after this lane removed TDD. But MAT-46's skill now declares
  it "Supersedes superpowers:systematic-debugging as the house debugging
  skill", so crediting superpowers with debugging clobbers exactly what
  the sibling lane shipped. Now reads "collaboration workflows".
- **The tracing-root-causes row.** Still "Disciplined causal analysis:
  competing hypotheses, evidence ranked by strength, active
  disconfirmation" — the pre-absorption description, understating a skill
  that grew by 245 lines, four evals and a references file. Now reads
  "Owns debugging end to end: reproduce, isolate, competing hypotheses,
  evidence ranked by strength, active disconfirmation, fix at the source",
  mirroring its own frontmatter.

The second edit touches a row for a sibling's skill. It is index-surface
work — README, this lane's surface this wave — not a change under
`skills/tracing-root-causes/**`, which stays untouched.
