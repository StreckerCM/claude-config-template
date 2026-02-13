# Refactoring Prompt Template

Copy and customize this template for code refactoring tasks.

---

## Template

```
Using the REFACTORER persona from docs/prompts/PERSONAS.md:

## Refactoring Goal
[WHAT YOU WANT TO ACHIEVE]

## Target Files
- [FILE 1]
- [FILE 2]

## Constraints
- Preserve existing behavior exactly
- Keep all tests passing
- Follow existing code patterns
- Make incremental changes

## Instructions
1. Review the target code and understand current behavior
2. Run existing tests to establish baseline: {{TEST_COMMAND}}
3. Make small, incremental changes
4. Run tests after each change
5. Commit frequently with descriptive messages
6. If behavior change is needed, stop and clarify

## Refactoring Techniques to Consider
- Extract Method/Class
- Rename for clarity
- Remove duplication
- Simplify conditionals
- Apply SOLID principles

## Success Criteria
- All existing tests pass
- {{BUILD_COMMAND}} succeeds
- No functional changes (same inputs produce same outputs)
- Code is cleaner and more maintainable

## Completion
When all success criteria are met, output:
<promise>REFACTOR COMPLETE</promise>
```
