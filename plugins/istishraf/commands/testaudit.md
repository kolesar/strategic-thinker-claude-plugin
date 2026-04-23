---
description: >
  Test semantics audit. Distinguishes tests that prove business rules from
  tests that merely exercise code paths (coverage theater). Applies the
  Islamic Epistemology cognitive levels to each test to classify its depth
  of proof, checks mutation resistance, and identifies which acceptance
  criteria or invariants have no meaningful test coverage. Accepts @file
  references, directory scopes, and "staged" keyword.
argument-hint: >
  [test files or @paths or "staged"] [--ac "acceptance criteria text"]
  [--rules "business rules"] [--all]
---

# /istishraf:testaudit — Test Semantics Audit

> ⚡ **`opusplan` users**: Enter plan mode first (**Shift+Tab**) — this is
> deep analytical reasoning, not code generation.

*"A test that passes is not the same as a test that proves."*

Coverage tells you which lines were touched. This audit tells you which
**business rules are actually enforced** — and which ones only appear to be.

---

## Step 1: Resolve the Test Scope

Priority order:
1. Explicit `@file` or `@dir` references in the prompt
2. `"staged"` keyword → `git diff --staged` filtered to test files
3. `"recent"` keyword → `git diff HEAD~1` filtered to test files
4. No scope → ask: *"Which test files should I audit?"*

Show the resolved list before starting. If business rules or acceptance
criteria were provided via `--ac` or `--rules`, keep them visible throughout.

---

## Step 2: Classify Each Test by Cognitive Level

For every test (or test group for large suites), apply the epistemology
framework to classify the *depth of proof* the test provides.

### The Five Levels of Test Proof

**Level 1 — Ḥissī (Coverage Touch)**
The test executes the code but asserts nothing specific about business
behaviour. It will pass even if the function returns a wrong value.
```
# Smell: no meaningful assertion, or assert result is not None
def test_process_payment():
    result = process_payment(order)
    assert result  # ← what does "truthy" prove?
```
*Verdict: Coverage theater. Proves nothing about the business rule.*

---

**Level 2 — Khayālī (Shape Assertion)**
The test checks that *something changed*, but not whether the right
thing changed. It would survive many mutations.
```
# Smell: checks type or existence, not specific value
assert isinstance(result, PaymentResult)
assert result.status is not None
```
*Verdict: Weak. Confirms an object was returned; proves nothing about correctness.*

---

**Level 3 — Wahmī (Happy Path)**
The test checks a specific value on the success path but does not
test the rule's boundaries or failure modes.
```
# Better — but only tests one scenario
assert result.status == "approved"
assert result.amount == 100.00
```
*Verdict: Partial. Proves the happy path; leaves boundary rules unverified.*

---

**Level 4 — 'Aqlī (Rule Enforcement)**
The test explicitly encodes a business rule and would fail if that
rule were removed or weakened. Includes boundary conditions.
```
# Good — tests the rule, not just the outcome
def test_payment_rejected_when_balance_insufficient():
    order = Order(amount=Decimal("100.01"))
    account = Account(balance=Decimal("100.00"))
    result = process_payment(order, account)
    assert result.status == "rejected"
    assert result.reason == "insufficient_funds"
    assert account.balance == Decimal("100.00")  # no deduction
```
*Verdict: Strong. Would catch a mutation that weakened the >= check.*

---

**Level 5 — Fikrī (Invariant Lock)**
The test encodes a system invariant that must hold regardless of
implementation. Mutation-resistant: changing internal logic cannot
make this test pass incorrectly.
```
# Excellent — locks the invariant across all scenarios
@pytest.mark.parametrize("amount,balance,expected", [
    (Decimal("100.00"), Decimal("100.00"), "approved"),   # exact boundary
    (Decimal("100.01"), Decimal("100.00"), "rejected"),   # just over
    (Decimal("0.01"),   Decimal("100.00"), "approved"),   # minimal valid
    (Decimal("0.00"),   Decimal("100.00"), "rejected"),   # zero amount invalid
])
def test_payment_approval_invariant(amount, balance, expected):
    result = process_payment(Order(amount), Account(balance))
    assert result.status == expected
```
*Verdict: Excellent. Locks the boundary rule. Cannot pass with a wrong implementation.*

---

## Step 3: The Coverage Theater Scan

Identify tests that exist primarily to raise coverage numbers rather
than to prove behaviour. Red flags:

