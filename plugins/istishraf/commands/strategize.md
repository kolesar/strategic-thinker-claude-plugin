---
description: >
  Apply the 5-lens strategic analysis framework. Enforces pattern recognition,
  root-cause inversion, resource reality, hands-on validation, and clear
  communication before any solution is proposed. Use for architectural decisions,
  refactoring debates, build-vs-buy choices, debugging complex issues, or any
  problem with multiple valid solutions.
argument-hint: <describe the decision or problem to analyze>
---

> **WHEN**: You have a problem or decision and need a clear recommendation.
> **GET**: One recommendation, confidence level, top 2 risks, one immediate next action.
> **NOT FOR**: Multi-perspective synthesis — use /istishraf:think-deeper when you're stuck, not deciding.

# /strategize

> ⚡ **`opusplan` users**: This command is pure strategic reasoning.
> Enter plan mode first with **Shift+Tab** so it runs on Opus.
> Exit plan mode (Shift+Tab again) before starting any implementation.

Apply the 5-Lens Framework to the problem. Do not rush to solution. The lens work IS the work.

Prefix response with `## [STRATEGIZE MODE]`.

---

## Instructions

1. **Ingest Context**: Read the problem from $ARGUMENTS thoroughly.

2. **Run the 5 Lenses silently** — do not narrate each step; distill into the output format below.

---

### Lens 1: Pattern Recognition

- What is the underlying trend in this codebase / system / problem? Why has it emerged now?
- Has this problem occurred before? In this codebase? In other projects you know?
- What is the "shape" of this problem — is there a named pattern that fits (N+1, TOCTOU, big ball of mud, single point of failure, etc.)?
- What happens if the current trajectory continues *unchanged* for 6 months?

*Constraint*: Do not invent patterns where none exist. "No clear pattern found" is a valid output.

---

### Lens 2: Root-Cause Inversion

- Separate evidence (what we have) from assumptions (what we're guessing)
- Invert: "What happens if we do nothing?" If the answer is "nothing bad," the urgency may be manufactured.
- What is the *real* problem — not the symptom being presented?
- If you solved the root cause instead of patching: what would need to change?
- What would this look like if we designed it from scratch with no legacy constraints?

---

### Lens 3: Resource Reality

**Critical**: Do NOT guess costs or latency numbers without data. Identify the *variables* instead.

- What drives cost/complexity here? (e.g., "This scales linearly with user count but exponentially with data interactions")
- What are the unit economics formula? (e.g., "Cost = requests × per_request_cost × retention_days")
- How does the current approach hold at 2×, 10×, 100× current scale?
- What metric actually determines success for this decision? (latency? cost? developer velocity? reliability? maintainability?)

---

### Lens 4: The $100 Experiment

- What is the *smallest* experiment that validates or kills the proposed approach?
- What can be learned in < 4 hours that would change the decision?
- What are the success criteria — define them *before* running the experiment
- What does failure look like? (equally important)

---

### Lens 5: Artifact Strategy

- Can this be explained better with a diagram? (sequence, flowchart, C4 component, decision tree)
- If yes, prepare a `mermaid` block in the output.
- What is the ONE thing the reader must take away from this analysis?

---

## Output Format

```markdown
## [STRATEGIZE MODE]

### Strategic Synthesis: [Problem Title]

**Recommendation**
[Direct, 1-sentence position. E.g., "Do not build X; use Y instead." or "Refactor the schema before adding features."]

**Confidence**: Low / Medium / High
*Reason: [One bullet: what data confirms or limits confidence]*

---

**Patterns Observed**
[What recurring trend or known pattern this matches — or "No clear pattern."]

**Reframed Problem**
[The real problem, which may differ from the stated ask]

**Resource Variable**
[The formula driving cost/complexity. Not a number — a relationship. E.g., "We are trading development speed now for maintenance cost at scale."]

**$100 Experiment**
[Specific next step. < 4 hours. Has defined pass/fail criteria.]

**Key Risks**
- *What breaks first*: [Risk 1]
- *The unknown unknown*: [Risk 2 — the one that surprises people]

---

**Why I Might Be Wrong**
[One honest reason this recommendation could be wrong. E.g., "If traffic spikes 50× overnight, this schema will not hold."]
```

---

## Adaptive Depth

- **Quick decisions** (naming, small refactors, tool choice): 1–2 sentences per lens, output is brief
- **Medium decisions** (API design, library selection, feature architecture): Full lens treatment, paragraph each
- **High-stakes decisions** (migrations, platform bets, team process changes): Deep analysis + diagram + consider running `/istishraf:think-deeper --all`

---

## Extended Frameworks

If the 5 lenses reveal conflicting signals, invoke `/istishraf:think-deeper` with:
- `--hats` → Team/stakeholder conflict, political dimensions
- `--levels` → Epistemic gap (we're deciding before we understand enough)
- `--bridges` → Classical wisdom applicable to this modern problem
- `--ibn-khaldun` → Team dynamics, cohesion, or organizational decay signals
