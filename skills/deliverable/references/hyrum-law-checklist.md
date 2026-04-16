# Hyrum's Law Checklist

> "With a sufficient number of users of an API, all observable behaviors of your system will be depended on by somebody." — Hyrum Wright

## The principle

Any behavior your system exhibits — whether intentional or accidental — will eventually become a dependency for someone. This means:

1. Unspecified behaviors are not "free" — they're implicit contracts
2. Performance characteristics become expected baselines
3. Error message text becomes parsed by downstream systems
4. Ordering of unordered results becomes assumed
5. Timing of operations becomes relied upon

## Checklist for requirements review

Use this during red-team review (phase 16) and SRS drafting (phases 8-10).

### API contracts

- [ ] Are all API behaviors explicitly specified, or are some "obvious"?
- [ ] Are error formats documented? Could a consumer parse error messages?
- [ ] Is the ordering of results specified? If "unordered," is that stated explicitly?
- [ ] Are rate limits documented? Will consumers build retry logic around current limits?
- [ ] Are pagination semantics specified? (Cursor vs offset, consistency guarantees.)

### Data

- [ ] Are field types locked down? (e.g., will an ID always be numeric, or could it become a UUID?)
- [ ] Are null vs empty vs missing semantics defined?
- [ ] Is the format of timestamps, currencies, and localized data specified?
- [ ] Are there computed fields whose formula could change?

### Performance

- [ ] Are latency characteristics specified or just "fast"?
- [ ] Could consumers be relying on current response times as SLAs?
- [ ] Are there caching behaviors that consumers might depend on?
- [ ] Are timeout values documented?

### Behavioral

- [ ] Are there side effects (emails, notifications, audit logs) that consumers expect?
- [ ] Is the order of operations specified? Could reordering break consumers?
- [ ] Are there retry behaviors that could cause duplicate processing?
- [ ] Are feature flags permanent or temporary? Will removing one break consumers?

### Versioning

- [ ] Is there a versioning strategy? How are breaking changes communicated?
- [ ] Are deprecated behaviors documented with sunset dates?
- [ ] Is there a migration path for breaking changes?
- [ ] Are there behaviors that should be versioned but aren't?

## How to use this checklist

During SRS review, scan each interface and data contract against these items. For each "no" answer:

1. Is this an intentional omission? → Document it as explicit non-guarantee
2. Is this an oversight? → Add specification to SRS
3. Is this an open question? → Add to open-questions.md with [OPEN] tag

The goal is not to specify everything — it's to be explicit about what IS and what ISN'T guaranteed.
