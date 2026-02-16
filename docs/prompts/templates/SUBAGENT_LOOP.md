# Subagent-Driven Development Template

Parallel persona review using the `Task` tool. Unlike the Ralph Loop (single-context, rotating persona per iteration), this pattern launches **multiple personas as parallel subagents** and manages iterations in the main conversation.

## When to Use This Instead of Ralph Loop

| Criteria | Ralph Loop | Subagent Loop |
|----------|-----------|---------------|
| Context | Single context, personas rotate | Main context orchestrates, personas run in parallel |
| Speed | Sequential — one persona per iteration | Parallel — 3-4 personas review simultaneously |
| Cost | Higher (full context replayed each iteration) | Lower (subagents get targeted context only) |
| Best for | Smaller features, single-file changes | Multi-phase features, large PRs |
| Model control | No per-persona model selection | Per-persona model hints (opus/sonnet/haiku) |

---

## Standard Pattern: Implement → Review → Fix → Coordinate

```
┌─────────────────────────────────────────────────┐
│  1. IMPLEMENTER (sequential, sonnet)            │
│     Complete tasks from tasks.md, commit         │
└──────────────────────┬──────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────┐
│  2. PARALLEL REVIEW (3 subagents)               │
│     ┌──────────────┬──────────────┬───────────┐ │
│     │ REVIEWER     │ TESTER       │ SECURITY  │ │
│     │ (opus)       │ (sonnet)     │ (opus)    │ │
│     └──────────────┴──────────────┴───────────┘ │
└──────────────────────┬──────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────┐
│  3. FIX PHASE (sequential, sonnet)              │
│     Address all findings from reviewers          │
└──────────────────────┬──────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────┐
│  4. PROJECT_MANAGER (sequential, haiku)         │
│     Update tasks.md, log iteration, assess next  │
└──────────────────────┬──────────────────────────┘
                       ▼
              ┌────────────────┐
              │ More tasks?    │──Yes──► Back to step 1
              │ Clean cycle?   │──No───► DONE
              └────────────────┘
```

---

## Template

### Step 1: IMPLEMENTER Phase

Run the IMPLEMENTER as a Task subagent (or do it directly in the main context):

```
Task(subagent_type: "general-purpose", model: "sonnet", description: "[IMPLEMENTER] Phase N")

Prompt:
"You are the IMPLEMENTER persona (#5) from docs/prompts/PERSONAS.md.

Branch: [BRANCH_NAME]
Feature: [FEATURE_NUMBER]-[feature-name]

Read docs/features/[FEATURE_FOLDER]/tasks.md and complete the following tasks:
[LIST SPECIFIC TASKS FOR THIS ITERATION]

After completing each task:
1. Mark it [x] in tasks.md
2. Build: [BUILD_COMMAND]
3. Commit: '[IMPLEMENTER] description'

Do NOT move to review — just implement and commit."
```

### Step 2: Parallel Review Phase

Launch 3 review personas simultaneously using the Task tool:

```
// All 3 launched in a single message (parallel)

Task(subagent_type: "superpowers:code-reviewer", model: "opus", description: "[REVIEWER] Iteration N")
Prompt:
"You are the REVIEWER persona (#9) from docs/prompts/PERSONAS.md.

Branch: [BRANCH_NAME]
Review the changes made in the latest commits on this branch.

Focus on:
- Bugs, logic errors, edge cases
- Pattern consistency with existing codebase
- Missing error handling
- Code quality and maintainability

For each finding, classify as Critical / Important / Suggestion.
If you find fixable issues, fix them and commit: '[REVIEWER] Fix description'.
Report your findings in this format:

## [REVIEWER] Review — Iteration N
### Findings
- [severity]: [description] ([file:line])
### Changes Made
- [file:line] — [description]
### Status
- [ ] Issues found requiring follow-up
- [x] Clean pass — no issues found"

---

Task(subagent_type: "general-purpose", model: "sonnet", description: "[TESTER] Iteration N")
Prompt:
"You are the TESTER persona (#7) from docs/prompts/PERSONAS.md.

Branch: [BRANCH_NAME]

Run the test suite and verify build:
[TEST_COMMANDS]
[BUILD_COMMAND]

Check:
- All existing tests still pass
- New code has adequate test coverage
- No new build warnings introduced

If tests fail or coverage gaps exist, write missing tests and commit:
'[TESTER] Add tests for description'.

Report findings in this format:

## [TESTER] Review — Iteration N
### Test Results
- [test project]: [pass count]/[total] passed
### Build Status
- Warnings: [count] (list any new ones)
### Coverage Gaps
- [description of untested code]
### Changes Made
- [file:line] — [description]
### Status
- [ ] Issues found requiring follow-up
- [x] Clean pass — all tests pass"

---

Task(subagent_type: "general-purpose", model: "opus", description: "[SECURITY] Iteration N")
Prompt:
"You are the SECURITY_AUDITOR persona (#10) from docs/prompts/PERSONAS.md.

Branch: [BRANCH_NAME]

Review the changes in the latest commits for security issues:
- SQL injection (check all query construction)
- Input validation and sanitization
- File path traversal
- Hardcoded credentials or secrets
- Resource disposal (IDisposable)
- Unsafe deserialization

For each finding, classify as Critical / High / Medium / Low.

Report findings in this format:

## [SECURITY] Audit — Iteration N
### Findings
| Severity | Issue | Location | Recommendation |
|----------|-------|----------|----------------|
| ... | ... | ... | ... |
### Status
- [ ] Critical/High issues requiring immediate fix
- [x] Clean pass — zero Critical/High"
```

