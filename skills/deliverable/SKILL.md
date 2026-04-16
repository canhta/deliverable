---
name: deliverable
description: >-
  Validates ideas through three adversarial voices (cynical customer, brutal PM,
  future maintainer) and produces a one-page Kill/Narrow/Proceed verdict with
  concrete evidence requirements. Routes to deliverable-charter, deliverable-brd,
  deliverable-srs, deliverable-interview for formal documents. Use when the user
  wants to validate an idea, evaluate a feature concept, write
  product/engineering requirements, or runs `deliverable "idea"`.
---

# deliverable — The Spec Trial

**One command. Three hostile voices. One verdict.**

You drop in an idea. It goes on trial. A cynical customer, a brutal PM, and a future
maintainer each take a run at it. Then a synthesis produces a one-page verdict:
Kill, Narrow, or Proceed — with the exact assumptions that decided it, saved to memory
so next time you bring a similar idea, it already knows what you learned.

Announce at start: _"Running the Spec Trial on your idea."_

---

## Preamble — run on every activation

### Step 1: First-run setup

```bash
"$DELIVERABLE_ROOT/skills/deliverable/bin/config" get auto_update 2>/dev/null || echo "FIRST_RUN"
```

Where `DELIVERABLE_ROOT` is the root of the deliverable installation (parent of `skills/`).

**If `FIRST_RUN`:** Ask the user:

> _"Welcome to deliverable. Stay up to date automatically?_
> 1. _Yes (recommended)_
> 2. _Ask me each time_
> 3. _Never"_

Save with:
```bash
"$DELIVERABLE_ROOT/skills/deliverable/bin/config" set auto_update <true|ask|false>
```

### Step 2: Version check

```bash
"$DELIVERABLE_ROOT/skills/deliverable/bin/update-check"
```

- **`UPGRADE_AVAILABLE <old> <new>` + `auto_update=true`:** Invoke deliverable-upgrade automatically.
- **`UPGRADE_AVAILABLE` + `auto_update=ask`:** Tell user version is available. Continue.
- **No output:** Continue silently.

---

## Routing

If the user's message includes a quoted idea or concept (e.g. `deliverable "build me a CRM"`
or just describes an idea to evaluate) → run **The Spec Trial** below.

If the user explicitly asks for a specific document type:

| Request | Invoke |
|---|---|
| "project charter", "initiative brief" | deliverable-charter |
| "BRD", "business requirements", "spec this feature" | deliverable-brd |
| "SRS", "technical spec", "engineering spec" | deliverable-srs |
| "interview questions", "stakeholder prep" | deliverable-interview |
| "upgrade deliverable" | deliverable-upgrade |
| "use the chain", `--chain` flag | Run chain: charter → brd → srs in sequence |
| "review requirements", "audit the BRD", "check the docs" | Tell the user: _"deliverable-review is no longer a separate skill. To improve an existing BRD or SRS, say 'update the BRD' or 'revise the SRS' — I'll open it and work through it section by section."_ Then offer to open the relevant doc. |
| "red-team this", "challenge this spec", "find holes" | Tell the user: _"Adversarial review is now built into the Spec Trial. Run `deliverable \"[your idea]\"` to put the idea through three hostile voices before speccing it. If you want to challenge an existing spec, share the relevant section and I'll apply the same voices to it."_ |

If ambiguous, default to **The Spec Trial**. Ask the user what their idea is.

If the user provides no idea or a very short phrase (fewer than 5 meaningful words), ask:
_"What's the idea you want to put on trial? Describe it in one sentence."_ Then proceed once they respond.

---

## The Spec Trial

Track progress through the trial:
- [ ] Step 1: Memory lookup
- [ ] Step 2: Voice 1 — Cynical Customer
- [ ] Step 3: Voice 2 — Brutal PM
- [ ] Step 4: Voice 3 — Future Maintainer
- [ ] Step 5: Synthesis — Verdict
- [ ] Step 6: Write verdict.md
- [ ] Step 7: Write to memory
- [ ] Step 8: Present verdict

### Step 1: Memory lookup

