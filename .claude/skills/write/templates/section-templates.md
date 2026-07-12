# Section Templates — Paper Drafting by Section and Paper Type

Structure guidance for each major section of an empirical software engineering paper, adapted by paper type (causal mining study, controlled experiment, survey, case study, descriptive/exploratory).

Standard section order: Introduction; Background/Related Work; Study Design/Methodology; Results (by RQ); Discussion; Threats to Validity; Conclusion.

---

## Paper Types

Identify the type from the strategy memo before drafting. The writer must recognize each type and shift structure accordingly.

| Type | Signature | Design section becomes |
|------|-----------|------------------------|
| **Causal mining study** | DiD, IV, RDD, event study on repository/telemetry data | Study Design (causal) |
| **Controlled experiment** | Participants, tasks, treatments, measured performance | Experiment Design |
| **Survey** | Instrument, sampling frame, response analysis | Survey Design |
| **Case study** | Industrial/project context, protocol, qualitative themes | Case Study Protocol |
| **Descriptive / exploratory** | New dataset, new measure, empirical characterization | Data Construction / Measurement |

Methods papers (new metric or analysis technique) combine the descriptive template with a validation study drawn from one of the other types.

---

## Introduction (1000--1500 words)

**Common backbone (all paper types):**
1. **Motivation** -- Opening fact or puzzle from software practice (1--2 sentences)
2. **Research question** -- One clear sentence; number the RQs (RQ1, RQ2, ...)
3. **Why it matters** -- Stake for practitioners or for SE research (1--2 sentences)

**Then diverge by type:**

**Causal mining study:**
4. **Design preview** -- We use [design] + [data] to answer [RQ]; the key threat is [X] (2--3 sentences). Example: "We exploit the staggered adoption of CI across 2,400 GitHub projects in a difference-in-differences design to estimate the effect of CI adoption on review latency."
5. **Result statement** -- Main result with magnitude and units (1--2 sentences). Example: "CI adoption reduces median review latency by 6.1 hours (18%)."
6. **Literature positioning** -- Contribution paragraph naming 2--3 specific papers

**Controlled experiment:**
4. **Experiment preview** -- We conduct a [between/within]-subjects experiment with [N participants] performing [tasks] under [treatments] (2--3 sentences)
5. **Effect statement** -- Main effect with magnitude, direction, and effect size (1--2 sentences)
6. **Literature positioning** -- Contribution relative to prior experiments and observational evidence

**Survey:**
4. **Instrument preview** -- We survey [N respondents from population] about [constructs] (2--3 sentences)
5. **Key finding** -- The most important response pattern, with numbers (1--2 sentences)
6. **Literature positioning** -- What the survey adds beyond mined data or prior surveys

**Case study:**
4. **Context preview** -- We study [practice] in [organization/project context] using [interviews/observations/artifacts] (2--3 sentences)
5. **Key insight** -- The central theme or explanatory finding (1--2 sentences)
6. **Literature positioning** -- What the in-context evidence adds

**Descriptive / exploratory:**
4. **Data or measurement innovation** -- We construct [new measure/dataset] using [method] (2--3 sentences)
5. **Key fact** -- The main finding is [fact with magnitude] (1--2 sentences)
6. **Why it matters** -- This fact implies [revision to existing understanding] (1--2 sentences)
7. **Literature positioning** -- What this changes about the empirical landscape

**All types end with:**
- **Roadmap** -- Optional, one sentence maximum

The contribution statement must appear in the first 2 pages. Every RQ stated here must be answered in Results.

---

## Data (800--1200 words)

**Common backbone:**
1. **Source and scope** -- Where the data comes from (e.g., GitHub API, CI logs, issue tracker, survey platform), sample period, sample size
2. **Variable/metric definitions** -- Table reference for summary statistics. Example metrics: review latency, defect density, commits per month, team size, uses CI, build success rate
3. **Sample restrictions** -- Each restriction justified with one sentence (e.g., exclude bots, forks, toy projects; state the filters)
4. **Data quality** -- Missingness, bot activity, identity merging, measurement concerns, how addressed

