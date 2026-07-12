# Causal Audit: 4-Phase Sequential Protocol

Extracted from `strategist-critic.md`. Used by the strategist-critic agent for strategy review.

---

## Key Principle

Verify the core design holds BEFORE checking robustness details. A paper with violated parallel trends doesn't need Oster bounds feedback. A mining study whose key metric lacks construct validity doesn't need effect-size sensitivity analysis.

**Phases execute in order, with early stopping when critical issues are found.**

---

## Phase 1: What's the Claim?

_Always runs. This is triage._

**First:** Identify the paper type:
- **Causal (mining)** -- causal inference on repository/telemetry data via exogenous variation
- **Repository mining (observational)** -- large-N correlational analysis of software artifacts
- **Theory + empirics** -- a theory of developer/team behavior tested with data
- **Descriptive / measurement** -- new data, facts, or metric definitions

**Then** identify the specifics:

### Causal (mining):
1. **Causal design(s) used:** DiD (classic or staggered), IV, RDD, Synthetic Control, Event Study, or combinations
2. **Estimand:** ATT, ATE, LATE -- what parameter is being estimated?
3. **Treatment:** What is the treatment (e.g., CI adoption, code-review policy)? Who receives it? When?
4. **Control:** What is the comparison group of projects/developers?
5. **Outcome(s):** What SE outcomes are studied (review latency, defect density, delivery performance)?

### Repository mining (observational):
1. **Sample:** Selection criteria, size, sampling frame (SEART GHS-style?)
2. **Metrics:** What is being measured and how is it operationalized?
3. **Model:** Regression, correlation, classification -- what relates predictors to outcomes?
4. **Confounds:** Which project/developer confounders are controlled?

### Theory + empirics:
1. **Theory type:** Team-behavior theory, motivation theory, socio-technical model, cognitive model
2. **Testable predictions:** What does the theory predict?
3. **Empirical tests:** How are predictions tested?

### Descriptive / measurement:
1. **What is being measured?** Concept and operationalization
2. **What's new?** New data, new metric, or new decomposition
3. **What beliefs does this revise?**

If the paper uses multiple designs (e.g., DiD + Event Study), list them in order of prominence. The PRIMARY design is reviewed first in Phase 2.

**Early stop:** If a descriptive paper makes no causal claims, skip the causal design checks in Phase 2. Route to the descriptive/measurement checklist instead.

---

## Phase 2: Does the Core Design Hold?

_Runs for the PRIMARY design first. If multiple designs, review them sequentially -- not interleaved._

### Step 2A: Design-Specific Assumption Check

For the identified design, check ONLY the critical assumptions (the 3-5 things that make or break the design):

#### Difference-in-Differences (Classic)
- [ ] Parallel trends assumption **explicitly stated**
- [ ] Pre-trend evidence shown (event study plot, formal test, or argued)
- [ ] No-anticipation assumption discussed
- [ ] Treatment timing clearly defined
- [ ] SUTVA / no-spillover addressed if relevant

#### Difference-in-Differences (Staggered Adoption)
- [ ] Heterogeneous treatment effects acknowledged as TWFE concern
- [ ] "Forbidden comparisons" (already-treated as controls) avoided or discussed
- [ ] Appropriate estimator chosen:
  - Callaway-Sant'Anna (2021): group-time ATT(g,t) with proper aggregation
  - Sun-Abraham (2021): interaction-weighted estimator
  - Borusyak-Jaravel-Spiess (2024): imputation estimator
  - de Chaisemartin-D'Haultfoeuille: heterogeneity-robust
- [ ] Aggregation scheme explicit (simple, group-size weighted, calendar-time, event-time)
- [ ] Never-treated vs. not-yet-treated control group choice justified
- [ ] Negative weights checked/discussed if using TWFE

