# Patterns & Anti-Patterns

Quick reference library for implementation. Each pattern includes the anti-pattern, the correct form, and the signal to watch for.

---

## Concurrency Patterns

### TOCTOU Prevention (Time-of-Check to Time-of-Use)

```
// ANTI-PATTERN: Race condition between check and act
status = read(record)            // Time of Check
// ← another process can modify record here ←
if status == "pending":          // Time of Use — potentially stale
    update(record, "processing")

// PATTERN A: Lock-based atomic check-and-act
lock(record)
status = read(record)
if status == "pending":
    update(record, "processing")
unlock(record)

// PATTERN B: Atomic conditional update (preferred when possible)
affected = UPDATE records
    SET status = 'processing', started_at = now()
    WHERE id = ? AND status = 'pending'

if affected > 0:
    proceed()   // We won the race
else:
    handle_conflict()  // Someone else got there first
```

**Signal to watch for**: Any `if X: then update X` pattern operating on shared state without a lock or atomic operation.

---

### Transaction Side-Effect Isolation

```
// ANTI-PATTERN: Audit log is rolled back with the transaction
begin_transaction()
    update_record(record, status="failed")  ← rolled back!
    create_audit_event("operation_failed")  ← rolled back!
    raise Error("something went wrong")     ← triggers rollback
end_transaction()

// PATTERN: Error state persists OUTSIDE the transaction
try:
    begin_transaction()
        do_work()
    commit()
except Error as e:
    // OUTSIDE the transaction — these persist
    update_record(record, status="failed")
    create_audit_event("operation_failed")
    raise e
```

**Signal to watch for**: Audit records, failure markers, or external calls (emails, webhooks) inside a transaction that can throw.

---

## Testing Patterns

### Mutation-Resistant Assertions

```
// WEAK: Passes even if the code returns garbage
assert result != null
assert succeeded == true

// STRONG: Would fail if any expected state change was missed
assert result.count == 5
assert result.status == "completed"
assert result.completed_at is not None
assert result.items[0].name == "expected_value"
assert result.processed_ids == {1, 2, 3}
```

**The mutation test**: If I changed `>` to `>=` in the implementation, would a test catch it? If not, add the test.

---

### Boundary Condition Testing

```python
# If code checks `value > 0`, always test:
test_with(0)    # boundary — must fail the check
test_with(1)    # just above — must pass the check
test_with(-1)   # below — must fail the check

# If code checks string length (max 100):
test_with("")           # empty
test_with("a")          # minimal
test_with("a" * 100)    # at max
test_with("a" * 101)    # over max — must reject

# If code processes a list:
test_with([])           # empty — must not crash
test_with([item])       # single item
test_with([item] * N)   # large list — must not timeout
```

---

## Implementation Patterns

### Constants Over Magic Values

```
// ANTI-PATTERN: Strings scattered across codebase
if user.status == "active":
    account.type = "traditional_ira"

// PATTERN: Centralized, searchable, refactorable constants
if user.status == UserStatus.ACTIVE:
    account.type = AccountType.TRADITIONAL_IRA
```

**Signal to watch for**: String literals or numbers in conditional logic that appear in more than one place.

---

### Defensive Existence Checks

```
// ANTI-PATTERN: Assumes the relationship exists
order.customer.email     ← NullPointerException if customer is null

// PATTERN: Explicit existence check with meaningful error
if not order.customer:
    raise ValidationError(f"Order {order.id} has no customer assigned")
email = order.customer.email
```

---

### DRY — But Wisely

```bash
# When fixing a bug in one place, always ask:
# "Where else does this same pattern exist?"
grep -rn "the_buggy_pattern" src/

# Fix ALL occurrences, or extract a shared abstraction.
# One-off fixes that leave duplicates are tech debt commits.
```

---

## Verification Commands

```bash
# Verify a function/method exists before using it
grep -rn "function methodName" src/
grep -rn "def method_name" src/

# Find all usages of a pattern
grep -rn "pattern" src/

# Check for existing constants
grep -rn "CONSTANT_NAME" src/

# Review what you're about to commit
git diff --staged

# Check if GitHub issue exists
gh issue list --search "keyword"

# Find files changed in recent commits (scope analysis)
git log --oneline -10 --name-only
```

---

## The Architect's Pre-Flight

Before writing ANY code, answer these six questions:

1. **Reach**: What are ALL the ways this code can be reached?
2. **Shared state**: What other code modifies the same data?
3. **Concurrency**: What happens under concurrent access?
4. **Edge cases**: What are the boundary conditions? (null, zero, negative, max, empty, duplicate)
5. **Invariants**: What must always be true after this code runs?
6. **Testability**: How would I test that those invariants hold?

If you cannot answer all six: you are not ready to code. Explore first.

---

## Anti-Pattern Quick Reference

| Anti-Pattern | Signal | Correct Approach |
|---|---|---|
| Cargo Cult | "Everyone uses X" without knowing why | Define the specific problem X solves for your context |
| Blind Pilot | Optimize without observability | Add metrics/tracing before optimizing |
| Ritualistic Guard | Process overhead with no outcome | Audit with Maqāsid: does this process achieve its purpose? |
| Scientist's Debt | "Try random things until it works" | Al-Kindi: isolate, reproduce deterministically, then fix |
| Gold Plating | "While I'm here, I'll also..." | Minimal implementation. No "while I'm here." |
| Hallucinatory Reference | Reference a method without verifying it exists | Grep before coding |
| Magic Numbers | `if status == "active"` | Use constants/enums |
| Transaction Blindness | Audit records inside rollbackable transactions | Error state outside the transaction |
