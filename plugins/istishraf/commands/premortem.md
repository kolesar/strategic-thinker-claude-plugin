---
description: >
  Pre-mortem analysis. Imagine the plan has already failed — then work backwards
  to find why. Use BEFORE starting implementation to surface hidden assumptions,
  overlooked failure modes, and risks the optimistic mind ignores. Combines
  Black Hat thinking, epistemic pressure-testing, and Al-Kindi's decomposition.
argument-hint: <describe the plan, feature, or approach to pressure-test>
---

> **WHEN**: You have a plan and want to find what will destroy it before you start building.
> **GET**: 5 failure stories, assumption inventory, reversibility map, and mitigations for each risk.
> **NOT FOR**: A review of code already written — use /istishraf:shura or /istishraf:review for that.

# /premortem — Attack the Plan Before You Execute It

> ⚡ **`opusplan` users**: Pre-mortem is strategic reasoning, not code.
> Enter plan mode first with **Shift+Tab** so it runs on Opus.

*"An army that plans only for victory will be surprised by defeat. An army that plans for defeat too will not be surprised by either."*

## [PREMORTEM MODE]

The human mind is optimistic by default. When you plan a feature, your brain generates reasons it will work. The pre-mortem is the deliberate counter-move: assume it has already failed, then explain why.

This is not pessimism. This is discipline.

---

## Step 1: The Inversion

State the plan clearly, then invert it:

> "It is [6 months / 1 sprint / 1 year] from today. The implementation of [X] has **failed badly**. 
> The system is broken / the team is blocked / the customers are angry / the costs exploded.
> What happened?"

Write at least **5 specific failure stories**. Not vague fears. Specific narratives with causes.

Example format:
```
Failure Story 1: "The race condition"
We deployed the new transfer status feature. Under load, two workers 
picked up the same transfer simultaneously. Both checked status='pending',
both proceeded to 'processing'. One succeeded, one wrote corrupted state.
Root cause: we did not make the state transition atomic.
```

---

## Step 2: The Assumption Inventory

List every assumption the plan relies on. For each, rate it:

| Assumption | Verified? | Risk if Wrong |
|---|---|---|
| [X service is available] | No | High — entire feature blocks |
| [DB schema has column Y] | Yes (grep confirms) | — |
| [Team knows pattern Z] | Assumed | Medium — silent bugs |

**Critical rule**: An unverified assumption in a high-stakes plan is an epistemic gap. It must be closed before implementation, not during.

---

## Step 3: The Single Point of Failure Scan

Ask: "Is there one thing whose failure would cause total system failure?"

Common single points of failure:
- One service all others depend on (no circuit breaker)
- One data store with no replica/fallback
- One person on the team who understands the critical path
- One process step that, if skipped, causes downstream corruption
- One environment variable whose absence causes silent wrong behavior

---

## Step 4: The Reversibility Assessment

For each major decision in the plan, classify:

| Decision | Reversible? | Reversal Cost |
|---|---|---|
| Chose library X | Yes | Medium — 2 days |
| Changed DB schema | Partially | High — migration required |
| Removed feature Y | No | Complete rewrite |

**Rule**: Irreversible or high-reversal-cost decisions require higher epistemic confidence before committing to them. If you are at Level 3 (Estimation) on a Level 6 (Intuitive) decision, you do not have the right to make it irreversible yet.

---

## Step 5: The Bridges Pressure Test

Map the plan to one of the classical failure archetypes:

- **The Cargo Cult**: Are we adopting complexity because it's "modern" rather than because we need it?
- **The Blind Pilot**: Are we optimizing something we cannot observe (no metrics, no logs)?
- **The Ritualistic Guard**: Are we following a process that exists for its own sake, not for the outcome?
- **The Scientist's Debt**: Are we debugging by guessing instead of isolating variables?

If the plan matches an archetype: apply the required challenge from `bridges-applied.md`.

---

## Step 6: The Mitigation Plan

For each identified failure story and risk, produce:

```
Risk: [Specific failure]
Probability: High / Medium / Low
Impact: High / Medium / Low
Mitigation: [What we do to prevent it]
Detection: [How we know it happened]
Recovery: [What we do if it does happen]
```

---

## Output Format

```markdown
### Pre-Mortem Report: [Plan Title]

**The Plan (as stated)**: [1-sentence summary]

#### Failure Stories
1. **[Story name]**: [Specific narrative] → Root cause: [X]
2. **[Story name]**: [Specific narrative] → Root cause: [X]
3. [...]

#### Assumption Inventory
| Assumption | Verified? | Risk if Wrong |
|---|---|---|
| ... | ... | ... |

#### Single Points of Failure
- [SPOF 1]: [Mitigation]
- [SPOF 2]: [Mitigation]

#### Reversibility Assessment
| Decision | Reversible? | Confidence Required |
|---|---|---|
| ... | ... | ... |

#### Classical Archetype Match
[Which archetype(s) this plan risks — and the required challenge]

#### Risk Mitigations
| Risk | P | I | Mitigation | Detection |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

#### Pre-Mortem Verdict
- **Proceed**: [Yes / No / With conditions]
- **Blocking issues**: [List — must be resolved before implementation]
- **Non-blocking concerns**: [Watch during implementation]

> Wisdom note: [What this pre-mortem revealed that optimistic planning would have missed]
```

---

## Why This Works

The pre-mortem exploits a cognitive asymmetry: the human brain is better at explaining things that have already happened than at predicting things that haven't. By treating failure as a historical fact to be explained rather than a future probability to be estimated, you access a different — and more accurate — mode of thinking.

*This technique was described by psychologist Gary Klein. It has been validated in military planning, medical diagnosis, and software architecture alike.*
