# Repository Guidelines

## Project Structure & Module Organization
This site uses Hugo with the PaperMod theme checked in under `themes/PaperMod` as a submodule. Global configuration lives in `hugo.yaml`. Content bundles belong in `content/…` (create if missing) and use the default front matter from `archetypes/default.md`. Layout overrides reside in `layouts/_defaults/`, where `list.html` customizes list views. Keep custom shortcodes or partials under `layouts/partials/` to avoid conflicts with the upstream theme. Assets placed in `static/` publish verbatim at the site root.

## Build, Test, and Development Commands
Use `hugo serve -D --buildDrafts` for live previews with drafts and auto-reload. Run `hugo --cleanDestinationDir --minify` before deploying to ensure a reproducible production build in `public/`. When the theme submodule changes, execute `git submodule update --init --recursive` to sync PaperMod assets. Add new archetypes with `hugo new archetype <name>.md` to standardize future content types.

## Coding Style & Naming Conventions
Write Markdown content with tidy 80–100 character lines, fences for code, and descriptive alt text. Keep front matter keys in kebab-case (`showReadingTime`, `cover.hidden`). Titles and filenames should be lowercase with hyphens, e.g. `content/posts/model-evaluation.md`. Maintain two-space indentation inside templates and Go templates in overrides. Respect Hugo parameter naming used in `hugo.yaml` to avoid drift.

## Testing Guidelines
Treat a clean `hugo` build as the baseline check; fix all warnings before committing. Verify new or updated pages locally via `hugo serve` and review the generated HTML in `public/`. For long-running sections, use `hugo --templateMetrics` to surface slow templates and adjust overrides as necessary.

## Commit & Pull Request Guidelines
Follow the existing history by using short, imperative commit subjects (`Update Hugo version`, `Add deployment workflow`). Reference related issues in the body when applicable. Pull requests should summarize net changes, list affected sections or pages, and, for visual tweaks, attach before/after screenshots from the local preview. Confirm that the deploy action (`.github/workflows`) will run cleanly by including the production `hugo` command in your verification steps.
