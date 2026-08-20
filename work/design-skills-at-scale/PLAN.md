# design-skills-at-scale — plan

## Constraints (every step)

- **Evals change BEFORE skill content.** Repo hard constraint (AGENTS.md). Steps 5-6 land
  before steps 7-8; no exception.
- **Baseline before evals.** SPEC §5 binding order: what the current skills actually do decides
  the content, not this plan's predictions.
- **Zero Agent-Engineering changes.** No frontmatter format, no `design-md-gen.mjs`, no
  `agent-lint.mjs` check, no version bump. SPEC §7.
- **Zero writes to Pegasuz.** It is read-only evidence. Referred to below as
  `<PEGASUZ>/Pegasuz-Core/frontend-admin/admin`.
- **Shipped-surface rules.** `skills/` is a shipped surface: no machine-absolute paths (the
  1.4.2 `machine-path` check fires on this repo), SKILL.md under 500 lines, references one level
  deep, third-person descriptions.
- **Runtime-neutral skill text.** No framework, product, or filename from any one repo in
  SKILL.md — Vue, `navConfig.js` and Pegasuz are evidence for us, never text in the skill.
- **Live references only.** The sweep targets paths an agent is told to use.
  `skills/tracing-root-causes/SKILL.md:9` is a historical provenance citation and stays verbatim.

## Steps

- [ ] 1. Lane init: `PROGRESS.md` + `DECISIONS.md` + `feature_list.json` with one row per SPEC §4
      requirement (11 rows, `state: not_started`, each `verification` naming the command that
      will prove it) — *mechanical* — accept: `node ../Agent-Engineering/scripts/agent-lint.mjs .`
      exits 0

- [ ] 2. Cold baseline of `designing-consistently`: run it as it stands today against
      `<PEGASUZ>/Pegasuz-Core/frontend-admin/admin`, recording what it does at each of its 5
      steps, verbatim, into `work/design-skills-at-scale/baseline-designing-consistently.md`
      under `## Step N` headings — *judgment* — accept:
      `test $(grep -c '^## Step ' work/design-skills-at-scale/baseline-designing-consistently.md) -eq 5`

- [ ] 3. Cold baseline of `extracting-design-md`: same method, its 7 steps, into
      `work/design-skills-at-scale/baseline-extracting-design-md.md` — *judgment* — accept:
      `test $(grep -c '^## Step ' work/design-skills-at-scale/baseline-extracting-design-md.md) -eq 7`

- [ ] 4. Reconcile the baselines against SPEC §5's three recorded predictions (step 1 offers to
      instantiate; step 4 elects 13px on frequency; no bounded slice): one dated ruling per
      prediction in `DECISIONS.md` — confirmed, contradicted, or unobserved. A contradicted
      prediction amends SPEC §3 in the same step, before any eval is written — *judgment* —
      accept: `test $(grep -c '^- 2026-' work/design-skills-at-scale/DECISIONS.md) -ge 3`

- [ ] 5. Evals for `designing-consistently`, written from the step-2 baseline and the step-4
      rulings (not from this plan): 2 new — discovery-without-DESIGN.md, and the bounded slice at
      157 surfaces — plus `eval-01.md` rewritten for the promote/demote repair gate —
      *judgment* — accept: `test $(ls skills/designing-consistently/evals/*.md | wc -l) -eq 5`
      and `node ../Agent-Engineering/scripts/agent-lint.mjs .` exits 0

- [ ] 6. Evals for `extracting-design-md`, from the step-3 baseline and the step-4 rulings: 2 new
      — reference surfaces beat frequency, and provisional-by-default output — plus `eval-01.md`
      rewritten so its drift report no longer treats the frequent variant as the token —
      *judgment* — accept: `test $(ls skills/extracting-design-md/evals/*.md | wc -l) -eq 6`
      and `node ../Agent-Engineering/scripts/agent-lint.mjs .` exits 0

- [ ] 7. Rewrite `skills/designing-consistently/SKILL.md` to pass the 5 evals from step 5:
      step 1 becomes discovery with SPEC §3.2's source-trust hierarchy, step 2 becomes the
      bounded slice (`### Global` + touched module + siblings), step 4 becomes the record+repair
      gate (status promote/demote, scope escalate/narrow), plus the red flag "I found a design
      doc, so I know the system" — *judgment* — accept:
      `node ../Agent-Engineering/scripts/agent-lint.mjs .` exits 0 and
      `test $(wc -l < skills/designing-consistently/SKILL.md) -lt 500`

- [ ] 8. Rewrite `skills/extracting-design-md/SKILL.md` to pass the 6 evals from step 6:
      reference surfaces replace frequency in step 4, output ships `[provisional]` by default,
      steps 5-6 emit the `### Global` + module architecture step 7 of this plan taught
      `designing-consistently` to read — *judgment* — accept:
      `node ../Agent-Engineering/scripts/agent-lint.mjs .` exits 0 and
      `test $(wc -l < skills/extracting-design-md/SKILL.md) -lt 500`

- [ ] 9. `[batch]` Sweep live stale references and refresh the two skill rows in `README.md` to
      match the new frontmatter descriptions: `skills/designing-consistently/SKILL.md`,
      `skills/extracting-design-md/SKILL.md`, `skills/extracting-design-md/evals/eval-03.md`,
      `README.md`. `skills/tracing-root-causes/SKILL.md:9` is excluded by the constraints block —
      *mechanical* — accept:
      `! git grep -n 'Context-Engineering\|context-lint' -- skills/designing-consistently skills/extracting-design-md README.md`

- [ ] 10. Write a DESIGN.md for the admin to the approved shape into
      `work/design-skills-at-scale/proving-run/DESIGN.md` (lane-local, never written to Pegasuz),
      then prove SPEC §4 requirement 11 against it — *integration* — accept:
      `node ../Agent-Engineering/scripts/design-md-gen.mjs work/design-skills-at-scale/proving-run/DESIGN.md --target cssvars`
      exits 0 and `node ../Agent-Engineering/scripts/agent-lint.mjs .` exits 0

- [ ] 11. `work-verify` pass over SPEC §4 requirements 1-11; every `feature_list.json` row moves
      to `passing` with its command evidence recorded in `PROGRESS.md` — *integration* — accept:
      `node ../Agent-Engineering/scripts/agent-lint.mjs .` exits 0 and no row remains
      `not_started` or `active`

## Interfaces between steps

- Step 4 consumes the two baseline files by name from steps 2 and 3, and writes rulings into
  `DECISIONS.md`. A ruling that contradicts a prediction amends `SPEC.md` §3 — the only step
  permitted to edit the SPEC after approval, and only with the ruling recorded.
- Steps 5 and 6 consume `DECISIONS.md` rulings plus their own baseline file. They do NOT consume
  this plan's descriptions of the failures — those are predictions, superseded by step 4.
- Steps 7 and 8 consume the eval files produced by steps 5 and 6 as their acceptance target: the
  skill is rewritten to pass those evals, not to match SPEC §3 prose.
- Step 8 emits the `### Global` + `### <module>` / `#### <route> — <page>` architecture that
  step 7's slice rule reads. The two must agree on that exact heading shape; step 8 changes it
  only by also changing step 7.
- Step 10 consumes the finished step-8 skill to produce the file, and the finished step-7 skill
  to read it back.
