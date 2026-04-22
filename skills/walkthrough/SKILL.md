---
name: walkthrough
description: Write a revealing architectural walkthrough as educational material — for a PR, design decision, code structure, or feature. Use when the user wants to create learning material for junior engineers, explain *why* decisions were made, or document the reasoning behind an architecture. Trigger on "walkthrough", "write a walkthrough", "educational material", "explain the decision", "why was this designed this way", or when asked to produce a document that teaches through re-experiencing a decision rather than summarising it.
---

# Walkthrough Skill

Produce a revealing, educational walkthrough document written for junior engineers. The document must re-create the experience of _making the decision_, not narrate it from a god-view perspective.

**Input**: A PR number, file path, feature name, design decision, or architectural concept. Read the actual code before writing — never write from PR descriptions alone.

**Output**: A well-structured markdown document saved to `<git-repo-root>/personal/walkthrough-<subject-slug>.md`. These are personal study notes — add the pattern `walkthrough-*.md` to `.gitignore` if it isn't there already.

## Approach: Revealing, Not Summarising

The key distinction: a summary tells you what was decided. A walkthrough puts you in the room when the decision is being made.

- Start from **the problem as it would be discovered** — open files, run greps, read models, hit the confusion.
- **Name problems precisely** before proposing solutions. Vague problems produce vague solutions.
- **Walk through alternatives** and explain concretely why each one fails — not vaguely, but with the specific line of code, constraint, or invariant that breaks down.
- **Explain each design decision as a choice** with a reason. "We used X" is a summary. "We needed X because Y would have caused Z" is a walkthrough.
- **End with a Final Verdict** (see structure below) that grades the design and grounds the grade in evidence.

## Document Structure

The document must follow this part structure. Scale the number of parts to the complexity of the subject — a small decision might need 4 parts, a large architectural change might need 8. Aim for ~500 words for a focused decision, ~2000+ words for a major architectural change. Let the subject dictate the length, not a formula.

### Required parts (always present):

**Opening — You Inherit [the Situation]**
Drop the reader into the codebase as a new engineer. Show the exact files, components, or interfaces they'd encounter first. Use grep results, code snippets, and definitions directly from the codebase. End on a moment of confusion or an unanswered question.

**Closing trio (always the last three parts, in this order):**

**Name the Problems**
Before solutions, enumerate the problems precisely. Each problem gets a name and a specific description grounded in actual code. Do not accept vague problem statements.

**Consider the Alternatives**
Walk through 3–4 realistic alternatives in the order an engineer would naturally consider them. For each:

- Describe what the approach would look like concretely (show code or schema sketches)
- Explain why it fails with a specific technical reason, not a hand-wave
- Label the chosen approach last as "Alternative [letter] (chosen)"

**What You Should Take Away**
3–5 explicit, generalizable lessons. Each lesson must state a principle and ground it in a specific example from the code. Not generic advice — lessons the reader could only have learned from _this_ walkthrough.

### Middle parts (between the opening and the closing trio — include as needed):

These fill the gap between discovery and verdict. Pick whichever apply to the subject:

- **Design the Data Model** — walk through modelling decisions (table structure, field choices, constraints, indexes) as named decisions with reasons
- **Design the [Component Tree / State Shape / API Surface]** — same treatment for frontend architecture, API design, or any structural decision
- **Design the [Pipeline / Service / Infrastructure]** — same treatment for backend or infra architecture
- **Integration Without a Big Bang** — if the change involves a migration or rollout strategy, explain the phased approach step by step
- **[The Specific Extraction / Refactor]** — for any significant code restructuring, dedicate a part to why and how

A small decision might have zero middle parts (opening → closing trio). A major architectural change might have 3–4.

### Final Verdict (always last before Appendix):

```markdown
## Final Verdict

> **[Grade]: [Label]**

### Why

[3–5 bullet points grounding the verdict in specific things from the code, not generalities]

### Suggestions (not blockers) ← include only for grades B or above with gaps

[Numbered list. Each suggestion must: name the specific gap, quote or reference the specific code involved, explain what could go wrong if left unaddressed, and suggest the concrete fix or follow-up action]
```

Grade scale:

- **A+** — Exemplary. Correct design, no meaningful gaps, implementation is clean and instructive.
- **A** — Solid. Right direction, sound execution, minor style or completeness gaps only.
- **B** — Acceptable. Right direction, but has 1–4 specific gaps that could cause future pain. Include suggestions.
- **C** — Needs work. Functional but with significant design or implementation problems that should be addressed.
- **D / F** — Fundamental flaw. The approach cannot be patched with suggestions; it requires rethinking. Explain exactly what the flaw is and what direction a rethink should take.

### Appendix (optional):

Include a dependency graph, entity-relationship diagram, or before/after comparison when it materially aids understanding. ASCII diagrams only.

## Before Writing

1. **Read the actual code.** Fetch the PR or explore the relevant files directly — don't rely on PR descriptions or summaries. Trace the code the way a senior engineer would: follow the entry points, find where data is defined and transformed, read the tests to understand intended behaviour. The right starting point depends on the stack — for a backend change it might be the data model; for a frontend change it might be the component tree or state shape; for an API change it might be the route handlers. Use judgement.
2. **Understand the context.** If the subject is a change (PR, refactor, migration), find what existed before — the walkthrough cannot explain _why_ something was changed without showing what it replaced. If the subject is an existing architecture or feature, trace the historical reasoning: read commit messages, comments, and surrounding code to reconstruct _why_ it was built this way.
3. **Identify the real audience level.** Default to junior engineer unless the user specifies otherwise. Calibrate explanation depth accordingly — a junior frontend engineer needs different context than a junior backend engineer, but both need the _why_ made explicit.
4. **Find the sharpest contrast.** The best walkthroughs pivot on one key insight — a naming mistake, a hidden coupling, a constraint that looks optional but isn't. Find it before writing.

## Writing Rules

- **Use real code, not pseudocode.** Snippets must come from the actual codebase. Trim to the relevant 5–15 lines.
- **Label every code snippet.** `# before`, `# after`, `# BROKEN`, `# CORRECT` — the label is part of the teaching.
- **Name your problems and decisions.** "Problem 1: user state lives in three places for no reason." "Decision 3: single source of truth in the store." Named sections are easier to reference and remember.
- **Show the trap before the fix.** For every non-obvious decision, show what the naive approach looks like and what breaks. Then show the fix.
- **Ground every lesson in evidence.** Don't say "always validate inputs." Say "the crash was exactly one missing null check in `processOrder()` — a guard you didn't write because you assumed the caller would 'of course' pass a valid object."
- **Final Verdict suggestions must be specific.** Each suggestion names the exact file/model/field involved, explains what could go wrong, and states the concrete follow-up action.

## Style

- Write in second person ("you open the file", "you grep for", "you notice") for the discovery sections.
- Switch to declarative ("the fix is", "the design principle is") for decision explanations.
- No walls of text. Every paragraph that runs more than 5 lines should become a list or a code block.
- Tables for comparisons. ASCII diagrams for flows and dependencies. Code blocks for everything from the codebase.
