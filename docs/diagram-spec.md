# Diagram Specification — standra.ai Architecture Diagrams

Conventions for Excalidraw architecture diagrams used in READMEs and technical docs across all standra.ai projects.

> See [design-system.md](design-system.md) for the full design system. This file covers Surface B (light-canvas Excalidraw architecture diagrams) only.

## Tool

All architecture diagrams are produced with the [excalidraw-diagram skill](https://github.com/coleam00/excalidraw-diagram-skill) installed at `~/.claude/skills/excalidraw-diagram/`. The skill generates `.excalidraw` JSON, then renders it to PNG via Playwright. **No MCP server.** Both files are committed to the repo.

```
project/
├── diagrams/
│   ├── ecosystem.excalidraw     ← editable source
│   ├── ecosystem.png            ← rendered, embedded in README
│   ├── dispatch-flow.excalidraw
│   └── dispatch-flow.png
```

## Canvas

| Property | Value |
|---|---|
| Background | `#ffffff` |
| Grid size | 20 |
| Roughness | 0 (clean / modern) |
| Opacity | 100 (always — use colour for hierarchy, never transparency) |
| Font family | 3 (monospace) for everything |

Most diagrams fit in a 1200–1400 × 800–900 region. Don't stretch beyond unless the content demands it.

## Semantic shape colours

Colours encode meaning. Pick the row that matches the element's role.

| Semantic role | Fill | Stroke | Use for |
|---|---|---|---|
| **Hub / primary** | `#3b82f6` | `#1e3a5f` | The thing the diagram is about. Switchyard in the ecosystem map. |
| **Inputs / context** | `#ddd6fe` | `#6d28d9` | CACP, Sieeve, Axiom — anything feeding context in. |
| **Outputs / success** | `#a7f3d0` | `#047857` | Diagnostics, telemetry, merged results. |
| **Start / trigger** | `#fed7aa` | `#c2410c` | The initiating task or event. |
| **Decision / gate** | `#fef3c7` | `#b45309` | Doctor, gates, conditional checks. |
| **Block / failure** | `#fee2e2` | `#dc2626` | Gate stack, response gate. |
| **Hard error** | `#fecaca` | `#b91c1c` | Rejected, terminal failure states. |
| **AI / LLM** | `#ddd6fe` | `#6d28d9` | Same as inputs — agents and context belong together visually. |

**Rule**: always pair the lighter fill with the darker stroke. Stroke widths: 2 for normal shapes, 3 for emphasised hubs/gates, 4 for the single most important hub.

## Text colours (free-floating labels)

Use colour on labels to create hierarchy without containers.

| Level | Colour | Use for |
|---|---|---|
| Title | `#1e40af` | Diagram title (28px) |
| Subtitle | `#3b82f6` | Subheadings (16–18px) |
| Annotation | `#64748b` | Slate captions under boxes (12px) |
| On light fills | `#374151` | Text inside coloured shapes |
| On dark fills | `#ffffff` | Text inside saturated/dark shapes (the hub) |

## Arrow conventions

| Style | When | Stroke |
|---|---|---|
| Solid arrow | Normal data/control flow | Uses source element's stroke colour |
| Solid + width 3 | Primary "happy path" through the diagram | Source colour |
| Dashed | Failure / block / exceptional paths | `#b91c1c` (block) or `#b45309` (retry) |
| Dotted | Asynchronous / feedback / telemetry edges | Source colour |

Always bind arrow `startBinding` and `endBinding` to actual element IDs — no floating arrows.

## Pattern library

Use the visual pattern that mirrors the concept's behaviour. **Each major concept in a diagram should use a different pattern** — no uniform grids.

| Concept | Pattern | Example |
|---|---|---|
| Many things converge on one | **Hub-and-spoke** (radial) | Switchyard with CACP, Sieeve, Axiom feeding in |
| Linear sequence of stages | **Horizontal pipeline** | Task → Context → Dispatch → Verify → Merge |
| Loop / continuous improvement | **Cycle** with return arrow | Telemetry → Afterburn → Sieeve rules |
| Decision branching | **Diamond** with labelled outgoing edges | Doctor with pass/fail |
| Hierarchy / containment | **Lines + free-floating text** (no boxes) | Component breakdown |

## Layout principles

- **Hierarchy through scale**: hub elements 280×130, primary boxes 180×90, secondary 120×60
- **Whitespace = importance**: most important element has the most empty space around it (≥200px)
- **Flow direction**: left-to-right or top-to-bottom for sequences, radial for hub-and-spoke
- **Section labels**: free-floating uppercase tags (`PROTOCOLS & CONTEXT`, `DIAGNOSTICS & EVAL`) to group radial children, coloured to match their semantic role

## Diagram types per project

Every project should ship at least these two diagrams in its `diagrams/` directory:

| Diagram | Pattern | Shows |
|---|---|---|
| `ecosystem.png` | Hub-and-spoke | Where this project sits in the standra.ai stack |
| `<flow>.png` | Horizontal pipeline | The single most important flow this project owns |

Optional third for complex projects: a sequence diagram (timeline pattern) showing temporal order of events.

## File naming

```
diagrams/
├── ecosystem.excalidraw       (always present)
├── ecosystem.png              (rendered)
├── <name>-flow.excalidraw     (the project's primary flow)
├── <name>-flow.png
└── <other>.excalidraw         (additional diagrams as needed)
```

The `.excalidraw` source file is **always committed** alongside the PNG so anyone can fork and edit.

## Rendering

```bash
cd ~/.claude/skills/excalidraw-diagram/references
uv run python render_excalidraw.py /path/to/diagram.excalidraw
```

Output PNG lands next to the source file. After rendering, **read the PNG** and check:
1. Does it match the conceptual design you planned?
2. Any text clipped, overlapping, or unreadable?
3. Do arrows route cleanly to the right targets?
4. Is the composition balanced?

Iterate until the answer to all four is yes. Typical: 2–3 render cycles per diagram.

## Anti-patterns

| Don't | Do instead |
|---|---|
| Five identical boxes in a row | Vary shapes by semantic role; use the pattern library |
| Arrows that cross other elements | Add waypoints to route around |
| Text larger than the box that contains it | Widen the container or shorten the label |
| Opacity below 100 for "subtlety" | Use a lighter colour from the palette |
| New colours not in this spec | Pick one from the table above; if nothing fits, the concept probably maps to one of the existing roles |
| Floating arrows (no `startBinding`/`endBinding`) | Always bind both ends to element IDs |

## See also

- [design-system.md](design-system.md) — master design system
- [og-spec.md](og-spec.md) — OG image specification (Surface A)
- The excalidraw-diagram skill: `~/.claude/skills/excalidraw-diagram/SKILL.md`
