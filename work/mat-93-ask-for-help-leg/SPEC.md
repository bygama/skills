# SPEC — MAT-93: the "ask for help" leg

Lane: `work/mat-93-ask-for-help-leg/` · Tier M · Judgment lane
Ticket: MAT-93 — *tracing-root-causes: carry superpowers' Phase 3.4
"ask for help" leg, deliberately left out of MAT-46*
Date: 2026-08-19 · Design input: the parent's dispatch brief

## The question

Superpowers' `systematic-debugging` (v6.3.0), Phase 3.4 "When You
Don't Know", carries four beats: *say "I don't understand X"*, *don't
pretend to know*, **ask for help**, *research more*. MAT-46 absorbed
the skill into the house `tracing-root-causes` and carried every beat
except "ask for help" — a named omission, recorded in MAT-46's D8 and
deliberately left for the parent's call. This lane judges whether that
leg earns a place in the house skill; a reasoned refusal is as valid a
close as shipped content.

## Verdict (rendered by this lane; parent approval gates PLAN.md)

**Carry it — in narrow form.** Not the upstream's bare "ask for help",
and not a universal escape hatch (MAT-47's D-003 YAGNI killed exactly
that shape for the TDD port), but a bounded stall-escalation leg: when
the investigation stalls because the critical unknown is out of reach
from the investigator's seat, escalate with the evidence — never guess
silently, never stop silently. Full reasoning, both branches
steelmanned: DECISIONS.md D1.

The load-bearing distinctions:

1. **AE's escalation shapes are channels; this leg is methodology.**
   The child's blocking `ask`, work-verify's owner gate, and the tier
   ratchet say *how* to reach a decision-maker. None of them says
   *when a debugging stall justifies the ask* or *what evidence a
   well-formed ask carries*. That content is evidence discipline —
   this skill's own domain.
2. **The house skill already reaches for this twice without a route.**
   "Get a decision before attempt four" (three strikes) and "Blocked
   by missing evidence? The ranked shortlist plus the probe IS the
   deliverable" (output shape) both presume an escalation path they
   never define. The leg completes machinery already half-present.
3. **The silent-stall failure mode is real for dispatched agents.** A
   child that writes its shortlist into PROGRESS.md and goes quiet has
   technically "delivered" while its parent hears nothing. The leg
   names the recipient.

## The leg's required beats

Phrased in AE's terms, runtime-neutral, with the non-AE fallback:

- **When**: reachable evidence is exhausted and the critical unknown
  (phase 4 already requires naming it) is a fact the investigator's
  seat cannot produce — access it lacks, knowledge a human holds, a
  decision that is the owner's. Not before: an affordable
  discriminating probe still on the table means run it, not ask.
- **WHO is asked**: the owner; the parent, when running as a
  dispatched child.
- **WHICH channel**: a child's blocking `ask` to its parent; an
  interactive question in a live session. Never a silent guess, never
  a silent stop.
- **WHAT travels with the ask**: the hypotheses tried and the evidence
  that disconfirmed each, the exact point of the stall, and the probe
  the investigator would run if the answer came back. An ask carrying
  its evidence lets the answerer act in one read; a bare "I'm stuck"
  does not.

## Scope

- `skills/tracing-root-causes/evals/eval-08.md` — NEW, written before
  any skill content, from an observed baseline probe (a fresh
  in-session subagent given the current SKILL.md and a stall fixture;
  its observed behavior documented honestly — MAT-47 D-005 sets the
  honesty bar for baseline claims).
- `skills/tracing-root-causes/SKILL.md` — the leg, one short section;
  coherence-only touches where existing lines reach for the undefined
  route (output shape's "Blocked by missing evidence?" line may gain
  the recipient); provenance comment gains the MAT-93 line — the
  surface the future upstream-watch loop (MAT-34) reads.

## Non-goals

- No universal escape hatch; no rerouting of every judgment call.
- No duplication of AE channel mechanics (flags, verbs, mailbox
  syntax) inside the skill.
- No restructuring of the phases; evals 01–07 and
  `references/techniques.md` untouched.
- No edits outside `skills/tracing-root-causes/` (README index is a
  fence per MAT-46 D4 precedent; CODE_OF_CONDUCT.md untouched per
  dispatch).

## If the parent overturns the verdict

The refusal branch: no skill edit; a dated decision record in
`docs/adrs/` (the repo's decision surface — ADR-001 lives there)
stating the leg was judged and refused, with the reasoning, so MAT-34's
upstream-watch does not re-raise it every superpowers release.

## Definition of done (M)

1. eval-08 committed before any SKILL.md change (repo hard constraint).
2. SKILL.md carries the leg, stays <500 lines, references one level
   deep, third-person description unchanged.
3. `node <AE>/scripts/agent-lint.mjs .` PASS with no findings
   attributable to this lane (baseline: PASS, 1 pre-existing LOW —
   PROGRESS.md).
4. CI `standard` check green on the PR (authoritative gate, MAT-46 D2
   lineage).
5. work-verify with fresh-context review (step 4) — verdict text
   recorded in the lane.
6. PR open with `Closes MAT-93`; never merged by this lane.
