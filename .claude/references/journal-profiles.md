# Venue Profiles

<!--
These profiles calibrate the domain-referee and methods-referee when reviewing
for a specific journal or conference. Each profile describes the venue's review
culture in plain language — the LLM adapts its priorities accordingly.

Used by: domain-referee.md, methods-referee.md (via /review --peer [venue])
-->

## How This Works

When `/review --peer [venue]` is invoked:

1. **Editor reads the paper** → desk review (reject or send to referees)
2. **Editor selects referees** → draws dispositions and pet peeves from the venue's **Referee pool**
3. **Profile found below** → referees calibrate using the full profile
4. **Profile NOT found** → referees use the venue name + .claude/references/domain-profile.md to adapt (still better than generic)
5. **No venue specified** → generic top-journal referee behavior

### Referee Pool Field

Each venue profile includes a **Referee pool** that weights which dispositions the editor draws from. The two referees always get DIFFERENT dispositions. Dispositions: RIGOR, RELEVANCE, MEASUREMENT, GENERALIZABILITY, THEORY, OPENSCIENCE (see editor.md for definitions).

- **RIGOR** — internal validity hawk: confounds, study design flaws, statistical conclusion validity
- **RELEVANCE** — practitioner impact: "what should a team lead or engineering manager do differently?"
- **MEASUREMENT** — construct validity of metrics: "does commit count actually measure productivity?"
- **GENERALIZABILITY** — external validity skeptic: "does this hold beyond your OSS sample / single company?"
- **THEORY** — theory building and testing: "what is the underlying theory, and does the study test it?"
- **OPENSCIENCE** — replication and artifact hawk: replication package, preregistration, data availability

### Table Format Convention

**Default:** Report effect sizes with confidence intervals alongside p-values, per the ACM SIGSOFT Empirical Standards. Significance stars (`*` p<0.05, `**` p<0.01, `***` p<0.001) are acceptable if defined in table notes, but stars alone never substitute for effect sizes. Standard errors or CIs in parentheses, booktabs formatting. A venue profile below may include a **Table format** override.

---

## Software Engineering — Top Journals

### IEEE Transactions on Software Engineering (TSE)
**Focus:** All areas of software engineering — the field's broadest and most established journal
**Bar:** Must interest software engineering researchers outside your subarea. Significant question, rigorous execution, clear contribution over the state of the art.
**Domain referee adjusts:** "Would a testing researcher care about this developer-productivity paper?" Contribution must be broad. Literature positioning against the *general* SE frontier, not just the subarea. Practitioner implications welcome but not required — insight is enough.
**Methods referee adjusts:** Study design must be convincing to non-specialists. Explicit threats-to-validity section (construct, internal, external, conclusion) is mandatory. Clean, transparent design preferred over a technically complex one. Replication package expected.
**Typical concerns:** "Why should SE researchers outside this subarea care?" "Is the contribution big enough for TSE?" "Are the threats to validity honestly discussed?"
**Referee pool:** RIGOR (high), GENERALIZABILITY (medium), MEASUREMENT (medium), THEORY (low), RELEVANCE (low), OPENSCIENCE (medium)

### ACM Transactions on Software Engineering and Methodology (TOSEM)
**Focus:** Software engineering with emphasis on methodology, foundations, and well-developed conceptual contributions
**Bar:** Depth and completeness. TOSEM values thorough treatment — a mature, well-rounded contribution rather than a first result.
**Domain referee adjusts:** Conceptual framing matters — the paper should articulate *why* the phenomenon occurs, not just *that* it occurs. Mechanism evidence and qualitative triangulation valued. Positioning within a well-developed related-work section expected.
**Methods referee adjusts:** Every design choice must be justified. Mixed-methods triangulation appreciated. Full battery of robustness/sensitivity analyses. Careful treatment of statistical inference — corrections for multiple comparisons, appropriate non-parametric tests, effect sizes.
**Typical concerns:** "What is the mechanism?" "Have you triangulated with qualitative evidence?" "Is the treatment complete enough for a journal-length contribution?"
**Referee pool:** THEORY (high), RIGOR (high), MEASUREMENT (medium), GENERALIZABILITY (medium), OPENSCIENCE (medium), RELEVANCE (low)

