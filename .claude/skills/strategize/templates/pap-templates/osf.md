# OSF Pre-Registration — Observational / Quasi-Experimental Mining Study

**Registry:** [OSF Registries](https://osf.io/registries/)
**Use for:** Repository mining studies — observational analyses of GitHub/Jira/CI data, and quasi-experimental designs around platform changes, policy rollouts, or staggered practice adoption (e.g., CI adoption, code-review policy introduction).
**Versioning:** Supports iterative updates with version history. Register before analyzing the mined data (declare what you have already seen).
**Companion standards:** ACM SIGSOFT Empirical Standards (Repository Mining); registered-report tracks at EMSE/ESEM/MSR are an alternative with in-principle acceptance.

---

## 1. Study Information

### Title
[Study title]

### Authors
[Full author list]

### Description
[Abstract: what you plan to do and why. 300 words or fewer.]

### Hypotheses
List each hypothesis with a number. Specify direction (one-sided or two-sided).

1. **H1:** [e.g., "CI adoption reduces median code-review latency in the following 12 months"]
   - Direction: [positive / negative / two-sided]
   - Rationale: [brief justification]

2. **H2:** [hypothesis]
   - Direction: [positive / negative / two-sided]
   - Rationale: [brief justification]

---

## 2. Design Plan

### Study type
- [ ] Quasi-experiment around a platform/policy change (DiD, event study, RDD)
- [ ] Observational repository mining (correlational — no causal claims)
- [ ] Natural experiment (exogenous shock to projects/teams)
- [ ] Meta-analysis / systematic literature review
- [ ] Other: [specify]

### Study design
[Describe the data structure and comparison strategy. E.g., "Project-month panel of 5,000 GitHub projects, 2015-2024; staggered DiD comparing projects that adopt CI to not-yet-adopters."]

### Source of identifying variation (quasi-experimental designs)
[What creates the "as-if random" variation in treatment? E.g., "GitHub Actions launch date is a platform-level shock; project-level adoption timing is staggered." Be honest about endogenous adoption timing.]

---

## 3. Sampling Plan

### Existing data
- [ ] Registration prior to creation of data
- [ ] Registration prior to any human observation of the data
- [ ] Registration prior to accessing the data
- [ ] Registration prior to analysis of the data
- [ ] Registration after some analysis (explain exactly what has been seen)

### Data sources and access
[E.g., GitHub REST/GraphQL API, GH Archive, SEART GHS, World of Code, TravisTorrent, Stack Overflow dump, Jira datasets, company telemetry (with ethics approval). Describe coverage and access procedure.]

### Repository / unit selection criteria
[Pre-commit the sampling frame. E.g., SEART GHS query: >= 10 contributors, >= 500 commits, not a fork, active in the last year, primary language in {Java, Python}. Justify each criterion. Note: stars alone do not indicate an engineered project.]

### Data curation rules (pre-committed)
- **Bot filtering:** [method — e.g., name heuristics + BoDeGHa/BIMAN; list of known bots]
- **Identity merging:** [how developer aliases are merged — email/name heuristics, tooling]
- **Survivorship handling:** [do deleted/archived projects enter the sample? how?]
- **Time windows:** [observation window per project; alignment to treatment timing]

### Sample size
[Expected number of projects/PRs/commits and justification. For causal designs: number of treated units and MDE at that sample size.]

### Stopping rule
[What determines the sample boundary — date range, API quota, query freeze date.]

---

## 4. Variables

### Treatment / exposure variables
[E.g., "uses CI" — first month with a CI configuration file and a recorded build. Define exactly when treatment switches on and for whom.]

### Measured / outcome variables
| Variable | Measurement | Type |
|----------|-------------|------|
| [e.g., review latency] | [hours from PR open to first review, median per project-month] | Primary |
| [e.g., defect density] | [bug-labeled issues per KLOC] | Primary |
| [e.g., build success rate] | [share of CI runs passing] | Secondary |

**Construct validity notes:** [why each mined metric measures the intended construct; known gaps — e.g., commit count is not productivity]

### Control variables
[Project age, size (LOC), team size, domain, activity level, primary language — justify each; note which are potentially post-treatment ("bad controls") and exclude them.]

### Constructed variables / indices
[If composite measures are constructed: describe the construction method and cite validation.]

---

## 5. Analysis Plan

### Estimating equations
[For each hypothesis, specify the model.]

**H1 test (staggered DiD example):**
$$
Y_{pt} = \alpha_p + \gamma_t + \beta D_{pt} + X_{pt}'\delta + \varepsilon_{pt}
$$
- Estimator: [Callaway-Sant'Anna group-time ATT — Python `differences`/csdid port, R `did` as fallback; never naive TWFE with staggered adoption unless justified]
- Standard errors: [clustered at project level]
- Comparison group: [never-treated / not-yet-treated — pre-commit]

**RDD example (if applicable):**
- Running variable, cutoff, and institutional basis: [describe]
- Bandwidth: [MSE-optimal via `rdrobust` (Python package; R fallback) — pre-commit the selector, not a hand-picked value]
- Manipulation test: [McCrary / Cattaneo-Jansson-Ma density test]

**2SLS example (if applicable):**
- Instrument and exclusion argument: [describe]
- Weak-instrument diagnostics: [effective F; Anderson-Rubin CIs if weak]

### Effect sizes and inference
- Effect sizes with confidence intervals alongside p-values (ACM SIGSOFT Empirical Standards)
- Multiple testing correction: [Bonferroni / BH / Romano-Wolf — method and family structure]
- One-sided vs. two-sided: [specify per hypothesis]
- Software: [Python primary — pyfixest, linearmodels, statsmodels, rdrobust; R secondary]

### Identifying assumptions (quasi-experimental designs)
| Assumption | Statement | Credibility | Test |
|-----------|-----------|-------------|------|
| [Parallel trends] | [formal/informal] | [why believable for adopter vs. non-adopter projects] | [pre-trends event study] |
| [No anticipation] | [statement] | [why believable] | [t=-1 coefficient] |

### Placebo and falsification tests (pre-specified)
| Test | Logic | Expected result |
|------|-------|-----------------|
| [Placebo outcome — e.g., star count] | [should not be affected by CI adoption] | [null effect] |
| [Placebo timing] | [fake adoption date 12 months earlier] | [null effect] |

### Pre-committed specification choices
- Bandwidth (RDD): [selection method]
- Functional form: [levels / log — skewed SE metrics usually logged or rank-based]
- Sample restrictions: [list, committed before seeing results]
- Bot-filter rules and time windows: [as in Section 3 — no post-hoc changes without a deviations-log entry]

### Transformations
[Log transforms for skewed metrics (LOC, churn, latency), winsorization percentiles, standardization.]

### Exclusion criteria
[Rules for excluding observations. Specify before analysis.]

### Missing data
[Listwise deletion, imputation, bounds.]

### Exploratory analysis
[Analyses NOT pre-specified. Label clearly as exploratory.]

---

## 6. Causal-Claim Discipline

- If the design is **observational mining without a causal design**: commit to correlational language ("associated with", "predicts") in all outputs. No causal claims.
- If the design is **quasi-experimental**: causal claims are limited to the estimand identified by the design, with the threats-to-validity discussion pre-outlined here (construct, internal, external, conclusion).

---

## 7. Open Science

- **Data and scripts:** [deposit location; target ACM/IEEE artifact badges: Available, Functional, Reusable]
- **Ethics:** [mining public data — terms-of-service compliance, no deanonymization; telemetry — ethics approval reference]

---

## 8. Deviations Log

| Date | Deviation | Reason | Impact |
|------|-----------|--------|--------|
| | [to be filled post-registration] | | |
