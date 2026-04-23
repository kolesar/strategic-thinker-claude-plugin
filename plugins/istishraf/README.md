# Istishraf Plugin — Internal Reference

This file is a quick reference for developers working on the plugin and for
Claude Code context. Full documentation is in the root `README.md`.

---

## Commands

Every command is invoked as `/istishraf:<command>`.

| Command | File | Purpose |
|---|---|---|
| `/istishraf:istishraf` | `commands/istishraf.md` | Full pipeline: Phase 0–5 |
| `/istishraf:strategize` | `commands/strategize.md` | 5-lens strategic analysis |
| `/istishraf:think-deeper` | `commands/think-deeper.md` | Multi-framework synthesis |
| `/istishraf:assess-depth` | `commands/assess-depth.md` | Epistemic audit |
| `/istishraf:premortem` | `commands/premortem.md` | Attack the plan before executing |
| `/istishraf:implement` | `commands/implement.md` | 8-phase TDD execution |
| `/istishraf:review` | `commands/review.md` | Adversarial 7-vector review (fast) |
| `/istishraf:shura` | `commands/shura.md` | Multi-perspective council review |

---

## How commands relate

```
THINK                          DECIDE              BUILD              VERIFY
─────────────────────────────  ──────────────────  ─────────────────  ──────────────────
/istishraf:assess-depth        Foresight Report    /istishraf:         /istishraf:review
/istishraf:strategize      →   (Phase 2 of     →   implement      →   /istishraf:shura
/istishraf:think-deeper        :istishraf)                             /istishraf:testaudit
/istishraf:premortem
```

`/istishraf:istishraf` orchestrates the full pipeline above in sequence.

**Quick path** (skip strategy, just execute): `/istishraf:implement`
**Quick review** (one voice, fast): `/istishraf:review`
**Full review** (six voices, thorough): `/istishraf:shura`

---

## Skills and what auto-triggers them

| Skill | Triggers on |
|---|---|
| `strategic-analysis` | `/istishraf:strategize`, "design", "architect", "should we", "trade-off" |
| `deep-cognition` | `/istishraf:think-deeper`, "multiple perspectives", "six hats", "deeper analysis" |
| `implementation` | `/istishraf:implement`, "implement", "fix the bug", "add the feature" |
| `adversarial-review` | `/istishraf:review`, "is this safe", "check for issues" |
| `shura` | `/istishraf:shura`, "review from different angles", "another role", "council review" |
| `testaudit` | `/istishraf:testaudit`, "are my tests fake", "coverage theater", "test quality", "do tests prove" |

---

## Model routing (opusplan)

`opusplan` only switches on Claude Code's native plan mode (Shift+Tab).
It does not auto-detect skill invocations.

| Enter plan mode (Shift+Tab) before | Exit plan mode (Shift+Tab) before |
|---|---|
| `:strategize`, `:think-deeper`, `:assess-depth`, `:premortem`, `:shura`, `:istishraf` Ph.0–2 | `:implement`, `:review`, `:istishraf` Ph.3–5 |

---

## File structure

```
commands/          One .md file per command. Filename = command name.
agents/            principal-architect.md — triage agent, routes to right command.
skills/
  strategic-analysis/   5-lens + Six Hats + Bridges + Ibn Khaldun references
  deep-cognition/       Extended framework playbooks
  implementation/       PATTERNS.md, CHECKLISTS.md for /implement
  adversarial-review/   7-vector attack skill for /review
  shura/                Council review skill for /shura
  testaudit/            Test semantics audit skill for /testaudit
```