### Step 3: Fix Phase

Address findings from all 3 reviewers. Run in main context or as a subagent:

```
Task(subagent_type: "general-purpose", model: "sonnet", description: "[REVIEWER] Fix findings")

Prompt:
"Address the following findings from the review cycle:

REVIEWER findings:
[PASTE REVIEWER OUTPUT]

TESTER findings:
[PASTE TESTER OUTPUT]

SECURITY findings:
[PASTE SECURITY OUTPUT]

Fix all Critical and Important items. Commit: '[REVIEWER] Address review feedback on Phase N'."
```

### Step 4: PROJECT_MANAGER Phase

```
Task(subagent_type: "general-purpose", model: "haiku", description: "[PM] Update tasks.md")

Prompt:
"You are the PROJECT_MANAGER persona (#2) from docs/prompts/PERSONAS.md.

Update docs/features/[FEATURE_FOLDER]/tasks.md:
1. Mark completed tasks [x]
2. Add an iteration log entry with this format:

### Iteration N (DATE) — [SCOPE]

**Approach:** Subagent-driven development with persona model hints
(opus=judgment, sonnet=execution, haiku=coordination).

**Completed:**
- [list of completed tasks]

**Review cycle (3 personas in parallel):**
- REVIEWER (opus): [PASS/FAIL] — [summary]
- TESTER (sonnet): [PASS/FAIL] — [summary]
- SECURITY (opus): [PASS/FAIL] — [summary]

**Commits:**
- `[sha]` [PERSONA] description

**Next:** [what comes next]

3. Commit: '[PROJECT_MANAGER] Update tasks.md after Iteration N'"
```

---

## Model Hints Reference

| Tier | Model | Personas | Rationale |
|------|-------|----------|-----------|
| **Judgment** | opus | Reviewer, Security, Architect | Mistakes are expensive |
| **Execution** | sonnet | Implementer, Tester, UI/UX, Debugger | Bulk code generation |
| **Coordination** | haiku | Project Manager, Documenter | Bookkeeping, prose |

This typically saves 60-70% on token costs vs. using opus for everything.

---

## Completion Criteria

An iteration is **clean** when all 3 review personas report no Critical/Important issues.

**Feature complete** when:
- All tasks in tasks.md are checked `[x]`
- Build succeeds with no errors
- All tests pass
- At least 1 clean review cycle after last code change

---

## Concrete Example

```
Iteration 1: Phases 0-1
├── IMPLEMENTER (sonnet) → implement Phase 0 + Phase 1
├── REVIEWER (opus) + TESTER (sonnet) + SECURITY (opus) → parallel review
├── Fix phase (sonnet) → address findings
└── PROJECT_MANAGER (haiku) → update tasks.md

Iteration 2: Phases 2-3
├── IMPLEMENTER (sonnet) → implement Phase 2 + Phase 3
├── REVIEWER (opus) + TESTER (sonnet) + SECURITY (opus) → parallel review
├── Fix phase (sonnet) → address findings
└── PROJECT_MANAGER (haiku) → update tasks.md

...continue until all phases complete...
```

---

## Variant: 4-Persona Review (With UI/UX)

For UI-heavy features, add a 4th parallel reviewer:

```
Task(subagent_type: "general-purpose", model: "sonnet", description: "[UI_UX] Iteration N")

Prompt:
"You are the UI_UX_DESIGNER persona (#3) from docs/prompts/PERSONAS.md.

Review the UI changes for:
- Consistent layout, spacing, typography
- Keyboard navigation and tab order
- Visual states (default, selected, error, disabled)
- Accessibility (screen reader labels, contrast)
- Match existing application style

Report findings with severity levels."
```

---

## Variant: Quick Review (Smaller Changes)

For small changes, skip the full subagent machinery and run reviews in the main context:

1. Implement the change directly
2. Launch just REVIEWER (opus) as a single subagent
3. Fix any findings
4. Update tasks.md directly

---

## Tips

- **Don't over-iterate.** If the review cycle is clean, move to the next phase. Don't re-review clean code.
- **Batch phases.** Group 2-3 related phases per iteration to reduce overhead.
- **IMPLEMENTER can be main context.** For complex implementations, doing it in the main context (not a subagent) gives better results because the orchestrator has full project context.
- **Paste findings, don't summarize.** When feeding reviewer output to the fix phase, paste the actual findings — don't paraphrase.
- **Background agents may have empty output.** On Windows, use `resume` on the agent ID if the output file is empty.
