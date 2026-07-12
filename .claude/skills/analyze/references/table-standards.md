# Table Standards

Publication-quality tables using standard academic formatting (booktabs rules, no vertical rules). Two approaches are supported:

- **tabularray (`tblr` / `talltblr`)** -- modern key-value interface. Preferred for hand-written tables in `main.tex`.
- **`tabular` + `booktabs` + `threeparttable`** -- traditional stack. Required for Python/R-generated output (scripts export bare `tabular`).

Venue-specific conventions (significance reporting, note format) adapt to the target venue -- see journal-profiles.md.

---

## No In-Table Titles or Notes

- **Never** embed titles inside the table body or as a table header row
- **Never** embed notes, sources, or footnotes inside the table itself
- Table numbering, titles, and notes are added in LaTeX via `\caption{}` and `\begin{tablenotes}` (or tabularray's `note{}` key)
- The file name and folder identify what the table contains

---

## Three-Line Format (Booktabs)

Every table uses exactly three horizontal rules and **zero vertical lines**:

**Traditional (Python/R output):**
```latex
\begin{table}[htbp]
\centering
\begin{threeparttable}
\caption{Effect of X on Y}\label{tab:main}
\begin{tabular}{lcccc}
\toprule
            & (1)     & (2)     & (3)     & (4)     \\
\midrule
...coefficients...
\bottomrule
\end{tabular}
\begin{tablenotes}\small
\item \textit{Notes:} Standard errors in parentheses.
\end{tablenotes}
\end{threeparttable}
\end{table}
```

**Modern (hand-written in main.tex):**
```latex
\begin{talltblr}[
  caption = {Effect of X on Y},
  label = {tab:main},
  note{*} = {Standard errors in parentheses.},
]{colspec = {lcccc}, rowsep = 4pt}
\toprule
            & (1)     & (2)     & (3)     & (4)     \\
\midrule
...coefficients...
\bottomrule
\end{talltblr}
```

- `\toprule` above column headers
- `\midrule` below column headers (and to separate panels)
- `\bottomrule` at the very end
- `\cmidrule(lr){2-4}` for partial rules spanning column groups
- **Python/R output:** wrap with `threeparttable` for notes via `\begin{tablenotes}`
- **Hand-written tables:** prefer `talltblr` with `note{}` keys -- unifies caption, label, and notes
- **Never** use `\hline`, `|`, or any vertical rules

---

## Coefficient Display

- Point estimates on one row, standard errors in parentheses on the row below
- Standard errors labeled in the table note (e.g., "Robust standard errors in parentheses" or "Clustered at project level")

**Significance reporting depends on the target venue (INV-4):**

| Context | Convention |
|---------|-----------|
| **Working papers (default)** | Stars: `*` p < 0.10, `**` p < 0.05, `***` p < 0.01. Note at bottom: `\textit{Notes:} * p < 0.10, ** p < 0.05, *** p < 0.01` |
| **Empirical-standards venues (EMSE, ESEM, MSR)** | Effect sizes (Cliff's delta, Vargha-Delaney Â12, or standardized coefficients) with 95% confidence intervals alongside p-values, per the ACM SIGSOFT Empirical Standards. Stars acceptable only in addition, and only if defined in the note. |
| **All other venues** | Stars acceptable if defined in table notes. Follow venue-specific conventions in journal-profiles.md. |

Working paper default example:
```
Uses CI          & 0.045**  & 0.038*   & 0.052*** \\
                 & (0.021)  & (0.020)  & (0.019)  \\
```

Empirical-standards venue example (effect-size and CI rows):
```
Uses CI          & 0.045    & 0.038    & 0.052    \\
                 & (0.021)  & (0.020)  & (0.019)  \\
95\% CI          & [0.004, 0.086] & [-0.001, 0.077] & [0.015, 0.089] \\
Cliff's $\delta$ & 0.21     & 0.18     & 0.24     \\
```

---

## Column and Row Structure

- **Column (1), (2), ...** headers in the first row after `\toprule`
- **Dependent variable** stated in a spanning header or the first subheader row
- **Variable names** left-aligned, human-readable (not raw code names)
  - `Log review latency` not `ln_review_latency_hrs`
  - `Uses CI` not `has_ci_yml`
  - `Team size` not `n_devs_90d`
- **Numeric columns** right-aligned or decimal-aligned
- **N**, **R-squared**, **Fixed effects** (Yes/No), **Controls** (Yes/No) at the bottom before `\bottomrule`

---

## Panel Structure

For tables with multiple panels:

```latex
\multicolumn{5}{l}{\textit{Panel A: All projects}} \\
\midrule
...
\\[0.5em]
\multicolumn{5}{l}{\textit{Panel B: Projects with 10+ contributors}} \\
\midrule
...
```

- Panel labels in italics, left-aligned, spanning all columns
- `\midrule` after each panel label
- Small vertical space (`\\[0.5em]`) between panels

---

## Preferred Packages

**Primary (Python): `pyfixest.etable`**

```python
import pyfixest as pf

m1 = pf.feols("defect_density ~ uses_ci | project + period",
              df, vcov={"CRV1": "project"})

tex = pf.etable(
    [m1],
    type="tex",
    signif_code=[0.01, 0.05, 0.10],   # add effect-size/CI rows for EMSE/ESEM/MSR
    coef_fmt="b \n (se)",
    labels={
        "uses_ci":   "Uses CI",
        "team_size": "Team size",
    },
)
# Export bare tabular only -- strip any float wrapper before writing (INV-13)
```

**Primary (Python) for summary / descriptive tables: `pandas.DataFrame.to_latex`**

```python
tex = df.style.hide(axis="index").to_latex(
    column_format="l" + "c" * (df.shape[1] - 1),
    hrules=True,  # booktabs rules
)
(TABLE_DIR / "sumstats_main_sample.tex").write_text(tex)
```

**Secondary (R): `modelsummary`**

```r
library(modelsummary)

modelsummary(
  models,
  output   = "latex_tabular",  # bare tabular, no wrapper
  stars    = c("*" = 0.10, "**" = 0.05, "***" = 0.01),  # add effect-size rows for EMSE/ESEM/MSR
  coef_rename = c(
    "uses_ci"   = "Uses CI",
    "team_size" = "Team size"
  ),
  gof_map = c("nobs", "r.squared", "adj.r.squared"),
  escape  = FALSE
)
```

**Secondary (R): `fixest::etable`**

```r
fixest::etable(
  models,
  tex      = TRUE,
  style.tex = style.tex(
    depvar.title = "",
    fixef.title  = "",
    yesNo    = c("Yes", "No")
  ),
  se.below = TRUE,
  signif.code = c("***" = 0.01, "**" = 0.05, "*" = 0.10)
)
```

**Secondary (R) for summary / descriptive tables: `kableExtra`**

```r
library(kableExtra)

kbl(df, format = "latex", booktabs = TRUE, escape = FALSE,
    align = c("l", rep("c", ncol(df) - 1))) |>
  kable_styling(latex_options = "hold_position")
```

---

## Typography

- Serif font throughout (inherits from document class -- no extra commands needed)
- `\small` or `\footnotesize` for tables that need to fit within column width
- Variable names in plain text, panel labels in `\textit{}`
- Never bold table body content; bold only for rare emphasis in headers

---

## Export

```python
# Primary: write .tex fragment (no \begin{table} wrapper -- added in main.tex)
(ROOT / "paper" / "tables" / "reg_main_specification.tex").write_text(tex_output)
```

```r
# Secondary (R)
writeLines(tex_output, here("paper", "tables", "reg_main_specification.tex"))
```

- Output **bare `tabular` environment** (no `\begin{table}` float)
- The paper's `main.tex` wraps it with `\begin{table}`, `\caption{}`, and `\input{}`
- Write to `paper/tables/`

---

## File Naming

```
tables/
  descriptive/
    sumstats_main_sample.tex
    balance_treatment_control.tex
  estimation/
    reg_main_specification.tex
    reg_heterogeneity_language.tex
    did_event_study_coefficients.tex
  robustness/
    reg_alternative_controls.tex
```

Pattern: `{table_type}_{content_description}.tex`

- `sumstats_` for summary statistics
- `balance_` for balance / pre-treatment tests
- `reg_` for regression output
- `did_` for difference-in-differences specific tables
- `first_stage_` for IV first stage
- `survey_` for survey-response tabulations
- `irr_` for inter-rater reliability (e.g., Cohen's kappa) tables

---

## Table Type Templates

Use these as defaults. Adapt columns based on the paper's needs.

**Descriptive Statistics:**
```
\toprule
                             &  Mean   &  SD     \\
\midrule
\multicolumn{3}{l}{\textit{Continuous variables}} \\
\quad Commits per month      &  38.4   &  22.1   \\
\quad Review latency (hours) &  17.2   &  14.8   \\
\quad Team size              &  6.5    &  4.2    \\
\\[0.5em]
\multicolumn{3}{l}{\textit{Binary variables (\%)}} \\
\quad Uses CI                &  62.3   &         \\
\quad Code review required   &  41.5   &         \\
\bottomrule
```
- Default: Mean and SD in separate columns (never stacked with parentheses)
- Categorical/binary: percentage in Mean column, SD blank
- Sample size stated once in table notes, not as a column
- Add Median (and Min/Max) when metrics are heavily skewed -- common with SE metrics like LOC, churn, resolution time

**Regression Results:**
```
\toprule
                        &  (1)    &  (2)    &  (3)    &  (4)    \\
                        &  OLS    &  OLS    &  IV     &  IV     \\
\midrule
Uses CI                 &  0.045**&  0.038* &  0.052**&  0.041* \\
                        & (0.021) & (0.020) & (0.025) & (0.022) \\
\midrule
Controls                &  No     &  Yes    &  No     &  Yes    \\
Fixed Effects           &  No     &  Yes    &  No     &  Yes    \\
Observations            &  10,000 &  10,000 &  10,000 &  10,000 \\
R$^2$                   &  0.05   &  0.12   &         &         \\
\bottomrule
```

**Multi-Outcome (Panel Structure):**
```
\toprule
                        &  (1)    &  (2)    &  (3)    &  (4)    \\
\midrule
\multicolumn{5}{l}{\textit{Panel A: Defect density}} \\
\midrule
Uses CI                 &  0.045**&  0.038* &  0.052**&  0.041* \\
                        & (0.021) & (0.020) & (0.025) & (0.022) \\
\\[0.5em]
\multicolumn{5}{l}{\textit{Panel B: Build success rate}} \\
\midrule
Uses CI                 &  0.021  &  0.033* &  0.015  &  0.028  \\
                        & (0.018) & (0.017) & (0.020) & (0.019) \\
\midrule
Controls                &  No     &  Yes    &  No     &  Yes    \\
Fixed Effects           &  No     &  Yes    &  No     &  Yes    \\
Observations            &  10,000 &  10,000 &  10,000 &  10,000 \\
\bottomrule
```

**Balance Table:**
```
\toprule
Variable                &  Treatment &  Control &  Difference &  SE     &  p-value \\
\midrule
Commits per month       &  39.1      &  37.6    &  1.5        &  (1.1)  &  0.197   \\
Team size               &  6.7       &  6.4     &  0.3        &  (0.2)  &  0.134   \\
Project age (years)     &  4.8       &  4.9     &  -0.1       &  (0.2)  &  0.505   \\
\bottomrule
```

**Robustness:**
```
\toprule
                        &  (1)        &  (2)           &  (3)          &  (4)            \\
                        &  Baseline   &  Alt. controls &  Alt. sample  &  Alt. estimator \\
\midrule
```
- Column headers describe what changes across specifications
- Same outcome variable across all columns

---

## Prohibited Patterns

| Pattern | Reason |
|---------|--------|
| Title row inside the table | Titles go in `\caption{}`, not the table body |
| Notes embedded in table body | Notes go below via `\begin{tablenotes}` |
| `\hline` | Use `\toprule` / `\midrule` / `\bottomrule` (booktabs) |
| Vertical rules (`\|` in column spec) | Never used in journal-quality tables |
| `stargazer` package (R) | Deprecated workflow; use `modelsummary` or `fixest::etable` |
| Raw variable names in labels | Human-readable labels required |
| `xtable` without booktabs | Produces non-journal-quality output |
| `\begin{table}` in script output | Scripts export bare `tabular`; float wrapper lives in `main.tex` |
| Stars without effect sizes at EMSE/ESEM/MSR | Empirical-standards venues require effect sizes + CIs (INV-4) |
