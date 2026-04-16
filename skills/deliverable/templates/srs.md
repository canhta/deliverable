# SRS — <project name>

> Created: YYYY-MM-DD · Preset: <preset>

## 1. Architecture shape

<High-level. Main components and how they relate. Diagram if helpful.>

## 2. Interfaces

<APIs, contracts, versioning strategy.>

## 3. Data

<Data model, storage, retention, classification.>

## 4. Non-functional requirements

### 4.1 SLOs

<Numeric targets for availability, latency, throughput. Always numeric — no vague targets.>

### 4.2 Security

<Threat model sketch. Required when auto-bump signals detected.>

### 4.3 Privacy

<Data classification. Required when auto-bump signals detected.>

### 4.4 Compliance

<Applicable regulations, evidence requirements, DPA status. Required when auto-bump signals detected. Omit entirely if no auto-bump signals.>

### 4.5 Observability

<Logs, metrics, traces, alerts that must exist.>

### 4.6 Testability

<What must be testable, what test surfaces are required.>

## 5. Rollout & rollback

### 5.1 Rollout plan

<Staged? Feature-flagged? Canary? Percentages and durations.>

### 5.2 Rollback plan

<Explicit abort criteria. Required when auto-bump signals detected.>

## 6. Alternatives considered

<Required when auto-bump signals detected. One paragraph per alternative: what, why rejected.>

## 7. Open questions

<Links to open-questions.md.>

## 8. Decisions log

<Links to decisions.md.>
