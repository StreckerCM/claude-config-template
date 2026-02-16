# Claude Prompts & Personas

This directory contains persona definitions and prompt templates for use with Ralph Wiggum loops in Claude Code.

## Contents

| File | Purpose |
|------|---------|
| `PERSONAS.md` | 11 development personas with mindsets and output formats |
| `templates/ROTATING_FEATURE.md` | Ralph Loop prompt templates (6, 4, and 11 persona variants) |

## Quick Start

The standard development workflow uses a 6-persona rotation:

```
[0] #5 IMPLEMENTER   - Complete tasks, write code
[1] #9 REVIEWER      - Review for bugs, code quality
[2] #7 TESTER        - Verify functionality, add tests
[3] #3 UI_UX_DESIGNER - Review UI/UX, accessibility
[4] #10 SECURITY     - Security review, input validation
[5] #2 PROJECT_MGR   - Check requirements, update tasks
```

Each iteration uses `ITERATION % 6` to determine the active persona. The loop continues until all tasks are complete and 2 consecutive clean cycles pass.

## Recommended: Subagent-Driven Development

For multi-phase features (migration waves, large PRs), use **parallel subagents** instead of the Ralph Loop. This launches multiple review personas simultaneously via the `Task` tool, with per-persona model hints for cost efficiency.

```
Implement (sonnet) → Review in parallel (opus+sonnet+opus) → Fix → PM update → repeat
```

### Key Advantages Over Ralph Loop

- **Parallel reviews** — 3 personas run simultaneously instead of sequentially
- **Model hints** — opus for judgment (Reviewer, Security), sonnet for execution (Tester), haiku for coordination (PM)
- **60-70% cost savings** vs. using opus for everything
- **Better for large changes** — subagents get targeted context, not full conversation history

### Quick Start

1. Implement the next batch of tasks (IMPLEMENTER, sonnet)
2. Launch 3 parallel review agents: REVIEWER (opus) + TESTER (sonnet) + SECURITY (opus)
3. Fix all Critical/Important findings
4. PROJECT_MANAGER (haiku) updates tasks.md with iteration log
5. Repeat until all tasks complete and review cycle is clean

See [templates/SUBAGENT_LOOP.md](./templates/SUBAGENT_LOOP.md) for the full template with copy-paste prompts.

---

## Adding to a New Repository

1. Copy this entire `docs/prompts/` directory to your repo
2. Ensure `CLAUDE.md` references these files
3. Customize persona behaviors if needed for your project

See the template repo README for full setup instructions.
