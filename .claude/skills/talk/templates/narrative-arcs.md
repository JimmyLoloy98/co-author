# Narrative Arcs by Paper Type

How to structure the story of a talk depends on what kind of paper you're presenting. Each arc below specifies the slide sequence, what to emphasize, what to deemphasize, and how to calibrate for different audiences.

---

## Causal Mining Study

**Arc:** Puzzle --> Quasi-experiment in the ecosystem --> Credible estimates --> So what for teams

| # | Section | Slides | What to do |
|---|---------|--------|------------|
| 1 | Hook | 1-2 | Engineering-practice question or empirical puzzle. Make it concrete. |
| 2 | What we know / don't know | 1 | Brief lit positioning -- NOT a literature review. |
| 3 | Data + variation | 1-2 | What repositories/telemetry, what exogenous variation (platform change, staggered adoption). |
| 4 | Study design | 1 | The design in one slide. Audience must get it in 30 seconds. |
| 5 | Key result | 1 | Main result with effect size and units. Visually distinct -- larger font, highlighted, different layout. |
| 6 | Visual evidence | 1-2 | Event study plot, RD plot, or equivalent. Let the figure speak. |
| 7 | Robustness + threats | 1 | Brief -- "result survives X, Y, Z; main threats addressed." Details in backup. |
| 8 | So what | 1 | Implication for teams/practitioners or what we learned. |

**Emphasize:** The study design and threats to validity. This is what the audience evaluates.
**Deemphasize:** Literature review (one slide max), robustness details (backup slides).
**Audience calibration:** Job market -- show you understand threats to internal validity. Seminar -- emphasize effect magnitudes and practical relevance. Conference -- skip lit, go straight to design and result.

---

## Controlled Experiment

**Arc:** Question --> Design --> Evidence --> Practice

| # | Section | Slides | What to do |
|---|---------|--------|------------|
| 1 | Hook | 1-2 | The practice or tool whose effect you test -- why it matters. |
| 2 | Hypotheses | 1-2 | Numbered, grounded in a theory of developer/team behavior where possible. |
| 3 | Design | 2-3 | Participants, tasks, treatments, within/between-subjects, randomization. One concept per slide. |
| 4 | Construct validity | 1 | Why the tasks and metrics measure what you claim. Address demand effects. |
| 5 | Results | 1-2 | Per-hypothesis evidence with effect sizes (Cliff's delta / Â12) and CIs, not just p-values. Visually distinct. |
| 6 | Threats to validity | 1 | Construct, internal, external, conclusion -- honest and brief. |
| 7 | Implications | 1 | The payoff. "For teams doing X, this means Y." Visually distinct. |

**Emphasize:** The design rigor and the effect sizes -- this is what justifies the claim.
**Deemphasize:** Task-material minutiae (backup slides). Statistical mechanics.
**Audience calibration:** Job market -- demonstrate you designed and ran the study rigorously. Seminar -- spend time on construct validity and design choices. Conference -- question + design + headline effect, minimal procedure detail.

---

## Survey / Case Study

**Arc:** Phenomenon --> Method --> Findings --> Implications

| # | Section | Slides | What to do |
|---|---------|--------|------------|
| 1 | Hook | 1-2 | The phenomenon or practice, and why it is not yet understood. |
| 2 | Method | 2-3 | Sampling frame + instrument (survey) or case selection + protocol (case study). Build with progressive reveal. |
| 3 | Validity | 1-2 | Instrument validation and response/non-response (survey); triangulation and member checking (case study). |
| 4 | Key finding | 1 | The most important finding, with magnitude or a representative quote. Key slide. |
| 5 | Findings | 1-2 | Additional themes/constructs. Pair each with its supporting evidence. |
| 6 | Implications | 1 | What changes for researchers and practitioners. |
| 7 | Threats to validity | 1 | Where the findings do and don't generalize. Honest. |

**Emphasize:** The method quality and the headline findings.
**Deemphasize:** Full instrument/protocol (backup slides). Exhaustive coding detail.
**Audience calibration:** Job market -- demonstrate methodological rigor AND insight. Seminar -- linger on findings and their implications. Conference -- one method, key findings, one implication.

---

## Descriptive / Measurement

**Arc:** Measurement challenge --> New approach --> What we learn

| # | Section | Slides | What to do |
|---|---------|--------|------------|
| 1 | Hook | 1-2 | Why existing measures are inadequate -- what we're missing. |
| 2 | Data innovation | 2-3 | What you built and how -- this IS the contribution. |
| 3 | Validation | 1-2 | Does the measure work? External benchmarks, known cases. |
| 4 | Key fact | 1 | The most surprising or important fact, with magnitude. Visually distinct. |
| 5 | Additional facts | 2-3 | Decompositions, patterns, correlations. |
| 6 | Implications | 1 | What changes about our understanding. What questions can now be answered. |

**Emphasize:** The data construction and validation -- this is the contribution.
**Deemphasize:** Methodology details of construction (backup slides). Standard decomposition results.
**Audience calibration:** Job market -- show the construction was careful and the measure is credible. Seminar -- emphasize the new facts and their implications. Conference -- one striking fact and why existing measures missed it.

---

## Pacing Rules (All Types)

- **Dense slides** (data, results, tables) alternate with **sparse slides** (key finding, transition, question). Never three dense slides in a row.
- **Key result slide** must be visually distinct from surrounding slides: larger font, highlighted number, or a different layout entirely.
- **Transition slides** between major sections signal where the talk is going -- a single sentence or question that bridges sections.
- For **job market talks**: the research question and contribution must be clear in the first 5 minutes, before any model or data.
- For **seminars**: you have more room to build context, but the main result should still appear by the halfway point.
- For **short/lightning**: ruthlessly cut. One result, one implication. Everything else is backup.
