# Design Checklist: Controlled Experiment (Human Subjects)

Anchored in Wohlin et al. (2012), *Experimentation in Software Engineering*, and the ACM SIGSOFT Empirical Standards (Ralph et al., 2021).

## Research Design
- [ ] **Goal, questions, metrics stated** (GQM-style): what construct does each metric operationalize?
- [ ] **Hypotheses** are explicit, directional where justified, and grounded in a theory of developer/team behavior where possible
- [ ] **Design type:** between-subjects, within-subjects, or mixed — and why
- [ ] **Independent variables (treatments)** and their levels defined
- [ ] **Dependent variables** defined with measurement procedure
- [ ] **Confounding factors** identified and controlled (blocking, counterbalancing, randomization)

## Participants
- [ ] **Population and sampling frame** described (students, professionals, mixed) — and the generalizability implication stated
- [ ] **Recruitment** procedure and inclusion/exclusion criteria
- [ ] **Sample size justified by an a priori power analysis** (effect size of interest, alpha, target power)
- [ ] **Assignment to conditions** is randomized; the mechanism is described and reproducible
- [ ] **Ethics/IRB** approval and informed consent

## Tasks and Materials
- [ ] **Tasks** representative of real engineering work; construct validity argued, not assumed
- [ ] **Task difficulty** calibrated (pilot); ceiling/floor effects checked
- [ ] **Materials** (code, tools, instructions) documented and shareable
- [ ] **Learning/ordering effects** controlled (counterbalancing in within-subjects designs)

## Procedure
- [ ] **Protocol** step-by-step, identical across participants except for treatment
- [ ] **Environment** controlled (same tooling, time limits, instructions)
- [ ] **Pilot run** completed; protocol revised accordingly
- [ ] **Demand characteristics** and experimenter effects mitigated (blinding where feasible)

## Analysis Plan
- [ ] **Statistical test matched to design and data** (paired vs. unpaired; parametric only if assumptions hold — otherwise Mann-Whitney/Wilcoxon)
- [ ] **Effect sizes reported** (Cliff's delta or Vargha-Delaney Â12) with confidence intervals — not p-values alone
- [ ] **Multiple-comparison correction** (Benjamini-Hochberg / Bonferroni) when several hypotheses are tested
- [ ] **Handling of outliers and failed attention/manipulation checks** pre-specified
- [ ] **Practical significance** interpreted, not just statistical significance

## Threats to Validity
- [ ] **Construct:** do the tasks and metrics measure the intended construct?
- [ ] **Internal:** confounds, selection, maturation, instrumentation, ordering
- [ ] **External:** participant representativeness, task realism, setting
- [ ] **Conclusion:** power, assumption violations, reliability of measures

## Pre-registration
- [ ] Design, hypotheses, and analysis plan pre-registered (OSF) before data collection — see `pap-templates/osf-experiment.md`
- [ ] Deviations from the plan logged and justified

## Referee Objections to Anticipate
- "Students are not professionals" — defend the population or scope the claim
- "The tasks are toy problems" — argue task realism and construct validity
- "Underpowered" — show the power analysis
- "No effect sizes" — report Cliff's delta / Â12 with CIs
