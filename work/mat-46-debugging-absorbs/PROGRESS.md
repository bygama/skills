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

`skills/tracing-root-causes/` grew from a 55-line causal-analysis skill
into the house debugging skill:

- **SKILL.md** (188 → 191 lines): the iron law; the phased spine
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
| 3 | `grep -c "" …/SKILL.md` | `191` (<500) |
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

Filled by `work-verify` — see the section below.

## PR

_URL recorded below once `gh pr create` returns it._ Body carries
`Closes MAT-46`. Not merged by this lane; merging is the parent's action
after its reviewers pass.
