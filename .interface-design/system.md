# Feldstation — Interface Design System

## Direction & Feel
Dark intelligence center / field station control room for monitoring wildlife YouTube channels. Data-dense, focused, purposeful. Like a nature documentary control room — data flowing in, critical signals highlighted.

## Domain
Naturforschung, Expeditionslogbuch, Feldstation, Artenkatalog, Beobachtungsposten. Channels are observation posts, viral videos are discoveries, KPIs are field measurements.

## Token System

### Depth Layers
- `--abyss: #0a0e14` — darkest background (app canvas)
- `--deep: #111820` — main surface
- `--shelf: #1a2230` — elevated surface (cards, panels)
- `--drift: #222d3d` — highest surface (modals, dropdowns)
- `--cavity: #2a3649` — inset/recessed areas

### Signal Colors
- `--signal: #22d3ee` — cyan accent, primary highlight (bioluminescence)
- `--signal-dim: rgba(34, 211, 238, 0.15)` — signal background tint
- `--signal-glow: rgba(34, 211, 238, 0.4)` — signal emphasis glow
- `--discovery: #f59e0b` — amber, viral/hot markers
- `--discovery-dim: rgba(245, 158, 11, 0.15)` — discovery background
- `--discovery-glow: rgba(245, 158, 11, 0.5)` — discovery emphasis
- `--growth: #34d399` — green, positive trends
- `--growth-dim: rgba(52, 211, 153, 0.15)` — growth background
- `--decline: #ef4444` — muted red, negative trends
- `--decline-dim: rgba(239, 68, 68, 0.15)` — decline background

### Text Hierarchy
- `--bone: #e8edf4` — primary text
- `--sediment: #94a3b8` — secondary text
- `--fossil: #526580` — muted/tertiary text

### Borders
- `--reef: rgba(148, 163, 184, 0.08)` — subtle borders
- `--trench: rgba(148, 163, 184, 0.18)` — stronger borders

## Depth Strategy
Borders-only with very subtle surface color shifts. No shadows. Dark mode where borders define everything.

## Spacing
Base unit: `--unit: 8px`. All spacing is multiples of this.

## Typography
- Sans: `'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif`
- Mono: `'JetBrains Mono', 'SF Mono', 'Fira Code', 'Cascadia Code', monospace`
- All numbers rendered in monospace with `.num` class
- Headlines: tight letter-spacing (-0.02em)

## Border Radius
Sharp: `--radius: 4px`, `--radius-lg: 6px`. Technical feel.

## Number Formatting
- German locale (dots as thousand separators)
- Compact abbreviations: 1,03M / 527K
- `tabular-nums` for columnar alignment

## Component Patterns

### KPI Cards
- Grid layout (5 columns desktop, 3 tablet, 2 mobile)
- Label in `--fossil`, value large in mono, optional sub-label
- Signal color classes: `.signal`, `.discovery`, `.growth`

### Data Table
- Sortable columns via header click
- Channel avatar + name + handle in first column
- All numeric columns right-aligned in mono
- Trend indicators: up arrow (green), down arrow (red), dash (neutral)
- Hover: row background shifts to `--shelf`

### Viral Video Cards
- Grid layout (3 columns desktop, 1 mobile)
- Thumbnail with aspect ratio 16:9
- View count with conditional glow: amber >500K, cyan >100K
- Stat row with views, likes, comments in mono

### Settings Modal
- Overlay with backdrop blur
- Password field for API key (stored in localStorage)
- Range sliders with live value display in `--signal`

### Toast Notifications
- Fixed top-right position
- Color-coded left border: red (error), green (success), amber (warning)
- Auto-dismiss after 5 seconds

## Animation
- Transitions: 150ms ease for micro-interactions
- Toast entrance: slide from right (200ms)
- Pulse animation for live indicators

## Responsive Breakpoints
- Desktop: full layout (1200px+)
- Tablet: 3-column KPI grid (768-1200px)
- Mobile: 2-column KPI, stacked viral cards (<768px)
