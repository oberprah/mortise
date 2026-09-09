# Angebot-PDF via print stylesheet

Date: 2026-09-09

## What

Give the carpenter a PDF they can hand to a customer. The PDF button
calls `window.print()` on a dedicated print-only quote layout instead
of the editing surface.

## Why

The old PDF button fired an alert. Printing the editor as-is leaked
dropdown options, editor chrome, and had no header or framing — not
a document a client could read.

## How

**Print CSS, no library.** A hidden `<section class="quote">` fills
from the active project on `beforeprint`. Editor views are hidden in
`@media print`. One static file, no build step, no PDF library.

**Layout.** Title "Kostenvoranschlag" + number + date + project name
on the left, company info on the right. A full-width Netto / MwSt /
Endpreis brutto line sits above the table — the total is the first
thing visible. One row per Möbel with the full piece total; components
and add-ons as sub-rows, each with unit price and sum on the right.
One horizontal divider between pieces.

**Company settings.** Logo, adresse, kontakt — stored in
`localStorage` under `mortise.company`. Dummy defaults until real
company data is decided. Logo stored as data URL, capped at ~600 KB.

**Settings view.** Back button dropped — click a project to return.
Project is deselected in the sidebar while settings is open. Section
titles at 16px; content capped at 680px.

**Deferred.** Real company data, number scheme, customer address
block, multi-page handling.
