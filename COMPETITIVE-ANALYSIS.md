# Competitive + Improvement Analysis — `bioinformatics-from-zero`

**Project:** Open intro curriculum for cancer data analysis from absolute zero (R + Python, open datasets) — teaching materials. Lane: donated. Risk tier: low (with medium survival/clinical-interpretation carve-outs).
**Plan reviewed:** `PLAN.md` v0.1.0 (2026-06-28).
**Analyst date:** 2026-06-29.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually mature for a v0.1.0 draft: 17-section structure, a binding dataset gate, honest `verifiedNeed: false`, outcome-based metrics, and a 25-item applied-improvements appendix. Below are findings grouped by the requested dimensions, with the most material first.

**Pedagogical design / learning objectives (PARTIALLY ADEQUATE — biggest gap).**
- The plan asserts "from-absolute-zero" and lists module-level outcomes, but it does **not specify per-lesson, measurable learning objectives** in Bloom's-taxonomy terms ("learner will be able to…"), nor a defined entry-skill floor ("never opened a terminal"). The Carpentries and HBC train-the-trainer practice anchors every episode to explicit objectives and questions; without that, "from zero" is an aspiration, not a spec. **Recommend: mandate a learning-objectives block per `.qmd` as a CI-checkable front-matter field.**
- No explicit **concept dependency graph / prerequisite map** between lessons. A from-zero curriculum lives or dies on ordering (e.g., you cannot teach `DESeq2` design formulas before factors/data frames). The plan implies a sequence (M1→M3) but does not commit to a prerequisite DAG that the parity/structure CI could validate.
- **Cognitive-load realism for absolute beginners is under-addressed.** Teaching environment setup + R + Python + git + shell + statistics + cancer biology + Bioconductor to someone who "has never opened a terminal" is an enormous surface. The dual-track R+Python decision (see scope) compounds this.

**Prerequisites.** Stated loosely ("no programming background"). Missing: an explicit statement of what biology/math IS assumed. Data Carpentry Genomics explicitly assumes "familiarity with biological concepts incl. genomic variation"; this plan claims zero biology too, which roughly doubles authoring scope. This tension is not resolved.

