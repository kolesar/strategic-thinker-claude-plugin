# Istishraf (استشراف) — Claude Code Plugin

> *Foresight. The discipline of seeing the horizon before crossing the river.*

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that transforms Claude from a fast coder into a **Principal Architect with foresight**: one who thinks deeply before acting, executes with discipline, and attacks their own work before committing it.

---

## Installation

```bash
# Add the marketplace
/plugin marketplace add kolesar/strategic-thinker-claude-plugin

# Install the plugin
/plugin install istishraf@strategic-thinker-claude-plugin
```

### Recommended model configuration

Add to `~/.claude/settings.json`:

```json
{
  "model": "opusplan"
}
```

---

## How to invoke commands

Claude Code plugins use the syntax `/plugin-name:command-name`. This plugin is named `istishraf`, so **every command is prefixed with `/istishraf:`**:

```
/istishraf:strategize      Plan before you build
/istishraf:shura           Review from multiple perspectives
/istishraf:implement       Build with discipline
```

This is the same pattern you used with the previous plugin:

```
# Before (strategic-thinker plugin)
/strategic-thinker:think-deeper Analyze @src/ @tests/ --all

# Now (istishraf plugin)
/istishraf:think-deeper Analyze @src/ @tests/ --all
```

The one exception is the full pipeline, whose command file shares the plugin name — so it is invoked as `/istishraf:istishraf`. If that double-name feels awkward, just use the individual commands directly.

---

## Commands

| Invoke as | Purpose |
|---|---|
| `/istishraf:istishraf <problem>` | **Full pipeline** — assess depth → strategize → Foresight Report → implement → review → quality gate |
| `/istishraf:strategize <problem>` | 5-lens strategic analysis |
| `/istishraf:think-deeper <problem> [flags]` | Multi-framework deep cognitive synthesis |
| `/istishraf:assess-depth <problem>` | Epistemic audit — do we understand enough to decide? |
| `/istishraf:premortem <plan>` | Attack the plan before executing it |
| `/istishraf:implement <feature or issue>` | 8-phase disciplined implementation with TDD and quality gates |
| `/istishraf:review` | Adversarial review — 7 attack vectors, fast single pass |
| `/istishraf:shura [files] [flags]` | Multi-perspective council review — 6 distinct reviewer personas |

---

## `/istishraf:shura` — Multi-Perspective Review

`/istishraf:shura` convenes a council of reviewer personas, each examining your code from a different professional angle. Use it when a single adversarial pass is not enough — after a large PR, before merging a risky change, or any time you want more than one voice.

### Specifying what to review

```bash
# Staged files only (most common — review what you're about to commit)
/istishraf:shura staged

# A specific directory
/istishraf:shura @src/payments/

# Specific files
/istishraf:shura @src/payments/processor.ts @tests/payments/processor.test.ts

# Recent commit (not staged)
/istishraf:shura recent

# With a Jira ticket for product-level review
/istishraf:shura staged -- ticket PROJ-89
```

### Council seats (flags)

| Flag | Persona | Asks |
|---|---|---|
| `--architect` | The Architect | Is the structure sound? Does this create coupling or bypass patterns? |
| `--adversary` | The Adversary | How do I break this? (7-vector adversarial protocol) |
| `--maintainer` | The Maintainer | Can a junior dev maintain this safely in 6 months? |
| `--ops` | The Operator | Can this be deployed, monitored, and debugged at 3am? |
| `--product` | The Product Guardian | Does this actually close the ticket? |
| `--security` | The Security Auditor | Auth, injection, data exposure, privilege boundaries |
| `--all` | Full council | All six seats |

**Default (no flags): `--architect --adversary --maintainer`** — covers 80% of everyday PRs.

### Examples

```bash
# Quick review before committing — default 3 seats
/istishraf:shura staged

# Full council on a payment feature
/istishraf:shura @src/payments/ --all

# Security-focused review
/istishraf:shura staged --security --adversary

# The same style you used before, with the new plugin:
/istishraf:shura @src/ @tests/ --all
```

### `/istishraf:shura` vs `/istishraf:review`

| Situation | Use |
|---|---|
| Quick safety check before a small commit | `/istishraf:review` |
| Large PR, risky change, multiple concerns | `/istishraf:shura` |
| Specific security concern | `/istishraf:shura staged --security` |
| Want architecture feedback | `/istishraf:shura staged --architect` |
| Full pre-merge verification | `/istishraf:shura staged --all` |

`/istishraf:review` is the fast path — one voice (the Adversary), 7 vectors, quick verdict.  
`/istishraf:shura` is the thorough path — multiple voices, each with a different blind spot.

---

## `/istishraf:think-deeper` flags

```bash
/istishraf:think-deeper <problem> --hats         # De Bono Six Thinking Hats
/istishraf:think-deeper <problem> --levels       # Islamic Epistemology cognitive levels
/istishraf:think-deeper <problem> --bridges      # Classical Wisdom × Modern Engineering
/istishraf:think-deeper <problem> --ibn-khaldun  # Team cohesion and organizational dynamics
/istishraf:think-deeper <problem> --all          # All frameworks, full synthesis

# Replaces your previous invocation exactly:
/istishraf:think-deeper Analyze @src/ @tests/ --all
```

---

## Model Routing with `opusplan`

`opusplan` uses Opus during Claude Code's native plan mode (Shift+Tab) and Sonnet everywhere else. It does **not** auto-detect plugin skill invocations — the switch only triggers on the built-in `/plan` mode.

Istishraf commands divide into two clear groups:

| Group | Commands | Model |
|---|---|---|
| **Thinking** | `:istishraf` (Ph.0–2), `:strategize`, `:think-deeper`, `:assess-depth`, `:premortem`, `:shura` | Opus |
| **Execution** | `:istishraf` (Ph.3–5), `:implement`, `:review` | Sonnet |

