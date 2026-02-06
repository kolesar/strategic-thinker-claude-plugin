# Islamic Epistemology — Seven Cognitive Levels for Engineering

Maps classical Islamic scholars' hierarchy of understanding onto engineering problem-solving. The levels represent increasing depth — use this to assess whether understanding matches the stakes before committing to a solution.

## Level 1: Hissī (Sensory) — Surface Observation
What is directly observable without interpretation. Read the error, trace the log, observe behavior without theorizing.
- Al-Kindi's insistence on empirical starting points
- **Test**: Can you reproduce the exact behavior?

## Level 2: Khayālī (Imagination) — Mental Modeling
Construct an internal model of how the system works. Draw it — gaps in the drawing reveal gaps in understanding.
- Ibn Tufayl's Hayy learned by constructing mental models through direct experience
- **Test**: Can you explain the system without looking at the code?

## Level 3: Wahmī (Estimation) — Forming Hypotheses
Pattern-matching from past experience: "This looks like a race condition."
- Al-Ghazali warned about confusing wahm (estimation) with 'ilm (knowledge)
- **Test**: Can you name two alternative hypotheses that also fit the evidence?

## Level 4: 'Aqlī (Rational) — Logical Analysis
Structured analysis to validate or refute hypotheses. Profiling, benchmarking, formal verification.
- Al-Kindi's systematic methodology: break, analyze each part, synthesize
- **Test**: Can you show someone else the evidence and have them reach the same conclusion?

## Level 5: Fikrī (Reflective) — Cross-Domain Connection
Synthesize knowledge from multiple domains to see deeper patterns.
- Ibn Tufayl's protagonist discovered truths by connecting observations across domains
- **Test**: Can you predict where similar problems appear elsewhere in the system?

## Level 6: Qudsī (Intuitive) — Expert Intuition
Deep understanding arriving as clarity, not through stepwise reasoning.
- Al-Ghazali's knowledge through sustained practice and contemplation
- **Test**: Ask "what makes you feel that way?" — the articulation reveals hidden patterns.

## Level 7: Dhawqī (Lived Experience) — Direct Knowledge
Knowledge through direct personal experience that transforms the knower.
- Ibn Tufayl's thesis: some knowledge can only be acquired through experience, not description
- **Test**: Have we operated this system under real failure conditions?

## Decision Mapping

| Decision Type | Minimum Level |
|---|---|
| Hotfix / quick patch | Level 1-2 |
| Feature implementation | Level 3-4 |
| Architecture change | Level 4-5 |
| Technology migration | Level 5-6 |
| Platform bet / multi-year | Level 6-7 |

## Red Flags
- Making Level 5 decisions with Level 2 understanding → costly rework
- Team claims Level 4 but cannot produce evidence → actually Level 3
- Ignoring Level 6 intuition because "there is no data" → data is not the only knowledge
- The most dangerous decisions are made at Level 3 while believing one is at Level 4
