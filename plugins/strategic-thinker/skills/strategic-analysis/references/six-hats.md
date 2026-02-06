# De Bono Six Thinking Hats — Engineering Adaptation

Apply each hat sequentially to force perspective separation. The power comes from preventing premature convergence.

## White Hat (Data & Facts)
- What metrics, logs, benchmarks, and documentation exist?
- What data is missing that we wish we had?
- What are the hard constraints (budget, timeline, team size, SLAs)?
- Distinguish verified facts from "everyone knows" assumptions.

## Red Hat (Gut Feeling & Intuition)
- What feels wrong about this approach, even if we cannot articulate why?
- What excites the team? What drains energy?
- Which option would we recommend to a friend — before analyzing?
- Name the unspoken concerns in the room.

## Black Hat (Risks & Caution)
- What are the failure modes — technical, organizational, market?
- What has gone wrong in similar past decisions?
- Where are the single points of failure?
- What happens under adversarial conditions (bad actors, peak load, team turnover)?

## Yellow Hat (Benefits & Optimism)
- What becomes possible if this succeeds?
- What secondary benefits emerge (developer experience, reusability, learning)?
- Why is now the right time for this change?

## Green Hat (Creative Alternatives)
- What would we do with unlimited resources?
- What if we combined two approaches?
- What would a completely different team (startup, FAANG, open-source) do?
- What is the "lazy" solution that might actually be good enough?

## Blue Hat (Process & Meta-Thinking)
- Which hats revealed the strongest signals?
- Are we spending too much time on risks (Black) and not enough on alternatives (Green)?
- What is the decision-making process — who decides, by when?

## Synthesis Template

```
DECISION: [Clear recommendation]
DATA CONFIDENCE: [What White Hat revealed]
GUT CHECK: [What Red Hat surfaced]
TOP 3 RISKS: [From Black Hat, with mitigations]
KEY UPSIDE: [From Yellow Hat]
ALTERNATIVE CONSIDERED: [Best Green Hat idea we did NOT choose, and why]
PROCESS NOTE: [Blue Hat observation]
```

## Applied Example: Monolith vs Microservices

**White**: 50K RPM, team of 8, deploy 2x/week, P95 200ms. No data on team productivity loss.
**Red**: Devs frustrated by merge conflicts. Excitement about "modern architecture" — is this genuine need or résumé-driven?
**Black**: Distributed complexity. Zero production microservice experience on team. 6-month migration stalls features.
**Yellow**: Independent deployability. Smaller codebases per team. Tech flexibility per service.
**Green**: Modular monolith as middle ground. Extract only the painful module. Strangler fig over 18 months.
**Blue**: Spent 80% on Black Hat. Strongest signal from Green — modular monolith was not in original framing.
