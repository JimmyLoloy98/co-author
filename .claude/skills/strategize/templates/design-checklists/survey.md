# Design Checklist: Survey

Anchored in Kitchenham & Pfleeger's survey guidelines and the ACM SIGSOFT Empirical Standards (Ralph et al., 2021).

## Objectives and Constructs
- [ ] **Research questions** the survey answers, stated explicitly
- [ ] **Constructs** defined before item writing (what latent concept does each block measure?)
- [ ] **Descriptive vs. relational** goals distinguished — no causal claims from cross-sectional survey data

## Sampling
- [ ] **Target population** defined precisely (role, experience, ecosystem)
- [ ] **Sampling frame** and method (probability vs. convenience) — and the generalizability implication stated
- [ ] **Sample size** justified (precision for estimates, or power for group comparisons)
- [ ] **Recruitment channel** documented; self-selection bias acknowledged

## Instrument
- [ ] **Item wording** neutral, unambiguous, single-barrelled
- [ ] **Response scales** appropriate (Likert, ordinal, categorical) and consistent
- [ ] **Validated scales reused** where they exist (cite source); new scales justified
- [ ] **Instrument validation:** pilot, expert review, and reliability (Cronbach's alpha for multi-item constructs)
- [ ] **Length and fatigue** controlled; attention checks included
- [ ] **Ordering effects** mitigated (randomize item/block order where feasible)

## Administration
- [ ] **Mode** (online, in-person) and platform documented
- [ ] **Consent and ethics/IRB** in place; anonymity/confidentiality stated
- [ ] **Field period** and reminders documented

## Analysis Plan
- [ ] **Response rate** computed and reported; **non-response bias** assessed (early vs. late responders, comparison to frame)
- [ ] **Missing data** handling pre-specified
- [ ] **Ordinal/Likert data** analyzed appropriately (non-parametric tests, ordinal models — not means of Likert items without justification)
- [ ] **Effect sizes and CIs** for group comparisons
- [ ] **Open-text responses** coded with a documented scheme and inter-rater reliability (Cohen's kappa)

## Threats to Validity
- [ ] **Construct:** do items measure the intended constructs? (content and face validity)
- [ ] **Internal:** common-method bias, social-desirability bias, ordering
- [ ] **External:** sampling frame coverage, self-selection, generalizability beyond respondents
- [ ] **Conclusion:** reliability, sample size, appropriate statistics

## Pre-registration
- [ ] Constructs, hypotheses, and analysis plan pre-registered (OSF) before data collection where the study is confirmatory
- [ ] Deviations logged and justified

## Referee Objections to Anticipate
- "Low response rate" — report it, assess non-response bias, scope the claim
- "Convenience sample" — defend or scope generalizability
- "Self-report is not behavior" — acknowledge the construct gap
- "You averaged Likert items" — justify or switch to ordinal methods
