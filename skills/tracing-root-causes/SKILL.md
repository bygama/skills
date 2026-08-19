---
name: tracing-root-causes
description: Owns debugging end to end — reproduce, isolate, hypothesize, disconfirm, fix — with competing hypotheses, evidence ranked by strength, active disconfirmation, and a fix at the source rather than the symptom. Use for any bug, test failure, crash, regression, flaky or intermittent failure, performance problem, build failure, or unexpected behavior, BEFORE proposing or applying a fix; and for explaining why something happened (production incidents, surprising benchmark results) when no fix is on the table yet. Especially when an "obvious culprit" is tempting, when a previous fix did not work, or when time pressure makes guessing attractive. Supersedes superpowers:systematic-debugging as the house debugging skill.
---

# Tracing root causes

<!-- Methodology distilled from OMC's tracer agent (MIT), 2026-07-30.
     Provenance: Context-Engineering repo, docs/adrs/ADR-002-omc-salvage.md.
     Absorbed superpowers' systematic-debugging (v6.3.0), 2026-08-19: the
     phased spine, the iron law, the rationalization tables, and four
     techniques (references/techniques.md). MAT-46. -->

Explain outcomes through evidence, not narrative, and fix causes rather
than symptoms. The failure mode this prevents: jumping from symptom to
favorite explanation, collecting only confirming evidence, then patching
where the error surfaced.

## The iron law

```
NO FIX WITHOUT A ROOT CAUSE FIRST
```

A fix proposed before the cause is known is a guess wearing a diff.
This holds hardest exactly where it feels most expensive — during an
outage, at hour four, with a senior engineer already typing.

Two things it does **not** forbid:

- **Mitigation under a live incident.** Stopping the bleeding is
  legitimate — a rollback, a circuit breaker, a feature flag. It is
  named as mitigation, kept reversible, and it does not end the
  investigation or get recorded as the cause.
- **Doing what you were asked.** If the owner has heard the concern and
  still wants their fix, write it, state the assumption it rests on and
  what would falsify it, and move on. State a disagreement once, plainly
  — never twice, and never as silent compliance.

## The phases

Run them in order. A phase skipped is a phase paid for later.

### 1. Reproduce

- **Read the error completely** — full message, full stack, line
  numbers, codes. It frequently names the answer.
- **Restate exactly what was observed** — which artifact, what behavior,
  when. If you catch yourself rewriting the observation to fit a theory,
  stop.
- **Trigger it reliably.** What are the exact steps? Every time, or one
  run in five? Not reproducible is a finding, not a dead end: it points
  at timing, environment, or shared state.
- **Check what changed** — recent commits, dependency bumps, config and
  environment differences between where it fails and where it doesn't.

### 2. Isolate

Narrow the failure to one component before explaining it.

- **Instrument the boundaries.** In a multi-component system (CI → build
  → sign, API → service → database), log what enters and what leaves
  each boundary, run once, and read which hop breaks. Evidence about
  *where* beats speculation about *why*.
- **Compare against something that works.** Find the nearest working
  case and list every difference, however small. "That can't matter" is
  a hypothesis, not an observation.
- **Trace backward to the origin** — see
  [references/techniques.md](references/techniques.md) for backward
  tracing, instrumenting a dangerous operation, and bisecting a test
  suite to find which test dirties the tree.

### 3. Hypothesize

- **Compete the explanations.** Hold ≥2 from deliberately different
  frames: code path, config/environment, measurement artifact, external
  dependency, timing, shared state. "The measurement is wrong" is always
  a candidate.