- **The Existence Test**: only checks the function returns without exception
- **The Type Test**: only checks `isinstance()` or `is not None`
- **The Echo Test**: asserts output equals input with no transformation logic
- **The Mock Tautology**: mocks the function being tested, then asserts the mock was called
- **The Partial Snapshot**: snapshots a large object but only because it was easy, not because the snapshot encodes a rule
- **The Always-True Boundary**: tests a boundary that cannot fail given the implementation (e.g., testing `> 0` with value `5` when `5` is hardcoded)

For each theater test found, identify:
1. What business rule it *should* be proving
2. What change to the implementation would make it fail correctly

---

## Step 4: Acceptance Criteria Coverage Map

If `--ac` or `--rules` was provided, map each criterion to the tests
that cover it, and classify the coverage depth:

```
AC: "A payment must be rejected if account balance is insufficient"
  → test_payment_rejected_when_balance_insufficient  [Level 4 ✔]
  → test_process_payment                             [Level 1 — does NOT cover this rule]

AC: "A payment of zero amount must always be rejected"
  → (no test found)                                  [UNCOVERED ✗]

AC: "Approved payments must deduct the exact amount from balance"
  → test_payment_approved_amount_deducted            [Level 3 — partial]
     Missing: boundary test for floating-point precision
```

---

## Step 5: Mutation Resistance Check

For the Level 3–4 tests, ask: **if I mutated this code, would the test catch it?**

Common mutations to check mentally:
- Change `>=` to `>` (or vice versa) — does a boundary test catch it?
- Change `==` to `is` — does a type-sensitive assertion catch it?
- Remove a side effect (e.g., don't deduct the balance) — does the test assert the side effect?
- Return a hardcoded value — does the test use varied inputs?
- Swap two similar status strings — does the test check the exact string?

If a mutation would not be caught, the test is weaker than it appears.

---

## Output Format

```markdown
## [TESTAUDIT MODE]

**Scope**: [test files audited]
**Business rules provided**: [yes — from --ac/--rules | no — inferred from test names]

---

### Test Classification

| Test | Level | Verdict | Business Rule Covered |
|---|---|---|---|
| `test_process_payment` | L1 — Ḥissī | ⚠️ Theater | "payment processing works" (too vague) |
| `test_payment_rejected_insufficient` | L4 — 'Aqlī | ✅ Strong | Balance < amount → rejected |
| `test_payment_approved` | L3 — Wahmī | 🔶 Partial | Happy path only |

---

### Coverage Theater Found

**[test name]**
- *What it actually proves*: [almost nothing]
- *What it should prove*: [the specific business rule]
- *How to fix*: [concrete rewrite suggestion]

---

### Acceptance Criteria Coverage *(if --ac provided)*

| Criterion | Tests | Depth |
|---|---|---|
| [AC text] | [test name] | L4 ✅ |
| [AC text] | [test name] | L2 ⚠️ weak |
| [AC text] | (none) | ❌ uncovered |

---

### Mutation Resistance Findings

**[test name]**: Would survive mutation of [specific operator/value].
Fix: add `[specific assertion]` to lock the boundary.

---

### Summary

| Metric | Count |
|---|---|
| Tests audited | N |
| Level 4–5 (proving rules) | N |
| Level 3 (partial) | N |
| Level 1–2 (theater) | N |
| Uncovered acceptance criteria | N |

**Overall verdict**: TRUSTWORTHY / PARTIALLY TRUSTWORTHY / COVERAGE THEATER

---

### Priority Fixes

1. **[Most critical]**: [Specific test to rewrite + how]
2. **[Second]**: [...]
3. **[Third]**: [...]

> **Wisdom note**: [One sentence on what this audit revealed about the
> relationship between the test suite and the actual business rules.]
```

---

## Usage Examples

```bash
# Audit all staged test changes
/istishraf:testaudit staged

# Audit a specific test file
/istishraf:testaudit @tests/payments/test_processor.py

# Audit with acceptance criteria for deeper mapping
/istishraf:testaudit @tests/payments/ --ac "Payments over account balance must be rejected. Zero-amount payments are invalid. Approved payments deduct exactly the payment amount."

# Full audit of a directory with known business rules
/istishraf:testaudit @tests/ --rules "Balance invariant: account.balance never goes negative. Idempotency: processing the same payment twice has no additional effect."

# After /istishraf:implement — verify the tests you just wrote
/istishraf:testaudit staged --ac "paste acceptance criteria from the ticket here"
```
