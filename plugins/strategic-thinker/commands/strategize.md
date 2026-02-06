---
description: Apply the 5-pillar strategic analysis framework. Focuses on 'Silent Reasoning' to produce a high-impact synthesis rather than a long report. Use for architectural decisions, refactoring, or complex debugging.
argument-hint: <describe the decision or problem to analyze>
---

# /strategize

Apply the 5-Lens Strategic Framework to the user's problem.

## Instructions

1. **Ingest Context**: Read the problem description from $ARGUMENTS.

2. **Execute Silent Reasoning (The 5 Lenses)**:
   Do not output these sections individually unless requested. Use them to filter your final recommendation.

   * **Lens 1: Pattern Recognition (History & Trend)**
       * Is this a recurring pattern? (e.g., "We fixed this twice before.")
       * Is this a standard industry problem with a standard solution (Golden Path)?
       * *Constraint:* Do not invent patterns if none exist.

   * **Lens 2: Root-Cause Inversion**
       * Invert the problem: What happens if we do *nothing*?
       * Are we solving a symptom (e.g., "optimize query") or the cause (e.g., "bad schema")?

   * **Lens 3: Resource Reality (The Equation, Not the Number)**
       * **CRITICAL**: Do NOT guess costs or latency numbers. 
       * Identify the **Variables**: What drives cost/complexity here? (e.g., "This solution scales linearly with *User Count* but exponentially with *Data Interaction*").

   * **Lens 4: The $100 Experiment**
       * What is the cheapest way to prove us wrong?
       * Draft a validation step that takes < 4 hours.

   * **Lens 5: Artifact Strategy**
       * Can this be explained better with a Diagram? (Sequence, Flowchart, C4).
       * If yes, prepare a `mermaid` block for the final output.

3. **Synthesize**: Compile the reasoning into the final output format. 
   * *Tone:* Decisive, Engineering-Manager level, High-Signal.

## Output Format

```markdown
### Strategic Synthesis

**Recommendation**
[Direct, 1-sentence position statement. E.g., "Do not build X; leverage Y instead."]

**Confidence Score**: [Low/Med/High]
*Reasoning: [One bullet point explaining the confidence level based on missing/present data]*

**The "Variable" Analysis**
[From Lens 3: Identify the trade-off formula. E.g., "We are trading *Development Speed* for *Runtime Cost*."]

** Key Risks (Pre-Mortem)**
* [Risk 1: What breaks first?]
* [Risk 2: The "Unknown Unknown"]

** Validation Plan (The $100 Experiment)**
[From Lens 4: The immediate next step to verify the recommendation.]

### Visual Model (Optional)
[If Lens 5 indicated a diagram is needed, render the Mermaid code here.]
```
