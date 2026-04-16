# Acceptance tests — <project name>

> Generated from: srs.md §Interfaces, brd.md §Success metrics · Date: YYYY-MM-DD

## Format

Gherkin-style scaffolding. These are test outlines, not executable specs — implementation teams fill in the details.

---

## AT-001 — <feature or flow>

**Source**: <BRD §Success metric N / SRS §Interface N>

```gherkin
Feature: <feature name>

  Scenario: <happy path>
    Given <precondition>
    When <action>
    Then <expected outcome>

  Scenario: <edge case>
    Given <precondition>
    When <action>
    Then <expected outcome>
```

### Notes

<Anything the test doesn't capture — timing constraints, data requirements, environment needs.>
