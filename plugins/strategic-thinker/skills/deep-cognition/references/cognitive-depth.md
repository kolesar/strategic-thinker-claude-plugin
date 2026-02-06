# Cognitive Depth Assessment — Applied Protocol

## Assessment Steps

For a given problem, walk through levels honestly:

### Level 1 — Hissī (Surface): "We see the symptom"
We read the error, see the red dashboard, notice the latency spike.
- Danger: Fixes at this level treat symptoms only.

### Level 2 — Khayālī (Model): "We can draw it"
We have a mental model of the architecture.
- Test: Draw the system on a whiteboard without looking at code. Gaps in the drawing = gaps in understanding.

### Level 3 — Wahmī (Hypothesis): "We think we know why"
"It is probably the database connection pool" — intuitive estimation.
- Danger: This is where most engineering mistakes happen — acting on hypotheses as proven facts.

### Level 4 — 'Aqlī (Proven): "We can prove it"
Profiling data, reproduction steps, formal analysis.
- Test: Show someone else the evidence. Can they independently reach the same conclusion?

### Level 5 — Fikrī (Connected): "We see the bigger pattern"
Why this class of problem occurs and how it connects to other system properties.
- Test: Can you predict where similar problems will appear elsewhere?

### Level 6 — Qudsī (Intuitive): "We just know"
The senior engineer who "just knows" the system will break.
- Capture this by asking "what makes you say that?" — the reasoning reveals invisible patterns.

### Level 7 — Dhawqī (Lived): "We have been through this"
Knowledge through direct operational experience — operating systems at scale under failure.
- You cannot fully understand production from documentation.

## Assessment Output Template

```
PROBLEM: [Description]
CURRENT LEVEL: [Number and name]
EVIDENCE: [What demonstrates this level vs. a lower one]
DECISION BEING CONSIDERED: [What we want to do]
REQUIRED LEVEL: [Minimum for this decision type]
GAP: [What must happen before deciding]
```

## Red Flag: The Level 3 Trap
The most dangerous engineering decisions are made at Level 3 (estimation) while believing one is at Level 4 (analysis). This maps directly to Al-Ghazali's critique: confusing opinion with knowledge.
