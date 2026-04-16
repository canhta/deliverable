# deliverable

**Stop writing requirements docs that nobody reads.**

Most AI tools dump a 20-page BRD in one shot. It looks complete. It's not. Half the assumptions are wrong, the scope is whatever the AI imagined, and the success metrics say "improve user experience." Nobody signs off on it because nobody trusts it.

`deliverable` is different. It's a suite of 7 skills that sit down with you and ask hard questions — the way a good PM or BA would. Each skill handles one part of the process, works section by section with your approval, and produces documents that are evidence-backed, assumption-tagged, and ready for someone to actually build from.

## Skills

| Skill                    | Trigger                                | What it does                                                       |
| ------------------------ | -------------------------------------- | ------------------------------------------------------------------ |
| `deliverable`            | any requirements request                  | Routes to the right skill based on what you need                   |
| `deliverable-charter`        | "project charter", "initiative brief"     | Establishes business justification, vision, stakeholders, go/no-go |
| `deliverable-brd`  | "write a BRD", "spec this feature"        | Intake, research, BRD drafting with role-based interviews          |
| `deliverable-srs` | "write an SRS", "technical spec"          | SRS drafting — architecture, interfaces, NFRs, rollout             |
| `deliverable-critic`    | "challenge this spec", "find holes"       | Adversarial deliverable-review against Cagan's four risks and Hyrum's Law      |
| `deliverable-review`    | "deliverable-review requirements", "audit the BRD"   | Quality audit — completeness, consistency, measurability           |
| `deliverable-interview`  | "prepare deliverable-interview questions"             | Generates structured deliverable-interview templates for real stakeholders     |
| `upgrade`    | "upgrade deliverable"                     | Auto-detects install type and upgrades to latest version           |

### The natural flow

```
deliverable-charter → deliverable-brd → deliverable-srs → deliverable-critic → deliverable-review
                          ↑
                  deliverable-interview (anytime)
```

Each skill works independently, but they chain through shared artifacts.

## Installation

### Claude Code

```
/plugin marketplace add canhta/deliverable
/plugin install deliverable@deliverable
```

### All platforms (npx)

```bash
npx skills add canhta/deliverable
```

### Manual

```bash
git clone https://github.com/canhta/deliverable.git ~/.claude/skills/deliverable
```

### Other platforms

| Platform       | Install path                              |
| -------------- | ----------------------------------------- |
| Cursor         | `~/.cursor/skills/deliverable`            |
| Codex CLI      | `~/.codex/skills/deliverable`             |
| GitHub Copilot | `.agents/skills/deliverable` (in project) |
| OpenCode       | `~/.opencode/skills/deliverable`          |

Also available on the [Claude Skills Marketplace](https://claudemarketplaces.com/skills?search=deliverable).

## Upgrading

### npx (recommended)

```bash
npx skills add canhta/deliverable --yes
```

### Git clone installs

```bash
cd ~/.claude/skills/deliverable && git pull origin main
```

### Inside your agent

Just say "upgrade deliverable" — the upgrade skill handles it automatically.

## Quick start

```
> Write a BRD for our new auth system
> Project deliverable-charter for the data platform migration
> Technical spec for the notification service
> Red-team this spec
> Review the requirements
> I need to interview the CTO about infrastructure constraints
```

## How it works

### Role-based interviews

Each skill interviews you through role lenses, playing whichever roles you're not:

| Role           | Concern                                         |
| -------------- | ----------------------------------------------- |
| Sponsor        | Why now, what outcome, budget of belief         |
| PM             | Target user, problem evidence, metrics, scope   |
| Tech Lead      | Constraints, build/buy, scale, existing systems |
| Designer       | Key flows, usability risks, accessibility       |
| QA             | Acceptance criteria, edge cases, testability    |
| SRE            | SLOs, rollout, observability, failure modes     |
| Security/Legal | Data handling, PII, regulations                 |

### Bounded sub-agents

Research agents dispatched with your approval:

- **Competitor scan** — up to 5 candidates, public info only
- **Feasibility check** — library maturity, licensing, technical blockers
- **Prior art** — existing solutions, patterns, lessons learned
- **Red-team critic** — adversarial review against Cagan's four risks
- **Dual-voice reviewer** — independent second opinion that challenges, not validates

### Adaptive depth

Mention credit card data, HIPAA, or regulated industry and the deliverable-srs skill automatically adds security, privacy, and compliance phases. No mode selection — it just adapts.

### Presets

| Preset     | Use case                      | Adjustments                                        |
| ---------- | ----------------------------- | -------------------------------------------------- |
| greenfield | New product, zero-to-one      | Adds market sizing, competitive landscape, pricing |
| feature    | Feature in existing product   | Heavy integration, migration, deprecation coverage |
| internal   | Internal tools, dev platforms | Adopter persona, API versioning emphasis           |
| auto       | Not sure                      | Skill asks clarifying questions, proposes a preset |

### Extras

Opt-in at intake, generated after main phases:

`prd-lite.md` · `exec-onepager.md` · `rfc.md` · `acceptance-tests.md` · `risks-register.md` · `planning-handoff.md` · `roadmap.md`

## Output

All artifacts go to `docs/requirements/` in your project:

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
│   ├── deliverable/SKILL.md              # Router
│   ├── deliverable-charter/SKILL.md          # Charter authoring
│   ├── deliverable-brd/SKILL.md    # BRD authoring
│   ├── deliverable-srs/SKILL.md   # SRS authoring
│   ├── deliverable-critic/SKILL.md      # Adversarial deliverable-review
│   ├── deliverable-review/SKILL.md      # Quality audit
│   ├── deliverable-interview/SKILL.md    # Interview prep
│   ├── upgrade/SKILL.md                  # Self-updater
│   └── deliverable/
│       ├── templates/                     # 15 artifact templates
│       ├── roles/                         # 7 role-deliverable-interview scripts
│       ├── sub-agents/                    # 5 sub-agent prompt packs
│       ├── references/                    # Cagan, Google design docs, Hyrum's Law
│       └── bin/                           # update-check, config, slug, status, validate, reset
├── .claude-plugin/                        # Claude Code plugin config
├── .cursor-plugin/                        # Cursor plugin config
└── LICENSE
```

## Inspired by

- **[Superpowers](https://github.com/obra/superpowers)** by Jesse Vincent — execution DNA, progressive disclosure, section-by-section validation
- **[gstack](https://github.com/garrytan/gstack)** by Garry Tan — role-as-reviewer, dual-voice adversarial critique, artifact persistence
- **Marty Cagan** — [_Inspired_](https://www.amazon.com/INSPIRED-Create-Tech-Products-Customers/dp/1119387507), [_Empowered_](https://www.amazon.com/EMPOWERED-Ordinary-People-Extraordinary-Products/dp/111969129X) — discovery before delivery, the four risks framework
- **[Software Engineering at Google](https://www.amazon.com/Software-Engineering-Google-Lessons-Programming/dp/1492082791)** — design-doc DNA, trade-offs explicit, alternatives documented
- **Hyrum's Law** — every observable behavior becomes a dependency

## Support

If this is useful, [drop a star](https://github.com/canhta/deliverable/stargazers). Found a bug? [Open an issue](https://github.com/canhta/deliverable/issues).

## Contributing

1. Fork the repo
2. Create a branch
3. Make your changes
4. Open a PR

## License

[MIT](LICENSE)
