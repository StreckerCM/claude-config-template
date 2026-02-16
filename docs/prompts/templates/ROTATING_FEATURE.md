# Rotating Persona Feature Implementation

Each iteration, the persona changes based on `iteration % N` — ensuring every perspective reviews the work multiple times.

## Standard 6-Persona Rotation

```bash
/ralph-loop "
Feature: [FEATURE_NUMBER]-[feature-name]
Branch: feature/[FEATURE_NUMBER]-[feature-name]

PHASE 1 - TASK COMPLETION:
- Read docs/features/[FEATURE_FOLDER]/tasks.md
- If any tasks unchecked (- [ ]), complete them first
- Mark completed tasks (- [x]) as you finish them
- Run: {{BUILD_COMMAND}}

PHASE 2 - ROTATING PERSONA REVIEW (cycle each iteration):
Current Persona = ITERATION MOD 6:

[0] #5 IMPLEMENTER (model:sonnet): Complete next task, follow patterns, run build
[1] #9 CODE REVIEWER (model:opus): Review for bugs/edge cases, check patterns, fix issues
[2] #7 TESTER (model:sonnet): Run tests, check coverage, write missing tests
[3] #3 UI_UX_DESIGNER (model:sonnet): Review UI consistency, accessibility, visual states
[4] #10 SECURITY_AUDITOR (model:opus): Check config values, input validation, file I/O
[5] #2 PROJECT_MANAGER (model:haiku): Verify tasks complete, check requirements, update tasks.md

EACH ITERATION:
1. Identify current persona (Iteration % 6)
2. Perform that persona's review/work
3. Commit: '[persona] description'
4. Post PR comment with findings/changes
5. If all tasks complete AND 2 clean cycles (12 iterations), output completion

OUTPUT <promise>FEATURE COMPLETE</promise> when:
- All tasks in tasks.md are checked [x]
- Build succeeds with no errors
- All personas report no issues for 2 consecutive cycles
" --completion-promise "FEATURE COMPLETE" --max-iterations 30
```

## Compact 4-Persona Rotation (Faster)

For simpler features:

```bash
/ralph-loop "
Feature: [FEATURE_NUMBER]-[feature-name]

ROTATING PERSONA (ITERATION MOD 4):
[0] #5 IMPLEMENTER (model:sonnet): Complete next task from tasks.md, run build
[1] #9 REVIEWER (model:opus): Review code for bugs/issues, fix problems
[2] #7 TESTER (model:sonnet): Verify functionality, add tests if needed
[3] #2 PROJECT_MANAGER (model:haiku): Check all requirements met, update tasks.md

EACH ITERATION: Run persona checks, commit '[persona] description', post PR comment.

OUTPUT <promise>DONE</promise> when all tasks complete and 2 clean cycles.
" --completion-promise "DONE" --max-iterations 20
```

## Full 11-Persona Rotation (Comprehensive)

For critical features requiring maximum scrutiny:

```bash
/ralph-loop "
Feature: [FEATURE_NUMBER]-[feature-name]

ROTATING PERSONA (ITERATION MOD 11):
[0] #1 BUSINESS_ANALYST (haiku)    [5] #6 REFACTORER (sonnet)
[1] #2 PROJECT_MANAGER (haiku)     [6] #7 TESTER (sonnet)
[2] #3 UI_UX_DESIGNER (sonnet)     [7] #8 DEBUGGER (sonnet)
[3] #4 UI_IMPLEMENTER (sonnet)     [8] #9 REVIEWER (opus)
[4] #5 IMPLEMENTER (sonnet)        [9] #10 SECURITY_AUDITOR (opus)
                                    [10] #11 DOCUMENTER (haiku)

OUTPUT <promise>FEATURE COMPLETE</promise> when all tasks done and clean cycle.
" --completion-promise "FEATURE COMPLETE" --max-iterations 44
```

## Commit & PR Format

**Commits:** `[IMPLEMENTER] Add feature X` / `[REVIEWER] Fix null check` / `[TESTER] Add unit test for Y`

**PR Comments:**
```markdown
## [PERSONA] Review - Iteration N

### Summary
[Brief description]

### Findings
- [Issues or observations]

### Changes Made
- [file:line - description]

### Status
- [ ] Issues found requiring follow-up
- [x] Clean pass - no issues found
```
