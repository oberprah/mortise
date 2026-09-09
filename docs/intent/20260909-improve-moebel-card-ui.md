# Improve möbel card UI and overall polish

Date: 2026-09-09

## What
Make the app feel less like a form and more like a clean, scannable
estimate. The möbel cards were hard to tell apart, inputs looked heavy,
and the zusatz area felt detached from the rest of the card.

## Why
Carpenters scan a project quickly to check totals and adjust prices.
The old UI fought that — boxed inputs everywhere, shadowed cards that
blurred together, and a zusatz panel that read as a separate widget
inside each möbel. The goal is a flat, readable layout where the title
and total stand out and the detail rows read as one block.

## How
- Cap card content at 1100px so wide screens don't stretch the reading
  line. The white card background still spans full width.
- Inputs are borderless and transparent by default; chrome, background,
  and the select arrow only appear on hover/focus. Keeps the resting
  state calm. Uses `background-color` (not `background`) on hover to
  avoid resetting `background-repeat`, which tiled the arrow SVG.
- Project title (26px/800) and möbel title (20px/800) bumped up. Moved
  their CSS below the base input rule with `input.`-prefixed selectors
  so they win the specificity tie they were silently losing before.
- Möbeltypen config panel moved above the möbel list so it's reachable
  without scrolling past the furniture.
- All card shadows removed. Cards, rows, and panels use borders only.
- Möbel rows use stronger borders and more gap for clearer separation.
- Zusatz area flattened: dropped the tinted box and border, replaced
  with a simple top divider so it reads as part of the card.
- Möbel detail restructured: Typ + m² on the first line, each component
  (Korpus, Front) on its own row with select and price together.
  Labels share a fixed 70px column so selects align.
- Collapse toggle removed from möbel rows — always fully expanded.
- Delete buttons use a trash SVG icon instead of ×. Möbel delete moved
  to the bottom-right with a labeled text button ("\<name\> löschen").
  Project delete moved to the card footer with the project name.
- PDF button moved to the left of the summary bar, inline with prices.
- Zusatz header only shows when at least one zusatz row exists.
