---
description: Interactive diagnostic tool using the 7 cognitive levels framework. It verifies if the user's provided evidence matches the stakes of the decision. Use this BEFORE solutioning critical problems to prevent "illusion of competence."
argument-hint: <describe the system or problem to assess>
---

# /assess-depth

Perform an Epistemic Audit on the user's problem description.

## Core Principle: Epistemic Humility
You cannot "guess" a user's internal level of understanding. You can only verify the **evidence provided** in the text. If evidence is missing, you must ask for it.

## Instructions

1. **Classify the Decision Stakes** based on the user's intent:
   - **Low Stakes**: Hotfix / Patch (Requires Level 1-2)
   - **Medium Stakes**: Feature Implementation (Requires Level 3-4)
   - **High Stakes**: Architecture Change (Requires Level 4-5)
   - **Critical Stakes**: Migration / Platform Bet (Requires Level 6-7)

2. **Scan for Explicit Evidence**:
   Analyze the input text *strictly* for these artifacts. Do not infer; look for the actual text.
   - **L1 Ḥissī (Sensory)**: Raw logs, error messages, stack traces, direct observation of behavior.
   - **L2 Khayālī (Mental Model)**: Clear explanation of *how* it works, flow descriptions, reference to diagrams.
   - **L3 Wahmī (Estimation)**: Multiple competing hypotheses, "it looks like X", pattern matching attempts.
   - **L4 'Aqlī (Rational)**: Benchmarks, profiler data, formal logic, reproduction steps, specific metrics.
   - **L5 Fikrī (Reflective)**: Connections to other domains, identifying structural patterns across systems.
   - **L6 Qudsī (Intuitive)**: Articulated "gut feelings" or instincts about *why* the data might be misleading.
   - **L7 Dhawqī (Lived Experience)**: References to past battle-scars, specific production failure modes, or "war stories."

3. **Determine the Gap**:
   - Compare **Highest Proven Level** (what they explicitly showed) vs **Required Level** (what the decision needs).
   - **Constraint**: If `Proven < Required`, you are in **Interview Mode**. Do NOT propose a solution yet.

4. **Generate the Response**:
   - If the gap is significant (e.g., making a Critical decision with only Level 2 evidence), output the Audit and the Interrogation Questions.
   - If the gap is closed, output the Audit and proceed to `/strategize`.

## Output Format

```markdown
### Epistemic Audit

| Dimension | Assessment |
|---|---|
| **Decision Stakes** | [e.g., High (Architecture Change)] |
| **Required Level** | [e.g., Level 5 (Fikrī)] |
| **Evidence Found** | [e.g., Level 2 (Mental Model) - We have a description but no hard data] |
| **Status** | 🔴 **UNSAFE TO PROCEED** / 🟢 **READY** |

### The Evidence Gap
[Briefly explain: "You are attempting a [Stakes] decision, but we only have evidence up to [Level]. We are missing [Missing Artifact]."]

### Interrogation (Required)
[If Status is 🔴, ask 2-3 hard questions to force the user to provide the missing evidence.]

> "To validate this decision at Level 4, I need to know:"
> 1. [Question asking for specific data/benchmarks]
> 2. [Question asking for failure scenarios]
```
