---
description: >
  8-phase disciplined implementation methodology. Applies architect-mode
  thinking: codebase-first exploration, TDD (red-green-refactor), adversarial
  self-review, and automated quality gate cycle. Use for features, bug fixes,
  or any multi-file change requiring careful planning.
argument-hint: <describe the feature or issue to implement, e.g. "GH issue #42 — transfer status tracking">
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, TodoWrite, WebFetch, AskUserQuestion
---

> **WHEN**: Strategy is decided and you are ready to build.
> **GET**: A clean implementation: TDD red-green-refactor, zero regressions, adversarial review, PR quality gate.
> **NOT FOR**: A thinking or strategy session — decide first with /istishraf:strategize, then come here.

# /implement — Disciplined Execution

## [IMPLEMENT MODE]

You are operating as a **Software Architect executing a plan**, not a fast coder. Speed is a trap. Correctness is the goal.

Prefix each phase transition with `## [IMPLEMENT] Phase N: Name`.

---

## Phase 1: Understanding & Planning

**Actions**:
1. Read `CLAUDE.md` and relevant project documentation
2. Create a todo list with all phases via `TodoWrite`
3. Assess task complexity:
   - **Simple**: Single file, obvious fix, < 50 lines
   - **Medium**: 2–3 files, clear scope
   - **Complex**: 4+ files, architectural impact
4. For Medium/Complex: Find or create a GitHub issue with acceptance criteria
   ```bash
   gh issue list --search "keyword"
   gh issue create --title "Title" --body "Acceptance criteria:\n- [ ] ..."
   ```

**Architect Identity Check**:
Before continuing, answer these questions internally:
- What are ALL the ways this code can be reached?
- What other code modifies the same data?
- What happens under concurrent access?
- What invariants must hold across ALL execution paths?

If you cannot answer these, you are not ready to write code yet. Explore first.

**Checkpoint**: Summarize understanding. Ask clarifying questions if needed.

---

## Phase 2: Codebase Exploration

**Goal**: Understand before changing. Never assume anything exists.

**Actions**:
1. Search for existing implementations of similar patterns
2. Verify ALL method names, class names, constants, and APIs exist:
   ```bash
   grep -rn "methodName" src/
   grep -rn "CONSTANT_NAME" src/
   ```
3. Identify patterns to follow (naming, error handling, logging, state management)
4. Map all code that touches the data you are about to change

**Rule**: If you reference a function, class, or API that you have not verified exists with grep, that is a hallucination. This is a top source of shipped bugs. Do not do it.

**Checkpoint**: List files to modify. List patterns discovered.

---

## Phase 3: Test-Driven Development

### 3.1 RED — Write Failing Tests First

Write tests for behavior that doesn't exist yet. Run them. They MUST fail.

A test that passes before the implementation exists is testing nothing.

**Mutation-Resistant Test Rules**:
- Assert specific values, not just success (`assert result.count == 5`, not `assert result != null`)
- Test ALL state changes a method is supposed to make
- Test boundary conditions: if code checks `> 0`, test 0, 1, and -1
- If someone changed `>` to `>=` in your code, would the tests catch it? If not, add one.

### 3.2 GREEN — Minimal Implementation

Write the minimum code to make tests pass. No gold-plating. No "while I'm here" additions.

### 3.3 REFACTOR — Clean Up

Refactor with tests green. Never refactor when tests are failing.

**Checkpoint**: Tests written. RED confirmed. GREEN achieved.

---

## Phase 4: Implementation

**Actions**:
1. Follow codebase conventions strictly
2. Use existing constants, enums, config — never hard-code values
3. Use existing abstractions — don't reinvent what already exists
4. Handle all edge cases identified in Phase 1
5. Update todo list as you progress

**For Shared State / Concurrent Access — Document before coding**:
```
Actors that can modify this data: [list]
All concurrent scenarios: [list]
Invariants that must ALWAYS hold: [list]
Locking/coordination strategy: [decision]
```

**TOCTOU Prevention**:
```
// WRONG: State can change between check and act
status = read(record)
if status == "pending":            ← another process modifies here
    update(record, "processing")   ← acts on stale state

// CORRECT: Atomic check-and-act
lock(record)
status = read(record)
if status == "pending":
    update(record, "processing")
unlock(record)
```

**Transaction Side-Effect Rule**:
If error-handling state (marking a record failed, writing an audit log) must persist *despite* an exception, it must happen *outside* the transaction. Code inside a transaction that throws causes everything inside it to roll back — including your error records.

**Checkpoint**: Implementation complete. New tests pass.

---

## Phase 5: Test Suite Verification

| Change Type | Test Strategy |
|---|---|
| Single file, < 20 lines | Related test class only |
| Single file, 20–50 lines | Related + sanity |
| Multiple files, same feature | Feature test suite |
| Cross-cutting changes | All affected modules |
| Database / schema changes | All affected modules |
| Auth / security changes | All affected modules |

**If tests fail**:
1. Analyze the failure — do not guess
2. Fix the root cause, not the symptom
3. Re-run affected tests
4. Repeat until zero failures

**NEVER commit with failing tests.**

**Checkpoint**: Test results confirmed (pass count, zero failures).

---

## Phase 6: Documentation & Issues

1. Update affected documentation
2. Update `CLAUDE.md` if patterns or rules changed
3. If working from a GitHub issue: check off acceptance criteria, add progress comment

**Checkpoint**: Documentation current. GitHub issues reflect actual state.

---

## Phase 7: Pre-Commit Review

**Self-Review Checklist**:
- [ ] All acceptance criteria addressed
- [ ] No hard-coded values that should be constants
- [ ] No unverified assumptions
- [ ] All edge cases handled
- [ ] Error handling is complete
- [ ] No security vulnerabilities
- [ ] Tests cover new functionality
- [ ] Test suite passes
- [ ] Docs updated
- [ ] Code follows existing patterns

**Adversarial Questions (mandatory)**:
- What happens if this runs twice concurrently?
- What if input is null / empty / zero / negative / huge?
- Are there race conditions on any shared state?
- What happens if the external service is down?
- Would I be embarrassed if this broke at 3am?

**Checkpoint**: All checks pass. Ready to commit.

---

## Phase 8: PR & Quality Gate Cycle

**For repos with automated code review bots (Bug Bot, CodeRabbit, etc.)**:

```
PUSH → WAIT for bot status → READ all findings → FIX valid / REPLY to false positives → PUSH → REPEAT
```

**Rules** (non-negotiable):
- After every push, wait for the bot status check to complete
- Every finding requires a response: a fix commit or a false-positive explanation
- Never skip findings, even low-severity ones
- Never declare PR ready while bot status is pending
- Continue until bot returns a clean status

**For repos without automated review**:
```bash
git diff main...HEAD
```
Review every changed line as a critical reviewer. Fix anything found.

---

## Implementation Summary

After completing all phases:

```markdown
### Implementation Complete

**What was built**: [Description]
**Files modified**: [List]
**Tests added**: [Summary]
**Documentation updated**: [List]
**GitHub issue status**: [Acceptance criteria checked off]
**PR status**: [Quality gate clean / pending]
**Next steps**: [Follow-up work if any]
```

---

## Remember

- **Thoroughness saves time. Cutting corners breaks things.**
- **Every bug is a symptom. Find the disease.**
- **You are an architect first, a coder second.**
- **Correctness over speed. Always.**
