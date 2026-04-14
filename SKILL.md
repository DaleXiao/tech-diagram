---
name: tech-diagram
description: >-
  Generate production-quality technical diagrams with dual output: SVG+PNG (default)
  or standalone HTML. Supports architecture, data flow, flowchart, sequence, agent,
  memory, mind map, timeline, comparison, UML, ER, and network diagrams.
  Trigger on: "画图" "架构图" "流程图" "可视化" "出图" "generate diagram" "draw diagram"
  "visualize" or any system/flow description the user wants illustrated.
---

# Tech Diagram

Generate production-quality technical diagrams with two output modes:
- **SVG+PNG** (default) — exported via `rsvg-convert`, ideal for docs/presentations
- **HTML** — self-contained shareable file with dark theme, cards, and responsive layout

## Output Modes

### SVG+PNG (Default)

Best for: documentation, blog posts, presentations, GitHub READMEs, embedding in other tools.

1. Write SVG following the technical rules below
2. Validate: `rsvg-convert file.svg -o /dev/null 2>&1`
3. Export: `rsvg-convert -w 1920 file.svg -o file.png`

Output: `./[name].svg` + `./[name].png`

### HTML (Shareable)

Best for: standalone sharing, dashboards, interactive presentations, when you want cards with summary info below the diagram.

Uses `assets/template.html` as base. Customize:
1. Update `<title>` and header text
2. Replace the SVG content in `<div class="diagram-container">`
3. Update the 3 summary cards below the diagram
4. Update footer metadata

The HTML template works with ALL diagram types — just swap the SVG content inside the diagram container. The card section below summarizes key aspects of whatever diagram is shown.

Output: `./[name].html`

**When to choose HTML over SVG+PNG:**
- Sharing via URL or email (self-contained, no external dependencies)
- Need supplementary info cards alongside the diagram
- Dark-themed presentation context
- When the audience will view in a browser

## Workflow

1. **Classify** the diagram type (see Diagram Types below)
2. **Choose output mode** — SVG+PNG (default) or HTML if user requests shareable/web
3. **Extract structure** — identify layers, nodes, edges, flows, semantic groups
4. **Plan layout** — apply layout rules for the diagram type
5. **Load style reference** — default `references/style-1-flat-icon.md`; load matching `references/style-N.md` for exact tokens
6. **Map nodes to shapes** — use Shape Vocabulary below
7. **Check icon needs** — load `references/icons.md` for known products
8. **Write SVG** (or embed in HTML template)
9. **Validate** (SVG mode): `rsvg-convert file.svg -o /dev/null 2>&1`
10. **Export** (SVG mode): `rsvg-convert -w 1920 file.svg -o file.png`
11. **Report** generated file paths

## Diagram Types & Layout Rules

### Architecture Diagram
Nodes = services/components. Group into **horizontal layers** (top→bottom or left→right).
- Typical layers: Client → Gateway/LB → Services → Data/Storage
- Use `<rect>` dashed containers to group related services
- Arrow direction follows data/request flow
- ViewBox: `0 0 960 600` standard, `0 0 960 800` for tall stacks

### Data Flow Diagram
Emphasizes **what data moves where**. Focus on data transformation.
- Label every arrow with the data type (e.g., "embeddings", "query", "context")
- Use wider arrows (`stroke-width: 2.5`) for primary data paths
- Dashed arrows for control/trigger flows
- Color arrows by data category

### Flowchart / Process Flow
Sequential decision/process steps.
- Top-to-bottom preferred; left-to-right for wide flows
- Diamond shapes for decisions, rounded rects for processes, parallelograms for I/O
- Keep node labels short (≤3 words); put detail in sub-labels
- Align on grid: x snap to 120px, y to 80px

### Sequence Diagram
Time-ordered message exchanges between participants.
- Participants as vertical **lifelines** (top labels + vertical dashed lines)
- Messages as horizontal arrows between lifelines, top-to-bottom time order
- Activation boxes (thin filled rects) show active processing
- Group with `<rect>` loop/alt frames
- ViewBox height = 80 + (num_messages × 50)

### Agent Architecture Diagram
Shows how an AI agent reasons, uses tools, and manages memory.
- **Input layer**: User, query, trigger
- **Agent core**: LLM, reasoning loop, planner
- **Memory layer**: Short-term, Long-term, Episodic
- **Tool layer**: Tool calls, APIs, search, code execution
- **Output layer**: Response, action, side-effects
- Use cyclic arrows for iterative reasoning

### Memory Architecture Diagram
Specialized agent diagram focused on memory operations.
- Show memory **write path** and **read path** separately (different arrow colors)
- Memory tiers: Working → Short-term → Long-term → External Store
- Label operations: `store()`, `retrieve()`, `forget()`, `consolidate()`

### Mind Map / Concept Map
Radial layout from central concept.
- Central node at `cx=480, cy=280`
- First-level branches evenly distributed (360/N degrees)
- Use curved `<path>` with cubic bezier for branches

