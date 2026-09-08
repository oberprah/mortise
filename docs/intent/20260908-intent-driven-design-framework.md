# Intent-driven design framework

Date: 2026-09-08

## What

A way to capture what the user wants, so future agents have the context and
the reasons behind decisions. This lets them take better decisions themselves.

The problem: when a piece of work spans multiple sessions, the reasoning is
lost between them. By the time a pull request exists, the "why" is gone. The
next agent starts from scratch.

## Why

Code shows what was done, not what the user asked for. Without the "why,"
every new agent has to reconstruct it from the code alone — slow and often
wrong.

With the intent written down, you could also recreate the app or parts of it
from these files alone — if the current implementation went the wrong
direction, a new agent could build a better one.

## How

Write it down in files that live in the repo.

- **One file per intent.** Written during the work. At least one per pull
  request. Immutable after PR.
- **`AGENTS.md` at the root.** The entry point. Names the convention and links
  the template. One source of truth, no folder-level READMEs.

We considered a separate `docs/adr/` folder for decisions and a `docs/notes/`
folder for observations. We dropped both. Decisions live in the "how" section
of the intent. Observations have no home yet — we add one when we feel that
pain.

We also considered living docs like a `product.md` or `architecture.md` that
give an overview of the current state. We don't have them yet. They may be
needed in the future, but we start without them.

Pull request descriptions don't work for this. When a piece of work spans
multiple sessions, the information is often lost between them before the PR
even exists.
