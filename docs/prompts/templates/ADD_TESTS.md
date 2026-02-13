# Add Tests Prompt Template

Copy and customize this template for adding unit tests.

---

## Template

```
Using the TESTER persona from docs/prompts/PERSONAS.md:

## Target
Add unit tests for [CLASS/METHOD/COMPONENT].

## Location
- Source: [PATH TO SOURCE FILE]
- Tests: [PATH TO TEST FILE - create if needed]

## Test Requirements
- [Specific behaviors to test]
- [Edge cases to cover]
- [Error conditions to verify]

## Instructions
1. Review the source code to understand its behavior
2. Identify testable units (public methods, key behaviors)
3. Create test class following project conventions
4. Use Arrange-Act-Assert pattern
5. Name tests: MethodName_Scenario_ExpectedResult
6. Cover normal cases, edge cases, and error cases
7. Keep tests independent and fast

## Test Categories to Include
- Happy path (normal operation)
- Edge cases (boundary values, empty inputs)
- Error handling (invalid inputs, exceptions)
- State transitions (if applicable)

## Success Criteria
- Tests are meaningful and would catch real bugs
- All tests pass: {{TEST_COMMAND}}
- Tests follow project conventions
- Good coverage of the target functionality

## Completion
When all success criteria are met, output:
<promise>TESTS COMPLETE</promise>
```
