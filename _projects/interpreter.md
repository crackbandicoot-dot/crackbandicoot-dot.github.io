---
title: "Language Interpreter"
description: "A tree-walking interpreter for a small custom language, built in C#."
tech: [C#, ".NET"]
date: 2025-06-01
repo: https://github.com/crackbandicoot-dot/CHANGE-ME-repo-name
---

## Overview

<!-- Replace this with the real writeup — this file just demonstrates the format. -->
A lexer, parser, and tree-walking evaluator for a small language, written from
scratch in C# with no parser-generator dependency.

## Design decisions

Before writing any code, I sketched the grammar on paper and worked through
how the parser would handle precedence and error recovery, since retrofitting
those into a hand-rolled recursive-descent parser later is expensive.

## What I learned

- Lexing/parsing theory turned into working code
- How error handling shapes a language's usability, not just its correctness
