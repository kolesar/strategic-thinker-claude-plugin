---
description: Orchestration layer for advanced reasoning. Applies multi-model synthesis (Hats, Epistemology, Bridges). Use when the standard /strategize analysis yields conflicting results or when the problem requires ethical/philosophical depth.
argument-hint: <problem description> [--hats] [--levels] [--bridges] [--all]
---

# /think-deeper

Execute a Deep Cognition Audit by intersecting multiple reasoning frameworks.

## Core Logic: The "Tension" Engine
True insight comes from the *conflict* between frameworks. Do not just list the outputs of each model. Find where they disagree. (e.g., "The Black Hat (Risk) says stop, but the 'Ijtihād' Bridge (Innovation) says proceed.")

## Instructions

1. **Context Check**:
   - Review conversation history. Has `/strategize` (5 Pillars) been run recently?
   - **Yes**: Summarize its key finding in 1 sentence. Do NOT re-run it.
   - **No**: Run the "Silent Reasoning" version of 5 Pillars first as a baseline.

2. **Select & Execute Frameworks (Silently)**:
   Run the requested models in your scratchpad. Do not output raw lists.
   
   - **`--hats` (Six Thinking Hats)**:
     *Act as the Blue Hat (Manager).* Cycle through White (Data), Red (Emotion), Black (Risk), Yellow (Optimism), Green (Alternatives). 
     *Focus:* What is the emotional/political reality vs. the technical reality?

   - **`--levels` (Islamic Epistemology)**:
     *Act as the Epistemic Auditor.* Check Level 1 (Sensory) through Level 7 (Lived Experience).
     *Focus:* Are we making a Level 5 decision with Level 2 data?

   - **`--bridges` (Classical Synthesis)**:
     *Act as the Translator.* Map the problem to a timeless archetype (e.g., "Al-Kindi's Decomposition").
     *Focus:* What is the timeless version of this modern problem?

   - **`--all` (Grand Synthesis)**:
     Run all three. Output ONLY the "Cross-Framework Synthesis" section.

3. **Analyze Tensions (The Insight Generation)**:
   - Identify **Convergences**: Where do all frameworks agree?
   - Identify **Divergences**: Where does Logic ('Aqlī) fight Intuition (Red Hat)?
   - *Note:* The Divergence is usually the correct place to focus.

## Output Format

```markdown
### Deep Cognition Synthesis

**The Baseline**
[1-sentence summary of the strategic position/5-pillars]

**Framework Tensions (The Conflict)**
* **Conflict 1**: [Framework A] suggests X, while [Framework B] suggests Y.
    * *Resolution*: [How we break the tie]
* **Conflict 2**: [e.g., Data (White Hat) is missing, but Intuition (Level 6) is high]

**Framework Specific Insights**
[Only include if specific flags were used]
* ** Hats**: [The strongest signal, e.g., "Black Hat identified a critical single-point-of-failure"]
* ** Levels**: [The maturity gap, e.g., "We are operating at Level 3 (Guessing)"]
* ** Bridges**: [The timeless principle, e.g., "Apply Al-Kindi's rule: Break it down further"]

**The Wisdom Note**
> [A single, philosophical synthesis statement that elevates the discussion beyond the immediate code. e.g., "Do not let the perfect (Yellow Hat) be the enemy of the shipped (Level 4 validation)."]

**Recommended Path Forward**
[Concrete next step based on the synthesis]
```
