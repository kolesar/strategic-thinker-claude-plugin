# Quick-Reference Checklists

---

## Pre-Implementation

- [ ] Read `CLAUDE.md` and project documentation
- [ ] Assessed complexity (Simple / Medium / Complex)
- [ ] Created or found GitHub issue with acceptance criteria (Medium+)
- [ ] Created todo list with all phases
- [ ] Verified all methods, APIs, and constants exist (grep confirmed)
- [ ] Identified patterns to follow
- [ ] Listed files to modify
- [ ] Answered the 6 Pre-Flight questions (see PATTERNS.md)

---

## TDD

- [ ] Wrote failing test FIRST (RED phase confirmed)
- [ ] Test fails for the *right* reason (not a setup error)
- [ ] Implemented minimal code (GREEN achieved)
- [ ] All new tests pass
- [ ] Boundary conditions tested (0, 1, -1, null, empty, max)
- [ ] Side-effect assertions included (not just success checks)
- [ ] Tests isolated from external dependencies

---

## Implementation

- [ ] Using constants/enums, not hard-coded strings
- [ ] Using project's existing logging patterns
- [ ] Following project's error handling conventions
- [ ] Input validation is complete
- [ ] All edge cases handled
- [ ] Race conditions checked for any shared state
- [ ] Transaction side-effects considered (audit/error state outside transaction)
- [ ] No assumptions made without grep verification

---

## Pre-Commit

- [ ] All acceptance criteria addressed
- [ ] No hard-coded values (use constants)
- [ ] No hallucinated references (all methods verified to exist)
- [ ] All edge cases handled
- [ ] Error handling is complete and correct
- [ ] No security vulnerabilities
- [ ] Tests cover new functionality
- [ ] Appropriate test suite passes
- [ ] Documentation updated
- [ ] Code follows existing patterns

---

## Adversarial Review (before commit)

- [ ] What happens if this runs twice concurrently? → SAFE / FIX REQUIRED
- [ ] What if input is null / empty / zero / negative / huge? → SAFE / FIX REQUIRED
- [ ] Are there race conditions on any shared state? → SAFE / FIX REQUIRED
- [ ] Is there a TOCTOU gap between check and act? → SAFE / FIX REQUIRED
- [ ] Any security vulnerabilities? (injection, XSS, privilege escalation) → SAFE / FIX REQUIRED
- [ ] Any transaction side-effects that shouldn't be rolled back? → SAFE / FIX REQUIRED
- [ ] Would I be embarrassed if this broke in production? → NO / FIX REQUIRED

---

## PR & Quality Gate

- [ ] Branch is clean (focused on one concern)
- [ ] PR description references the GitHub issue
- [ ] Ran affected test suite locally before pushing
- [ ] All automated bot findings reviewed (no unresolved findings)
- [ ] Valid issues fixed with a fix commit
- [ ] False positives replied to with explanation
- [ ] Bot status is clean before declaring PR ready
- [ ] GitHub issue updated with completed acceptance criteria

---

## Test Strategy Reference

| Change Type | Test Strategy |
|---|---|
| Single file, < 20 lines | Related test class only |
| Single file, 20–50 lines | Related + sanity |
| Multiple files, same feature | Feature test suite |
| Cross-cutting changes | All affected modules |
| Database / schema | All affected modules |
| Auth / security | All affected modules |

---

## GitHub Issue Quick Commands

```bash
# Find existing issue
gh issue list --search "keyword"

# Create with acceptance criteria
gh issue create --title "Feature: X" --body "## Acceptance Criteria\n- [ ] ...\n- [ ] ..."

# Check off criteria (edit body)
gh issue edit 42 --body "## Acceptance Criteria\n- [x] Done\n- [ ] Pending"

# Add progress comment
gh issue comment 42 --body "Phase 4 complete. Tests passing."

# Close on completion
gh issue close 42 --comment "Completed in PR #57."
```
