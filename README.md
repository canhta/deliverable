# deliverable

**Stop building the wrong thing.**

Most AI tools dump a 20-page BRD in one shot. It looks complete. It's not. Half the
assumptions are wrong, the scope is hallucinated, and the success metrics say "improve
user experience." Teams build for three months and ship something nobody wanted.

`deliverable` catches bad ideas before a single line of code gets written.

## The Spec Trial

Drop in an idea. It goes on trial.

```
deliverable "AI CRM for freelancers"
```

On the second run with a related idea, it surfaces what you learned last time:
_"Memory: You evaluated 'AI CRM for freelancers' in April — verdict: Narrow. Fatal assumption: cold-start network effect. What changed?"_

Three hostile voices take a run at it:

- **Cynical Customer** — why wouldn't I pay for this? what's cheaper?
- **Brutal PM** — what assumption is baked in that you haven't verified?
- **Future Maintainer** — what breaks in 18 months?

Then a synthesis produces a one-page verdict:

```
Verdict:  NARROW
Why:      Cold-start network effect (PM + Maintainer). Freelancers won't change
          invoice workflow for <50 contacts (Customer).
Wedge:    Invoice follow-up automation only — no CRM, no contacts, just reminders.
Evidence: Interview 5 freelancers who currently send manual follow-ups. Ask what
          they'd pay to never write a follow-up email again.
```

`verdict.md` goes in your project. Every verdict is saved to `~/.deliverable/decisions.jsonl`.
Next time you bring a similar idea, it remembers: _"You killed something like this in April
because of cold-start. What changed?"_

## Installation

```bash
npx skills add canhta/deliverable
```

Other platforms:

| Platform       | Install path                              |
| -------------- | ----------------------------------------- |
| Claude Code    | `/plugin install deliverable@deliverable` |
| Cursor         | `~/.cursor/skills/deliverable`            |
| Codex CLI      | `~/.codex/skills/deliverable`             |
| GitHub Copilot | `.agents/skills/deliverable` (in project) |
| OpenCode       | `~/.opencode/skills/deliverable`          |

Also on the [Claude Skills Marketplace](https://claudemarketplaces.com/skills?search=deliverable).

## Usage

**Run the trial on an idea:**
```
deliverable "your idea here"
```

**Write formal requirements after the trial:**
```
write a BRD for [the surviving wedge]
write an SRS for [the feature]
project charter for [the initiative]
I need to interview the CTO before writing this spec
```

**Upgrade:**
```
upgrade deliverable
```

## How the memory works

Every verdict is appended to `~/.deliverable/<project-name>/decisions.jsonl` (one directory per project, derived from the git repo name):

```json
{"ts":"2026-04-16T15:16:42Z","idea":"AI CRM for freelancers","keywords":["crm","freelancer","ai","client","billing"],"verdict":"Narrow","fatal_assumptions":["cold-start network effect","freelancers won't pay > $20/mo"],"wedge":"Invoice follow-up automation only","project":"my-project"}
```

Each project gets its own directory under `~/.deliverable/` — derived from the git repo
name. Projects never share memory. When you run the trial on a new idea, deliverable
greps that project's history for keyword overlap and surfaces relevant prior verdicts
before the voices start. No database, no service — just a log per project that makes
the tool smarter over time.

## The full chain (for teams with formal process)

```
deliverable-charter → deliverable-brd → deliverable-srs
                          ↑
                  deliverable-interview (anytime)
```

Each skill works independently and chains through shared artifacts in `docs/requirements/`.
Use `deliverable --chain` to run the full sequence, or invoke individual skills directly.

## Skills

| Skill | What it does |
| --- | --- |
| `deliverable` | **The Spec Trial** — judges your idea, writes verdict.md, logs to memory |
| `deliverable-charter` | Project charter — business justification, vision, stakeholders, go/no-go |
| `deliverable-brd` | BRD — intake, research, requirements with role-based interviews |
| `deliverable-srs` | SRS — architecture, interfaces, NFRs, rollout |
| `deliverable-interview` | Interview prep — structured templates for real stakeholder conversations |
| `deliverable-upgrade` | Self-updater — auto-detects install type, upgrades to latest |

## Output

Trial output:
```
verdict.md                                    ← in your project directory
~/.deliverable/
└── <project-name>/
    └── decisions.jsonl                       ← per-project memory log
```

Chain output (in `docs/requirements/`):
```
docs/requirements/
├── charter.md
├── brd.md
├── srs.md
├── decisions.md
├── open-questions.md
├── research/
├── interviews/
└── <extras>.md
```

## Project structure

```
.
├── skills/
│   ├── deliverable/
│   │   ├── SKILL.md                   # Spec Trial entry point
│   │   ├── templates/                 # Document templates
│   │   ├── roles/                     # Role-interview scripts
│   │   ├── sub-agents/                # Sub-agent prompt packs
│   │   ├── references/                # Cagan, Hyrum's Law, Google design docs
│   │   └── bin/                       # update-check, config, slug, status, validate, reset
│   ├── deliverable-charter/SKILL.md
│   ├── deliverable-brd/SKILL.md
│   ├── deliverable-srs/SKILL.md
│   ├── deliverable-interview/SKILL.md
│   └── deliverable-upgrade/SKILL.md
├── .claude-plugin/
├── .cursor-plugin/
└── LICENSE
```

## Inspired by

- **[Superpowers](https://github.com/obra/superpowers)** by Jesse Vincent — execution DNA, progressive disclosure
- **[gstack](https://github.com/garrytan/gstack)** by Garry Tan — role-as-reviewer, adversarial critique
- **Marty Cagan** — [_Inspired_](https://www.amazon.com/INSPIRED-Create-Tech-Products-Customers/dp/1119387507) — discovery before delivery, the four risks
- **[Software Engineering at Google](https://www.amazon.com/Software-Engineering-Google-Lessons-Programming/dp/1492082791)** — trade-offs explicit, alternatives documented
- **Hyrum's Law** — every observable behavior becomes a dependency

## Support

[Drop a star](https://github.com/canhta/deliverable/stargazers) if this is useful.
[Open an issue](https://github.com/canhta/deliverable/issues) if something's wrong.

## License

[MIT](LICENSE)
