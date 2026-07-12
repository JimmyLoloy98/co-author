# OSF Pre-Registration — Controlled Experiment in Software Engineering

**Registry:** [OSF Registries](https://osf.io/registries/)
**Use for:** Controlled experiments with human subjects — tool evaluations, practice comparisons, task-based studies with developers or students.
**Requirement:** Register BEFORE participants are recruited or data collection begins.
**Companion standards:** Wohlin et al. (2012), *Experimentation in Software Engineering*; ACM SIGSOFT Empirical Standards (Experiments); consider a registered report at EMSE/ESEM/MSR instead if null results are likely and valuable.

---

## 1. Title and Authors

**Title:** [Study title]
**Authors:** [Full author list with affiliations]
**Registration date:** [YYYY-MM-DD]
**Pre-registration plan date:** [YYYY-MM-DD]

---

## 2. Study Information

**Abstract:** [150 words or fewer describing the study]
**Study type:** [Controlled experiment / quasi-experiment with human subjects]
**Keywords:** [list]
**ACM CCS concepts:** [e.g., Software and its engineering~Software development process management]

**Ethics / IRB information:**
- Approving institution: [name]
- Approval number: [number]
- Approval date: [date]
- Informed consent procedure: [describe]
- Data protection: [anonymization, storage, access, retention]

**Funding sources:** [list all]
**Conflicts of interest:** [e.g., authors developed the tool under evaluation — disclose]

---

## 3. Hypotheses

### Primary hypotheses
1. **H1:** [precise, falsifiable — e.g., "Developers using tool-assisted code review find more defects per hour than developers using manual review"]
   - Direction: [one-sided / two-sided]
2. **H2:** [precise, falsifiable statement]

### Secondary hypotheses
3. **H3:** [precise, falsifiable statement]

---

## 4. Design

### Experimental design
- **Design type:** [between-subjects / within-subjects / mixed / crossover]
- **Treatment conditions:** [describe each condition, e.g., "code review with static-analysis assistance" vs. "manual code review"]
- **Randomization method:** [simple / blocked / stratified — on what variables, e.g., experience level]
- **Randomization unit:** [participant / team / session]
- **Counterbalancing:** [task and period order, if within-subjects — Latin square, etc.]
- **Blinding:** [can participants be blinded to condition? Can graders/coders be blinded?]

### Participants
- **Target population:** [professional developers / graduate students / OSS contributors]
- **Recruitment:** [how participants are recruited — company, course, platform]
- **Sample size:** [N per condition, total N]
- **Eligibility criteria:** [experience requirements, language proficiency, inclusion/exclusion rules]
- **Compensation:** [amount and form]
- **Representativeness:** [how participants relate to the target population — threats to external validity]

### Tasks
- **Task description:** [what participants do — e.g., review three pull requests with seeded defects]
- **Task provenance:** [real-world tasks / adapted from real projects / constructed — justify realism]
- **Construct validity of tasks:** [why these tasks measure the construct of interest]
- **Task duration:** [expected time per task and total session length]
- **Pilot:** [pilot study plan — participants, purpose, what would trigger task revision]

---

## 5. Outcomes

### Primary outcomes
| Variable | Measurement | Data source | Timing |
|----------|-------------|-------------|--------|
| [e.g., defects found] | [count of seeded defects correctly identified] | [task artifacts / screen recording] | [per task] |
| [e.g., task completion time] | [minutes, logged automatically] | [instrumented environment] | [per task] |

### Secondary outcomes
| Variable | Measurement | Data source | Timing |
|----------|-------------|-------------|--------|
| [e.g., perceived usability] | [SUS or validated scale] | [post-task questionnaire] | [end of session] |

### Mechanism / process variables
| Variable | Measurement | Channel tested |
|----------|-------------|---------------|
| [e.g., navigation patterns] | [event logs] | [which mechanism] |

---

## 6. Analysis Plan

**Primary analysis:**
- **Test:** [e.g., Mann-Whitney U (between-subjects) / Wilcoxon signed-rank (within-subjects) — SE task metrics are typically non-normal; state the parametric fallback and the normality check]
- **Effect sizes:** [Cliff's delta or Vargha-Delaney Â12 with confidence intervals — required alongside p-values, per Arcuri & Briand (2014)]
- **Model (if regression-based):**
$$
Y_{ij} = \alpha + \beta T_i + X_i'\gamma + \varepsilon_{ij}
$$
  - $Y_{ij}$: [outcome for participant i on task j]
  - $T_i$: [treatment indicator]
  - $X_i$: [covariates — experience, blocking variables]
  - Standard errors: [robust / clustered at participant level for repeated tasks]
- **Software:** [Python primary — scipy.stats, statsmodels, pingouin; R secondary]

**Demand effects and confound handling:**
- [How demand characteristics / experimenter expectancy are mitigated — neutral instructions, blinded grading]
- [Learning and fatigue effects (within-subjects): counterbalancing analysis plan]

**Heterogeneity (pre-specified):**
$$
Y_{ij} = \alpha + \beta_1 T_i + \beta_2 T_i \times H_i + \beta_3 H_i + X_i'\gamma + \varepsilon_{ij}
$$
- $H_i$: [heterogeneity dimension — e.g., experience level; justify each]

---

## 7. Subgroup Analyses

| Subgroup | Justification | Pre-specified? |
|----------|---------------|---------------|
| [Experience level] | [novices may benefit more from tool assistance] | Yes |
| [Task type] | [effect may differ for defect classes] | Yes |

---

## 8. Multiple Testing

- **Number of primary outcomes:** [N]
- **Correction method:** [Bonferroni / Benjamini-Hochberg / Romano-Wolf]
- **Family structure:** [which outcomes are grouped into families]
- **Adjusted significance level:** [if Bonferroni: 0.05/N]

---

## 9. Power Analysis / MDE

| Parameter | Value | Source |
|-----------|-------|--------|
| Significance level ($\alpha$) | 0.05 | Standard |
| Power ($1-\beta$) | 0.80 | Standard |
| Minimum detectable effect (MDE) | [value, in effect-size units — e.g., Â12 = 0.64] | [pilot / prior studies] |
| Baseline mean | [value] | [pilot / prior studies] |
| Baseline SD | [value] | [pilot / prior studies] |
| ICC (if clustered/teams) | [value] | [prior studies] |
| Sample size per condition | [N] | |
| Total sample size | [N] | |

**Sensitivity:** MDE at power 0.80 for alternative sample sizes:
| N per condition | MDE |
|-----------------|-----|
| [N1] | [MDE1] |
| [N2] | [MDE2] |

---

## 10. Data and Analysis Infrastructure

- **Software:** [Python (primary) / R (secondary)]
- **Analysis scripts:** [will be deposited at {location} — target ACM/IEEE artifact badges: Available, Functional, Reusable]
- **Data availability:** [public anonymized / restricted / embargoed until {date}]
- **Dropout/attrition handling:** [exclusion rules, intention-to-treat stance]
- **Outlier treatment:** [winsorize at {percentile} / rank-based methods / robust regression]
- **Missing data:** [listwise deletion / imputation method]

---

## 11. Timeline

| Milestone | Date |
|-----------|------|
| OSF registration | [date] |
| Pilot study | [date] |
| Participant recruitment | [date] |
| Experiment sessions | [start — end] |
| Analysis | [date] |

---

## 12. Deviations Log

| Date | Deviation | Reason | Impact |
|------|-----------|--------|--------|
| | [empty — to be filled post-registration] | | |
