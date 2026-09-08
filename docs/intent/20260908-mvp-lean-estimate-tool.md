# MVP: lean offline furniture estimate tool

Date: 2026-09-08

## What

A single static `index.html` at the repo root that lets a carpenter build a
rough cost estimate for a customer: add furniture pieces, pick materials and
add-ons, see a live total, and hand the result to the customer as a PDF.

This is a proof of concept for a new way of working, not yet in use. The
existing `moebelkalkulation/` files were an earlier draft; they are removed in
favor of this lean version.

## Why

The company's current process for rough estimates is painful. Excel didn't
look good enough. A static HTML file gave better results, so they kept it.

That earlier file works but carries ~1 MB of inlined SheetJS plus jsPDF just
to support Excel export and a programmatic PDF. They don't need Excel — they
need a PDF. The Excel round-trip was also the only way to save work, because
nothing is persisted in the browser.

The goal now is a minimal, good-looking base to propose and build on: same
estimate workflow, far less baggage, a sensible save mechanism, and a clear
path to the deferred features.

## How

**Scope.** Keep the working UI and calculation from the existing app: the
Möbel list (Bezeichnung, m², Korpus, Front, Zusätze), the editable Preisliste,
the 22 % MwSt summary. Drop only the two inlined libraries and the features
that depend on them.

**Export/import.** Replace Excel with JSON. One `.json` file carries the
project name, MwSt rate, all Möbel with their Zusätze, and the full Preisliste.
Import restores all of it. This is simpler than Excel, drops 1 MB of
library, and the data model maps cleanly.

**PDF.** Replace jsPDF with a "Drucken / PDF" button that calls
`window.print()`. A small print stylesheet hides the interactive chrome
(buttons, Preisliste, add-buttons) and keeps the estimate readable. The
real customer-facing print layout is deferred — this is a placeholder so the
button exists and works.

**Deferred, by explicit decision:** browser persistence (localStorage),
multi-company config (logo, name, catalog as JSON files), and a designed
print/PDF layout. These were discussed and parked to keep the MVP small.

**Accepted consequence.** Closing the tab still loses all work until
localStorage lands. JSON export is the only save path for now — same shape
of risk as the old Excel export, just a different format.