```bash
# Determine project name from git repo or current directory
PROJECT_NAME=$(git rev-parse --show-toplevel 2>/dev/null | xargs basename 2>/dev/null || basename "$(pwd)")
DELIVERABLE_DIR=~/.deliverable/$PROJECT_NAME
mkdir -p "$DELIVERABLE_DIR"
DECISIONS_LOG="$DELIVERABLE_DIR/decisions.jsonl"
touch "$DECISIONS_LOG"

```

Take the user's idea text. Extract up to 5 meaningful keywords: lowercase nouns and
verbs, stripped of common stop words and filler words (articles, prepositions,
generic verbs like make/create/get/use, vague nouns like app/tool/platform/system).
If fewer than 5 content words remain, use all available — do not pad with stop words.

**Memory lookup algorithm** (do not use `grep -l`; follow these steps):

1. Read every line of `$DECISIONS_LOG`
2. For each line, count how many of your extracted keywords appear in that line
   (case-insensitive substring match)
3. If the count is ≥ 2 (or ≥ 1 if you extracted fewer than 3 keywords total), that
   line is a match — parse and surface it

Surface matching lines as prior context before the trial begins.

**If prior decisions found:** Show them before starting the trial:

> _"Memory: You evaluated something similar before:_
> - _[date] — "[idea]" → [verdict]: [wedge or top fatal assumption]"_

This context is passed to all three voices.

**If no prior decisions or file is empty:** Proceed silently.

---

### Step 2: Voice 1 — Cynical Customer

Adopt the persona of a skeptical, budget-conscious potential customer who has been burned
by overpromised products before. You are NOT trying to be helpful. You are trying to
find reasons NOT to buy or use this.

Analyze the idea from this perspective. Identify:

1. **Why wouldn't I pay for this?** — What's the cheaper/easier alternative I'm already using?
2. **What trust problem does this create?** — What would make me reluctant to adopt it?
3. **What job am I actually trying to do?** — Is this solving the real job or a symptom?

