---
name: diagram-review
description: Review an existing architecture diagram (image, Mermaid, PlantUML, draw.io) against the 12 rules from INNOQ's "Better Architecture Diagrams" — produces a findings report with verdicts per rule, prioritized fix actions, and a rule-compliant redraw. Use this skill whenever the user asks to review, check, verify, critique, audit, or improve an existing diagram, asks "is this diagram good?", or points at a diagram file/image/screenshot and wants feedback. Also use it when diagrams turn up during doc reviews (arc42, ADRs, READMEs, wikis) and their quality is in question.
---

# Diagram Review

Read the rulebook first: `${CLAUDE_PLUGIN_ROOT}/skills/diagram-create/references/diagram-rules.md` (the 12 rules, checklist, and review weighting).

## Input handling

- **Diagram source** (`.mmd`, `.puml`, `.drawio`, Mermaid/PlantUML block in markdown): read the source. Fixes can later be applied directly to it.
- **Images** (PNG/JPG, screenshots, whiteboard photos, PDF pages): Read the file directly — images render visually. The Read tool detects images by file extension; if the file has no extension (or a wrong one), check the real type with `file <path>`, copy it to the scratchpad with the correct extension, and Read the copy.
- Image review is read-only: findings plus a redraw as diagram-as-code (Mermaid/PlantUML). The original image cannot be edited.
- If text in the image is unreadable due to resolution, say so — that is itself a rule 10 finding, not just a tooling problem.

## Review

Check the diagram against all 12 rules. Report in this exact format:

```
## Diagram review: <name/path>

**Purpose (rule 1):** <stated / inferable / unclear — what is it?>

| # | Rule | Verdict | Finding |
|---|------|---------|---------|
| 1 | One purpose | ⚠️ | Mixes static structure with deployment |
| 2 | Legend | ❌ | Dashed vs. solid arrows unexplained |
| ... all 12 rows ... |

**Top 3 fixes (highest impact first):**
1. ...
2. ...
3. ...
```

Verdicts: ✅ pass, ⚠️ partial, ❌ fail, ➖ not applicable. Apply the weighting guidance from the rulebook: content rules (1–5) over cosmetic ones, and calibrate to the diagram's audience and formality.

Each top fix must be an *action* ("draw fan-out to show the parallelism", "add legend distinguishing sync/async arrows"), not a restatement of the finding.

## Redraw

After the report, produce a redraw applying the fixes. A redraw is a full create — follow the `diagram-create` workflow steps 4–6, including legend, element table, and the checklist self-check. It's easy to focus the redraw on the specific findings and forget the baseline requirements (the legend is the most commonly dropped one); the checklist exists to catch exactly that.

- User provided source → apply fixes directly in the source format.
- User provided an image → redraw as Mermaid (or PlantUML if their docs use it).
- If the diagram violates rule 7 badly, the correct redraw may be *two* diagrams (overview + refinement) — say so rather than cramming.

## Bulk reviews

Reviewing several diagrams (e.g. "review all diagrams in docs/") means several image reads — don't pull them all into the main context. Fan out the `diagram-reviewer` agent, one per diagram, in parallel; each returns a compact findings report. Aggregate the reports and rank the diagrams by score.

## Optional: HTML report

After delivering the review, offer an HTML report showing original, issues, and redraw side by side — useful for sharing with the diagram's author or a team. If the user wants it, use the `diagram-report` skill.
