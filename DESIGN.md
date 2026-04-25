# Design System — MorseClick

## Product Context
- **What this is:** A morse code telegraph key toy. Single HTML file. Tap/hold a key to send dit/dah signals through a circuit schematic that resolves into letters and digits.
- **Who it's for:** Builders playing around. Side project, creative outlet.
- **Space/industry:** Web toys, interactive instruments, signal communication
- **Project type:** Single-page interactive toy (mobile-first)

## Aesthetic Direction
- **Direction:** Circuit Schematic — an electronics diagram that happens to run morse code
- **Decoration level:** Intentional — grid background, functional labels, wire traces. No decoration without purpose.
- **Mood:** You're reading a signal circuit. Precision, clarity, purpose. Green phosphor on black. Every pixel earns its place.
- **Reference:** Electronics schematics, oscilloscope screens, telegraph circuit diagrams

## Memorable Thing
"This is a real circuit." The green wire traces, the dashed decoder box, the antenna picking up signals, the headphone output. It should feel like you found a piece of signal processing equipment that speaks morse code.

## Typography
- **Display/Hero:** JetBrains Mono 700 — precise, technical, instrument readout font
- **Body:** JetBrains Mono 400 — covers all monospace needs
- **Loading:** Google Fonts CDN (`https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;700`)
- **Scale:**
  - xs: 8px — status labels, HUD text
  - sm: 10px — node labels, secondary text
  - md: 12px — body text
  - lg: 14px — morse code display
  - xl: 28px — letter display (the resolved character)

## Color
- **Approach:** Restrained — one accent (green) + neutrals, color is rare and meaningful
- **Primary:** `#00ff41` — Phosphor green. Active wires, resolved letters, active nodes.
- **Background:** `#0a0e14` — Near-black with blue tint. Circuit board background.
- **Dim:** `#0a3a0a` — Dim green for inactive wires, edges, and components
- **Labels (inactive):** `#00661a` — Dark green for letter labels on unvisited nodes
- **Labels (active):** `#00ff41` — Bright green for letter labels on visited nodes
- **Signal dot:** `#ffffff` — White with green glow halo. The traced signal.
- **Schematic labels:** `#00ff41` at 50% opacity — functional labels (ANT, KEY, DECODER, OUT, GND)
- **Ground lines:** `#00ff41` at 30% opacity — ground bus and returns
- **Error:** `#ff3333` — Red flash for invalid sequences
- **Key (idle):** `#1a2a1a` border `#2a4a2a` — Dark green tinted button
- **Key (pressed):** `#003a00` border `#00ff41` — Glowing green when held

## Spacing
- **Base unit:** 4px
- **Density:** Compact — circuit schematic density, not marketing whitespace
- **Scale:** 2xs(2) xs(4) sm(8) md(16) lg(24) xl(32) 2xl(48) 3xl(64)

## Layout
- **Approach:** SVG schematic fills viewport, HTML panels for readout and key
- **Max content width:** 430px (mobile-first)
- **Border radius:** sm(4px) — decoder box, key button. Sharp edges, like real components.

## SVG Structure
- **ViewBox:** 480x520
- **Z-order (back to front):**
  1. `g-sch-bg` — component labels
  2. `g-ground` — ground bus, return wires (low opacity, behind everything)
  3. `g-output` — output wire to headphone
  4. `g-antenna` — antenna V-shape, mast, signal waves, key symbol, wire to decoder
  5. `g-decoder` — dashed rectangle box with pin dots
  6. `g-edges` — orthogonal tree paths
  7. `g-dirs` — dit/dah direction labels (·/−)
  8. `g-nodes` — tree node shapes (circles, rectangles, root triangle)
  9. `g-labels` — letter/digit labels
  10. `signal` — animated white dot (always on top)

## Schematic Layout
- **Antenna:** Left side, V-shape arms at y:20-30, mast down to y:75, faint signal wave arcs
- **Key symbol:** At (36, 85), two dots with angled lever line (telegraph key switch)
- **Wire to decoder:** From key tip, routes right then down into decoder box top
- **Decoder box:** Dashed rectangle from (55, 100) to (460, 430), labeled "DECODER U1"
- **Tree:** Inside decoder box, 5 levels deep, orthogonal routing
  - Levels 0-4: even spacing
  - Level 5: clusters children under parent x positions, with overlap resolution
  - Root node: triangle shape, not circle
  - Dit nodes: circles, Dah nodes: small rectangles
