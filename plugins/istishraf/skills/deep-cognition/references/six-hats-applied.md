# Six Hats — Engineering Scenario Playbook

Pre-computed arguments for `/istishraf:think-deeper --hats`. Match the scenario, inject the points.

## Scenario A: Buy vs. Build (Auth, Billing, Search, etc.)
* **🔴 Red**: Devs hate black boxes; they love building "pure" code. Fear of vendor lock-in.
* **⚫ Black — Build Risk**: Security vulnerabilities (Auth), compliance scope (PCI/SOC2), maintenance burden.
* **⚫ Black — Buy Risk**: Vendor price hikes, availability dependency, data ownership.
* **🟡 Yellow — Buy**: Immediate SOC2 compliance, focus on core product, MFA/SSO out of the box.
* **🟢 Green**: "Strangler Pattern" — Buy now for speed, build an abstraction layer so you can swap it later.
* **🔵 Decision anchor**: Buy unless there is a specific, measurable competitive advantage in building.

## Scenario B: Monolith vs. Microservices
* **🔴 Red**: FOMO on resume-building tech. Frustration with spaghetti in the monolith.
* **⚪ White**: Team size? (< 10 engineers: microservices is usually math suicide). Deploy frequency?
* **⚫ Black — Microservices**: Distributed tracing complexity. Network latency. CAP theorem data consistency.
* **⚫ Black — Monolith**: Slow CI, tight coupling, hard to scale individual components.
* **🔵 Decision anchor**: If the pain is "code organization," use Modules. If the pain is "independent scalability" or "org boundaries," consider services.

## Scenario C: Speed vs. Quality (Refactor Debate)
* **🔴 Red**: "The code is ugly." "I feel dirty committing this."
* **⚫ Black — Speed**: Accumulating tech debt, potential regressions.
* **⚫ Black — Quality**: Over-engineering risk, missing the market window.
* **🟡 Yellow — Refactor**: Developer happiness, faster future velocity.
* **🟢 Green**: Scout Rule (clean only the file you're touching) + Feature Flag (ship messy code behind a flag, refactor before 100% rollout).

## Scenario D: Technology Switch (React→Vue, Go→Rust, etc.)
* **⚪ White**: Team's current proficiency? Hiring pool at our budget range?
* **⚫ Black**: The Rewrite Trap — velocity drops to zero for 6 months. Unknown unknowns in the new stack.
* **🟡 Yellow**: Better ecosystem, performance gains (sometimes), team morale.
* **🔵 Decision anchor**: Is the current tech *actually preventing* a real business requirement? If not, the switch is probably boredom.

## Scenario E: Team Process (PR size, code review, standups)
* **🔴 Red**: "Reviews take too long." "Nobody reads the PRs anyway."
* **⚪ White**: What is the current defect escape rate? What is review cycle time? Do we have data?
* **⚫ Black**: Removing review without measurement could increase production defects.
* **🟢 Green**: Run a Maqāsid audit — what is the process *actually* for? Measure whether it achieves that.
* **🔵 Decision anchor**: If the outcome (defect prevention, knowledge sharing) is not being achieved by the current ritual, the ritual needs reform — not preservation.
