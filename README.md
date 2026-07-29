# Nirumand Skills — Claude Code Plugin Marketplace

A public [Claude Code](https://claude.com/claude-code) plugin marketplace by **Reza Nirumand**, hosting agentic skills and agents as installable plugins.

## Installation

Add the marketplace once:

```
/plugin marketplace add nirumand/skills
```

Then install any plugin from it:

```
/plugin install diagram-tools@nirumand-skills
```

To update later:

```
/plugin marketplace update nirumand-skills
```

## Available Plugins

### diagram-tools

Create, review, and report on software architecture diagrams following the 12 rules from INNOQ's ["Better Architecture Diagrams"](https://www.innoq.com/en/blog/2022/09/better-architecture-diagrams/) (Gernot Starke).

| Component | Type | What it does |
|---|---|---|
| `diagram-create` | Skill | Creates architecture diagrams (Mermaid/PlantUML) from requirements, with a mandatory self-verification checklist before delivery. Triggers on "create/draw/generate a diagram". |
| `diagram-review` | Skill | Reviews an existing diagram (image, Mermaid, PlantUML, draw.io) against all 12 rules — findings table, prioritized fixes, and a rule-compliant redraw. Triggers on "review/check/critique this diagram". |
| `diagram-report` | Skill | Renders a completed review as a standalone shareable HTML report: original, findings, fixes, and redraw side by side. |
| `diagram-reviewer` | Agent | Reviews one diagram and returns a compact findings report. Designed for bulk reviews — fan out one agent per diagram in parallel so image reads stay out of the main context. |

**Usage examples:**

```
> Create a context diagram for our payment service
> Review docs/architecture/overview.png — is this diagram good?
> Review all diagrams in docs/ and rank them
> Generate an HTML report for that review
```

**How the pieces fit together:**

1. `diagram-create` holds the shared rulebook (`references/diagram-rules.md`) — the 12 rules, checklist, and review weighting used by everything else.
2. `diagram-review` reviews a single diagram inline; for many diagrams it fans out `diagram-reviewer` agents in parallel.
3. `diagram-report` turns review output into a portable HTML file (Mermaid redraw rendered live via CDN).

**Attribution:**

This plugin is drafted from the article ["Better Architecture Diagrams — 12 rules"](https://www.innoq.com/en/blog/2022/09/better-architecture-diagrams/) by **Gernot Starke** ([INNOQ](https://www.innoq.com/)). The 12 rules, the checklist, and the review weighting embedded in the skills are a condensed adaptation of that work — full credit for the underlying methodology belongs to its author. If you find the plugin useful, read the original article.

## Repository Structure

```
.
├── .claude-plugin/
│   └── marketplace.json          # Marketplace manifest (lists all plugins)
├── plugins/
│   └── diagram-tools/
│       ├── .claude-plugin/
│       │   └── plugin.json       # Plugin manifest
│       ├── agents/
│       │   └── diagram-reviewer.md
│       └── skills/
│           ├── diagram-create/
│           │   ├── SKILL.md
│           │   └── references/diagram-rules.md
│           ├── diagram-review/
│           │   └── SKILL.md
│           └── diagram-report/
│               ├── SKILL.md
│               └── assets/report-template.html
├── evals/                            # Eval suite (dev tooling, not shipped with the plugin)
│   ├── fixtures/                     # Test diagrams and pre-baked review
│   └── <skill-name>/                 # evals.json + trigger-evals.json per skill
├── LICENSE
└── README.md
```

## Evals

Each skill has a thin eval suite under [`evals/`](evals/): trigger-accuracy prompts (should/should-not invoke the skill, with deliberate near-misses) and end-to-end scenarios with objective format assertions (12-row findings table, legend present, no unfilled template placeholders). Subjective quality is left to human review by design. See [`evals/README.md`](evals/README.md) for how to run them.

## Adding a New Plugin

1. Create `plugins/<plugin-name>/` with a `.claude-plugin/plugin.json` manifest.
2. Add `skills/`, `agents/`, `commands/`, or `hooks/` directories as needed — Claude Code auto-discovers them.
3. Register the plugin in `.claude-plugin/marketplace.json` under `plugins`.
4. Reference files across skills with `${CLAUDE_PLUGIN_ROOT}/...` — never absolute paths, so the plugin works wherever it is installed.

## License

[MIT](LICENSE) © Reza Nirumand
