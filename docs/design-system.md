# standra.ai Design System

The single source of truth for visual identity across every standra.ai project — OG images, architecture diagrams, slides, web, and any other visual artifact.

## Why this exists

Every project in the standra.ai family ships visuals: OG images, architecture diagrams, screenshots, slides. Without a system, each one drifts. This document defines the rules so any contributor (human or agent) produces consistent output without negotiating colors or fonts.

---

## 1. Brand axis

The standra.ai parent brand uses a **blue → purple → magenta** gradient. Every sub-project picks a 4-stop gradient that lives somewhere on a colour wheel and reads as part of the same family.

| Project | Stop 1 | Stop 2 | Stop 3 | Stop 4 | Metaphor |
|---|---|---|---|---|---|
| **standra** (parent) | `#0066ff` | `#5544ff` | `#aa44ff` | `#ff44dd` | platform / hub |
| **Dispatch** | `#0066ff` | `#3388ff` | `#55aaff` | `#88ccff` | rail / signal |
| **Sieeve** | `#8833ff` | `#9955ff` | `#aa77ff` | `#cc99ff` | filter / prism |
| **Axiom** | `#6644dd` | `#8866ee` | `#aa88ff` | `#ccaaff` | rule / law |
| **CACP** | `#3344ff` | `#5566ff` | `#7788ff` | `#99aaff` | protocol / wire |
| **Afterburn** | `#ff4500` | `#ff6b35` | `#ffa500` | `#ffd700` | fire / residue |
| **PawBench** | `#ff3366` | `#ff5588` | `#ff77aa` | `#ff99cc` | benchmark / scorecard |
| **ServingCard** | `#00cc99` | `#33ddaa` | `#66eebb` | `#99ffcc` | spec / chip |

**Rule**: pick the gradient that matches the project's *behaviour*, not just an unused colour. The metaphor column is non-negotiable — it's how someone scanning the family recognises what each project does.

---

## 2. Background & neutrals

Every visual uses the same canvas and neutral text scale. **Do not invent new neutrals.**

### Background (always)
```
Primary:   #0a0a0f  (near-black)
Secondary: #0f1729  (dark navy)
Gradient:  linear 0% → 50% → 100% diagonal
           #0a0a0f → #0f1729 → #0a0a0f
```

### Text scale
```
Title:        #ffffff  (pure white)
Subtitle:     #8899aa  (muted blue-gray)
Body:         #667788  (dim)
Annotation:   #556677
Subtle/grid:  #445566
Footer/dim:   #334455
```

### Grid overlay (OG images and dark visuals)
```
Color:    #ffffff at 4% opacity
Stroke:   0.5px
Spacing:  60px
```

---

## 3. Typography

Two font families. Monospace for technical content, Inter for narrative.

| Use | Font Stack | Weight |
|---|---|---|
| Titles, code, technical labels | `'SF Mono', 'Fira Code', 'JetBrains Mono', 'Cascadia Code', monospace` | 500–800 |
| Taglines, descriptions, footers | `'Inter', 'Helvetica Neue', Arial, sans-serif` | 400–600 |

### Size scale (OG images, 1200×630 canvas)
| Element | Size | Weight | Letter-spacing |
|---|---|---|---|
| Hero title | 82px | 800 | 6px |
| Tagline | 24px | 400 | 1px |
| Pill label | 13px | 500 | default |
| Architecture label | 10px | 400 | default |
| Footer | 13–14px | 400 | default |

### Size scale (Excalidraw diagrams)
| Element | Size | Notes |
|---|---|---|
| Section title | 28px | Free-floating, no container |
| Subtitle | 16px | Slate `#64748b` |
| Hub label | 22px | White on filled hub |
| Component label | 14px | Inside coloured boxes |
| Annotation | 12px | Below boxes, slate |

---

## 4. Two visual surfaces

standra.ai diagrams and visuals split into two surfaces with different rules:

### Surface A — OG / brand visuals (dark canvas)
- 1200×630 SVG, dark gradient background
- Hero title + tagline + pills + architecture mini-diagram + footer
- Used for: OG images, slide intros, brand banners
- **Spec**: [docs/og-spec.md](og-spec.md)

### Surface B — Architecture diagrams (light canvas)
- Excalidraw `.excalidraw` JSON rendered to PNG
- White background, semantic shape colours, clean strokes
- Used for: README architecture diagrams, technical documentation
- **Spec**: [docs/diagram-spec.md](diagram-spec.md)

The brand axis (Section 1) is shared. The neutrals and typography differ because dark and light surfaces need different contrast handling — see each spec for details.

---

## 5. Tooling

| Surface | Tool | Where |
|---|---|---|
| OG images | Hand-written SVG | `docs/og-image.svg` per repo |
| Architecture diagrams | [excalidraw-diagram skill](https://github.com/coleam00/excalidraw-diagram-skill) | Installed at `~/.claude/skills/excalidraw-diagram/` |
| Diagram colours | Skill's `references/color-palette.md` | Mapped to standra's brand axis |

To produce a new diagram:
1. Identify the project (pick its accent gradient from §1)
2. Pick the surface (A or B)
3. Read the corresponding spec
4. Generate the file
5. For Excalidraw: render to PNG via the skill's render script

---

## 6. Per-project customisation checklist

When adding a new project to standra.ai, only these things change. Everything else is shared.

- [ ] Pick a 4-stop accent gradient (Section 1) and add it to this table
- [ ] Pick a metaphor (one word, max two)
- [ ] Generate `docs/og-image.svg` following [og-spec.md](og-spec.md)
- [ ] Generate architecture diagrams following [diagram-spec.md](diagram-spec.md)
- [ ] Embed both in the project's README
- [ ] Add a "Part of standra.ai" badge to the OG image (Section 7)

---

## 7. The standra.ai badge

All sub-project OG images include a small badge bottom-right linking to the parent brand.

```
Position:  x=920, y=548
Size:      220×40, rx=6
Border:    #334455, 1px stroke, no fill
Line 1:    "Part of"  (11px Inter, #556677)
Line 2:    "standra.ai" (14px Inter bold, #8899aa)
```

The standra parent repo's OG image replaces this with `Open source / standra.ai` since it *is* standra.ai.

---

## 8. Anti-patterns

Don't do these. Each one breaks the system.

| Don't | Do instead |
|---|---|
| Invent a new accent colour | Pick one from §1, or add a new project to the table first |
| Use Comic Sans / Helvetica / Times | Monospace for technical, Inter for narrative — that's it |
| Use a white background for OG images | Dark canvas only (Surface A) |
| Use the standra purple gradient on a sub-project | The parent brand owns the full blue→magenta range |
| Skip the badge | Every sub-project links back. No exceptions. |
| Use opacity for hierarchy | Use colour and size. Opacity stays at 100. |

---

## See also

- [og-spec.md](og-spec.md) — full OG image specification
- [diagram-spec.md](diagram-spec.md) — Excalidraw diagram conventions
