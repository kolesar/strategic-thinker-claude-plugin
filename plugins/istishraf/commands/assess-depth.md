---
description: >
  Interactive epistemic audit. Verifies that the depth of understanding matches
  the stakes of the decision using the 7-level cognitive framework. Runs BEFORE
  solution proposals to prevent "illusion of competence" — the most common cause
  of costly rework. Use this when you suspect someone is making a high-stakes
  decision with insufficient evidence.
argument-hint: <describe the system or decision to assess>
---

> **WHEN**: You are about to make a significant decision and want to verify your understanding is deep enough.
> **GET**: A cognitive level (1–7) verdict: safe to proceed, or specific questions you must answer first.
> **NOT FOR**: A pre-implementation gate — /istishraf:istishraf Phase 0 runs this automatically in the full pipeline.

# /assess-depth

> ⚡ **`opusplan` users**: This command is pure epistemic reasoning.
> Enter plan mode first with **Shift+Tab** so it runs on Opus.

Perform an **Epistemic Audit** on the problem description.

*"You cannot prescribe medicine without a diagnosis. You cannot architect without understanding."*

---

## Core Principle

You cannot infer a user's internal understanding. You can only verify the **evidence present in the text**. If evidence is missing, ask for it. Do not proceed to solutions in the absence of evidence commensurate with the stakes.

---

## Step 1: Classify Decision Stakes

| Stakes | Examples | Required Level |
|---|---|---|
| **Low** | Hotfix, rename, config change | Level 1–2 |
| **Medium** | Feature implementation, new endpoint | Level 3–4 |
| **High** | Architecture change, schema migration | Level 4–5 |
| **Critical** | Platform migration, multi-year bet | Level 6–7 |

---

## Step 2: Scan for Evidence

Search the input text **strictly** for these artifacts. Do not infer; look for actual evidence.

| Level | Name | Valid Evidence Indicators |
|---|---|---|
| L1 | **Ḥissī** (Sensory) | Raw logs, stack traces, error messages, direct observed behavior |
| L2 | **Khayālī** (Mental Model) | Explanation of data flow, architecture description, sequence diagram |
| L3 | **Wahmī** (Hypothesis) | "I suspect...", "It looks like...", multiple competing explanations |
| L4 | **'Aqlī** (Rational/Proven) | Profiler data, benchmarks, reproduction script, specific metrics |
| L5 | **Fikrī** (Systemic) | Connections across subsystems, pattern identified in multiple locations |
| L6 | **Qudsī** (Expert Intuition) | Articulated instinct with partial reasoning — "feels wrong because..." |
| L7 | **Dhawqī** (Lived Experience) | Production failure stories, operated this system under real conditions |

---

## Step 3: Determine the Gap

- **Proven Level** = Highest level with actual evidence in the text
- **Required Level** = Based on decision stakes
- If `Proven < Required` → **Interview Mode**: do not propose solutions until evidence gap is closed

---

## Output Format

```markdown
## [ASSESS-DEPTH MODE]

### Epistemic Audit

| Dimension | Assessment |
|---|---|
| **Decision Stakes** | [e.g., High — Architecture Change] |
| **Required Level** | [e.g., Level 4–5 ('Aqlī + Fikrī)] |
| **Evidence Found** | [e.g., Level 2–3 — mental model + hypothesis, no measurement data] |
| **Status** | 🔴 UNSAFE TO PROCEED / 🟡 PROCEED WITH CAUTION / 🟢 READY |

### The Evidence Gap
[Explain: "You are making a [Stakes] decision, but the evidence reaches only [Level]. 
Missing: [specific artifact — benchmark, reproduction script, failure log, etc.]"]

### Interrogation *(if status is 🔴 or 🟡)*
To raise confidence to the required level, I need:

1. [Question requesting specific measurement or artifact]
2. [Question asking for failure scenarios or production behavior]
3. [Question testing whether the hypothesis has been validated or is still assumed]

*(Once you provide this evidence, I will proceed to `/istishraf:strategize`.)*
```

---

## The Auditor's Reminder

The most dangerous decisions are made at Level 3 (Estimation) while *believing* you are at Level 4 (Rational). The engineer who says "I know it's a race condition" without a reproduction script is at Level 3. The engineer who says "I suspect it's a race condition, and here is the profiler trace showing two workers entering the lock-free zone simultaneously" is at Level 4.

*The gap between Level 3 and Level 4 is the difference between a hypothesis and a fact. Architecture decisions require facts.*
