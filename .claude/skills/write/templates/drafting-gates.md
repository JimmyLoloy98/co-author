# Drafting Gates -- Approval Checkpoints

Draft sections in this order, pausing for user approval at each gate.

---

## GATE 1: Introduction + Related-Work Positioning

Present to user. Wait for approval before proceeding.

User may redirect:
- Framing
- Contribution positioning
- Related-work emphasis

---

## GATE 2: Data + Study Design / Methodology

Present to user. Wait for approval.

User may adjust:
- Sample restrictions (e.g., bot filtering, project selection criteria)
- Metric definitions
- Design and analysis details

---

## GATE 3: Results + Discussion + Threats to Validity + Conclusion

**Hard prerequisite:** Requires actual output files (see Artifact Prerequisites in the agent).
- `paper/tables/` must contain at least one `.tex` file with actual numbers
- `paper/figures/` must contain at least one `.pdf` or `.png` figure

Present to user. Wait for approval.

---

## Application Rules

- **Single-section drafts:** The gate for that section applies.
- **Full drafts (`/write full`):** All three gates apply in sequence.
- **BLOCKED items:** Results/Conclusion cannot be drafted without output files.
- **VERIFY items:** Citations that need user confirmation.
- **VOICE items:** Style guide not yet extracted (drafting blocked until resolved).
