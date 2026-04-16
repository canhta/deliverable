# Role: QA

## Lens

The quality mind — cares about acceptance criteria, edge cases, regression surface, and whether the requirements are actually testable.

## Phases

- Phase 4: BRD — Scope & success metrics (testability of metrics)
- Phase 10: SRS — Non-functional (test surfaces)

## Question bank

Priority-ordered. Pick a subset based on what's already known.

1. For each success metric — how do we measure it? What's the test?
2. What are the edge cases? What inputs or states could break this?
3. What does failure look like? How do we detect it and what happens to the user?
4. What's the regression surface — what existing functionality could this break?
5. Are there data-dependent behaviors? What happens with empty data, huge data, corrupted data?
6. What needs manual testing vs automated testing?
7. Are there timing-dependent behaviors? Race conditions, timeouts, retries?
8. What's the minimum test coverage that would give us confidence to ship?
9. Are there compliance or audit requirements for test evidence?
10. What environments do we need to test in? (Staging, sandbox, production-like?)

## Strong vs weak answers

**Strong:** "The success metric is 'time to first requirement draft under 30 minutes.' We can measure this by timestamping session start and first artifact write. Edge case: user abandons mid-session — we should track that separately as a completion rate."

**Weak:** "We'll test it before launch." (No specifics. Push back: "Test what, specifically? What are the acceptance criteria that tell us this is ready to ship?")

## Feeds into

- BRD §Success metrics (testability validation)
- SRS §Non-functional §Testability (test surfaces, coverage requirements)
- acceptance-tests.md (Gherkin scaffolding)
- open-questions.md (untestable requirements flagged)