#### Instrumental Variables
- [ ] First-stage F-statistic reported (Montiel Olea-Pflueger effective F preferred)
- [ ] Exclusion restriction **argued**, not just stated -- WHY is it plausible?
- [ ] Independence/relevance assumptions explicitly stated
- [ ] LATE vs. ATE distinction made -- who are the compliers?
- [ ] For weak instruments: Anderson-Rubin confidence sets or tF procedure
- [ ] Monotonicity discussed if heterogeneous effects
- [ ] Overidentification test if multiple instruments (Hansen J)

#### Regression Discontinuity Design
- [ ] Continuity assumption stated
- [ ] McCrary density test (`rddensity`) run and reported
- [ ] Bandwidth selection method documented (MSE-optimal via `rdrobust`, or CER-optimal)
- [ ] Covariate balance at cutoff shown
- [ ] Donut-hole robustness (exclude observations near cutoff)
- [ ] Alternative bandwidth robustness (half, double)
- [ ] Fuzzy vs. sharp distinction clear
- [ ] Local linear preferred; higher polynomial orders justified

#### Synthetic Control
- [ ] Pre-treatment fit quality shown (RMSPE or visual)
- [ ] Predictor balance table (treated vs. synthetic)
- [ ] Donor pool composition justified (why these units?)
- [ ] Inference via permutation (placebo-in-space): RMSPE ratios for all donor units
- [ ] No extrapolation (synthetic weights between 0 and 1, sum to 1)
- [ ] Sensitivity to donor pool composition tested
- [ ] Post-treatment gap interpretation

#### Event Studies
- [ ] Leads and lags specification clear
- [ ] Normalization period explicit (typically $t = -1$)
- [ ] Pre-event coefficients near zero (parallel trends evidence)
- [ ] Binning of distant endpoints documented
- [ ] Confidence intervals plotted (not just point estimates)
- [ ] For staggered settings: heterogeneity-robust event study used

### Step 2A (Repository Mining / Observational): Data Curation and Confound Check

_Use this checklist when the paper type is a repository-mining or other observational study._

#### Sample Construction
- [ ] **Repository selection criteria stated and justified** -- stars are NOT a proxy for engineered software (perils-of-mining literature). SEART GHS-style criteria or equivalent.
- [ ] **Bots filtered** -- automated accounts (dependabot, CI bots) removed or explicitly retained with justification.
- [ ] **Developer identities merged** -- aliases across commits/emails reconciled before any per-developer metric.
- [ ] **Survivorship bias addressed** -- sampling only active/popular projects biases estimates; discussed or corrected.
- [ ] **Time window justified** -- release-cycle effects, platform changes, and project-age confounds accounted for.

#### Metric Construct Validity
- [ ] **Each metric measures the claimed construct.** "Commits/LOC/churn measures [productivity/quality] because [argument]" -- not asserted, argued.
- [ ] **Operationalization documented** -- exact query, thresholds, and edge-case handling reproducible.
- [ ] **Skew handled** -- LOC, churn, and resolution-time distributions are heavy-tailed; log/rank transforms or non-parametric methods used.

#### Confounding and Causal Claims
- [ ] **Confounders identified and controlled** -- project size, age, domain, team size, language. Or an explicit causal design (DiD/IV/RDD around a platform or policy change).
- [ ] **Causal language matched to design.** Observational correlation must NOT be reported with causal verbs unless a causal design supports it (INV-8).
- [ ] **Clustering at the project level** when observations are commits/PRs/issues nested within projects.

#### Reproducibility
- [ ] **Mining scripts and dataset shared** -- or a clear reason they cannot be (private telemetry, ethics).
- [ ] **Snapshot/version pinned** -- API pulls are time-dependent; the extraction date and tool versions are recorded.

### Step 2A (Theory + Empirics): Prediction and Test Check

_Use this checklist when the paper type is Theory + Empirics._

