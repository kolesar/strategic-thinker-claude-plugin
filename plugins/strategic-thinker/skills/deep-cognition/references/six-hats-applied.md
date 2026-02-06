# Six Hats — Engineering Scenario Playbook

Use these pre-computed arguments to fuel the `/think-deeper --hats` simulation. If the user's problem fits a scenario, inject these specific points into the debate.

## Scenario A: Buy vs. Build (e.g., Auth0, Billing, Search)
* **🔴 Red (Emotion)**: Devs hate integrating black boxes; they love building "pure" code. Fear of vendor lock-in.
* **⚫ Black (Risk)**: **Build Risk**: Security vulnerabilities (Auth), compliance scope (PCI/SOC2), maintenance burden. **Buy Risk**: Vendor price hikes, downtime dependency.
* **🟡 Yellow (Value)**: **Buy Value**: Immediate SOC2 compliance, focus on core product, features (MFA/SSO) out of the box.
* **🟢 Green (Alt)**: "Strangler Pattern" — Buy for now to move fast, architect an abstraction layer to swap it out later if costs explode.

## Scenario B: Monolith vs. Microservices
* **🔴 Red (Emotion)**: FOMO (Fear Of Missing Out) on resume-building tech. Frustration with "spaghetti code" in the monolith.
* **⚪ White (Facts)**: Current team size? (If < 10, Microservices is usually math suicide). Current deploy frequency?
* **⚫ Black (Risk)**: **Microservices Risk**: Distributed tracing is hard. Network latency. Data consistency (CAP theorem). **Monolith Risk**: Slow CI/CD pipelines, tight coupling.
* **🔵 Blue (Decision)**: If the primary pain is "Code Organization," use Modules, not Microservices. Only use Microservices for "Independent Scalability" or "Organizational Boundaries."

## Scenario C: Speed vs. Quality (The Refactor Debate)
* **🔴 Red (Emotion)**: "The code is ugly." "I feel dirty committing this."
* **⚫ Black (Risk)**: **Speed Risk**: Tech debt accumulation, potential regressions. **Quality Risk**: Over-engineering, missing market window.
* **🟡 Yellow (Value)**: **Refactor**: Developer happiness, faster future velocity. **Speed**: Immediate customer feedback.
* **🟢 Green (Alt)**: **The Scout Rule**: Do not stop to refactor; just clean the specific file you are touching. **Feature Flag**: Ship the messy code behind a flag, refactor before enabling for 100%.

## Scenario D: Technology Switch (e.g., React vs Vue, Go vs Rust)
* **⚪ White (Facts)**: What is the team's current proficiency? What is the hiring pool in our budget range?
* **⚫ Black (Risk)**: The "Rewrite Trap" — velocity drops to zero for 6 months. Unknown unknowns in the new stack.
* **🟡 Yellow (Value)**: Access to better ecosystem/libraries. Performance gains (maybe).
* **🔵 Blue (Decision)**: Is the current tech *actually* preventing a business requirement? If not, the switch is likely boredom-driven.
