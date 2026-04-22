# skills-for-learning

Four Claude Code skills that cover the full learning loop — understand what exists, and learn what's new.

| Skill                                          | What it does                                                                                                                                                                                                                           |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`explain`](./skills/explain/SKILL.md)         | Structures any explanation into three sections: **High-Level**, **Break-Down**, **Glossary**. Use for "what is…", "how does…work", "ELI5", or "walk me through…" questions.                                                            |
| [`walkthrough`](./skills/walkthrough/SKILL.md) | Writes a revealing architectural walkthrough — a document that re-creates the experience of making a design decision instead of summarising it. Built for junior engineers reading about a PR, refactor, or architecture.              |
| [`tutorial`](./skills/tutorial/SKILL.md)       | Generates a self-contained learning repository for any software topic — theory with Mermaid diagrams plus a practice project with milestone-based TODOs the learner builds commit-by-commit.                                           |
| [`guide`](./skills/guide/SKILL.md)             | Coaches the learner through a `tutorial` repo. Acts as a patient senior-engineer mentor — asks questions, escalates hints, tracks weak points. Does NOT give answers unless explicitly asked.                                          |

`explain` and `walkthrough` help you understand code that already exists. `tutorial` and `guide` pair up to help you learn something new — one generates the material, the other coaches you through it without spoiling the answer.

## Install

### Claude Code plugin marketplace (recommended)

Inside Claude Code:

```
/plugin marketplace add iannbing/skills-for-learning
/plugin install skills-for-learning@skills-for-learning
```

After install, the skills are available as `/skills-for-learning:explain`, `/skills-for-learning:walkthrough`, `/skills-for-learning:tutorial`, and `/skills-for-learning:guide`. They're also model-invoked — Claude will use them automatically when the request matches.

Update later with:

```
/plugin marketplace update skills-for-learning
```

### skills.sh

```
npx skills add iannbing/skills-for-learning/explain
npx skills add iannbing/skills-for-learning/walkthrough
npx skills add iannbing/skills-for-learning/tutorial
npx skills add iannbing/skills-for-learning/guide
```

### Manual / local install

Clone the repo and either:

- load as a plugin during development: `claude --plugin-dir ./skills-for-learning`
- or copy individual skills into your user skills dir:

  ```bash
  mkdir -p ~/.claude/skills
  cp -r skills/explain      ~/.claude/skills/
  cp -r skills/walkthrough  ~/.claude/skills/
  cp -r skills/tutorial     ~/.claude/skills/
  cp -r skills/guide        ~/.claude/skills/
  ```

  Installed this way the skills have no namespace prefix — they're just `/explain`, `/walkthrough`, `/tutorial`, and `/guide`.

## Usage

### `explain`

```
/skills-for-learning:explain how does OAuth 2.0 authorization code flow work
```

Produces:

1. **High-Level** — 3–7 bullets plus an ASCII flow diagram.
2. **Break-Down** — components, lifecycles, sequence diagrams. Real code snippets when explaining code.
3. **Glossary** — domain-specific terms only (no `API`, `HTTP`, `DNS` filler).

Calibrated for a junior engineer — the glossary skips general-computing basics and covers only terms specific to the topic.

### `walkthrough`

```
/skills-for-learning:walkthrough PR #482
/skills-for-learning:walkthrough src/auth/session.ts
/skills-for-learning:walkthrough why we moved from Redux to Zustand
```

Produces a markdown document at `<git-repo-root>/personal/walkthrough-<slug>.md` that:

- drops the reader into the codebase as a new engineer would find it
- names the problems precisely before proposing solutions
- walks through 3–4 realistic alternatives and why each fails with a _specific_ technical reason
- ends with a graded **Final Verdict** (A+ / A / B / C / D / F) with evidence-backed suggestions

Add `personal/walkthrough-*.md` to your `.gitignore` — these are personal study notes.

### `tutorial`

Run inside an empty directory where you want the learning repo to live.

```
/skills-for-learning:tutorial GraphQL
/skills-for-learning:tutorial teach me Kubernetes, intermediate level
/skills-for-learning:tutorial React Testing Library
```

Generates `learn-<topic>/` containing:

- `theory/` — three markdown files: core concepts (with Mermaid diagrams), controversies & gotchas, and a 10-question expert quiz
- `practice/<project>/` — a minimal scaffold plus a milestone-based README with ordered TODOs you check off as you build
- `.guide/learner-profile.md` — tracks weak points across sessions; feeds follow-up tutorials

If a `learner-profile.md` already exists when you generate a new tutorial, the next tutorial will target your recorded weak points.

### `guide`

Used _inside_ a tutorial repo, when you start working on a practice milestone.

```
/skills-for-learning:guide I'm starting milestone 2
/skills-for-learning:guide I'm stuck on step 3.1
/skills-for-learning:guide here's my code, what do you think? [paste]
```

Coaching behaviour:

- **You drive.** The skill does not write code for you unless you explicitly say "show me".
- **Escalating hints** — reframe → narrow → point → demonstrate → explain. You never hit a dead end, but you also don't get spoonfed.
- **Logs every struggle** and conceptual mistake to `.guide/learner-profile.md`, so follow-up tutorials from the `tutorial` skill can target your weak spots.
- **Marks steps done** in the practice README as you complete them.

## Why these four?

Two complementary loops:

- **Understand what exists** — `explain` decomposes any concept into a reliably-structured form; `walkthrough` teaches the _why_ behind a specific design decision.
- **Learn what's new** — `tutorial` generates a learning repo from scratch; `guide` coaches you through the practice without giving away the answer.

Each skill encodes the shape of its output so Claude produces consistent, well-organised results every time instead of drifting into walls of text.

## License

MIT — see [LICENSE](./LICENSE).

## Contributing

Issues and PRs welcome at <https://github.com/iannbing/skills-for-learning>.
