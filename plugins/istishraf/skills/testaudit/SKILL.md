---
name: testaudit
description: >
  Test semantics audit. Distinguishes tests that prove business rules from
  tests that merely exercise code paths for coverage. Applies Islamic
  epistemology cognitive levels (Hissi through Fikri) to classify test
  proof depth. Auto-triggers on: /istishraf:testaudit, "are my tests fake",
  "coverage theater", "do my tests prove", "test quality", "are tests
  meaningful", "business logic coverage", "test intent", "mutation resistant
  tests", "tests just for coverage", or any question about whether a test
  suite actually validates expected behaviour vs. just passing.
---

# Test Audit Skill

*The question is never "do the tests pass?" — it is "what do the tests prove?"*

## Identity

When this skill activates, you read tests the way a business analyst
and a mutation tester would simultaneously — not as a developer checking
syntax, but as someone asking: "If this test passed, what can I guarantee
about the system's behaviour?"

## The Core Distinction

**Coverage**: which lines were executed during the test run.
**Proof**: which business rules are enforced and cannot be violated
without a test failing.

These are different things. A suite can have 100% coverage and prove
almost nothing. The audit makes that visible.

## Classification Framework

Apply the five proof levels to every test:

| Level | Name | What it proves |
|---|---|---|
| L1 | Ḥissī — Coverage Touch | Code ran without exception |
| L2 | Khayālī — Shape | Something was returned |
| L3 | Wahmī — Happy Path | Correct output on one scenario |
| L4 | 'Aqlī — Rule Enforcement | Specific business rule holds at boundaries |
| L5 | Fikrī — Invariant Lock | Rule holds across all scenarios; mutation-resistant |

Target: L4 for all business-critical paths. L5 for core invariants.
L1–L2 are coverage theater and should be flagged.

## Depth Calibration

- Small file (< 10 tests): classify every test individually
- Medium suite (10–50 tests): classify all, group similar theater patterns
- Large suite (50+ tests): sample-based classification, flag the pattern,
  prioritise by business criticality

## Key Output

1. Classification table — level + verdict for each test
2. Theater list — specific tests with rewrites
3. AC coverage map — only if criteria were provided
4. Mutation resistance findings — for L3–4 tests
5. Priority fix list — ordered by business risk

See `commands/testaudit.md` for the full protocol and output format.
