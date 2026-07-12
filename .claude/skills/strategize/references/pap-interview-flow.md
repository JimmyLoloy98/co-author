# PAP Interview Flow

**Purpose:** 6-question guided interview for building a pre-registration plan for a software engineering study. Used when `/strategize pap interactive` is invoked. Ask each question one at a time, wait for the answer, then proceed.

---

## The 6 Questions

### Question 1: Research Question
> What is the research question?

**Listen for:** A precise, falsifiable question. If vague, probe:
- "What is the specific effect or relationship you want to estimate?" (e.g., "does CI adoption reduce review latency?")
- "Who or what is affected? Developers, teams, projects? By what practice, tool, or policy?"
- "What is the counterfactual or comparison?"

**Red flag:** "I want to see if X is related to Y" -- needs sharpening into a causal, correlational, or descriptive question with explicit claim discipline.

---

### Question 2: Study Design
> What is the study design? (controlled experiment / quasi-experiment / observational repository mining / survey)

**Listen for:** The research strategy and, for causal designs, the source of identifying variation.

**Follow-ups by design:**
- **Controlled experiment:** How is randomization done? Between- or within-subjects? Participants — students, professionals? What tasks?
- **Quasi-experiment:** DiD, IV, RDD? What is the platform change, policy rollout, running variable, instrument, or adoption timing? (e.g., staggered CI adoption, a GitHub feature launch)
- **Observational mining:** What is the sampling frame (SEART GHS criteria, GH Archive window)? What confound-control argument — and is the claim explicitly correlational?
- **Survey:** What is the sampling frame and target population? Is the instrument validated or piloted?

---

### Question 3: Primary Outcome Variables
> What are the primary outcome variables? (names, measurement, data source)

**Listen for:** Precise variable definitions, not vague concepts.

**Probe if needed:**
- "How exactly is this measured?" (e.g., review latency = hours from PR open to first review)
- "What data source provides this variable?" (GitHub API, CI logs, survey instrument, task artifacts)
- "At what level is it measured? (commit / PR / developer / team / project)"
- "What is the timing of measurement relative to treatment or exposure?"
- "Does the mined metric actually measure the construct?" (commit count is not productivity; LOC is not effort)

**Red flag:** More than 3-4 primary outcomes without a multiple testing plan.

---

### Question 4: Source of Identifying Variation
> What identifies the effect? (randomization mechanism / treatment assignment / platform or policy change — or, for descriptive mining, the sampling frame)

**Listen for:** The specific variation that identifies the causal effect, or an honest statement that the study is correlational.

**Probe if needed:**
- "What is the comparison group and why is it valid?" (never-adopters vs. not-yet-adopters?)
- "What must be true for this comparison to give you the causal effect?" (parallel trends, exclusion restriction, continuity, etc.)
- "Is adoption timing plausibly exogenous, or do projects self-select into the practice?"

**Connect to Question 2:** The design and the identification argument should be consistent. If there is no identifying variation, the claim must be correlational.

---

### Question 5: Subgroup Analyses
> What subgroup analyses are pre-specified? (with justification for each)

**Listen for:** Theory-driven subgroups, not data-driven fishing.

**Good examples:** Project size or team size (if the mechanism is coordination overhead), developer experience (if novices benefit differently from tooling), project domain or language (if institutional differences are expected).

**Red flag:** "We'll look at all possible subgroups and report what's significant." This is exactly what pre-registration is designed to prevent.

**Probe:** "What theoretical reason justifies this subgroup analysis?"

---

### Question 6: Multiple Testing
> What multiple testing concerns exist? (number of primary outcomes, family-wise error rate plan)

**Listen for:** Awareness of the problem and a planned correction method.

**Key dimensions:**
- How many primary outcomes? (if >1, correction needed)
- How many subgroups? (each additional subgroup multiplies tests)
- Are outcomes in the same "family"? (corrections apply within families)

**Correction methods to suggest:**
- **Bonferroni:** Simple, conservative. Good for 2-3 outcomes.
- **Benjamini-Hochberg:** Controls false discovery rate. Good for 4-10 outcomes.
- **Romano-Wolf:** Accounts for correlation structure. Best for many correlated outcomes.

**Also confirm:** Effect sizes (Cliff's delta, Vargha-Delaney Â12) with CIs will be reported alongside p-values, per the ACM SIGSOFT Empirical Standards.

---

## After All 6 Answers

1. Summarize what was collected
2. Ask: "Is there anything else you'd like to pre-specify?" (mechanisms, exploratory analyses, data curation decisions — bot filtering, identity merging, time windows)
3. Ask: "Registration route?" (OSF pre-registration / registered report at EMSE, ESEM, or MSR)
4. Proceed to PAP drafting using the appropriate template:
   - Controlled experiment → `templates/pap-templates/osf-experiment.md`
   - Observational / quasi-experimental mining → `templates/pap-templates/osf.md`

---

## Observational Study Adaptations

For observational/quasi-experimental mining designs, adapt Questions 2, 4, and 5:

**Question 2 (Design):** Focus on the source of quasi-random variation (platform change, staggered rollout), not randomization.
**Question 4 (Identification):** Focus on the key identifying assumption and its testable implications — or lock in correlational language if there is none.
**Question 5 (Subgroups):** Also include pre-committed specification choices:
- Bandwidth (RDD)
- Control variables (confound adjustment — project age, size, domain)
- Sample restrictions and repository selection criteria (all designs)
- Bot-filter rules, identity merging, time windows (mining)
- Functional form (polynomial order, log vs. levels for skewed SE metrics)
