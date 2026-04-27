---
description: >
  Shūrā (شورى — consultation). Convenes a council of distinct reviewer
  personas, each examining the code from a different professional angle.
  Use after implementation, before committing, or when you want a richer
  review than a single adversarial pass. Accepts @file references, directory
  scopes, and git-staged file filters.
argument-hint: >
  [files or @paths or "staged"] [--architect] [--adversary] [--maintainer]
  [--ops] [--product] [--security] [--all]
---

> **WHEN**: You have staged changes or a branch ready for a PR and want a thorough multi-perspective review.
> **GET**: 3–7 council seat verdicts with a synthesis, must-fix list, and a wisdom note.
> **NOT FOR**: A quick pre-commit check — use /istishraf:review for that. Shura is for before you open the PR.

# /shura — شورى (Consultation)

*"Matters are decided by consultation among those who know."*

A council of distinct reviewer personas examines the code. Each sees
what the others miss. The synthesis is the verdict.

Prefix response with `## [SHURA MODE]`.

> ⚡ **`opusplan` users**: Enter plan mode first (**Shift+Tab**) — this is
> deep analytical reasoning, not code generation.

---

## Resolving the Code Scope

Before convening, determine what to review:

1. **Explicit paths** — `@src/payments/` or specific files passed as arguments
2. **"staged" keyword** — run `git diff --staged` to get staged changes
3. **"recent" keyword** — run `git diff HEAD~1` or `git log --oneline -5` then diff
4. **No argument** — ask: *"Which files should the council review?"*

For staged files, always show the list first:
```bash
git diff --staged --name-only
```
Confirm with the user before proceeding if the list is large (> 10 files).

---

## The Council Seats

Default seats (no flags): **Architect + Adversary + Maintainer**
Use `--all` to convene the full council.

---

### 🏛️ The Architect (`--architect`)
*Structural integrity and long-term system health.*

Questions this seat asks:
- Does this follow the established patterns in the codebase?
- Does this introduce coupling where there should be none?
- Are there abstractions that should be created, or existing ones being bypassed?
- Does this move the codebase towards or away from its intended architecture?
- Is there a simpler design that achieves the same goal?
- Where will this break when requirements change next quarter?

Verdict: **SOUND / CONCERN / REFACTOR REQUIRED**

---

### ⚔️ The Adversary (`--adversary`)
*Attack the code before production does.*

Applies the full 7-vector adversarial protocol (see `review.md`):
concurrency, input, state, TOCTOU, security, transactions, the 3am test.

This seat is not a gentle critic. It is looking for failure modes.

Verdict: **SAFE / FIX REQUIRED** (with specific fixes)

---

### 🔧 The Maintainer (`--maintainer`)
*Can a developer who didn't write this maintain it safely in 6 months?*

Questions this seat asks:
- Can the intent of this code be understood without reading the ticket?
- Are there implicit assumptions that aren't documented?
- Are variable and function names honest about what they do?
- Is the error handling informative enough to debug from logs alone?
- Are there any "clever" constructs that should be made obvious instead?
- What is the bus factor of this change? (Does it require the author to explain it?)
- Would a junior developer accidentally break an invariant by modifying this?

Verdict: **CLEAR / NEEDS COMMENTS / NEEDS SIMPLIFICATION**

---

### 🖥️ The Operator (`--ops`)
*Can this be deployed, monitored, and debugged in production?*

Questions this seat asks:
- Is there sufficient logging to reconstruct what happened during a failure?
- Are there metrics or traces that would let an on-call engineer diagnose issues?
- Does the deployment path have rollback capability?
- Are there any new failure modes that don't alert?
- Does this introduce new external dependencies or single points of failure?
- What happens to in-flight requests during a deploy?
- Is there a feature flag if this needs to be disabled quickly?

Verdict: **OPERABLE / NEEDS OBSERVABILITY / DEPLOYMENT RISK**

---

### 📦 The Product Guardian (`--product`)
*Does this actually deliver what was intended?*