- **Output wire:** From decoder right pin, routes to headphone at bottom-right
- **Headphone:** Arc headband + two earpiece rectangles at bottom-right
- **Ground bus:** Horizontal line at y:455 with 3-line ground symbol at center
- **Ground returns:** Thin low-opacity wires from key and output down to ground

## Tree Node Classes
- `.edge` — inactive tree path
- `.edge-traced` — previously traced path (dim green)
- `.edge-active` — currently active path (bright green + glow)
- `.nd` — inactive node shape
- `.nd-active` — currently active node (bright green + glow)
- `.nd-visited` — previously visited node (dim green)
- `.lbl` — inactive letter label
- `.lbl-on` — active/visited letter label (bright green)
- `.dir` — inactive direction label (·/−)
- `.dir-on` — active direction label

## Schematic Wire Classes
- `.wire` — inactive schematic trace
- `.wire-active` — active wire (bright green + glow)
- `.wire-traced` — traced wire (dim green)
- `.component` — inactive component (headphone earpieces)
- `.component-active` — active component (bright green + glow)
- `.decoder-box` — dashed rectangle
- `.gnd-line` — ground bus line

## Telegraph Key (HTML)
- **Visual:** Dark green-tinted button, full width at bottom of screen
- **Pressed state:** Glows green, depresses 2px (translateY)
- **Label:** "KEY" in JetBrains Mono 10px uppercase, green text
- **Touch target:** 72px height, 8px margin. Exceeds 48px minimum.
- **Auto-dah:** Triggers dah automatically after 2 seconds of held key

## Readout Panel (HTML)
- **Position:** Below schematic, above key button
- **Letter display:** 28px bold, green. Pop animation on resolve.
- **Morse sequence:** 14px, green at 80% opacity
- **Status:** 8px, green at 60% opacity. Shows: READY, SIGNAL, RESOLVED, LOCKED, NO LETTER, OVERFLOW
- **Path bar:** 8px, green at 60% opacity. Shows "PATH: ·−··" and "NODE: 3/5"

## Motion
- **Approach:** Intentional — signal trace animation, letter reveal, node glow transitions. Nothing decorative.
- **Easing:** enter(ease-out) exit(ease-in)
- **Duration:** micro(50ms key press) short(150ms) medium(250ms letter pop) signal(200ms per hop)
- **Signal dot:** White circle, 3.5px radius, animated along orthogonal waypoints

## Background
- Subtle grid via CSS `::before` pseudo-element
- `linear-gradient(rgba(0,255,65,0.04) 1px, transparent 1px)` + 90deg variant
- 20px grid spacing, `pointer-events: none`

## SVG Glow Filter
```xml
<filter id="glow">
  <feGaussianBlur stdDeviation="2.5" result="b"/>
  <feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge>
</filter>
<filter id="sig-glow">
  <feGaussianBlur stdDeviation="4" result="b"/>
  <feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge>
</filter>
```
Apply to group elements, not individual paths, for mobile performance.

## State Machine
1. **IDLE** — Waiting for input. All visuals inactive.
2. **TIMING** — Key is held. Antenna/key light up. Tone plays. Auto-dah after 2s.
3. **ANIM** — Signal dot hopping from parent to child node.
4. **WAIT** — Between signals. Path traced. Timeout to complete.
5. **Complete** — Letter locked or no letter. Brief display then reset to IDLE.

## Schematic Labels
Only functional labels are shown. No fake electronic values.
- **ANT** — next to antenna arms
- **KEY** — next to key switch symbol
- **DECODER U1** — above decoder box
- **OUT** — next to headphone output
- **GND** — below ground bus
- **MORSE 1.0** — top-right corner

## Decisions Log
| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-04-25 | Initial design system created | Created by /design-consultation. PFD/Avionics direction chosen. |
| 2026-04-25 | Orthogonal tree routing | User requested right-angle lines instead of diagonal. |
| 2026-04-25 | Antenna root icon | User requested antenna symbol at root node. |
| 2026-04-25 | Include digits (0-9) | User extended scope beyond A-Z to include digits at level 5. |
| 2026-04-25 | Circuit schematic redesign | Complete visual overhaul from PFD to circuit schematic. Decoder box, key symbol, headphone, ground bus. |
| 2026-04-25 | Remove fake schematic values | 7MHz, 600Ω, CW MODE were decorative, undermined "real instrument" feel. Keep only functional labels. |
| 2026-04-25 | Auto-dah at 2s | Prevents infinite 700Hz tone if key is held too long. Stays within dit/dah timing model. |
| 2026-04-25 | No first-time guidance | User preference: purest aesthetic, the circuit diagram IS the instruction. |