- **Rank evidence by strength.** Strongest to weakest: controlled repro
  / discriminating experiment → primary artifact with tight provenance
  (timestamped logs, git history, file:line behavior) → independent
  sources converging → single-source inference that fits →
  circumstantial (naming, temporal proximity, stack position, "this same
  fix worked on another service") → intuition, seniority, confidence.
  When tiers conflict the higher tier wins; never treat support as flat.

### 4. Disconfirm

- **Ask the two questions** of each serious hypothesis: "what should we
  observe if this were true — do we?" and "what observation would be
  hard to explain if this were true?" A hypothesis that survives only
  because nobody looked for counter-evidence keeps LOW confidence.
- **Run a rebuttal round.** Let the strongest alternative challenge the
  leader with its best contrary evidence before you conclude. Down-rank
  explanations that need fresh unverified assumptions to survive, and
  don't merge hypotheses that merely sound alike (fake convergence).
- **Test minimally.** One variable, the smallest change that
  discriminates. A bigger timeout after a failed timeout is not a new
  hypothesis — it is the same one, retried louder.
- **Name the critical unknown** — the single missing fact behind most of
  the remaining uncertainty — and the ONE probe that best separates the
  top hypotheses. Prefer probes that discriminate over probes that
  gather more of the same support.

### 5. Fix

- **Write the failing reproduction first.** Simplest case that fails;
  a test if there's a framework, a script if there isn't. It proves the
  cause and proves the fix.
- **Fix at the source**, not where the error surfaced. One change, one
  concern — no bundled refactor, no "while I'm here".
- **Then make it impossible.** Add validation at the layers the bad
  value crossed — entry, business logic, environment guard,
  instrumentation ([references/techniques.md](references/techniques.md)
  — defense in depth, and condition-based waiting when the cause was
  timing).
- **Verify, then hand off.** Confirm the reproduction now passes and
  nothing else broke — then stop; see *Where this ends*.

## Three strikes → question the architecture

Count fix attempts. At three failures, the problem is no longer the
hypothesis:

- Each fix reveals a new problem somewhere else.
- Each fix needs "just a bit of refactoring" to land.
- The same symptom keeps returning in a new costume.

Stop. Say plainly that this looks structural, name the pattern you think
is wrong, and get a decision before attempt four. This is not a failed
hypothesis — it is a wrong architecture, and more fixes make it worse.

## Rationalizations

| Thought | Reality |
|---|---|
| "Emergency — no time for process" | Systematic is FASTER than guess-and-check thrashing. The process is the short path out, not a tax on the outage. |
| "It's simple, it doesn't need this" | Simple bugs have causes too, and the process is quick on them. |
| "Quick fix now, investigate after" | The first fix sets the pattern, and "after" rarely arrives. |
| "Just try it and see if it works" | A change that fixes a symptom you don't understand hides the cause. |
| "It's probably X, let me fix that" | Seeing a symptom is not understanding a cause. |
| "This same fix worked last week elsewhere" | A pattern match on a different system is circumstantial evidence about this one. |
| "It broke right after the deploy" | Temporal proximity is bottom-tier evidence. Correlate with changed code paths and rollout timing before believing it. |
| "The senior engineer is sure" | Experience raises a prior; it is not an observation. Ask what would show it. |
| "One more attempt" (after 2+) | Three failures is an architecture signal. Stop and say so. |
| "Four hours in — I can't waste them" | Sunk cost. Those hours are spent either way; only the next hour is a choice. |
| "Bigger timeout, then ship it" | A passing sleep is a race CI will lose. Wait on the condition. |
| "Multiple fixes at once saves time" | You cannot tell which one worked, and you added new bugs. |
| "The reference is long, I'll adapt it" | Partial understanding guarantees bugs. Read it completely. |
| "No root cause — it's just flaky" | 95% of "no root cause" is incomplete investigation. Prove environmental before accepting it. |

Redirection from a human — *"is that not happening?"*, *"stop
guessing"*, *"we're stuck?"* — means the same thing: return to phase 1.

## Output shape

Ranked hypothesis table (confidence + evidence strength + why it
survives), evidence for AND against each, current best explanation
(explicitly provisional when evidence is incomplete), critical unknown,
discriminating probe. When a fix follows, the reproduction, the change,
and what it means if the change didn't help.

Blocked by missing evidence? The ranked shortlist plus the probe IS the
deliverable, not a failure.

## Where this ends

This skill investigates and fixes. It does not certify.

- **Evidence lands in the lane's `PROGRESS.md`** — the reproduction, the
  ranked hypotheses, the probe results, the change. Not a scratch
  debugging log, not the chat alone.
- **`work-verify` owns the completion gate.** Hand off there. "The test
  passes now" is a result to be verified, never a verdict.
- **`work-handoff` closes the lane.** This skill never declares work
  done, fixed, or shippable on its own authority.

## Judgment notes

- Investigation only? Stop after phase 4 and deliver the trace. Asked to
  fix as well? Finish the trace first, then fix — the phases are the
  order, not a menu.
- Framing pressure ("confirm it was the deploy so we can roll back") is
  a request for a conclusion, not for analysis. Give the analysis; if it
  supports the framing, say so with the evidence.
- This skill supersedes `superpowers:systematic-debugging`, whose
  machinery it absorbed. Superpowers remains installed as the fallback
  where this library is not.
