---
title: "Gwent-Pro"
description: "Unity single-player prototype of a Gwent-like card game (Gravity Falls themed), with an embedded DSL that compiles card scripts into runtime cards."
tech: [C#, Unity, ShaderLab, DSL]
date: 2026-01-01 # TODO: set actual date
repo: https://github.com/crackbandicoot-dot/Gwent-Pro
---

## Overview

Gwent-Pro is a Unity-based single-player prototype of a Gwent-like card game,
themed around Gravity Falls. It combines a Unity front end (scenes, prefabs, UI)
with a small in-game rule engine and an embedded domain-specific language that
compiles card-definition scripts into runtime card objects.

The project brings together three layers:

- **Unity front end** (`Assets/Scripts`) — `GameManager` and related
  MonoBehaviours drive the game loop, turns, and UI (hand, drag/drop, card
  display).
- **Game rule model** (`Assets/GwentLogic`) — interfaces and structures for
  context, cards, decks, board, and hand.
- **Embedded DSL compiler** (`Assets/GwentPPCompiler`) — parses and executes
  card-definition scripts, producing `ICard` instances through an
  `ICardFactory`, with effects wired in as `DynamicEffect`.

## Architecture

`GameManager` coordinates players and turns and calls into `GwentLogic` for rule
enforcement; `GwentLogic`'s `Context` implementations expose the accessors that
card effects operate on. Card content itself isn't hardcoded in C# — it's
authored in the DSL and compiled at runtime via
`Compiler.Compile(programString, cardFactory, printFunction)`, which returns a
set of `ICard` objects that the Unity layer can populate decks and prefabs with.
UI components (`HandScript`, drag/drop, `CardDisplay`) present those cards and
forward player actions back to `GameManager`.

## Design decisions

- Kept the **card content as data** (DSL scripts) instead of C# classes per
  card, so new cards and effects can be authored without touching or
  recompiling the Unity project.
- Separated the **rule model** (`GwentLogic`) from the **Unity presentation
  layer** (`Scripts`), so game logic doesn't depend on MonoBehaviours or scene
  state directly.
- Reused the same DSL compiler design (`Compiler.Compile` +
  `ICardFactory`) as the standalone `GwentPlusPlusInterpeter` project, letting
  the interpreter be developed and reasoned about independently of the Unity
  integration.

## What I learned

- Integrating a custom-built compiler/interpreter into a game engine, including
  bridging DSL-produced data into engine-native objects via a factory pattern.
- Structuring a Unity project so gameplay logic stays testable and separate
  from MonoBehaviours and scene wiring.
- Working with third-party Unity packages (LeanTween for tweening, TextMesh Pro
  for UI text) alongside custom systems.