### Timeline / Gantt
Horizontal time axis showing durations, phases, milestones.
- X-axis = time; Y-axis = items/tasks/phases
- Bars: rounded rects, colored by category
- Milestone markers: diamond or filled circle
- ViewBox: `0 0 960 400` typical; `0 0 1200 400` for many periods

### Comparison / Feature Matrix
Side-by-side comparison of approaches/systems.
- Column headers = systems, row headers = attributes
- Row height: 40px; column width: min 120px; header: 50px
- Checked: tinted background + `✓`; unsupported: neutral fill
- Max readable columns: 5

### Class Diagram (UML)
3-compartment boxes (name / attributes / methods) with relationship lines.
- Inheritance: solid + hollow triangle; Interface: dashed + hollow triangle
- Association with multiplicity; Aggregation: hollow diamond; Composition: filled diamond

### Use Case Diagram (UML)
Actors (stick figures) outside system boundary, use cases (ellipses) inside.
- Include/Extend: dashed arrows with stereotypes
- Primary actors left, secondary right

### State Machine Diagram (UML)
States (rounded rects) with transitions (arrows + event/guard/action labels).
- Initial: filled circle; Final: filled circle in hollow circle
- Choice: diamond; Fork/Join: thick bar

### ER Diagram
Entities (rect with header + attributes), relationships (diamond on line).
- PK underlined, FK italic; Cardinality labels (1, N, 0..*)

### Network Topology
Tiered layout: Internet → Edge → Core → Access → Endpoints.
- Subnets/Zones as dashed rect containers
- Device-specific shapes (router=circle+cross, switch=rect+grid, etc.)

## Shape Vocabulary

| Concept | Shape | Notes |
|---------|-------|-------|
| User / Human | Circle + body path | Stick figure or avatar |
| LLM / Model | Rounded rect, double border | Accent color, spark icon |
| Agent / Orchestrator | Hexagon or double-border rounded rect | "Active controller" |
| Memory (short-term) | Rounded rect, dashed border | Ephemeral = dashed |
| Memory (long-term) | Cylinder | Persistent = solid |
| Vector Store | Cylinder with grid lines | 3 horizontal lines inside |
| Graph DB | 3 overlapping circles | |
| Tool / Function | Rect with gear icon | |
| API / Gateway | Hexagon (single border) | |
| Queue / Stream | Horizontal tube (pipe) | |
| File / Document | Folded-corner rect | |
| Browser / UI | Rect with 3-dot titlebar | |
| Decision | Diamond | Flowcharts only |
| Process / Step | Rounded rect | Standard box |
| External Service | Rect with dashed border | |
| Data / Artifact | Parallelogram | I/O in flowcharts |

## Arrow Semantics

| Flow Type | Color | Stroke | Dash | Meaning |
|-----------|-------|--------|------|---------|
| Primary data flow | `#2563eb` | 2px solid | none | Main request/response |
| Control / trigger | `#ea580c` | 1.5px solid | none | Triggering another system |
| Memory read | `#059669` | 1.5px solid | none | Retrieval from store |
| Memory write | `#059669` | 1.5px | `5,3` | Write/store operation |
| Async / event | `#6b7280` | 1.5px | `4,2` | Non-blocking, event-driven |
| Embedding / transform | `#7c3aed` | 1px solid | none | Data transformation |
| Feedback / loop | `#7c3aed` | 1.5px curved | none | Iterative reasoning |

Always include a **legend** when 2+ arrow types are used.

## Layout Rules & Validation

### Spacing
- Same-layer nodes: 80px horizontal, 120px vertical between layers
- Canvas margins: 40px minimum
- Snap to 8px grid: horizontal 120px intervals, vertical 120px intervals

### Arrow Labels (CRITICAL)
- MUST have background rect: `<rect fill="canvas_bg" opacity="0.95"/>` with 4px/2px padding
- Place mid-arrow, ≤3 words, stagger 15-20px when arrows converge
- 10px safety distance from nodes

### Arrow Routing
- Prefer orthogonal (L-shaped) paths to minimize crossings
- Anchor on component edges, not centers
- Route around dense clusters; different y-offsets for parallel arrows
- Jump-over arcs (5px radius) for unavoidable crossings

### Validation Checklist
1. **Arrow-Component Collision**: Arrows MUST NOT pass through component interiors
2. **Text Overflow**: All text MUST fit with 8px padding (`text.length × 7px ≤ shape_width - 16px`)
3. **Arrow-Text Alignment**: Endpoints connect to shape edges; all labels have background rects
4. **Container Discipline**: Arrows enter/leave containers through gaps, not through inner components

## SVG Technical Rules

### Canvas
- ViewBox: `0 0 960 600` default; `0 0 960 800` tall; `0 0 1200 600` wide
- Fonts: embed via `<style>font-family: ...</style>` — **no external `@import`** (breaks rsvg-convert)
- `<defs>`: arrow markers, gradients, filters, clip paths

### Text
- Minimum 12px, prefer 13-14px labels, 11px sub-labels, 16-18px titles
- Use `<clipPath>` if text might overflow a node box

### Arrows
- All arrows use `<marker>` with `markerEnd`, sized `markerWidth="10" markerHeight="7"`
- Drop shadows: `<feDropShadow>` in `<filter>`, apply sparingly