#### Model Assessment
- [ ] **Predictions are sharp** -- they rule out some empirical patterns. "X increases Y" alone is too weak if the alternative also predicts this.
- [ ] **At least one distinguishing prediction** that competing models do NOT generate
- [ ] **Predictions numbered and clearly stated** before any empirical evidence
- [ ] **Model assumptions justified** -- why these preferences, this information structure, this timing?

#### Mapping Predictions to Tests
- [ ] **Each prediction has a clearly specified test** -- not just "we check whether the data is consistent"
- [ ] **Test has power to reject the prediction** -- would you see a different result if the model were wrong?
- [ ] **Controls for alternative explanations** -- other theories that generate the same prediction
- [ ] **Direction of test stated ex ante** -- what would confirmation look like? What would rejection look like?

#### Honesty Assessment
- [ ] **Can any result be rationalized by the model?** If yes, the test is uninformative -- flag it.
- [ ] **Multiple equilibria handled?** Which equilibrium does the empirical setting select?
- [ ] **Where the model fails acknowledged?** If all predictions confirmed, is the paper being honest or just not testing sharp predictions?
- [ ] **Post-hoc rationalization risk:** Were predictions derived before or after seeing the data?

### Step 2A (Descriptive / Measurement): Measurement Validity Check

_Use this checklist when the paper type is Descriptive / Measurement._

#### Construct Validity
- [ ] **Concept clearly defined** -- what exactly is being measured?
- [ ] **Measure maps to concept** -- is the operationalization faithful, or is there a gap between concept and measure?
- [ ] **Measurement error discussed** -- noise, systematic bias, attenuation
- [ ] **Alternative operationalizations considered** -- why this construction over alternatives?

#### Construction and Replicability
- [ ] **Data sources documented** -- complete enough to replicate
- [ ] **Construction steps explicit** -- thresholds, imputations, weights, linking methodology
- [ ] **Key decisions justified** -- each subjective choice in construction has a reason
- [ ] **Sensitivity to construction choices** -- how do results change with alternative decisions?

#### Validation
- [ ] **Internal validation:** consistency checks, monotonicity, face validity
- [ ] **External validation:** correlation with established measures
- [ ] **Benchmark comparison:** on known cases, does the measure get the right answer?
- [ ] **Discriminant validity:** the measure captures what it claims, not something correlated

#### Causal Language Check
- [ ] **No causal claims without a design.** Descriptive papers use "associated with," "predicts," "correlates with" -- not "causes" or "leads to"
- [ ] **If the paper does make causal claims:** it needs a design, and the causal-design checklists above apply to that component

### Step 2B: Sanity Check (MANDATORY)

**Before proceeding to Phase 3, verify that results actually make sense.** This is the most important step -- it catches nonsensical results that pass all the checklist items above.

**Causal (mining) designs:**
- [ ] **Sign:** Does the direction of the effect make engineering sense? If adopting code review *increases* defect density, that needs explanation.
- [ ] **Magnitude:** Is the effect size plausible? A CI-adoption effect that cuts review latency by 95% is implausible. Use back-of-envelope reasoning.
- [ ] **Dynamics (event studies):** Do pre-treatment coefficients look like noise around zero, or is there a clear pre-trend? Do post-treatment coefficients tell a coherent story (e.g., gradual phase-in, immediate jump, fade-out)?
  - **Flag:** Pre-event coefficients trending toward the post-treatment effect -> parallel trends likely violated
  - **Flag:** Post-treatment coefficients that bounce wildly with no pattern -> specification may be wrong
  - **Flag:** Event study that "looks good" only because confidence intervals are enormous
- [ ] **Consistency:** Do results across specifications tell a consistent story, or does the main result only survive one particular specification?

**Experiments and mining (descriptive):**
- [ ] **Effect sizes reported and plausible?** Cliff's delta / Â12 with CIs -- do they fall in ranges consistent with the literature and with practical significance?
- [ ] **Distributions inspected:** Heavy-tailed SE metrics can make means misleading; were medians/quantiles and non-parametric tests used where appropriate?
- [ ] **Practical significance addressed?** A statistically significant 0.3-hour change in review latency may be practically negligible -- is this discussed?
- [ ] **Sensitivity:** Do results change dramatically with alternative metric definitions or repository samples? If yes, the finding depends on operationalization more than the phenomenon.

