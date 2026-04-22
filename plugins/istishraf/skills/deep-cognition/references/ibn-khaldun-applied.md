# Ibn Khaldun — Engineering Scenarios

Applied scenarios for `/istishraf:think-deeper --ibn-khaldun`.

## Scenario: "Why is our team so slow now?"

**'Aṣabiyya Diagnosis**:
Ask: Has cohesion decayed? Check signals:
- Knowledge silos (only one person knows X)?
- Blame culture (bugs assigned, not owned)?
- Process as religion (standups nobody believes in)?
- "Legacy zones" nobody will touch?

If ≥ 3 signals present: This is not a technical problem. It is a 'Aṣabiyya decay problem.
**Action**: Before any technical change, run a Team Readiness Audit (see `ibn-khaldun.md`).

## Scenario: "Should we attempt this migration?"

**Ḥaḍāra Decay Assessment**:
Where are we on the Badāwa → Ḥaḍāra spectrum?
- Can most engineers explain the full system? (Badāwa indicator)
- How long does CI take? (Complexity indicator)
- How long does onboarding take? (Decay indicator)

Migration requires **Badāwa-leaning state + High 'Aṣabiyya**. Attempting it in Ḥaḍāra state will fail.

## Scenario: "New team members can't contribute"

**Generation Cycle Pattern**:
If founding-era engineers still hold all the architectural knowledge, the team is in Generation 2 (Inheritors). The knowledge has not transferred.
**Action**: Before next milestone, mandate knowledge transfer. Documentation is not enough — pair programming on the critical path is required.

## Scenario: "The rewrite keeps failing"

**'Aṣabiyya Split**:
Two factions with competing visions for the future system = split 'Aṣabiyya. The rewrite is failing not because of technical difficulty but because there is no shared mental model of the destination.
**Action**: Resolve architectural authority before technical debate. One person or small council must have final decision rights.
