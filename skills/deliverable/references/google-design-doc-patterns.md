# Google Design Doc Patterns

Reference patterns from _Software Engineering at Google_ (Titus Winters, Tom Manshreck, Hyrum Wright) and Google eng-practices. Use this when shaping SRS architecture sections.

## Core principles

### 1. Trade-offs explicit

Every architectural decision should state what it gains AND what it costs. "We chose X" is incomplete. "We chose X because it gives us Y, at the cost of Z" is a design decision.

In the SRS: every entry in §Alternatives considered should follow this pattern.

### 2. Alternatives rejected, documented

Don't just document what you chose — document what you didn't choose and why. Future readers (including future-you) need to understand why the alternatives were rejected to avoid re-litigating settled decisions.

In the SRS: §Alternatives considered should always be populated — every architectural decision should document what was rejected.
In decisions.md: every D-entry should have an "Alternatives considered" section.

### 3. Testability as first-class

If it can't be tested, it can't be trusted. Requirements should specify:

- What must be testable in CI
- What can only be tested in staging/production
- What test surfaces are required (unit, integration, e2e, load)
- What the minimum confidence threshold is for shipping

In the SRS: §Non-functional §Testability is not optional.

### 4. Observability as first-class

If it can't be observed, it can't be operated. Requirements should specify:

- What logs must exist (and at what level)
- What metrics must be emitted (and what dashboards)
- What traces are needed (and what sampling rate)
- What alerts must fire (and who gets paged)

In the SRS: §Non-functional §Observability is not optional.

### 5. Time dimension

Code lives for years. Design documents should consider:

- How will this system evolve? What's the next likely change?
- What backward compatibility guarantees are we making?
- What's the deprecation story for things this replaces?
- What will be hard to change later? (These decisions deserve extra scrutiny.)

In the SRS: §Interfaces should address versioning. Preset weighting: migration/compat is required for feature and internal presets.

### 6. Context for future readers

Write for the person who joins the team in six months and needs to understand why things are the way they are. This means:

- Rationale, not just decisions
- Links to evidence, not just conclusions
- Explicit assumptions, not implicit ones
- Cross-references between related decisions

In all artifacts: aggressive cross-linking between BRD, SRS, decisions.md, and open-questions.md.

## Architecture shape patterns

When drafting SRS §Architecture, consider which pattern fits:

| Pattern          | When to use                                     | Trade-off                                     |
| ---------------- | ----------------------------------------------- | --------------------------------------------- |
| Monolith         | Small team, early stage, rapid iteration needed | Simple to deploy, hard to scale independently |
| Service-oriented | Clear domain boundaries, multiple teams         | Independent scaling, operational complexity   |
| Event-driven     | Async workflows, loose coupling needed          | Eventual consistency, harder to debug         |
| Serverless       | Spiky traffic, low operational investment       | Cold starts, vendor lock-in                   |
| Edge/CDN         | Static content, global audience                 | Cache invalidation complexity                 |

Don't prescribe — describe the shape and why it fits this project's constraints.
