# DECISIONS — MAT-46

Rulings and judgment calls for this lane. Owner rulings arrived through
the parent orchestrator (`orca orchestration ask`); they are recorded
verbatim in substance, not re-litigated.

## 2026-08-19 — D1: SPEC approved, absorption shape confirmed

**Ruling (parent).** SPEC approved as written; proceed to PLAN.md.

The parent confirmed the load-bearing part explicitly: growing the
frontmatter `description` to carry `systematic-debugging`'s discovery
triggers is not cosmetic — *"a house skill that does not win discovery
loses to the suite by default."* Name and identity survive; the phased
spine grows **around** the causal-analysis discipline rather than
replacing it; one merged rationalization table; one techniques
reference.

YAGNI casualties approved: `condition-based-waiting-example.ts`,
`find-polluter.sh` as a shipped executable, the Graphviz `digraph`
blocks, `CREATION-LOG.md`. Seven evals committed before content:
correct.

## 2026-08-19 — D2: the lint gate is scoped, not absolute

**Context.** The dispatch brief set the gate as `agent-lint` exit 0 from
the worktree root. Baseline exits **1** before any edit of this lane:

```
MEDIUM AGENTS.md:15  file not found: ../Agent-Engineering/scripts/agent-lint.mjs  [cmd-drift]
0 high, 1 medium, 0 low — FAIL
```

The sibling path `../Agent-Engineering` resolves in a normal checkout and
in CI (the `standard` workflow checks the repo out at `repo/` and
Agent-Engineering beside it), but not from an orca worktree, whose
parent directory is the orca workspace. Invoking the lint by absolute
path does not clear the finding — the finding is about the path
**AGENTS.md cites**, not the path used to run the linter. `AGENTS.md` is
fenced for this lane.

Three options were put to the parent: (a) create a machine-local
junction at `C:/Users/mateo/orca/workspaces/skills/Agent-Engineering`,
(b) scope the gate to findings attributable to this lane plus CI green,
(c) unfence AGENTS.md.

**Ruling (parent): (b), and explicitly never (a).**

- **(a) rejected** — *"a machine-local junction is invisible environment
  state that no adopter and no fresh clone reproduces, and the
  standard's evidence discipline says record reality rather than paper
  over it."*
- **(c) rejected** — *"AGENTS.md's cited command is CORRECT in the
  checkout and in CI, so editing it would break the real gate to satisfy
  a worktree artifact."*

**Accepted acceptance for this lane:**

1. Zero lint findings **attributable to this lane**.
2. The repo's CI `standard` check green on the PR — *"that check is the
   authoritative gate."*
3. The pre-existing `AGENTS.md:15` cmd-drift proved pre-existing by
   running the lint against clean `main`, and recorded verbatim in
   PROGRESS.md.

The parent is filing the cmd-drift check itself as a separate upstream
finding (the check flags a cross-repo sibling path that is legitimately
correct in its documented context). **Not fixed here.**

## 2026-08-19 — D3: techniques ship as one reference, not four

**Call (lane, within approved scope).** `systematic-debugging` ships
four technique artifacts in its skill directory. AE's authoring rules
require references exactly one level deep from SKILL.md, and the house
pattern (`skills/reviewing-plans/references/gap-taxonomy.md`) is a
single reference file per skill.

Four thin files would each cost a frontmatter-to-content round trip for
~30 lines of technique. One `references/techniques.md` with a table of
contents keeps the rule satisfied, keeps SKILL.md lean, and lets the
`find-polluter.sh` technique survive as instructions rather than as an
`npm test`-bound shell script.

## 2026-08-19 — D4: the README index row is reported, not edited

**Call (fence compliance).** `README.md:21` describes the skill as
*"Disciplined causal analysis: competing hypotheses, evidence ranked by
strength, active disconfirmation"* — accurate before the absorption,
incomplete after it (no mention of the fix loop the skill now owns). A
sibling lane owns the index this wave, so the needed wording is
**reported in PROGRESS.md** and left unapplied here.

## 2026-08-19 — D5: work-run executes inline, not by subagent

**Call (runtime constraint).** The dispatch brief fences this lane from
spawning workers of its own ("No grandchildren"), and this runner is
configured not to call the Agent tool unless requested. work-run's own
qualify step covers exactly this case: *"No subagent capability on this
runner: work-run is never mandatory (the standard is runtime-neutral).
Fall back to executing the SAME lane inline under the SAME ceremony:
PLAN steps in order, acceptance per step, PROGRESS updated. Never
simulate a dispatch."*

So: inline execution, ceremony unchanged — each step's acceptance
command is actually run and its output recorded in PROGRESS. No
dispatch is simulated, and no step is marked done without its command
having exited.
