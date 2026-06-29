# PLAN — bioinformatics-from-zero

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated · Risk tier: low (with medium/high exceptions, see §8)

> **Binding cancer guardrails (read §7 first).** This curriculum may use **only open-access,
> aggregate, or de-identified** data. Controlled-access genomics (dbGaP, EGA, individual-level
> biobanks) and any identifiable patient data are **out of scope**. Every dataset passes a
> per-source license + de-identification gate before it appears in a lesson. The curriculum is
> **education-only**, is **not medical advice**, and any lesson that interprets clinical or
> survival data carries a standing disclaimer and oncologist/patient-advocate review.

---

## Executive summary

Cancer research generates enormous volumes of openly available data — gene expression matrices,
de-identified clinical tables, aggregate incidence statistics, cell-line dependency maps — yet the
skills to load, clean, analyse, and *correctly interpret* that data are concentrated in a small
number of well-resourced institutions. Aspiring analysts, patient-advocate researchers, biology
students, career-changers, and scientists in low-resource settings repeatedly hit the same wall:
the existing material assumes either prior programming ability, prior statistics, paid software, or
access to data behind agreements they cannot sign. There is no single, free, reproducible,
**from-absolute-zero** path that takes a motivated beginner from "I have never opened a terminal"
to "I can reproduce a published cancer-data analysis and explain why each step is correct."

`bioinformatics-from-zero` produces exactly that path: an **open intro curriculum for cancer data
analysis** in both **R and Python**, built entirely on **open / aggregate / de-identified**
datasets. The deliverable is a self-contained set of versioned, executable lessons — narrative
Markdown plus runnable notebooks — organised into modules that progress from environment setup and
data literacy, through tabular wrangling, statistics, and visualisation, to genuine cancer-genomics
workflows (differential expression, survival analysis, clustering) and a reproducibility/ethics
capstone. Each lesson runs end-to-end in continuous integration from a pinned, declared environment,
so "it works on my machine" is never the answer a learner gets.

The **deliverable is teaching material, not data and not a tool a patient would consult.** We do not
republish patient records, we do not mirror controlled-access data, and we do not produce clinical
guidance. The central, non-negotiable design constraint is the **dataset gate**: before any dataset
is used in a lesson, its license must be verified to permit educational reuse and redistribution of
the small teaching subset, and it must be confirmed open/aggregate/de-identified. Anything with
unclear terms, individual-level identifiability, or a non-commercial/share-alike clause that the
lesson cannot honour is flagged and excluded rather than guessed at.

Risk tier is **low** for the bulk of the curriculum (programming, statistics, visualisation on open
data). Two categories carry elevated review: lessons that **interpret clinical or survival outcomes**
are **medium** (oncologist-aware review, "not medical advice" framing); any **patient-facing** spin-off
material would be **high** and is explicitly out of scope here. The plan front-loads the dataset gate,
the reviewer roles, and the reproducibility harness so that correctness — factual, statistical, legal,
and ethical — is enforced by process, not hoped for.

---

## Problem & beneficiaries

**Who is helped.**
- **Aspiring data analysts / career-changers** who want to enter cancer bioinformatics but cannot
  afford bootcamps or paid tools, and find existing tutorials assume too much.
- **Biology / pre-med / nursing students and wet-lab scientists** who can read the biology but have
  never programmed, and need a ramp that starts at zero.
- **Patient-advocate researchers** (e.g. rare-cancer foundations, parent-led research groups) who
  want to read and re-run the analyses behind the papers that affect their community — this directly
  feeds Elyos's cancer track (`patient-advocate-research-primer`, `oncology-data-literacy`).
- **Educators** in under-resourced universities and community programs who need a free,
  reproducible, adoptable curriculum they can teach without licensing closed software.
- **Contributors to other Elyos cancer projects** — the curriculum is the on-ramp that turns a
  newcomer into someone who can responsibly work `ewing-expression-reanalysis`,
  `cancer-dataset-datasheets`, `oncogene-knowledge-graph`, and similar.

**The verified need.** The *general* need is well established: the gap between freely available
cancer data and the freely available skills to use it is widely documented in training literature
and is the explicit motivation for efforts like The Carpentries, Bioconductor training, and Galaxy
Training. We treat the general need as real. However, the **specific, per-partner need is TO BE
SECURED**: no named educator, advocacy training program, or training organisation has yet agreed to
*adopt and review* this curriculum. Under Elyos's "delivered, not merged" bar, output must be
accepted and used by a real beneficiary — so until a named partner confirms adoption/review, tasks
carry `verifiedNeed: false`. This is deliberate honesty, not pessimism.

