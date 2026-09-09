# Browser tab branding

Date: 2026-09-09

## What
Give the browser tab the product name and an icon, so the tab reads
"Mortise" instead of the generic "Möbelkalkulation".

## Why
The product is called Mortise everywhere else (repo, storage keys,
docs). The tab title and missing favicon were leftovers from the MVP
that did not match the rest.

## How
Rename `<title>` to "Mortise" and add an inline SVG favicon as a data
URI: a slate "M" mark on the app's `--accent` background. Inline data
URI keeps the single-file, offline, no-build setup intact — no extra
request, no asset to ship.
