# PROGRESS — MAT-46

Lane: `work/mat-46-debugging-absorbs/` · Tier M · Branch
`bygama/mat-46-debugging-absorbs`

Spec: [SPEC.md](SPEC.md) · Plan: [PLAN.md](PLAN.md) · Rulings:
[DECISIONS.md](DECISIONS.md)

## Status

| Step | What | State |
|---|---|---|
| 0 | Lane opened (SPEC + PLAN + DECISIONS), SPEC approved by parent | done |
| 1 | Four evals grading the absorbed machinery | done — `28a5fc2` |
| 2 | `references/techniques.md` | done — `613fc6f` |
| 3 | SKILL.md absorbed rewrite | done — `089b9d4` |
| 4 | Self-grade the seven evals | done — one gap, fixed in `0aa5ac7` |
| 5 | Evidence + fenced-file report | done — this file |
| 6 | Push + PR | done — see *PR* below |

Executed inline rather than by dispatched subagents, under work-run's own
runtime-neutral fallback (D5): same lane, same ceremony, every acceptance
command actually run.

## What shipped

`skills/tracing-root-causes/` grew from a 57-line causal-analysis
skill into the house debugging skill:

- **SKILL.md** (57 → 214 lines): the iron law; the phased spine
  `reproduce → isolate → hypothesize → disconfirm → fix`, with the
  original six-step discipline surviving intact as phases 3 and 4; the
  three-strikes architecture stop; one merged 14-row rationalization
  table; the output shape; and the AE handoff.
- **references/techniques.md** (139 lines, new): backward tracing,
  defense in depth, condition-based waiting, test-pollution bisection.
- **evals/eval-04..07.md** (new): pressure resistance and the absorbed
  machinery, landed before the content.

Identity preserved: `name: tracing-root-causes` unchanged, the OMC
provenance comment intact, the three original evals untouched.

## Evidence

### Commit order — evals before content (SPEC A3)

```
0aa5ac7 fix: scale the probe to the time available
        skills/tracing-root-causes/SKILL.md
089b9d4 feat: tracing-root-causes absorbs systematic-debugging
        skills/tracing-root-causes/SKILL.md
613fc6f docs: techniques reference for the absorbed debugging skill
        skills/tracing-root-causes/references/techniques.md
28a5fc2 test: evals grading the absorbed debugging machinery — before the content
        skills/tracing-root-causes/evals/eval-04.md
        skills/tracing-root-causes/evals/eval-05.md
        skills/tracing-root-causes/evals/eval-06.md
        skills/tracing-root-causes/evals/eval-07.md
a222c2b plan: open the MAT-46 lane
```

`git diff --stat 28a5fc2 HEAD -- skills/tracing-root-causes/evals/` →
empty: no eval was touched after it was written, so nothing was weakened
to make the content pass.

### Per-step acceptance

| Step | Command | Result |
|---|---|---|
| 1 | `ls skills/tracing-root-causes/evals/*.md \| wc -l` | `7` |
| 1 | `git show --stat --name-only HEAD` | only `evals/` paths |
| 1 | `git diff HEAD~1 HEAD -- …/SKILL.md` | empty (0 bytes) |
| 2 | `test -f …/references/techniques.md` | exit 0 |
| 2 | `grep -c "^## " …/techniques.md` | `4` |
| 2 | `head -20 …/techniques.md \| grep -q "^- \["` | exit 0 (TOC present, file is 139 lines) |
| 2 | `grep -E "\]\((\.\./)?references/" …/techniques.md` | exit 1 — no nested reference, one level deep |
| 3 | `grep -q "^name: tracing-root-causes$" …/SKILL.md` | exit 0 |
| 3 | `grep -c "" …/SKILL.md` | `191` (<500) — count at step 3; now 214 after the review fixes |
| 3 | `grep -q "work-verify" …/SKILL.md` | exit 0 |
| 3 | `grep -q "references/techniques.md" …/SKILL.md` | exit 0 |
| 3 | `grep "complements superpowers:systematic-debugging"` | exit 1 — delegation removed |
| 3 | description length | 719 chars (≤1024) |

### Eval self-grading (step 4)

Each eval's checklist read against the landed SKILL.md and
techniques.md. "Supported" means the skill text prescribes the behavior
the checklist grades — this is authorship review, not a model run.

| Eval | Grades | Verdict |
|---|---|---|
| eval-01 | intermittent CI failure: precise observation, ≥2 frames, evidence both ways, probe | supported — phases 1, 3, 4 + output shape |
| eval-02 | perf regression: measurement-as-hypothesis, evidence hierarchy, shortlist when blocked | supported — phase 3 frames name the measurement artifact explicitly |
| eval-03 | obvious-culprit trap: temporal proximity, framing pressure, alternatives kept | supported — hierarchy + table rows + judgment note on framing |
| eval-04 | iron law under emergency: mitigation vs fix, cross-system pattern match, no reluctant compliance | supported — iron law carve-outs + table rows 1 and 6 |
| eval-05 | thrashing: attempt counting, three strikes, sunk cost, condition-based waiting | supported — phase 4 ("the same one, retried louder"), three-strikes section, table |
| eval-06 | authority pressure: seniority as prior not evidence, competing readings, cheap probe | **gap found** — probe guidance was silent on cost; fixed in `0aa5ac7`, eval untouched |
| eval-07 | trace to source: backward trace, empty-is-not-absent, defense in depth, AE handoff | supported — techniques reference + *Where this ends* |

