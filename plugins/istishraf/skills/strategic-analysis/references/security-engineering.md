# Security Engineering — Anderson Reference

Distilled from Ross Anderson's *Security Engineering: A Guide to Building
Dependable Distributed Systems* (3rd edition, 2020). 1044 pages compressed
to the frameworks that matter most for design-time security decisions.

Use this reference when:
- Producing a Foresight Report that covers security properties
- Applying `/istishraf:strategize` to a security-relevant decision
- Enriching `/istishraf:think-deeper` with security system design thinking
- The `/istishraf:shura --security` seat needs deeper framework support

---

## The 4-Element Framework (Chapter 1)

Every security failure in history can be traced to one of four root causes.
Use this as a diagnostic before proposing any security mechanism.

```
POLICY      What are we trying to prevent?
             A security goal that cannot be stated in one sentence
             cannot be enforced by any mechanism.

MECHANISM   How does the implementation enforce the policy?
             The mechanism must cover all paths — including error
             conditions, timeouts, retries, and concurrent access.
             A mechanism that only works on the happy path is not
             a mechanism; it is an assumption.

ASSURANCE   How confident are we that the mechanism works?
             Assurance is proportionate to evidence: a unit test,
             a penetration test, a formal proof, operational experience.
             High-risk components with no assurance evidence are the
             most dangerous items in any system.

INCENTIVES  Are the people responsible for this security motivated
             to maintain it?
             The most common reason security mechanisms fail is not
             technical — it is that the people responsible for them
             had no reason to care, or had reasons to bypass them.
             A security mechanism that makes the secure path harder
             than the insecure path will be bypassed.
```

**Apply in Foresight Report**: For any security-relevant component,
verify all four elements are addressed before approving implementation.
Missing any one is a design gap, not an implementation detail.

**The most common failures by element:**
- Missing Policy: "secure" as a goal, no concrete invariant stated
- Broken Mechanism: happy-path-only enforcement, error paths unguarded
- Missing Assurance: no test that would fail if the control were removed
- Wrong Incentives: security check requires extra developer effort to maintain

---

## Threat Modeling — Who Is the Opponent? (Chapter 2)

Security mechanisms cannot be designed without knowing who they must
resist. Anderson classifies realistic opponents by capability and
motivation. Use this to scope the threat model in a Foresight Report.

### Threat Actor Classes

**Opportunistic / Automated**
- Capability: runs known exploits, automated scanners, credential stuffing
- Motivation: volume — they hit everyone, not you specifically
- Implication: close the obvious attack surface. Standard controls suffice.

**Motivated Outsider**
- Capability: targeted research, chained vulnerabilities, social engineering
- Motivation: specific value in your system — data, money, reputation
- Implication: defence-in-depth required. Assume one control will be bypassed.

**Malicious Insider**
- Capability: legitimate access, knowledge of your systems, patient
- Motivation: financial gain, grievance, coercion
- Implication: audit trail, separation of duties, least privilege are essential.
  Technical controls alone are insufficient — organisational controls required.

**Nation-State / Advanced Persistent Threat**
- Capability: zero-days, supply chain compromise, long-dwell operations
- Motivation: intelligence, disruption, strategic advantage
- Implication: relevant only for critical infrastructure and high-value targets.
  Most systems do not need to resist this threat class.

### Threat Model Output Format

For any Foresight Report with security properties:

```
Realistic opponent:    [class from above]
Primary motivation:    [what they want from this system]
Most likely attack:    [the specific vector given opponent + system]
Worst case outcome:    [what a successful attack achieves]
Proportionate controls: [what level of defence matches this threat]
Out of scope:          [threat classes we are explicitly not defending against]
```

**Anderson's key warning**: Over-engineering controls for a threat class
that will never attack your system wastes resources and creates friction
that causes people to bypass the controls — making you less secure
than a simpler, well-maintained approach would have been.

---

## Psychology and Usability (Chapter 3)

Security that users work around provides no security. This is not a
usability concern — it is a security invariant.

### The Usability Security Laws

**Least Surprise**: Security controls that behave unexpectedly will be
disabled or bypassed. Design controls to match users' mental models.

**Fewest Moving Parts**: Every additional step in a security procedure
is a point of failure. Simplify authentication, authorisation and key
management relentlessly.

**Safe Defaults**: The default configuration must be the secure one.
Insecure-by-default is a deployment failure waiting to happen.

**Visible Feedback**: Users need to know when security is active and
when it is not. Invisible security mechanisms are disabled without
anyone noticing.

**The Barn Door Problem**: Once data is exposed, it cannot be
unexposed. Security decisions that are hard to reverse (publishing
an API endpoint, granting a permission) must be taken with more care
than decisions that are easy to reverse.

### Apply When

- Designing authentication flows: would a real user complete this
  correctly under time pressure? Under stress?
- Reviewing permission models: are the defaults safe? Will developers
  read the documentation before granting permissions?
- Evaluating API security: does the secure path require more code
  than the insecure path? If so, most callers will use the insecure path.

---

## Economics of Security (Chapter 8)

