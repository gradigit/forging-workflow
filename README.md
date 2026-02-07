# Forging Workflow

Claude Code agent skills for transforming rough ideas into implementation-ready prompts and plans. A structured, multi-phase pipeline with adaptive questioning, mandatory research, and adversarial challenge phases.

## Skills

### forging-plans

Full prompt-to-plan pipeline for **new projects**. Operates in two modes:

- **Mode 1 (Prompt Perfection)** — Runs in normal mode. Takes a rough idea through adaptive questionnaire, prior-art research, gap analysis, self-critique, and sub-agent adversarial review. Outputs a perfected prompt to `architect/prompt.md`.
- **Mode 2 (Plan Iteration)** — Runs in plan mode. Takes Claude's draft plan and iterates on it with structured review, research, and adversarial challenge. Outputs a finalized plan to `architect/plan.md`.

Also generates `CLAUDE.md` (persistent project context) and `TODO.md` (persistent task tracker) so execution survives context clears.

**Triggers**: `forge a plan`, `perfect this prompt`, `flesh out this idea`, `/forging-plans`

### forging-prompts

Prompt perfection for **mid-project features** in forked sessions. Same pipeline as forging-plans Mode 1, but assumes project context is already loaded from the fork.

Use when you're working on an existing project and want to add a well-specified feature without losing your current session context.

**Triggers**: `forge a prompt`, `sharpen this prompt`, `flesh out this feature`, `/forging-prompts`

### forging-shared

Shared reference files used by both skills:

| File | Purpose |
|------|---------|
| `questionnaire.md` | Adaptive questionnaire protocol with 12 categories. Categories 1-3 always asked; 4-12 assessed for relevance |
| `research.md` | Prior-art research protocol — official docs first, 2-year recency filter, cross-reference requirement |
| `challenge.md` | Adversarial challenge protocol — Phase A (self-critique) + Phase B (sub-agent review) |

## Depth Modes

Both skills support two depth levels:

- **Quick** — Core questions only (categories 1-3), skip research, self-critique only, one iteration. For small, well-understood features.
- **Full** (default) — Full questionnaire, prior-art research, adversarial challenge with sub-agent. For complex or high-stakes work.

## Installation

Copy the skills into your Claude Code skills directory:

```bash
# Clone the repo
git clone https://github.com/gradigit/forging-workflow.git

# Copy all three skill directories
cp -r forging-workflow/.claude/skills/forging-plans ~/.claude/skills/
cp -r forging-workflow/.claude/skills/forging-prompts ~/.claude/skills/
cp -r forging-workflow/.claude/skills/forging-shared ~/.claude/skills/
```

All three directories must be installed together — forging-plans and forging-prompts reference forging-shared.

## Typical Workflow

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
- **study skill** (optional, from [claude-skills](https://github.com/gradigit/claude-skills)) — enhances research phase

## License

MIT