**Partner org.** TO BE SECURED. Candidate channels (no commitment assumed): The Carpentries /
Data Carpentry lesson incubator, Bioconductor and Galaxy training communities, rare-cancer
foundations with research-literacy programs, university intro-bioinformatics courses seeking OER,
and citizen-science/advocate networks. M0 includes explicit partner-outreach and reviewer-securing
work; no partner or reviewer is assumed to exist.

---

## Goals and non-goals

**Goals**
- Produce a **complete from-zero learning path** (R and Python) that a motivated beginner with no
  programming background can follow to reproduce a real cancer-data analysis and explain each step.
- Make every lesson **reproducible by construction**: pinned environments, executed in CI from a
  clean machine, deterministic outputs, no hidden state.
- Build all content on a **verified-open dataset catalogue** with a machine-readable license +
  provenance + de-identification record for every source.
- Teach **correct interpretation and statistical humility** — multiple-testing, confounding,
  survivorship, "correlation is not causation," and the limits of what aggregate/de-identified data
  can tell you — not just code that runs.
- Make the curriculum **adoptable and remixable**: clear learning outcomes, instructor notes,
  exercises with solutions, CC-BY-licensed, and structured for translation/i18n later.
- Be **accessible**: works on low-spec hardware and free cloud notebooks; meets basic content
  accessibility (alt text, contrast, readable structure).

**Non-goals**
- **Not** a research platform, analysis tool, or pipeline product. We teach; we do not ship a
  service a learner or clinician operates in production.
- **Not** patient-facing material and **not** medical, diagnostic, or treatment guidance of any kind.
- **Not** a genomics methods reference or a graduate course — it is *intro*, from zero; depth beyond
  the intro arc is explicitly deferred/linked, not authored here.
- **Not** a data repository — we do not mirror or republish datasets; we vendor only tiny teaching
  subsets where the license clearly permits it, otherwise we fetch-by-script with provenance.
- **Not** tied to any paid/closed software, cloud vendor, or for-profit platform.
- **Not** a route to controlled-access or identifiable data — that path is never taught here.

---

## Success metrics (outcomes)

Outcome-based, beneficiary-centric. Baselines are 0 because the project is greenfield.

| Outcome | Baseline | Target (12 mo) | How measured |
|---|---|---|---|
| Learners who complete the from-zero arc and reproduce the capstone analysis unaided | 0 | ≥ 50 self-reported completions with a working capstone artifact | Optional completion survey + public "I did it" capstone gallery (opt-in, no PII) |
| Independent reproducibility | n/a | 100% of lessons execute clean in CI on every release; ≥ 3 external learners confirm clean local runs | CI logs + reproducibility reports filed by reviewers/learners |
| Educator / program adoption | 0 | ≥ 2 named programs adopt ≥ 1 module (the "delivered" gate) | Written confirmation from adopting educator/program |
| Advocate-researcher enablement | 0 | ≥ 5 patient-advocate researchers report being able to re-run a paper-relevant analysis | Structured testimonial (opt-in) |
| Pipeline into other Elyos cancer projects | 0 | ≥ 5 learners go on to open/contribute a task in another cancer-track project | Cross-link from contributor PRs |
| Dataset-gate integrity | n/a | 0 datasets used without a passing license + de-identification record | Audit of `datasets.yml` vs. lessons in CI |
| Accessibility conformance | n/a | 100% of published lessons pass the accessibility checklist | Automated + manual a11y review |

Explicitly **not** primary metrics: page views, stars, "learners enrolled," or notebooks produced —
these are vanity signals and are excluded from the definition of success.

---

## Scope

**In scope**
- A modular, sequenced curriculum (M1–M3 content) covering: environment setup; data & cancer-data
  literacy; tabular data wrangling; visualisation; statistics foundations; gene-expression basics;
  differential expression; survival analysis (with elevated review); dimensionality reduction &
  clustering; and a reproducibility + research-ethics capstone.
- **Parallel R and Python tracks** authored from one source where practical (Quarto), with parity
  checks so neither track lags.
