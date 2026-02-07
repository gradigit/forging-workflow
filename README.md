# Forging Workflow

Claude Code skills that take a rough idea and turn it into an implementation-ready prompt and plan. Instead of jumping straight into code from a vague description, these skills walk you through questions, research, and adversarial review first.

## Skills

### forging-plans

Full prompt-to-plan pipeline for **new projects**. Two modes:

- **Mode 1 (Prompt Perfection)** — Runs in normal mode. Takes your rough idea through a questionnaire, prior-art research, gap analysis, and adversarial review. Writes the perfected prompt to `architect/prompt.md`.
- **Mode 2 (Plan Iteration)** — Runs in plan mode. Takes Claude's draft plan and pressure-tests it with structured review and research. Writes the finalized plan to `architect/plan.md`.

Mode 1 also generates `CLAUDE.md` and `TODO.md` so execution state survives context clears.

**Triggers**: `forge a plan`, `perfect this prompt`, `flesh out this idea`, `/forging-plans`

### forging-prompts

Same pipeline as forging-plans Mode 1, but for **adding features to existing projects**. Designed for forked sessions where project context is already loaded — you fork your working session, forge the prompt, then go back and use it.

**Triggers**: `forge a prompt`, `sharpen this prompt`, `flesh out this feature`, `/forging-prompts`

### forging-shared

Reference files used by both skills:

| File | Purpose |
|------|---------|
| `questionnaire.md` | Adaptive questionnaire with 12 categories. 1-3 always asked; 4-12 assessed for relevance |
| `research.md` | Prior-art research protocol — official docs first, 2-year recency filter, cross-referencing |
| `challenge.md` | Adversarial review — self-critique (Phase A) then sub-agent review (Phase B) |

## Depth modes

Both skills support quick and full modes:

- **Quick** — Core questions (categories 1-3), no research, self-critique only, one iteration. Good for small features you already understand well.
- **Full** (default) — Full questionnaire, prior-art research, sub-agent adversarial review. Use this for anything complex or ambiguous.

## Installation

```bash
git clone https://github.com/gradigit/forging-workflow.git

# All three directories are required (plans and prompts reference shared)
cp -r forging-workflow/.claude/skills/forging-plans ~/.claude/skills/
cp -r forging-workflow/.claude/skills/forging-prompts ~/.claude/skills/
cp -r forging-workflow/.claude/skills/forging-shared ~/.claude/skills/
```

## Typical workflow

### New project (forging-plans)

```
1. Start Claude Code
2. Describe your idea: "I need a CLI tool that converts markdown to slides"
3. Invoke /forging-plans — goes through Q&A, research, challenge
4. Output: architect/prompt.md, CLAUDE.md, TODO.md
5. /clear → enter plan mode → paste prompt → Claude drafts plan
6. Invoke /forging-plans again for Mode 2 (plan iteration)
7. Approve plan → /clear → execution begins
```

### Mid-project feature (forging-prompts)

```
1. Working in Session A on your project
2. Fork: claude --continue --fork-session
3. In forked session: "I want to add theme support"
4. Invoke /forging-prompts — goes through Q&A, research, challenge
5. Output: architect/prompt.md
6. Switch back to Session A
7. "Read architect/prompt.md and implement this"
```

## Dependencies

- **Claude Code** with plan mode support
- **study skill** (optional, from [claude-skills](https://github.com/gradigit/claude-skills)) — improves the research phase

## License

MIT
