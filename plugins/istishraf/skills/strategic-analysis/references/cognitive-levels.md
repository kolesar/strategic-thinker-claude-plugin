# Islamic Epistemology — Seven Cognitive Levels for Engineering

Maps classical Islamic scholars' hierarchy of understanding onto engineering problem-solving. Levels represent increasing depth of knowledge — use this to assess whether understanding matches the stakes before committing to a solution.

## The Seven Levels

### Level 1: Ḥissī (حسي) — Sensory / Raw Observation
What is directly observable without interpretation. Read the error, trace the log, observe behavior without theorizing.
- *Al-Kindi's empirical starting point*
- **Test**: Can you reproduce the exact behavior right now?

### Level 2: Khayālī (خيالي) — Mental Modeling
Construct an internal model of how the system works. Draw it — gaps in the drawing reveal gaps in understanding.
- *Ibn Tufayl's Hayy learned by constructing mental models through direct experience*
- **Test**: Can you explain the system's data flow without looking at the code?

### Level 3: Wahmī (وهمي) — Estimation / Hypothesis
Pattern-matching from past experience: "This looks like a race condition."
- *Al-Ghazali warned about confusing wahm (estimation) with 'ilm (knowledge)*
- **Test**: Can you name two alternative hypotheses that also fit the evidence?

### Level 4: 'Aqlī (عقلي) — Rational / Proven
Validated hypothesis. Profiling, benchmarking, formal reproduction scripts, specific metrics.
- *Al-Kindi's systematic methodology: decompose, analyze each part, synthesize*
- **Test**: Can you show someone else the evidence and have them reach the same conclusion independently?

### Level 5: Fikrī (فكري) — Reflective / Systemic
Synthesize knowledge across domains to see structural patterns.
- *Ibn Tufayl's protagonist discovered truths by connecting observations across domains*
- **Test**: Can you predict where similar problems appear elsewhere in the system?

### Level 6: Qudsī (قدسي) — Expert Intuition
Deep understanding arriving as clarity, not through stepwise reasoning.
- *Al-Ghazali's knowledge through sustained practice and contemplation*
- **Test**: "What makes you feel that way?" — articulating the instinct reveals hidden pattern recognition.

### Level 7: Dhawqī (ذوقي) — Lived Experience
Knowledge through direct personal experience that transforms the knower.
- *Ibn Tufayl's thesis: some knowledge can only be acquired through experience, not description*
- **Test**: Have you personally operated this system under real failure conditions?

---

## Decision Stakes Mapping

| Decision Type | Minimum Level Required |
|---|---|
| Hotfix / quick patch | Level 1–2 |
| Feature implementation | Level 3–4 |
| Architecture change | Level 4–5 |
| Technology migration | Level 5–6 |
| Platform bet / multi-year | Level 6–7 |

---

## Red Flags

- Making Level 5 decisions with Level 2 understanding → costly rework
- Team claims Level 4 but cannot produce concrete evidence → actually Level 3
- Ignoring Level 6 intuition because "there is no data" → data is not the only form of knowledge
- **The most dangerous gap**: deciding at Level 3 while believing you are at Level 4
