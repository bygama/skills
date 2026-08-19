# PROGRESS — MAT-94

Lane: `work/mat-94-attribution-skills/` · Tier M
Branch: `bygama/mat-94-attribution-skills` (stacked on
`bygama/mat-93-ask-for-help-leg`, PR base = that branch)

## State

- [x] SPEC.md written and committed (c7565bb)
- [x] SPEC approved by parent at the design-first gate (D1)
- [x] PLAN.md shaped (5 steps)
- [x] Step 1 — D2: classify testing-first
- [ ] Step 2 — D3: classify tracing-root-causes
- [ ] Step 3 — D4: NOTICE judgment
- [ ] Step 4 — notices applied/normalized
- [ ] Step 5 — README Provenance lines
- [ ] work-verify (M) incl. fresh-context review
- [ ] work-handoff: PR open (base mat-93 branch), Linear attach,
      worker_done

## Done

### Step 1 — D2: classify testing-first vs upstream v6.3.0

**Implemented.** Diffed both testing-first files against their upstream
counterparts in
`C:/Users/mateo/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/skills/test-driven-development/`
and recorded the classification in DECISIONS.md under
`## D2 — classification: testing-first`. Version and license confirmed
from the upstream tree itself (`.claude-plugin/plugin.json` →
`"version": "6.3.0"`, `"license": "MIT"`; `LICENSE` → `Copyright (c)
2025 Jesse Vincent`) rather than from the port record.

Method: whole-file read of all four files; a mechanical pass for
whitespace-normalized identical lines (≥20 chars) and longest common
word runs; then targeted `diff -u` over every section those passes
flagged. Every claim in D2 carries local + upstream line numbers.

**Verdicts.**

- `skills/testing-first/SKILL.md` → **SUBSTANTIAL**. Nine identical
  normalized lines, a 40-word verbatim code block (the RED example), a
  byte-identical Iron Law, a 19-word verbatim prose run in the
  rationalization table; and, heavier, dense structural parallels —
  the RED/verify/GREEN/verify/REFACTOR spine, 10 of 11 rationalization
  rows in order, 12 of 13 red flags in order, 7 of 8 checklist boxes,
  the same four When-Stuck rows, the same worked bugfix example, the
  same final-rule block.
- `skills/testing-first/references/writing-good-tests.md` →
  **SUBSTANTIAL** (much stronger). 33 identical normalized lines, a
  111-word contiguous verbatim run, both gate blocks near
  byte-identical, all three code examples verbatim, the ten-row quick
  reference in upstream order with eight rows byte-identical, and
  11-for-11 warning signs several of which differ only by a terminal
  period.

Actions implied (consumed by steps 3-5): whole-file notice form for
both files, normalized in place over the MAT-47 blocks
(`SKILL.md:8-12`, `references/writing-good-tests.md:6-10`) — never
stacked; D4's NOTICE judgment is live, not moot; README Provenance
records testing-first as derived-with-notice.

**Acceptance.**

```
$ grep -q "## D2 — classification: testing-first" \
    work/mat-94-attribution-skills/DECISIONS.md; echo $?
0

$ git diff --name-only HEAD
work/mat-94-attribution-skills/DECISIONS.md
# → no skills/** file touched
```

Lane constraint re-checked (not a step-1 gate):

```
$ node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .
0 high, 0 medium, 1 low — PASS
```

The single LOW is the pre-existing `AGENTS.md:15` cmd-drift note; not
attributable to this lane.

**Files changed.** `work/mat-94-attribution-skills/DECISIONS.md`
(D2 entry appended), `work/mat-94-attribution-skills/PROGRESS.md`
(this report). No skill file edited, per the step.

**Concerns.** None on the verdicts — the writing-good-tests evidence is
overwhelming and SKILL.md's structural case is dense and quotable. One
handoff note for step 4: the SPEC cites the existing testing-first
notices as `SKILL.md:8-9` and `writing-good-tests.md:6-7`; the actual
blocks are five lines each (8-12 and 6-10) and carry a stale
`work/mat-47-house-tdd/DECISIONS.md` pointer that this lane's
normalization supersedes.
