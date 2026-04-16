---
name: deliverable
description: >-
  [deliverable] Use when the user asks to write, plan, or structure product or
  engineering requirements — BRD, SRS, PRD, RFC, design doc, project charter,
  roadmap, "spec this feature," "requirements for X," or any variant. Routes to
  the appropriate deliverable sub-skill based on what the user needs.
---

# Deliverable — Requirements Authoring Suite

A suite of skills for producing requirements documents — from project charter through BRD, SRS, and beyond. Each skill handles one part of the process. This skill figures out which one you need.

Announce at start: _"I'm using the deliverable skill suite to help with requirements authoring."_

## Preamble — run on every activation

### Step 1: First-run setup

```bash
"$DELIVERABLE_ROOT/bin/config" get auto_update 2>/dev/null || echo "FIRST_RUN"
```

Where `DELIVERABLE_ROOT` is the root of the deliverable skill installation (parent of `skills/`).

**If output is `FIRST_RUN`:** Ask the user before doing anything else:

> _"Welcome to deliverable. Would you like to stay up to date automatically?_
>
> 1. _Yes, auto-update (recommended)_
> 2. _Ask me each time_
> 3. _Never check for updates"_

Then save:

```bash
"$DELIVERABLE_ROOT/bin/config" set auto_update <true|ask|false>
```

### Step 2: Version check

```bash
"$DELIVERABLE_ROOT/bin/update-check"
```

**If output is `UPGRADE_AVAILABLE <old> <new>`:**

- **If `auto_update=true`:** Invoke the **upgrade** skill automatically. Tell user: _"Auto-updating deliverable v`old` → v`new`..."_
- **If `auto_update=ask`:** Tell user: _"deliverable v`new` is available (you're on v`old`). Say 'upgrade deliverable' to update."_

Then continue with routing — never block the user's request.

**If no output:** up to date or disabled. Continue silently.

### Step 3: Resume check

Before routing, check for existing state:

1. Read `~/.claude/projects/<slug>/deliverable/state.md` if it exists
2. If a skill was in progress, offer to resume before routing to a new skill
3. If all phases complete, proceed to routing normally

## Routing

```mermaid
flowchart TD
    A[User request] --> B{What do they need?}
    B -->|"charter", "initiative brief", "why are we doing this"| C[charter]
    B -->|"BRD", "requirements", "spec this", "business requirements"| D[brd]
    B -->|"SRS", "technical spec", "architecture", "engineering spec"| E[srs]
    B -->|"red-team", "challenge this", "what's wrong with this"| F[critic]
    B -->|"review", "audit", "check the docs"| G[review]
    B -->|"interview", "prepare questions", "ask stakeholders"| H[interview]
    B -->|"upgrade", "update deliverable"| U[upgrade]
    B -->|ambiguous| I[Ask user to clarify]
```

### Decision table

| User says                               | Invoke                          | Why                                   |
| --------------------------------------- | ------------------------------- | ------------------------------------- |
| "Write a BRD for X"                     | **brd**       | Explicit BRD request                  |
| "Spec this feature"                     | **brd**       | Feature spec = BRD                    |
| "Requirements for X"                    | **brd**       | General requirements = start with BRD |
| "Write an SRS" / "Technical spec"       | **srs**      | Explicit SRS/tech spec                |
| "Project charter" / "Initiative brief"  | **charter**             | Explicit charter                      |
| "Plan this project properly"            | **charter**             | Start from the top                    |
| "Review the requirements"               | **review**         | Review existing docs                  |
| "Red-team this" / "Challenge this spec" | **critic**       | Adversarial review                    |
| "I need to interview the CTO"           | **interview**       | Interview prep                        |
| "upgrade deliverable" / "update"        | **upgrade**         | Self-update                           |
| First time, no docs exist               | Offer **charter** first | Start from the top for greenfield     |
| "Continue where we left off"            | Check `state.md`                | Resume whichever skill was active     |

### The natural flow

```
charter → brd → srs → critic → review
                          ↑
                  interview (anytime)
```

Each skill works independently, but they chain through shared artifacts in `docs/requirements/`.

### Shared state

**Project artifacts (git-committed):**

```
docs/requirements/
├── charter.md
├── charter-data.json   ← Excel source data (if Excel output chosen)
├── charter.xlsx        ← Excel export (if Excel output chosen)
├── brd.md
├── srs.md
├── decisions.md
├── open-questions.md
├── research/
├── interviews/
├── roadmap.md          ← extra
├── roadmap-data.json   ← Excel source data (if Excel output chosen)
├── roadmap.xlsx        ← Excel export (if Excel output chosen)
└── <other extras>.md
```

**Session state (never committed):**

```
~/.claude/projects/<slug>/deliverable/
├── state.md
├── session-log.md
├── sub-agent-outputs/
└── draft-scratch/
```

### When unsure

Ask:

> _"I can help with requirements. What would you like to do?_
>
> - \*Start from scratch → **charter\***
> - \*Write business requirements → **brd\***
> - \*Write technical requirements → **srs\***
> - \*Challenge existing requirements → **critic\***
> - \*Quality check existing docs → **review\***
> - \*Prepare for stakeholder interviews → **interview\***"

### Resume protocol

1. Read `~/.claude/projects/<slug>/deliverable/state.md` if it exists
2. If a skill was in progress, offer to resume
3. If all phases complete, offer review or new project
