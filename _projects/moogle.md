---
title: "Moogle!"
description: "Offline document search engine in C# with a Blazor UI, vector-space ranking, and a custom query operator language."
tech: [C#, Blazor, .NET, PdfPig]
date: 2026-01-01 # TODO: set actual date
repo: https://github.com/crackbandicoot-dot/Moogle
---

## Overview

Moogle! is an offline search engine that indexes `.txt` and `.pdf` documents from a
configurable folder and lets you search them through a Blazor web interface. It was
built as a Programming I project for the Faculty of Mathematics and Computer Science
at the University of Havana (2021–2022 courses).

Under the hood it combines a **vector-space model with cosine similarity** for
relevance ranking with a small **query operator language**:

- `^word` — the word must exist in the document.
- `!word` — the word must not exist.
- `*word` — boosts the importance of that word in the ranking.

Results come back with a title, a context snippet, and the matched pages, and the
engine caches the last query so repeated searches don't get recomputed.

## Architecture

The codebase is split into clear layers:

- `MoogleEngine/` — search logic, document reading (via `PdfPig` for PDFs), text
  normalization, vectorization, the query compiler, and ranking.
- `MoogleUI/` — the Blazor web interface.
- `MoogleController/` — a small desktop controller to start/stop the UI.
- `Shared/` — interfaces (`ISearchService`, `IConfigurationService`) and shared
  models (`SearchItem`, `SearchResult`) used across layers.

At query time, the flow is: load `appconfig.json` → read and vectorize the corpus →
compile the query string into an operator expression tree → score every page by
combining cosine similarity with operator evaluation over term frequencies → rank
and return results.

## Design decisions

- Separated the **search engine** from the **UI** behind shared interfaces
  (`ISearchService`, `IConfigurationService`), so the Blazor front end and the
  desktop controller can both drive the same engine without depending on its
  internals.
- Built a small **operator compiler** (lexer → parser → operator tree) instead of
  hardcoding `^`, `!`, `*` handling inline, so query semantics are explicit and
  testable on their own.
- Chose a classic **vector-space / cosine similarity** ranking model for
  simplicity and explainability over a more opaque scoring approach, which fit
  the scope of a coursework project.
- Cached the last query result to avoid recomputing scores when a user re-runs
  or slightly tweaks the same search.

## What I learned

- How to implement a vector-space retrieval model and cosine similarity ranking
  from scratch, rather than relying on an existing search library.
- Designing a small custom query language (lexer, parser, operator tree) and
  wiring it into a scoring pipeline.
- Structuring a C#/.NET solution into separate projects (engine, UI, controller,
  shared contracts) so responsibilities stay decoupled.
