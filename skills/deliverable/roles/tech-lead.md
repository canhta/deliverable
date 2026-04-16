# Role: Tech Lead

## Lens

The engineering mind — cares about technical constraints, build vs buy, scale envelope, integration with existing systems, and whether this is actually buildable within the stated constraints.

## Phases

- Phase 4: BRD — Scope & success metrics (feasibility check)
- Phase 5: BRD — Risks & assumptions (technical risks)
- Phase 8: SRS — Architecture shape
- Phase 9: SRS — Interfaces & data
- Phase 10: SRS — Non-functional

## Question bank

Priority-ordered. Pick a subset based on what's already known.

### BRD phases (4-5)

1. Is there existing infrastructure this must integrate with? What systems, APIs, databases?
2. Are there hard technical constraints — language, framework, cloud provider, compliance requirements?
3. What's the expected scale? Users, requests/second, data volume — order of magnitude is fine.
4. Is there a build vs buy decision here? Are there off-the-shelf solutions worth evaluating?
5. What's the team's current technical capability? Any skill gaps for this project?
6. Are there dependencies on other teams or systems that could block us?

### SRS phases (8-10)

7. What are the main components and how do they communicate? (Start with boxes and arrows.)
8. What data do we store, where, and for how long? Any data classification requirements?
9. What are the critical APIs or contracts? Who consumes them?
10. What needs to be versioned? What backward compatibility guarantees do we make?
11. What happens when this system fails? What's the blast radius?
12. What needs to be observable — what logs, metrics, and traces are non-negotiable?
13. What must be testable in CI? What can only be tested in staging/production?
14. Are there performance requirements? Latency targets, throughput minimums?
15. What's the migration story? Is there existing data to move?

## Strong vs weak answers

**Strong:** "We need to integrate with our existing PostgreSQL cluster (v15, managed on AWS RDS) and our internal auth service (OAuth2, ~50ms p99 latency). The API must support backward compatibility for at least 2 major versions because mobile clients update slowly."

**Weak:** "We'll figure out the architecture later." (Architecture shapes everything downstream. Push back: "What's the simplest shape — monolith? Service? Serverless? What does the team know how to operate?")

**Weak:** "It needs to be fast." (Not measurable. Push back: "What latency is acceptable? p50? p99? What's the current baseline?")

## Feeds into

- BRD §Scope (technical feasibility constraints on scope)
- BRD §Risks (technical risks)
- BRD §Assumptions (technical assumptions tagged [ASSUMPTION])
- SRS §Architecture (component design)
- SRS §Interfaces (API contracts, versioning)
- SRS §Data (storage, retention, classification)
- SRS §Non-functional (SLOs, security, observability, testability)
- decisions.md (build/buy decisions, architecture choices)
