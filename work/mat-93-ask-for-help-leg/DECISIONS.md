# DECISIONS — MAT-93

Rulings and judgment calls for this lane. Owner rulings arrive through
the parent orchestrator (`orca orchestration ask`) and are recorded
verbatim in substance.

## 2026-08-19 — D1: the verdict — carry the leg, in narrow form

**The question.** Does upstream Phase 3.4's "ask for help" beat earn a
place in `tracing-root-causes`? The parent's brief holds both branches
open; this lane renders the verdict, the parent's SPEC approval
confirms or overturns it.

**Verdict: carry it, as a bounded stall-escalation leg** — not the
upstream's bare imperative.

**The refusal branch, steelmanned first.**

1. *AE already has escalation shapes* — a child's blocking `ask`,
   work-verify's owner gate, the tier ratchet. A debugging skill
   restating them duplicates process machinery inside methodology.
2. *MAT-47's D-003 killed the sibling line*: "'Ask your human partner'
   as the universal escape hatch" was dropped from the TDD port as
   decoration.
3. *The skill already has an honest-stall exit*: "Blocked by missing
   evidence? The ranked shortlist plus the probe IS the deliverable,
   not a failure." The guess is already forbidden by making the
   shortlist the deliverable.
4. *Every line costs tokens* on every debugging invocation, in every
   repo the skill is junction-linked into.

**Why the carry branch wins anyway.**

1. **MAT-47's own precedent cuts both ways — and its nuance decides.**
   D-003 did not drop escalation wholesale; it dropped the *universal*
   escape hatch and kept "the AE route where the escape hatch is
   genuinely needed (the TDD exceptions)". In a debugging skill the
   stall is not decoration — it is a first-class state (upstream gave
   it a numbered step), and it is precisely where the skill's target
   failure mode, guessing under pressure, fires hardest.
2. **The existing text reaches for the route twice and never defines
   it.** Three strikes says "get a decision before attempt four" —
   from whom, through what? Output shape says the shortlist "IS the
   deliverable" — delivered to whom? In an interactive session the
   answer is obvious; in a dispatched child it is not, and a child
   that writes its shortlist into PROGRESS.md and goes silent has
   technically complied while the parent hears nothing. That is the
   silent stall the dispatch discipline exists to prevent.
3. **What the leg adds is methodology, not channel mechanics.** The
   redundancy argument (refusal point 1) holds only against restating
   *how* to reach a decision-maker. The leg's content is *when* a
   stall justifies the ask (reachable evidence exhausted; the critical
   unknown is a fact the seat cannot produce) and *what evidence a
   well-formed ask carries* (hypotheses tried, the disconfirming
   evidence for each, the exact stall point, the probe that would run
   if the answer came back). That is evidence discipline — the
   skill's own domain — and no AE process skill states it.
4. **The library is used outside AE repos.** This repo's skills are
   junction-linked as personal methodology usable from any repo; where
   no orchestration or work-verify exists, the escalation route is an
   interactive question to the user. The leg must therefore be phrased
   runtime-neutrally — which the upstream's bare "ask for help" is,
   but without the WHO/WHAT discipline that makes the ask actionable.
5. **Token cost is bounded and bought back.** One short section. The
   alternative — an agent that guesses because asking was never
   licensed, or spins because asking felt like failure — costs a
   debugging session, not a dozen tokens.

**Shape guard.** The leg must not become the universal escape hatch
MAT-47 killed: it fires only at the genuine stall (an affordable
discriminating probe still on the table means run it, not ask), and it
never carries AE flag/verb syntax — channels are named, not spelled.

## 2026-08-19 — D2: the lint gate — baseline PASS, findings scoped to the lane

**Context.** MAT-46 D2 and MAT-47 D-002 (same wave, same parent) ruled
the lint acceptance as "zero findings attributable to this lane, CI
`standard` green as the authoritative gate", because the cited sibling
path `../Agent-Engineering` does not resolve from an Orca worktree and
a machine-local junction was explicitly refused.

**What changed since.** The linter's cmd-drift check was softened
upstream: the same AGENTS.md:15 finding is now LOW and the run exits 0.
Baseline on this worktree, clean tree at `e969b67`, run via the
absolute path `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`:

```
LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
0 high, 0 medium, 1 low — PASS
```

**Acceptance for this lane** (inheriting the D2/D-002 lineage, now with
a green baseline): lint PASS (exit 0) with no findings attributable to
this lane — the pre-existing LOW stays reported, not fixed here — plus
the CI `standard` check green on the PR.

## 2026-08-19 — D3: SPEC approved — the design-first gate ruling

**Ruling (parent, blocking ask), verbatim in substance:**

> APPROVED as written. CARRY-narrow is the right verdict and the
> differentiation from MAT-47 D-003 is the load-bearing part: that lane
> killed a universal "ask your human" escape hatch; yours is a bounded
> stall-escalation that fires only on exhausted reachable evidence,
> with the evidence package (hypotheses tried + disconfirming evidence
> + exact stall point + the probe you'd run given the answer) as the
> price of asking — AE's channels say HOW to ask, your leg says WHEN,
> which nothing else covers. Baseline probe before eval-08 before
> content is the skill-authoring method applied correctly. Shape
> PLAN.md and proceed. Record this ruling in your DECISIONS.md.

PLAN.md proceeds under this ruling; the WHEN-not-HOW framing is the
acceptance bar for the leg's wording in step 3.

## 2026-08-19 — D4: step-2 minors enter the fix loop, against the default

**Context.** work-run's default: Minor findings never enter the fix
loop — they are recorded and deferred to work-verify's triage. The
step-2 review returned 1 Important (negative guard vs ask items not
sequenced; item 2's last sentence literally unsatisfiable in a
prompt-only fixture) and 5 Minors, four of which are edits to
eval-08's text (fixture completeness, hard-coded probe name,
unavailable channel listed, a line-count record error).

**Ruling (controller).** Minors 2, 3, 4 and 6 join the round-1 fix
dispatch; Minor 5 (optional consolidation of item 8) is skipped and
recorded here. Reason: the repo's hard constraint is that evals change
BEFORE skill content. Step 3 writes SKILL.md against eval-08's
checklist; a Minor deferred to work-verify would land as an eval edit
AFTER the skill content it gates — the exact inversion the constraint
forbids. Deferring is the norm; here the ordering constraint outranks
the norm. Minor 5 is skipped because consolidation is style, changes
no grading outcome, and eval-06 sets the house precedent for
pressure-restating items.
