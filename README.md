# crackbandicoot-dot.github.io

Cristhian Delgado's CV / portfolio site, built with Jekyll for GitHub Pages.

## How it's structured

- `index.html` — the homepage (hero/bio + a project grid that's generated automatically)
- `_projects/*.md` — one Markdown file per project; each becomes its own page
  and a card on the homepage
- `_layouts/` — shared page templates (you shouldn't need to touch these)
- `assets/css/style.css` — all styling, in one place

## Adding a project

See `PROJECT_TEMPLATE.md` for the exact format. Short version: add a new
`.md` file to `_projects/`, fill in the front matter (title, description,
tech, date, repo link), write whatever you want below it — it shows up on
the site automatically on your next push. No other file needs editing.

## Running locally (optional)

```
bundle exec jekyll serve
```

(requires Ruby + Bundler + the `github-pages` gem — GitHub Pages builds the
site the same way automatically when you push, so this is only needed if
you want to preview changes before committing.)
