# MorseClick

A morse code telegraph key toy. Single HTML file. Tap/hold to send signals through an orthogonal circuit tree.

## Design System

Always read DESIGN.md before making any visual or UI decisions.
All font choices, colors, spacing, and aesthetic direction are defined there.
Do not deviate without explicit user approval.
In QA mode, flag any code that doesn't match DESIGN.md.

## Key Decisions

- Single HTML file, no build step, no framework
- PFD/Avionics aesthetic: green phosphor (#00ff41) on near-black (#0a0e14)
- Orthogonal (right-angle) tree routing, no diagonal lines
- Root node = antenna icon
- Intermediate nodes labeled · (dit) and − (dah)
- A-Z + 0-9 in the morse tree (5 levels deep)
- One telegraph key: tap < 200ms = dit, hold > 200ms = dah
- Letter resolves after 500ms of silence
