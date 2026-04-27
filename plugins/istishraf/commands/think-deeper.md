---
description: >
  Multi-framework deep cognitive synthesis. Layers Six Thinking Hats, Islamic
  Epistemology cognitive levels, Bridges classical synthesis, and Ibn Khaldun's
  organizational dynamics on top of the 5-pillar analysis. Use when standard
  strategic analysis yields conflicting signals, when the problem has ethical or
  team-dynamics dimensions, or when explicit multi-perspective thinking is needed.
argument-hint: <problem description> [--hats] [--levels] [--bridges] [--ibn-khaldun] [--all]
---

> **WHEN**: You have conflicting signals, are stuck between options, or need to understand WHY.
> **GET**: The point of divergence between frameworks — that conflict is the insight you need.
> **NOT FOR**: A simple decision — use /istishraf:strategize when you need an answer, not understanding.

# /think-deeper

> ⚡ **`opusplan` users**: This command is pure deep reasoning.
> Enter plan mode first with **Shift+Tab** so it runs on Opus.

Execute a **Deep Cognition Synthesis** by intersecting multiple reasoning frameworks.

**Core Principle**: True insight comes from *conflict between frameworks*, not from listing their outputs. Find where they disagree. That divergence is the signal.

Prefix response with `## [DEEP COGNITION MODE]`.

---

## Step 1: Context Check

- Has `/istishraf:strategize` been run recently in this conversation?
  - **Yes**: Summarize its key finding in 1 sentence. Do NOT re-run it.
  - **No**: Run a silent 5-lens baseline first. Surface only the key tension.

---

## Step 2: Framework Selection and Execution

Run only the frameworks requested via flags (or all, if `--all` is used). Execute silently; do not output raw lists. Find the *tensions*, not the summaries.

### `--hats` (De Bono Six Thinking Hats)
*Act as the Blue Hat Facilitator*. Conduct the full session internally.

White (Facts) → Red (Emotion/Intuition) → Black (Risks) → Yellow (Benefits) → Green (Alternatives) → Blue (Synthesis)

**Focus**: What is the emotional/political reality versus the technical reality? Where does what the team *feels* diverge from what the data *says*?

See `references/six-hats.md` for the full protocol.

---

### `--levels` (Islamic Epistemology Cognitive Levels)
*Act as the Epistemic Auditor*. Check levels 1–7.

**Focus**: Are we making a decision whose stakes require Level 5 (systemic understanding) with only Level 2 (mental model) or Level 3 (hypothesis) evidence?

The most dangerous zone: making Level 5 decisions while *believing* we are at Level 4.

See `references/cognitive-levels.md` for definitions.

---

### `--bridges` (Classical Wisdom × Modern Engineering)
*Act as the Translator*. Find the timeless version of this problem.

Ask: "What is this problem, stripped of its technology clothing?"

Map to a classical principle:
- Al-Kindi's Decomposition — for debugging, incident response
- Ibn Tufayl's Autodidact — for production observability vs. upfront design
- Al-Ghazali's Certainty — for test strategy (unit vs. integration vs. prod)
- Ijmā' (Consensus) — for standards adoption vs. novel approaches
- Ijtihād (Innovation) — for breaking convention (requires mastery first)
- Maqāsid (Higher Objectives) — for questioning process overhead

See `references/bridges-synthesis.md` for full bridge library.

---

### `--ibn-khaldun` (Ibn Khaldun's Muqaddimah — Team & Organization)
*Act as the Organizational Analyst*. Apply Ibn Khaldun's dynamics to the team or org context.

**Focus**: The human/team dimension that purely technical frameworks miss.

See `references/ibn-khaldun.md` for the full framework.

---

## Step 3: Tension Analysis (The Insight Engine)

After running frameworks, identify:

**Convergences** — Where ALL frameworks agree → High confidence signal
**Divergences** — Where frameworks disagree → This is where the real insight lives
**Blind Spots** — What NONE of the frameworks addressed

---

## Output Format

```markdown
## [DEEP COGNITION MODE]

### Deep Synthesis: [Problem Title]

**Baseline Position**
[1-sentence summary of the 5-lens strategic position, or "Not yet assessed."]

---

**Framework Tensions (The Conflict)**

* **Conflict 1**: [Framework A] signals X, while [Framework B] signals Y.
    * *Resolution*: [How we break the tie — which framework wins and why]
* **Conflict 2**: [e.g., "Data (White Hat) is absent, but Intuition (Level 6) is high"]
    * *Resolution*: [How to handle the epistemic gap]

---

**Framework Insights** *(only for flags used)*

* **🎩 Hats**: [Strongest signal — e.g., "Black Hat identified a single point of failure the team is emotionally invested in ignoring"]
* **📊 Levels**: [The maturity gap — e.g., "Team is operating at Level 3 (Estimation) on a Level 5 (Reflective) decision"]
* **🌉 Bridges**: [The timeless principle — e.g., "This is the Blind Pilot pattern: optimizing a system we cannot observe"]
* **🏛️ Ibn Khaldun**: [The organizational signal — e.g., "Team 'Aṣabiyya is at peak; this is the time to attempt the migration"]

---

**Convergence** *(where all active frameworks agree)*
[High-confidence signal — treat this as near-certain]

**Divergence** *(where frameworks conflict — the real question)*
[This is the decision that requires human judgment. State the trade-off clearly.]

**Blind Spots** *(what no framework covered)*
[Honest acknowledgement of analytical gaps]

---

> **The Wisdom Note**
> [A single philosophical synthesis statement that elevates the discussion beyond the immediate technical question. Should be something worth saving.]

**Recommended Path Forward**
[Concrete next step. Specific. Actionable.]
```
