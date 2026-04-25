# Design System — MorseClick

## Product Context
- **What this is:** A morse code telegraph key toy. Single HTML file. Tap/hold a key to send dit/dah signals through a circuit tree that resolves into letters and digits.
- **Who it's for:** Builders playing around. Side project, creative outlet.
- **Space/industry:** Web toys, interactive instruments, signal communication
- **Project type:** Single-page interactive toy (mobile-first)

## Aesthetic Direction
- **Direction:** Retro-Futuristic / Avionics — a Primary Flight Display that happens to run morse code
- **Decoration level:** Intentional — scanline texture, subtle CRT feel, phosphor glow
- **Mood:** You're operating a signal instrument. Precision, clarity, purpose. Green phosphor on black. Every pixel earns its place.
- **Reference:** Aircraft PFDs, oscilloscope screens, military comm panels

## Memorable Thing
"This is a real instrument." The green phosphor glow, the antenna icon at root, the signal tracing through orthogonal paths. It should feel like you found a piece of avionics equipment that speaks morse code.

## Typography
- **Display/Hero:** JetBrains Mono 700 — precise, technical, the de facto instrument readout font
- **Body:** Instrument Sans 500 — clean sans-serif for labels and UI elements
- **UI/Labels:** Instrument Sans 400 — same family, lighter weight for secondary labels
- **Data/Tables:** JetBrains Mono 400 (tabular-nums) — for timing readouts, status text, node labels
- **Code:** JetBrains Mono — covers all monospace needs
- **Loading:** Google Fonts CDN (`https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;700&family=Instrument+Sans:wght@400;500;600`)
- **Scale:**
  - xs: 8px — status labels, HUD text
  - sm: 10px — node labels, secondary text
  - md: 12px — body text, readouts
  - lg: 16px — morse code display
  - xl: 28px — letter display (the resolved character)
  - xxl: 48px — not used in v1

## Color
- **Approach:** Restrained — one accent (green) + neutrals, color is rare and meaningful
- **Primary:** `#00ff41` — Phosphor green. The signal color. Active paths, resolved letters, current node glow.
- **Background:** `#0a0e14` — Near-black with blue tint. Instrument panel background.
- **Dim:** `#0a3a0a` — Dim green for inactive paths and nodes
- **Labels (inactive):** `#00661a` — Dark green for letter labels on unvisited nodes
- **Labels (active):** `#00ff41` — Bright green for letter labels on visited nodes
- **Signal dot:** `#ffffff` — White with green glow halo. The traced signal.
- **HUD text:** `#00ff41` at 60% opacity — status bar text
- **Error:** `#ff3333` — Red flash for invalid sequences
- **Key (idle):** `#1a2a1a` border `#2a4a2a` — Dark green tinted button
- **Key (pressed):** `#003a00` border `#00ff41` — Glowing green when held

## Spacing
- **Base unit:** 4px
- **Density:** Compact — instrument panel density, not marketing whitespace
- **Scale:** 2xs(2) xs(4) sm(8) md(16) lg(24) xl(32) 2xl(48) 3xl(64)

## Layout
- **Approach:** Grid-disciplined — instrument panel zones. Everything has its place.
- **Grid:** Single column, full width. No responsive breakpoints needed (single page toy).
- **Max content width:** 100vw (full bleed, single page)
- **Border radius:** sm(4px) — instrument panels, buttons. Nothing round. Instruments have sharp edges.

## Motion
- **Approach:** Intentional — signal trace animation, letter reveal, node glow transitions. Nothing decorative.
- **Easing:** enter(ease-out) exit(ease-in) move(ease-in-out)
- **Duration:** micro(50ms) short(150ms) medium(250ms) signal(200ms per hop)

## Tree Structure
- **Layout:** Orthogonal (right-angle) routing. No diagonal lines. Manhattan-style like PCB traces.
- **Root node:** Antenna icon (⚡ or SVG antenna shape). Lights up on first tap.
- **Intermediate nodes:** Labeled with `·` (dit/left) and `−` (dah/right). These show which direction the signal takes.
- **Leaf nodes:** Letter labels (A-Z) and digit labels (0-9).
- **Depth:** 5 levels for full A-Z + 0-9. Some nodes at level 1-4 also resolve to letters (E, T, A, I, N, M, etc.).
- **Path style:** Right-angle lines with rounded corners at bends (4px radius). Active paths glow brighter.

## Telegraph Key
- **Visual:** Dark green-tinted button, full width at bottom of screen
- **Pressed state:** Glows green, depresses 2px (translateY)
- **Label:** "KEY" in JetBrains Mono 10px uppercase, green text
- **Touch target:** Minimum 48px height for mobile

## HUD Elements
- **Top bar:** "MORSE 1.0" | "SIG TRACK" | "CH 01" — in 9px uppercase JetBrains Mono, green at 60% opacity
- **Letter readout:** Bottom of tree area. Character in 28px bold + morse sequence in 16px + status label in 8px
- **Path annotation:** "PATH: ·−··" and "NODE: 4/5" in 8px at bottom of tree

## Scanline Effect
- Subtle repeating-linear-gradient overlay on the instrument frame
- `transparent, transparent 2px, rgba(0,255,65,0.03) 2px, rgba(0,255,65,0.03) 4px`
- Applied via CSS `::before` pseudo-element with `pointer-events: none`

## SVG Glow Filter
```xml
<filter id="glow">
  <feGaussianBlur stdDeviation="3" result="blur"/>
  <feMerge>
    <feMergeNode in="blur"/>
    <feMergeNode in="SourceGraphic"/>
  </feMerge>
</filter>
```
Apply to group elements, not individual paths, for mobile performance.

## Decisions Log
| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-04-25 | Initial design system created | Created by /design-consultation. PFD/Avionics direction chosen over Circuit Board and CRT Oscilloscope. |
| 2026-04-25 | Orthogonal tree routing | User requested right-angle lines instead of diagonal. More circuit-board feel, better mobile readability. |
| 2026-04-25 | Antenna root icon | User requested antenna symbol at root node. Reinforces the "signal instrument" metaphor. |
| 2026-04-25 | Labeled intermediate nodes (·/−) | User wanted dit/dah labels on branch nodes so you can see which direction you're taking. |
| 2026-04-25 | Include digits (0-9) | User extended scope beyond A-Z to include digits at level 5. |
