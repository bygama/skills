# SPEC — `design-skills-at-scale`

**Tier:** L (four lane files + `feature_list.json`).
**Tracker:** Linear — workspace `bygama` · team MAT · project `skills`. No issue opened yet.
**Design input:** the shaping conversation of 2026-08-20 (binding). Approved by the owner
before this file was written.
**Evidence repo:** `Pegasuz-Core/frontend-admin/admin` in the Pegasuz checkout — read-only here.
This lane changes nothing in Pegasuz.

---

## 1. Problem

`designing-consistently` and `extracting-design-md` were written for single-app repos. Run
against the Pegasuz admin ERP — one feature-gated Vue 3 build serving every tenant — each step
fails for a different reason.

Measured on that admin (2026-08-20): **157 views** (125 admin + 20 servicio + 7 pos + 5 tenant),
**175 components**, **13 nav groups**, **no DESIGN.md**, **83** `--nl-*` variables already
defined, **386** distinct hex literals in `src`, **371** `text-[Npx]` type-scale escapes
(13px x92, 11px x76, 14px x74, 10px x68, 12px x61), and a radius ladder of 8/9/10/11/12/13/14px
plus 999px.

Four defects follow:

- **Discovery dead-ends.** `designing-consistently` step 1 is "locate the app's DESIGN.md …
  missing? Offer to instantiate it". In this admin the answer is wrong: no DESIGN.md exists,
  but the system does — 83 `--nl-*` tokens in code, four dated gotchas in the admin's own
  `AGENTS.md`, prose docs of unknown authorship. The skill proposes creating a file instead of
  finding what already governs.
- **The slice does not exist.** Step 2 says "read tokens + Decisions for the target surfaces".
  `## Decisions` is flat (`### <surface>` per route). At 157 views that is 157 flat sections —
  unreadable, and unbounded per session.
- **Frequency canonizes drift.** `extracting-design-md` step 4 decides tokens by "semantic role
  + frequency". `text-[13px]` occurring 92 times is drift with a large number, not a decision.
  A first extraction from a drifted admin therefore emits contaminated tokens **as law**, and
  neither skill can tell a real decision from a frequent accident afterwards.
- **Both skills cite a repo that no longer exists.** `designing-consistently:28` and `:54` cite
  `Context-Engineering templates/repo/DESIGN.md.template` and `context-lint`;
  `extracting-design-md:13` and `:61` cite `Context-Engineering reference/design-md.md` and
  "`design-md-gen` and `context-lint` from the Context-Engineering clone".
  `../Context-Engineering` does not exist — verified. The assets live in `Agent-Engineering`
  (`templates/repo/DESIGN.md.template`, `reference/design-md.md`, `scripts/design-md-gen.mjs`,
  `scripts/agent-lint.mjs`).

## 2. Owner statements (verbatim, from the shaping conversation)

- "we need to focus also in correct the design.md while iterating because is a huge admin and
  somethings are not consistent so is normal to contaminate the design.md the first time if
  didnt exist at first"
- "i think on decisions to distribute in section like modules mercado libre, idk for more orden
  you know for big projects … para un proyecto grande estaria bueno que en las decisions tenga
  una arquitectura de carpetas viste tipo te diga el titulo de que pagina es esta es productos,
  esta es stock, esta es facturas"
- "my friend told me the best front and tokens was from products and stock just in case but well
  is huge and then i need to see page by page"
- "what if si se equivoca y algo que seguro se repite en todas las paginas se pone en cierto
  modulo? tendria que haber una seccion de global que se lea siempre"
- "los 10 principios de UX-UI-STANDARDS.md eso ignoralo es algo que hizo mi amigo que esta gaga"

The last one is binding and generalizes: a design doc found in a repo has unknown authorship and
unknown currency. It is a candidate, never law.

## 3. Approved design

**Approach A — redivide the two skills.** `designing-consistently` absorbs discovery and repair
and becomes able to stand alone; `extracting-design-md` stops being a prerequisite and starts
emitting an honestly-provisional file.

### 3.1 Provisional model

The marker lives inside the entry text, after the date, so the existing lint rule
(`- YYYY-MM-DD — `) is untouched:

```
- 2026-08-20 — [provisional] cards use radius 14px; 10px is the migration target
- 2026-08-20 — back button sits in the sticky header
```

1. Only confirmed entries are binding; a `[provisional]` entry is a candidate.
2. Provisional entries are exempt from the format's never-silently-drop rule. Confirmed entries
   keep it.
