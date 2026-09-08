# Multi-project sidebar with per-project type snapshots

Date: 2026-09-08

## What

Turn the single-project app into a multi-project tool: a left sidebar lists all
projects, the user switches between them and creates new ones there. The main
content sits in one floating card on a soft background (modern look). The
global furniture-type catalog moves to a full-page Settings view reachable
from the sidebar. Each project gets its own snapshot of the catalog, editable
per project. Drop all JSON import/export; the PDF button stays as a placeholder
alert.

## Why

One project per browser tab was fine for the MVP, but a carpenter works on
several estimates in parallel and needs to jump between them. Keeping them as
files is friction; the browser already persists, so store the list there.

A snapshot per project matters for pricing: when a material price changes in
the global catalog, already-finished estimates and their printed documents
must not silently change retroactively. The project keeps the prices it was
created with. Per-project editing is still wanted for one-off client specials
(a type only this client needs, a discount for this job), so the snapshot is
editable inside the project and can be refreshed from global on demand.

Import/export JSON is dropped to simplify the UI; everything lives in the
browser. PDF export is real work we are not doing yet, so the button says so
instead of pretending.

## How

**Storage.** Three keys: `mortise.catalog` (global Möbeltypen template),
`mortise.projects` (array of project objects, each carrying its own
`moebelTypen` snapshot), `mortise.activeProjectId`. Old `mortise.project` is
migrated into a single project on first load.

**Project object.** `{ id, name, mwstPct, moebel, moebelTypen }`. The snapshot
lives on the project, not shared. Calc and rendering read the active
project's snapshot; the global catalog only seeds new projects and the
"reload from global" action.

**Layout.** A flex `.app` with a fixed 248px sidebar (project list + New at
top, Settings at the bottom) and a `.main` area holding one floating
`.card`. The card swaps between project view and settings view via a view
state. Body gets a soft gradient so the white card floats.

**Type editor.** Generalized to take a context (`typen`, optional `moebel`
for name migrations, `onSave`, `onChange`, `onReset`, `resetLabel`). The same
editor renders the global catalog in Settings and the project snapshot in the
project's collapsible panel. Global reset restores defaults; project reset
reloads from global (overwriting project-specific edits, with confirmation).

**Settings view.** Full-page swap inside the card with a back button, not a
modal — the editor needs the width and the existing modal pattern is for
small forms.

**Dropped.** JSON export/import buttons, the hidden file input, the import
code paths, the "Bestehendes importieren" button in the new-project modal.
PDF button calls an alert instead of `window.print()`.

**Responsive.** Below 860px the sidebar becomes an off-canvas drawer toggled
by a menu button in the card header.

## Decisions considered

- *Snapshot vs. live link to catalog.* Snapshot. Retroactive price changes on
    finished estimates are the failure mode the user wants to avoid.
- *Project-local type editing vs. reload-only.* Both. Reuse the same editor
    UI for the project snapshot; reload-from-global overwrites it. The user
    wants one-off client specials, so editing must be possible.
- *Settings as modal vs. full-page view.* Full-page. The editor is wide and
    dense; a modal would cramp it.
- *localStorage vs. IndexedDB for the project list.* localStorage. Same
    reasoning as the MVP: small JSON, sync, offline, no library.
