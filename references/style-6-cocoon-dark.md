# Style 6: Cocoon Dark

Professional dark theme with semi-transparent fills and grid background. Inspired by Cocoon AI's architecture diagram style.

## Colors

```
Background:     #020617  (slate-950)
Grid color:     #1e293b  (slate-800), 40px spacing
Panel fill:     rgba(..., 0.4) per semantic type
Panel stroke:   solid 1.5px per semantic type
Box radius:     6px

Text primary:   #ffffff
Text secondary: #94a3b8  (slate-400)
Text muted:     #475569  (slate-600)
```

### Semantic Component Colors

| Component Type | Fill (rgba) | Stroke | Category |
|---------------|-------------|--------|----------|
| Frontend | `rgba(8, 51, 68, 0.4)` | `#22d3ee` (cyan-400) | frontend |
| Backend | `rgba(6, 78, 59, 0.4)` | `#34d399` (emerald-400) | backend |
| Database | `rgba(76, 29, 149, 0.4)` | `#a78bfa` (violet-400) | database |
| Cloud / AWS | `rgba(120, 53, 15, 0.3)` | `#fbbf24` (amber-400) | cloud |
| Security | `rgba(136, 19, 55, 0.4)` | `#fb7185` (rose-400) | security |
| Message Bus | `rgba(251, 146, 60, 0.3)` | `#fb923c` (orange-400) | message_bus |
| External / Generic | `rgba(30, 41, 59, 0.5)` | `#94a3b8` (slate-400) | external |

## Typography

```
font-family: 'JetBrains Mono', monospace
font-size:   12px component names, 9px sublabels, 8px annotations, 7px tiny labels
font-weight: 400 normal, 600 semi-bold for component names, 700 bold for titles
```

Google Fonts link (HTML output only):
```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
```

For SVG output, use system monospace fallback:
```xml
<style>
  text { font-family: 'JetBrains Mono', 'SF Mono', 'Fira Code', monospace; }
</style>
```

## Background with Grid

```xml
<defs>
  <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
    <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#1e293b" stroke-width="0.5"/>
  </pattern>
</defs>
<rect width="960" height="600" fill="#020617"/>
<rect width="960" height="600" fill="url(#grid)"/>
```

## Box Styles

```xml
<!-- Standard component (e.g., backend) -->
<rect x="X" y="Y" width="W" height="H" rx="6"
      fill="rgba(6, 78, 59, 0.4)" stroke="#34d399" stroke-width="1.5"/>
<text x="CENTER_X" y="Y+20" fill="white" font-size="12" font-weight="600"
      text-anchor="middle" font-family="JetBrains Mono, monospace">API Server</text>
<text x="CENTER_X" y="Y+36" fill="#94a3b8" font-size="9"
      text-anchor="middle" font-family="JetBrains Mono, monospace">FastAPI :8000</text>

<!-- Security group (dashed boundary) -->
<rect x="X" y="Y" width="W" height="H" rx="8"
      fill="transparent" stroke="#fb7185" stroke-width="1" stroke-dasharray="4,4"/>
<text x="X+8" y="Y+14" fill="#fb7185" font-size="8">sg-name :port</text>

<!-- Region boundary (cloud) -->
<rect x="X" y="Y" width="W" height="H" rx="12"
      fill="rgba(251, 191, 36, 0.05)" stroke="#fbbf24" stroke-width="1" stroke-dasharray="8,4"/>
<text x="X+12" y="Y+18" fill="#fbbf24" font-size="10" font-weight="600">AWS Region</text>

<!-- Message bus connector -->
<rect x="X" y="Y" width="120" height="20" rx="4"
      fill="rgba(251, 146, 60, 0.3)" stroke="#fb923c" stroke-width="1"/>
<text x="CENTER_X" y="Y+14" fill="#fb923c" font-size="7" text-anchor="middle">Kafka</text>
```