3. Promotion is earned by real work: a session touching a surface governed by a provisional
   entry must promote it (built on it, it held — drop the marker, re-date) or demote it (replace
   with the corrected entry plus one line saying what beat it).

### 3.2 Source-trust hierarchy — discovery never confirms

| Rank | Source | Status on entry |
|---|---|---|
| 1 | Code at the owner-designated reference surfaces | **confirms** |
| 2 | Owner context files (`AGENTS.md` gotchas) | `[provisional]` — strongest candidates |
| 3 | Prose docs found in the repo (authorship/currency unknown) | `[provisional]` |
| 4 | Frequency in non-reference code | tie-break only, never a source of truth |

Only the owner or a reference surface confirms. Finding a file is not evidence that it governs.

### 3.3 Decisions gains a module architecture

`### <module>` then `#### <route> — <page>` then dated entries. The taxonomy is derived from the
app's own declared navigation source when one exists, else the route tree, else asked. Skill text
stays runtime-neutral: no framework, product, or filename from any one repo.

### 3.4 The `### Global` tier

First section under `## Decisions`, always read whatever page is touched. Two sub-blocks, no more:
`#### Sistema` (values and scales: tokens, radii, type, modes, reference-surface declaration) and
`#### Patrones` (form and behavior crossing modules).

- **Entry:** owner declaration (confirmed) · escalation when the same decision appears in two or
  more modules (keeps its status) · discovery (always `[provisional]`).
- **Precedence:** the more specific wins **only if it declares the exception** —
  `- YYYY-MM-DD — exception to Global/<block>: <what> — <why>`. Without that declaration a module
  entry contradicting a global is drift, and the repair gate flags it. Provisional never beats
  confirmed at any level; confirmed-vs-confirmed with no declared exception stops and asks.
- **Exit:** narrowing (a legitimate, owner-confirmed contradiction moves the global down to one
  module) · demotion (a provisional global that did not hold).
- **Guard rail:** a global is written as a rule and never names a route. A global naming a route
  is almost always a misplaced module entry.

### 3.5 designing-consistently — rewritten loop

| Step | Now | After |
|---|---|---|
| 1 | Locate DESIGN.md; missing → offer to instantiate | **Discover the standing system** per 3.2. Instantiating is the last option, never the first answer |
| 2 | Read tokens + Decisions for target surfaces | **Slice**: `### Global` always + the touched module + the 2-3 sibling pages it must match. Never the whole file |
| 3 | Build consuming the system | unchanged, plus: conflicting with a **provisional** entry is evidence, not a violation |
| 4 | Record decisions (gate) | **Record + repair (gate)**: promote/demote status *and* escalate/narrow scope |
| 5 | Verify | unchanged, repointed at `agent-lint` |

> **Amended at step 4 (2026-08-20) — SPEC §5 prediction 1 contradicted; ruling in `DECISIONS.md`.
> Revised at step 4 fix round 1 (2026-08-20).** The cold baseline
> (`baseline-designing-consistently.md`, `## Step 1`) shows the run made **no** offer to
> instantiate. The `Now` cell describes the skill's *text* correctly, but its behavior is worse
> than the prediction assumed: step 1 dead-ends. The template it names does not resolve at any
> plausible sibling location searched (controller addendum 4; SPEC §1); neither the read-only
> fence nor the absent owner accounts for the missing offer (the run recorded four other
> counterfactual writes and never this one); nor does the fourth candidate cause, the runner's
> instruction to "proceed through steps 2–5 anyway" (same section), which the runner never cites
> as its reason and which that same counterfactual convention existed to work around; and step 1
> produced neither an offer nor a discovery — the standing system surfaced only at `## Step 2`, as
> an improvisation the runner flagged as outside what the skill asked for. That second half, the
> missing discovery, depends on none of the four causes. The `After` column is unchanged: the
> baseline strengthens it.

### 3.6 extracting-design-md — honest output

- Reference surfaces replace frequency as the token criterion; frequency only breaks ties among
  non-reference surfaces.
- Anything not confirmed against a reference surface ships `[provisional]`. A first extraction
  from a drifted app being mostly provisional is the correct output, not a failure.
- It writes the module architecture and the `### Global` tier.
- It stops being a prerequisite: `designing-consistently` no longer depends on it having run.

