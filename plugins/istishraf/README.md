# Istishraf Plugin — Internal Reference

This file is the primary context Claude Code loads when the plugin runs.
Full documentation for humans is in the root `README.md`.

---

## Which command to use — and why

This is the single most important section. Every command has one situation
that calls for it, one output it produces, and one thing it is not for.

```
SITUATION                                    COMMAND              OUTPUT
────────────────────────────────────────────────────────────────────────────────
I have a problem and need a recommendation   :strategize          One decision +
                                                                  confidence +
                                                                  top 2 risks

I have conflicting signals or don't know     :think-deeper        Where frameworks
why we're stuck                                                   disagree — that
                                                                  divergence is the
                                                                  insight

Am I making a high-stakes decision with      :assess-depth        Level 1–7 verdict:
enough understanding?                                             safe to proceed,
                                                                  or questions first

We have a plan — what could destroy it?      :premortem           5 failure stories +
                                                                  assumption inventory
                                                                  + mitigations

Strategy decided — now I build               :implement           8-phase execution:
                                                                  TDD, zero regressions,
                                                                  clean PR

Quick check before git commit                :review              7 attack vectors,
                                                                  pass/fail, verdict

Full check before opening a PR              :shura               3–7 council seats,
                                                                  synthesis verdict,
                                                                  must-fix list

Are my tests proving rules or just          :testaudit           Classification table
running code for coverage?                                        (L1–L5), theater list,
                                                                  priority rewrites

I need thinking + building in one           :istishraf           Full pipeline with
sequence for a significant feature                                checkpoint between
                                                                  thinking and building
```

**The one-line decision rule**:
- Use `:strategize` when you need **a decision**.
- Use `:think-deeper` when you need to understand **why you're stuck**.
- Use `:review` before **git commit** (30 seconds).
- Use `:shura` before **opening a PR** (thorough).
- When unsure: start with `:strategize`.

---

## Command → file → invocation

| Invoke as | File | Model |
|---|---|---|
| `/istishraf:istishraf` | `commands/istishraf.md` | Opus (Ph.0–2) → Sonnet (Ph.3–5) |
| `/istishraf:strategize` | `commands/strategize.md` | Opus |
| `/istishraf:think-deeper` | `commands/think-deeper.md` | Opus |
| `/istishraf:assess-depth` | `commands/assess-depth.md` | Opus |
| `/istishraf:premortem` | `commands/premortem.md` | Opus |
| `/istishraf:implement` | `commands/implement.md` | Sonnet |
| `/istishraf:review` | `commands/review.md` | Sonnet |
| `/istishraf:shura` | `commands/shura.md` | Opus |
| `/istishraf:testaudit` | `commands/testaudit.md` | Opus |

---

## How commands relate as a pipeline

```
THINK                    DECIDE              BUILD            VERIFY
────────────────────     ────────────────    ─────────────    ──────────────────
:assess-depth            Foresight Report    :implement       :review       (fast)
:strategize          →   (Phase 2 of     →               →   :shura        (full)
:think-deeper            :istishraf)                          :testaudit    (tests)
:premortem
```

`:istishraf` orchestrates the full pipeline above in one sequence.

---

## Skills and auto-triggers

| Skill | Auto-triggers on |
|---|---|
| `strategic-analysis` | `:strategize`, "design", "architect", "should we", "trade-off" |
| `deep-cognition` | `:think-deeper`, "multiple perspectives", "six hats", "deeper analysis" |
| `implementation` | `:implement`, "implement", "fix the bug", "add the feature" |
| `adversarial-review` | `:review`, "is this safe", "check for issues" |
| `shura` | `:shura`, "review from different angles", "another role", "council review" |
| `testaudit` | `:testaudit`, "are my tests fake", "coverage theater", "test quality" |

---

## Model routing (opusplan)

`opusplan` only switches on Claude Code's native plan mode (Shift+Tab).
It does not auto-detect skill invocations.

**Enter plan mode (Shift+Tab) before**: `:strategize`, `:think-deeper`,
`:assess-depth`, `:premortem`, `:shura`, `:testaudit`, `:istishraf` Ph.0–2

**Exit plan mode (Shift+Tab) before**: `:implement`, `:review`,
`:istishraf` Ph.3–5

---

## File structure

```
commands/          One .md file per command. Filename = command name.
agents/            principal-architect.md — triage agent, routes to right command.
skills/
  strategic-analysis/   5-lens + Six Hats + Bridges + Ibn Khaldun references
  deep-cognition/       Extended framework playbooks
  implementation/       PATTERNS.md, CHECKLISTS.md for :implement
  adversarial-review/   7-vector attack skill for :review
  shura/                Council review skill for :shura
  testaudit/            Test semantics audit skill for :testaudit
```
