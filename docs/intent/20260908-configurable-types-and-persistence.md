# Configurable furniture types and browser persistence

Date: 2026-09-08

## What

Make the previously hardcoded Preisliste and Möbel shape configurable: each
furniture type (Schrank, Decke, Wandverkleidung) carries its own components and
extras inline, edited in the UI, persisted in localStorage. The current project
is also persisted so closing the tab no longer loses work.

## Why

The MVP assumed every Möbel is a cabinet: m², one Korpus, one Front, a shared
add-on pool. That does not fit other pieces a carpenter builds. A Decke
(Akustikdecke) has no Front and its own extras (Ausschnitte, Aufpolsterung,
Lüftung, Inspektionsdeckel). A Wandverkleidung has its own (Türverkleidungen,
Fensterverkleidungen, Rundungen). Forcing all of them through the cabinet shape
is wrong.

The Preisliste and the Möbel shape were also hardcoded in `index.html`, so
adding a type or an extra meant editing source. The carpenter should be able to
do that from the UI, and the config should survive a tab close.

## How

**Schema.** Replace the flat `preisliste` object with a list of `moebelTypen`.
Each type is standalone and complete: its own `komponenten` (each with
`optionen`) and its own `zusaetze`. No cross-type sharing, no separate catalog.
Duplication between types (Korpus, Montage, Lieferung) is accepted in exchange
for simplicity; if it hurts at 15 types, a catalog refactor can merge it later
without touching instance data.

**Quantity kind.** Each add-on carries a `mengeArt` of `flaeche`, `laenge`,
`stueck`, or `pauschal`. The card renders a menge input only when the kind is
not `pauschal`. The display unit is derived from the kind, so they cannot drift.

**Required components.** Dropped. A component is optional iff its `optionen`
include a zero-price "Keine" entry. No separate flag, no duplication.

**m².** Stays hardcoded and always present on every Möbel. A `flaeche` flag on
the type was considered and dropped — every current type uses m².

**Persistence.** Two localStorage keys, kept separate:
`mortise.catalog` (the `moebelTypen` config) and `mortise.project` (the current
project: name, MwSt, Möbel list). Each has its own reset path. Export/import
JSON carries `moebelTypen` so configs move between machines.

**Build order.** Persistence first (catalog + project, load/save/reset), then
the type editor (add/rename/remove types, edit components and extras inline),
then the dynamic Möbel card rendering from the picked type, then fold
`moebelTypen` into export/import.

## Decisions considered

- *Shared catalog with type references vs. standalone types.* Standalone.
    The three current types have little real overlap and standalone is far
    simpler. Refactor cost is low if shared catalogs become painful.
- *`required` flag on components vs. "Keine" option.* "Keine" option. Avoids
    duplication between the flag and a zero-price entry that means the same.
- *m² as a type flag vs. always present.* Always present. Only one current
    type shape and all use m². Revisit when a piece genuinely has no area.
- *localStorage vs. IndexedDB vs. File System Access API.* localStorage. Two
    small JSON blobs, sync, works offline, no library. IndexedDB is overkill;
    File System Access lacks Safari support for a single-file app.
