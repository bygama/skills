# SPEC — tracing-root-causes absorbs systematic-debugging

Ticket: [MAT-46](https://linear.app/bygama/issue/MAT-46/feat-tracing-root-causes-absorbs-systematic-debugging-one-house)
· Tier: M · Lane: `work/mat-46-debugging-absorbs/`

## Problem

Two skills cover the same genre with the same lineage. The house library
ships `skills/tracing-root-causes` (causal explanation: competing
hypotheses, evidence strength, disconfirmation); the superpowers plugin
ships `systematic-debugging` (the fix loop: four phases, iron law,
rationalization tables), whose own `root-cause-tracing.md` reference is
the ancestor of the house skill's name.

The split is not clean, it is a seam. The house skill's own closing note
delegates the fix loop outward — `"This complements
superpowers:systematic-debugging: that skill governs the fix loop; this
one governs the causal explanation that precedes it."` So the agent that
hits a bug has to pick between two skills that each own half a loop, and
the frontmatter `description` — the only discovery surface — makes the
house skill invisible for the plain trigger `"fix this bug"`.

## Outcome

One house debugging skill. `tracing-root-causes` keeps its name, its
identity, and its causal-analysis core, and absorbs the machinery worth
keeping from `systematic-debugging`, so that a single skill carries a
bug from symptom to verified fix. When it ships, `systematic-debugging`
joins the not-used list; superpowers stays installed as the
no-AE-environment fallback.

This is **absorption, not replacement**: the surviving artifact is the
existing skill grown, not a new skill wearing an old name.

## Scope

### In

1. **The phased spine.** `reproduce → isolate → hypothesize →
   disconfirm → fix`, replacing the current flat six-step discipline
   list. The causal-analysis steps (competing hypotheses, evidence
   ranked by strength, disconfirmation, rebuttal round, discriminating
   probe) survive intact as the `hypothesize` and `disconfirm` phases —
   nothing of the existing discipline is dropped to make room.
2. **The iron law.** `NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST`,
   plus the 3-strikes rule: after three failed fixes the problem is the
   architecture, not the hypothesis — stop and say so.
3. **One pressure-tested rationalization table.** The strongest rows of
   `systematic-debugging`'s *Common Rationalizations* and *Red Flags*,
   merged with the causal-analysis failure modes already implied by the
   existing skill (temporal proximity as evidence, fake convergence,
   confirming the user's framing).
4. **Techniques that survive YAGNI**, distilled into ONE reference file
   one level deep (`references/techniques.md`, with a table of
   contents): backward tracing to the source, defense-in-depth
   validation, condition-based waiting, test-pollution bisection.
5. **The discovery interface.** The frontmatter `description` grows to
   carry `systematic-debugging`'s triggers (any bug, test failure, or
   unexpected behavior, before proposing fixes) alongside the causal-
   analysis ones it already has. Third person, ≤1024 chars.
6. **Evals grading the absorbed machinery**, added BEFORE the content
   lands: pressure resistance (emergency, sunk cost, authority),
   trace-to-source with defense-in-depth, and the AE handoff.

### Out — deliberately not absorbed

- `condition-based-waiting-example.ts` (158 lines of TypeScript). The
  pattern is ~10 lines of pseudocode; the example is a language-bound
  artifact that a Markdown reference cannot verify and the repo cannot
  test. The technique survives; the file does not.
- `find-polluter.sh` as a shipped executable. It hard-codes `npm test`
  and a `find`-based glob; the *technique* (bisect test files one at a
  time until the pollution appears) is three lines of instruction that
  work for any runner. Technique in, script out.
- `CREATION-LOG.md` and the `test-*.md` pressure files as-is — the
  scenarios are absorbed, translated into this repo's eval format
  (`## Query` + `## Expected behavior` checklist).
- The Graphviz `digraph` blocks. Decorative token cost, no information
  the surrounding prose lacks.

### Fenced — this lane touches none of these

- The repo's `README.md` and `AGENTS.md` (a sibling lane owns the index
  this wave; needed mentions are reported in PROGRESS.md, not applied).
- Any other skill's directory under `skills/`.
- Anything in the Agent-Engineering repo — the supersession row in its
  `reference/skills.md` belongs to a sibling AE lane.

## AE integration — preserved, not re-litigated

The absorbed fix loop does not import superpowers' ending. Specifically:

- Evidence lands in the lane's `PROGRESS.md`, never in a suite-default
  folder or a standalone debugging log.
- `work-verify` owns the completion gate. The skill hands off there and
  never claims done itself — "the test passes now" is a handoff, not a
  verdict.
- `work-handoff` closes the lane. The skill's fix phase ends at the
  handoff boundary.

## Constraints

- **Evals-first, provable in history.** The eval commit precedes the
  content commit; `git log` is the proof, not a claim in PROGRESS.
- **AE authoring rules** (`reference/skills.md`): SKILL.md <500 lines,
  references exactly one level deep, reference files >100 lines open
  with a TOC, third-person description, forward slashes in paths.
- **Absorption, not replacement.** `name:` stays `tracing-root-causes`;
  the OMC provenance comment stays; no existing eval is weakened.

## Acceptance

| # | Criterion | Proof |
|---|---|---|
| A1 | Lint clean | `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` reports no finding attributable to this lane |
| A2 | CI green | the repo's `standard` check passes on the PR |
| A3 | Evals-first | `git log --stat` shows the eval commit strictly before the content commit |
| A4 | ≥3 evals | `ls skills/tracing-root-causes/evals/` — 7 files, the 3 existing ones unmodified in substance |
| A5 | One level deep | `references/techniques.md` links no further reference file |
| A6 | Identity kept | `name: tracing-root-causes` unchanged in frontmatter |
| A7 | Discovery | the `description` carries both trigger families (causal analysis AND bug/test-failure/unexpected-behavior before fixes) |
| A8 | AE handoff | SKILL.md names `work-verify` as the completion gate and `PROGRESS.md` as where evidence lands |

## Open question carried to the owner

Baseline `agent-lint` in this worktree already exits 1 on a pre-existing
`[cmd-drift]` MEDIUM: `AGENTS.md:15  file not found:
../Agent-Engineering/scripts/agent-lint.mjs`. The sibling path the
repo's AGENTS.md documents does not exist from an orca worktree, and
`AGENTS.md` is fenced for this lane. Exit 0 is therefore unreachable
here without either editing a fenced file or creating an out-of-repo
junction. Ruling requested — recorded in DECISIONS.md.
