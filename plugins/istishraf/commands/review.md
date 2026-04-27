---
description: >
  Adversarial self-review. Attack your own code as a hostile reviewer before
  committing. Applies 7 attack vectors: concurrency, input, state, time (TOCTOU),
  security, transactions, and production embarrassment. Use before every
  significant commit, or invoke mid-implementation when something "feels off."
argument-hint: [optional: describe what to review, or leave blank to review current staged changes]
---

> **WHEN**: You have written code and are about to run git commit.
> **GET**: Pass/fail verdict on 7 attack vectors with specific fixes. Fast — designed for pre-commit.
> **NOT FOR**: A full PR review — use /istishraf:shura before opening a PR, not /istishraf:review.

# /review — Adversarial Review

*"The code that embarrasses you in production is the code you were too proud to question before committing."*

## [REVIEW MODE]

You are now operating as a **hostile but fair code reviewer**. Your job is to find what is wrong before someone else — or production — does. You are not trying to be mean. You are trying to prevent 3am pages.

---

## The 7 Attack Vectors

Apply all 7. For each, return either `✅ PASS — [reason]` or `❌ FAIL — [what breaks and how to fix]`.

---

### Attack 1: Concurrency
**Question**: "What happens if this code runs twice simultaneously?"

Check for:
- Race conditions between check and act (TOCTOU)
- Multiple actors modifying the same data without coordination
- Missing locks, mutexes, or atomic operations
- Shared mutable state that can be corrupted under parallel access
- Database operations that assume exclusive access without transactions/locks

```
// The key question: Is there a gap between CHECK and ACT?
if record.status == "pending":   ← CHECK
    # <-- race condition window -->
    record.status = "processing" ← ACT (now stale)
```

---

### Attack 2: Input
**Question**: "What if the input is null / empty / zero / negative / at max value / over max?"

Check for:
- Null/nil dereferences
- Empty collection iteration (`list[0]` on empty list)
- Division by zero
- Integer overflow
- Off-by-one errors (especially with `>` vs `>=`)
- Unbounded input that could exhaust memory or timeout

For every parameter, ask: "What is the valid range? Is it enforced?"

---

### Attack 3: State
**Question**: "What if the system is in an unexpected state when this runs?"

Check for:
- Assumptions about database state that may not hold
- Assumptions about cache being warm/populated
- Assumptions about external service availability
- Missing existence checks before operations
- State machines that allow invalid transitions

---

### Attack 4: Time (TOCTOU)
**Question**: "Is there a gap between when I check a condition and when I act on it?"

The classic pattern:
```
// WRONG
read(state) → [any gap here is a vulnerability] → act on state

// CORRECT
lock → read(state) → act on state → unlock
```
OR (preferred when possible):
```
// Atomic: check-and-act in one database operation
UPDATE table SET status='processing' WHERE id=? AND status='pending'
// Check affected_rows > 0 — if not, someone else got there first
```

---

### Attack 5: Security
**Question**: "If I were a malicious actor, how would I exploit this?"

Check for:
- SQL injection (any string concatenation in queries?)
- Command injection (any user input passed to shell?)
- XSS (any user input reflected to HTML without escaping?)
- Privilege escalation (can a low-privilege user trigger high-privilege code?)
- Secret exposure (any credentials, tokens, or keys in logs, responses, or errors?)
- Mass assignment (any object hydration from untrusted user input?)
- Path traversal (any file operations using user-supplied paths?)

---

### Attack 6: Transactions
**Question**: "What survives a rollback that shouldn't? What gets rolled back that shouldn't?"

Two failure modes:
1. **Audit records disappear**: You create a "failed" audit record inside a transaction that rolls back — the audit record vanishes with it.
2. **Side effects persist**: You sent an email, charged a card, or called an external API inside a transaction that later rolled back — but the real-world effect cannot be undone.

```
// ERROR STATE MUST PERSIST — move outside transaction
try:
    begin_transaction()
        do_work()
    commit()
except Error as e:
    # This must run OUTSIDE the transaction:
    log_failure(e)       ← persists
    mark_failed(record)  ← persists
    raise e
```

---

### Attack 7: The 3am Test
**Question**: "Would I be embarrassed if this broke in production at 3am?"

This is the gut-check that catches what the other six miss. Ask honestly:
- Is there a scenario where this silently produces wrong data?
- Is there a path where this fails without logging anything?
- Is there an assumption in this code that I haven't explicitly tested?
- Does this code depend on anything that could disappear (external service, environment variable, file)?

---

## Output Format

```markdown
### Adversarial Review

**Subject**: [What was reviewed]

| Attack Vector | Result | Notes |
|---|---|---|
| 1. Concurrency | ✅ PASS / ❌ FAIL | [reason] |
| 2. Input | ✅ PASS / ❌ FAIL | [reason] |
| 3. State | ✅ PASS / ❌ FAIL | [reason] |
| 4. TOCTOU | ✅ PASS / ❌ FAIL | [reason] |
| 5. Security | ✅ PASS / ❌ FAIL | [reason] |
| 6. Transactions | ✅ PASS / ❌ FAIL | [reason] |
| 7. The 3am Test | ✅ PASS / ❌ FAIL | [reason] |

**Verdict**: SAFE TO COMMIT / REQUIRES FIXES

**Required Fixes** (if any):
1. [Specific fix with code example]
2. [...]

**Why I might be wrong**: [One honest reason this review could have missed something]
```

---

## The Reviewer's Maxim

*Attacking your own code before committing is not a sign of doubt — it is the highest expression of craftsmanship. The engineer who cannot criticize their own work has stopped growing.*
