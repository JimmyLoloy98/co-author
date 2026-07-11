---
name: write
description: Draft academic paper sections using paragraph-level argument moves. Cleanup pass strips AI patterns after drafting. Replaces /draft-paper and /humanizer.
argument-hint: "[section or mode: intro | design | results | threats | conclusion | abstract | full | humanize | style-guide] [file path (optional)]"
allowed-tools: Read,Grep,Glob,Write,Edit,Task
---

# Write

Draft paper sections, apply a cleanup pass, or extract a personal style guide from prior papers by dispatching the **Writer** agent.

**Input:** `$ARGUMENTS` — section name or mode, optionally followed by file path.

> **Language policy:** System files, prompts, and templates stay in English, but the paper/thesis **prose is drafted in Spanish** (per the Language Policy in `CLAUDE.md`; `polyglossia` + XeLaTeX). LaTeX structure, `\label{}`s, file names, code identifiers, and bibliography keys stay in English.

---

## Modes

### `/write [section]` — Draft Paper Section
Draft a specific section: `intro`, `design`, `results`, `threats`, `conclusion`, `abstract`, or `full`.

**Agent:** Writer
**Output:** LaTeX section file in paper/sections/

Workflow:

#### 1. Context Gathering

Before drafting, read all available context:
1. Read existing paper draft in `paper/` (if it exists)
2. Read `master_supporting_docs/` for notes, outlines, research specs
3. Read most recent `quality_reports/research_spec_*.md` or `quality_reports/lit_review_*.md`
4. Read `.claude/references/domain-profile.md` for field conventions
5. Check `Bibliography_base.bib` for available citations
6. Scan `paper/tables/` and `paper/figures/` for generated output
7. Read `quality_reports/results_summary.md` if it exists (from Coder)

#### 2. Paper Type Detection

Before routing, identify the paper type from the strategy memo or existing draft:
- **Causal mining study** — DiD, IV, RDD, event study on repository/telemetry data
- **Controlled experiment** — Human-subjects or tool experiment with tasks and treatments
- **Survey** — Instrument, sampling frame, response analysis
- **Case study** — Industrial or project context, protocol, qualitative themes
- **Methods paper** — New metric, technique, or analysis method with validation
- **Descriptive / exploratory** — New dataset, new measure, empirical characterization

This determines which section templates the Writer uses.

#### 3. Section Routing

Based on `$ARGUMENTS`:
- **`full`**: Draft all sections in sequence, pausing between major sections for user feedback
- **`intro`**: Draft introduction (most common request)
- **`design`**: Draft the Study Design / Methodology section — RQs, data, metrics, analysis procedure; causal design detail (DiD/IV/RDD/event study) for causal mining studies; participants/tasks/treatments for experiments; instrument/sampling for surveys; context/protocol for case studies
- **`results`**: Draft results organized by RQ — narration style depends on paper type and output type (regression tables, event-study figures, experiment results by task/treatment, survey results by construct, qualitative themes)
- **`discussion`**: Draft discussion — implications for practitioners and researchers
- **`threats`**: Draft Threats to Validity (construct, internal, external, conclusion)
- **`conclusion`**: Draft conclusion with type-appropriate ending (implications for practice, research agenda)
- **`abstract`**: Draft abstract (must have other sections first)
- **`data`**: Draft data/data-collection section — expanded for descriptive/exploratory papers
- **No argument**: Ask user which section to draft

#### 4. Dispatch Writer

Dispatch Writer with paper type and argument-move templates for the target section. The writer drafts using paragraph types (motivation, result statement, mechanism, etc.), applies design-specific moves, then runs the cleanup pass. Prose is drafted in Spanish; LaTeX structure, labels, and citation keys stay English. Save to `paper/sections/[section].tex`.

#### 5. Quality Self-Check