The eval-06 gap is the evals-first loop working in the intended
direction: the eval was written first, the content failed to meet it,
and the content changed.

### Lint (SPEC A1 — scoped per D2)

This worktree, after all lane commits:

```
agent-lint C:\Users\mateo\orca\workspaces\skills\mat-46-debugging-absorbs
  MEDIUM AGENTS.md:15  file not found: ../Agent-Engineering/scripts/agent-lint.mjs  [cmd-drift]
0 high, 1 medium, 0 low — FAIL
```

Clean `main` at `0e6faade99150f86d7b1789ff697755a93ddbb17` — the commit
this branch was cut from — extracted with `git archive main` to a
scratch directory and linted there, untouched by this lane:

```
agent-lint …\scratchpad\clean-main
  MEDIUM AGENTS.md:15  file not found: ../Agent-Engineering/scripts/agent-lint.mjs  [cmd-drift]
0 high, 1 medium, 0 low — FAIL
```

**Identical, byte for byte.** The finding is pre-existing and not
attributable to this lane: it fires because an orca worktree's parent
directory is the orca workspace, so the `../Agent-Engineering` sibling
path that AGENTS.md documents — correct in a normal checkout and in CI —
does not resolve here. `AGENTS.md` is fenced for this lane and the
parent ruled it stays unedited (D2); the parent is filing the check
itself as a separate upstream finding.

The lint was invoked by absolute path throughout, as the brief
instructed: `node
C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`

**Zero findings attributable to this lane.**

### CI (SPEC A2 — the authoritative gate)

See *PR* below. The `standard` workflow checks the repo out at `repo/`
and Agent-Engineering beside it, so the sibling path resolves and the
cmd-drift finding does not fire there.

## Reported, not applied — fenced files

This lane touched no fenced file. `git show --stat` across all commits
lists only `work/mat-46-debugging-absorbs/` and
`skills/tracing-root-causes/`.

**`README.md:21` needs the sibling lane's attention.** The current row
reads:

```
| [`tracing-root-causes/`](skills/tracing-root-causes/) | Disciplined causal analysis: competing hypotheses, evidence ranked by strength, active disconfirmation |
```

Accurate before this change, incomplete after it — the skill now owns
the fix loop as well. Suggested replacement, for whoever owns the index
this wave:

```
| [`tracing-root-causes/`](skills/tracing-root-causes/) | The house debugging skill: reproduce, isolate, hypothesize, disconfirm, fix — competing hypotheses, evidence ranked by strength, a fix at the source |
```

**AGENTS.md** needs nothing from this lane. Its `## Commands` entry is
correct as written (D2).

**Agent-Engineering** — untouched, as fenced. The supersession row in
its `reference/skills.md` belongs to the sibling AE lane; note that the
file's current text still reads *"TDD and systematic-debugging are
untouched"*, which this change makes stale.

## Verification

### 2026-08-19 — M DoD — PASS (review rung outstanding, owned by the parent)

