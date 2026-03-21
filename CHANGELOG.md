# Changelog

All notable changes to this project will be documented in this file.

## 0.2.0 - 2026-03-21

### Added
- Added `/discover` for early-stage ideation, problem reframing, and design draft generation.
- Added `/scaffold` to replace `/init` and generate project scaffold documents.
- Added `references/` templates for `discovery.md`, `design.md`, and scaffold outputs.
- Added `AGENTS.md` as the primary cross-platform agent rules file.
- Added `CLAUDE.md` as an optional compatibility shell.

### Changed
- Refactored `SKILL.md` to a shorter, progressive-disclosure structure.
- Updated workflow order to `/discover -> /mbank -> /scaffold`.
- Updated README to reflect the new workflow and repository layout.
- Clarified that `/check` runs only when explicitly triggered.
- Clarified that `/archive` is a manual milestone archival step.
