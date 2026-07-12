# Figure Standards

Publication-quality figures for empirical software engineering papers. All figures must be directly includable in the LaTeX manuscript without manual editing.

---

## Core Rules

- **Never add titles or subtitles inside the figure** -- no `plt.title()` / `ax.set_title()` on single-panel figures, no ggplot `title`/`subtitle`
- **Figure information goes in two places:**
  1. **File name** -- descriptive, e.g., `fig_event_study_ci_adoption.pdf`
  2. **LaTeX `\caption{}`** -- the authoritative title, numbered and editable without re-running the script
- **Panel labels are the exception** -- "Panel A: Review latency" inside multi-panel figures (matplotlib subplots, `patchwork`/`cowplot` in R) is fine since they identify sub-panels, not the whole figure
- **Axis labels must be publication-quality** -- "Review Latency (hours)" not "review_latency_hrs". Clean labels stay in the figure; titles and context go in the caption
- **Use serif fonts** -- figures should match the paper's body text
- **Output PDF for figures** -- vector graphics for LaTeX. PNG only for raster content (heatmaps of huge matrices, screenshots)

---

## Font and Style

**Primary (Python, matplotlib):**
```python
import matplotlib.pyplot as plt

plt.rcParams.update({
    "font.family": "serif",
    "font.size": 11,
    "axes.grid": True,
    "grid.alpha": 0.3,
    "axes.spines.top": False,
    "axes.spines.right": False,
})
```

**Secondary (R, ggplot2):**
```r
theme_paper <- theme_minimal(base_family = "serif", base_size = 11) +
  theme(
    panel.grid.minor = element_blank(),
    legend.position  = "bottom",
    plot.title       = element_blank(),  # No titles -- INV-12
    plot.subtitle    = element_blank()
  )

theme_set(theme_paper)
```

---

## Axis Labels

- **Show all periods on the x-axis** when the panel spans ~20 periods or fewer:
  ```python
  ax.set_xticks(range(min_period, max_period + 1))
  ```
  Only thin out labels when they overlap (roughly >20 ticks).

- **Human-readable labels:**
  - "Log Review Latency (hours)" not "ln_review_latency"
  - "Share of PRs Merged" not "pct_merged"
  - Include units where applicable

---

## Color

- **Colorblind-friendly palettes** -- matplotlib's `tab10`/`viridis`, Okabe-Ito values, or ColorBrewer `Set2`
- **Never rely on red/green contrast alone**
- **Color-independent design** -- figures must be readable in grayscale:
  - Combine color with marker shape
  - Combine color with linestyle
  - Series remain distinguishable without color

Recommended palettes:
```python
# Option 1: Okabe-Ito (colorblind-safe)
colors = ["#0072B2", "#D55E00", "#009E73", "#CC79A7"]

# Option 2: Viridis (perceptually uniform)
plt.cm.viridis(np.linspace(0, 0.9, n_series))
```

```r
# R secondary
scale_color_brewer(palette = "Set2")
scale_color_viridis_d()
```

---

## Figure Width

- **Single-panel:** `width=0.8\textwidth` in LaTeX, `figsize=(6, 4)` in matplotlib
- **Side-by-side panels:** `width=0.48\textwidth` each in LaTeX
- **Full-width landscape:** use `\begin{landscape}` environment

In Python:
```python
fig, ax = plt.subplots(figsize=(6, 4))
# ... plot ...
fig.savefig(ROOT / "paper" / "figures" / "fig_event_study.pdf",
            bbox_inches="tight")
```

---

## Common Figure Types

### Event Study Plot (e.g., CI adoption)
```python
fig, ax = plt.subplots(figsize=(6, 4))
ax.errorbar(es["relative_time"], es["estimate"],
            yerr=1.96 * es["se"], fmt="o", capsize=3, color="#0072B2")
ax.axhline(0, linestyle="--", color="gray")
ax.axvline(-0.5, linestyle=":", color="gray")
ax.set_xlabel("Quarters Relative to CI Adoption")
ax.set_ylabel("Estimated Effect on Build Success Rate")
fig.savefig(FIGURE_DIR / "fig_event_study_ci_adoption.pdf", bbox_inches="tight")
```

