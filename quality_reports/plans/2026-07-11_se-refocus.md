# Refocus: Economics → Empirical Software Engineering

## Context

This repo is a fork of the **clo-author** template — a Claude Code research pipeline (discover → strategize → analyze → write → review → submit) built for **empirical economics**. The user's field is **software engineering and development team management**, not economics. The task: rewrite every economics-specific element (terminology, journals, datasets, methods, formats, examples) so the pipeline serves an **empirical software engineering (ESE) thesis**.

**User decisions (confirmed):**
- Nature: still an academic research pipeline (thesis/papers), domain = empirical SE / dev team management
- Target venues: IEEE/ACM academic SE venues (IEEE TSE, ACM TOSEM, EMSE, ICSE/FSE/ESEM)
- Analysis language: **Python primary, R secondary, Julia removed**
- Language: system docs stay **English**; deliverables (thesis/paper text) written in **Spanish**
- guide/ site: update `.qmd` sources only, **no re-render** of `docs/`
- No thesis topic yet → SE-flavored placeholders (later filled by `/discover interview`)

**Key finding from exploration:** `paper/`, `data/`, `scripts/R/` are empty scaffolding — no real research content exists yet, so this is a pure template refocus with no risk to actual work.

---

## Domain Mapping Canon

This mapping is applied consistently across all files (it will be embedded in every rewrite-agent prompt):

| Economics (current) | Software Engineering (target) |
|---|---|
| Field: applied economics | Empirical software engineering; software team management, developer productivity, DevOps |
| Top-5: AER, Econometrica, JPE, QJE, REStud | Top journals: IEEE TSE, ACM TOSEM, Empirical Software Engineering (EMSE) |
| Field journals (JHR, JHE, AEJ…) | JSS, IST, IEEE Software; top conferences: ICSE, FSE, ASE, ESEM, MSR, ICSME, CHASE |
| NBER / SSRN / RePEc working papers | arXiv cs.SE, ACM DL, IEEE Xplore, DBLP, Semantic Scholar |
| Datasets: CPS, PSID, ACS, Medicare… | GitHub (REST/GraphQL API, GH Archive, SEART GHS), Stack Overflow (data dump, Developer Survey), TravisTorrent, World of Code, Jira/issue-tracker datasets, DORA State of DevOps, project telemetry |
| JEL codes | Keywords + ACM CCS concepts |
| AEA style (no stars) | ACM/IEEE style; effect sizes + CIs per ACM SIGSOFT Empirical Standards |
| AEA RCT Registry / EGAP | OSF preregistration (SE standard); registered reports (EMSE/MSR) |
| AEA replication package audit | ACM/IEEE artifact evaluation (badges: Available / Functional / Reusable) |
| Method taxonomy: DiD/IV/RDD/structural | **Keep causal designs** (DiD, IV, RDD, event study — genuinely used in modern MSR research) but re-example with SE data; **replace structural estimation** with SE designs: controlled experiments (human subjects), surveys, case studies, repository mining (MSR), systematic literature reviews; add simulation |
| "Identification section" framing | Study design + **Threats to Validity** (construct / internal / external / conclusion) |
| Welfare analysis | Practical significance / implications for practitioners |
| R primary: fixest, modelsummary, did, rdrobust | Python primary: pandas, statsmodels, linearmodels, pyfixest, scipy, matplotlib; PyDriller + GitHub API for mining. R secondary (fixest et al. kept as alternative). Julia removed. |
| Seminal refs: Angrist, Callaway-Sant'Anna, Oster… | Wohlin et al. (2012) *Experimentation in SE*; Runeson & Höst (2009) case studies; Kitchenham & Charters (2007) SLR guidelines; Ralph et al. (2021) ACM SIGSOFT Empirical Standards; Forsgren, Humble & Kim (*Accelerate*/DORA); Stol & Fitzgerald (2018) ABC framework. Causal-inference cites (Callaway-Sant'Anna, etc.) kept where the design checklists keep those methods. |
| Referee dispositions (Structuralist, Credibility Rev., …) | SE referee archetypes: Rigor (internal validity), Relevance (practitioner impact), Measurement (construct validity of metrics), Generalizability (beyond-OSS skeptic), Theory, Replication/Open-Science |
| "Reads like an economist" | "Reads like a software engineering researcher" |

