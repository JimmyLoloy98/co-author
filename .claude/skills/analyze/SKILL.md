---
name: analyze
description: End-to-end data analysis dispatching Coder and Data-engineer for implementation, coder-critic for review. Supports Python (primary) and R (secondary). Replaces /data-analysis.
argument-hint: "[dataset path or goal] Options: --dual [lang1,lang2]"
allowed-tools: Read,Grep,Glob,Write,Edit,Bash,Task
---

# Analyze

Run end-to-end data analysis by dispatching the **Coder** (analysis), **Data-engineer** (cleaning + figures), and **coder-critic** (code review).

**Input:** `$ARGUMENTS` — dataset path or description of analysis goal.

---

## Workflow

### Step 1: Pre-Code Report (mandatory)
Before writing any code, the Coder must output a structured report proving it read the strategy inputs:

```markdown
## Pre-Code Report
**Strategy memo:** [path or "not found"]
**Domain profile:** [loaded / not found]
**Language:** [Python / R — from CLAUDE.md]
**Study type:** [causal (DiD/IV/RDD) / experiment / survey / case study / repository mining / descriptive]

**Study design:** [one sentence from memo]
**Key variables:**
- Outcome: [paper name] → [code name]
- Treatment: [paper name] → [code name]
- Controls: [list]
- Fixed effects: [list]
- Clustering: [level — typically project]
**Data source:** [path or description]
**Estimator:** [from strategy memo]
**Robustness checks required:** [list from memo]
**Naming map confirms:** [yes / no — do planned code names match paper notation?]

Proceeding to implementation.
```

If the strategy memo is missing, the Coder proceeds with the user's description — but flags that no memo was found and strategic alignment checks (coder-critic categories 1-3) cannot be verified.

### Step 2: Data Preparation (if needed)
If raw data provided, dispatch **Data-engineer** first:
- Clean and wrangle raw data (repository extracts, survey responses, CI logs)
- Handle missing values, filter bots, merge developer identities, construct variables per strategy memo
- Generate summary statistics table
- Create publication-quality descriptive figures
- Save cleaned data, codebook, and figures

### Step 3: Main Analysis
Dispatch **Coder** agent:
- Stage 0: Data loading (from cleaned data or raw)
- Stage 1: Main specification (from strategy memo or user description)
- Stage 2: Robustness checks
- Stage 3: Publication-ready output (tables to `paper/tables/`, figures to `paper/figures/`)
- Produce `results_summary.md` with all estimates, SEs, effect sizes, and key statistics (MANDATORY)
- Save scripts to `scripts/python/` (or `scripts/R/` when R is the project language)

The Coder follows these principles:
- **Script structure:** Use the Script Structure Template below
- **Packages:** `pandas` + `numpy` for data, `pyfixest`/`statsmodels`/`linearmodels` for estimation, `matplotlib` for figures (R secondary: `fixest`, `modelsummary`, `ggplot2`)
- **Standard errors:** Cluster at appropriate level (match treatment assignment — typically project level when observations are commits/PRs within projects)
- **Output:** `.tex` tables for LaTeX, `.pdf`/`.png` figures, `.parquet` for intermediate data
- **No hardcoded paths.** All paths relative to repository root via `pathlib.Path` (or `here()` in R).
- **Write parquet/CSV intermediates.** Every computed dataset (cleaned frames, estimation samples, coefficient tables, summary statistics) gets written to `.parquet` (or `.csv` for small tables) for downstream use by the writer and other agents. Model objects that don't serialize to parquet go to pickle (Python) or `.rds` (R).

### Step 4: Code Review
Dispatch **coder-critic** agent — run the full 12-category checklist:

**Strategic (categories 1-3):**
1. **Code-strategy alignment** — Does the code implement the strategy memo faithfully? Correct dependent variable, treatment, controls, fixed effects, sample restrictions?
2. **Sanity checks** — Are summary statistics printed before regressions? Do coefficient signs match substantive intuition (e.g., larger teams → longer review latency)? Are sample sizes reasonable?
3. **Robustness sufficiency** — Are required robustness checks present? Alternative specifications, placebo tests, sensitivity analysis per strategy memo?

**Code Quality (categories 4-12):**
4. **Structure** — Does the script follow the standard template? Clear section headers, logical flow from setup to export?
5. **Console hygiene** — No spurious `print()` statements polluting output. Intentional output only.
6. **Reproducibility** — Seed set once at top (`np.random.default_rng(SEED)` / `set.seed()`) if any stochastic elements. No absolute paths. All packages imported at top. Directory creation with `mkdir(parents=True, exist_ok=True)`.
7. **Functions** — Repeated logic extracted into functions. No copy-paste code blocks with minor variations.
8. **Figure quality** — Publication-ready: proper axis labels, legends, font sizes. Consistent style across all figures.
9. **Intermediates pattern** — Every computed dataset saved to `.parquet`/`.csv` (models to pickle/`.rds`) for downstream use. Not just final outputs — intermediate objects too.
10. **Comments** — Section headers present. Non-obvious code commented. No commented-out dead code left behind.
11. **Error handling** — Graceful handling of missing files, empty data subsets, API rate limits, convergence failures. Informative error messages.
12. **Polish** — Consistent naming conventions. No magic numbers. Clean whitespace. Professional quality ready for the artifact-evaluation package.

If strategy memo exists, cross-reference code against stated design.
Save report to `quality_reports/[script]_code_review.md`.

Generate HTML version and refresh dashboard:
```bash
python3 scripts/generate_html_report.py code-audit quality_reports/[script]_code_review.md
python3 scripts/generate_dashboard.py
```

### Step 5: Fix Issues
If coder-critic finds Critical or Major issues:
1. Re-dispatch Coder with specific fixes (max 3 rounds)
2. Re-run coder-critic to verify fixes

### Step 6: Present Results
1. **Results summary** — key estimates with SEs, effect sizes, and interpretation (from `results_summary.md`)
2. **Scripts created** — paths and descriptions
3. **Output files** — tables in `paper/tables/`, figures in `paper/figures/`
4. **Code review score** — from coder-critic
5. **TODO items** — missing data, additional specifications needed

---

## Script Structure Template

```python
# ============================================================
# [Descriptive Title]
# Author: [from project context]
# Purpose: [What this script does]
# Inputs: [Data files]
# Outputs: [Figures, tables, parquet files]
# ============================================================

# 0. Setup ----
from pathlib import Path

import numpy as np
import pandas as pd
import pyfixest as pf
import matplotlib.pyplot as plt

SEED = 42
rng = np.random.default_rng(SEED)

ROOT = Path(__file__).resolve().parents[2]
(ROOT / "paper" / "tables").mkdir(parents=True, exist_ok=True)
(ROOT / "paper" / "figures").mkdir(parents=True, exist_ok=True)

# 1. Data Loading ----

# 2. Exploratory Analysis ----

# 3. Main Analysis ----

# 4. Tables and Figures ----

# 5. Export ----
# coef_table.to_parquet(ROOT / "scripts" / "python" / "output" / "main_results.parquet")
# analysis_df.to_parquet(ROOT / "scripts" / "python" / "output" / "analysis_sample.parquet")
```

---

## Results Summary (Mandatory Artifact)