- A **verified-open dataset catalogue** (`datasets.yml`) with license, provenance, de-identification
  status, checksum, and fetch instructions for every source.
- A **reproducibility harness**: declared, pinned environments (`renv` for R, conda/pixi for Python)
  and CI that executes every lesson end-to-end from a clean environment.
- Exercises with solutions, instructor notes, a glossary, and learning outcomes per module.
- Accessibility baseline and translation-readiness (externalised strings/structure, i18n notes).

**Out of scope**
- Any controlled-access, individual-level, or identifiable patient data — never fetched, never taught.
- Patient-facing, clinical, diagnostic, prognostic, or treatment content; "what does my result mean"
  guidance of any kind.
- Hosting, mirroring, or redistributing full datasets; building an analysis service or web app.
- Advanced/graduate methods (e.g. variant calling at scale, single-cell deep methods, deep learning
  on pathology images) — linked as "where to go next," not authored here.
- Vendor-specific or paid-tool workflows; anything requiring a license to follow.
- Translations themselves (the *content* is built translation-ready; actual translation is a future
  project, and patient-facing translation would be `high`-risk and out of scope).

---

## Solution approach & architecture

This is a **content + reproducibility-engineering** project, not a software product. The "system" is
the authoring pipeline, the dataset gate, and the CI that guarantees every lesson runs.

**Components**
1. **Curriculum source** — lessons authored as **Quarto** (`.qmd`) documents that interleave
   narrative and executable R/Python, rendered to static HTML + downloadable notebooks. One source
   format, two language tracks, reproducible render.
2. **Dataset catalogue (`datasets.yml`)** — the single source of truth for every dataset used:
   `id`, `source`, `url`, `license` (SPDX-style id + URL + text snapshot ref), `permitsReuse`
   (boolean, evidence-backed), `accessTier` (open | aggregate | de-identified), `deIdentified`
   (boolean + basis), `provenance`, `sha256`, `fetch` (script or vendored-subset path), `attribution`.
   No lesson may reference a dataset absent from, or failing, this catalogue.
3. **Environment lockfiles** — `renv.lock` (R) and `environment.yml`/`pixi.lock` (Python), pinned to
   exact versions; the *only* supported way to reproduce a lesson.
4. **Reproducibility CI** — GitHub Actions that (a) builds both environments from lockfiles in a
   clean runner, (b) executes/renders every lesson, (c) fails on any execution error or undocumented
   network fetch, (d) runs the dataset-gate audit, (e) runs link/accessibility/parity checks.
5. **Assessment layer** — exercises with hidden/solution variants; optional lightweight autograder
   (post-MVP) checking learner outputs against expected results.
6. **Static site** — rendered curriculum published to GitHub Pages (or equivalent), no backend.

**Tech stack.** Quarto; R (tidyverse, Bioconductor: e.g. `limma`/`DESeq2`, `survival`,
`survminer`) and Python (pandas, numpy, matplotlib/seaborn, scikit-learn, `lifelines`,
`statsmodels`); `renv` + conda/pixi for environments; GitHub Actions for CI; MkDocs/Quarto site for
publishing. All MIT (code/tooling) or CC-BY-4.0 (lesson content). No paid or closed dependencies.

**Data model — dataset record (illustrative).**
```yaml
- id: tcga-brca-expr-teaching-subset
  source: "TCGA (open-access tier) via <curated Bioconductor/recount package>"
  url: "https://…"
  license: { id: "see-source", url: "https://…", snapshotRef: "licenses/tcga-open.txt#sha256=…" }
  permitsReuse: true            # evidence-backed; false/unknown => excluded
  accessTier: open              # open | aggregate | de-identified
  deIdentified: true            # with stated basis
  provenance: "Derived, de-identified expression subset; original program: TCGA; processed by <pkg> vX"
  sha256: "…"
  fetch: "scripts/fetch_brca_subset.R"   # or vendored subset path if license permits
  attribution: "Required citation string per source terms"
```

**Key decisions**
- **Quarto single-source, dual-track.** Reduces drift between R and Python and lets CI execute both.
- **Fetch-by-script over vendoring**, except tiny subsets with unambiguous redistribution rights —
  keeps us out of redistribution-license trouble and keeps the repo small and honest about provenance.
- **CI executes everything.** A lesson that does not run in clean CI is not "done."
- **Survival/clinical-interpretation lessons are gated** behind oncologist-aware review and a standing
  "educational, not medical advice" disclaimer (see §8).
