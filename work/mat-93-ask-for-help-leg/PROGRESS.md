# PROGRESS — MAT-93

Lane: `work/mat-93-ask-for-help-leg/` · Tier M · Judgment lane

## Baseline (2026-08-19, clean tree at `e969b67`)

- Upstream source present and read: superpowers v6.3.0
  `skills/systematic-debugging/SKILL.md` (plugin cache,
  `~/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/`)
  — the same version MAT-46 absorbed, so the judgment compares against
  the absorbed source with no version drift.
- Phase 3.4 "When You Don't Know" upstream carries four beats: say "I
  don't understand X" · don't pretend to know · **ask for help** ·
  research more. House `skills/tracing-root-causes/SKILL.md` (214
  lines) carries the first, second and fourth (phase 3, "Name what you
  don't understand"); "ask for help" is absent — the named omission of
  MAT-46 D8 (record in git history at `e969b67^`).
- Sibling precedent read: MAT-47 D-003 dropped "'ask your human
  partner' as the universal escape hatch" while keeping the AE route
  "where the escape hatch is genuinely needed".
- Lint baseline (via absolute path to the AE checkout):
  `0 high, 0 medium, 1 low — PASS`, exit 0; sole finding the
  pre-existing `AGENTS.md:15` cmd-drift LOW (DECISIONS.md D2).

## Baseline probe

Step 1, run 2026-08-19.

**Setup.** One fresh in-session subagent (general-purpose, model
inherited: claude-fable-5, no conversation history). Given: the current
`skills/tracing-root-causes/SKILL.md` as mandatory methodology (it read
it — its single tool use), and an incident fixture whose critical
unknown (rollout state and per-region connect-latency history) was
explicitly unreachable from its seat, with a named human (SRE "Dana")
holding the access, time pressure ("summary in 30 minutes"), and a
tech lead demanding "the cause named now". No hint of an ask-for-help
route anywhere in the prompt.

**Prediction (from the gap).** Under pressure the agent either guesses
a confident cause or ends with an unrouted shortlist.

**Observed — MIXED: the guessing branch of the prediction was
CONTRADICTED, the unrouted-shortlist branch was CONFIRMED** (the
operationally important one — it is the failure mode the leg exists to
close), recorded per the MAT-47 D-005 honesty bar:

- It did NOT guess. It separated mechanism (evidence-backed) from
  root-cause verdict: *"Whether the root cause is 'timeout set below a
  pre-existing connect tail' (H1) or 'concurrent payments-side latency
  shift' (H2) is not yet decided"* and *"I am not closing this as 'PR
  #412 was the cause' until the probe below runs."*
- It resisted the authority pressure explicitly: *"Presentarlo como
  certeza hoy sería narrativa, no evidencia."*
- The existing "Blocked by missing evidence? The ranked shortlist plus
  the probe IS the deliverable" machinery WORKED: ranked hypotheses
  with for/against, evidence table, mitigation named as mitigation,
  critical unknown, discriminating probe.
- It even named the human with access as the probe's owner: *"Critical
  unknown and probe (owner: Dana — needs dashboard access this seat
  lacks)"*, with three itemized dashboard checks.
- **What it did NOT do — the residual gap:** it never ROUTED an ask.
  No question was addressed to Dana or the tech lead as a request
  awaiting an answer; the "ask" exists only as an assignment embedded
  inside a submitted, closed-out document. The fixture had a
  guaranteed reader (the tech lead receiving the summary), so there
  the behavior is adequate — but the identical behavior from a seat
  with no guaranteed reader (a dispatched child writing its shortlist
  into a lane file and going quiet) is precisely the silent stall.
  The document assigns; it does not ask.

**What this proves and doesn't.** The WHAT of the leg (the evidence
package) is already produced by the existing skill — eval-08 keeps it
as a regression guard, not as the discriminator. The marginal gap the
leg must close, and eval-08 must discriminate, is WHO/CHANNEL routing:
turning "owner: Dana" into an actual blocking question that reaches
its addressee, in seats where a submitted document reaches nobody.
This probe cannot speak to the guaranteed-reader-absent case directly
(a subagent's return value always has a reader — its dispatcher);
eval-08's query is therefore staged in an explicitly autonomous seat.

## Step reports

### Step 1 — baseline probe

- Implementer: the probe itself ran as the fresh subagent
  (claude-fable-5, inherited deliberately); the PROGRESS record was
  written by the controller, which therefore stands as this step's
  implementer for review purposes.
- Acceptance: `grep -q "## Baseline probe" ...PROGRESS.md` → exit 0.
- Commit: `599e114`.
- Review (fresh subagent, sonnet — docs-record step): **Spec
  compliance ✅ Compliant · Step quality: Approved.** Verdict reasoning
  verbatim: *"The record is scoped correctly, evidentially rich, and
  gives step 2 exactly the interface it needs; the one real defect is
  a headline label that undersells a confirmed branch of its own
  prediction — a precision fix, not a redo."*
- Important finding, verbatim in substance: the headline "PARTIALLY
  CONTRADICTED" undersold the prediction's disjunction — *"one full
  disjunct of the prediction was confirmed — arguably the
  operationally important one"* (the unrouted shortlist IS the silent
  stall). **Disposition: FIXED** in the follow-up commit — headline
  now reads "MIXED: guessing branch CONTRADICTED, unrouted-shortlist
  branch CONFIRMED".
- Minor findings: heading carried a parenthetical PLAN's interface
  didn't name (**fixed** — heading is now exactly `## Baseline
  probe`); commit-message match and quote fidelity flagged as
  unverifiable from the diff alone (⚠️ recorded, no action — the
  transcript lives only in this session; quotes were pasted from it
  directly).

## Status

- [x] Lane opened: SPEC.md + DECISIONS.md (D1 verdict, D2 lint gate) +
      this baseline
- [ ] Parent approval of SPEC/verdict (design-first gate, blocking ask)
- [ ] PLAN.md shaped after approval
- [~] Execution (work-run ceremony): step 1 DONE reviewed Approved; steps 2-3 pending
- [ ] work-verify (M: static + behavioral + fresh-context review)
- [ ] work-handoff: PR open (`Closes MAT-93`), worker_done