**Theory + empirics:**
- [ ] **Test results coherent?** Do the empirical findings tell a consistent story across predictions?
- [ ] **Confirmation bias check:** Are all predictions confirmed? If yes, are the tests sharp enough to reject?
- [ ] **Magnitude of predicted effects vs. observed:** Does the model predict effects of the right order of magnitude?

**Descriptive / measurement:**
- [ ] **Facts surprising or important?** If the facts confirm what everyone already knew, what's the contribution?
- [ ] **Magnitudes meaningful?** Are the documented patterns large enough to matter for theory or policy?
- [ ] **Patterns robust to measurement choices?** Do the key facts survive alternative constructions?

**Early stop logic:** If Phase 2 finds CRITICAL issues (e.g., clear parallel trends violation, nonsensical effect sizes, first-stage F < 5, key metric lacking construct validity, unfiltered bots corrupting the sample, all-confirming weak tests), the report should **focus on these**. Still run Phases 3-4 but explicitly note: "These issues should be resolved before the following feedback becomes relevant."

---

## Phase 3: Is the Inference Sound?

_Runs after Phase 2. If Phase 2 found critical issues, still review but flag that design issues take priority._

### Observational-Mining Inference (when paper type is Repository Mining)
- [ ] **Standard errors match the nesting:** Cluster-robust at the project level when commits/PRs/issues are nested within projects
- [ ] **Effect sizes reported:** Cliff's delta / Vargha-Delaney Â12 with CIs, not p-values alone
- [ ] **Multiple comparisons corrected:** Benjamini-Hochberg or Bonferroni when many metrics/RQs are tested
- [ ] **Non-parametric tests where warranted:** Mann-Whitney/Wilcoxon for heavy-tailed SE metrics
- [ ] **Sensitivity to metric definition:** Do conclusions survive alternative operationalizations?

### Theory + Empirics Inference (when paper type is Theory + Empirics)
- [ ] **Each test has appropriate inference** -- clustering, standard errors match the data structure
- [ ] **Joint test of multiple predictions:** If testing several predictions, are they tested jointly or only marginally?
- [ ] **Power assessment:** Could the data detect the predicted effect size? If power is low, a null result is uninformative.

### Causal-Design Standard Errors & Clustering
- [ ] Clustering level justified (matches treatment assignment unit)
- [ ] For DiD: cluster at treatment-group level, not individual
- [ ] When few clusters ($\leq 50$): wild cluster bootstrap (`boottest`, `fwildclusterboot`)
- [ ] When very few clusters ($\leq 10$): randomization inference or effective df adjustment
- [ ] Conley spatial SEs if geographic spillovers possible
- [ ] Heteroskedasticity-robust SEs: HC1 vs HC2/HC3 (small-sample correction)

### Multiple Testing
- [ ] Bonferroni/Benjamini-Hochberg/Romano-Wolf when testing multiple outcomes
- [ ] Stars match stated significance levels

### Code-Theory Alignment (when R scripts exist)
- [ ] Estimand in code matches paper claim (ATT vs ATE vs LATE)
- [ ] Standard errors in code match stated method (cluster level, HC type)
- [ ] Sample restrictions in code match paper description

#### Package-Specific Checks

**`fixest`:**
- [ ] `feols()` clustering via `cluster = ~unit` (not deprecated `se = "cluster"`)
- [ ] Fixed effects specification matches paper equation
- [ ] `i()` used correctly for event study interactions
- [ ] `sunab()` correctly specified if using Sun-Abraham
- [ ] Absorbed variables not also included as controls

