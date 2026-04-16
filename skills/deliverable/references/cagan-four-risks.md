# Cagan's Four Risks

Reference framework from Marty Cagan's _Inspired_ and _Empowered_. Use this lens when evaluating whether requirements are complete.

## The four risks

Every product initiative must address four risks. If any is unresolved, the project is not ready for implementation.

### 1. Value risk

**Question:** Will users actually want this?

**What to look for in requirements:**

- Evidence of user demand (interviews, data, support tickets) — not just internal conviction
- Clear problem statement tied to real user pain
- Success metrics that measure user value, not just output
- JTBD framing — what job is the user hiring this product to do?

**Red flags:**

- "We think users will love this" without evidence
- Features driven by stakeholder requests without user validation
- Success metrics that measure activity (page views) not outcomes (task completion)

### 2. Usability risk

**Question:** Can users figure out how to use this?

**What to look for in requirements:**

- Key user flows described step by step
- Onboarding or first-use experience considered
- Error states and recovery paths defined
- Accessibility requirements stated

**Red flags:**

- Complex flows with no error handling
- "Users will figure it out" assumptions
- No consideration of the new user experience
- Missing accessibility requirements for user-facing products

### 3. Feasibility risk

**Question:** Can the engineering team actually build this?

**What to look for in requirements:**

- Technical constraints identified and addressed
- Dependencies on other teams or systems mapped
- Build vs buy decisions made with rationale
- Scale requirements stated with numbers
- Team capability gaps identified

**Red flags:**

- "We'll figure out the architecture later"
- Dependencies on systems the team doesn't control
- No consideration of existing technical constraints
- Unstated assumptions about team capability

### 4. Viability risk

**Question:** Does this work for the business?

**What to look for in requirements:**

- Business model impact considered (pricing, cost, margin)
- Legal and compliance requirements identified
- Operational cost of running this system estimated
- Alignment with company strategy stated

**Red flags:**

- No consideration of operational cost
- Regulatory requirements not yet researched
- No stakeholder alignment on business impact
- "We'll figure out pricing later" for revenue-affecting features

## How to use this framework

During red-team deliverable-review (phase 16), evaluate the BRD and SRS against all four risks. For each risk:

1. Is there evidence the risk has been addressed?
2. Is the evidence from users/data (strong) or from assumptions (weak)?
3. Are there [ASSUMPTION] tags where evidence is missing?
4. Are there [OPEN] items that block risk resolution?

Flag any risk that has no evidence and no [ASSUMPTION] tag — this is an unacknowledged blind spot.
