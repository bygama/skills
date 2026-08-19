# PLAN — MAT-93

Lane: `work/mat-93-ask-for-help-leg/` · Tier M · SPEC approved (D3)

## Constraints (every step)

- Evals change BEFORE skill content (repo hard constraint): eval-08
  lands in a commit strictly before any SKILL.md edit.
- Only `skills/tracing-root-causes/` and this lane's `work/` files are
  touched. README.md, AGENTS.md, CODE_OF_CONDUCT.md and all other
  skills are fenced.
- SKILL.md stays <500 lines; references one level deep; the
  frontmatter description is not changed (discovery already won —
  MAT-46; the leg is content, not a trigger).
- The leg says WHEN and WHAT, names WHO/CHANNEL runtime-neutrally, and
  never spells AE flag/verb syntax (D3: "AE's channels say HOW to ask,
  your leg says WHEN").
- Lint acceptance per D2: `node
  C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` PASS
  (exit 0), no findings attributable to this lane.
- All artifacts in English.

## Steps

1. **[judgment] Baseline probe — observe the gap before writing to it.**
   Dispatch ONE fresh in-session subagent (no conversation history)
   with: the current `skills/tracing-root-causes/SKILL.md` as its
   methodology, a debugging fixture whose critical unknown is a fact
   outside its reach (it must name it but cannot produce it), and
   pressure to close the investigation. No hint of an ask-for-help
   route. Record the observed behavior — guessed fix, silent stop, or
   unprompted escalation — verbatim in a `## Baseline probe` section
   of PROGRESS.md, honestly labeled (MAT-47 D-005 is the bar; a probe
   that contradicts the prediction is recorded as exactly that).
   *Acceptance:* `grep -q "## Baseline probe"
   work/mat-93-ask-for-help-leg/PROGRESS.md` → exit 0.
   *Commit:* `docs(lane): baseline probe — behavior without the leg`.

2. **[judgment] eval-08 — the failing test, before content.** Write
   `skills/tracing-root-causes/evals/eval-08.md` in the house eval
   format (`## Query` verbatim + `## Expected behavior` objective
   checklist), built from step 1's `## Baseline probe` section. The
   checklist must demand: no silent guess AND no silent stop at the
   stall; escalation to the owner (parent when dispatched) through a
   blocking/interactive question; the ask carrying the evidence
   package (hypotheses tried, disconfirming evidence per hypothesis,
   exact stall point, the probe that would run given the answer); and
   the negative guard — no premature ask while an affordable
   discriminating probe remains runnable from the seat.
   *Acceptance:* `test -f skills/tracing-root-causes/evals/eval-08.md`
   → exit 0, AND lint PASS per constraints, AND `git diff --name-only
   HEAD` shows no SKILL.md change in the same commit.
   *Commit:* `test(tracing-root-causes): eval-08 — the stall
   escalation, before content`.

3. **[judgment] The leg lands in SKILL.md.** Add one short section
   (working title: "When the investigation stalls") carrying the four
   approved beats — WHEN (reachable evidence exhausted; the phase-4
   critical unknown is a fact the seat cannot produce; an affordable
   probe still on the table means run it, not ask), WHO (the owner;
   the parent when dispatched), CHANNEL (a blocking ask or an
   interactive question — never a silent guess, never a silent stop),
   WHAT (the evidence package as the price of asking). Coherence
   touches only: the Output-shape line "Blocked by missing evidence?"
   gains its recipient; the provenance HTML comment gains a MAT-93
   line (the surface MAT-34's upstream-watch reads). Target: eval-08's
   checklist is satisfiable by the new text.
   *Acceptance:* `wc -l skills/tracing-root-causes/SKILL.md` < 500,
   AND lint PASS per constraints, AND `grep -qi "stall"
   skills/tracing-root-causes/SKILL.md` → exit 0.
   *Commit:* `feat(tracing-root-causes): carry the ask-for-help leg as
   bounded stall-escalation`.

## Interfaces between steps

- Step 2 consumes step 1's `## Baseline probe` section of PROGRESS.md
  by that exact heading.
- Step 3 consumes `skills/tracing-root-causes/evals/eval-08.md` as its
  acceptance target: the section it writes must make every checklist
  item passable.

## After the steps

work-verify (M): static + behavioral layers, step-4 fresh-context
review with verdict text recorded in the lane; then work-handoff — PR
open with `Closes MAT-93`, Linear attach, worker_done. Never merged by
this lane.