- **i18n-ready from the start** (externalised structure, plain language) so translation is cheap later
  — but actual translation is out of scope and, if patient-facing, would be `high`-risk.

---

## Data, licensing & compliance

**This section leads on the binding cancer guardrails and is the project's central control.**

### Cancer data guardrails (binding, non-skippable)
1. **Open / aggregate / de-identified only.** Permitted: open-access tiers (e.g. open expression and
   de-identified clinical tables), **aggregate** statistics (e.g. SEER*Explorer / GLOBOCAN published
   aggregates), curated teaching datasets, cell-line resources (e.g. DepMap), and openly published
   processed data (e.g. recount-style uniformly processed public RNA-seq).
2. **Controlled-access is out of scope, always.** dbGaP, EGA, individual-level biobanks, raw germline
   genomes, or anything requiring a Data Use Agreement / IRB / authorized access is **never** fetched
   or taught. The curriculum never demonstrates *how to obtain* controlled data; it teaches that this
   data exists and why it is gated.
3. **No identifiable patient data.** Any dataset that is individual-level must be confirmed
   de-identified, with the basis recorded. If identifiability is unclear → exclude.
4. **Per-source license verification.** Each dataset's license is verified to permit our specific use
   (educational reuse, and redistribution of any vendored subset). **Non-commercial (NC)** and
   **share-alike** clauses are evaluated explicitly: NC is acceptable for non-commercial educational
   use *if* the lesson and any republished derivative honour it and clearly label it; share-alike is
   acceptable only if we can comply (and we flag any conflict with the CC-BY content license).
   Anything closed, ambiguous, or "academic-only/no-redistribution" → **fetch-by-script, never
   vendored**, or excluded.
5. **Provenance on every assertion.** Every factual or biological claim in a lesson cites a source;
   every dataset carries a provenance trail; every number a learner sees can be traced to its origin.
6. **Education-only, not medical advice.** No lesson gives clinical, diagnostic, prognostic, or
   treatment guidance. Survival/clinical-interpretation lessons carry a standing disclaimer and are
   reviewed by an oncologist-aware reviewer (§8). Patient-facing material is out of scope.

### Candidate sources (all subject to the gate before use)
- **TCGA open-access tier** (de-identified expression + clinical), accessed via curated, openly
  licensed Bioconductor/`recount`-style packages rather than raw portals where possible.
- **DepMap** (cell-line dependency/expression; CC-BY-style terms — verify current version).
- **Bioconductor example datasets** (e.g. classic teaching expression sets; verify each package's
  license, often Artistic-2.0/CC).
- **GEO open series** and **recount3** uniformly processed public RNA-seq (verify per-series terms).
- **SEER*Explorer / GLOBOCAN aggregate statistics** (aggregate only; individual-level SEER requires a
  DUA → out of scope).
- **cBioPortal public studies** (verify per-study source terms; many are TCGA-derived).

Each becomes a row in `datasets.yml` only after passing the gate. The gate is enforced in CI: a
lesson referencing a dataset id that is missing, `permitsReuse: false`, or `accessTier` outside
{open, aggregate, de-identified} **fails the build**.

### License / attribution / provenance model
- **Lesson content:** CC-BY-4.0. **Code/tooling/scripts:** MIT. Both stated in repo and per artifact.
- **Attribution:** every dataset's required citation is surfaced in the lesson that uses it and in a
  consolidated `ATTRIBUTION.md`.
- **License snapshots:** each source license is captured (committed text + SHA-256 + archival URL) so
  the basis of `permitsReuse` is auditable later even if the upstream page changes.

### Privacy / PII stance
We do not handle patient data. We use only de-identified or aggregate data. We collect **no learner
PII**: completion tracking, the capstone gallery, and testimonials are strictly opt-in and contain no
identifying information beyond what a learner volunteers publicly. No analytics that fingerprint users.

---

## Quality, review & risk gates

**Risk tiers within this project**
- **low** — setup, programming, tabular wrangling, visualisation, general statistics on open data.
- **medium** — lessons that **interpret clinical/survival data** or cancer-biology claims (need a
  reviewer with statistics + oncology-aware judgement; "not medical advice" framing enforced).
- **high** — any patient-facing/clinical-guidance material → **out of scope** here; if ever proposed,
  it requires credentialed oncologist + patient-advocate sign-off before merge (per Elyos policy).