## Arrow Z-Order (Opaque Mask Technique)

Since this style uses semi-transparent fills, arrows behind components will show through. Use the opaque mask pattern:

1. Draw ALL arrows early in the SVG (after background grid)
2. For each component, draw an opaque background rect THEN the styled rect:

```xml
<!-- Arrows drawn first -->
<line x1="..." y1="..." x2="..." y2="..." stroke="#64748b" stroke-width="1.5" marker-end="url(#arrowhead)"/>

<!-- Component: opaque mask + styled rect -->
<rect x="X" y="Y" width="W" height="H" rx="6" fill="#0f172a"/>
<rect x="X" y="Y" width="W" height="H" rx="6"
      fill="rgba(76, 29, 149, 0.4)" stroke="#a78bfa" stroke-width="1.5"/>
```

The opaque rect (`fill="#0f172a"`) blocks the arrow beneath, then the semi-transparent styled rect renders on top.

## Arrows

```xml
<defs>
  <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
    <polygon points="0 0, 10 3.5, 0 7" fill="#64748b"/>
  </marker>
</defs>

<!-- Standard connection -->
<line stroke="#64748b" stroke-width="1.5" marker-end="url(#arrowhead)"/>

<!-- Auth/security flow (dashed rose) -->
<line stroke="#fb7185" stroke-width="1.5" stroke-dasharray="5,5" marker-end="url(#arrowhead)"/>

<!-- Data flow label -->
<text fill="#94a3b8" font-size="9" text-anchor="middle">HTTPS</text>
```

## Legend

```xml
<text x="X" y="Y" fill="white" font-size="10" font-weight="600">Legend</text>

<rect x="X" y="Y+12" width="16" height="10" rx="2" fill="rgba(8, 51, 68, 0.4)" stroke="#22d3ee" stroke-width="1"/>
<text x="X+22" y="Y+20" fill="#94a3b8" font-size="8">Frontend</text>

<rect x="X" y="Y+28" width="16" height="10" rx="2" fill="rgba(6, 78, 59, 0.4)" stroke="#34d399" stroke-width="1"/>
<text x="X+22" y="Y+36" fill="#94a3b8" font-size="8">Backend</text>

<rect x="X" y="Y+44" width="16" height="10" rx="2" fill="rgba(120, 53, 15, 0.3)" stroke="#fbbf24" stroke-width="1"/>
<text x="X+22" y="Y+52" fill="#94a3b8" font-size="8">Cloud Service</text>

<rect x="X" y="Y+60" width="16" height="10" rx="2" fill="rgba(76, 29, 149, 0.4)" stroke="#a78bfa" stroke-width="1"/>
<text x="X+22" y="Y+68" fill="#94a3b8" font-size="8">Database</text>

<rect x="X" y="Y+76" width="16" height="10" rx="2" fill="rgba(136, 19, 55, 0.4)" stroke="#fb7185" stroke-width="1"/>
<text x="X+22" y="Y+84" fill="#94a3b8" font-size="8">Security</text>

<rect x="X" y="Y+92" width="16" height="10" rx="2" fill="rgba(30, 41, 59, 0.5)" stroke="#94a3b8" stroke-width="1"/>
<text x="X+22" y="Y+100" fill="#94a3b8" font-size="8">External</text>
```

## SVG Template

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 960 600" width="960" height="600">
  <style>
    text { font-family: 'JetBrains Mono', 'SF Mono', 'Fira Code', monospace; }
  </style>
  <defs>
    <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
      <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#1e293b" stroke-width="0.5"/>
    </pattern>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#64748b"/>
    </marker>
  </defs>
  <!-- Background -->
  <rect width="960" height="600" fill="#020617"/>
  <rect width="960" height="600" fill="url(#grid)"/>
  <!-- Arrows (draw first for z-order) -->
  <!-- Components (opaque mask + styled rect) -->
  <!-- Legend -->
</svg>
```