Questions this seat asks:
- Does the implementation match the acceptance criteria on the ticket?
- Are there edge cases in the requirements that aren't handled?
- Could a user reach an unexpected state through normal usage?
- Is the error UX acceptable — or will users see raw stack traces?
- Does this change anything that existing users depend on?
- Is the scope right — neither gold-plated nor incomplete?

This seat requires a ticket or acceptance criteria to be effective.
If none was provided, flag it: *"No acceptance criteria found — product review
is limited to surface-level observations."*

Verdict: **COMPLETE / GAPS FOUND / OUT OF SCOPE**

---

### 🔐 The Security Auditor (`--security`)
*Dedicated deep security analysis — beyond the Adversary's pass.*

This seat applies two layers of analysis. The first is Anderson's
4-element framework from *Security Engineering* — the four root causes
of every security failure in history. The second is implementation-level
checks that the Adversary seat does not cover in depth.

#### Layer 1 — Anderson's 4-Element Framework

Ask these four questions about the system or component being reviewed:

- **Policy**: Is there an explicit, testable statement of what this
  component is supposed to prevent? If the security goal cannot be
  stated in one sentence, the implementation cannot enforce it.
  *"Authenticated users only" is a policy. "Secure" is not.*

- **Mechanism**: Does the implementation actually enforce the policy?
  Is the mechanism complete — does it cover all paths, not just the
  happy path? Does it remain enforced under error conditions, timeouts,
  and retries?

- **Assurance**: What is the evidence that the mechanism works?
  Is there a test that would fail if the mechanism were removed?
  Has it been reviewed by someone other than the author?
  Is the assurance proportionate to the risk — or is high-risk logic
  covered by an existence test?

- **Incentives**: Are the people responsible for this security
  actually motivated to maintain it? Does the system make the secure
  path the easy path — or does it require extra effort that people
  will skip under pressure? If a developer bypasses this control,
  will anyone notice?

#### Layer 2 — Implementation Checks

- **Authentication & authorization**: every entry point — who can call
  this? Is it checked on every path, including error paths?
- **Input sanitization**: every user-supplied value — validated before
  use, rejected with a clear error, never silently truncated?
- **Data exposure**: what does this leak in logs, responses, or errors?
  Does the error message reveal internal structure to an attacker?
- **Dependency risk**: new packages introduced? Are they maintained,
  well-audited, and minimal in scope?
- **Secrets**: any credentials, tokens, or keys that could be extracted
  from source, logs, responses, or environment variables?
- **Privilege boundary**: does this cross a privilege boundary?
  Is the crossing explicit, minimal, and logged?
- **Audit trail**: are security-relevant actions logged with enough
  context to reconstruct an incident — who, what, when, outcome?

#### The Threat Model Check

Before issuing a verdict, ask: *who is the realistic opponent here?*

- **Opportunistic attacker** (automated scanners, script kiddies):
  Is the obvious attack surface closed?
- **Motivated outsider** (targeted attacker with time):
  Are there chained vulnerabilities that require effort but lead to
  significant impact?
- **Malicious insider** (someone with legitimate access):
  Can a legitimate user escalate privileges or exfiltrate data?
  Would the audit trail catch it?

A finding that blocks an opportunistic attacker is Low severity.
A finding that enables a motivated outsider is High.
A finding that enables a malicious insider with no audit trail is Critical.

Verdict: **CLEAN / FINDINGS** (severity: Critical / High / Medium / Low)

---

### 🧪 The Test Semanticist (`--tests`)
*Do the tests prove the business rules, or just exercise the code?*

This seat reads tests the way a business analyst would — not checking
syntax or coverage, but asking what each test actually *guarantees*.

Applies the epistemology proof levels to the test suite in scope:
- **L1–2 (theater)**: executes code or checks shape — proves nothing about rules
- **L3 (partial)**: happy path only — boundary rules unverified
- **L4 (rule enforcement)**: specific business rule encoded at its boundary
- **L5 (invariant lock)**: mutation-resistant, holds across all scenarios

