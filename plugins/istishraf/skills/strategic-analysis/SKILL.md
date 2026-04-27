---
name: strategic-analysis
description: >
  Apply structured strategic thinking before implementation. Use when facing
  architectural decisions, system design, refactoring, build-vs-buy choices,
  debugging complex issues, planning migrations, writing technical proposals,
  or any task where jumping straight to code would be premature. Triggers on:
  "design", "architect", "plan", "strategy", "evaluate", "decide", "trade-off",
  "should we", "how should", "what approach", "refactor", team dynamics questions,
  or when the problem has multiple valid solutions. Also activates on /strategize.
---

# Strategic Analysis Framework

Apply all 5 lenses sequentially before proposing any solution. Each lens is a genuine reasoning step, not a checkbox.

## Lens 1: Pattern Recognition

Identify what is actually happening before deciding what to do.

- What is the underlying trend in this codebase / system / problem?
- Why does this problem keep recurring, or why has it emerged now?
- What happens next if the current trajectory continues unchanged?
- Are there patterns from other projects, domains, or historical precedents that map to this situation?

Output a brief **Patterns Observed** section.

## Lens 2: Root-Cause Questioning

Challenge the framing of the problem itself.

- What evidence do we have vs. what are we assuming?
- What is the *real* problem — not the symptom?
- What would need to change if we solved the root cause instead of patching?
- If starting from zero with no legacy constraints, how would we design this?

Output a brief **Reframed Problem** section that may differ from the original ask.

## Lens 3: Resource & Impact Analysis

Understand the economics before committing resources.

- Where does the real value come from in this system / feature / decision?
- What drives cost and complexity (identify the variables, not the numbers)?
- How sustainable is the current approach at 2×, 10×, 100× scale?
- What metrics actually matter for this decision?

Output a brief **Resource Reality** section.

## Lens 4: Hands-On Validation

Bias toward experiments over speculation.

- What is the smallest experiment that validates or kills the proposed approach?
- Define success criteria before starting.
- What is the fastest path to real-world feedback?

Output a brief **Validation Plan** with a concrete next step.

## Lens 5: Clear Communication

Ensure the strategy can be understood and acted upon.

- What is the ONE thing the reader must take away?
- Can this be shown visually? (diagram, comparison table, decision tree)
- Who is the audience and what depth do they need?

Output a brief **Key Message** section and offer to generate a visual artifact.

## Synthesis

1. **Strategic Recommendation** — clear, concise position
2. **Confidence Level** — High / Medium / Low with reasoning
3. **Key Risks** — what could go wrong and how to mitigate
4. **Immediate Next Action** — the single most important thing to do right now

## Adaptive Depth

- **Quick decisions** (naming, small refactors, tool choice): 1–2 sentences per lens
- **Medium decisions** (API design, library choice, feature architecture): Full treatment, paragraph each
- **High-stakes decisions** (migrations, redesigns, platform bets): Deep analysis, include diagrams

## Extended Frameworks

Available for deeper analysis when core lenses reveal conflicting signals:

- **De Bono Six Thinking Hats** → `references/six-hats.md` — Team-oriented decisions, stakeholder conflict
- **Islamic Epistemology Cognitive Levels** → `references/cognitive-levels.md` — Depth-of-understanding analysis
- **Bridges Synthesis** → `references/bridges-synthesis.md` — Classical wisdom × modern engineering
- **Ibn Khaldun's Muqaddimah** → `references/ibn-khaldun.md` — Team dynamics, organizational cohesion, decay signals
- **Security Engineering (Anderson)** → `references/security-engineering.md` — 4-element security framework, threat modeling, usability, economics of security. Load when the decision or Foresight Report has security properties.

Invoke extended frameworks when:
- Core lenses reveal conflicting signals
- The problem has team/organizational dimensions
- The problem has philosophical or ethical dimensions
- The user invokes `/istishraf:think-deeper` or `/istishraf:think-deeper --all`
