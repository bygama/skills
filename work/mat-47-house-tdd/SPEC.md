# SPEC — MAT-47 house TDD skill

Tier: M · Lane: `work/mat-47-house-tdd/` · Tracker: MAT-47

## Problem

TDD is the last big discipline this workstation borrows daily from the
superpowers plugin. Every session that implements anything reaches for
`superpowers:test-driven-development`, a skill written for superpowers'
own chain: it ends by claiming completion through its own checklist,
points at its own suite's finishers, and knows nothing about AE lanes.
Under AE that is a live collision — `work-verify` owns the completion
gate (ADR-005), and evidence belongs in the lane's PROGRESS.md, not in
a self-assessment inside a thinking skill.

After MAT-45 (shaping) and MAT-46 (tracing-root-causes) land, this is
the last daily-use borrow. Owning it means superpowers stays installed
only as the no-Orca fallback, with MAT-34's upstream-watch loop
comparing releases.

## Goal

A house TDD skill in `bygama/skills` that keeps everything that makes
the upstream skill work and hands the completion gate to AE.

**Keep** (the parts that do the work):

- The Iron Law — no production code without a failing test first.
- RED → GREEN → REFACTOR, with both verification beats mandatory
  (watch it fail; watch it pass).
- The rationalization table — the excuse/reality pairs that catch the
  agent mid-negotiation.
- The red-flag list — thoughts that mean stop and start over.
- The `writing-good-tests` reference: name the break, exercise the
  real thing, the gate functions, the mutation check.

**Adapt to AE surfaces:**

- Cycle evidence (the RED failure line, the GREEN pass line) lands in
  the lane's PROGRESS.md as it happens — that is what `work-verify`
  reads. At S tier, with no lane, the evidence is quoted in the reply.
- The skill never claims done. Its checklist is a self-check that ends
  by handing off to `work-verify`; the completion gate is not the
  thinking skill's to own (ADR-005).
- It produces no artifacts outside the lane — no scratch test plan, no
  separate ledger, no notes file of its own.

**YAGNI** — dropped because it only made sense inside the suite's
chain: the graphviz cycle diagram (renders nowhere, costs ~25 lines),
"ask your human partner" as the escape hatch for every judgment call
(a dispatched child reaches its owner through the lane and the
orchestration mailbox, not a chat partner), and the pointers into
superpowers' own debugging/finishing skills.

## Scope

In scope:

- `skills/<name>/SKILL.md` — the house TDD skill.
- `skills/<name>/references/writing-good-tests.md` — the adapted
  reference, one level deep.
- `skills/<name>/evals/eval-0N.md` — ≥3 evals, written and committed
  BEFORE the skill content, order provable in `git log`.
- `README.md` — the new skill's row in the Skills table, and the
  "Used alongside" line that still credits superpowers with TDD.
- This lane's four files.

Out of scope (explicit fences):

- `skills/tracing-root-causes/**` — MAT-46 is in flight on it.
- Anything in the Agent-Engineering repo — the supersession row in its
  `reference/skills.md` belongs to an AE sibling lane.
- `AGENTS.md` — it must NOT enumerate skills; the standard lints that
  as an anti-pattern. No edit needed there.
- Folding receiving-code-review's rule into work-run's fix loop — the
  ticket parks that behind MAT-43's templates.

## Acceptance

- `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  from the worktree root, with no finding attributable to this lane.
  (The AGENTS.md-cited `../Agent-Engineering` sibling does not resolve
  from an Orca worktree — see PROGRESS for the baseline and the
  owner's ruling on the exit-0 wording.)
- CI `standard` check green on the PR.
- `git log --follow` shows every eval file committed before the first
  commit that adds SKILL.md content.
- SKILL.md <500 lines, frontmatter `description` third-person with
  what + when, references exactly one level deep.
- README lists the skill; AGENTS.md still does not enumerate skills.
- PR open against main with `Closes MAT-47`, never merged by this lane.
