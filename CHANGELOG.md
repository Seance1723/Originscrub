# Changelog — OriginScrub

All notable changes to this project are documented in this file.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versioning follows [Semantic Versioning](https://semver.org/) (Plan §21). This file is also the changelog gated by `tools/release.mjs` — a release cannot ship without an entry for its version.

## Rules (do not violate)

1. **Append-only:** never delete, rewrite, or remove an existing entry. Corrections are made by adding a new entry that references the old one.
2. Every completed task from `TASKS.md` gets an entry here on the day it is completed, referencing its task ID (e.g. `[1.3.4]`).
3. Bug fixes reference their `BUGS.md` ID (e.g. `[BUG-003]`).
4. New entries go at the **top** of the `[Unreleased]` section. On release, `[Unreleased]` content moves under the new version heading and a fresh empty `[Unreleased]` section is created above it.
5. Entry format: `- YYYY-MM-DD — [task/bug ID] Description of what changed.`

---

## [Unreleased]

### Added
- 2026-07-20 — [meta] Project tracking scaffolding created from `OriginScrub_Implementation_Plan_v3.md`: `TASKS.md` (123 tasks across Phases 0–6), `CHANGELOG.md` (this file), and `BUGS.md`.

### Changed
- _(nothing yet)_

### Fixed
- _(nothing yet)_

### Removed
- _(nothing yet)_