**`did` / `fastdid`:**
- [ ] `control_group` parameter matches paper choice ("nevertreated" vs "notyettreated")
- [ ] `anticipation` parameter set if pre-treatment effects expected
- [ ] Aggregation method matches paper presentation (simple, group, calendar, event)
- [ ] Panel vs. repeated cross-section correctly specified

**`rdrobust`:**
- [ ] Bandwidth selector matches paper description
- [ ] Kernel choice documented (triangular default)
- [ ] Bias-corrected confidence intervals used (not conventional)
- [ ] Cluster option used if data is clustered

**`Synth` / `tidysynth` / `augsynth`:**
- [ ] Predictor variables match paper
- [ ] Time periods for fitting correct
- [ ] Permutation loop covers all donor units

**`sandwich` / `clubSandwich`:**
- [ ] Correct `type` argument (HC1/HC2/HC3, CR0/CR1/CR2)
- [ ] Small-sample adjustment appropriate for cluster count

**Other recognized packages:**
- `staggered`, `did2s`, `didimputation`, `eventstudyr` -- check options match design
- `ivreg`, `ivpack` -- check instrument specification
- `rdlocrand` -- check window selection for randomization inference RDD
- `gsynth`, `augsynth` -- check factor model or augmented specifications
- `sensemakr` -- Oster-style sensitivity for observational studies
- `wildrwolf`, `fwildclusterboot` -- check bootstrap parameters
- `pwr`, `DeclareDesign` -- check power calculation assumptions

**Note:** Flag non-standard package choices for user awareness but do NOT treat them as errors. Validate correctness within the chosen package's API.

---

## Phase 4: Polish & Completeness

_Runs only if Phases 2-3 have no unresolved CRITICAL issues. Lower priority -- a working paper missing some of these is MINOR, not MAJOR._

### Causal-Design Robustness Checks
- [ ] Oster (2019) bounds: $\delta$ and $R^2_{\max}$ reported for key coefficients
- [ ] Placebo tests: wrong treatment group, wrong treatment timing
- [ ] Alternative specifications: varying controls, functional form
- [ ] Alternative samples: dropping outliers, different time windows
- [ ] Alternative clustering: robustness to different cluster levels
- [ ] Coefficient stability: adding controls shouldn't drastically change estimates
- [ ] Leave-one-out: drop one project/organization/language at a time (for aggregate designs)

### Repository-Mining Robustness Checks
- [ ] **Sample sensitivity:** Do results change under an alternative repository sample (different selection criteria, stricter engineered-project filter)?
- [ ] **Bot-filter sensitivity:** Do results survive stricter/looser bot and identity-merge rules?
- [ ] **Metric-definition sensitivity:** Does a different operationalization of the key metric change the conclusion?
- [ ] **Subsample stability:** Do estimates hold across languages, domains, project ages, and time windows?
- [ ] **Alternative models:** Does a simpler model produce similar conclusions? If so, what does the richer model buy?
- [ ] **Survivorship check:** Are results driven by only the surviving/active projects?

### Theory + Empirics Robustness Checks
- [ ] **Alternative model specifications:** Do predictions survive under relaxed assumptions?
- [ ] **Competing models:** What alternative models generate different predictions? Can the data distinguish them?
- [ ] **Robustness of empirical tests:** Do test results hold under alternative specifications, samples, or measures?
- [ ] **Heterogeneity in support:** Is the model supported more in some subsamples than others? What does that imply?

### Descriptive / Measurement Robustness Checks
- [ ] **Alternative construction choices:** Do key facts survive different thresholds, weights, imputations?
- [ ] **Alternative data sources:** Can the patterns be replicated with different data?
- [ ] **Temporal stability:** Do the facts hold across different time periods?
- [ ] **Subgroup patterns:** Are the facts driven by a specific subgroup, or do they hold broadly?

