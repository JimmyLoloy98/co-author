---
name: strategize
description: Design study design (causal, experiment, survey, case study, repository mining, SLR), pre-registration plan, or formal methods section. Dispatches Strategist / Theorist (proposer) and the paired critic (validator). Replaces /identify and /pre-analysis-plan.
argument-hint: "[mode: strategy | pap | pap interactive | theory] [research question or spec path]"
allowed-tools: Read,Grep,Glob,Write,Task
---

# Strategize

Design a study design, pre-registration plan, or formal methods section by dispatching the appropriate creator (**Strategist** or **Theorist**) and its paired critic.

The skill covers the full empirical software engineering design space:
- **Causal designs** (DiD, IV, RDD, event study, synthetic control) — used in modern MSR research around CI adoption, code-review policy rollouts, and platform changes
- **Controlled experiments** with human subjects (tool/practice evaluations)
- **Surveys** of developers and teams
- **Case studies** of practices in industrial context
- **Repository mining** (observational, with or without a causal component)
- **Systematic literature reviews**

**Input:** `$ARGUMENTS` — mode keyword followed by research question or path to research spec.

---

## Modes

### `/strategize [question]` or `/strategize strategy [question]` — Study Design
Design the study: pick the research strategy, specify it, and defend it against threats to validity (construct, internal, external, conclusion).

**Agents:** Strategist → strategist-critic
**Output:** Strategy memo + robustness plan + falsification/sensitivity tests

Workflow:
1. **Pre-Strategy Report (mandatory).** Before proposing any design, the Strategist must output a structured report proving it read the discovery inputs:

```markdown
## Pre-Strategy Report
**Research spec:** [path or "not found"]
**Literature review:** [path or "not found"]
**Data assessment:** [path or "not found"]
**Domain profile:** [loaded / not found]

**Research question:** [one sentence from spec]
**Key findings from literature:**
- [What methods have been used for this question]
- [What gaps remain]
**Available data:**
- [Dataset name] — [key variables, coverage, access — e.g., GitHub API, GH Archive, SEART GHS, TravisTorrent, Stack Overflow, company telemetry]
- [Variation or population available for the design]: [describe]
**Candidate designs from domain profile:** [list relevant designs — causal / experiment / survey / case study / mining / SLR]

Proceeding to study design.
```

If research spec, literature review, or data assessment are missing, the Strategist proceeds with ASSUMED placeholders — but flags each clearly.

2. Read .claude/references/domain-profile.md for common study designs in the field
3. Dispatch Strategist to produce:
   - Strategy memo: design choice, estimand (for causal designs) or research questions (RQ1, RQ2, ...), assumptions, comparison group or sampling frame
   - Pseudo-code or protocol: implementation sketch (Python tools first — pyfixest, linearmodels, statsmodels, rdrobust; R equivalents second)
   - Robustness plan: ordered list of checks with rationale
   - Falsification tests: what SHOULD NOT show effects (causal designs) or triangulation plan (qualitative designs)
   - Referee objection anticipation: top 5 objections with responses, organized by validity type (construct, internal, external, conclusion)
4. Dispatch strategist-critic to review through 4 phases:
   - Phase 1: Claim identification (design, estimand or RQs, treatment/exposure, comparison)
   - Phase 2: Core design validity (assumption checks, threats to validity, sanity checks)
   - Phase 3: Inference soundness (clustering, effect sizes, multiple testing, power)
   - Phase 4: Polish and completeness (robustness, citations)
5. If CRITICAL issues found, iterate (max 3 rounds per three-strikes)
6. Save memo to `quality_reports/strategy_memo_[topic].md`
7. Save review to `quality_reports/strategy_memo_[topic]_review.md`
8. Generate HTML version and refresh dashboard:
   ```bash
   python3 scripts/generate_html_report.py strategy-review quality_reports/strategy_memo_[topic]_review.md
   python3 scripts/generate_dashboard.py
   ```