**Required review before a lesson is "done"**
1. **Dataset gate (blocking):** every dataset used has a passing `datasets.yml` record (license +
   de-identification + provenance), reviewed by the **License + Data-Ethics reviewer**.
2. **Reproducibility gate (blocking):** lesson executes clean in CI from lockfiles; outputs
   deterministic; R/Python parity confirmed where both tracks exist.
3. **Technical review:** code idiomatic, correct, beginner-appropriate; exercises solvable; solutions
   correct.
4. **Pedagogy review:** learning outcomes met; difficulty ramp sane; plain language; accessible.
5. **Domain/medical review (medium lessons):** oncologist-aware reviewer confirms biological/clinical
   statements are accurate, sourced, and framed as education with the standing disclaimer.

**Definition of Shipped (per lesson):** dataset gate passed · executes clean in CI · technical +
pedagogy review approved · (domain review approved for medium lessons) · accessibility checklist
passed · licensed + attributed · **and** at least one external learner or an adopting educator has
successfully used it (the "delivered, not merged" gate). Until a partner/reviewer is secured, lessons
can reach *review/delivered-pending* but not *done*.

---

## Roadmap & milestones

Phased, with measurable exit criteria. M0 is a thin foundation/cold-start; later phases scale content.

**M0 — Foundation & cold-start.** Stand up the rails before any content.
- *Goal:* dataset gate, reviewer roles, reproducibility harness, curriculum skeleton, and one pilot
  lesson proving the whole pipeline end-to-end.
- *Exit criteria:* `datasets.yml` schema + gate live and enforced in CI; License+Data-Ethics and
  Pedagogy/Domain reviewer roles named or explicitly "TO BE SECURED"; renv + conda/pixi lockfiles
  build clean in CI; Quarto dual-track render works; **one pilot lesson** (setup or a tiny tabular
  lesson) passes the full gate chain; partner-outreach shortlist drafted.

**M1 — Core data skills (no genomics yet).**
- *Goal:* the from-zero programming + data + stats foundation on tiny open teaching data.
- *Scope:* setup module, tabular wrangling, visualisation, statistics foundations (incl.
  multiple-testing intuition).
- *Exit criteria:* all M1 lessons execute clean in CI in both tracks; reviewed (technical + pedagogy);
  a beginner test-reader completes the module unaided; accessibility passed.

**M2 — Cancer-genomics basics.**
- *Goal:* apply the foundation to real (open) cancer data.
- *Scope:* gene-expression data & normalisation; differential expression (`limma`/`DESeq2` /
  `statsmodels`); **survival analysis** (Kaplan–Meier, Cox) — *medium risk, domain-reviewed*.
- *Exit criteria:* every dataset passes the gate; survival/interpretation lessons carry the disclaimer
  and have domain review; CI green; sources cited on every biological claim.

**M3 — Synthesis, capstone & adoption.**
- *Goal:* tie it together and make it adoptable.
- *Scope:* dimensionality reduction & clustering; **reproducibility + research-ethics capstone**
  (learner reproduces a small published-style analysis from zero); instructor guide; glossary;
  translation-readiness pass.
- *Exit criteria:* end-to-end capstone reproducible by an external learner; instructor guide complete;
  ≥ 1 named educator/program has reviewed and (target) adopted ≥ 1 module.

**M4+ (backlog) — Scale.** Autograder, additional datasets/modules, i18n/translation tracks,
captioned video walkthroughs, deeper optional topics.

---

## Work breakdown

The itemized, schema-mapped backlog lives in **TASKS.md**, organised by the milestones above. Each
task maps to an Elyos Task JSON (validated against `packages/schema/src/schemas.ts`), with a stable
`bioinfo-zero-<area>-NNN` id, a size, a risk tier, a deliverable, dependencies, and a named reviewer
role. TASKS.md also carries per-task acceptance criteria for the key tasks, each milestone's
Definition of Done, a sized-but-unscheduled backlog, and one complete example Task JSON.

---

## Governance, roles & stakeholders

- **Maintainer (Owner): TBD.** Accountable for the repo, roadmap, and that every gate is honoured.
- **License + Data-Ethics reviewer: TO BE SECURED (blocking role).** Owns the dataset gate; verifies
  license, de-identification, aggregate/open status, and provenance for every dataset. No dataset
  enters a lesson without this sign-off. This role is **non-skippable**.