### Assumption Stress Test (all paper types)
- [ ] Internal validity threats enumerated and addressed
- [ ] External validity discussed: who/what/where does this generalize to?
- [ ] Spillover / general equilibrium effects considered
- [ ] Selection on unobservables: Oster bounds or similar sensitivity (causal designs)
- [ ] Measurement error: attenuation bias discussed if relevant
- [ ] Sample selection: Heckman-style concerns if applicable

### Citation Fidelity
For methodological claims, verify correct citations:
- [ ] Callaway-Sant'Anna: Callaway & Sant'Anna (2021, Journal of Econometrics)
- [ ] Sun-Abraham: Sun & Abraham (2021, Journal of Econometrics)
- [ ] Borusyak-Jaravel-Spiess: BJS (2024, Review of Economic Studies)
- [ ] de Chaisemartin-D'Haultfoeuille: dCDH (2020, American Economic Review)
- [ ] `rdrobust`: Calonico, Cattaneo & Titiunik (2014, Econometrica) and CCT (2020)
- [ ] Wild cluster bootstrap: Cameron, Gelbach & Miller (2008, REStat)
- [ ] Oster bounds: Oster (2019, Journal of Business & Economic Statistics)
- [ ] Romano-Wolf: Romano & Wolf (2005, Econometrica; 2016)
- [ ] Goodman-Bacon decomposition: Goodman-Bacon (2021, Journal of Econometrics)
- [ ] Montiel Olea-Pflueger: (2013, Journal of Business & Economic Statistics)
- [ ] Roth pre-trends test: Roth (2022, American Economic Review: Insights)
- [ ] Synthetic control: Abadie, Diamond & Hainmueller (2010, JASA; 2015, AJPS)

Cross-reference against `Bibliography_base.bib`.

**Weight by relevance:** Not every paper needs every robustness check. A missing Oster bound is minor if the design is strong. A missing placebo test is more concerning if the identifying variation is novel.

---

## Report Format

Save report to `quality_reports/[FILENAME]_strategy_review.md`:

```markdown
# Strategy Review: [Filename]
**Date:** [YYYY-MM-DD]
**Reviewer:** strategist-critic

## Phase 1: Claim Identification
- **Paper type:** [Causal (mining) / Repository mining (observational) / Theory+Empirics / Descriptive]
- **Design(s) or approach:** [DiD (staggered) / IV / RDD / Experiment / Survey / Case study / Mining regression / Propositions+tests / Measurement / etc.]
- **Estimand or target:** [ATT / ATE / LATE / effect size (Cliff's delta, Â12) / association / stylized fact]
- **Treatment or variation:** [description]
- **Control or comparison:** [description]
- **Outcome(s):** [description]

## Phase 2: Core Design Validity
### Design Check: [Design Name]
**Assessment:** [SOUND / CONCERNS / CRITICAL ISSUES]

#### Issues Found: N
##### Issue 2.1: [Brief title]
- **Location:** [file:line or slide/section]
- **Severity:** [CRITICAL / MAJOR / MINOR]
- **Problem:** [what's wrong]
- **Suggested fix:** [specific correction]

### Sanity Check
- **Sign:** [plausible / questionable -- why]
- **Magnitude:** [plausible / questionable -- back-of-envelope]
- **Dynamics:** [coherent / concerning -- what pattern]
- **Consistency:** [stable / fragile -- across what]

## Phase 3: Inference
### Issues Found: N
[issues if any]

## Phase 4: Polish & Completeness
### Issues Found: N
[issues if any -- note these are lower priority]

## Summary
- **Overall assessment:** [SOUND / MINOR ISSUES / MAJOR ISSUES / CRITICAL ERRORS]
- **Critical issues (must fix):** N
- **Major issues (should fix):** N
- **Minor issues (consider):** N

## Priority Recommendations
1. **[CRITICAL]** [Most important -- fix before anything else]
2. **[MAJOR]** [Second priority]
3. **[MINOR]** [Nice to have]

## Positive Findings
[2-3 things the analysis gets RIGHT -- acknowledge rigor where it exists]
```
