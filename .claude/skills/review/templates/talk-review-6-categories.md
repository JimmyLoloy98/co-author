# Talk Review: 6 Check Categories

Extracted from `storyteller-critic.md`. Used by the storyteller-critic agent for presentation review.

---

## Prerequisite Checks

**Before running categories:**

- Read `.claude/rules/content-invariants.md` -- enforce INV-20 and INV-21. Cite invariant numbers (e.g., "violates INV-20") in report alongside deductions.
- Identify the paper type (causal mining, experiment, survey/case study, descriptive). This determines which narrative arc checks apply.

---

## 1. Narrative Flow

- Does the hook work? (first 2 slides)
- Is there a clear story arc?
- Does the audience know "so what" by the end?
- Is the key slide clearly identifiable?

**Paper-type-specific arc checks:**

| Paper Type | The talk must... |
|-----------|-----------------|
| Causal (mining) | Lead with the engineering question, show the variation/quasi-experiment, present the main result with effect size |
| Experiment | Motivate the practice tested, describe the design (tasks, treatments), present effect sizes as the payoff |
| Survey / Case study | Establish the phenomenon, describe the method and its validity, present the key findings |
| Descriptive | Lead with what's missing in current measures, present the data/metric innovation, show the most surprising fact |

---

## 2. Visual Quality

- Text overflow on any slide?
- Font sizes readable for projection (>= 10pt)?
- Tables readable (not too many columns/rows)?
- Figures at appropriate size with clear labels?
- Consistent formatting throughout?
- One idea per slide? (flag slides trying to do two things)

---

## 3. Content Fidelity

- Do numbers on slides match the paper exactly?
- Is the study design correctly represented?
- Are robustness results accurately summarized?
- No results that aren't in the paper?

**Experiment talks additionally:**
- Are effect sizes (Cliff's delta / Â12) on slides interpreted for practical significance, not just reported?
- Is the task/treatment design shown clearly?
- Are threats to validity acknowledged?

**Theory+empirics additionally:**
- Are predictions stated before evidence?
- Is the distinguishing prediction clearly flagged?

---

## 4. Scope for Format

- Is the talk the right length for the format?
- Is the content depth appropriate? (job market != lightning)
- Are the right things cut for shorter formats?
- Backup slides available for anticipated questions?

**What to cut by paper type (shorter formats):**

| Paper Type | Keep | Cut |
|-----------|------|-----|
| Causal (mining) | Main result + one robustness | Extra robustness, heterogeneity details |
| Experiment | Effect size + task design | Procedure minutiae, per-participant detail (move to backup) |
| Theory+empirics | Distinguishing prediction + test | Other predictions, model derivation (move to backup) |
| Descriptive | Most surprising fact + validation | Construction details, decompositions |

---

## 5. Compilation

- **Beamer:** Does it compile without errors? No overfull hbox warnings?
- **Quarto:** Does `quarto render` produce clean HTML? No missing references?
- All referenced figures/tables exist?

---

## 6. Paper-Type Coherence

- Does the narrative arc match the paper type?
- Experiment talk without a clear task/treatment description? Flag it -- the audience can't judge the result without knowing what participants did.
- Theory talk without the distinguishing prediction? Flag it -- the audience needs to know what's unique.
- Descriptive talk that makes causal claims? Flag it -- the paper doesn't have a design for that.

---

## Report Format

```markdown
# Talk Review -- [Format]
**Date:** [YYYY-MM-DD]
**Reviewer:** storyteller-critic
**Paper type:** [Causal (mining) / Experiment / Survey / Case study / Descriptive]
**Score:** [XX/100] (advisory)

## Narrative Arc: [Correct for type / Wrong arc]
## Issues Found
[Per-issue with severity and deduction]

## Score Breakdown
- Starting: 100
- [Deductions]
- **Final: XX/100**
```
