# Meta-Governance: This Repository's Dual Nature

This repository is both a working project and a public template for empirical software engineering research. It can be adapted to adjacent fields (HCI, CSCW, information systems) by customizing the domain profile and venue profiles.

## Working Project
- We develop research papers, seminars, guides, and documentation
- We accumulate project-specific learnings and institutional context
- We test and iterate on the architecture itself

## Public Template
- Others fork this repo to run their own research workflows
- They share the same pipeline (design → analyze → write → submit) and tools (LaTeX, Python/R, Beamer)
- Field-specific differences (journals, methods, conventions) are handled by `.claude/references/domain-profile.md` and `.claude/references/journal-profiles.md`

## The One Rule

Before committing, ask: **would another empirical researcher forking this repo benefit from this?**

- **Yes** → commit (workflow patterns, skills, agents, rules, templates)
- **No** → keep local in `.claude/state/` (machine paths, tool versions, institutional requirements, API keys)

## Learning Promotion

When a learning has been validated across 3+ projects (confirmed by user):

| Pattern Type | Promotion Target | Requires |
|-------------|-----------------|----------|
| PATTERN (replicable success) | New best-practice in relevant agent's protocol | User approval |
| FRICTION (recurring failure) | Agent prompt revision or rubric adjustment | User approval |
| HIGH-PERF (consistent excellence) | New content invariant (INV-XX) or rule addition | User approval |

**Protocol:**
1. The orchestrator identifies a pattern recurring across 3+ traces
2. The orchestrator drafts the specific change (new invariant text, agent prompt addition, or rubric adjustment)
3. The user reviews and approves or rejects
4. If approved, the change is committed to the template repo

**Constraint:** Promotion ALWAYS requires user approval. The system suggests; the user decides. No autonomous self-modification of rules, invariants, or agent prompts.