Specific checks:
- Are there tests that exist only to raise coverage numbers?
- Would any of these tests pass even if the business rule were removed?
- Are boundary conditions (the exact threshold, the off-by-one) tested?
- Are side effects (balance deductions, audit logs, state changes) asserted — or just the return value?
- If acceptance criteria are visible, is each criterion backed by at least one L4 test?

This seat does **not** do a full `/istishraf:testaudit` — for a dedicated
deep audit of the entire test suite, use that command directly.

Verdict: **TRUSTWORTHY / PARTIALLY TRUSTWORTHY / COVERAGE THEATER**

---

## Output Format

```markdown
## [SHURA MODE] — Council Review

**Scope**: [files reviewed, e.g. "4 staged files: payments/processor.ts, ..."]
**Seats convened**: [list of active seats]

---

### 🏛️ Architect
**Verdict**: SOUND / CONCERN / REFACTOR REQUIRED
[2–4 sentences. Specific. References actual code constructs.]

---

### ⚔️ Adversary
**Verdict**: SAFE / FIX REQUIRED
[Attack findings. Use the 7-vector format from review.md if FIX REQUIRED.]

---

### 🔧 Maintainer
**Verdict**: CLEAR / NEEDS COMMENTS / NEEDS SIMPLIFICATION
[Specific observations. Line numbers or function names where relevant.]

---

### 🖥️ Operator *(if --ops)*
**Verdict**: OPERABLE / NEEDS OBSERVABILITY / DEPLOYMENT RISK
[...]

---

### 📦 Product Guardian *(if --product)*
**Verdict**: COMPLETE / GAPS FOUND / OUT OF SCOPE
[...]

---

### 🔐 Security Auditor *(if --security)*
**Verdict**: CLEAN / FINDINGS

**Anderson Framework**:
- Policy: [explicit and testable? or vague?]
- Mechanism: [enforces policy on all paths including errors?]
- Assurance: [evidence the mechanism works?]
- Incentives: [secure path is the easy path?]

**Threat Model**: [realistic opponent for this component]

**Implementation Findings** *(if any)*:
- [severity]: [specific finding with location]

---

### 🧪 Test Semanticist *(if --tests)*
**Verdict**: TRUSTWORTHY / PARTIALLY TRUSTWORTHY / COVERAGE THEATER
[Classification table: test name | level | what it proves | verdict]
[Theater tests found with rewrite suggestions.]
[Uncovered rules if AC was provided.]

---

### شورى Synthesis

**Overall verdict**: APPROVE / APPROVE WITH CONDITIONS / REWORK REQUIRED

**Must-fix before merge** *(blocking)*:
- [If any — specific, actionable]

**Should-fix soon** *(non-blocking)*:
- [If any]

**Observations for the future** *(not blocking)*:
- [Architectural notes, tech debt flags, patterns to watch]

> **Wisdom note**: [One sentence that captures the most important
> insight from the council — the thing worth remembering beyond this PR.]
```

---

## Usage Examples

```bash
# Review staged files, default seats (Architect + Adversary + Maintainer)
/istishraf:shura staged

# Full council on a specific directory
/istishraf:shura @src/payments/ --all

# Security-focused review of recent changes
/istishraf:shura staged --security --adversary

# After /implement — final check before PR
/istishraf:shura staged --all

# Review specific files with product context
/istishraf:shura @src/checkout/order.ts — ticket PROJ-89 acceptance criteria: [paste AC]
```

---

## Relationship to Other Commands

| Need | Command |
|---|---|
| Quick safety check before commit | `/istishraf:review` (Adversary only, fast) |
| Full multi-perspective review | `/istishraf:shura` (council, thorough) |
| Are my tests proving the rules? | `/istishraf:testaudit` (dedicated deep audit) |
| Quick test quality check (mixed PR) | `/istishraf:shura staged --tests` |
| Strategic design review | `/istishraf:strategize` or `/istishraf:think-deeper` |
| Plan attack before building | `/istishraf:premortem` |
| Full pipeline | `/istishraf:istishraf` |

*The Adversary in Shūrā uses the same 7-vector protocol as `/istishraf:review`.
When time is short, `/istishraf:review` is the fast path; `/istishraf:shura` is the thorough path.*