9. **Save decision record** → `quality_reports/decisions/strategy_[topic].md`
   Using `templates/decision-record.md`, record:
   - **Decision:** The chosen study design (design + estimator or protocol)
   - **Alternatives:** Other designs the Strategist considered (e.g., DiD, controlled experiment, survey, case study, mining-only)
   - **Why rejected:** For each, the specific reason (no exogenous variation, participants unavailable, single-project access only, purely correlational data, etc.)
   - **Key assumptions:** What must hold (parallel trends, exclusion restriction, continuity, task construct validity, sampling-frame coverage, etc.)
   - **What would invalidate:** What findings would force a design change (pre-trends failure, weak first stage, manipulation at cutoff, failed pilot, response rate collapse)

### `/strategize pap [spec]` — Pre-Registration Plan
Draft a pre-registration plan following OSF standards, or prepare a registered report for venues that offer that track (EMSE, ESEM, MSR).

**Input:** `$ARGUMENTS` — path to research spec file, a topic, or `interactive` for guided interview.

- If `$ARGUMENTS` includes a file path: read it (research spec from `/discover interview`)
- If `$ARGUMENTS` includes `interactive`: conduct the guided PAP interview (see below)
- Otherwise: treat as topic and draft with ASSUMED placeholders marked clearly

**Agents:** Strategist (in PAP mode), optionally strategist-critic
**Output:** Pre-registration plan document

#### Interactive PAP Interview (6-Question Guided Flow)

When invoked as `/strategize pap interactive`, ask these questions one at a time before drafting:

1. **What is the research question?**
2. **What is the study design?** (controlled experiment / quasi-experiment around a platform or policy change / observational repository mining / survey)
3. **What are the primary outcome variables?** (names, measurement, data source — e.g., review latency, defect density, build success rate)
4. **What is the source of identifying variation?** (randomization mechanism / treatment assignment / policy rollout / platform change — or, for descriptive mining, the sampling frame)
5. **What subgroup analyses are pre-specified?** (with justification for each)
6. **What multiple testing concerns exist?** (number of primary outcomes, family-wise error rate plan)

After all 6 answers are collected, proceed to PAP drafting.

#### PAP Sections

Dispatch Strategist in PAP mode to produce all standard sections:

1. **Study overview** — research question, design, treatment/exposure, comparison
2. **Outcomes** — primary, secondary, mechanism variables with measurement details and construct validity notes
3. **Estimating equations or analysis protocol** — with full notation protocol
4. **Subgroup analyses** — pre-specified, with justification for each
5. **Multiple testing correction** — Bonferroni / Benjamini-Hochberg / Romano-Wolf (specify which and why)
6. **Power calculations** — MDE, baseline statistics, sample size, assumptions stated explicitly with sensitivity
7. **Sample and exclusion rules** — inclusion criteria, attrition handling, outlier treatment, bot filtering (mining studies)
8. **Data and analysis** — sources, software, randomization/assignment mechanism
9. **Timeline** — data collection, analysis, registration dates
10. **Deviations log** — empty template for tracking post-registration changes

#### Registration Options

Ask the user which route they plan to use (if unclear from context):

**OSF (Open Science Framework)** — the default registry for empirical SE:
- Flexible format. Works for experiments, quasi-experiments, mining studies, and surveys.
- Allows iterative updates with version history.
- Supports pre-registration of observational/archival studies (register before analyzing the mined data).
- Use `templates/pap-templates/osf-experiment.md` for controlled experiments with human subjects.
- Use `templates/pap-templates/osf.md` for observational and quasi-experimental mining studies.

**Registered Report** — for venues with a registered-reports track (EMSE, ESEM, MSR):
- Stage 1: introduction, method, and analysis plan are peer-reviewed BEFORE data collection/analysis.
- In-principle acceptance means the paper is published regardless of results.
- Strongest commitment device; also the best fit when null results are likely and valuable.
- Draft the Stage 1 manuscript from the same PAP content; check the venue's registered-report guidelines.

#### Observational Study PAP Adaptation

For observational, quasi-experimental, or mining designs, adapt the PAP template:

