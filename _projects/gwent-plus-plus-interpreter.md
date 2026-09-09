---
title: "G++ (GwentPPCompiler)"
description: "A C# DSL compiler for authoring Gwent-like card game content — a JSON-like language with delegate-based effects, compiled into runtime card objects."
tech: [C#, DSL, Compilers]
date: 2026-01-01 # TODO: set actual date
repo: https://github.com/crackbandicoot-dot/GwentPlusPlusInterpeter
---

## Overview

GwentPPCompiler implements **G++**, a domain-specific language for defining
cards and effects for Gwent-style card games. It reads a G++ program, tokenizes
and parses it into an AST, executes it, collects the card definitions it
produces, and converts them into a host application's own card objects via a
user-provided `ICardFactory`. The repository is the language and compiler
layer only — not a game itself — meant to be embedded in a larger project (as
it is, for example, in the companion `Gwent-Pro` Unity prototype).

The language itself is a **JSON-like structure with delegates**:

- Block declarations such as `card { ... }`, `effect { ... }`, `Params { ... }`,
  and `Selector { ... }`.
- Delegate-style behavior for effect bodies, e.g.
  `Action: (targets, context) => { ... }` and `Predicate: (unit) => ...`.
- A composable activation pipeline where `OnActivation` chains an `Effect`, a
  `Selector`, and a `PostAction`.
- General-purpose constructs: variables, arithmetic/boolean expressions, `if`,
  `while`, `for`, ternary expressions, lists, and `print(...)`.

## Architecture

The compiler is a straightforward pipeline: `LexerStream` tokenizes the source,
`ProgramParser` builds an AST, executing the AST populates an evaluation
context, and the card definitions collected there are handed to the host's
`ICardFactory` to produce runtime `ICard` instances. The public surface is a
single facade:

```csharp
DSL.Compiler.Compile(string programString, ICardFactory cardFactory, Action<string> printFunction)
```

Internally this is organized into a **Lexer** (`Token`, `TokenType`), a
**Parser** (`ProgramParser`, `ExpressionParser`, statement parsing), and an
**AST/Evaluator** layer that executes the parsed program and exposes results
through the `Compiler` facade.

## Design decisions

- Delegated **card object creation** to a host-supplied `ICardFactory` instead
  of hardcoding a card model in the compiler, so any game can plug in its own
  runtime card representation.
- Chose a **JSON-like block syntax with embedded delegate expressions** rather
  than a fully custom imperative syntax, aiming for something readable to
  someone used to card-game data files while still allowing arbitrary effect
  logic.
- Injected the `print(...)` output via a callback (`printFunction`) rather than
  writing to the console directly, keeping the DSL runtime decoupled from any
  particular I/O environment (useful since it's meant to run inside a game
  engine like Unity).
- Structured `OnActivation` as a chain of `Effect` → `Selector` → `PostAction`,
  separating "what happens", "who it targets", and "what happens after" as
  distinct, reusable pieces.

## What I learned

- Writing a hand-built lexer, parser, and evaluator for a custom language,
  including handling delegate/lambda-like syntax inside a data-oriented
  format.
- Designing a compiler's public API around a factory-injection pattern so the
  same language core can serve multiple, unrelated host applications (a plain
  test harness and a Unity game, in this case).
- Balancing a data-description language (JSON-like) with the need for real
  conditional/looping logic inside effects.