- **Technical reviewer(s):** verify code correctness, idiomatic R/Python, reproducibility, parity.
- **Pedagogy reviewer:** verifies learning outcomes, difficulty ramp, plain language, accessibility.
- **Domain / oncology-aware reviewer: TO BE SECURED (required for medium lessons).** Reviews
  biological/clinical statements and survival-analysis framing; enforces "education, not advice."
  For any (out-of-scope) `high` patient-facing material, a **credentialed oncologist + patient
  advocate** sign-off would be mandatory.
- **Steward (last-mile owner): TBD.** Owns the "delivered" gate — secures an adopting educator/program
  and confirms real-world use, not just merge.
- **Partner / requestor: TO BE SECURED.** Adopting educator, advocacy training program, or training org.
- **Contributors:** donated-lane AI sessions + human authors; all work passes the gates above.

Reviewer rotation and conflict-of-interest follow Elyos governance; medium-risk sign-off may not be
self-certified by the lesson's author.

---

## Dependencies & integrations

- **Elyos core:** task schema (`packages/schema`), CLI workspace/PR flow, registry entry.
- **Open data sources** (each gated): TCGA open tier via curated packages, DepMap, Bioconductor
  example datasets, GEO/recount3, SEER*Explorer/GLOBOCAN aggregates, cBioPortal public studies.
- **Tooling:** Quarto, R + Bioconductor, Python scientific stack, `renv`, conda/pixi, GitHub Actions,
  GitHub Pages.
- **Sibling Elyos cancer projects** (consumers/feeders): `oncology-data-literacy`,
  `open-teaching-datasets`, `reproducibility-curriculum`, `patient-advocate-research-primer`,
  `cancer-data-dictionaries`, `cancer-dataset-datasheets`. We reuse `open-teaching-datasets` outputs
  where available rather than re-curating.
- **External communities** (outreach targets, no commitment): The Carpentries, Bioconductor/Galaxy
  training, rare-cancer foundations.

---

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| A dataset turns out to be controlled-access or identifiable | Medium | High | Hard dataset gate before any use; CI fails on missing/failed record; exclude on doubt | License+Data-Ethics reviewer |
| License misread (NC/share-alike conflict with CC-BY content) | Medium | High | Explicit NC/share-alike policy; license text snapshots; fetch-by-script vs vendor decision recorded | License+Data-Ethics reviewer |
| Lessons rot / stop running as packages update | High | Medium | Pinned lockfiles; CI executes every lesson on every change; scheduled re-run; version-bump tasks | Technical reviewer |
| Incorrect biological/clinical statement misleads a learner | Medium | High | Provenance on every claim; domain review for medium lessons; "not medical advice" standing disclaimer | Domain/oncology reviewer |
| Curriculum drifts into implicit medical advice | Low | High | Education-only scope; patient-facing out of scope; disclaimer; domain review gate | Maintainer |
| R and Python tracks diverge in quality/coverage | Medium | Medium | Single Quarto source where possible; CI parity check; track-parity task | Technical reviewer |
| No partner/educator adopts → output never "delivered" | Medium | High | M0 outreach; steward role owns last-mile; honest `verifiedNeed: false` until secured | Steward |
| Reviewer roles unfilled (esp. domain/license) block progress | Medium | High | Name/secure roles in M0; gate medium content until domain reviewer exists | Maintainer |
| Datasets too large for low-spec learners | Medium | Medium | Tiny teaching subsets; free cloud-notebook fallback; document minimum specs | Technical reviewer |
| Statistical mis-teaching (p-hacking, ignoring multiple testing) | Medium | High | Dedicated statistical-humility content; technical+domain review; reproducible examples | Pedagogy + Domain reviewers |

---

## Security & privacy

- **Threat surface is small:** a static content site and CI; no user accounts, no backend, no patient
  data, no secrets that grant data access.
- **Secrets handling:** no API keys or tokens are needed to fetch open data; if any source ever
  requires a token, that source is reconsidered for scope. No secrets in lessons, logs, or commits
  (per CLAUDE.md). CI uses only the default repo token with least privilege.
- **PII:** none collected from learners; completion/testimonial/capstone-gallery participation is
  opt-in and self-published. No tracking that fingerprints users. No patient data ever ingested.
