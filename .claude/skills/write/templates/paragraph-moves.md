# Paragraph Argument Moves

Every paragraph has one job. Before writing a paragraph, identify its type. Then follow its structure.

---

## The 7 Paragraph Types

| Type | Structure | What It Does |
|------|-----------|-------------|
| **Motivation** | Fact or puzzle --> why it matters --> what we don't know | Opens a section or subsection. Establishes the gap. |
| **Design preview** | We use [design] + [data] to answer [RQ]; the key threat is [X]. We address it by [Y]. | Tells the reader the study design before the formalism. |
| **Result statement** | Finding with magnitude + units --> comparison to prior estimates --> practical significance | Lead with the number, not the table reference. |
| **Literature positioning** | What [Author, Year] found --> how we differ --> what our contribution adds | Citations are surgical -- position the paper, don't pad the bibliography. |
| **Mechanism** | The effect operates through [channel]. We show this by [test]. Alternative [X] ruled out by [Y]. | Explains *why*, not just *that*. |
| **Robustness narration** | Core result survives [checks]. Main threat: [X]; Table N addresses this by [approach]. | Brief. Don't re-argue the result -- confirm it holds. |
| **Qualification** | May not generalize to [context] because [reason]. | Short. One paragraph maximum. |

---

## Sentence-Level Principles

- **Lead with the finding, not the setup.** "CI adoption reduces median review latency by 6.1 hours" -- not "In order to investigate whether CI adoption might affect review latency, we..."
- **Active voice, concrete subjects.** "The policy change increased build success rates" -- not "An increase in build success rates was observed"
- **Vary sentence length.** Short sentences for key findings. Longer sentences for nuance and qualifications.
- **One claim per sentence.** If a sentence has two claims, split it.
- **No announcements.** Delete any sentence whose only job is to say what comes next ("In the next section, we will discuss...").
- **Citations are evidence, not filler.** Cite when you're building on specific work. Don't cite to prove you've read the literature.

---

## Effect Size Conventions

- Always report with units: "CI adoption reduces median review latency by 6.1 hours", "defect density falls by 0.4 defects per KLOC"
- For group comparisons, report standardized effect sizes with CIs: "Cliff's delta = 0.38, 95% CI [0.21, 0.53]" or Vargha-Delaney $\hat{A}_{12}$
- Never: "the coefficient is significant" or "the difference is statistically significant" without magnitude