### Empirical Software Engineering (EMSE)
**Focus:** Empirical studies of software engineering — experiments, case studies, surveys, repository mining, replications
**Bar:** Methodologically exemplary empirical work. The venue where empirical rigor is the contribution's core. Offers a registered-reports track.
**Domain referee adjusts:** The empirical method is scrutinized as much as the finding. Replications and negative results welcome if rigorous. Deep engagement with empirical-SE methodological literature (Wohlin et al., Runeson & Höst, Kitchenham, ACM SIGSOFT Empirical Standards) expected.
**Methods referee adjusts:** Design must follow the relevant empirical standard for its method (experiment, case study, survey, mining). Power analysis for experiments. Sampling strategy justified for mining studies. Instrument validation for surveys. Preregistration and replication packages viewed very favorably.
**Typical concerns:** "Does the design follow the empirical standards for this method?" "What is the sampling frame?" "Is the effect practically significant, not just statistically?" "Where is the replication package?"
**Referee pool:** RIGOR (high), MEASUREMENT (high), OPENSCIENCE (high), GENERALIZABILITY (medium), THEORY (low), RELEVANCE (low)

---

## Software Engineering — Strong Journals

### Journal of Systems and Software (JSS)
**Focus:** Broad software engineering and systems — development practices, architecture, quality, maintenance, process
**Bar:** Solid contribution with sound methodology. Below TSE/TOSEM bar but same expectations of methodological transparency.
**Domain referee adjusts:** Contribution should be meaningful to the subarea. Practical applicability appreciated. Literature positioning within the subarea acceptable. In-practice studies (industry collaborations) valued.
**Methods referee adjusts:** Standard empirical-SE rigor: threats to validity, effect sizes, appropriate statistics. Systematic literature reviews held to Kitchenham guidelines. Replication package increasingly expected.
**Typical concerns:** "Is this incremental relative to [closely related paper]?" "Would practitioners recognize this problem?" "Are the threats to validity addressed?"
**Referee pool:** RIGOR (high), RELEVANCE (medium), MEASUREMENT (medium), GENERALIZABILITY (medium), THEORY (low), OPENSCIENCE (low)