Every analysis run MUST produce `results_summary.md` containing:
- All point estimates with standard errors and significance levels
- Effect sizes (Cliff's delta, Vargha-Delaney Â12) with CIs where the venue expects them
- Sample sizes for each specification
- Key summary statistics (means, medians, standard deviations of main variables)
- Robustness check results (brief table or comparison)
- Any flags or anomalies discovered during analysis

This file is the primary handoff artifact to the writer agent. Without it, the writer cannot draft the results section.

---

## Dual-Language Mode (`--dual python,r`)

When `--dual [lang1,lang2]` is provided (e.g., `--dual python,r`):

1. **Data-engineer** runs once — language-agnostic cleaning, saves to `data/cleaned/`
2. **Two Coder agents** dispatched in parallel — same strategy memo, different languages
3. **coder-critic** reviews each implementation independently (max 3 rounds each)
4. **Comparison step** — verify numerical alignment per `.claude/references/domain-profile.md` tolerances:
   - Point estimates must match within declared tolerance
   - Standard errors must match within declared tolerance
   - Flag any divergences with exact values from both languages
5. Save comparison report to `quality_reports/cross_language_comparison.md`

### Replication Tolerance Approach

Inspired by Scott Cunningham's replication methodology: **if two independent implementations agree, neither has a bug.** This is the core rationale for dual-language mode.

**Tolerance thresholds:**
- **Floating-point differences are normal.** Minor numerical differences (e.g., 1e-10) between Python and R arise from different linear algebra backends, optimizer defaults, and floating-point arithmetic. These are expected, not bugs.
- **Point estimates:** Must agree within 1e-6 (relative) or as declared in `domain-profile.md`
- **Standard errors:** Must agree within 1e-4 (relative) — SE computation varies more across implementations due to degrees-of-freedom corrections and clustering algorithms
- **P-values:** Must agree on significance at conventional levels (0.01, 0.05, 0.10). If one language says p=0.049 and the other says p=0.051, flag for manual review but do not treat as a bug.
- **Sample sizes:** Must match exactly. Any discrepancy indicates a data handling difference that must be resolved.

**When results diverge beyond tolerance:**
1. Both Coder agents are re-dispatched to investigate
2. Check: different default options (e.g., dropna handling, convergence criteria)
3. Check: different variable coding or categorical/factor ordering
4. The comparison report includes a side-by-side table of all estimates
5. If divergence persists after investigation, escalate to user with exact values from both languages

---

## Bundled Resources

### Templates
| File | Purpose |
|------|---------|
| `analyze/templates/pre-code-report.md` | Mandatory pre-check report format before writing code |
| `analyze/templates/paper-to-code-map.md` | Naming map protocol: paper notation to code variables |
| `analyze/templates/python-script-structure.py` | Boilerplate Python script scaffold following INV-14..19 |
| `analyze/templates/r-script-structure.R` | Boilerplate R script scaffold (secondary language) |
| `analyze/templates/results-summary.md` | Mandatory results summary output for writer handoff |

### References
| File | Purpose |
|------|---------|
| `analyze/references/table-standards.md` | Full table formatting standards: booktabs, coefficient display, panel structure, Python/R packages, file naming |
| `analyze/references/figure-standards.md` | Full figure formatting standards: fonts, colors, axis labels, export settings, common plot types |

### Config
| File | Purpose |
|------|---------|
| `analyze/config/replication-tolerances.json` | Tolerances for dual-language replication comparison (point estimates, SEs, p-values) |

### Gotchas
| File | Purpose |
|------|---------|
| `analyze/gotchas.md` | Known failure points: package quirks, numerical issues, output traps, cross-language divergence sources |

---

## Principles
- **Reproduce, don't guess.** If the user specifies a regression, run exactly that.
- **Show your work.** Print summary statistics before jumping to regressions.
- **Strategy alignment.** If strategy memo exists, code MUST implement it faithfully.
- **Worker-critic pairing.** Coder creates, coder-critic critiques. Never skip review.
- **Persist intermediates.** Every computed dataset gets written to `.parquet`/`.csv` (models to pickle/`.rds`) for downstream use -- estimation samples, coefficient tables, summary statistics, not just final tables.
- **Publication-ready output.** Tables and figures directly includable in the paper.
- **Cross-language convergence.** When `--dual` is used, divergence is a bug until proven otherwise.
