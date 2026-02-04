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

## Adding to a New Repository

1. Copy this entire `docs/prompts/` directory to your repo
2. Ensure `CLAUDE.md` references these files
3. Customize persona behaviors if needed for your project

See the template repo README for full setup instructions.
