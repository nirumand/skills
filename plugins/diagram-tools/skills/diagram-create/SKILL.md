---
name: diagram-create
description: Create software architecture diagrams from requirements, following the 12 rules from INNOQ's "Better Architecture Diagrams", with a mandatory self-verification step before delivery. Use this skill whenever the user asks to create, draw, sketch, or generate an architecture diagram, system diagram, context diagram, component diagram, deployment diagram, sequence diagram, flowchart, or C4/UML/Mermaid/PlantUML diagram — even if they just say "diagram my system" or "visualize this architecture". Also use it when producing diagrams as part of documentation (arc42, ADRs, READMEs) or when redrawing a diagram after a review.
---

# Diagram Create

Read the rulebook first: `references/diagram-rules.md` (the 12 rules + checklist). Every step below refers to it.

## Workflow

1. **Clarify purpose and audience.** Ask (or infer from context): what single question should this diagram answer, and for whom? Write the objective as one sentence and put it in the diagram title or caption. This drives every other decision — notation choice, abstraction level, element count. If the sentence needs "and", plan two diagrams.

2. **Choose the abstraction level** so the result lands under ~12 elements (rule 7). If the system is bigger, plan a small hierarchy: one overview diagram + refinement diagrams that each zoom into one cluster (C4's context → container → component levels are a good template).

3. **Pick notation** the audience knows (rule 3). For developer-facing docs, Mermaid or PlantUML with C4/UML conventions is usually right; for mixed audiences, simple boxes-and-arrows with a strict legend. Default output format is Mermaid (renders in GitHub/GitLab/most wikis) unless the user or the target document says otherwise.

4. **Draw it** applying rules 8–12: minimal meaningful styling, key element central, aligned layout, no line crossings. Choose a flow direction (top-down or left-right) that matches the reading order of what's shown.

5. **Add the legend and element table** (rules 2 and 4). For Mermaid, the legend can be a small subgraph or text directly beside the diagram; the element table goes in the surrounding document. Spell out every abbreviation once.

6. **Verify before delivering — not optional.** Walk the checklist in `references/diagram-rules.md` against your own output and state the result. Two common self-inflicted failures: forgetting the legend, and exceeding the element budget by inlining details. If any item fails, fix it before presenting. Then tell the user which trade-offs you made (e.g. "clustered the 5 backend services into one box to stay under 12 elements — refinement diagram available on request").

## Output

Deliver:

1. The diagram source (Mermaid/PlantUML code block, or file if the user named a target file)
2. Legend (in-diagram or directly beneath)
3. Element table (name → responsibility, one line each)
4. One-line verification note: which checklist items passed, which trade-offs were made

## Related skills

- Reviewing an existing diagram → `diagram-review`
- HTML report with original / issues / redraw → `diagram-report`