**New language policy** (added to CLAUDE.md): system files English; `paper/` deliverables and talks drafted in **Spanish** (`polyglossia` with XeLaTeX); bibliography entries stay in original language.

**Format rule:** `working-paper-format.md` keeps the article/12pt/double-spaced thesis-draft format (good for a Spanish thesis manuscript) but: JEL block → Keywords + ACM CCS; adds Spanish-language setup; notes that final venue formats (IEEEtran/acmart) are produced at `/submit` stage. Filenames are **never renamed** — cross-references throughout agents/skills stay valid.

---

## What Is NOT Touched (historical record)

- `CHANGELOG.md` and `guide/changelog.qmd` — history; instead **append a new entry** documenting this refocus
- `quality_reports/` existing content (demo/, plans/, session_logs/, mas-evolution changelog)
- `MEMORY.md`, `SESSION_REPORT.md` (past-session learnings/log)
- `docs/` (rendered site — not re-rendered per user decision)

---

## Execution Phases

### Phase 0 — Bookkeeping
- Copy this plan to `quality_reports/plans/2026-07-11_se-refocus.md` (project convention)

### Phase 1 — Domain canon in `.claude/references/` (done directly, sets the vocabulary everything else follows)
- `domain-profile.md` — rewrite template: SE field examples, venue tiers (table above), SE data sources, study-design table (experiments/surveys/case studies/MSR/causal designs), SE conventions (effect sizes + CIs, non-parametric tests + Cliff's delta, threats-to-validity), SE seminal/theoretical refs, SE referee concerns (construct validity of metrics, OSS generalizability, mining-study confounds, human-subjects ethics). Default fallback line: "agents default to empirical software engineering"
- `journal-profiles.md` — full rewrite keeping the profile field structure (Focus / Bar / referee adjusts / concerns / referee pool). Profiles: IEEE TSE, ACM TOSEM, EMSE, JSS, IST, IEEE Software + conference profiles ICSE, FSE, ASE, ESEM, MSR, ICSME, CHASE. New referee-pool dispositions per the archetype mapping. Table-format convention: effect sizes + CIs, stars optional per venue
- `coding-standards-python.md` — becomes primary: pandas/statsmodels/linearmodels/pyfixest stack, PyDriller/GitHub API, matplotlib; remove "sklearn not for causal inference" and "seaborn non-standard for econ" framing
- `coding-standards-r.md` — becomes secondary, de-econ'd
- **Delete** `coding-standards-julia.md`; strip Julia references repo-wide in later phases
- `personal-style-guide.md` — swap econ example lexicon for SE equivalents

### Phase 2 — `.claude/rules/` (done directly — small set, high leverage)
- `working-paper-format.md` — per Format rule above (JEL→keywords+CCS, Spanish setup, venue-format note)
- `content-standards.md` — SE table conventions, Python-first code examples, SE file-naming (`sumstats_`, `reg_` kept; `first_stage_` re-examples)
- `content-invariants.md` — INV-4 (stars per venue profile, drop AEA special-case), INV-6 (JEL → keywords + CCS), INV-13–19 Python-first, drop Julia
- `meta-governance.md` — "empirical software engineering research"; adjacent fields → HCI, CSCW, information systems
- `workflow.md` — worked examples swapped (e.g., "effect of code-review practices on defect rates using GitHub data"); "R Scripts" simplified mode → Python
- `html-dashboard.md` — section examples de-econ'd (Identification → Study Design; Treatment & IV / Wind-instrument examples → generic SE examples)
- `permissions.md`, `quality.md`, `agents.md`, `lifecycle.md`, `revision.md` — trivial term swaps (theorist CONDITIONAL wording, "identification validity" → "study design validity", etc.)
- CLAUDE.md language-policy addition happens in Phase 5

### Phase 3 — `.claude/agents/` (parallel subagents, canon embedded in prompts)
- HEAVY: `explorer.md` (SE data-source taxonomy), `librarian.md` (venue tiers + ACM DL/IEEE Xplore/arXiv cs.SE/DBLP), `methods-referee.md` (ESE methods taxonomy incl. kept causal designs), `strategist.md` (study designs per Empirical Standards; OSF prereg), `coder.md` (Python primary, "default to Python")
- MODERATE: `data-engineer.md` (parquet primary, matplotlib), `verifier.md` (AEA audit → artifact-evaluation badges), `writer.md`, `theorist.md`/`theorist-critic.md` (target methods venues; keep statistical theory core), `domain-referee.md`, `strategist-critic.md`
- TRIVIAL: remaining critics, editor, orchestrator, storyteller pair

### Phase 4 — `.claude/skills/` (parallel subagents)
- `strategize/` — SKILL.md broadened to SE study designs. `pap-templates/`: `aea-rct.md` → OSF-based experiment preregistration; `egap.md` → deleted (redundant once OSF is the registry); `osf.md` adapted. `design-checklists/`: keep `did/iv/rdd/event-study.md` (de-econ examples, Python tools); `structural.md` → replaced by new `experiment.md`, `survey.md`, `case-study.md`, `msr.md` checklists; `descriptive.md` adapted. Memos/robustness templates re-exampled
- `review/` — `causal-audit-4-phases.md` → methods audit (keeps causal-design checks, adds experiment/survey/case-study checks; Python package checks; citation anchors: causal cites kept + Wohlin/Kitchenham/Ralph added); `disposition-pool.md` → SE referee archetypes; `scoring-rubrics.md`, 8-categories, theory-review de-econ'd
- `analyze/` — Python default everywhere (`pre-code-report.md` "default Python"), `table-standards.md`/`figure-standards.md` (SE conventions, matplotlib examples), `paper-to-code-map.md` Python
- `submit/` — AEA replication package → artifact-evaluation package (badges); checklist JEL→CCS
- `discover/` — lit sources + data sources per canon; interview questions reframed (study design instead of natural experiment)
- `write/` — `section-templates.md` for SE paper types (experiment / MSR / survey / case study, incl. Threats to Validity section), `notation-protocol.md` adapted, Spanish-deliverable note
- `talk/` — `narrative-arcs.md` per SE paper types
- `tools/` (lint table Python-first, drop Julia), `new-project/`, `revise/` — minor swaps

### Phase 5 — Root files & `templates/` (parallel with Phase 4)
- `CLAUDE.md` — title "Empirical Software Engineering Research with Claude Code"; Field: Software Engineering / dev team management; **new Language Policy section** (system English, deliverables Spanish); Python primary noted; skills table wording
- `README.md` — rewrite domain framing; keep attribution to the original clo-author template (Hugo Sant'Anna); Prerequisites: Python + XeLaTeX
- `Bibliography_base.bib` — example entries → SE classics (e.g., `Wohlin2012_experimentation`, `Kitchenham2007_slr`)
- `templates/skill-template.md` — two worked examples → SE equivalents; `constitutional-governance.md` — econ block → ESE block; `decision-record.md` example swap
- `CHANGELOG.md` — **append** new entry documenting the refocus

### Phase 6 — `guide/*.qmd` sources (parallel subagent; NO render)
- Update: `index`, `user-guide`, `rules`, `reference`, `agents`, `customization`, `architecture`, `blog-html-reports` (leave `changelog.qmd` and `hooks.qmd` mostly/fully alone)
- Note to user at the end: `docs/` is now stale until they re-render with Quarto

### Phase 7 — Verification
1. Grep sweep for residual econ terms — `econom|econometr|AEA|NBER|RePEc|JEL|AER|QJE|Econometrica|REStud|CPS|PSID|stargazer|Julia` — excluding the historical paths listed above; every remaining hit justified or fixed
2. Consistency spot-reads: `domain-profile.md` ↔ `librarian.md` ↔ `journal-profiles.md` venue lists agree; `coder.md` ↔ `analyze/` ↔ `coding-standards-python.md` all say Python primary; no file references the deleted Julia standards or `egap.md`
3. `git diff --stat` review; confirm no historical file modified
4. Report summary to user (Spanish); **no commit** unless the user asks

## Execution strategy

Phases 1–2 done directly by me (the canon must be authored once, coherently). Phases 3–6 fan out to parallel `general-purpose` subagents (one per area) with the Domain Mapping Canon embedded verbatim in each prompt, plus rules: keep file names and structure, keep English, don't touch historical files. Phase 7 by me.

~70 files modified, 1–2 deleted, 4 new design-checklist files created.