### Coefficient Plot
```python
fig, ax = plt.subplots(figsize=(6, 4))
ax.errorbar(coefs["estimate"], range(len(coefs)),
            xerr=1.96 * coefs["se"], fmt="o", capsize=3, color="#0072B2")
ax.axvline(0, linestyle="--", color="gray")
ax.set_yticks(range(len(coefs)))
ax.set_yticklabels(coefs["label"])  # human-readable: "Uses CI", "Team size"
ax.set_xlabel("Estimated Effect on Defect Density")
fig.savefig(FIGURE_DIR / "fig_coefplot_main.pdf", bbox_inches="tight")
```

### RDD Plot (Python `rdrobust`)
```python
from rdrobust import rdplot

rdplot(y=df["review_latency"], x=df["running_var"], c=cutoff,
       x_label="Running Variable (e.g., PR size threshold)",
       y_label="Review Latency (hours)", title="")
```

### R secondary equivalents
```r
# Event study
ggplot(es_data, aes(x = relative_time, y = estimate)) +
  geom_point(size = 2) +
  geom_errorbar(aes(ymin = ci_lower, ymax = ci_upper), width = 0.2) +
  geom_hline(yintercept = 0, linetype = "dashed", color = "gray50") +
  geom_vline(xintercept = -0.5, linetype = "dotted", color = "gray50") +
  labs(x = "Quarters Relative to CI Adoption", y = "Estimated Effect") +
  theme_paper

# Coefficient plot
modelsummary::modelplot(models, coef_omit = "Intercept") +
  geom_vline(xintercept = 0, linetype = "dashed") +
  theme_paper

# RDD
rdrobust::rdplot(y = df$outcome, x = df$running_var, c = cutoff,
                 x.label = "Running Variable", y.label = "Outcome")
```

---

## Export

```python
# PDF for LaTeX inclusion (vector graphics) -- primary
fig.savefig(ROOT / "paper" / "figures" / "fig_main.pdf", bbox_inches="tight")

# PNG only for raster content
fig.savefig(ROOT / "paper" / "figures" / "heatmap_commit_activity.png",
            dpi=300, bbox_inches="tight")
```

```r
# R secondary
ggsave(here("paper", "figures", "fig_main.pdf"),
       plot = p, width = 6, height = 4, device = cairo_pdf)
```

---

## File Naming

```
figures/
  descriptive/
    fig_histogram_review_latency.pdf
    fig_time_series_ci_adoption.pdf
  estimation/
    fig_event_study_main.pdf
    fig_coefplot_heterogeneity.pdf
    fig_rdd_main.pdf
  robustness/
    fig_placebo_test.pdf
    fig_sensitivity_bandwidth.pdf
```

Pattern: `fig_{description}.pdf`

---

## LaTeX Inclusion

```latex
\begin{figure}[htbp]
\centering
\includegraphics[width=0.8\textwidth]{figures/estimation/fig_event_study_main.pdf}
\caption{Event study estimates of the effect of CI adoption on build success rate.
The figure plots point estimates and 95\% confidence intervals for each quarter
relative to CI adoption, with standard errors clustered at the project level. The
dashed vertical line marks adoption. Pre-adoption coefficients are not statistically
different from zero, consistent with parallel trends. Source: GitHub repository
panel (SEART GHS sample).}
\label{fig:event_study}
\end{figure}
```

Key elements of figure captions (INV-2):
- What is shown
- How to read it
- Data source

---

## Prohibited Patterns

| Pattern | Reason |
|---------|--------|
| `plt.title()` / `ax.set_title()` on the whole figure | Titles go in LaTeX `\caption{}` (INV-12) |
| `ggtitle()` or `labs(title = "...")` | Same reason |
| Default ggplot theme (gray background) | Use `theme_minimal` or custom theme |
| Red/green only color schemes | Not colorblind-friendly |
| JPG format | Lossy compression; use PDF for vector, PNG for raster |
| Axis labels with underscores | Human-readable labels required |
| Legend inside plot area (overlapping data) | Place legend below the plot |