> **Amended at step 4 (2026-08-20) — SPEC §5 prediction 2 CONFIRMED, per the parent's ruling in
> `DECISIONS.md`; note rewritten at step 4 fix round 1 (2026-08-20).** Election ran on frequency
> and landed unconfirmed values as law. In `baseline-extracting-design-md.md` `## Step 4`,
> `radius: card: '14px'` was elected on a 984-against-roughly-751 count the run itself flagged as
> a near-tie needing the owner's eyes, and `radius: sheet: '20px'` shipped in the emitted
> frontmatter for a role the run states verbatim it "did **not** elect". Evidence detail beneath
> that verdict, not a competing one: `13px` itself was never elected, the run having declined to
> elect any token for the 430 arbitrary-pixel type values, and the `small: '0.8125rem'` in that
> frontmatter is the doc47 `text-small` role (398 uses), not the 92-count drift value.
>
> **Decided by the parent, binding on steps 6 and 8** (ruling in `DECISIONS.md`; SPEC §4
> requirement 5 unchanged): a role with a genuine but unconfirmed election puts its token in
> frontmatter, which is the compile source, **and** gets a `[provisional]` entry under
> `### Global` / `#### Sistema` naming the token and what it beat, carrying both counts — "an
> entry that says only `[provisional] card is 14px` is not enough." A role with **no** election
> gets **no** frontmatter token at all, plus an open-question entry recording the role, the
> competing candidates and their file locations. Marking something provisional does not license
> inventing it.

## 4. Requirements

1. `designing-consistently` step 1 discovers the standing system from code, owner context files
   and prose docs, and does not propose instantiating a DESIGN.md while a system is discoverable.
2. Both skills apply the source-trust hierarchy of 3.2; nothing discovered is emitted confirmed.
3. `designing-consistently` reads a bounded slice (`### Global` + touched module + siblings),
   never the whole Decisions section.
4. Its step 4 gate promotes or demotes every provisional entry the work touched, and escalates to
   `### Global` on the two-or-more-module trigger.
5. `extracting-design-md` elects tokens from reference surfaces; frequency appears only as a
   tie-break, and every unconfirmed token ships `[provisional]`.
6. Both skills emit and read the `### module` / `#### route — page` architecture with `### Global`
   first.
7. A module entry contradicting a global is treated as drift unless it declares
   `exception to Global/<block>`.
8. Zero stale `Context-Engineering` / `context-lint` references remain in either skill.
9. `node ../Agent-Engineering/scripts/agent-lint.mjs .` exits 0.
10. Each skill ships at least 3 evals, and the evals change **before** the skill content (repo
    hard constraint).
11. A DESIGN.md written to this design passes the `agent-lint` design checks unchanged — no
    Agent-Engineering modification of any kind.

## 5. Evidence method (binding order)

1. **Cold baseline first.** Run both skills *as they are today* against the Pegasuz admin and
   record what they actually do. That transcript — not this SPEC's predictions — decides the
   final skill content.
2. Evals encode the observed failures, before skill content changes.
3. The admin is the proving ground for the finished skills.

Predicted baseline failures, recorded here so the run can contradict them: step 1 offers to
instantiate a DESIGN.md; step 4 elects `13px` a token on frequency; neither produces a bounded
slice. If the baseline shows otherwise, the design section above is amended before content moves.

## 6. Verified constraints

- `parseDesignMd` (`Agent-Engineering/scripts/design-md-gen.mjs:46-59`) collects only lines
  starting with `- ` under `## Decisions`; heading depth beneath it is never inspected.
- `agent-lint.mjs:290-307` checks `##` section order and duplicates, and the `- YYYY-MM-DD — `
  prefix on entries. Nothing constrains `###`/`####` nesting.
- Therefore the module architecture, the `### Global` tier and the `[provisional]` marker are all
  lint-clean and generator-safe with **zero** changes to Agent-Engineering. Verified, not assumed.

## 7. Out of scope

- Any change to Agent-Engineering: no frontmatter format change, no `design-md-gen.mjs` change,
  no `agent-lint.mjs` check, no AE version bump, no migration note.
- Any write to the Pegasuz repo. It is read-only evidence for this lane.
- Fixing Pegasuz's own context rot — its admin `AGENTS.md` gotcha #4 cites
  `Documentation/UX-UI-STANDARDS.md` as the current visual line while the owner has disowned that
  document's principles. Recorded here, repaired elsewhere.
- Merging the two skills into one, or adding a third repair skill (approaches B and C, rejected).
- Producing the admin's real DESIGN.md as a deliverable of this lane. The admin is the proving
  ground; whether its file is committed to Pegasuz is a separate decision.

## 8. Definition of done

Requirements 1-11 each demonstrated by a command that exited 0, recorded in `PROGRESS.md`, with
every `feature_list.json` row passing. Baseline transcript archived in the lane before any skill
content changed.