Before presenting the draft:
- [ ] Paper type identified and correct template used
- [ ] Every paragraph has an identifiable purpose (argument move type)
- [ ] Findings lead sentences — not buried after setup
- [ ] Design-specific elements present (see writer.md for checklists per design)
- [ ] Every RQ stated in the introduction/design is answered in Results (and vice versa)
- [ ] Threats to Validity covers construct, internal, external, and conclusion validity
- [ ] Every displayed equation is numbered (`\label{eq:...}`)
- [ ] All `\cite{}` keys exist in `Bibliography_base.bib`
- [ ] Introduction contribution paragraph names specific papers
- [ ] Effect sizes stated with units (and Cliff's delta / Vargha-Delaney Â12 with CIs where group comparisons are reported)
- [ ] No banned hedging phrases
- [ ] Notation consistent throughout
- [ ] All tables/figures referenced actually exist in `paper/tables/` or `paper/figures/`
- [ ] Results narrated correctly for output type (regression tables, event-study figures, experiment tasks, survey constructs, qualitative themes)
- [ ] Prose in Spanish; structure, labels, file names, and bib keys in English
- [ ] Personal style guide loaded (not template) — or user prompted to run `/write style-guide`
- [ ] Claim-source map produced for all numerical claims (`quality_reports/claim_source_map_{project}.md`)
- [ ] Results/Conclusion only drafted after verifying actual output files exist

#### 6. Present to User

Present sections through drafting gates, pausing for approval at each:

**GATE 1:** Introduction + Related-work positioning → present, wait for approval
**GATE 2:** Data + Study Design / Methodology → present, wait for approval
**GATE 3:** Results + Discussion + Threats to Validity + Conclusion → present, wait for approval

For single-section drafts, present the section directly. For `full`, use all three gates.

Flag items that need attention:
- **BLOCKED items:** Results/Conclusion cannot be drafted without output files
- **VERIFY items:** Citations that need user confirmation
- **VOICE items:** Style guide not yet extracted (drafting blocked until resolved)

### `/write style-guide [paper-dir]` — Extract Personal Voice

One-shot extraction of the user's writing voice from their published or drafted papers. Produces `.claude/references/personal-style-guide.md`, which the writer auto-loads on every subsequent invocation.

**When to run:**
- Once at the start of a project, after pointing at a directory of the user's prior papers
- After publishing a new paper that shifts voice (re-run to refresh the profile)

**Input:** `$ARGUMENTS` — path to a directory containing prior papers (.tex or .pdf). If omitted, defaults to `master_supporting_docs/` and scans for .tex/.pdf files.

**Agent:** Writer (style-extraction mode)
**Output:** `.claude/references/personal-style-guide.md`

Workflow:
1. **Discover corpus.** List .tex and .pdf files in the target directory. If fewer than 2 papers found, flag and ask before proceeding (style extraction on a single paper overfits).
2. **Sample strategically.** For each paper, extract:
   - The full introduction
   - The first two paragraphs of each major section
   - The abstract and conclusion
   - A random sample of 5–10 results-section paragraphs
   This keeps context usage bounded while capturing voice variation across sections.
3. **Extract patterns.** The Writer (in style-extraction mode) produces quantitative and qualitative patterns:
   - Sentence-length distribution (median, 10th–90th pct)
   - Passive-voice frequency, first-person-plural frequency, em dash rate
   - Paragraph opening and closing moves
   - Section-architecture patterns (how introductions open, how results lead)
   - Lexicon: words used repeatedly, words demonstrably avoided
   - Hedging and comparison patterns
   - Citation conventions (textual vs. parenthetical split; papers-per-claim)
   - Tone markers and anti-patterns already stripped
4. **Write to `.claude/references/personal-style-guide.md`.** Fill every template section with quoted examples from the corpus. Never invent patterns — if a section has no evidence, write "[insufficient corpus evidence]".
5. **Present summary.** One-paragraph recap of the voice profile: sentence length, passive rate, signature lexicon, distinguishing tone markers. User confirms before the guide takes effect on subsequent `/write` calls.

Principles for the extraction:
- **Ground every claim in the corpus.** Each pattern must have at least one quoted example.
- **Extract, don't prescribe.** The guide records the author's observed behavior, not what the Writer thinks is good style.
- **Don't duplicate `domain-profile.md`.** The style guide is about voice; the domain profile is about field conventions.
- **Don't override working-paper-format invariants.** Voice doesn't trump INV-1..21.

### `/write humanize [file]` — Cleanup Pass Only
Strip AI writing patterns from existing text without rewriting content.

**Agent:** Writer (cleanup mode)
**Output:** Edited file with AI patterns removed

Strips 24 patterns across 4 categories:
- Structural: forced narrative arcs, artificial progression
- Lexical: "delve, leverage, nuanced, robust"
- Rhetorical: rule-of-three, negative parallelisms, em dash overuse
- Formatting: excessive bullet points, promotional language

---

## Section Standards

**All paper types share the same backbone. Moves diverge by type — see writer.md for full templates.**

Standard section order: Introduction; Background/Related Work; Study Design/Methodology (RQs, data, metrics, analysis); Results (by RQ); Discussion (implications for research and practice); Threats to Validity; Conclusion.

| Section | Length | Causal Mining | Experiment | Survey | Case Study | Descriptive |
|---------|--------|---------------|-----------|--------|-----------|-------------|
| Introduction | 1000-1500 | ...design preview → result → contribution | ...experiment preview → effect → contribution | ...instrument preview → key finding → contribution | ...context preview → themes → contribution | ...data innovation → key fact → contribution |
| Data | 800-1200 | Treatment, outcome metrics, controls | Participants, tasks, materials | Sampling frame, respondents | Case context, data sources | 1200-1800 (core contribution) |
| Study Design | 800-1500 | Design-specific (DiD/IV/RDD/ES) + RQs | Treatments → tasks → procedure → hypotheses | Instrument → sampling → analysis plan | Protocol → data collection → coding | Merged into Data + RQs |
| Results | 800-1500 | By RQ: main spec → robustness → heterogeneity | By hypothesis/task/treatment | By construct/RQ | Themes with triangulated evidence | Key facts → decompositions → implications |
| Discussion | 500-900 | Implications for teams and researchers | Implications for tool/practice adoption | What practitioners should do | Transferability to other contexts | What the facts change |
| Threats to Validity | 400-800 | Construct, internal, external, conclusion | Same | Same | Same | Same |
| Conclusion | 300-500 | Answer to RQs + takeaway | Same | Same | Same | Same |
| Abstract | 100-150 | Question, design, finding with magnitude | Question, experiment, effect | Question, sample, key finding | Question, case, insight | Question, measurement, key fact |

---

## LaTeX Conventions

- `\citet{}` for textual citations ("Smith (2024) shows...")
- `\citep{}` for parenthetical citations ("...is well documented (Smith, 2024)")
- `booktabs` rules (`\toprule`, `\midrule`, `\bottomrule`) — never `\hline`
- Notation protocol (causal designs): `Y_{it}`, `D_{it}`, `\gamma_i`, `\delta_t`, `\varepsilon_{it}`; research questions numbered RQ1, RQ2, ...

---

## Bundled Resources (Level 3)

Loaded on demand by the writer agent:

| Resource | Path | When |
|----------|------|------|
| Section templates | `templates/section-templates.md` | Always -- defines section structure |
| Paragraph moves | `templates/paragraph-moves.md` | Always -- defines argument types |
| Cleanup patterns | `templates/cleanup-patterns.md` | After drafting -- cleanup pass |
| Style extraction | `templates/style-extraction-protocol.md` | `/write style-guide` mode |
| Drafting gates | `templates/drafting-gates.md` | Full draft mode |
| Claim-source map | `templates/claim-source-map.md` | After results section |
| Notation protocol | `references/notation-protocol.md` | Design + results sections |

See also: `gotchas.md` for known failure points and edge cases.

---

## Principles
- **This is the user's paper, not Claude's.** Match their voice and style.
- **Never fabricate results.** Use TBD placeholders.
- **Citations must be verifiable.** Only cite confirmed papers.
- **Argument moves first, cleanup second.** Draft with structure, then strip AI patterns.
- **Spanish prose, English scaffolding.** Draft deliverable text in Spanish; keep labels, file names, identifiers, and bib keys in English.
