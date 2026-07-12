# Notation Protocol -- Standard Conventions (Empirical SE)

Notation and naming conventions for consistency across all sections of the paper.

---

## Research Question Numbering

- Research questions are numbered **RQ1, RQ2, ...** and stated verbatim in the Introduction and the Study Design section
- Every RQ maps to at least one analysis, and every analysis answers a stated RQ -- no orphan analyses
- Hypotheses (experiments) are numbered **H1, H2, ...** (with H1$_0$/H1$_a$ for null/alternative when stated formally) and linked to their RQ
- Results subsections are organized by RQ and end with an explicit answer statement

## Metric Naming

- Name metrics by what they measure, with units: *review latency (hours)*, *defect density (defects per KLOC)*, *commits per month*, *team size (active contributors)*, *build success rate (%)*
- Define every metric once, in the Data section, and use the identical name everywhere (text, tables, figures)
- Binary practice indicators read as predicates: *uses CI*, *has code review policy*
- Skewed metrics (LOC, churn, resolution time) note their transformation at definition (log, rank)
- Table/figure column names must match the metric names in the text

---

## Core Variables (Causal Designs)

For causal mining studies (DiD, IV, RDD, event study on repository data), use panel notation:

| Symbol | Meaning | Usage |
|--------|---------|-------|
| $Y_{it}$ | Outcome metric | Unit $i$ (project, repo, team), time $t$ |
| $D_{it}$ | Treatment indicator | Binary or continuous treatment (e.g., CI adopted) |
| $X_{it}$ | Control variables | Covariate vector (e.g., team size, project age) |
| $\gamma_i$ | Unit fixed effect | Absorbs time-invariant project heterogeneity |
| $\delta_t$ | Time fixed effect | Absorbs common time shocks (platform changes) |
| $\varepsilon_{it}$ | Error term | Idiosyncratic shock |

---

## Rules

- **Consistent throughout** -- the same symbol never means two things across sections
- **Define every symbol at first use** -- no implicit notation
- **Match the strategy memo** -- if the coder's naming map uses specific notation, the paper must match
- **Subscripts matter** -- $i$ for units (projects, repos), $t$ for time, $g$ for adoption cohorts/groups, $j$ for secondary units (developers, PRs within projects)
- **Hats for estimates** -- $\hat{\beta}$ for estimated coefficients, $\beta$ for population parameters
- **Bold for vectors/matrices** -- $\mathbf{X}$ for the control matrix, $X_{it}$ for a single observation's controls

---

## Common Estimands (for Causal Mining Studies)

These targets apply when the paper uses a causal design; descriptive, survey, and case-study papers do not state estimands.

| Estimand | Symbol | Definition |
|----------|--------|------------|
| Average Treatment Effect | $\text{ATE}$ | $E[Y_i(1) - Y_i(0)]$ |
| Average Treatment Effect on the Treated | $\text{ATT}$ | $E[Y_i(1) - Y_i(0) \mid D_i = 1]$ |
| Local Average Treatment Effect | $\text{LATE}$ | $E[Y_i(1) - Y_i(0) \mid \text{compliers}]$ |
| Group-time ATT | $\text{ATT}(g,t)$ | Callaway-Sant'Anna notation for staggered adoption |

---

## Effect Sizes (Group Comparisons)

For experiments and group comparisons, report standardized effect sizes with confidence intervals per the ACM SIGSOFT Empirical Standards:

| Measure | Symbol | Use |
|---------|--------|-----|
| Cliff's delta | $\delta$ (Cliff) | Ordinal/non-normal outcome comparisons |
| Vargha-Delaney | $\hat{A}_{12}$ | Probability that treatment outperforms control |

Note: Cliff's $\delta$ is always written with the qualifier "(Cliff)" or in context that cannot be confused with the time fixed effect $\delta_t$.
