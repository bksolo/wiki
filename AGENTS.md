# Repository Guidelines

## Project Structure & Module Organization

This repository combines a Quartz site with a curated markdown wiki.

- `wiki/`: primary knowledge base; keep pages flat, in German, and linked with `[[Wikilinks]]`.
- `raw/`: immutable source material for the wiki; read from here, but do not rewrite source files.
- `quartz/`: TypeScript, Preact, and SCSS code for the site generator, components, plugins, and tests.
- `public/`: generated site output from Quartz builds.
- `doc/`: local project notes and setup assets.
- Root config: `package.json`, `quartz.config.ts`, `quartz.layout.ts`, `tsconfig.json`, `CLAUDE.md`.

## Build, Test, and Development Commands

- `npm ci`: install dependencies exactly as locked in `package-lock.json`.
- `npx quartz build --directory wiki`: build the static site into `public/`.
- `npx quartz build --directory wiki --serve`: build and run a local preview server.
- `npm run check`: run TypeScript checks and Prettier verification.
- `npm test`: run the `tsx --test` test suite.
- `npm run format`: apply Prettier formatting across the repo.

Use Node 22 and npm 10.9.2+ to match `package.json` and the GitHub Pages workflow.

## Coding Style & Naming Conventions

TypeScript files follow the existing Prettier style: 2-space indentation, double quotes, and trailing commas where valid. Keep tests close to the code they cover, for example `quartz/util/path.test.ts`.

Markdown conventions matter here:

- Wiki filenames use `kebab-case` like `llm-wiki-pattern.md`.
- Source summaries use `source-<name>.md`.
- Frontmatter keys stay in English; wiki content stays in German.

## Testing Guidelines

Tests use Node’s built-in test runner via `tsx --test`. Name new tests `*.test.ts` and place them beside the module under test. Run `npm test` before opening a PR, and run `npm run check` for type and formatting validation.

## Commit & Pull Request Guidelines

Recent history uses short, lowercase prefixes such as `feat:` and `update:`. For wiki maintenance, `CLAUDE.md` also defines `ingest:`, `query:`, `lint:`, and `update:`. Keep subject lines imperative and specific.

Pull requests should describe the scope, list affected areas (`wiki/`, `quartz/`, config), link the related issue or task when applicable, and include screenshots for visible site changes. Do not commit generated output, local Obsidian state, or `node_modules/`.
