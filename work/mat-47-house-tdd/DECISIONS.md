# DECISIONS — MAT-47 house TDD skill

## D-001 — Skill directory is `skills/testing-first/`, not `test-driven-development`

**Decision.** The ported skill lives at `skills/testing-first/`. Proposed
by this lane, approved by the parent (dispatch ask, 2026-08-19).

**Why.**

1. *Namespace collision, permanent.* This repo's skills are junction-linked
   into `~/.claude/skills` under bare names, while plugin skills keep their
   `superpowers:` prefix — both appear in the same picker. MAT-47's own
   ticket keeps superpowers installed as the no-Orca fallback, so a house
   `test-driven-development` would sit next to
   `superpowers:test-driven-development` forever. Two identical names in
   one picker is a discovery defect, and it would be shipped on purpose.
2. *House naming convention.* Every skill here is gerund/action-oriented —
   `reviewing-plans`, `tracing-root-causes`, `designing-consistently`,
   `extracting-design-md` — and AE's authoring reference requires it
   (`reference/skills.md`, Frontmatter: "Gerund or action-oriented").
   `test-driven-development` is a noun phrase naming a movement.
3. *It names the rule, not the movement.* The Iron Law is the skill's
   whole content; `testing-first` states it.

**Cost accepted.** The universally recognized term is not the directory
name. Discovery does not depend on it: the frontmatter `description`
carries "TDD" and "test-driven development" as trigger terms, and the
model picks skills by description alone.

## D-002 — Lint acceptance is "no findings attributable to this lane", not exit 0

**Ruling.** Parent, via blocking ask (2026-08-19). Option B; option A
(a machine-local junction) explicitly refused.

**The finding.** From an Orca worktree root, agent-lint exits 1 with one
MEDIUM: `AGENTS.md:15 file not found: ../Agent-Engineering/scripts/agent-lint.mjs
[cmd-drift]`. The lint resolves a cited `node <path>` command relative to
the linted root; from
`C:/Users/mateo/orca/workspaces/skills/mat-47-house-tdd` that sibling is
`workspaces/skills/Agent-Engineering`, which does not exist — the repo is
at `C:/Briar/repos/mine/Agent-Engineering`, and `workspaces/Agent-Engineering`
holds AE lane worktrees, not the repo. Invoking lint through the absolute
path does not clear it: the finding is about AGENTS.md's content, not the
invocation. It reproduces on clean main; this lane did not cause it.

**Rejected — edit AGENTS.md to an absolute path.** Refused by the parent.
The cited command is correct in the owner's checkout and in CI, which
checks Agent-Engineering out at the sibling position on purpose
(commit 0e6faad). An absolute, machine-specific path would break both.

**Rejected — create a junction `workspaces/skills/Agent-Engineering`.**
Refused by the parent: invisible environment state that no fresh clone or
adopter reproduces, shared with the in-flight sibling lane, and Orca may
treat the folder as a worktree. Record reality instead.

**Accepted.** Acceptance for this lane is: zero findings attributable to
this lane (summary line stays `0 high, 1 medium, 0 low — FAIL`, sole
finding stays the AGENTS.md:15 cmd-drift), plus the CI `standard` check
green on the PR as the authoritative gate. The parent is filing the
worktree false-positive as its own upstream ticket.

## D-003 — What was dropped from the upstream skill, and why

- **The graphviz cycle diagram** (~25 lines of `dot` in a fenced block).
  It renders nowhere in an agent's context and the same cycle is stated in
  prose one paragraph later. Token cost without non-inferable content —
  AE's authoring rule ("challenge every paragraph") kills it.
- **"Ask your human partner" as the universal escape hatch.** The upstream
  skill routes every judgment call there. Under AE the owner is reached
  through the lane and, for a dispatched child, the orchestration mailbox;
  a child session has no chat partner to ask. Replaced with the AE route
  where the escape hatch is genuinely needed (the TDD exceptions), dropped
  where it was decoration.
- **Pointers into superpowers' own debugging and finishing skills.** The
  house equivalents own those phases: `tracing-root-causes` for the
  diagnosis (MAT-46), `work-verify`/`work-handoff` for the ending
  (ADR-005). Named in prose without a relative link, so this file does not
  break if the sibling lane reshapes its own skill.
- **The "Verification Checklist / before marking work complete".** This is
  the port's whole point: `work-verify` owns the completion gate. The
  checklist survives as a cycle-local self-check that ends by handing off,
  never by declaring done.

## D-004 — work-run's steps ran inline; the review rungs ran as subagents

**Superseded reasoning, recorded because it was acted on.** This lane
first read the dispatch's "no grandchildren" rule as fencing off *all*
subagents, and executed work-run's steps 2-5 inline under its
runtime-neutral fallback (same PLAN steps, same per-step acceptance)
rather than with a fresh implementer and reviewer per step.

**The owner's correction** (blocking ask, 2026-08-19): "no grandchildren"
forbids spawning **orchestration workers** — a Task, a Dispatch,
`worker_done` authority. It does not forbid in-session subagents in this
worktree, which is what work-run's per-step reviewers and work-verify's
step-4 fresh-context review are. Sibling lanes in this wave ran theirs
that way. The owner is filing the template ambiguity as its own ticket.

**What actually happened, therefore.** Steps 2-5 were built inline —
already committed by the time the correction landed, and not rebuilt,
since re-running them through dispatch would change no artifact. The
review rungs did run as subagents:

- work-verify step 4, the fresh-context review: dispatched with the lane
  path, the diff range, and the DoD, and nothing of this session's
  reasoning. Verdict and findings in PROGRESS.md.
- A baseline probe for eval-02 (see D-005).

**What this lane may not claim.** No per-step reviewer ran, so steps 2-5
carry acceptance evidence but no independent per-step verdict. The
whole-diff fresh-context review covers the lane; the cross-model
adversarial seat is the parent's, dispatched after `worker_done`.

## D-005 — the eval baselines are daily-use friction, not measured runs

**The finding.** The fresh-context reviewer caught each eval asserting an
"Observed baseline" while no baseline had been observed. AE's authoring
reference requires evals be written from tasks run *without* the skill,
with the concrete failures documented; this repo's README goes further
and claims baseline subagent runs are observed before content. Neither
happened here — the fixtures were written from the friction MAT-47
itself records from daily use.

**What was done.** One real probe, for eval-02 (the load-bearing adapted
behavior): a fresh agent, given the upstream skill as its methodology
and eval-02's fixture, asked to "mark it done and close the lane out".

**It did not confirm the prediction.** The agent did not self-claim done.
It invoked `work-verify` on its own, ran the acceptance command, got exit
127, and left the lane open with a `## Tried and failed` entry — the
behavior eval-02 was written to demand.

**Why the probe proves less than it looks.** It was confounded: the AE
skills were installed and discoverable in that environment, and the
fixture's empty `## Verification` section cued the agent toward them. It
isolates nothing about the upstream skill's behavior where AE is absent.

**Ruling.** Every "Observed baseline" block is relabelled to what it
actually is — friction recorded from daily use, sourced to MAT-47 — and
eval-02 additionally carries the probe result verbatim, including that
it contradicted the prediction. The evals stand as **regression guards**
with unmeasured discriminating power, and say so. A properly isolated
baseline run (AE skills absent) is the honest follow-up, and is not in
this lane's scope.

The alternative — quietly deleting the word "observed" — was rejected.
A skill about not claiming what you did not verify cannot ship evals
that claim what was not measured.
