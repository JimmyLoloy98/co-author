# Domain Profile

<!--
HOW TO USE: Fill this in manually OR let /discover (interactive interview) generate it.
All agents read this file to calibrate their field-specific behavior.
Delete sections that don't apply. Add sections specific to your subfield.
If no field is specified, agents default to empirical software engineering.
-->

## Field

**Primary:** [e.g., Empirical Software Engineering, Software Team Management, Developer Productivity, DevOps, Human Aspects of SE]
**Adjacent subfields:** [e.g., CSCW, HCI, Information Systems — fields whose methods and venues overlap]

---

## Target Venues (ranked by tier)

<!-- The Orchestrator uses this for venue selection. The Librarian prioritizes these in searches. -->

| Tier | Venues |
|------|--------|
| Top journals | IEEE TSE, ACM TOSEM, Empirical Software Engineering (EMSE) |
| Top conferences | [e.g., ICSE, FSE, ASE, ESEM, MSR] |
| Strong journals | [e.g., JSS, IST] |
| Specialty | [e.g., CHASE, ICSME, IEEE Software (practitioner)] |

---

## Common Data Sources

<!-- The Explorer prioritizes these. The explorer-critic knows their quirks. -->

| Dataset | Type | Access | Notes |
|---------|------|--------|-------|
| [e.g., GitHub via REST/GraphQL API] | [repository mining] | [public, rate-limited] | [key strengths and limitations — bots, forks, stars ≠ quality] |
| [e.g., GH Archive / SEART GHS] | [repository metadata] | [public] | [curated sampling of engineered projects] |
| [e.g., Stack Overflow Developer Survey] | [survey] | [public] | [large-N developer demographics and practices; self-selection] |
| [e.g., TravisTorrent / CI logs] | [build telemetry] | [public] | [CI outcomes linked to commits] |
| [e.g., Jira issue-tracker datasets] | [process data] | [public/restricted] | [issue lifecycles, effort estimates] |
| [e.g., DORA / State of DevOps] | [survey] | [aggregate public] | [delivery performance constructs] |
| [e.g., Company telemetry] | [admin] | [restricted, NDA] | [requires ethics approval and anonymization] |

---

## Common Study Designs

<!-- The Strategist considers these first. The strategist-critic knows design-specific threats. -->

| Design | Typical Application | Key Threat to Defend |
|--------|-------------------|----------------------|
| [e.g., Repository mining + regression] | [Practice adoption vs. outcome metrics across projects] | [Confounding by project maturity, size, domain] |
| [e.g., DiD around a platform/policy change] | [Feature or policy rollout on GitHub/CI] | [Parallel trends across adopter and non-adopter projects] |
| [e.g., Controlled experiment (human subjects)] | [Tool or practice effect on task performance] | [Construct validity of tasks; participant representativeness] |
| [e.g., Survey] | [Developer perceptions and practices] | [Sampling frame, non-response bias, instrument validity] |
| [e.g., Case study] | [Practice in its industrial context] | [Generalizability; triangulation across data sources] |

---

## Field Conventions

<!-- The Coder and Writer follow these. The writer-critic checks for them. -->

- [e.g., Report effect sizes (Cliff's delta, Vargha-Delaney Â12) with CIs, not just p-values]
- [e.g., Non-parametric tests (Mann-Whitney, Wilcoxon) when normality fails — common with SE metrics]
- [e.g., Explicit Threats to Validity section: construct, internal, external, conclusion]
- [e.g., Skewed metrics (LOC, churn, resolution time) → log transform or rank-based methods]
- [e.g., Cluster analysis at the project level when observations are commits/PRs within projects]
- [e.g., Mixed methods triangulation valued at TOSEM/CHASE]

---

## Notation Conventions

<!-- The Writer and writer-critic enforce these. -->

| Symbol | Meaning | Anti-pattern |
|--------|---------|-------------|
| [e.g., $Y_{pt}$] | [Outcome for project p in period t] | [Don't use $y$ without subscripts] |
| [e.g., RQ1, RQ2…] | [Numbered research questions mapped to analyses] | [Analyses that answer no stated RQ] |

---

## Seminal References

<!-- The Librarian ensures these are cited when relevant. The strategist-critic knows their methods. -->

| Paper | Why It Matters |
|-------|---------------|
| Wohlin et al. (2012), *Experimentation in Software Engineering* | Canonical guide for controlled experiments in SE |
| Runeson & Höst (2009) | Case study research guidelines for SE |
| Kitchenham & Charters (2007) | Systematic literature review guidelines |
| Ralph et al. (2021), ACM SIGSOFT Empirical Standards | Per-method reporting and review standards |
| Forsgren, Humble & Kim, *Accelerate* / DORA reports | Delivery-performance constructs for DevOps research |
| Stol & Fitzgerald (2018) | ABC framework for choosing research strategies in SE |

---

## Methodological Foundational References

<!-- The Theorist and theorist-critic default to these anchors when building or reviewing a formal
     methods section. Only needed if the paper has formal statistical content (new metrics with
     validity arguments, causal designs, measurement models).
     Leave empty to fall back to the generic statistical-methods defaults baked into the theorist agent. -->

| Topic | Anchor references |
|-------|------------------|
| [e.g., DiD with staggered adoption (MSR causal studies)] | [e.g., Callaway & Sant'Anna (2021)] |
| [e.g., Effect sizes for tool comparisons] | [e.g., Vargha & Delaney (2000); Arcuri & Briand (2014)] |
| [e.g., Survey instrument validation] | [e.g., Kitchenham & Pfleeger (2008)] |

---

## Paper Author Team

<!-- Used by the theorist-critic to calibrate respect. If the authors are themselves among the reference
     literature on a topic, the critic avoids lecturing them on their own contributions.
     List author surnames + the topics they are foundational on. -->

| Author | Foundational on |
|--------|----------------|
| [e.g., surname] | [topic] |

---

## Field-Specific Referee Concerns

<!-- The domain-referee and methods-referee watch for these. -->

- [e.g., "Does commit count / LOC actually measure productivity?" — construct validity of mined metrics is the #1 objection]
- [e.g., "Does this generalize beyond open source?" — OSS convenience samples vs. industrial settings]
- [e.g., "Correlation dressed as causation" — observational mining studies making causal claims without a causal design]
- [e.g., "Bots and identity merging" — unfiltered bot activity and unmerged aliases corrupt repository data]
- [e.g., "Ethics and consent" — human-subjects studies (surveys, interviews, telemetry) need IRB/ethics approval]
- [e.g., "Survivorship bias" — sampling only active, popular projects]

---

## Quality Tolerance Thresholds

<!-- Customize for your domain's standards. Used by quality.md. -->

| Quantity | Tolerance | Rationale |
|----------|-----------|-----------|
| Point estimates | [e.g., 1e-6] | [Numerical precision] |
| Standard errors | [e.g., 1e-4] | [Bootstrap/MC variability] |
| Inter-rater reliability | [e.g., Cohen's kappa >= 0.7] | [Qualitative coding quality] |
