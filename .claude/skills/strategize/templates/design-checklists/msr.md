# Design Checklist: Mining Software Repositories (MSR)

Anchored in the MSR methodological literature (perils-of-mining work; SEART GHS-style curation) and the ACM SIGSOFT Empirical Standards (Ralph et al., 2021).

## Research Questions
- [ ] **RQs stated** and answerable with repository/telemetry data
- [ ] **Descriptive vs. causal** goals distinguished up front (causal claims require a causal design — see `did.md`, `iv.md`, `rdd.md`, `event-study.md`)

## Sample Construction
- [ ] **Repository selection criteria** explicit and justified — stars are NOT a proxy for engineered software; use curated criteria (SEART GHS, minimum activity/history, non-fork, real releases)
- [ ] **Sampling frame and size** documented; convenience vs. representative sampling acknowledged
- [ ] **Bots filtered** (CI bots, dependabot) or explicitly retained with justification
- [ ] **Developer identities merged** across emails/handles before any per-developer metric
- [ ] **Survivorship bias** addressed — not only active/popular projects
- [ ] **Time window** justified (release cycles, platform changes, project-age effects)
- [ ] **Snapshot pinned:** extraction date and tool versions recorded (API pulls are time-dependent)

## Metrics and Construct Validity
- [ ] **Each metric measures the claimed construct** — argued, not asserted ("commits/LOC/churn measures X because ...")
- [ ] **Operationalization documented** — exact query, thresholds, edge cases, reproducible
- [ ] **Skew handled** — LOC/churn/latency are heavy-tailed; log or rank-based methods, medians over means
- [ ] **Manual labels** (if any) coded with a scheme and inter-rater reliability (Cohen's kappa)

## Analysis Plan
- [ ] **Confounders identified and controlled** — project size, age, domain, team size, language
- [ ] **Clustering at the project level** when observations are commits/PRs/issues within projects
- [ ] **Effect sizes with CIs** (Cliff's delta / Â12) reported; **multiple-comparison correction** across many metrics/RQs
- [ ] **Causal language matched to design** (INV-8) — no causal verbs for correlational results
- [ ] **Non-parametric tests** where distributional assumptions fail

## Reproducibility
- [ ] **Mining scripts and dataset shared** (or a clear reason: private telemetry, ethics)
- [ ] **Pipeline reproducible** end-to-end from raw pull to results
- [ ] **Ethics** for developer data (aggregation, anonymization) where applicable

## Threats to Validity
- [ ] **Construct:** metric validity, operationalization
- [ ] **Internal:** confounds, unfiltered bots, unmerged identities
- [ ] **External:** OSS-only generalizability, ecosystem/language bias, survivorship
- [ ] **Conclusion:** appropriate statistics, multiple comparisons, effect sizes

## Referee Objections to Anticipate
- "Stars don't mean engineered software" — show curated selection criteria
- "Did you filter bots and merge identities?" — document both
- "This is correlation dressed as causation" — add a causal design or soften the claim
- "Not generalizable beyond open source" — scope the claim; discuss industrial relevance
- "Where is the dataset?" — share scripts and data (artifact evaluation)