Security failures are often not technical failures — they are economic
ones. The people who make security decisions are not the people who
bear the consequences.

### The Incentive Misalignment Pattern

The most important insight in Anderson's economics chapter:

**When the cost of a security failure falls on someone other than
the person who made the security decision, the decision will be
systematically wrong.**

Examples:
- A developer ships insecure code; the customer bears the breach cost
- A vendor sells a product with known vulnerabilities; the buyer bears the risk
- An ops team disables a security control to hit a deadline; users bear the exposure

**Apply in Foresight Report**: For any security-relevant design decision,
ask — who bears the cost if this fails? If it is not the decision-maker,
the incentive structure is misaligned and the mechanism will be
undermaintained.

### The Market for Lemons

Anderson applies Akerlof's market for lemons to security products:
buyers cannot tell good security from bad security before purchase,
so vendors have no incentive to invest in security beyond the minimum
needed to pass evaluation. This means:

- Standard compliance (SOC2, ISO27001) is a floor, not a ceiling
- Evaluation results are less meaningful than operational track record
- "Certified secure" is a legal/commercial claim, not a technical one

### Security Economics in Practice

| Situation | Economic signal | Implication |
|---|---|---|
| Vendor offers free security tool | Revenue model is data, not tool | Your threat model includes the vendor |
| Compliance deadline drives security work | Incentive is audit pass, not security | Controls will be minimal and unmaintained |
| Security team has no authority to block releases | Incentive structure says security is optional | Findings will accumulate without remediation |
| Bug bounty program exists | Incentive to find and report, not exploit | Better signal than penetration test alone |

---

## Secure Systems Development (Chapter 27)

Anderson's principles for building security in from the start,
not bolting it on at the end.

### The Core Principles

**Security is a system property, not a component property.**
A system of secure components can be insecure. A component cannot
be evaluated for security without knowing the system context.

**Attack the design before attacking the implementation.**
Most security failures are design failures. Implementation review
finds bugs; design review finds architectural vulnerabilities.
The cost of finding a design flaw in review is 10× lower than
finding it in production.

**Assume breach.**
Design systems so that when (not if) one component is compromised,
the blast radius is bounded. Defence-in-depth is not paranoia —
it is the engineering response to the fact that no mechanism
provides perfect assurance.

**Log everything security-relevant. Alert on anomalies.**
An attacker who operates for months undetected causes more damage
than one detected in hours. Observability is a security property.

**Patch fast. Decay is inevitable.**
Every deployed system accumulates vulnerabilities over time.
The question is not whether vulnerabilities will appear but how
quickly they will be remediated. A system that cannot be patched
quickly is not a secure system — it is a liability accumulating
interest.

### The Security Development Lifecycle

For the Foresight Report, confirm these are addressed:

```
1. Threat model documented before design is finalised
2. Security requirements explicit and testable (not "secure")
3. Design reviewed against threat model before implementation
4. Implementation reviewed for the 4-element framework
5. Tests prove security properties, not just functionality
6. Patch and update mechanism exists before first deployment
7. Incident response plan exists before first deployment
8. Security properties re-evaluated when system context changes
```

---

## Assurance and Sustainability (Chapter 28)

### The Assurance Gap

The most dangerous situation in security engineering is high confidence
in a mechanism that has not been tested. Anderson's assurance hierarchy:

| Level | Evidence | Confidence |
|---|---|---|
| L1 — Claimed | Vendor says it's secure | None |
| L2 — Evaluated | Third-party evaluation against a standard | Low |
| L3 — Tested | Penetration test by a skilled team | Medium |
| L4 — Monitored | Operational evidence over time, anomaly detection | High |
| L5 — Attacked and survived | Withstood real attacks with evidence | Very high |

**Apply**: Match assurance level to risk. A payment processor at L1
is a liability. A low-risk internal tool at L3 may be over-engineered.

### Sustainability

Security that cannot be maintained will decay. For any design decision:

- **Can this be patched quickly when a vulnerability is found?**
  If patching requires months of process, the vulnerability window
  is measured in months.
- **Does this component have a clear owner?**
  Unowned security controls are unmaintained security controls.
- **What is the end-of-life plan?**
  A security mechanism that cannot be replaced will eventually
  be circumvented rather than updated.

---

## Quick Reference — Security Questions for Foresight Reports

When producing a Foresight Report for a security-relevant feature,
add a security section covering:

```
Policy
  □ Security goal stated as a testable invariant (one sentence)
  □ Out-of-scope threats identified explicitly

Mechanism
  □ Enforcement covers all paths (happy, error, timeout, concurrent)
  □ Fails closed, not open (failure → deny, not → allow)
  □ Mechanism survives component failure

Assurance
  □ Test exists that would fail if control were removed
  □ Assurance level matches risk level

Incentives
  □ Secure path is the easy path for developers
  □ Cost of failure falls on the decision-maker
  □ Ownership is clear

Threat Model
  □ Realistic opponent identified
  □ Worst-case outcome bounded
  □ Controls proportionate to threat class

Sustainability
  □ Patch mechanism exists
  □ Audit trail is actionable
  □ Owner identified
```