**Type-specific additions:**

**Causal mining study:** Define treatment (e.g., CI adoption date), outcome metrics, and controls. Explain treatment variation (adoption timing, platform rollout, policy change). Show pre-treatment balance if relevant.

**Controlled experiment:** Describe participants (recruitment, experience, demographics), materials (task systems, code bases), and measured outcomes. Report ethics/IRB approval and consent.

**Survey:** Describe the sampling frame, recruitment channels, response rate, and respondent demographics vs. the target population.

**Case study:** Describe the case context (organization, team, product, process) and the data sources triangulated (interviews, documents, repository data, observations).

**Descriptive / exploratory:** The data section IS the core contribution. Describe construction in detail -- sources, linking, cleaning decisions, validation against external benchmarks. This section is longer (1200--1800 words).

---

## Study Design / Methodology (800--1500 words)

Start from the strategy memo. State the RQs first, then map each RQ to its analysis. The internal structure depends on the paper type.

### Causal Mining Study: Study Design

**Common sequence (all designs):**
1. **Design preview** -- Design and key assumption in plain language, before any equations
2. **Estimand** -- State what you're estimating (ATT, ATE, LATE) and why it's the right target for the RQ
3. **Formal specification** -- Numbered equation with notation from the notation protocol
4. **Key assumption** -- Name it, state it formally, explain what it means in plain language
5. **Assumption validation** -- How you test or support the assumption (pre-trends, balance, placebo, falsification)
6. **Threats preview** -- What could go wrong; point forward to the Threats to Validity section

**Design-specific moves:**

