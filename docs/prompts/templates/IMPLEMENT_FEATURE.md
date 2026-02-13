# Feature Implementation Prompt Template

Copy and customize this template for implementing features.

---

## Template

```
Using the IMPLEMENTER persona from docs/prompts/PERSONAS.md:

## Task
Implement Feature [PRIORITY] ([FEATURE_NAME]).

## Reference Documents
- Specification: docs/features/[FOLDER]/spec.md
- Implementation Plan: docs/features/[FOLDER]/plan.md
- Task Checklist: docs/features/[FOLDER]/tasks.md

## Instructions
1. Read all reference documents first
2. Follow the implementation plan phases in order
3. Check off tasks in tasks.md as you complete them using `[x]`
4. Run {{BUILD_COMMAND}} after significant changes
5. Commit logical chunks with descriptive messages
6. If blocked, document the issue in tasks.md and continue with other tasks

## Success Criteria
- All tasks in tasks.md are checked `[x]`
- Build succeeds with no errors
- No new compiler warnings
- Code follows existing project patterns (see CLAUDE.md)

## Completion
When all success criteria are met, output:
<promise>FEATURE COMPLETE</promise>
```