### Information and Software Technology (IST)
**Focus:** Software engineering with emphasis on evidence-based practice — SLRs, surveys, industrial studies
**Bar:** Sound evidence-based contribution. Historically the home of systematic literature reviews and mapping studies.
**Domain referee adjusts:** Evidence synthesis quality is paramount for SLRs — search strategy, inclusion criteria, quality assessment. For primary studies, industrial relevance and realistic settings valued.
**Methods referee adjusts:** SLRs: Kitchenham & Charters protocol compliance, reproducible search strings, inter-rater reliability (Cohen's kappa). Surveys: sampling frame, response rate, instrument validation. Threats-to-validity discussion mandatory.
**Typical concerns:** "Is the search strategy reproducible?" "What is the response rate and non-response bias?" "How were disagreements between coders resolved?"
**Referee pool:** MEASUREMENT (high), RIGOR (high), GENERALIZABILITY (medium), OPENSCIENCE (medium), THEORY (low), RELEVANCE (medium)

### IEEE Software
**Focus:** Practitioner-oriented software engineering — actionable insight for working engineers and engineering managers
**Bar:** Must offer something a practitioner can use. Rigor matters, but accessibility and actionability are the differentiators. Shorter format.
**Domain referee adjusts:** Practitioner takeaways front and center — not an afterthought. Jargon-free writing. Real-world context and war stories valued. Sidebar-friendly structure.
**Methods referee adjusts:** Evidence must be sound but exhaustive robustness is not expected given the format. One well-executed study with honest limitations is enough. Visual evidence valued.
**Typical concerns:** "What should a team lead do differently on Monday?" "Can a practitioner read this in one sitting?" "Is the advice supported by the evidence presented?"
**Referee pool:** RELEVANCE (high), MEASUREMENT (medium), RIGOR (medium), GENERALIZABILITY (low), THEORY (low), OPENSCIENCE (low)

---

## Software Engineering — Top Conferences

### International Conference on Software Engineering (ICSE)
**Focus:** The flagship SE conference — all areas, strongest competition
**Bar:** Novel, significant, and rigorously evaluated. Must survive a program committee that reads hundreds of submissions — the contribution must be crisp.
**Domain referee adjusts:** Novelty weighted heavily — "what is new here?" must be answerable in one sentence. Broad interest to the ICSE audience. Strong motivation section tied to a real SE problem.
**Methods referee adjusts:** Evaluation must match the claim: user study for usability claims, benchmark for technique claims, statistical rigor for empirical claims. Threats to validity mandatory. Artifact evaluation (Available/Functional/Reusable badges) strongly encouraged.
**Typical concerns:** "What exactly is the delta over prior work?" "Does the evaluation support the claims?" "Is this ICSE-level significance or a workshop paper?"
**Referee pool:** RIGOR (high), THEORY (medium), MEASUREMENT (medium), GENERALIZABILITY (medium), OPENSCIENCE (medium), RELEVANCE (low)

### ACM International Conference on the Foundations of Software Engineering (FSE)
**Focus:** Foundations and practice of software engineering — sibling flagship to ICSE
**Bar:** Same tier as ICSE. Slightly more receptive to foundational and conceptual contributions.
**Domain referee adjusts:** As ICSE. Conceptual and foundational contributions (new abstractions, new problem formulations) valued alongside empirical results.
**Methods referee adjusts:** As ICSE — evaluation matched to claims, threats to validity, artifact badges encouraged.
**Typical concerns:** "Is the problem formulation itself a contribution?" "Does the evaluation generalize?" "Artifact available?"
**Referee pool:** RIGOR (high), THEORY (high), MEASUREMENT (medium), GENERALIZABILITY (medium), OPENSCIENCE (medium), RELEVANCE (low)

### International Conference on Automated Software Engineering (ASE)
**Focus:** Automation in software engineering — analysis, testing, synthesis, AI4SE
**Bar:** A working automated technique with rigorous evaluation against strong baselines.
**Domain referee adjusts:** The technique is the star — clear algorithmic contribution. Comparison against the strongest available baselines, not strawmen. Benchmark selection justified.
**Methods referee adjusts:** Benchmark hygiene: no data leakage, statistical tests over multiple runs, effect sizes for tool comparisons (Vargha-Delaney Â12), ablation studies. Reproducibility package near-mandatory.
**Typical concerns:** "Did you compare against [strongest recent baseline]?" "Is the benchmark representative?" "Multiple runs with statistical testing?"
**Referee pool:** RIGOR (high), MEASUREMENT (high), OPENSCIENCE (high), GENERALIZABILITY (medium), THEORY (low), RELEVANCE (low)

### Empirical Software Engineering and Measurement (ESEM)
**Focus:** Empirical methods and measurement in SE — the methodology-centric conference
**Bar:** Methodologically careful empirical study. The conference counterpart of EMSE. Registered-reports track available.
**Domain referee adjusts:** Method-focused audience — expects explicit study design reporting (goal, questions, metrics; GQM tradition). Replications welcome.
**Methods referee adjusts:** Full compliance with the empirical standard for the method used. Explicit research questions mapped to analyses. Power analysis, effect sizes, non-parametric tests where assumptions fail. Preregistration valued.
**Typical concerns:** "Are the research questions answerable with this design?" "Where is the power analysis?" "Construct validity of the metrics?"
**Referee pool:** RIGOR (high), MEASUREMENT (high), OPENSCIENCE (high), THEORY (medium), GENERALIZABILITY (medium), RELEVANCE (low)

### Mining Software Repositories (MSR)
**Focus:** Mining and analysis of software repository data — GitHub, issue trackers, CI logs, Q&A platforms
**Bar:** Novel insight from repository data with careful data curation and analysis. Data-and-tool papers also welcome.
**Domain referee adjusts:** Data curation is a first-class concern — repository selection criteria (stars ≠ quality; see perils-of-mining literature), bot filtering, identity merging. Insight must go beyond descriptive counting.
**Methods referee adjusts:** Sampling strategy justified (SEART GHS-style criteria). Confounds in observational repository data addressed — causal claims need causal designs (DiD, RDD, matching around platform changes). Statistical rigor with large-N data: effect sizes matter more than p-values. Data and scripts shared.
**Typical concerns:** "How did you select the repositories?" "Did you filter bots and merge developer identities?" "Is this correlation dressed as causation?" "Is the dataset shared?"
**Referee pool:** MEASUREMENT (high), RIGOR (high), OPENSCIENCE (high), GENERALIZABILITY (medium), THEORY (low), RELEVANCE (low)

### International Conference on Software Maintenance and Evolution (ICSME)
**Focus:** Software maintenance, evolution, refactoring, technical debt, program comprehension
**Bar:** Solid contribution on how software changes over time. Industrial studies valued.
**Domain referee adjusts:** Maintenance-specific framing — technical debt, code smells, refactoring, legacy systems, comprehension. Longitudinal perspective valued.
**Methods referee adjusts:** Longitudinal repository analyses: survivorship bias, project-age confounds, release-cycle effects. Tool evaluations against realistic maintenance tasks.
**Typical concerns:** "Does this hold across project lifecycles?" "Survivorship bias in your project sample?" "Would maintainers actually use this?"
**Referee pool:** RIGOR (high), MEASUREMENT (medium), RELEVANCE (medium), GENERALIZABILITY (medium), OPENSCIENCE (medium), THEORY (low)

### Cooperative and Human Aspects of Software Engineering (CHASE)
**Focus:** Human and social aspects — teams, collaboration, communication, developer experience, well-being
**Bar:** Rigorous study of the human side of software engineering. The natural home for dev-team-management research.
**Domain referee adjusts:** Grounding in behavioral/social science theory expected (teamwork models, motivation theory, organizational behavior). Mixed methods valued. Developer experience and team process constructs must be carefully defined.
**Methods referee adjusts:** Human-subjects rigor: ethics/IRB, informed consent, instrument validation (Cronbach's alpha), qualitative coding reliability, member checking for interviews. Surveys held to Kitchenham-Pfleeger standards.
**Typical concerns:** "What theory of team behavior underpins this?" "How were the qualitative data coded and validated?" "Ethics approval?" "Self-selection of participants?"
**Referee pool:** THEORY (high), MEASUREMENT (high), RIGOR (medium), GENERALIZABILITY (medium), RELEVANCE (medium), OPENSCIENCE (low)

---

## Add Your Own Venue

Copy this template and add it above this section:

```markdown
### [Venue Name] ([Abbreviation])
**Focus:** [areas and topics covered]
**Bar:** [what it takes to publish here]
**Domain referee adjusts:** [what matters most to domain reviewers at this venue]
**Methods referee adjusts:** [rigor expectations, preferred methods, required checks]
**Typical concerns:** [common referee questions at this venue]
**Referee pool:** [disposition] (high/medium/low) for each: RIGOR, RELEVANCE, MEASUREMENT, GENERALIZABILITY, THEORY, OPENSCIENCE
**Table format:** [optional — only include if venue deviates from the default effect-sizes-plus-CIs convention]
```
