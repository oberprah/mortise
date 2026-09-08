# Agents

## Project

Mortise is a lean, static, offline tool that helps carpenters build rough
cost estimates for customers. A carpenter adds furniture pieces, picks
materials and add-ons, sees a live total, and hands the result to the
customer as a PDF.

It is a proof of concept for a new way of working, not yet in use.

The app is a single `index.html` with no build step and no backend. It runs
locally or via GitHub Pages. Projects are saved as JSON files.

## Intent-driven design

Code shows what was done, not what the user asked for or why. We write that
down so the next agent knows what goal a piece of work served and what was
decided to get there.

One file per intent in `docs/intent/`, named `YYYYMMDD-<slug>.md`. At least one
per pull request. Written during the work, not at the end. When a decision is
made or changed mid-work, update the intent doc in the same commit as the
code — not in a later pass. The doc and the diff ship together.

Immutable after PR.

See [the template](docs/intent/_TEMPLATE.md).

## Writing style

Lean, as short as possible (focus on the key parts), simple and plain English.
One idea per sentence.

Verdict first. The most important thing goes at the top, every time.

When several paragraphs belong to one group, put them under bullets or a
heading. Structure is there to help the eye scan, not to decorate. In a bullet
list of parallel items, open each line with a bold label of one to three
words, so the eye can pick the line it needs and skip the rest. Never add a
list the answer did not already need.
