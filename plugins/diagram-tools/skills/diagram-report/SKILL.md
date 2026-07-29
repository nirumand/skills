---
name: diagram-report
description: Generate a standalone HTML report for a diagram review — original diagram, findings per rule, prioritized fixes, and the rule-compliant redraw rendered side by side. Use this skill when the user asks for an HTML report, a shareable review page, or a visual before/after comparison of a diagram review, or accepts the HTML-report offer made by the diagram-review skill. Requires review findings and a redraw to already exist (run diagram-review first if not).
---

# Diagram Report (HTML)

Turns the output of `diagram-review` into a single self-contained HTML file: original diagram, findings table, top fixes, and the redraw rendered live via Mermaid.

Rulebook (for reference in the report): `${CLAUDE_PLUGIN_ROOT}/skills/diagram-create/references/diagram-rules.md`

## Prerequisites

A completed review: findings table, top fixes, redraw source. If they don't exist yet, run the `diagram-review` skill first — this skill only renders, it doesn't re-review.

## Build the report

1. Copy `${CLAUDE_PLUGIN_ROOT}/skills/diagram-report/assets/report-template.html` to the output location. Default: `./diagram-review-<name>.html` in the current directory, unless the user names a path.

2. Fill the placeholders:

   | Placeholder | Content |
   |---|---|
   | `__TITLE__` | Diagram name/path |
   | `__DATE__` | Review date |
   | `__PURPOSE__` | Purpose assessment (rule 1) from the review |
   | `__ORIGINAL__` | Original diagram — see step 3 |
   | `__FINDINGS_ROWS__` | `<tr>` rows: rule #, rule name, verdict emoji, finding text (all 12) |
   | `__FIXES__` | `<li>` items for the top fixes |
   | `__REDRAW_MERMAID__` | Redraw Mermaid source, raw (inside the `<pre class="mermaid">`) |
   | `__REDRAW_SOURCE__` | Same source, HTML-escaped (inside the copyable `<pre><code>`) |
   | `__LEGEND__` | Legend text/table for the redraw |
   | `__ELEMENT_TABLE_ROWS__` | `<tr>` rows: element name, meaning/responsibility |

3. **Original diagram embedding** — the report must stay a single portable file:
   - Image: base64 data-URI (`<img src="data:image/png;base64,...">`). Generate with `base64 -i <file>`.
   - Mermaid source: second `<pre class="mermaid">` block, rendered live.
   - Other source (PlantUML/draw.io): escaped code block; note that it isn't rendered.

4. **Deliver.** Tell the user the file path. If a Lavish session tool (`lavish-axi`) is available and the user wants to annotate/give feedback, open it with `lavish-axi <file>`; otherwise `open <file>` on macOS.

Note: the redraw renders via the Mermaid CDN — it needs network access when opened. Everything else works offline.
