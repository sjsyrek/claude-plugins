# Changelog

All notable changes to this marketplace are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Each plugin keeps its own changelog in its own repo.

## [Unreleased]

### Added

- **Card standard v1** (`docs/card-standard.md`): one look for every plugin's plan cards, status lines and previews. Cards are unfenced markdown that Claude Code renders, with a bold title, bulleted bold-label fields, tables for parallel records only, and a final `Reply:` line of backticked verbs separated by pipes. The standard includes a Grader section the plugins copy into their evals, and an invented example card per plugin. Decided by a design council on 2026-11-21.
- **"Adding a new plugin" step** linking the card standard.
- **This changelog.**
- **accessibility-board** in the catalog and the README: WCAG 2.2 A/AA review with a guarded single-page scan and five read-only auditor seats (v0.1.0). Its plan card joins the card standard's canonical examples.

### Changed

- **README:** the repo also holds the shared card standard, not only the catalog.
- **Card standard, rule 4:** a plugin may add its own labels, as long as it doesn't reuse a standard label with a different meaning. This is a clarification; the stamp stays `standard: v1`.
