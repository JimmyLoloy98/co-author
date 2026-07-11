---
paths:
  - "**/*.py"
  - "**/*.R"
  - "**/*.tex"
  - "paper/tables/**"
  - "paper/figures/**"
  - "master_supporting_docs/**"
  - "explorations/**"
---

# Content Standards: Tables, Figures, PDFs, and Explorations

---

## 1. Table Standards

**Target:** Publication-quality tables using standard academic formatting (booktabs rules, no vertical rules). Two approaches are supported:

- **tabularray (`tblr` / `talltblr`)** — modern key-value interface. Preferred for hand-written tables in `main.tex`.
- **`tabular` + `booktabs` + `threeparttable`** — traditional stack. Required for Python/R-generated output (scripts export bare `tabular`).

Venue-specific conventions (significance reporting, note format) adapt to the target venue — see journal-profiles.md.

### No In-Table Titles or Notes

- **Never** embed titles inside the table body or as a table header row
- **Never** embed notes, sources, or footnotes inside the table itself
- Table numbering, titles, and notes are added in LaTeX via `\caption{}` and `\begin{tablenotes}` (or tabularray's `note{}` key)
- The file name and folder identify what the table contains

### Three-Line Format (Booktabs)

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
- **Hand-written tables:** prefer `talltblr` with `note{}` keys — unifies caption, label, and notes
- **Never** use `\hline`, `|`, or any vertical rules

### Coefficient Display

- Point estimates on one row, standard errors in parentheses on the row below
- Standard errors labeled in the table note (e.g., "Robust standard errors in parentheses" or "Clustered at project level")

**Significance reporting depends on the target venue:**

| Context | Convention |
|---------|-----------|
| **Working papers (default)** | Stars: `*` p < 0.10, `**` p < 0.05, `***` p < 0.01. Note at bottom: `\textit{Notes:} * p < 0.10, ** p < 0.05, *** p < 0.01` |
| **Empirical-standards venues** (EMSE, ESEM, MSR) | Effect sizes (Cliff's delta, Vargha-Delaney Â12) with confidence intervals reported alongside p-values, per the ACM SIGSOFT Empirical Standards. Stars alone are not sufficient. |
| **All other venues** | Stars acceptable if defined in table notes. Follow venue-specific conventions in journal-profiles.md. |

Working paper default example:
```
Treatment        & 0.045**  & 0.038*   & 0.052*** \\
                 & (0.021)  & (0.020)  & (0.019)  \\
```

Effect-size reporting example (empirical-standards venues):
```
Treatment        & 0.045    & 0.038    & 0.052    \\
                 & (0.021)  & (0.020)  & (0.019)  \\
Cliff's $\delta$ & 0.31     & 0.27     & 0.35     \\
```

### Column and Row Structure

- **Column (1), (2), ...** headers in the first row after `\toprule`
- **Dependent variable** stated in a spanning header or the first subheader row
- **Variable names** left-aligned, human-readable (not raw dataframe column names)
  - `Log review latency` not `ln_review_lat`
  - `Uses CI` not `has_ci_2`
  - `Team size` not `n_devs`
- **Numeric columns** right-aligned or decimal-aligned
- **N**, **R²**, **Fixed effects** (Yes/No), **Controls** (Yes/No) at the bottom before `\bottomrule`

### Panel Structure

For tables with multiple panels:

```latex
\multicolumn{5}{l}{\textit{Panel A: Full sample}} \\
\midrule
...
\\[0.5em]
\multicolumn{5}{l}{\textit{Panel B: Male workers}} \\
\midrule
...
```

- Panel labels in italics, left-aligned, spanning all columns
- `\midrule` after each panel label
- Small vertical space (`\\[0.5em]`) between panels

### Preferred Packages

**Primary (Python): `pyfixest.etable` for regression tables**

```python
import pyfixest as pf

pf.etable(
    models,
    type="tex",                 # bare tabular output
    signif_code={"***": 0.01, "**": 0.05, "*": 0.10},
    coef_fmt="b \n (se)",
    labels={
        "treatment": "Treatment",
        "log_team_size": "Log team size",
    },
)
```

**Primary (Python): `pandas` for summary / descriptive tables**

```python
tex_output = (
    summary_df.style
    .format(precision=2)
    .to_latex(hrules=True, column_format="l" + "c" * (summary_df.shape[1]))
)
```

**Secondary (R): `modelsummary` / `fixest::etable`**

```r
library(modelsummary)

modelsummary(
  models,
  output   = "latex_tabular",  # bare tabular, no wrapper
  stars    = c("*" = 0.10, "**" = 0.05, "***" = 0.01),
  coef_rename = c("treatment" = "Treatment"),
  gof_map = c("nobs", "r.squared", "adj.r.squared"),
  escape  = FALSE
)
```

**Secondary (R): `kableExtra` for descriptive tables**

```r
library(kableExtra)

kbl(df, format = "latex", booktabs = TRUE, escape = FALSE,
    align = c("l", rep("c", ncol(df) - 1))) |>
  kable_styling(latex_options = "hold_position")
```

### Typography

- Serif font throughout (inherits from document class — no extra commands needed)
- `\small` or `\footnotesize` for tables that need to fit within column width
- Variable names in plain text, panel labels in `\textit{}`
- Never bold table body content; bold only for rare emphasis in headers

### Export

```python
# Write .tex fragment (no \begin{table} wrapper -- added in main.tex)
from pathlib import Path

Path("paper/tables/reg_main_specification.tex").write_text(tex_output, encoding="utf-8")
```

- Output **bare `tabular` environment** (no `\begin{table}` float)
- The paper's `main.tex` wraps it with `\begin{table}`, `\caption{}`, and `\input{}`
- Write to `paper/tables/`

### File Naming

```
tables/
├── descriptive/
│   ├── sumstats_main_sample.tex
│   └── balance_treatment_control.tex
├── estimation/
│   ├── reg_main_specification.tex
│   ├── reg_heterogeneity_gender.tex
│   └── did_event_study_coefficients.tex
└── robustness/
    └── reg_alternative_controls.tex
```

Pattern: `{table_type}_{content_description}.tex`

- `sumstats_` for summary statistics
- `balance_` for balance / pre-treatment tests
- `reg_` for regression output
- `did_` for difference-in-differences specific tables
- `first_stage_` for IV first stage
- `survey_` for survey-response tables
- `irr_` for inter-rater reliability tables

### Prohibited Patterns

| Pattern | Reason |
|---------|--------|
| Title row inside the table | Titles go in `\caption{}`, not the table body |
| Notes embedded in table body | Notes go below via `\begin{tablenotes}` |
| `\hline` | Use `\toprule` / `\midrule` / `\bottomrule` (booktabs) |
| Vertical rules (`\|` in column spec) | Never used in journal-quality tables |
| `stargazer` package (R) | Deprecated workflow; use `modelsummary` or `fixest::etable` |
| Raw variable names in labels | Human-readable labels required |
| `xtable` without booktabs (R) | Produces non-journal-quality output |
| `\begin{table}` in script output | Scripts export bare `tabular`; float wrapper lives in `main.tex` |

### Table Type Templates

Use these as defaults. Adapt columns based on the paper's needs (e.g., add Min/Max, percentiles, or subgroup columns when substantively important).

**Descriptive Statistics:**
```
\toprule
                        &  Mean   &  SD     \\
\midrule
\multicolumn{3}{l}{\textit{Continuous variables}} \\
\quad Commits per month &  84.3   &  61.2   \\
\quad Team size         &  6.4    &  3.1    \\
\quad Project age (years)& 4.8    &  2.9    \\
\\[0.5em]
\multicolumn{3}{l}{\textit{Categorical variables (\%)}} \\
\quad Uses CI           &  72.1   &         \\
\quad Has code review   &  58.4   &         \\
\bottomrule
```
- Default: Mean and SD in separate columns (never stacked with parentheses — that's for regression SEs)
- Categorical/binary: percentage in Mean column, SD blank
- Sample size stated once in table notes, not as a column
- Add Min/Max only when the range is substantively important (RDD bandwidth, data coverage)

**Regression Results:**
```
\toprule
                        &  (1)    &  (2)    &  (3)    &  (4)    \\
                        &  OLS    &  OLS    &  IV     &  IV     \\
\midrule
Treatment               &  0.045**&  0.038* &  0.052**&  0.041* \\
                        & (0.021) & (0.020) & (0.025) & (0.022) \\
\midrule
Controls                &  No     &  Yes    &  No     &  Yes    \\
Fixed Effects           &  No     &  Yes    &  No     &  Yes    \\
Observations            &  10,000 &  10,000 &  10,000 &  10,000 \\
R$^2$                   &  0.05   &  0.12   &         &         \\
\bottomrule
```
- Coefficients on one row, standard errors in parentheses below
- Stars: `*` p < 0.10, `**` p < 0.05, `***` p < 0.01
- Bottom rows: Controls (Yes/No), Fixed Effects (Yes/No), Observations, R²

**Multi-Outcome (Panel Structure):**
```
\toprule
                        &  (1)    &  (2)    &  (3)    &  (4)    \\
\midrule
\multicolumn{5}{l}{\textit{Panel A: Review latency}} \\
\midrule
Treatment               &  0.045**&  0.038* &  0.052**&  0.041* \\
                        & (0.021) & (0.020) & (0.025) & (0.022) \\
\\[0.5em]
\multicolumn{5}{l}{\textit{Panel B: Defect density}} \\
\midrule
Treatment               &  0.021  &  0.033* &  0.015  &  0.028  \\
                        & (0.018) & (0.017) & (0.020) & (0.019) \\
\midrule
Controls                &  No     &  Yes    &  No     &  Yes    \\
Fixed Effects           &  No     &  Yes    &  No     &  Yes    \\
Observations            &  10,000 &  10,000 &  10,000 &  10,000 \\
\bottomrule
```
- Each outcome gets its own panel with same column structure
- Panel labels in italics, left-aligned, spanning all columns
- Controls/FE/Observations rows appear once at the bottom (shared across panels)

**Balance Table:**
```
\toprule
Variable                &  Treatment &  Control &  Difference &  SE     &  p-value \\
\midrule
Commits per month       &  86.2      &  82.5    &  3.7        &  (2.9)  &  0.197   \\
Team size               &  6.5       &  6.3     &  0.2        &  (0.15) &  0.134   \\
Uses CI (\%)            &  71.8      &  72.6    &  -0.8       &  (1.2)  &  0.505   \\
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

## 2. Figure Standards

- **Never add titles or subtitles inside matplotlib/ggplot figures** — no `ax.set_title()` / `plt.suptitle()` for the overall figure; in ggplot use `labs(title = NULL, subtitle = NULL)`
- **Figure information goes in two places:**
  1. **File name** — descriptive, e.g., `fig1_review_latency_event_study.pdf`
  2. **LaTeX `\caption{}`** — the authoritative title, numbered and editable without re-running the script
- **Panel labels are the exception** — "Panel A: Review latency" inside multi-panel figures (subplot titles via `ax.set_title()`, or `patchwork`/`cowplot` in R) is fine since they identify sub-panels, not the whole figure
- **Axis labels must be publication-quality** — "Review Latency (hours)" not "review_lat". Clean labels stay in the figure; titles and context go in the caption
- **Use serif fonts** — figures should match the paper's body text. In matplotlib, set `plt.rcParams["font.family"] = "serif"`; in ggplot, `theme_minimal(base_family = "serif")`
- **Show all periods on the x-axis** when the panel spans ~20 periods or fewer — set explicit ticks (`ax.set_xticks(range(min_t, max_t + 1))`). Only thin out labels when they overlap (roughly >20 ticks)
- **Output PDF for figures** — vector graphics for LaTeX. Use `fig.savefig("fig.pdf", bbox_inches="tight")` (or `ggsave("fig.pdf")` in R). PNG only for raster content (heatmaps of large matrices, screenshots).
- **Colorblind-friendly palettes** — use `viridis`, matplotlib's `tab10`/`Set2`, or similar. Never rely on red/green contrast alone.
- **Color-independent design** — figures must be readable in grayscale. Combine color with marker shape and linestyle so series remain distinguishable without color.
- **Figure width** — single-panel: `width=0.8\textwidth`. Side-by-side panels: `width=0.48\textwidth` each.

---

## 3. PDF Processing

### The Safe Processing Workflow

**Step 1: Receive PDF Upload**
- User uploads PDF to `master_supporting_docs/supporting_papers/` or `supporting_slides/`
- Claude DOES NOT attempt to read it directly

**Step 2: Check PDF Properties**
```bash
pdfinfo paper_name.pdf | grep "Pages:"
ls -lh paper_name.pdf
```

**Step 3: Create Subfolder and Split**
```bash
mkdir -p paper_name/

for i in {0..9}; do
  start=$((i*5 + 1))
  end=$(((i+1)*5))
  gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER \
     -dFirstPage=$start -dLastPage=$end \
     -sOutputFile="paper_name/paper_name_p$(printf '%03d' $start)-$(printf '%03d' $end).pdf" \
     paper_name.pdf 2>/dev/null
done
```

**Step 4: Process Chunks Intelligently**
- Read chunks ONE AT A TIME using the Read tool
- Extract key information from each chunk
- Build understanding progressively
- Don't try to hold all chunks in working memory

**Step 5: Selective Deep Reading**
- After scanning all chunks, identify the most relevant sections
- Only read those sections in detail for slide development
- Skip appendices, references, or less relevant sections unless needed

### Error Handling Protocol

**If a chunk fails to process:**
1. Note the problematic chunk (e.g., "Chunk p021-025 failed")
2. Try splitting into 1-2 page pieces
3. If still failing, skip and document the gap

**If splitting fails:**
1. Check if Ghostscript is installed: `gs --version`
2. Try alternative: `pdftk paper.pdf burst output paper_%03d.pdf`
3. If all else fails, ask user to upload specific page ranges manually

**If memory/token issues persist:**
1. Process only 2-3 chunks per session
2. Focus on specific sections user identifies as most important

---

## 4. Exploration Folder Protocol

**All experimental work goes into `explorations/` first.** Never directly into production folders.

### Folder Structure

```
explorations/
├── ACTIVE_PROJECTS.md
├── [project]/
│   ├── README.md          # Goal, status, findings
│   ├── src/               # Code (use _v1, _v2 for iterations)
│   ├── scripts/           # Test scripts
│   ├── output/            # Results
│   └── SESSION_LOG.md     # Progress notes
└── ARCHIVE/
    ├── completed_[project]/
    └── abandoned_[project]/
```

### Lifecycle

1. **Create** — `mkdir -p explorations/[name]/{src,scripts,output}` + README from `templates/exploration-readme.md`
2. **Develop** — work entirely within the exploration folder
3. **Decide:**

   - **Graduate to production** — copy to `scripts/`; requires quality >= 80, tests pass, code clear. Move to `ARCHIVE/completed_[project]/`
   - **Keep exploring** — document next steps in README
   - **Abandon** — move to `ARCHIVE/abandoned_[project]/` with explanation (use `templates/archive-readme.md`)

### Graduate Checklist

- [ ] Quality score >= 80
- [ ] All tests pass
- [ ] Results replicate within tolerance
- [ ] Code is clear without deep context
- [ ] README explains approach and findings

---

## 5. Exploration Fast-Track

**Lightweight workflow for experimental work.** Quality threshold: 60/100 (vs 80 for production). No planning needed.

### Steps

1. **Research value check** — Does this improve the project? If NO, don't build it.
2. **Create folder** — `mkdir -p explorations/[name]/{src,scripts,output}` + README + SESSION_LOG.md
3. **Code immediately** — no plan needed. Must-haves: code runs, results correct, goal documented. Not needed: full docstrings, full tests, perfect style.
4. **Log progress** — append 2-3 lines to SESSION_LOG.md as you work
5. **Decision point** — keep exploring, graduate to production (upgrade to 80/100), or archive with brief explanation

### When to Stop (Kill Switch)

At any point: stop, archive with note ("Attempted X, hit blocker Y"), move on. No guilt — exploration is inherently uncertain.
