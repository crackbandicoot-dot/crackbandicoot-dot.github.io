# Adding a new project

Copy this into a new file in `_projects/`, e.g. `_projects/my-interpreter.md`.
The filename doesn't matter for display — only the front matter does.

You do NOT need to add `layout: project` — `_config.yml` sets that automatically
for everything in `_projects/`. Just fill in the fields below and write the
body in normal Markdown; it appears on the project's own page under the tags
and links.

```
---
title: "Project name"
description: "One sentence — this is what shows on the homepage card."
tech: [C#, PostgreSQL, Docker]
date: 2026-01-15
repo: https://github.com/crackbandicoot-dot/repo-name
demo: https://example.com          # optional, omit if there isn't one
---

## Overview

What it does and why you built it.

## Design decisions

The tradeoffs you weighed before writing code — this is the section that
matters most for you specifically.

## What I learned
```

That's it — save the file, commit, push. It shows up on the homepage grid
automatically, newest `date` first, no other file needs to change.
