# Evals for diagram-tools

A thin eval suite for the three skills in `plugins/diagram-tools`. It covers the two things worth checking automatically — **trigger accuracy** and **output-format compliance** — and deliberately skips subjective quality scoring (no LLM judge).

These files live outside `plugins/` on purpose: they are development tooling and must not ship into user installs.

## Layout

```
evals/
├── fixtures/
│   ├── diagrams/                 # reviewable flawed diagrams — safe to scan as a directory
│   │   ├── order-flow.mmd        # no legend, cryptic names (rule 2 fail, rule 4 fail pinned)
│   │   └── user-signup.mmd       # 13 elements, unexplained hex colors, cryptic names
│   └── report-input/
│       └── review-findings.md    # pre-baked completed review — isolates diagram-report
├── diagram-create/
│   ├── evals.json                # scenario evals with format assertions
│   └── trigger-evals.json        # should-trigger / should-not-trigger prompts
├── diagram-review/
│   ├── evals.json
│   └── trigger-evals.json
└── diagram-report/
    ├── evals.json
    └── trigger-evals.json
```

Two layout rules worth knowing:

- `review-findings.md` lives outside `fixtures/diagrams/` **deliberately** — the bulk-review eval scans that directory, and a pre-baked review of one of its diagrams sitting next to it would be an answer key a lazy run could transcribe.
- All `files` paths and paths inside prompts are **relative to the repo root** — run evals with the repo root as working directory.

## What each file checks

- **`evals.json`** — end-to-end scenarios in the [skill-creator](https://github.com/anthropics/claude-plugins-official) schema. Each eval has a realistic prompt plus `expectations`: objectively checkable statements about the output format (e.g. "findings table has exactly 12 rule rows", "a legend is present", "no `__TITLE__` placeholder remains in the HTML"). A grader agent evaluates each expectation against the run output.

- **`trigger-evals.json`** — prompts that *should* and *should not* invoke the skill. The negatives are near-misses on purpose (e.g. "review this PR", "fix my mermaid syntax", "ER diagram from prisma schema") — obviously-irrelevant prompts test nothing. Format matches the skill-creator description-optimization loop: `[{"query": "...", "should_trigger": true}]`.

## Running

Both suites run via the `skill-creator` plugin skill in Claude Code:

- **Scenario evals**: ask Claude to run the skill-creator eval loop against a skill, pointing it at the skill path (`plugins/diagram-tools/skills/<name>`) and the matching `evals/<name>/evals.json`. It spawns with-skill and baseline runs, grades the expectations, and opens a review viewer.

- **Trigger evals**: ask Claude to run the skill-creator description-optimization loop with `--eval-set evals/<name>/trigger-evals.json --skill-path plugins/diagram-tools/skills/<name>`. It reports trigger accuracy per query and can propose an improved `description` if accuracy is low.

## Scope decisions

- Format assertions only — whether a redraw is *beautiful* is subjective and would need a validated LLM judge; not worth it at this size.
- The fixture diagrams are intentionally bad in known ways (no legend, cryptic names, unexplained colors), so review assertions pin specific verdicts — both negative (rule 2 must fail) *and* positive (rule 7 must pass: 9 elements) — a run that marks everything ❌ fails too.
- `diagram-report` is tested in isolation via the pre-baked review fixture, so a report failure can't be caused by an upstream review failure. A second scenario tests the prerequisite gate (report requested with no review existing).
- The review fixture's redraw contains a literal `&` ("Pricing & Tax Core") so the HTML-escaping assertion is actually falsifiable.
- Trigger negatives target the sibling-skill boundaries (create vs. review vs. report) — the likeliest real-world misfires.
- This suite was adversarially verified by independent reviewer agents; ambiguous trigger labels (e.g. ER-diagram generation) and non-discriminating assertions were removed as a result.
