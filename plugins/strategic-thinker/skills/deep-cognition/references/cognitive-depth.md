# Cognitive Levels — The Verification Protocol

Reference guide for the `/assess-depth` command. Use these definitions to grade the evidence provided by the user.

## Level 1: Ḥissī (Sensory/Raw Data)
* **Definition**: Uninterpreted input. The "What".
* **Valid Evidence**: Raw log dumps, screenshots of errors, copy-pasted stack traces, timestamps.
* **The Test**: "Can you reproduce the error right now?" (If No -> They are not even at Level 1).

## Level 2: Khayālī (Mental Model)
* **Definition**: The internal schematic of the system.
* **Valid Evidence**: Architecture diagrams, sequence flows, clear explanation of "how data moves from A to B."
* **The Test**: "Explain the data flow to me as if I were a junior dev, without looking at the code."

## Level 3: Wahmī (Estimation/Hypothesis)
* **Definition**: Pattern matching based on past experience. **DANGER ZONE**.
* **Valid Evidence**: "I suspect it's the DB," "It feels like a race condition."
* **The Trap**: Users often present Level 3 (guesses) as Level 4 (facts).
* **The Test**: "What other hypotheses fits the data? Name at least one alternative."

## Level 4: 'Aqlī (Rational/Proven)
* **Definition**: Hypothesis validated by logic or experiment.
* **Valid Evidence**: Profiler flame graphs, isolated reproduction scripts, unit tests that fail/pass deterministically.
* **The Test**: "Show me the specific line of log/code that proves the hypothesis is true and excludes the alternatives."

## Level 5: Fikrī (Systemic/Reflective)
* **Definition**: Connecting the specific instance to general system dynamics.
* **Valid Evidence**: "This queue fills up whenever the batch job runs because of backpressure mismatch."
* **The Test**: "Where else in the system does this specific pattern exist?"

## Level 6: Qudsī (Intuitive/Expert)
* **Definition**: High-fidelity instinct born of deep exposure.
* **Valid Evidence**: Accurate predictions of failure before they happen. (Hard to verify in chat).
* **The Test**: "What is the 'Ick' factor? What specifically worries you about this, even if you can't prove it yet?"

## Level 7: Dhawqī (Lived Experience)
* **Definition**: Knowledge burned in through fire/failure.
* **Valid Evidence**: War stories. "We tried this in 2019 and it failed because of X."
* **The Test**: "Have you personally operated this specific stack at this specific scale during a verified outage?"