Output for Voice 1:
- 1–3 **fatal assumptions** (specific, falsifiable — e.g. "assumes users will change
  their existing workflow" not "might have adoption challenges")
- 1 **surviving element** (the one part of the idea that does address a real pain)

---

### Step 3: Voice 2 — Brutal PM

Adopt the persona of a senior product manager who has killed more features than they've
shipped. You have seen every flavor of premature specification and you have zero patience
for unvalidated assumptions.

Analyze the idea from this perspective. Identify:

1. **What assumption is baked in that hasn't been verified?** — The riskiest leap of faith.
2. **What gets cut in sprint 1?** — What sounds essential but will be deferred the moment
   scope pressure hits?
3. **What's the dependency the team hasn't named?** — The prerequisite that makes or breaks this.

Output for Voice 2:
- 1–3 **fatal assumptions** (specific, falsifiable)
- 1 **surviving element** (the one requirement that would survive a scope massacre)

---

### Step 4: Voice 3 — Future Maintainer

Adopt the persona of an engineer who will inherit this codebase 18 months from now,
after the original team has moved on. You are reading the spec trying to understand what
you're getting into.

Analyze the idea from this perspective. Identify:

1. **What breaks in 18 months?** — The part that will require a rewrite as the system grows.
2. **What's the hidden complexity that doubles scope?** — The seemingly simple requirement
   that has an iceberg underneath.
3. **What will the next engineer curse the original team for?** — The design decision that
   feels fine now but creates compounding pain.

Output for Voice 3:
- 1–3 **fatal assumptions** (specific, falsifiable)
- 1 **surviving element** (the architectural choice that would age well)

---

### Step 5: Synthesis — Verdict

Now step out of all personas and act as the synthesis layer.

**Count fatal assumptions per voice** (per-voice, not per-assumption):

| Verdict | Rule | JSONL casing |
|---|---|---|
| **KILL** | 2+ voices each independently raised ≥1 fatal assumption with no reasonable mitigation | `Kill` |
| **NARROW** | Exactly 1 voice raised a fatal assumption that cannot be mitigated, AND the other voices' objections are addressable — but the full idea fails; a smaller wedge survives all 3 | `Narrow` |
| **PROCEED** | All 3 voices' objections can be addressed with evidence or specific design decisions; no voice raised an unmitigatable fatal assumption | `Proceed` |

Two voices each raising one fatal issue = Kill.
One voice raising three issues = not automatically Kill — check if those issues are mitigatable.
When in doubt between NARROW and PROCEED, choose NARROW and name the wedge.

**Determine the narrowest wedge:** The smallest version of this idea that survives all
three voices. Even for Kill verdicts, name the wedge — it might be worth pursuing.

**Determine evidence required:** 3 specific things to learn or validate before writing
specs. Make them concrete (e.g. "Interview 5 freelancers who currently send invoices
manually and ask what they'd pay for automation" not "validate with users").

---

### Step 6: Write verdict.md

Write the verdict to `verdict.md` in the current project directory:

```markdown
# Verdict: [KILL | NARROW | PROCEED]

**Idea:** [user's original idea text]
**Date:** [today's date]

## Why

### Fatal Assumptions
[List the top fatal assumptions identified across all voices. Group by which voices flagged them. Mark assumptions flagged by 2+ voices with ⚠️]

### Voice Summary
- **Cynical Customer:** [1-sentence summary of their main concern]
- **Brutal PM:** [1-sentence summary of their main concern]
- **Future Maintainer:** [1-sentence summary of their main concern]

## Narrowest Wedge

[The smallest version worth testing. Even on a Kill verdict, name it.]

## Evidence Required

Before writing any spec, validate:
1. [Specific thing to learn — concrete, actionable]
2. [Specific thing to learn — concrete, actionable]
3. [Specific thing to learn — concrete, actionable]

## Prior Context

[If memory lookup surfaced a prior decision, summarize it here — date, idea, verdict, top fatal assumption. Omit this entire section if no prior decisions were found.]
```

---

### Step 7: Write to memory

Append to `~/.deliverable/<project-name>/decisions.jsonl` (the `$DECISIONS_LOG` path
set in Step 1). One JSON line:

```json
{"ts":"<ISO8601>","idea":"<user idea text>","keywords":["<kw1>","<kw2>","<kw3>","<kw4>","<kw5>"],"verdict":"<Kill|Narrow|Proceed>","fatal_assumptions":["<assumption1>","<assumption2>"],"wedge":"<narrowest wedge text>","project":"<project-name>"}
```

Use title case in JSONL (`Kill`/`Narrow`/`Proceed`) and all-caps in displayed output (`KILL`/`NARROW`/`PROCEED`). The `keywords` field uses the same 5 keywords extracted in Step 1.

Memory is stored at `~/.deliverable/<project-name>/` — one directory per project,
derived from the git repo name (or current directory if not in a git repo). Projects
never share memory.

---

### Step 8: Present verdict

After writing both files, present the verdict to the user:

- State the verdict clearly: **KILL**, **NARROW**, or **PROCEED**
- Name the top 1–2 fatal assumptions
- State the narrowest wedge
- Tell them `verdict.md` has the full breakdown

Then offer the next step based on the verdict:

- **Kill:** _"The narrowest wedge is [X]. Want to run the trial on that instead?"_
- **Narrow:** _"The surviving wedge is [X]. Want to run `deliverable-brd` on the wedge to spec it out?"_
- **Proceed:** _"Your idea survived the trial. Ready to write the BRD? Say 'write a BRD for [idea]' to continue."_

---

## Shared resources

Load these on demand — only when the task requires it:

- `roles/*.md` — stakeholder personas; load when a voice needs deeper domain grounding
- `templates/*.md` — document templates; load when generating charter, BRD, or SRS output
- `sub-agents/*.md` — sub-agent instructions; load when delegating a step to a sub-agent
- `references/*.md` — load when a fatal assumption maps to a known framework (e.g. Cagan's four risks, Hyrum's Law)

---

## Tone

Direct. The point of the trial is to find what's wrong before you build it. The voices
should be genuinely hostile, not diplomatically cautious. "This assumes users will abandon
a workflow they've used for 10 years" is better than "there may be adoption challenges."

But the synthesis is fair. If the idea survives, say so clearly. Don't manufacture
concerns to seem thorough.

---

## The chain (available via `--chain` or explicit request)

For teams that need formal process documentation, the full chain is still available:

```
deliverable-charter → deliverable-brd → deliverable-srs
                          ↑
                  deliverable-interview (anytime)
```

These work independently and chain through shared artifacts in `docs/requirements/`.