- **Source of variation replaces randomization** — describe the platform change, policy rollout, or staggered adoption
- **Comparison group replaces control group** — define which projects/teams are compared to whom and why
- **Identifying assumption discussion** — explicitly state and defend each assumption
- **Placebo and falsification tests** — pre-specify what SHOULD NOT show effects
- **Robustness to specification choices** — pre-commit to bandwidth, functional form, sample restrictions, bot-filter rules, time windows
- **Treatment of confounding concerns** — document known threats (project age, size, domain, activity level) and planned diagnostics

#### ASSUMED Placeholder Safety

**CRITICAL: Flag every ASSUMED item clearly. The researcher must review and approve before registration.**

When drafting a PAP from a topic (without a full research spec or interactive interview), many details will be assumed. For each assumed item:

- Mark it with `[ASSUMED]` in bold
- Explain what was assumed and why
- Provide the most reasonable default but flag it for review

A registered PAP with unchecked assumptions is worse than no PAP. The final section of every PAP must include:

```markdown
## Pre-Registration Checklist

**Review every [ASSUMED] item before registering this plan.**

- [ ] [ASSUMED] Item 1 — [what was assumed]
- [ ] [ASSUMED] Item 2 — [what was assumed]

**Do not register until all items are reviewed and confirmed or corrected.**
```

#### Optional strategist-critic Review

After PAP creation, optionally dispatch the strategist-critic to review:
- Are identifying assumptions clearly stated and defensible?
- Is the estimator or analysis method appropriate for the design?
- Are power calculation assumptions reasonable? Show sensitivity.
- Are pre-specified subgroups justified (not fishing)?
- Are multiple testing corrections appropriate?
- Are any [ASSUMED] items potentially problematic if left uncorrected?

Save review to `quality_reports/pre_analysis_plan_[topic]_review.md`

Save PAP to `quality_reports/pre_analysis_plan_[topic].md`

---

### `/strategize theory [target]` — Formal Methods Section

Produce a formal statistical-methodology section: assumptions, definitions, lemmas, theorems, and proofs.

**When to use:**
- Paper type is **methods paper** (the statistical method or metric is the contribution)
- Paper type is **theory + empirics** (theoretical predictions are tested)
- Paper type is **methodological causal design** (the design contributes a new estimator or identification argument)

**Skip this mode** for applied papers that use off-the-shelf estimators or standard empirical methods — the strategist's memo is sufficient.

**Input:** `$ARGUMENTS` — research question, path to strategy memo, or path to existing paper/draft.

**Agents:** Theorist → theorist-critic
**Output:** Theory memo + assumptions.tex + results.tex + proofs.tex + notation glossary

Workflow:
1. **Pre-Theory Report (mandatory).** Before writing any math, the Theorist must output a structured report showing what was read:

```markdown
## Pre-Theory Report
**Research spec:** [path or "not found"]
**Strategy memo:** [path or "not found"]
**Existing paper/draft:** [path or "not found"]
**Domain profile:** [loaded / not found]
**Notation conventions:** [header.tex path / domain-profile notation table / "not found"]
**Bibliography base:** [path / "not found"]

**Paper type:** [methods paper / theory+empirics / methodological causal design]
**Theoretical object(s) to produce:** [identification / consistency / asymp. normality / influence function / DML / bootstrap / test / proposition / metric validity]
**Data structure:** [iid / panel / staggered / clustered / triangular array]
**Target parameter:** [definition as functional of P]
**Estimator:** [definition]
**Assumptions anticipated:** [A1 sampling, A2 parallel trends, ...]

Proceeding to theory drafting.
```

If strategy memo or paper type is missing, the Theorist flags it and asks before proceeding.

2. Read `.claude/references/domain-profile.md` for the Methodological Foundational References table and Author Team table.
3. Dispatch **Theorist** to produce:
   - `quality_reports/theory/[topic]/theory_memo.md`
   - `quality_reports/theory/[topic]/assumptions.tex`
   - `quality_reports/theory/[topic]/results.tex`
   - `quality_reports/theory/[topic]/proofs.tex`
   - `quality_reports/theory/[topic]/notation_glossary.md`
4. Dispatch **theorist-critic** to review through 4 sequential phases:
   - Phase 1: Claim identification (object type, target parameter, estimator, assumptions)
   - Phase 2: Proof validity (logical, measurability, expansions, identification, asymptotic distribution) — **early-stop on critical gaps**
   - Phase 3: Assumption minimality + statement calibration + notation consistency (INV-7)
   - Phase 4: Citation fidelity + linkage to empirical claims + exposition