- **L1 static:** `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  → exit 1, `0 high, 1 medium, 0 low`. The single MEDIUM is the
  pre-existing `AGENTS.md:15 [cmd-drift]`, reproduced identically on
  clean `main` (`0e6faad`) extracted in isolation — **zero findings
  attributable to this lane** (D2). The authoritative gate, CI
  `standard`, is green on the final tree: `gh pr checks 9` → `standard
  pass`, [run 32222154873](https://github.com/bygama/skills/actions/runs/32222154873),
  `"conclusion":"success"` — and green on **every** pushed head of this
  branch, latest run 32222154873 onward (`gh run list --branch
  bygama/mat-46-debugging-absorbs`). Stated that way deliberately: every
  commit to this file displaces whatever head the previous line named.
- **L2 behavioral:** the repo ships no test runner, so the behavioral
  layer is the artifacts loading and resolving as a skill:
  - frontmatter parses, exactly `name` + `description`, `name:
    tracing-root-causes`, description 719 chars ≤1024 → exit 0
  - every internal link resolves: 2 checked, 0 broken → exit 0
  - reference depth: `techniques.md` links no further reference → clean
  - all 4 TOC anchors match real headings → exit 0
  - repo invariants intact: every skill ≥3 evals (7/3/3/4), every
    SKILL.md <500 lines (214/67/69/87)
  - identity: `name` unchanged, OMC provenance intact, both trigger
    families present, `work-verify` + `PROGRESS.md` named (A6/A7/A8)
  - `git diff main..HEAD -- evals/eval-0{1,2,3}.md` empty — the three
    original evals are byte-identical to main (A4)
- **L3 end-to-end:** n/a — single component. The change is one skill
  directory; there is no cross-component flow to execute.
- **Fresh-context review: PASS** — in-session subagent, no shared
  conversation, ran the DoD itself (D6). It independently extracted
  clean `main` and reproduced the `cmd-drift` MEDIUM byte-identically,
  confirming rather than accepting D2's claim, and confirmed CI green on
  the exact reviewed head via `gh run view 32221538508` →
  `"conclusion":"success"`. On substance it judged the absorption to
  keep the source's machinery rather than summarize it, and the causal
  core preserved with additions only. Findings: 0 Critical, 2 Important
  (both fixed in `b331654`), 4 Minor (triaged below).
- **Adversarial review:** deferred to the parent — 1 ballena,
  cross-model, dispatched after `worker_done` per the dispatch config.
  It is an additional seat, not a replacement for the rung above.

**Scoped re-review: NO VERDICT RETURNED.** The same seat was asked three
times to verdict the fix diff and signalled idle each time without
answering (D7). The PASS above stands on its own terms — it was given on
`c3e4038`, before the fixes, with both Important findings called
*"additive gaps in the absorbed text, not defects in what shipped"* —
and every layer the reviewer ran was re-run here after the fixes. But no
second pair of eyes confirmed the *closures*; that check is open and
flagged for the parent's adversarial seat.

#### Review findings and disposition

| # | Sev | Finding | Disposition |
|---|---|---|---|
| 1 | Important | the "I don't know" machinery (superpowers Phase 3.4) did not survive | **fixed** `b331654` — phase 3 bullet |
| 2 | Important | the honest-environmental terminal state kept the disbelief, lost the procedure | **fixed** `b331654` — new section |
| 3 | Minor | dependency sweep dropped from Isolate | **fixed** `b331654` |
| 4 | Minor | no eval ever executed against a model | **accepted, standing gap** — see below |
| 5 | Minor | PROGRESS quoted three different baselines for one line count | **fixed** — true baseline is 57 (`git show main:…SKILL.md \| grep -c ""`), not 55/188 |
| 6 | Minor | README and AE `reference/skills.md` go stale on merge | **not a lane defect** — both fenced and reported; merge-order dependency, see below |

**Standing gap (finding 4), stated plainly:** no eval in this repo has
been executed against a model, here or before this lane. The repo ships
no eval runner, so "L2 pass" means *the skill text prescribes the graded
behavior*, verified by reading — not *a fresh session was graded and
passed*. That is a property of the repo's eval method, not of this
change, and it is the honest ceiling on what this PASS claims.

Every command above was run in this session against this tree.

## PR

https://github.com/bygama/skills/pull/9 — body carries `Closes MAT-46`.
Not merged by this lane; merging is the parent's action after its
reviewers pass and after any requested rebase onto fresh main.

## Handoff — PAUSED, not closed

`work-handoff` ran in **pause** mode on 2026-08-19. The lane folder
survives deliberately: a close would remove it, and the parent's review
wave may return findings that need this lane's SPEC, PLAN, and rulings
intact to act on.

**Why pause rather than close.** Close needs a current PASS covering the
whole DoD. Two things are outstanding, and neither is mine to complete:

1. The cross-model adversarial review is the parent's (1 ballena,
   dispatched after `worker_done`). This lane's own fresh-context rung
   DID run and returned PASS (D6) — the outstanding seat is the
   additional cross-model one, not step 4.
2. The PR is open, not merged. Merging is the parent's action, after its
   reviewers pass and after any requested rebase onto fresh main.

**Merge-order dependency the parent now owns** (review finding 6): if
this lane merges and the sibling lanes do not, `main` carries a
`README.md` index row describing half the skill, and Agent-Engineering's
`reference/skills.md:126` still asserts *"TDD and systematic-debugging
are untouched"*. Both files are fenced from this lane by ruling; both
strings were confirmed to still exist at review time.

**Nothing is red.** Every command in `## Verification` exited as
recorded, the worktree is clean (`git status --porcelain` empty), and CI
`standard` is green on the final tree. This is a pause on *authority*,
not on a blocker.

**Next, for whoever resumes:**

- Await the parent's review verdict; fix findings on this branch and
  re-run the L1/L2 commands in `## Verification` before re-reporting.
- Rebase onto fresh `main` when the parent asks (rebase-only house
  rule). Do not merge from this seat.
- The README index row in *Reported, not applied* is the sibling lane's
  to apply — do not apply it here.
- On merge, `Closes MAT-46` in the PR body moves the Linear issue via
  the GitHub integration; no manual status move was made from this lane.

**Debris sweep:** worktree clean, no stray logs/scratch/`.orig` files,
no placeholders. The two `TODO` matches in `eval-05.md` are quoted
scenario text (the eval grades *refusing* to ship a TODO'd stopgap), not
debris. The `git archive` extraction used for the clean-main lint proof
lived in the session scratchpad, outside the repo, and was removed.