**Difference-in-Differences:**
- State parallel trends assumption in words and formally (e.g., adopter and non-adopter projects would have trended alike absent CI adoption)
- Pre-trends evidence (event study plot reference)
- If staggered adoption: explain the estimator choice (Callaway-Sant'Anna, Sun-Abraham, etc.) and why naive TWFE is inappropriate
- Never-treated vs. not-yet-treated comparison group -- which and why

**Instrumental Variables:**
- Instrument description and institutional motivation (why it's as-good-as-random -- e.g., a platform default change, an ecosystem-wide policy shift)
- Exclusion restriction -- state it, explain why it holds, acknowledge what would violate it
- First stage strength (F-statistic, effective F)
- LATE interpretation -- who are the compliers? Is LATE the practice-relevant parameter?
- Monotonicity -- state and justify

**Regression Discontinuity:**
- Running variable and cutoff (e.g., a size or activity threshold triggering a policy)
- Bandwidth selection method (CCT, IK, or cross-validation)
- Continuity assumption -- what would violate it
- Manipulation tests (McCrary/Cattaneo density test)
- Covariate balance at the cutoff
- Visual evidence (RD plot reference)

**Event Study:**
- Event definition and timing (e.g., first CI build, migration date, incident)
- Pre-period length and why it's sufficient
- Reference period choice
- Dynamic effects interpretation -- distinguish anticipation from pre-trends
- Binning of distant leads/lags if needed

### Controlled Experiment: Experiment Design

Follow Wohlin et al. (2012) and the ACM SIGSOFT Empirical Standards (Ralph et al., 2021):
1. **Hypotheses** -- Null and alternative for each RQ, numbered and linked to RQs
2. **Participants** -- Recruitment, sample size (with power analysis if available), experience, assignment to conditions
3. **Treatments and tasks** -- What varies between conditions; the tasks and materials; how construct validity of the tasks is argued
4. **Procedure** -- Session flow, training, time limits, data capture
5. **Variables** -- Independent, dependent, and controlled variables with operationalizations
6. **Analysis plan** -- Tests per hypothesis (prefer non-parametric when normality fails), effect sizes (Cliff's delta, Vargha-Delaney Â12) with CIs, correction for multiple comparisons
7. **Ethics** -- IRB/ethics approval, consent, compensation

### Survey: Survey Design

Follow Kitchenham & Pfleeger's survey guidelines:
1. **Constructs and RQs** -- What each construct measures and which RQ it serves
2. **Instrument** -- Question development, scales, piloting, validation
3. **Sampling** -- Target population, sampling frame, recruitment, expected biases
4. **Response analysis** -- Response rate, demographic comparison to population, handling of partial responses
5. **Analysis plan** -- Quantitative analysis per construct; coding procedure and inter-rater reliability (e.g., Cohen's kappa) for open-ended responses

### Case Study: Protocol

Follow Runeson & Höst (2009):
1. **Case selection and context** -- Why this case; unit(s) of analysis
2. **Data collection protocol** -- Interviews (guide, sampling, recording), artifacts, observations
3. **Coding and analysis** -- Coding procedure, themes, inter-rater agreement
4. **Triangulation** -- How multiple sources corroborate findings

### Descriptive / Exploratory

No separate design section beyond the RQs and analysis mapping. The contribution is in the data construction (already expanded above) and the presentation of facts in Results.

---

## Results (800--1500 words)

Organize by research question: answer RQ1, then RQ2, etc. Each RQ subsection ends with an explicit answer statement.

**Causal mining study:**
1. **Result statement** -- Main specification, lead with the number
2. **Practical significance** -- What does the magnitude mean for a team? (e.g., "6.1 hours off median review latency is roughly one working day per week for an active reviewer")
3. **Comparison** -- How does this relate to prior estimates?
4. **Heterogeneity** -- Which projects or teams are affected more or less?
5. **Robustness narration** -- What doesn't change the result?

How to narrate by output type:
- **Regression table:** Lead with the preferred specification. "Column 3, which includes [controls/FE], shows [effect]. Adding [X] in Column 4 does not change the estimate."
- **Event study figure:** "Figure N shows [pattern]. The pre-period coefficients are close to zero [confirming parallel trends]. The effect appears in period [T] and [persists/fades/grows]."
- **IV results:** Present first stage, reduced form, and 2SLS together. "The first stage F-statistic is [X]. The reduced-form effect is [Y]. The 2SLS estimate implies [Z], consistent with a LATE of [interpretation]."
- **RD results:** "Figure N shows the discontinuity visually. The local polynomial estimate is [X] (bandwidth [B], chosen by [method]). The effect is robust to alternative bandwidths (Table N)."

**Controlled experiment:**
1. **Results by hypothesis** -- For each hypothesis: descriptive statistics per treatment, the test result, and the effect size with CI. "Participants using [tool] completed the task in [X] minutes vs. [Y] minutes (Â12 = 0.71, 95% CI [0.60, 0.81])."
2. **Results by task/treatment** -- If effects vary across tasks or treatments, report per-cell and discuss why
3. **Secondary outcomes** -- Correctness, perceived workload, qualitative feedback
4. **Deviations** -- Dropouts, protocol deviations, and their handling

**Survey:**
1. **Results by construct/RQ** -- Response distributions per construct, with figures for Likert batteries
2. **Group comparisons** -- Differences across demographics or contexts, with effect sizes
3. **Open-ended themes** -- Coded themes with frequencies and representative quotes
4. **Convergence** -- Where quantitative and qualitative responses agree or diverge

**Case study:**
1. **Themes** -- Each theme stated as a finding, supported by triangulated evidence (quotes + artifacts + repository data)
2. **Chain of evidence** -- Make the path from raw data to interpretation explicit
3. **Negative/deviant cases** -- Evidence that complicates the themes, and how it was handled
4. **Answer per RQ** -- Explicit answer statements grounded in the themes

**Descriptive / exploratory:**
1. **Key facts** -- Numbered, each with magnitude and units. Lead with the most important.
2. **Decompositions** -- Break down variation (across projects, over time, within teams)
3. **Correlations and patterns** -- What predicts the measure? What moves with it?
4. **Comparison to existing measures** -- If replacing or improving on existing data, show the difference matters
5. **Implications** -- What do these facts imply for research or practice?

---

## Discussion (500--900 words)

**All types:**
1. **Interpretation** -- What the answers to the RQs mean, taken together
2. **Implications for practitioners** -- What should teams, tool builders, or engineering leaders do differently based on these results?
3. **Implications for researchers** -- What should the research community study, measure, or replicate next?
4. **Relation to prior work** -- Where results confirm, extend, or contradict the literature

---

## Threats to Validity (400--800 words)

**Required for every paper type.** Organize into the four standard categories (Wohlin et al., 2012):

1. **Construct validity** -- Do the metrics measure the intended constructs? (e.g., is review latency a valid proxy for review efficiency? does commit count measure productivity?) State the threat, the mitigation, and the residual risk.
2. **Internal validity** -- Could something other than the treatment/factor explain the results? Confounding by project maturity, size, domain; selection into treatment; maturation and history effects in experiments.
3. **External validity** -- To what populations, projects, and settings do results generalize? OSS convenience samples vs. industrial settings; participant representativeness; single-case limits.
4. **Conclusion validity** -- Are the statistical conclusions sound? Power, multiple comparisons, violated assumptions, clustered observations (commits/PRs within projects), skewed metrics.

**Rules:**
- One paragraph per category minimum; state threat → mitigation → residual risk
- For causal mining studies, the assumption-validation evidence (pre-trends, placebo, balance) is summarized here and cross-referenced, not repeated
- Do not pad with generic threats; every threat must be specific to this study
- Never claim a threat is "fully eliminated" -- state what remains

---

## Conclusion (300--500 words)

**Common backbone:**
1. **Restatement** -- Answer to each RQ with effect size or key finding (one paragraph)
2. **Takeaway** -- The one thing a reader should remember

**Type-specific endings:**

**Causal mining study:**
3. **Implications for practice** -- What should teams or platforms change based on these estimates?
4. **Future work** -- Brief, one paragraph (e.g., replication in industrial settings, mechanism studies)

**Controlled experiment:**
3. **Implications for tool/practice adoption** -- When does the measured effect justify adoption?
4. **Future work** -- Replications, larger samples, field deployment

**Survey:**
3. **What practitioners and researchers should take away** -- The perception/practice gaps that matter
4. **Future work** -- Triangulation with mined or telemetry data

**Case study:**
3. **Transferability** -- What aspects of the findings plausibly transfer to other contexts, and why
4. **Future work** -- Multi-case extension, quantitative follow-up

**Descriptive / exploratory:**
3. **What changes** -- How should these facts revise existing beliefs?
4. **Agenda** -- What questions can now be answered with this data/measure?

---

## Section Length Summary

| Section | Length | Causal Mining | Experiment | Survey | Case Study | Descriptive |
|---------|--------|---------------|-----------|--------|-----------|-------------|
| Introduction | 1000-1500 | design preview, result, contribution | experiment preview, effect, contribution | instrument preview, finding, contribution | context preview, insight, contribution | data innovation, key fact, contribution |
| Data | 800-1200 | Treatment, outcome metrics, controls | Participants, tasks, materials | Sampling frame, respondents | Case context, data sources | 1200-1800 (core contribution) |
| Study Design | 800-1500 | Design-specific (DiD/IV/RDD/ES) | Hypotheses, treatments, procedure | Instrument, sampling, analysis plan | Protocol, coding, triangulation | Merged into Data + RQs |
| Results | 800-1500 | By RQ: main spec, robustness, heterogeneity | By hypothesis/task/treatment | By construct, themes | Themes with triangulation | Key facts, decompositions |
| Discussion | 500-900 | Implications for practice + research | Same | Same | Same | Same |
| Threats to Validity | 400-800 | Construct, internal, external, conclusion | Same | Same | Same | Same |
| Conclusion | 300-500 | Answers to RQs + takeaway | Same | Same | Same | Same |
| Abstract | 100-150 | Question, design, finding with magnitude | Question, experiment, effect | Question, sample, key finding | Question, case, insight | Question, measurement, key fact |
