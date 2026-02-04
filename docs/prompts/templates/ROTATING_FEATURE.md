# Rotating Persona Feature Implementation

This template uses rotating personas that cycle through different perspectives on each iteration.

## Standard 6-Persona Rotation

Best for most feature implementations:

```bash
/ralph-loop "
Feature: [FEATURE_NUMBER]-[feature-name]
Branch: feature/[FEATURE_NUMBER]-[feature-name]

PHASE 1 - TASK COMPLETION:
- Read docs/features/[FEATURE_FOLDER]/tasks.md
- If any tasks unchecked (- [ ]), complete them first
- Mark completed tasks (- [x]) as you finish them
- Run build command

PHASE 2 - ROTATING PERSONA REVIEW (cycle each iteration):
Current Persona = ITERATION MOD 6:

[0] #5 IMPLEMENTER:
- Check tasks.md for incomplete items
- Implement next unchecked task
- Follow existing code patterns
- Run build after changes

[1] #9 CODE REVIEWER:
- Review recent changes for bugs, edge cases
- Check error handling and null safety
- Verify code follows project patterns
- Fix any issues found

[2] #7 TESTER:
- Run tests (if test project exists)
- Check test coverage for new code
- Write missing unit tests
- Verify edge cases are covered

[3] #3 UI_UX_DESIGNER (if UI changes):
- Review UI for consistency
- Check accessibility (keyboard nav, tab order)
- Verify visual states (hover, disabled, error)

[4] #10 SECURITY_AUDITOR:
- Check for hardcoded values that should be config
- Verify input validation
- Look for potential security issues
- Review any new file I/O or network code

[5] #2 PROJECT_MANAGER:
- Verify all tasks in tasks.md are checked
- Check requirements are met
- Document any gaps or issues found
- Update tasks.md if new work discovered

EACH ITERATION:
1. Identify current persona (Iteration % 6)
2. Perform that persona's review/work
3. Make improvements or fixes as needed
4. Commit changes with message: '[persona] description'
5. Post PR comment with findings/changes
6. If all tasks complete AND no issues found by ANY persona for 2 full cycles (12 iterations), output completion

OUTPUT <promise>FEATURE COMPLETE</promise> when:
- All tasks in tasks.md are checked [x]
- Build succeeds with no errors
- All personas report no issues for 2 consecutive cycles
" --completion-promise "FEATURE COMPLETE" --max-iterations 30
```

---

## Compact 4-Persona Rotation (Faster)

For simpler features:

```bash
/ralph-loop "
Feature: [FEATURE_NUMBER]-[feature-name]

ROTATING PERSONA (ITERATION MOD 4):

[0] #5 IMPLEMENTER: Complete next task from tasks.md, run build
[1] #9 REVIEWER: Review code for bugs/issues, fix problems
[2] #7 TESTER: Verify functionality, add tests if needed
[3] #2 PROJECT_MANAGER: Check all requirements met, update tasks.md

EACH ITERATION:
1. Run current persona's checks
2. Make one fix/improvement
3. Commit: '[persona] description'
4. Post PR comment with findings/changes

OUTPUT <promise>DONE</promise> when all tasks complete and 2 clean cycles.
" --completion-promise "DONE" --max-iterations 20
```

---

## PR Comment Format

Each persona should post a comment to the PR:

```markdown
## [PERSONA] Review - Iteration N

### Summary
[Brief description of what was reviewed/implemented]

### Findings
- [Finding 1]
- [Finding 2]

### Changes Made
- [Change 1 - file:line - description]

### Status
- [ ] Issues found requiring follow-up
- [x] Clean pass - no issues found
```

---

## Commit Message Format

```
[IMPLEMENTER] Add CoordinateSystem property to ProjectHeader
[REVIEWER] Fix null check in Zone property setter
[TESTER] Add unit test for UTM hemisphere selection
[SECURITY_AUDITOR] Add input validation for Zone number
[PROJECT_MANAGER] Mark data model tasks complete in tasks.md
```