**Reproducible compute environment for learners (STRONG on CI, WEAKER on the learner's own machine).**
- Excellent: CI executes every lesson from pinned `renv.lock` + conda/pixi lockfiles on a clean runner. This is best-in-class and beats most competitors (Carpentries lessons are not all CI-executed end-to-end).
- Gap: the plan pins the **authoring/CI** environment but is vague on the **learner-facing zero-install path**. It mentions "free cloud notebooks" once. For true from-zero, the dominant friction is local install of R+Bioconductor+conda on a low-spec/Windows machine. The plan should name a concrete browser-based fallback (Binder/`repo2docker`, Google Colab, Posit Cloud, or GitHub Codespaces) and **test that path in CI too** — not just the lockfile build. Note mybinder.org has known limits (10-min idle timeout, ≤2 CPU) and sustainability questions; a single hosted dependency is a risk. WebGPU/WASM options (`webR`, JupyterLite/Pyodide) are worth evaluating for a truly install-free R/Python experience.
- `renv` + conda/pixi is a sound, current 2026 choice (pixi is the modern conda-compatible solver). Good.

**Dataset licensing for teaching (STRONG — the plan's standout strength).** The `datasets.yml` gate with `permitsReuse` (default-exclude), `accessTier` enum, license SHA-256 snapshots, fetch-vs-vendor decision, and CI enforcement is rigorous and correct. Two corrections/refinements:
- **TCGA open-access tier is effectively public domain / no reuse restrictions** (NCI/CGC confirm Open Access data is not covered by the Data Use Certification). The plan treats TCGA open tier as `license: see-source` — it can be more confidently classified, though attribution/publication-guideline norms still apply. Good that the gate forces verification.
- **DepMap/CCLE is the licensing trap.** DepMap data is distributed under CC BY (4.0) but the community guidance is "educational use" with attribution, and CCLE redistribution has historically been murky. The plan's "verify current version" note is correct; flag DepMap as **fetch-by-script, not vendored** by default.

**Assessment (UNDER-SPECIFIED).** Exercises-with-solutions and an optional post-MVP autograder are mentioned, but there is no assessment design: no formative checks per lesson, no rubric for the capstone, no diagnostic/pre-test to validate the "from zero" claim, no mastery checks. Rosalind's auto-graded problem tree is the gold standard for self-paced verification; the plan currently has no equivalent feedback loop for a self-learner working unaided (which is the stated primary persona).

**Accessibility (GOOD intent, THIN spec).** Alt text, contrast, structure, 100% checklist conformance, and i18n-readiness are named. Missing specifics: target standard (WCAG 2.2 AA?), how plots/figures get text descriptions (auto-generated alt text for generated plots is non-trivial), colour-blind-safe palettes mandated in the plotting lessons themselves, keyboard/screen-reader testing of the rendered Quarto site, and captioning policy for the backlog videos. "Automated + manual a11y review" should name the tools (e.g., axe, pa11y).

**Currency of tools (CURRENT).** Quarto, tidyverse, `limma`/`DESeq2`, `survival`/`survminer`, pandas, `lifelines`, `statsmodels`, scikit-learn, `renv`, conda/pixi, GitHub Actions/Pages — all current and appropriate for 2026. One note: `survminer` is comparatively lightly maintained; consider `ggsurvfit` as the modern alternative.

**Scope realism (THE central risk).** Dual full R+Python parity × from-zero × zero-biology-assumed × genomics depth (DE + survival + clustering) × reproducibility harness × accessibility × i18n-ready is a very large surface for a greenfield, partner-less, reviewer-unsecured project. Open Question #4 (full parity vs single-track) is the right lever; the plan should **resolve it toward selective parity** (foundations dual-track, advanced genomics single-track-first) to be deliverable. The capstone "reproduce a published-style analysis from zero" is ambitious but well-chosen as the proof outcome.

**Not duplicating Carpentries (ADEQUATE but needs sharper proof).** The plan name-checks Carpentries/Bioconductor/Galaxy as outreach targets and claims differentiation via "from-absolute-zero + cancer-focused + reproducible-by-construction." This is defensible (see §2–3) but the plan does not include an explicit **gap-map / "why not just fork bioc-intro"** artifact. Given that Bioconductor's `carpentries-incubator/bioc-intro` already does "intro to data analysis with R and Bioconductor, no prior experience," the differentiation must be argued concretely, not asserted. **Recommend adding a one-page "prior-art delta" doc to M0.**

**Minor/other:**
- Success metric "≥50 self-reported completions" depends on opt-in surveys with no PII — realistic but measurement is weak; consider a verifiable capstone-PR signal as a harder proxy.
- The plan reuses a sibling `open-teaching-datasets` project "where available" but those outputs may not exist yet — a sequencing dependency risk not called out in Risks.
- No mention of **localization of number/date formats or units**, which matters for i18n-readiness claims.

Overall: **correct and well-guarded on licensing/ethics/reproducibility-CI; under-specified on pedagogy mechanics (objectives, prerequisite DAG, assessment) and on the learner's own zero-install path.**

---

## 2. Competitive landscape

**The Carpentries — Data Carpentry Genomics / Software Carpentry.** Two-day, CC-BY, instructor-led genomics workshop (shell, QC, variant calling, AWS, R for visualization); assumes no tool experience but *does* assume some biology. Strengths: gold-standard pedagogy (per-episode objectives/questions), huge instructor network, proven OER governance, CC-BY. Weaknesses: **not cancer-focused**, variant-calling/NGS oriented (not expression/survival), assumes biology, relies on a live instructor, not fully CI-executed end-to-end, and uses AWS/cloud assumptions. URLs: https://datacarpentry.org/lessons/ , https://datacarpentry.github.io/genomics-workshop/ , https://datacarpentry.github.io/wrangling-genomics/

**Bioconductor training / Carpentries Incubator (`bioc-intro`, `bioc-project`).** "Introduction to data analysis with R and Bioconductor," no prior experience required, plus bulk and single-cell RNA-seq courses; CC-BY; 94% completion in their instructor program; global reach (Africa, Brazil, India). Strengths: directly adjacent, R/Bioconductor-authoritative, Carpentries-quality, active 2025 community. Weaknesses: **R-only** (no Python parity), **not cancer-specific**, assumes some prior R for the RNA-seq courses, instructor-oriented. URLs: https://training.bioconductor.org/ , https://carpentries-incubator.github.io/bioc-intro/ , https://carpentries-incubator.github.io/bioc-project/

**Galaxy Training Network.** >230 tutorials incl. transcriptomics (reference-based RNA-seq → genes → pathways), some cancer examples (e.g., colorectal cell-line treatment study); browser-based Galaxy = no install, runs on hosted infra. Strengths: zero-install via Galaxy servers, reproducible, free, FAIR, huge breadth, self-directed. Weaknesses: teaches the **Galaxy GUI**, not transferable R/Python coding skills (the stated goal here), so it is complementary rather than competitive for "learn to code cancer analysis." URLs: https://training.galaxyproject.org/training-material/ , https://training.galaxyproject.org/training-material/topics/transcriptomics/

**JHU Genomic Data Science Specialization (Coursera).** 8 courses (command line, Python, R/Bioconductor, Galaxy, statistics); "background helpful but not required"; auditable free. Strengths: structured, credentialed, beginner-tolerant, covers both languages, brand. Weaknesses: **certificate paywalled**, platform-locked (not adoptable/remixable OER), not cancer-specific, NGS/sequencing emphasis, not reproducible-by-construction for the learner. URLs: https://www.coursera.org/specializations/genomic-data-science , https://www.coursera.org/learn/introduction-genomics

**Harvard Chan Bioinformatics Core (`hbctraining`).** Excellent free CC-BY self-learning modules — DGE with DESeq2/clusterProfiler, intro R/shell. Strengths: high-quality, cancer-relevant DE workflows, CC-BY, widely reused, on GitHub. Weaknesses: **R-only**, assumes basic R for DGE, not a single from-zero arc (modular short workshops), not cancer-curriculum-framed, not CI-executed for learners. URLs: https://github.com/hbctraining , https://hbctraining.github.io/main/

**EMBL-EBI Training (Cancer Genomics / Cancer Genomics & Transcriptomics).** Virtual cohort courses on cancer data with hosted compute (no powerful computer needed). Strengths: cancer-specific, authoritative, hosted practicals. Weaknesses: **aimed at advanced PhD/postdoc**, requires prior R/Bioconductor + Unix, cohort/scheduled (not self-paced OER), not from-zero. URLs: https://www.ebi.ac.uk/training/events/cancer-genomics-virtual/ , https://www.ebi.ac.uk/training/

**Rosalind.** Auto-graded bioinformatics problem tree, 88k+ solvers, free, builds programming+biology+algorithms concurrently. Strengths: self-paced, instant verification (the killer feature competitors lack), gamified. Weaknesses: **sequence-algorithm focused** (strings, alignment, combinatorics), **not data-analysis / not cancer / not R-Bioconductor / no statistics or survival**, no reproducible-environment story. URLs: https://rosalind.info/ , https://rosalind.info/problems/list-view/

**freeCodeCamp / Kaggle.** Strong free foundations (Python data analysis, pandas, viz, ML) and scattered Biopython/drug-discovery notebooks; Kaggle/Colab give zero-install compute. Strengths: excellent free coding on-ramps, hosted notebooks, large audiences. Weaknesses: **generic, not cancer/genomics curricula**, fragmented, no licensing/ethics gate, no R-Bioconductor or survival. URLs: https://www.kaggle.com/learn , https://www.freecodecamp.org/

**glittr.org (meta).** Curated index (664+ repos) of git-hosted computational-life-science training; discourages single-tool tutorials, ranks by stars/recency. Strategic relevance: the **discovery layer** where this curriculum must appear to be found and to prove it is a "general topic" (not a tool tutorial). URLs: https://glittr.org/ , https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0308729

**Landscape verdict:** abundant high-quality OER exists for adjacent goals, but it splits along three axes none of the leaders close simultaneously — (a) **truly from absolute zero incl. no-biology-assumed**, (b) **cancer-specific data**, and (c) **dual R+Python + reproducible-by-construction with a verified-open dataset gate**. No competitor occupies all three.

---

## 3. Gaps we can fill

1. **The single from-absolute-zero → reproduce-a-cancer-paper arc.** Everyone has pieces (Carpentries = foundations, HBC = DGE, EBI = advanced cancer); nobody ships one continuous path from "never opened a terminal" to "I reproduced and can critique a cancer analysis." The capstone is the differentiating outcome.
2. **Cancer-specific framing on open data, from zero.** Galaxy/EBI have cancer but assume prior skill or a GUI; Carpentries/Bioconductor are from-low but generic. The intersection is empty.
3. **Dual R + Python parity from one source.** Competitors are overwhelmingly R-only (Bioconductor, HBC) or Python-only (Rosalind/Kaggle). Authentic dual-track from a single Quarto source is rare and valuable for learners who don't yet know which ecosystem they'll land in.
4. **Reproducible-by-construction for the learner, not just the author.** A CI-executed, lockfile-pinned, zero-install-fallback curriculum where "works on my machine" is impossible is a genuine differentiator over PDF/HTML lessons that silently rot.
5. **A first-class license + de-identification gate as teaching content.** No competitor teaches dataset licensing, controlled-vs-open tiers, de-identification, and provenance *as part of the curriculum* while enforcing it on its own materials. This doubles as ethics pedagogy.
6. **Statistical-humility thread for cancer data** (multiple testing, confounding, survivorship bias, correlation≠causation, batch effects). Most tutorials teach the happy path; few teach how to be wrong less often — uniquely valuable for patient-advocate researchers.
7. **Self-paced verification.** Rosalind proves auto-grading drives completion; no cancer-data curriculum offers it. A lightweight autograder/answer-check is an open gap.
8. **Adoptable OER governance for cancer specifically** — CC-BY, instructor notes, remixable, glittr-discoverable, i18n-ready — aimed at under-resourced educators and advocacy programs the big providers don't serve.

---

## 4. Differentiators to win

1. **"Reproduce a real cancer analysis from zero" as the promise and the proof.** A concrete capstone outcome, not a topic list — directly aligned with Elyos "delivered, not merged."
2. **Reproducible-by-construction + zero-install fallback.** Every lesson runs in CI and in a browser; the learner never debugs an install. This is a trust moat over static OER.
3. **The dataset gate as both guardrail and lesson.** Licensing/de-identification/provenance enforced in CI *and* taught — a credibility signal no competitor matches, and a perfect fit for the cancer track.
4. **Genuine R+Python parity from one source** with CI parity checks — serves both ecosystems and future-proofs the learner.
5. **Statistical-humility + ethics woven throughout**, with oncologist-aware review on interpretation lessons — positions the curriculum as *correct interpretation*, not just code that runs.
6. **Built to be adopted and remixed by the under-served** (CC-BY, instructor guide, low-spec/free-cloud, i18n-ready, glittr-listed) — and as the on-ramp feeding the rest of the Elyos cancer track.

---

## 5. Claude API leverage (and hard limits)

**Where Claude adds leverage (draft-and-verify, human-in-the-loop):**
- **Lesson drafting from open datasets** — generate first-draft narrative `.qmd`, worked examples, and language-parallel R/Python code from a verified `datasets.yml` row, with mandatory citations to provenance.
- **Exercise/solution/distractor generation at scale** — produce graded practice problems, hidden solutions, and plausible-but-wrong multiple-choice distractors (the labor-intensive part of assessment), then verify by execution.
- **Reproducible-environment scaffolding** — draft `renv.lock`/`environment.yml`/`pixi.toml`, `repo2docker`/Binder config, GitHub Actions CI, and fetch scripts; propose minimal pinned dependency sets.
- **Practice-problem and capstone-variant generation** — many isomorphic problems from one template so self-learners get fresh, auto-checkable repetition (Rosalind-style) without author toil.
- **Accessibility assist** — draft alt text for generated figures, flag low-contrast palettes, suggest plain-language rewrites, check reading level, and generate caption/transcript drafts for videos.
- **Pedagogy scaffolding** — propose per-lesson learning objectives, prerequisite DAG edges, glossary entries, and difficulty-ramp critiques for human review.
- **i18n/structure prep** — externalize strings, normalize structure, draft translation-ready segments (translation itself stays human-reviewed).
- **Plan-quality tooling** — generate the "prior-art delta" gap-map vs Carpentries/Bioconductor; lint `datasets.yml` for missing fields.

**Where Claude must NOT decide (hard gates, per CLAUDE.md guardrails):**
- **Technical accuracy is verified by RUNNING code in CI**, never by model assertion. No lesson "works" because Claude says so.
- **No fabricated biology or statistics.** Every biological/clinical claim must trace to a cited source; un-sourced claims are blocked. Survival/clinical-interpretation lessons require **oncologist-aware human review** and the standing "not medical advice" disclaimer — Claude cannot self-certify these.
- **Dataset licenses and de-identification status are human-verified** by the License + Data-Ethics reviewer. Claude may *draft* a `datasets.yml` row and surface the license text, but `permitsReuse` and `accessTier` are human decisions; unknown ⇒ excluded.
- **Pedagogy and final content quality reviewed by educators.** Learning-objective adequacy, difficulty ramp, and "from zero" suitability need human pedagogy sign-off (ideally a beginner test-reader).
- **No steering toward controlled/identifiable data or re-identification**, and no clinical/diagnostic/prognostic guidance — refusal guardrails apply; such tasks are flagged, not fulfilled.
- **Medium-risk content cannot be merged on AI output alone** — domain reviewer is blocking.

---

## 6. Ten concrete optimizations

1. **Mandate a machine-checkable learning-objectives + prerequisites block** in every `.qmd` front-matter, and add a CI lint that fails lessons missing them. Converts "from zero" from claim to spec.
2. **Author and commit a prerequisite-DAG** (concept dependency graph) and validate lesson ordering against it in CI; publish it as the learner's map.
3. **Add a CI-tested zero-install learner path** (Binder/`repo2docker` + a Colab/Posit Cloud/Codespaces fallback, and evaluate `webR`/JupyterLite for install-free R/Python). Don't depend on a single hosted service; test the fallback, not just the lockfile build.
4. **Resolve dual-track scope to *selective* parity** (foundations dual R+Python; advanced genomics single-track-first with a parity backlog) to make the project deliverable — close Open Question #4 decisively.
5. **Build a lightweight auto-checker** (Rosalind-style expected-output/answer checks) into exercises from M1, not deferred to "post-MVP" — it is the missing self-learner feedback loop and a top differentiator.
6. **Write the "prior-art delta" gap-map** (vs `bioc-intro`, HBC DGE, Data Carpentry Genomics) as an M0 deliverable that justifies non-duplication and guides reuse/forking where CC-BY permits.
7. **Reuse, don't re-author, where licenses allow** — fork/adapt CC-BY Carpentries/Bioconductor/HBC foundation episodes for the R track and spend net-new effort on the cancer-specific, Python-parity, and reproducibility layers. Faster, and good OER citizenship.
8. **Tighten accessibility to a named standard** (WCAG 2.2 AA), mandate colour-blind-safe palettes inside the plotting lessons, name the a11y tools (axe/pa11y) in CI, and define an alt-text-for-generated-plots workflow.
9. **Pre-classify the candidate datasets now**: TCGA open tier (public-domain-like, attribution norms) vendorable subset OK; DepMap/CCLE → fetch-by-script only; GEO/recount3 → per-series check; SEER*Explorer/GLOBOCAN → confirm aggregate redistributable vs reference-only. Seed `datasets.yml` with these provisional rulings to de-risk M2.
10. **Get listed on glittr.org and frame the repo as a "general topic" curriculum** (not a tool tutorial) for discoverability; add a completion/capstone-PR signal as a harder outcome metric than opt-in surveys.

---

## 7. Parallel & perpendicular spin-offs

- **`oncology-data-literacy` (parallel).** A non-coding sibling: read/interpret cancer charts, statistics, and study claims for patient-advocate researchers. This curriculum's statistical-humility thread is the natural feeder; share the glossary and disclaimer framework.
- **`reproducibility-curriculum` (perpendicular).** Lift the lockfile + CI-executes-everything + provenance harness into a domain-agnostic "reproducible analysis from zero" course. The reproducibility engine here is reusable well beyond cancer.
- **`open-teaching-datasets` (parallel, bidirectional).** The `datasets.yml` gate + license-snapshot pattern *is* a teaching-dataset catalogue; promote it to a shared, queryable, gated catalogue that other Elyos projects consume — and consume its outputs here.
- **`open-coding-curriculum` (perpendicular).** Extract the from-zero programming/data/stats foundation (M1) as a domain-neutral coding on-ramp; cancer becomes one of several "tracks" plugged onto a shared base.
- **`stats-for-clinicians` (parallel).** Repackage the survival/multiple-testing/confounding content (with credentialed review) for clinicians and trainees — higher risk tier, but high impact.
- **Reusable "from-zero curriculum engine" (perpendicular product).** Generalize the whole rig — Quarto dual-track source + objectives/prereq-DAG linting + dataset gate + reproducibility CI + autograder + a11y checks — into a template repo any Elyos education project forks. This is arguably the highest-leverage byproduct.
- **An MCP server (perpendicular).** A "curriculum/dataset-gate" MCP server exposing tools like `lint_dataset_record`, `check_license`, `verify_lesson_runs`, `generate_exercise_variants`, and `draft_learning_objectives` — lets any Claude-driven authoring session enforce the gates programmatically. (Confirm MCP/tooling design against the `claude-api` reference before building.)

---

## 8. Open questions

1. **Prior-art reuse boundary:** which Carpentries/Bioconductor/HBC CC-BY episodes do we fork-and-adapt vs author fresh, and does forking change our "from-zero/no-biology-assumed" claim? (Resolve in the M0 gap-map.)
2. **Biology prerequisite floor:** do we truly assume *zero* biology (≈doubling scope vs Data Carpentry's "some biology assumed"), or define a minimal biology primer as lesson 0?
3. **Parity commitment:** full dual R+Python everywhere, or selective parity (the recommended path)? Decision gates total effort and deliverability.
4. **Learner zero-install platform:** Binder vs Colab vs Posit Cloud vs Codespaces vs `webR`/JupyterLite — which is primary, which is fallback, and who funds/maintains hosted compute? (mybinder.org sustainability/limits are a live risk.)
5. **Assessment depth:** how far does the autograder go (output checks only, or mastery/diagnostic pre-tests to validate "from zero")? When in the roadmap?
6. **Partner & reviewers (from the plan, still blocking):** first adopting educator/program; the blocking License+Data-Ethics reviewer; the medium-risk oncology-aware reviewer.
7. **DepMap/CCLE and SEER/GLOBOCAN specifics:** redistributable-subset vs fetch-only, per current terms — needs the License reviewer's per-source ruling.
8. **Accessibility for generated plots:** what is the sustainable workflow for meaningful alt text / data tables for every executed figure across both tracks?

---

### Sources
- https://datacarpentry.org/lessons/ · https://datacarpentry.github.io/genomics-workshop/ · https://datacarpentry.github.io/wrangling-genomics/
- https://training.bioconductor.org/ · https://carpentries-incubator.github.io/bioc-intro/ · https://carpentries-incubator.github.io/bioc-project/
- https://training.galaxyproject.org/training-material/ · https://training.galaxyproject.org/training-material/topics/transcriptomics/
- https://www.coursera.org/specializations/genomic-data-science · https://www.coursera.org/learn/introduction-genomics
- https://github.com/hbctraining · https://hbctraining.github.io/main/
- https://www.ebi.ac.uk/training/events/cancer-genomics-virtual/ · https://www.ebi.ac.uk/training/
- https://rosalind.info/ · https://rosalind.info/problems/list-view/
- https://www.kaggle.com/learn · https://www.freecodecamp.org/
- https://glittr.org/ · https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0308729
- https://2i2c.org/blog/2025/frictionless-reproducibility/ · https://jupyter.org/binder
- TCGA open tier: https://www.cancergenomicscloud.org/data-use · DepMap/CCLE: https://forum.depmap.org/t/re-distribution-of-the-ccle-data-licensing/3063
