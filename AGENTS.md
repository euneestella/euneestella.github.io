# Repository Guidelines

## Project Structure & Module Organization
- Hugo configuration lives in `hugo.yaml`; keep site-wide params aligned with theme expectations.
- Author new content under `content/` using bundles; start from `archetypes/default.md` for front matter.
- Layout overrides belong in `layouts/_defaults/`; custom partials or shortcodes sit in `layouts/partials/` to avoid PaperMod drift.
- Assets you want published verbatim land in `static/`; the production output from `hugo` is written to `public/` and should stay untracked.

## Build, Test, and Development Commands
- `hugo serve -D --buildDrafts` spins up a live preview with drafts, hot reload, and console warnings.
- `hugo --cleanDestinationDir --minify` produces the deployable site in `public/`, clearing stale files and trimming assets.
- `git submodule update --init --recursive` refreshes the PaperMod submodule when upstream changes are pulled.
- `hugo new archetype <name>.md` seeds additional archetypes whenever you introduce a new content type.

## Coding Style & Naming Conventions
- Keep Markdown lines between 80–100 characters, prefer fenced code blocks, and add descriptive alt text for images.
- Use lowercase, hyphenated filenames (`content/posts/model-evaluation.md`) and kebab-case front matter keys (`showReadingTime`, `cover.hidden`).
- Maintain two-space indentation in templates and Go template blocks; mirror parameter names already declared in `hugo.yaml`.

## Testing Guidelines
- Treat a clean `hugo --cleanDestinationDir --minify` run as the release gate; resolve every warning before committing.
- Validate page updates locally with `hugo serve` and spot-check generated HTML in `public/` for layout or asset regressions.
- Profile slow renders with `hugo --templateMetrics` when list or taxonomy pages lag.

## Commit & Pull Request Guidelines
- Write short, imperative commit subjects (e.g., `Add author profile shortcode`) and reference issues in the body where helpful.
- Pull requests should summarize net changes, call out affected sections, and attach before/after screenshots for visual tweaks.
- Confirm the deploy workflow succeeds by listing the production `hugo` command under your PR verification steps.

## Security & Configuration Tips
- Store secrets outside the repo; environment variables feed `hugo` via the shell when needed.
- Pin Hugo version upgrades in the PR description and verify the PaperMod submodule remains compatible before merging.