**How to route correctly:**

```
[Shift+Tab]                               ← enter plan mode → Opus activates

/istishraf:strategize <problem>           ← runs on Opus
/istishraf:think-deeper <problem> --all   ← runs on Opus
/istishraf:shura staged --all             ← runs on Opus
/istishraf:istishraf <problem>            ← Phases 0–2 run on Opus
                                          ← Foresight Report presented
[Shift+Tab]                               ← exit plan mode → Sonnet activates

yes / proceed                             ← Phases 3–5 run on Sonnet
/istishraf:implement <feature>            ← runs on Sonnet
/istishraf:review                         ← runs on Sonnet
```

Each thinking command includes an inline reminder at the top.

> **Future**: [Claude Code issue #19269](https://github.com/anthropics/claude-code/issues/19269) proposes per-skill model routing that would make this automatic. Once it ships, the Shift+Tab step will no longer be needed.

---

## The Full Pipeline (`/istishraf:istishraf`)

```
Phase 0 — Epistemic Gate        [Opus]
  Is our understanding deep enough for the stakes of this decision?
  If not → Interview Mode. No solutions until the gap is closed.

Phase 1 — Strategic Analysis    [Opus]
  5-lens framework: pattern, root cause, resource reality,
  $100 experiment, artifact strategy.

Phase 2 — Foresight Report      [Opus]
  Bridging artifact: invariants, threat model, epistemic gaps,
  success criteria, implementation contract.
  ── Checkpoint: explicit approval required ───────────────────
  ⚡ opusplan: press Shift+Tab here, then approve

Phase 3 — Implementation        [Sonnet]
  8-phase: understand → explore → TDD → implement →
  test suite → docs → pre-commit review → PR quality gate.

Phase 4 — Adversarial Review    [Sonnet]
  7 attack vectors: concurrency, input, state, TOCTOU,
  security, transactions, the 3am test.

Phase 5 — Quality Gate          [Sonnet]
  Foresight accuracy check. Wisdom note for future work.
```

---

## Frameworks

### Five Lenses (Core)
1. **Pattern Recognition** — What is the underlying trend?
2. **Root-Cause Inversion** — What is the *real* problem?
3. **Resource Reality** — What drives cost and complexity?
4. **The $100 Experiment** — What is the smallest validation?
5. **Artifact Strategy** — What diagram makes this clear?

### De Bono Six Thinking Hats
White (facts) → Red (emotion) → Black (risk) → Yellow (benefit) → Green (alternatives) → Blue (synthesis). Conducted as a full facilitated session, not a list.

### Islamic Epistemology — Seven Cognitive Levels
Maps Al-Kindi, Ibn Tufayl, and Al-Ghazali's epistemological hierarchy onto engineering understanding — from raw log observation (Level 1) to lived production experience (Level 7). Used to verify that understanding matches decision stakes before committing.

### Bridges Synthesis — Classical Wisdom × Modern Engineering
- Al-Kindi's Decomposition × Scientific Debugging
- Ibn Tufayl's Autodidact × Learning from Production
- Al-Ghazali's Three Certainties × Test Strategy
- Ijmā' (Consensus) × Engineering Standards
- Ijtihād (Innovation) × Novel Architecture
- Maqāsid (Higher Objectives) × Process Reform

### Ibn Khaldun's Muqaddimah — Organizational Dynamics
- **'Aṣabiyya** — social cohesion as the engine of collective achievement; its decay explains most team failures that look technical
- **Badāwa → Ḥaḍāra** — why successful systems accumulate complexity until they collapse
- **The Generational Cycle** — why teams that were fast are now slow
- **'Ilm al-'Umrān** — individual performance is mostly determined by environment, not innate ability

---

## Structure

```
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   └── istishraf/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       ├── commands/
│       │   ├── istishraf.md       ← /istishraf:istishraf  (full pipeline)
│       │   ├── strategize.md      ← /istishraf:strategize
│       │   ├── think-deeper.md    ← /istishraf:think-deeper
│       │   ├── assess-depth.md    ← /istishraf:assess-depth
│       │   ├── premortem.md       ← /istishraf:premortem
│       │   ├── implement.md       ← /istishraf:implement
│       │   ├── review.md          ← /istishraf:review
│       │   ├── shura.md           ← /istishraf:shura
│       │   └── testaudit.md       ← /istishraf:testaudit
│       ├── agents/
│       │   └── principal-architect.md
│       ├── skills/
│       │   ├── strategic-analysis/
│       │   ├── deep-cognition/
│       │   ├── implementation/
│       │   ├── adversarial-review/
│       │   ├── shura/
│       │   └── testaudit/
│       └── README.md
├── CLAUDE.md.template             ← copy to your project root as CLAUDE.md
└── README.md
```

---

## CLAUDE.md template

The repo includes `CLAUDE.md.template` — copy it to your project root as `CLAUDE.md` and fill in the `[EDIT]` sections. It configures model routing, issue tracker integration (Jira, GitHub), coding standards, system invariants, and complexity zones. Every `/istishraf:implement` session reads this file in Phase 1 before touching any code.

---

## About

Built by Asim Husanović. Combines:
- The original "Become a Strategist" 5-pillar framework
- De Bono's Six Thinking Hats
- Islamic epistemology cognitive levels (Al-Kindi, Ibn Tufayl, Al-Ghazali)
- Bridges synthesis connecting classical scholarship with modern engineering
- Ibn Khaldun's Muqaddimah for organizational dynamics
- The `/wizard` 8-phase execution methodology by [vlad-ko](https://github.com/vlad-ko/claude-wizard), adapted for Istishraf
- Adversarial review and pre-mortem analysis

## License

MIT
