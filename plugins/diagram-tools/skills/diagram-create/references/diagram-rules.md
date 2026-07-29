# The 12 Rules for Better Architecture Diagrams

Source: https://www.innoq.com/en/blog/2022/09/better-architecture-diagrams/ (Gernot Starke, INNOQ)

Shared rulebook for the diagram skills (`diagram-create`, `diagram-review`, `diagram-report`).

The reasoning behind all of these: diagrams are read far more often than they are drawn, usually by stakeholders without the author available to explain them. Every rule removes a way a reader can misunderstand or give up.

## Content rules

1. **One purpose per diagram.** State the diagram's objective in one sentence — if that sentence needs the word "and", split into two diagrams. Good: "static structure of the business components", "external interfaces with data flow". Bad: structure *and* deployment *and* data flow in one picture.

2. **Legend for every symbol type.** Explain what box shapes, line styles, and arrowheads mean. A box could be source code, a runtime instance, a physical machine, or a plan — the reader must not have to guess. An arrow could mean "calls", "notifies", "data flows", sync or async. If the diagram tool supports it, put the legend in the diagram; otherwise directly beside it.

3. **Use established notation when the audience knows it.** UML, SysML, or C4 give you a shared vocabulary for free — but only if stakeholders actually know it. A custom notation is fine as long as rule 2 is followed. Never *mix* notations (e.g. UML deployment nodes + class inheritance + freehand clouds in one diagram).

4. **Define the names.** A legend explains symbols; a table beside the diagram should explain the *elements*: each component's name and its responsibility in one line. Names alone ("Manager", "Core", "Gateway") carry almost no information. Spell out abbreviations at least once.

5. **Consistent naming across diagrams.** The same element gets the same name everywhere — in every diagram, in the text, and ideally in the code. Don't mix languages (English code with German diagram labels or vice versa).

## Scope rules

6. **Spare with details.** Omit anything better explained in text or likely to change often (exact interface signatures, version numbers, port lists). Show an abstracted interface instead of every endpoint. Split long sequences into several small diagrams instead of one wall-sized one. Prose paragraphs belong in the surrounding document, not inside the image.

7. **~12 elements per diagram.** Static/structure diagrams: about a dozen boxes. Dynamic diagrams (sequences, flows): up to ~20 steps. Larger systems: cluster/abstract at the top level and create separate refinement diagrams that zoom into one cluster each.

## Visual rules

8. **Minimal, consistent styling.** Use color, line style, shading, and icons sparingly, and make each visual difference *mean* something (then put that meaning in the legend). Keep the style uniform within a diagram and across all diagrams in the same document.

9. **Important things in the center.** Place the key element(s) centrally, supporting elements around them. A context diagram is the canonical example: the system in the middle, neighbors around it.

10. **Readable without a microscope.** Font sizes must survive the real output medium — check readability at A4 landscape / typical wiki page width. If a diagram is only readable when zoomed in, it has too many elements (see rule 7).

11. **Basic layout discipline.** Size signals importance. Leave white space. Align boxes to a grid. Put related elements near each other. Balance the picture. One readable font family throughout (not Comic Sans).

12. **Minimize line crossings.** Rearrange elements to avoid crossings. Where a crossing is truly unavoidable, make it visually explicit (line-jump/bridge) so it can't be misread as a junction.

## Quick checklist

- [ ] Title/caption states one purpose, without "and"
- [ ] Every symbol type (box, line, arrow style, color) explained in a legend
- [ ] No mixed notations
- [ ] Element names defined (table) and consistent with other diagrams/code
- [ ] ≤ ~12 elements (≤ ~20 for sequences/flows); details abstracted or moved to refinement diagrams
- [ ] Key element visually central; related elements adjacent; aligned; white space
- [ ] Colors/styles minimal and meaningful
- [ ] Readable at target output size
- [ ] Line crossings minimized, remaining ones marked

## Weighting for reviews

Judge pragmatically — a whiteboard sketch for the team has a lower bar than a diagram in customer-facing docs. Weight the content rules (1–5) over cosmetic ones: an unexplained arrow misleads; a slightly crooked box doesn't.