- **Abuse/misuse prevention:** the curriculum never teaches how to obtain controlled or identifiable
  data, never includes re-identification techniques, and frames all clinical interpretation as
  education-only. Refusal guardrails (CLAUDE.md) apply: any task drifting toward identifiable-data
  handling, re-identification, or clinical advice is refused and flagged.
- **Supply chain:** dependencies pinned via lockfiles; CI builds from locks; license snapshots
  committed so provenance is auditable.

---

## Sustainability & maintenance

- **Who maintains after delivery:** the maintainer plus the reviewer pool; lessons are designed to be
  individually owned and re-runnable. Each lesson is a self-contained unit a future contributor can
  update without understanding the whole.
- **Keeping it alive:** scheduled CI re-runs catch dependency rot early; version-bump and
  re-verification are first-class backlog tasks; the dataset gate is re-audited when sources change.
- **Outcome tracking:** the opt-in completion/capstone/testimonial signals and cross-links from other
  cancer-project PRs feed the success metrics; adoption is tracked via written educator confirmations.
- **Graceful degradation:** if a dataset's terms change, its lessons are swapped to an alternative
  gated dataset; the gate makes such swaps localised and safe.

---

## Open questions

1. **Partner & reviewers:** who is the first adopting educator/program, and who fills the (blocking)
   License+Data-Ethics and (required-for-medium) oncology-aware reviewer roles? — needs a human decision.
2. **Vendor vs fetch:** for each candidate dataset, does its license permit vendoring a tiny teaching
   subset, or must we fetch-by-script? (Per-source legal judgement.)
3. **NC/share-alike vs CC-BY:** confirm the acceptance policy where a source's NC/share-alike terms
   interact with our CC-BY lesson license — does the policy exclude, isolate, or relabel such lessons?
4. **Track parity scope:** do we commit to *full* R+Python parity for every lesson, or designate some
   lessons single-track to control effort?
5. **i18n depth now vs later:** how much translation-readiness do we build into M0–M3 vs defer?
6. **SEER/GLOBOCAN aggregates:** confirm which aggregate products are redistributable vs reference-only.

---

## References

- `C:\code\elyos\CLAUDE.md` — Elyos work rules, lanes, quality bar, refusal guardrails.
- `C:\code\elyos\docs\good-deed-definition.md` — the 5 criteria + risk tiers.
- `C:\code\elyos\packages\schema\src\schemas.ts` — Task JSON schema (TASKS.md maps to this).
- `C:\code\elyos\planning\ROADMAP.md` — portfolio + Track 8 cancer guardrails (the binding source).
- `C:\code\elyos\planning\projects\open-data-datasheets\{PLAN,TASKS}.md` — sibling project; dataset
  license-gate pattern reused here.
- Quarto; Bioconductor (`limma`, `DESeq2`, `survival`); Python (`pandas`, `lifelines`, `statsmodels`);
  `renv`, conda/pixi — tooling references (to be cited precisely in lessons).

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified during drafting and **applied** to this plan
and to TASKS.md (not left as suggestions):

1. **Reordered Data/licensing to lead with the binding cancer guardrails** and made the dataset gate
   the project's central control, per Track-8 policy — not a buried compliance note.
2. **Made the dataset gate CI-enforced**, not just a checklist: a lesson referencing a missing/failed
   dataset id fails the build (§6, §7), turning policy into a hard mechanism.
3. **Added an explicit `accessTier` enum** (open | aggregate | de-identified) to the dataset record so
   "controlled-access" cannot slip in implicitly.
4. **Added `permitsReuse` as an evidence-backed boolean** with default-exclude semantics (unknown =
   excluded), mirroring the sibling project's no-default-allow rule.
5. **Required license text snapshots** (committed text + SHA-256 + archival URL) so the basis of every
   reuse decision is auditable later.
6. **Separated "fetch-by-script" from "vendor a subset"** as an explicit per-dataset license decision,
   reducing redistribution risk.
7. **Down-scoped risk honestly:** project is `low` overall but survival/clinical-interpretation lessons
   are `medium` with domain review, and patient-facing material is declared **out of scope** rather
   than quietly `high`.
8. **Added a standing "education, not medical advice" disclaimer** requirement on clinical-interpretation
   lessons, with a named domain-review gate.
9. **Made reviewer roles first-class and blocking** — License+Data-Ethics is non-skippable; domain
   reviewer is required for medium lessons; both marked TO BE SECURED honestly.
