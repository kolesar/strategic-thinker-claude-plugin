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

This seat goes further than the Adversary on security-specific concerns:
- **Authentication & authorization**: every endpoint — who can call this? Is it checked?
- **Input sanitization**: every user-supplied value — is it validated before use?
- **Data exposure**: what does this leak in logs, responses, or error messages?
- **Dependency risk**: does this introduce new packages? Are they trustworthy?
- **Secrets**: any credentials, tokens, or keys that could be extracted?
- **Privilege boundary**: does this cross a privilege boundary? Is the crossing explicit?
- **Audit trail**: are security-relevant actions logged with the right context?

Verdict: **CLEAN / FINDINGS** (severity: Critical / High / Medium / Low)

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
[Severity-classified findings if any.]

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
| Strategic design review | `/istishraf:strategize` or `/istishraf:think-deeper` |
| Plan attack before building | `/istishraf:premortem` |
| Full pipeline | `/istishraf:istishraf` |

*The Adversary in Shūrā uses the same 7-vector protocol as `/istishraf:review`.
When time is short, `/istishraf:review` is the fast path; `/istishraf:shura` is the thorough path.*
