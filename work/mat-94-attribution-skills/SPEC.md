# SPEC — MAT-94: evidence-based attribution for ported upstream material

Lane: `work/mat-94-attribution-skills/` · Tier M · Judgment lane
Ticket: MAT-94 — *skills repo: decide the repo-wide attribution stance
for ported upstream material*
Date: 2026-08-19 · Design input: the parent's dispatch brief + the
owner ruling recorded in the ticket
Stacked on: branch `bygama/mat-93-ask-for-help-leg` (open PR #12) — this
lane touches `skills/tracing-root-causes/SKILL.md`, which that PR edits.
PR base is that branch, never main.

## The ruling (binding, owner, 2026-08-19)

Evidence-based classification. Copyright protects expression, not
ideas. Per candidate: diff its text/structure against the real
upstream; **substantial ported expression/structure** → per-file
upstream notice (additive — the repo stays MIT © 2026 Mateo García);
**idea-only rewrite** → NO notice, classification + diff evidence
recorded. A sibling AE lane applies the same ruling to
shaping/skill-authoring and lands the stance rule in
`reference/skills.md`; this lane covers ONLY the bygama/skills repo.

## Upstream ground truth

`C:/Users/mateo/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/skills/`
— verified present this session. The files that matter:

- `test-driven-development/SKILL.md`
- `test-driven-development/writing-good-tests.md` (a reference inside
  the TDD skill, not a separate skill — the port record's phrasing
  "test-driven-development + writing-good-tests" means these two files)
- `systematic-debugging/SKILL.md` plus its technique references
  (`root-cause-tracing.md`, `condition-based-waiting.md`,
  `defense-in-depth.md`, `find-polluter.sh`)

The dispatch's fallback (upstream unreachable → classify from port
records, never guess) is moot: the path is reachable.

## Candidates and the priors the history sets

1. **`skills/testing-first/**`** — declared PORT of
   test-driven-development + writing-good-tests (MAT-47). Two files
   already carry the MAT-47 child's conservative notices
   (`SKILL.md:8-9`, `references/writing-good-tests.md:6-7`). Expect
   substantial — the Iron Law and the rationalization table are
   distinctive upstream expression — but the verdict comes from the
   diff, not the expectation. Action: classify, then NORMALIZE the
   existing notices to the ruling's format; never a second notice
   stacked on the old one.
2. **`skills/tracing-root-causes/**`** — the BASE is owner-original
   (Context-Engineering salvage; verified: `git show
   9d1b574~1:skills/tracing-root-causes/SKILL.md` = 57 lines).
   Candidates are ONLY the MAT-46 additions (commits 9d1b574 + c147b56
   + 0e11081: the phased spine, the iron law, the rationalization
   tables, `references/techniques.md`'s four techniques) and the
   ask-for-help section this branch's base carries (PR #12, derived
   from upstream Phase 3.4 in narrow WHEN-not-HOW form). Classify
   those, section by section, against systematic-debugging v6.3.0. If
   only parts are substantial, the notice names those parts — it never
   blankets an owner-original file. The file already carries a
   provenance HTML comment (lines 10-14, MAT-46/MAT-93); any notice
   extends that block rather than adding a second one.
3. **`README.md ## Provenance`** — the section exists (MAT-47 child).
   Normalize/extend it: one line per classified skill
   (derived-with-notice / idea-only-rewritten) with the evidence
   pointer. Honest and small.
4. **LICENSE** — stays MIT © 2026 Mateo García, byte-untouched. Judge
   whether a root NOTICE file carrying the upstream MIT permission
   text is needed for files classified substantial (MIT wants the
   copyright notice + permission notice to travel with substantial
   portions of the Software); if yes, ONE NOTICE at the repo root
   enumerating them; the judgment is recorded either way.

## The normalized notice format (this lane's proposal, approved with this SPEC)

Per-file HTML comment immediately after the frontmatter, one block,
four facts: upstream file + version, upstream license + copyright,
adaptation date, classification verdict with the evidence pointer.

Whole-file substantial:

```
<!-- Derived from superpowers' <upstream-path> (v6.3.0), MIT License,
     Copyright (c) 2025 Jesse Vincent. Adapted 2026-08-19.
     Classified substantial (MAT-94); evidence:
     work/mat-94-attribution-skills/DECISIONS.md. -->
```

Parts-only substantial (owner-original base): same block, but opening
with the named sections — "Sections <X, Y> derived from …; base and
remaining sections original." Idea-only rewrite: NO notice; the verdict
and diff evidence live in DECISIONS.md and the README Provenance line.

## Evidence record

One decision entry per candidate (per section group for
tracing-root-causes) in this lane's DECISIONS.md: what was diffed
(exact upstream file, exact local range), the matches found (verbatim
runs, structural parallels — quoted, not asserted), the verdict, the
action taken. The README Provenance lines point here.

## Scope (files touched)

- `skills/testing-first/SKILL.md`,
  `skills/testing-first/references/writing-good-tests.md` — notice
  normalization only.
- `skills/tracing-root-causes/SKILL.md` — provenance-comment extension
  per classification; `skills/tracing-root-causes/references/techniques.md`
  — notice only if classified substantial.
- `README.md` — `## Provenance` section only.
- `NOTICE` (repo root) — only if the judgment lands yes.
- `work/mat-94-attribution-skills/` — the lane's own files.

## Non-goals

- No eval changes: notices are comments and change no graded behavior
  (the evals-first hard constraint stays honored because nothing
  beyond notices is in scope). Anything that would change graded
  behavior is out of scope for this lane.
- No skill content or behavior changes — comments, README, and an
  optional NOTICE file only.
- No LICENSE edit, no license switch; the repo stays MIT © 2026
  Mateo García.
- No edits to the AE repo (the sibling lane owns
  shaping/skill-authoring and `reference/skills.md`).
- `CODE_OF_CONDUCT.md` untouched (dispatch fence — the owner keeps a
  deliberate uncommitted change in the main checkout).

## Definition of done (M)

1. Every candidate classified with diff evidence recorded in
   DECISIONS.md — per section for tracing-root-causes, per file for
   testing-first.
2. Notices normalized/applied exactly per classification: no notice on
   idea-only material, parts named on partial files, no stacked
   duplicates.
3. README `## Provenance` carries one line per classified skill with
   verdict + evidence pointer.
4. NOTICE judgment recorded in DECISIONS.md; file created iff yes.
5. `node <AE>/scripts/agent-lint.mjs .` PASS with no findings
   attributable to this lane.
6. CI `standard` check green on the PR.
7. work-verify (M) with the step-4 fresh-context review — verdict text
   recorded in the lane.
8. PR open with base `bygama/mat-93-ask-for-help-leg`, body carries
   `Closes MAT-94`; never merged by this lane.