10. **Set `verifiedNeed: false` everywhere** until a named adopting partner exists, consistent with
    "delivered, not merged," and added a steward role to own the last-mile.
11. **Replaced vanity metrics with outcome metrics** (completions with a working capstone, educator
    adoption, advocate enablement, pipeline into other cancer projects), explicitly excluding views/stars.
12. **Made reproducibility a build gate**: every lesson executes clean in CI from pinned lockfiles;
    "works on my machine" is disallowed by construction.
13. **Committed to dual R+Python tracks with a CI parity check** and a dedicated parity task, preventing
    silent divergence.
14. **Chose Quarto single-source dual-track** to minimise drift and enable CI execution of both languages.
15. **Added a statistical-humility thread** (multiple testing, confounding, survivorship, correlation≠
    causation) as explicit content, mitigating the mis-teaching risk.
16. **Pinned environments via `renv` + conda/pixi lockfiles** and made lockfile builds an M0 exit
    criterion, addressing lesson rot proactively.
17. **Designed i18n-readiness in from the start** (plain language, externalised structure) while keeping
    actual translation out of scope and flagging patient-facing translation as `high`.
18. **Specified an accessibility baseline** (alt text, contrast, structure) as a per-lesson gate, with a
    100%-conformance success metric.
19. **Linked the curriculum into the wider Elyos cancer track** as both a consumer (reuse
    `open-teaching-datasets`) and a feeder (pipeline learners into other projects), making it leverage
    not duplicate.
20. **Added scheduled CI re-runs + version-bump backlog tasks** so dependency rot is caught and fixed,
    addressing the high-likelihood maintenance risk.
21. **Wrote a concrete NC/share-alike acceptance policy** task and open question, instead of hand-waving
    license edge cases.
22. **Forbade teaching how to obtain controlled/identifiable data** and any re-identification technique —
    an explicit abuse-prevention stance in §14.
23. **Collected zero learner PII**; made all completion/testimonial/gallery signals opt-in and
    self-published, removing a privacy surface.
24. **Defined Definition of Shipped per lesson** including the real-world-use ("delivered") gate, so a
    merged-but-unused lesson is never counted as done.
25. **Provided a schema-valid example Task JSON** for the first M0 task with honest `verifiedNeed: false`,
    correct enums, and an explicit `outputLicense`, so the backlog is provably schema-conformant.

---

## Review sign-off

**Reviewer:** senior staff engineer + TPM (drafting reviewer). **Date:** 2026-06-28.

**Completeness check (against PLAN_SPEC 17-section structure):** all 17 required H2 sections present
and in order (Executive summary → References), with Data/licensing & compliance leading on the binding
cancer guardrails as instructed. Appendix A (25 applied improvements) and this sign-off added.

**Correctness check:**
- *Guardrails:* open/aggregate/de-identified only; controlled-access and identifiable data declared
  out of scope; per-source license verification with default-exclude; provenance on every claim;
  education-only with "not medical advice" framing and domain review for medium lessons. ✔
- *Schema:* TASKS.md maps every task to the Task JSON fields in `schemas.ts`; the example JSON includes
  all required fields, valid enum values, and a donated lane (so no `fundedBudgetUsd` needed). ✔
- *Honesty:* partner, requestor, and reviewer roles marked TO BE SECURED; `verifiedNeed: false`
  throughout; metrics are outcome-based with baseline 0. ✔
- *Risk tiering:* low overall with explicit medium (clinical-interpretation) carve-outs and high
  (patient-facing) excluded — consistent with Elyos risk-tier policy. ✔

**Fixes applied during review:** (a) made the dataset gate explicitly CI-blocking rather than advisory;
(b) added the `accessTier` enum to forbid implicit controlled-access; (c) clarified that the
curriculum never teaches how to *obtain* controlled data; (d) added the track-parity CI check and task;
(e) tied the "delivered" gate into Definition of Shipped and the steward role.

**Residual items requiring a human decision:** secure the License+Data-Ethics reviewer (blocking),
the oncology-aware domain reviewer (required for M2), and the first adopting educator/partner; resolve
per-source vendor-vs-fetch and NC/share-alike acceptance. These are tracked in Open Questions and as
M0 tasks. **Status: APPROVED to proceed to M0, with medium-risk (M2) content gated until the domain
reviewer is secured.**
