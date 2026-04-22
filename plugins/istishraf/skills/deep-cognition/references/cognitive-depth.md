# Cognitive Levels — The Verification Protocol

Use to grade the evidence provided in a conversation. Do not infer; look for artifacts.

## Level 1: Ḥissī — Sensory/Raw Data
* **Valid Evidence**: Raw log dumps, screenshots, stack traces, timestamps, direct error text.
* **Test**: "Can you reproduce the error right now?" If No → not yet at Level 1.

## Level 2: Khayālī — Mental Model
* **Valid Evidence**: Architecture diagrams, sequence flows, explanation of data movement A→B.
* **Test**: "Explain the data flow without looking at the code."

## Level 3: Wahmī — Hypothesis ⚠️ DANGER ZONE
* **Valid Evidence**: "I suspect it's the DB." "Feels like a race condition."
* **The Trap**: Users present Level 3 (guesses) as Level 4 (facts) constantly.
* **Test**: "Name at least one alternative hypothesis that fits the same evidence."

## Level 4: 'Aqlī — Rational/Proven
* **Valid Evidence**: Profiler flame graphs, reproduction scripts that fail deterministically, benchmarks, specific log lines that exclude alternatives.
* **Test**: "Show me the specific evidence that proves this hypothesis and excludes the alternatives."

## Level 5: Fikrī — Systemic/Reflective
* **Valid Evidence**: "This queue fills whenever the batch job runs because of backpressure mismatch — and the same pattern exists in the payment processor."
* **Test**: "Where else in the system does this specific pattern appear?"

## Level 6: Qudsī — Expert Intuition
* **Valid Evidence**: Accurate predictions of failure before they happen.
* **Test**: "What specifically worries you, even if you can't prove it yet?"

## Level 7: Dhawqī — Lived Experience
* **Valid Evidence**: War stories. "We tried this in 2019 and it failed because of X."
* **Test**: "Have you personally operated this stack at this scale during a real outage?"