5. If CRITICAL issues found, iterate (max 3 rounds per three-strikes). Escalation target: User.
6. Save review to `quality_reports/theory_[topic]_review.md`
7. **Save decision record** → `quality_reports/decisions/theory_[topic].md`
   Record:
   - **Decision:** The theoretical objects proved (identification, asymptotic distribution, etc.)
   - **Assumptions:** Full list with interpretation
   - **What's open:** What the theory does NOT cover (caveats for the writer)
   - **Linkage:** Which empirical claims each theorem supports

---

## Bundled Resources

### Templates
| File | Purpose |
|------|---------|
| `strategize/templates/pre-strategy-report.md` | Mandatory pre-check report before designing the study |
| `strategize/templates/strategy-memo.md` | Strategy memo output format (5 required sections) |
| `strategize/templates/robustness-plan.md` | Ordered robustness checklist template |
| `strategize/templates/theory-memo.md` | Methods section output format (assumptions, results, proofs) |
| `strategize/templates/decision-record.md` | Template for documenting design decisions with alternatives |

### Design Checklists
| File | Design |
|------|--------|
| `strategize/templates/design-checklists/did.md` | Difference-in-Differences (parallel trends, staggered adoption, estimator selection) |
| `strategize/templates/design-checklists/iv.md` | Instrumental Variables (relevance, exclusion, monotonicity, LATE) |
| `strategize/templates/design-checklists/rdd.md` | Regression Discontinuity (bandwidth, manipulation, balance) |
| `strategize/templates/design-checklists/event-study.md` | Event Study (pre-trends, binning, heterogeneity-robust estimators) |
| `strategize/templates/design-checklists/experiment.md` | Controlled Experiment (randomization, tasks, participants, power, effect sizes) |
| `strategize/templates/design-checklists/survey.md` | Survey (sampling frame, instrument validation, non-response bias) |
| `strategize/templates/design-checklists/case-study.md` | Case Study (case selection, triangulation, protocol, member checking) |
| `strategize/templates/design-checklists/msr.md` | Repository Mining (repository selection, bot filtering, confounds, causal-claim discipline) |
| `strategize/templates/design-checklists/descriptive.md` | Descriptive/Measurement (construction, validation, no causal language) |

### PAP Templates
| File | Use |
|------|-----|
| `strategize/templates/pap-templates/osf-experiment.md` | OSF pre-registration for controlled experiments with human subjects |
| `strategize/templates/pap-templates/osf.md` | OSF pre-registration for observational and quasi-experimental mining studies |

### References
| File | Purpose |
|------|---------|
| `strategize/references/pap-interview-flow.md` | 6-question guided interview for building a PAP interactively |

### Gotchas
| File | Purpose |
|------|---------|
| `strategize/gotchas.md` | Known failure points: design selection traps, memo pitfalls, PAP anti-patterns, theory-mode caveats |

---

## Principles

- **Strategist proposes, strategist-critic critiques.** Adversarial pairing catches design flaws early.
- **Theorist proves, theorist-critic checks the proof.** Proof validity gates everything downstream -- notation, citations, polish.
- **Strategy memo is the contract.** Once approved, the Coder implements it faithfully.
- **Catch problems before coding.** A flawed design caught now saves weeks of wasted analysis.
- **Match the design to the claim.** Causal claims need causal designs; correlational mining supports only correlational claims.
- **Threats to validity are first-class.** Every memo addresses construct, internal, external, and conclusion validity.
- **Multiple designs are OK.** Present trade-offs and let the user choose.
- **The user decides.** If Strategist and strategist-critic disagree after 3 rounds, the user resolves it.
- **Pre-specification is the point.** Everything in a PAP is decided before seeing outcomes.
- **Be honest about what's exploratory.** Label subgroups and secondary outcomes clearly.
- **Power calculations require assumptions.** State every assumption. Show sensitivity.
- **A PAP is a commitment device.** Make sure the researcher understands what they're committing to.
