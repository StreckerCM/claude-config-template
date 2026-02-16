# Bug Fix Prompt Template

Copy and customize this template for fixing bugs.

---

## Template

```
Using the DEBUGGER persona from docs/prompts/PERSONAS.md:

## Bug Report
[DESCRIPTION OF THE BUG]

## Expected Behavior
[WHAT SHOULD HAPPEN]

## Actual Behavior
[WHAT ACTUALLY HAPPENS]

## Reproduction Steps (if known)
1. [Step 1]
2. [Step 2]
3. [Step 3]

## Instructions
1. First, try to reproduce the bug
2. Add logging/diagnostics if needed to understand the issue
3. Identify the root cause
4. Implement a fix
5. Add a regression test to prevent recurrence
6. Verify the fix works
7. Clean up any diagnostic code

## Success Criteria
- Bug is fixed and no longer reproducible
- Regression test added and passing
- {{BUILD_COMMAND}} succeeds
- All existing tests still pass
- No new warnings introduced

## Completion
When all success criteria are met, output:
<promise>BUG FIXED</promise>
```
