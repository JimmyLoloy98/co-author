# Analyze Skill -- Gotchas

Known failure points and edge cases for data analysis.

## Python Packages

- `pyfixest.etable()` may emit a full table environment depending on `type=`. Export bare tabular only (INV-13) -- strip or avoid float wrappers before writing to `paper/tables/`.
- `pyfixest` and R's `fixest` differ in small-sample corrections for cluster-robust SEs. Check the `ssc()` / `ssc=` options when comparing across languages.
- `pandas.read_stata()` decodes value labels to strings by default (`convert_categoricals=True`) -- R's `haven::read_dta()` keeps them as labelled numerics until `as_factor()`. Decide once and document; this is a classic cross-language divergence source.
- pandas categoricals preserve insertion order unless `ordered=True` with explicit categories; R factors default to alphabetical. This silently changes dummy encoding and reference levels.
- `statsmodels` formula API drops NA rows silently (`missing='drop'`). Log the estimation sample size explicitly after every fit.
- `df.groupby()` drops NA group keys by default (`dropna=True`) -- another silent sample-size trap.
- `rdrobust` (Python port) mirrors the R API but returns pandas objects -- access point estimates via the results attributes, not positional indexing.

## R Packages (secondary)

- `fixest::etable()` with `tex=TRUE` includes `\begin{table}` wrappers by default. Use `style.tex = style.tex(tpt=TRUE)` or export manually for bare tabular output (INV-13).
- `modelsummary` silently drops coefficients when `coef_rename` keys don't match variable names exactly. Always check output dimensions against model object.
- `modelsummary` with `output = "latex"` adds table float wrappers. Use `output = "latex_tabular"` for bare tabular.
- Clustering syntax differs between `fixest` and `lm` -- always use `fixest::feols()` for cluster-robust standard errors.
- R's `haven::read_dta()` preserves Stata value labels as attributes -- use `as_factor()` explicitly or they'll be invisible numeric codes.
- `did::att_gt()` requires the group variable to be the year of first treatment (0 for never-treated). Miscoding this produces silent wrong results.
- `rdrobust` (R) returns an object where `$coef` is a matrix, not a vector. Access the point estimate with `$coef[1]` or `$Estimate[1]`.

## Data Handling

- Pre-Code Report blocks on strategic alignment if strategy memo doesn't exist. Run `/strategize` first, or proceed with user description and flag the gap.
- Write parquet/CSV intermediates for ALL computed datasets, not just final outputs (models to pickle/`.rds`). Intermediates enable debugging and writer handoff.
- `Path(__file__).resolve().parents[k]` -- count the depth to project root carefully; `scripts/python/01_setup.py` needs `parents[2]`. In R, `here()` resolves from a `.here` or `.Rproj` file at project root.
- Never use `os.chdir()` or `setwd()` -- they break reproducibility across machines (INV-19).
- When merging datasets, always check merge rates. A 60% merge rate is a red flag. Document non-merges.
- Repository data needs bot filtering (dependabot, renovate, CI bots) and developer identity merging (multiple emails per author) BEFORE computing any per-developer metric. Unfiltered bots inflate commit counts dramatically.
- GitHub API pagination and rate limits truncate silently if unhandled -- verify pulled counts against expected totals.

## Numerical Issues

- Never compare floats with `==`. Use `np.isclose()` / `math.isclose()` (R: `all.equal()`) or an explicit tolerance: `abs(a - b) < 1e-10`.
- `scipy.stats.norm.ppf(0)` returns `-inf` and `ppf(1)` returns `inf`. Guard inverse link functions with `np.clip(p, eps, 1 - eps)`.
- SE metrics (LOC, churn, resolution time) are heavily right-skewed -- log-transform or use rank-based methods; means alone mislead.
- R only: `1:n` when `n == 0` produces `c(1, 0)`. Use `seq_len(n)`. `sapply()` returns a list or matrix depending on lengths -- use `vapply()` for type safety.

## Output

- Figures must not have titles inside matplotlib/ggplot (INV-12). Titles go in LaTeX `\caption{}`.
- Tables must be bare `tabular` -- no `\begin{table}` wrapper (INV-13).
- PDF is the required format for figures (vector graphics for LaTeX). PNG only for raster content.
- Empirical-standards venues (EMSE, ESEM, MSR) require effect sizes with CIs alongside p-values (INV-4) -- compute Cliff's delta / Vargha-Delaney Â12 at analysis time, not at writing time.
- `results_summary.md` is mandatory. Without it, the writer agent cannot draft the results section.

## Cross-Language (--dual mode)

- Python and R handle NA differently in groupby/aggregate operations. Explicit `dropna()` decisions in pandas; `na.rm = TRUE` in R.
- pandas categoricals preserve insertion order; R factor ordering defaults to alphabetical. This affects dummy variable encoding and reference levels.
- `pyfixest` and `fixest` may differ slightly in cluster-robust SEs due to small-sample corrections.
- Sample sizes must match exactly across languages. Any difference is a data handling bug.
