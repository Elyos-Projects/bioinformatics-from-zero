# TASKS — bioinformatics-from-zero

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-29 · Owner: TBD (maintainer) · Lane: donated

> Binding cancer guardrails apply to every task: **open / aggregate / de-identified data only**;
> controlled-access and identifiable patient data are out of scope; per-source license verification;
> provenance on every claim; education-only, "not medical advice." See PLAN.md §7.

## How these tasks map to Hee-Lee Oss

Each task below becomes a Hee-Lee Oss **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug id from the tables, e.g. `bioinfo-zero-gate-001`.
- `title` — the table's Title.
- `project` — `bioinformatics-from-zero`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (per table).
- `lane` — `donated` for all tasks here (no funded escrow). A funded task would add `fundedBudgetUsd`.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["cancer","bioinformatics","education","open-data"]`.
- `riskTier` — `low | medium | high`. **low** for programming/stats on open data; **medium** for
  clinical/survival-interpretation and cancer-biology claims (domain review); **high** patient-facing
  content is out of scope here.
- `urgent` — boolean; `false` for all current tasks.
- `deliverable` — `pr | dataset | document | translation`. Lessons/specs/guides → `document`;
  tooling/CI/harness → `pr`. We never deliver `dataset` (data is out of scope; we describe/fetch it).
- `tokenEstimate` — `small | medium | large` (the Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — **TO BE SECURED** until an adopting educator/program is confirmed.
- `verifiedNeed` — **`false`** for every task until a named partner agrees to adopt/review (general
  need is real; per-task delivery need is unproven — "delivered, not merged").
- `outputLicense` — `CC-BY-4.0` for lesson/content/spec documents; `MIT` for code/tooling/CI.

**Reviewer roles** referenced below: **Maintainer**, **License+Data-Ethics** (blocking dataset gate),
**Technical**, **Pedagogy**, **Domain** (oncology-aware; required for medium tasks).

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| bioinfo-zero-gate-001 | Dataset license + de-identification gate (schema + blocking policy) | design-spec | small | medium | document | — | License+Data-Ethics |
| bioinfo-zero-reviewer-002 | Name/secure the License+Data-Ethics and Domain reviewer roles | research | small | low | document | — | Maintainer |
| bioinfo-zero-catalog-003 | Verified-open dataset catalogue (`datasets.yml`) seeded with 3 sources | data | medium | medium | document | gate-001 | License+Data-Ethics |
| bioinfo-zero-ncpolicy-004 | NC / share-alike vs CC-BY acceptance policy | design-spec | small | medium | document | gate-001 | License+Data-Ethics |
| bioinfo-zero-skeleton-005 | Curriculum skeleton + per-module learning outcomes + per-lesson objectives/prereq schema | writing | small | low | document | — | Pedagogy |
| bioinfo-zero-harness-006 | Reproducibility harness: renv + conda/pixi locks + Quarto dual-track render | code | medium | low | pr | — | Technical |
| bioinfo-zero-ci-007 | CI: execute every lesson + dataset-gate audit + objectives/DAG lint + a11y/parity + zero-install fallback | code | medium | low | pr | harness-006, gate-001, skeleton-005, zeroinstall-012 | Technical |
| bioinfo-zero-a11y-008 | Accessibility (WCAG 2.2 AA) + plain-language authoring checklist | design-spec | small | low | document | — | Pedagogy |
| bioinfo-zero-outreach-009 | Partner/educator outreach + adopting-program shortlist | research | small | low | document | — | Maintainer |
| bioinfo-zero-pilot-010 | Pilot lesson end-to-end (setup / tiny tabular) through full gate chain | writing | medium | low | document | gate-001, catalog-003, skeleton-005, harness-006, ci-007, a11y-008 | Technical, Pedagogy |
| bioinfo-zero-gapmap-011 | Prior-art delta / gap-map (vs bioc-intro, HBC DGE, Data Carpentry Genomics) + reuse/fork rulings | research | small | low | document | — | Pedagogy, Maintainer |
| bioinfo-zero-zeroinstall-012 | CI-tested zero-install browser fallback (Binder/repo2docker + fallback) with MB/RAM/cold-load budgets | code | medium | low | pr | harness-006 | Technical |
| bioinfo-zero-objdag-013 | Learning-objectives + prerequisite-DAG front-matter schema + CI lint | design-spec | small | low | pr | skeleton-005 | Pedagogy, Technical |

**Acceptance criteria — key tasks**

- **gate-001 (dataset gate)**
  - [ ] Defines the `datasets.yml` record schema: `id, source, url, license{id,url,snapshotRef},
        permitsReuse, accessTier(open|aggregate|de-identified), deIdentified, provenance, sha256,
        fetch, attribution`.
  - [ ] Objective pass rule: a dataset is usable only if `permitsReuse: true` (evidence-backed),
        `accessTier ∈ {open, aggregate, de-identified}`, and (if individual-level) `deIdentified: true`
        with a stated basis. Missing/unparseable evidence = **EXCLUDE** (no default-allow).
  - [ ] Explicitly excludes controlled-access (dbGaP/EGA/individual biobanks) and identifiable data;
        curriculum must never teach how to *obtain* such data.
  - [ ] Gate is machine-checkable so CI (ci-007) can fail a lesson referencing a missing/failed id.
  - [ ] Output licensed CC-BY-4.0.

- **catalog-003 (`datasets.yml` seed)**
  - [ ] At least 3 candidate sources fully recorded and each PASSING the gate (e.g. a Bioconductor
        teaching expression set, DepMap, a SEER*Explorer/GLOBOCAN aggregate) — or flagged/excluded
        with reason if they fail.
  - [ ] **Provisional per-source rulings recorded:** TCGA open tier = effectively public-domain (subset
        vendorable, attribution norms apply); **DepMap/CCLE = fetch-by-script, never vendored** (CC BY but
        murky redistribution); GEO/recount3 = per-series check; SEER/GLOBOCAN = confirm
        aggregate-redistributable vs reference-only. **NC sources are not claimed as open reuse and no
        NC-derived material is republished under CC-BY.**
  - [ ] Each record has a license snapshot (committed text + SHA-256 + archival URL) and a
        vendor-vs-fetch decision recorded.
  - [ ] No record has `accessTier` outside the permitted enum; no individual-level record without
        `deIdentified: true` + basis.
  - [ ] Reviewed and signed off by License+Data-Ethics.

- **harness-006 (reproducibility harness)**
  - [ ] `renv.lock` (R) and `environment.yml`/`pixi.lock` (Python) build clean in a fresh runner.
  - [ ] Quarto renders a dual-track (R + Python) example from one source; outputs deterministic.
  - [ ] Documented minimum specs; the **zero-install fallback** is built by zeroinstall-012, not a vague mention.

- **gapmap-011 (prior-art delta / gap-map)**
  - [ ] One-page delta vs `carpentries-incubator/bioc-intro`, Harvard Chan `hbctraining` DGE, and Data
        Carpentry Genomics: what each covers, what they assume, and where this curriculum is net-new
        (from-zero/no-biology-assumed, cancer-specific, dual R+Python, reproducible-by-construction).
  - [ ] Records, per foundation episode, a **fork-and-adapt (CC-BY) vs author-fresh** ruling to avoid
        duplication and honour OER licensing.

- **zeroinstall-012 (zero-install browser fallback)**
  - [ ] A learner can run a lesson with **no local install** via Binder/`repo2docker` (primary), with a
        Colab/Posit Cloud/Codespaces fallback; `webR`/JupyterLite evaluated for install-free R/Python.
  - [ ] **CI tests the fallback path** (not just the lockfile build) against declared **download-MB /
        RAM / cold-load-time budgets**; no single hosted service is a hard dependency.

- **objdag-013 (objectives + prerequisite-DAG lint)**
  - [ ] Defines a CI-checkable `.qmd` front-matter block: measurable learning objectives ("learner will
        be able to…"), declared entry-skill floor, and prerequisite lesson ids.
  - [ ] CI **fails** any lesson missing the block or whose prerequisites violate the committed concept
        DAG; the DAG is published as the learner map.

- **pilot-010 (pilot lesson)**
  - [ ] A complete from-zero lesson executes clean in CI in both tracks from lockfiles **and** runs via
        the zero-install fallback.
  - [ ] Uses only datasets passing the gate; every factual claim cited; attribution surfaced.
  - [ ] Carries the **objectives + prerequisite front-matter block** (passes objdag-013 lint); passes the
        WCAG 2.2 AA accessibility checklist; learning objectives stated; exercise + solution + an
        auto-checkable formative check included.
  - [ ] Carries correct license; demonstrates the entire gate chain works end-to-end.

**Definition of Done (M0):** dataset gate live and CI-enforced; `datasets.yml` seeded with ≥ 3 passing
sources (with provisional per-source rulings); reviewer roles named or explicitly TO BE SECURED;
lockfiles + Quarto dual-track render build clean in CI; **zero-install browser fallback CI-tested against
MB/RAM/cold-load budgets**; **objectives + prerequisite-DAG front-matter lint live in CI**; **prior-art
gap-map drafted**; one pilot lesson passes the full gate chain; outreach shortlist drafted.

---

## Milestone M1 — Core data skills

> **Parity decision (resolves Open Question on parity):** M1 foundations are authored **dual R+Python**
> (selective-parity policy: advanced genomics in M2 ships single-track-first). Every M1 lesson carries
> the objectives + prerequisite front-matter block and an **auto-checkable formative check** — the
> lightweight auto-checker lands **in M1, not post-MVP**.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| bioinfo-zero-autocheck-100 | Lightweight auto-checker (expected-output/answer checks) wired into exercises | code | medium | low | pr | ci-007 | Technical |
| bioinfo-zero-setup-101 | Module: environment setup from zero (terminal, R/Python, notebooks) | writing | medium | low | document | pilot-010 | Technical, Pedagogy |
| bioinfo-zero-tabular-102 | Module: tabular data wrangling (clinical metadata, tidyverse/pandas) | writing | large | low | document | setup-101 | Technical, Pedagogy |
| bioinfo-zero-viz-103 | Module: data visualisation for cancer data | writing | medium | low | document | tabular-102 | Technical, Pedagogy |
| bioinfo-zero-stats-104 | Module: statistics foundations + statistical humility (multiple testing) | writing | large | medium | document | tabular-102 | Technical, Pedagogy, Domain |

**Acceptance criteria — key tasks**

- **tabular-102 (tabular wrangling)**
  - [ ] Both R and Python tracks; CI parity check passes; executes clean from lockfiles.
  - [ ] Uses a gated, de-identified clinical-metadata teaching subset; no identifiable fields.
  - [ ] Exercises with solutions; learning outcomes met; accessibility passed.

- **stats-104 (statistics foundations)**
  - [ ] Teaches distributions, hypothesis testing, p-values, **multiple-testing correction**,
        confounding, and "correlation ≠ causation," with reproducible examples.
  - [ ] Domain review confirms framing is education-only and statistically sound; "not medical advice"
        disclaimer present where any clinical example appears.
  - [ ] Every claim cited; both tracks; CI green.

**Definition of Done (M1):** all M1 lessons execute clean in CI (both tracks, parity verified) and via
the zero-install fallback; objectives + prerequisite front-matter present (DAG lint green);
auto-checkable formative checks present; technical + pedagogy (+ domain for stats-104) review approved;
a beginner test-reader completes the module unaided; accessibility (WCAG 2.2 AA) passed; datasets all
gated.

---

## Milestone M2 — Cancer-genomics basics

> **Parity decision:** M2 advanced-genomics lessons ship **single-track-first (R-primary, given
> Bioconductor)** with a Python-parity backlog item — per the selective-parity policy that keeps the
> project deliverable. Survival viz uses **`ggsurvfit`** (preferred over `survminer`).

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| bioinfo-zero-expr-201 | Module: gene-expression data + normalisation basics | writing | large | medium | document | stats-104, catalog-003 | Technical, Pedagogy, Domain |
| bioinfo-zero-de-202 | Module: differential expression (limma/DESeq2 / statsmodels) | writing | large | medium | document | expr-201 | Technical, Pedagogy, Domain |
| bioinfo-zero-survival-203 | Module: survival analysis (Kaplan–Meier, Cox) — education-only | writing | large | medium | document | stats-104, catalog-003 | Technical, Pedagogy, Domain |

**Acceptance criteria — key tasks**

- **de-202 (differential expression)**
  - [ ] Uses a gated, open/de-identified expression dataset; provenance + attribution surfaced.
  - [ ] Correctly teaches normalisation, model design, and **multiple-testing control** (FDR); warns
        against common pitfalls (batch effects, p-hacking).
  - [ ] Both tracks; CI green; every biological claim cited; domain review approved.

- **survival-203 (survival analysis)**
  - [ ] Education-only; standing "**not medical advice**" disclaimer prominent; no prognostic guidance
        framed as applicable to an individual.
  - [ ] Uses de-identified/aggregate outcome data passing the gate; teaches censoring, KM curves,
        hazard ratios, and their limits.
  - [ ] Domain (oncology-aware) review **required and obtained** before done; both tracks; CI green.

**Definition of Done (M2):** every dataset passes the gate; survival/interpretation lessons carry the
disclaimer and have obtained domain review; sources cited on every biological claim; CI green in both
tracks. **Blocked until the Domain reviewer (reviewer-002) is secured.**

---

## Milestone M3 — Synthesis, capstone & adoption

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| bioinfo-zero-cluster-301 | Module: dimensionality reduction & clustering (PCA, UMAP, k-means) | writing | medium | low | document | expr-201 | Technical, Pedagogy |
| bioinfo-zero-capstone-302 | Capstone: reproduce a small published-style analysis from zero + research ethics | writing | large | medium | document | de-202, survival-203, cluster-301 | Technical, Pedagogy, Domain |
| bioinfo-zero-instructor-303 | Instructor guide + glossary + translation-readiness pass | writing | medium | low | document | capstone-302 | Pedagogy |

**Acceptance criteria — key tasks**

- **capstone-302 (capstone)**
  - [ ] An external learner can reproduce the full analysis from a clean environment using only the
        curriculum and gated datasets; CI executes it end-to-end.
  - [ ] Includes a **research-ethics + reproducibility** section (provenance, licensing, limits of
        de-identified/aggregate data, "not medical advice").
  - [ ] Domain review confirms biological/clinical statements; both tracks; every claim cited.

- **instructor-303 (instructor guide)**
  - [ ] Maps modules to learning outcomes, timings, and prerequisites; glossary complete.
  - [ ] Translation-readiness pass: plain language, externalised structure, i18n notes (actual
        translation deferred; patient-facing translation flagged out-of-scope/high-risk).

**Definition of Done (M3):** capstone reproducible by an external learner; instructor guide + glossary
complete; translation-readiness pass done; ≥ 1 named educator/program has reviewed (target: adopted)
≥ 1 module — the "delivered" gate.

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| bioinfo-zero-autograde-401 | Autograder depth: mastery checks + diagnostic pre-test | code | medium | low | pr | Basic output-check auto-checker already lands in M1 (autocheck-100); this adds mastery/diagnostic depth |
| bioinfo-zero-i18n-402 | First translation track (non-patient-facing content) + number/date/unit localization | writing | large | medium | translation | Per-language reviewer; localize formats/units |
| bioinfo-zero-video-403 | Captioned video walkthroughs of key modules | writing | medium | low | document | Reuse `caption-commons` pattern |
| bioinfo-zero-dataset-404 | Add 3 more gated teaching datasets to `datasets.yml` | data | medium | medium | document | Each via the gate |
| bioinfo-zero-maint-405 | Scheduled CI re-run + dependency version-bump + re-verification | maintenance | small | low | pr | Recurring; catches lesson rot |
| bioinfo-zero-singlecell-406 | Optional advanced module: intro single-cell (open data) | writing | large | medium | document | Only after core arc proven |
| bioinfo-zero-pyparity-407 | Python-parity backfill for the advanced genomics (M2) track | writing | large | medium | document | Selective-parity follow-through after R-primary M2 ships |
| bioinfo-zero-glittr-408 | List the curriculum on glittr.org + frame as a general-topic curriculum | maintenance | small | low | document | Discoverability; adds verifiable capstone-PR outcome signal |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task (`bioinfo-zero-gate-001`). Donated lane, so no
`fundedBudgetUsd`. `verifiedNeed` is honestly `false` (no adopting partner secured yet).

```json
{
  "id": "bioinfo-zero-gate-001",
  "title": "Dataset license + de-identification gate (schema + blocking policy)",
  "project": "bioinformatics-from-zero",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["cancer", "bioinformatics", "education", "open-data", "data-ethics"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "bioinformatics-from-zero is an open, from-zero curriculum for cancer data analysis (R/Python) built only on open/aggregate/de-identified data. Per the binding Track-8 cancer guardrails, no dataset may appear in a lesson until its license, provenance, and de-identification/aggregate status are verified. This task defines the gate that every later dataset and lesson depends on.",
  "objective": "Specify a machine-checkable dataset gate: the datasets.yml record schema and the pass/exclude policy that CI enforces, so controlled-access and identifiable data can never enter a lesson and every reuse decision is evidence-backed.",
  "acceptanceCriteria": [
    "Defines the datasets.yml record schema: id, source, url, license{id,url,snapshotRef}, permitsReuse, accessTier(open|aggregate|de-identified), deIdentified, provenance, sha256, fetch, attribution.",
    "Pass rule: usable only if permitsReuse=true (evidence-backed), accessTier in {open,aggregate,de-identified}, and individual-level data has deIdentified=true with a stated basis; missing/unparseable evidence => EXCLUDE (no default-allow).",
    "Explicitly excludes controlled-access (dbGaP, EGA, individual-level biobanks) and identifiable patient data; the curriculum must never teach how to obtain such data.",
    "Requires a license text snapshot (committed text + SHA-256 + archival URL) and a recorded vendor-vs-fetch decision per dataset.",
    "Gate is machine-checkable so CI can fail any lesson referencing a missing or failing dataset id."
  ],
  "resources": [
    "C:/code/hee-lee-oss/planning/ROADMAP.md (Track 8 cancer guardrails)",
    "C:/code/hee-lee-oss/planning/projects/open-data-datasheets/TASKS.md (license-gate pattern to reuse)",
    "C:/code/hee-lee-oss/docs/good-deed-definition.md"
  ],
  "output": "A CC-BY-4.0 design-spec document defining the datasets.yml schema and the blocking license/de-identification gate policy, ready for CI enforcement.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Task count summary

- **M0 — Foundation & cold-start:** 13 tasks (added gapmap-011, zeroinstall-012, objdag-013)
- **M1 — Core data skills:** 5 tasks (added autocheck-100)
- **M2 — Cancer-genomics basics:** 3 tasks
- **M3 — Synthesis, capstone & adoption:** 3 tasks
- **Backlog / future:** 8 tasks (added pyparity-407, glittr-408)
- **Total:** 24 scheduled + 8 backlog. All `lane: donated`, `verifiedNeed: false`, `urgent: false`
  until a partner/reviewer is secured.

---

## Generated task index

Every table row above now has a corresponding executable `tasks/<id>.json` (validated against the
Hee-Lee Oss taskSchema; the Hee-Lee Oss CLI executes these JSON files, not this Markdown). The seed
`bioinfo-zero-gate-001.json` was already present; the remaining 25 were generated by decomposing the
milestone tables one-to-one (no fan-out: each table row is already a concrete task, and no row
enumerates a bounded dimension — e.g. languages/datasets — to fan out across). Field policy follows
"How these tasks map to Hee-Lee Oss": `lane: donated`, `verifiedNeed: false`, `urgent: false`,
`requestor: TO BE SECURED`, `status: open`; `outputLicense` = `CC-BY-4.0` for content/spec documents
and `MIT` for code/tooling/CI.

Guardrail notes carried into the JSON `context`/`acceptanceCriteria`:
- M2 cancer-genomics + `stats-104` are `riskTier: medium` and require oncology-aware **Domain** review;
  `expr-201`/`de-202`/`survival-203`/`singlecell-406`/`capstone-302` note they are blocked until the
  Domain reviewer (`reviewer-002`) is secured.
- `survival-203` (and any clinical-interpretation content) is **education-only**, carries a standing
  "not medical advice" disclaimer, and gives no individual prognostic guidance — no task authors
  patient-facing medical advice.
- `i18n-402` is a **representative** translation task only: the target language and per-language
  reviewer are "TO BE SECURED"; concrete per-language tasks expand on confirmation (no fabricated
  languages). Patient-facing translation is flagged out-of-scope/high-risk.

| Milestone | Task ids |
| --- | --- |
| M0 | `bioinfo-zero-gate-001` (seed), `bioinfo-zero-reviewer-002`, `bioinfo-zero-catalog-003`, `bioinfo-zero-ncpolicy-004`, `bioinfo-zero-skeleton-005`, `bioinfo-zero-harness-006`, `bioinfo-zero-ci-007`, `bioinfo-zero-a11y-008`, `bioinfo-zero-outreach-009`, `bioinfo-zero-pilot-010` |
| M1 | `bioinfo-zero-setup-101`, `bioinfo-zero-tabular-102`, `bioinfo-zero-viz-103`, `bioinfo-zero-stats-104` |
| M2 | `bioinfo-zero-expr-201`, `bioinfo-zero-de-202`, `bioinfo-zero-survival-203` |
| M3 | `bioinfo-zero-cluster-301`, `bioinfo-zero-capstone-302`, `bioinfo-zero-instructor-303` |
| Backlog | `bioinfo-zero-autograde-401`, `bioinfo-zero-i18n-402`, `bioinfo-zero-video-403`, `bioinfo-zero-dataset-404`, `bioinfo-zero-maint-405`, `bioinfo-zero-singlecell-406` |

Total: **26** executable task files (1 pre-existing seed + 25 generated).
