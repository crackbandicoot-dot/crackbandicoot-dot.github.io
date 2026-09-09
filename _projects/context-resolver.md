---
title: "ContextResolver"
description: "A .NET CLI tool that resolves the types used in a .cs file via Roslyn ASTs — useful for feeding accurate type context to LLMs."
tech: [C#, ".NET", Roslyn, CLI]
date: 2026-01-01 # TODO: set actual date
repo: https://github.com/crackbandicoot-dot/ContextResolver
---

## Overview

ContextResolver is a dotnet tool (still in beta) that analyzes a `.cs` file and
resolves the types used within it, using Abstract Syntax Trees via the Roslyn
API. The stated use case is feeding precise, resolved type information to LLMs
— particularly handy in chat-based coding assistants or agents that need
accurate context about a file's dependencies without having to load or explain
an entire codebase.

## Design decisions

- Built on **Roslyn's AST APIs** rather than regex or naive text parsing, so
  type resolution reflects the actual C# semantics (namespaces, using
  directives, generics) instead of guessing from syntax alone.
- Packaged as a **dotnet tool** (CLI) so it can be dropped into any workflow —
  including being invoked by an LLM-based agent as a subprocess — without
  requiring a library reference in the target project.

## What I learned

- Working with the Roslyn compiler platform to walk and query C# syntax trees
  for semantic (not just syntactic) information.
- Designing a small tool specifically to serve as context-gathering
  infrastructure for LLM-assisted coding workflows.


