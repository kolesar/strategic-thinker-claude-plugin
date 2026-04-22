# Bridges Library — Archetypal Engineering Patterns

Match the trigger, apply the required challenge. No exceptions.

## Pattern 1: The Cargo Cult (False Ijtihād)
* **Triggers**: User wants specific tech (Microservices, K8s, Blockchain) because "everyone uses it."
* **Principle**: Ijtihād (innovation) is invalid without mastery of fundamentals.
* **REQUIRED CHALLENGE**: "Define the exact CAP theorem trade-off that forces this move. If you cannot, stay monolithic."

## Pattern 2: The Blind Pilot (Lack of Lived Experience)
* **Triggers**: Optimizing a system with no observability. Performance debates without profiler data.
* **Principle**: Ibn Tufayl — Description (hearing) ≠ Experience (seeing).
* **REQUIRED CHALLENGE**: "I will not recommend optimizations until we have basic tracing. Implement observability first."

## Pattern 3: The Ritualistic Guard (Empty Maqāsid)
* **Triggers**: Process overhead complaints. Code review delays. Linting slowing shipping without catching bugs.
* **Principle**: Maqāsid — rules exist to serve outcomes. If the outcome fails, reform the rule.
* **REQUIRED CHALLENGE**: "Suspend this process for 1 week. Measure if defect rates change. If not, delete the rule."

## Pattern 4: The Scientist's Debt (Pre-Al-Kindi Debugging)
* **Triggers**: "We tried everything." Random parameter changes. "It works now, I don't know why."
* **Principle**: Al-Kindi — Define precisely, decompose, test independently.
* **REQUIRED CHALLENGE**: "Stop. Revert recent changes. Write a reproduction script that fails deterministically. Then fix."
