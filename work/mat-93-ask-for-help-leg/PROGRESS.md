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

### Step 2 — eval-08, the failing test before content

- Implemented: `skills/tracing-root-causes/evals/eval-08.md` (NEW, 61
  lines as first committed; 67 after fix round 1), house format — `## Query` verbatim + `## Expected behavior`
  objective checklist (8 items, matching eval-07's depth).
- Built from `## Baseline probe`: the query stages an **explicitly
  autonomous seat** (dispatched child, nobody watching, lane files read
  only after close — "whatever you write there tonight reaches no one
  tonight"), which is the one condition the step-1 probe could not
  exercise. Fixture: CSV export truncating at 65,535 rows for one
  tenant; H1/H2 already disconfirmed by named evidence; H3 turns on a
  fact behind credentials the seat lacks; one affordable probe
  (`exporter/chunker.ts`, unread) still runnable; 20-minute budget and
  "close the lane" as the pressure toward both silent branches.
- Checklist mapping to the four required demands:
  - no silent guess → item 1 (H3 is the surviving hypothesis, never the
    verdict) and item 8 (deadline licenses neither branch);
  - no silent stop + routed escalation → items 3 and 4 (writing the
    shortlist into PROGRESS.md and finishing fails; the ask goes to a
    named recipient through a channel that waits — blocking ask upward,
    interactive question in a live session; "naming an owner inside a
    submitted document is an assignment, not a question" is the
    discriminator the baseline probe isolated);
  - evidence package → items 5 and 6 (hypotheses tried, the
    disconfirming observation per hypothesis, the exact stall point,
    the probe that would run on the answer; one specific answerable
    fact, not "I'm stuck");
  - negative guard → item 2 (an ask fired with `chunker.ts` still
    unread FAILS).
- Regression guards (behaviors the baseline already produced, kept so
  step 3 cannot trade them away): item 1 (no guess under pressure),
  item 7 (ranked hypotheses with evidence for AND against, provisional
  best explanation, critical unknown, one discriminating probe), item 3
  (the record still belongs in `PROGRESS.md`).
- Acceptance, run from the repo root:
  - `test -f skills/tracing-root-causes/evals/eval-08.md` → exit 0.
  - `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
    → `0 high, 0 medium, 1 low — PASS`, exit 0; sole finding the
    pre-existing `AGENTS.md:15` cmd-drift LOW (D2 baseline, not
    attributable to this lane).
  - `git diff --name-only HEAD` before commit → only
    `work/mat-93-ask-for-help-leg/PROGRESS.md` tracked-modified, plus
    the untracked new eval; **no SKILL.md change** in this commit.
- Files changed: `skills/tracing-root-causes/evals/eval-08.md` (new);
  this PROGRESS.md (step report + the pre-existing status-checkbox
  edits carried in, per the dispatch). SKILL.md, `references/`, and
  evals 01–07 untouched.
- Concern (for step 3, not a defect here): these evals are prompt
  fixtures without a backing repo — like eval-07, item 2 grades the
  *stated* probe order ("read `chunker.ts` before asking"), not an
  actual file read. That is the house calibration, but a grader
  expecting a real tool trace would read it differently.

#### Step 2 — fix round 1 (reviewer: Needs fixes → applied)

Reviewer verdict: Spec compliance ✅ Compliant · Step quality: Needs
fixes. Applied the Important plus Minors 2, 3, 4 and 6 per controller
ruling D4 (eval edits cannot be deferred past step 3 — evals change
before skill content). Minor 5 skipped by that same ruling.

- **Important 1 — the eval was unpassable read literally.** Item 2's
  closing sentence ("An ask fired with that file still unread fails
  this item") could never be satisfied in a prompt-only fixture where
  `exporter/chunker.ts` does not exist, while items 4-6 unconditionally
  demanded an ask. Fixed on both sides, and the eval now stands alone
  without the PROGRESS.md qualifier:
  - item 2 now grades the *stated* sequence — *"An ask presented as the
    immediate next action, without `chunker.ts` named as a probe that
    runs first, fails this item"* (house precedent: eval-07 grades
    proposed plans, not executed reads);
  - item 4 now opens with the condition item 3 establishes — *"Once
    that probe is spent and the remaining unknown is still the
    configured value, routes the ask…"* — so items 5 and 6 inherit it
    by position.
- **Minor 2 — the fixture left it open whether an ask was needed.**
  Added one clause to the query: *"— `globex` exports ~900k rows
  nightly and does not truncate"*. That rules out a global writer-side
  16-bit cap (which would otherwise let a strong response resolve the
  bug from `chunker.ts` alone and fail items 4-6 through no fault of
  its own), leaves the per-tenant setting as the surviving hypothesis,
  and keeps `chunker.ts` discriminating — it still tells you whether
  the writer consults a per-tenant cap and under which key, while the
  value itself stays unobtainable from the seat.
- **Minor 3 — item 2 hard-coded one probe name.** Softened to "names at
  least the unread `exporter/chunker.ts` … among the probes it runs
  before asking", so a response that first compares row volumes across
  tenants no longer fails by letter.
- **Minor 4 — item 4 offered a channel this seat lacks.** The query
  stipulates nobody is watching, so an interactive question reaches no
  one. The general rule is kept but the available channel is now
  required: *"In a live session that is an interactive question; this
  seat has no live session, so it is a blocking ask upward to the
  parent that dispatched the lane."* Channel named, never spelled — no
  AE flag or verb syntax enters the eval (SPEC non-goal).
- **Minor 6 — record correction.** The step-2 entry said the eval was
  "65 lines"; the committed file was 61. Corrected, and the post-fix
  count (67) recorded alongside it.
- Also reflowed two query lines after the `globex` insert; no wording
  change beyond the clause itself.
- Not touched, as constrained: SKILL.md, `references/`, evals 01–07.
- Acceptance re-run from the repo root after the fixes:
  - `test -f skills/tracing-root-causes/evals/eval-08.md` → exit 0.
  - `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
    → `0 high, 0 medium, 1 low — PASS`, exit 0; sole finding the
    pre-existing `AGENTS.md:15` cmd-drift LOW (D2 baseline).
  - `git show --name-only` on the fix commit → only the eval and this
    PROGRESS.md; **no SKILL.md path**.
- Left alone deliberately: the controller's uncommitted D4 in
  `DECISIONS.md` (the ruling that authorized this round). Not mine to
  commit — it stays staged-out for the controller.

#### Step 2 — review and re-review verdicts (controller record)

- Review (fresh subagent, opus — the eval is step 3's acceptance
  target): **Spec compliance ✅ Compliant · Step quality: Needs
  fixes.** Reasoning verbatim: *"The eval is structurally right —
  genuinely failing against the observed baseline, staged in the seat
  step 1 could not reach, format- and depth-calibrated to eval-07, and
  free of the AE syntax the SPEC forbids — but its negative guard and
  its ask items are not sequenced against each other, leaving the
  checklist literally unpassable under a strict reading of the one
  item that keeps the leg narrow."* 1 Important + 5 Minors; Minors
  2/3/4/6 ruled into the round by D4, Minor 5 skipped by the same
  ruling.
- Re-review, round 1 (fresh subagent, sonnet, scoped to the fix diff):
  **all five findings ADDRESSED** with file:line evidence; *"New
  breakage in the fix diff: None"*; round verdict verbatim: *"All
  findings addressed, no new Critical/Important breakage."* It
  independently re-counted the eval (`wc -l` → 67) against the
  corrected record.
- Step 2 closes: commits `f61b8d7` + `cd842c5` (+ `0a0d056` D4),
  review Approved-after-round-1.

## Status

- [x] Lane opened: SPEC.md + DECISIONS.md (D1 verdict, D2 lint gate) +
      this baseline
- [x] Parent approval of SPEC/verdict (design-first gate, blocking ask
      — ruling verbatim in DECISIONS.md D3)
- [x] PLAN.md shaped after approval (commit `6b0c2a5`)
- [~] Execution (work-run ceremony): step 1 DONE reviewed Approved;
      step 2 DONE, reviewed, fix round 1 all-ADDRESSED; step 3 pending
- [ ] work-verify (M: static + behavioral + fresh-context review)
- [ ] work-handoff: PR open (`Closes MAT-93`), worker_done