### Arrow Z-Order (Opaque Mask Technique)

For styles with **semi-transparent fills** (style-2, 3, 5, 6), arrows behind components will show through the transparent fill. Use the opaque mask pattern:

1. Draw ALL connection arrows **early** in the SVG (right after background/grid)
2. For each component, draw an **opaque background rect** first, then the styled semi-transparent rect on top:

```xml
<!-- Step 1: Arrows drawn first (behind everything) -->
<line x1="100" y1="200" x2="400" y2="200" stroke="#64748b"
      stroke-width="1.5" marker-end="url(#arrowhead)"/>

<!-- Step 2: Component with opaque mask -->
<!-- Opaque rect blocks the arrow underneath -->
<rect x="180" y="175" width="140" height="50" rx="6" fill="#0f172a"/>
<!-- Semi-transparent styled rect on top -->
<rect x="180" y="175" width="140" height="50" rx="6"
      fill="rgba(76, 29, 149, 0.4)" stroke="#a78bfa" stroke-width="1.5"/>
<text x="250" y="205" fill="white" font-size="12" text-anchor="middle">Database</text>
```

**Note:** Opaque styles (style-1, style-4) don't need this technique — their fills already fully cover arrows beneath.

### Curved Paths
- Use `M x1,y1 C cx1,cy1 cx2,cy2 x2,y2` cubic bezier for loops/feedback arrows

## SVG Generation (Python List Method)

**MANDATORY** — prevents truncation and syntax errors:

```python
python3 << 'EOF'
lines = []
lines.append('<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 960 700">')
lines.append('  <defs>')
# ... each line separately
lines.append('</svg>')

with open('/path/to/output.svg', 'w') as f:
    f.write('\n'.join(lines))
print("SVG generated successfully")
EOF
```

**Pre-Tool-Call Checklist** (use EVERY time):
1. ✅ Can I write out the COMPLETE command/content right now?
2. ✅ Do I have ALL required parameters ready?
3. ✅ Have I checked for syntax errors?

**Error Recovery**: First error → targeted fix. Second → switch method. Third → STOP and report.

**Validation** (after generation):
```bash
rsvg-convert file.svg -o /tmp/test.png 2>&1 && echo "✓ Valid" && rm /tmp/test.png
```

**Common Syntax Errors to Avoid**:
- ❌ `fill=#fff` → ✅ `fill="#ffffff"`
- ❌ Missing `</svg>` at end
- ❌ `marker-end=` → ✅ `marker-end="url(#arrow)"`
- ❌ Unquoted attributes, missing y coordinates

## Styles

| # | Name | Background | Best For | Transparent Fill? |
|---|------|-----------|----------|-------------------|
| 1 | **Flat Icon** (default) | White `#ffffff` | Blogs, docs, presentations | No |
| 2 | **Dark Terminal** | `#0f0f1a` | GitHub, dev articles | Yes (use mask) |
| 3 | **Blueprint** | `#0a1628` | Architecture docs | Yes (use mask) |
| 4 | **Notion Clean** | White `#ffffff` | Notion, wikis | No |
| 5 | **Glassmorphism** | Dark gradient | Product sites, keynotes | Yes (use mask) |
| 6 | **Cocoon Dark** | `#020617` + grid | Shareable HTML, dashboards | Yes (use mask) |

Load `references/style-N-*.md` for exact color tokens and SVG patterns.
Load `references/icons.md` for product-specific icon templates.

## HTML Output Details

The HTML template (`assets/template.html`) provides:

1. **Header** — Project title with pulsing dot indicator + subtitle
2. **Diagram container** — Rounded border card containing the SVG
3. **Summary cards** — Grid of 3 cards below the diagram with key details
4. **Footer** — Minimal metadata line

### Template Customization

| Placeholder | What to Change |
|-------------|----------------|
| `[PROJECT NAME]` | Project/system name in title and h1 |
| `[Subtitle description]` | One-line description |
| SVG `viewBox` | Adjust dimensions for your diagram |
| Component `<rect>` + `<text>` blocks | Your actual components |
| Arrow `<line>` / `<path>` elements | Your connections |
| Card titles + list items | Summary info relevant to diagram |
| Footer text | Project metadata |

### Card Content Guidelines
- **Card 1**: Security / Infrastructure highlights
- **Card 2**: Architecture / Technology choices
- **Card 3**: Data / Integration details
- Adapt card topics to match the diagram type (e.g., for a flowchart: Inputs, Process Steps, Outputs)

## Common Patterns

**RAG Pipeline**: Query → Embed → VectorSearch → Retrieve → Augment → LLM → Response
**Agentic RAG**: adds Agent loop with Tool use between Query and LLM
**Mem0 / Memory Layer**: Input → Memory Manager → [Write: VectorDB + GraphDB] / [Read: Retrieve+Rank] → Context
**Multi-Agent**: Orchestrator → [SubAgent A / B / C] → Aggregator → Output
**Tool Call Flow**: LLM → Tool Selector → Tool Execution → Result Parser → LLM (loop)
