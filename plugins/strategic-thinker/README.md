# Strategic Thinker

Transform Claude from a reactive code generator into a strategic thinker that reasons deeply before implementing.

## Plugin Structure

```
strategic-thinker/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── strategize.md
│   ├── think-deeper.md
│   └── assess-depth.md
├── agents/
│   └── strategic-advisor.md
├── skills/
│   ├── strategic-analysis/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── six-hats.md
│   │       ├── cognitive-levels.md
│   │       └── bridges-synthesis.md
│   └── deep-cognition/
│       ├── SKILL.md
│       └── references/
│           ├── six-hats-applied.md
│           ├── cognitive-depth.md
│           └── bridges-applied.md
└── README.md
```

## Commands

| Command | Purpose |
|---------|---------|
| `/strategize <problem>` | Apply 5-pillar strategic analysis |
| `/think-deeper <problem>` | Extended analysis with experimental frameworks |
| `/think-deeper <problem> --hats` | De Bono Six Thinking Hats |
| `/think-deeper <problem> --levels` | Cognitive Levels assessment |
| `/think-deeper <problem> --bridges` | Bridges Synthesis |
| `/think-deeper <problem> --all` | All frameworks with full synthesis |
| `/assess-depth <system>` | Quick cognitive depth assessment |

## Skills

**strategic-analysis** — Auto-triggers on architecture decisions, system design, refactoring, migrations, build-vs-buy, and problems with multiple valid solutions.

**deep-cognition** — Triggers on `/think-deeper`, requests for deeper analysis, or problems with philosophical/ethical dimensions.

## Core: Five Pillars

1. **Pattern Recognition** — What's the underlying trend?
2. **Root-Cause Questioning** — What's the *real* problem?
3. **Resource Analysis** — What are the unit economics?
4. **Hands-On Validation** — What experiment proves this?
5. **Clear Communication** — What's the one takeaway?

## Experimental: Extended Frameworks

- **De Bono Six Thinking Hats** — Separate facts, feelings, risks, benefits, creativity, process
- **Islamic Epistemology Cognitive Levels** — 7 levels from surface observation to lived experience
- **Bridges Synthesis** — Classical wisdom × modern engineering problems

## License

MIT
