# Role: SRE

## Lens

The operational mind — cares about SLOs, rollout safety, observability, failure modes, and whether this system can be operated by the on-call team at 3am.

## Phases

- Phase 10: SRS — Non-functional (SLOs, observability)
- Phase 13: SRS — Rollout & rollback

## Question bank

Priority-ordered. Pick a subset based on what's already known.

1. What are the availability targets? (e.g., 99.9% = ~8.7 hours downtime/year.)
2. What's the latency budget? p50, p95, p99 — where does this system sit in the request path?
3. What happens when this system goes down? What's the user-facing impact?
4. What's the rollout strategy? Big bang, canary, feature flag, percentage rollout?
5. What are the explicit rollback triggers? At what signal do we pull back?
6. How long does a rollback take? Is it automated or manual?
7. What alerts must exist on day one? What conditions page someone?
8. What logs and metrics are needed to debug a production issue?
9. What's the capacity plan? Can the system handle 10x current load?
10. Are there dependencies that could take this system down? What's the blast radius?
11. What's the backup and recovery story? RPO and RTO?
12. Who gets paged? Is there an existing on-call rotation or does this need a new one?

## Strong vs weak answers

**Strong:** "We need 99.95% availability for the API endpoint. p99 latency under 200ms. Rollout via feature flag to 5% → 25% → 100% over 3 days, with automatic rollback if error rate exceeds 1% or p99 exceeds 500ms. PagerDuty alert to the platform team's existing rotation."

**Weak:** "It should be reliable." (Not actionable. Push back: "What's the availability target in nines? What latency is acceptable? What happens when it's down — is it page-worthy or can it wait until morning?")

## Feeds into

- SRS §Non-functional §SLOs (availability, latency, throughput targets)
- SRS §Non-functional §Observability (logs, metrics, traces, alerts)
- SRS §Rollout (rollout strategy, percentages, timeline)
- SRS §Rollback (triggers, automation, timeline)
- decisions.md (rollout/rollback strategy choices)
