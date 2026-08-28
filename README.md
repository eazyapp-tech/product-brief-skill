# brief — Product Brief skill

A reusable Claude Code **skill** for writing a Product Brief: PM intent before design, before the PRD. One opinionated person's bet, in plain language, no AI slop.

> A brief is not a PRD. It sits upstream. It lands the WHY so the WHAT and the HOW have something to stand on.

## What it does

Routes on "brief", "product brief", "operator brief", "PM brief", `/brief`, or any "I want to land what we're building before designing". Then it enforces three things most briefs get wrong:

1. **Plain language, tested.** Every word has to pass the reconstruction test: can the reader rebuild this from concepts they already own, without your dictionary? Simplifying is decompression toward shared words, not dumbing down. `references/vocab-stop-list.md` is that test's common outputs.
2. **The right altitude.** A brief that drifts into build detail stops being a brief. `references/altitude-check.md` and `references/build-sheet.md` name the ladder and where each document sits on it.
3. **Critique before delivery.** A multi-lens pass (`references/critique-lenses.md`) run as sub-agents, so the brief gets argued with before a human has to.

## Install

**As a personal skill** (per user):
```bash
cp -R brief ~/.claude/skills/
```
Restart Claude Code (or start a new session). It appears in the skill list and auto-routes on the triggers above.

**As a project skill** (shared via a repo):
```bash
cp -R brief <your-project>/.claude/skills/
```

## Files

| File | What |
|---|---|
| `brief/SKILL.md` | The skill — the writing rules, the pipeline, the register. |
| `brief/references/template.md` | Section-by-section brief template, distilled from a real shipped brief. |
| `brief/references/altitude-check.md` | Is this sentence at brief altitude, or has it slid into build detail? |
| `brief/references/build-sheet.md` | The document-altitude ladder: brief → spec → build sheet, and what belongs where. |
| `brief/references/critique-lenses.md` | The lenses for the adversarial sub-agent pass before delivery. |
| `brief/references/synthesis-patterns.md` | Patterns for turning a pile of input into one claim. |
| `brief/references/vocab-stop-list.md` | Banned words and their plain replacements. |
| `brief/learnings.md` | Living file — corrections land here and change future runs. |

## Adapting it

The core is system-agnostic. RentOk specifics appear in `SKILL.md`, `learnings.md`, and a few reference examples (personas, doc locations, register rules). Swap those for your own team's vocabulary and doc conventions; the writing tests and the altitude ladder carry over unchanged.

## Related skills

- `feature-map` — the altitude above: decides what exists in a product area and why, before any one item gets a brief.
- `feature-design-pipeline` — the altitude below: takes a brief to engineer-ready design docs.
- `screen-handoff-pipeline` — for a design that already exists and needs a code-free developer handoff.
