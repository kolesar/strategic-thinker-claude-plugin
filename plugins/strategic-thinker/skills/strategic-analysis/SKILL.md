---
name: strategic-analysis
description: Apply structured strategic thinking before implementation. Use when Claude faces architectural decisions, system design, refactoring, build-vs-buy choices, debugging complex issues, planning migrations, writing technical proposals, or any task where jumping straight to code would be premature. Triggers on requests involving "design", "architect", "plan", "strategy", "evaluate", "decide", "trade-off", "should we", "how should", "what approach", "refactor", or when the problem has multiple valid solutions. Also use when the user explicitly asks to "think strategically" or invokes /strategize.
---

# Strategic Analysis Framework

Apply all 5 lenses sequentially before proposing a solution. Spend genuine reasoning effort on each — do not treat them as a checklist to rush through.

## Lens 1: Pattern Recognition

Identify what is actually happening before deciding what to do.

- What is the underlying trend in this codebase / system / problem?
- Why does this problem keep recurring or why has it emerged now?
- What happens next if the current trajectory continues unchanged?
- Are there patterns from other projects, domains, or historical precedents that map to this situation?

Output a brief **Patterns Observed** section summarizing findings.

## Lens 2: Root-Cause Questioning

Challenge the framing of the problem itself.

- What evidence do we actually have (vs. assumptions)?
- What is the *real* problem we are trying to solve — not the symptom?
- What would need to change if we solved the root cause instead of patching?
- If starting from zero with no legacy constraints, how would we design this?

Output a brief **Reframed Problem** section that may differ from the original ask.

## Lens 3: Resource & Impact Analysis

Understand the economics before committing resources.

- Where does the real value come from in this system / feature / decision?
- What are the unit economics — cost per request, per user, per deployment?
- How sustainable is the current approach at 2x, 10x, 100x scale?
- What metrics actually matter for this decision? Latency? Cost? Developer velocity? Reliability?

Output a brief **Resource Reality** section with concrete numbers where possible.

## Lens 4: Hands-On Validation

Bias toward experiments over speculation.

- What is the smallest experiment that validates or invalidates the proposed approach?
- What can we learn by building a quick prototype or spike?
- How will we measure whether the solution actually works? Define success criteria.
- What is the fastest path to real-world feedback?

Output a brief **Validation Plan** section with a concrete next step.

## Lens 5: Clear Communication

Ensure the strategy can be understood and acted upon by others.

- What is the ONE thing the reader must take away from this analysis?
- How do we make this recommendation memorable and defensible?
- Can we show this visually — a diagram, a comparison table, a decision tree?
- Who is the audience and what level of detail do they need?

Output a brief **Key Message** section and offer to generate a visual artifact if appropriate.

## Synthesis

After applying all 5 lenses, produce:

1. **Strategic Recommendation** — a clear, concise position
2. **Confidence Level** — high / medium / low, with reasoning
3. **Key Risks** — what could go wrong and how to mitigate
4. **Immediate Next Action** — the single most important thing to do right now

## Adaptive Depth

Match analysis depth to the stakes:

- **Quick decisions** (naming, small refactors, tool choices): Apply lenses briefly, 1-2 sentences each
- **Medium decisions** (API design, library selection, feature architecture): Full lens treatment, paragraph each
- **High-stakes decisions** (migrations, infrastructure changes, system redesigns): Deep analysis, include diagrams and reference data

## Extended Frameworks (Experimental)

For deeper analysis, extended cognitive frameworks are available:

- **De Bono Six Thinking Hats**: See `references/six-hats.md` — Use for team-oriented decisions where multiple perspectives need explicit separation
- **Islamic Epistemology Cognitive Levels**: See `references/cognitive-levels.md` — Use for problems requiring depth-of-understanding analysis
- **Bridges Synthesis**: See `references/bridges-synthesis.md` — Use for connecting classical wisdom patterns with modern engineering problems

Invoke extended frameworks when:
- The 5 core lenses reveal conflicting signals
- The problem has philosophical or ethical dimensions
- The user explicitly requests deeper analysis or uses /think-deeper
