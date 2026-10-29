# Changelog

## 2.0.0

### Added
- **guided phase plan** (`references/phase-plan.md`): seven phases, each with questions, actions, and a checkpoint. Bring-up runs all; tune runs the dirty ones.
- **desktop-surfaces reference** (`references/desktop-surfaces.md`): the real Desktop routes for each phase, plus the CLI fallback table for what the app does not cover.
- **plugin baseline** (`references/plugin-baseline.md`): matched plugin set by category (agent runtime, office files, document reading) with a four-question fit test.
- **auxiliary config shape** (`references/auxiliary-config-shape.md`): canonical slot names, per-slot fallback chains, custom endpoints, and the umbrella-key failure mode.
- **audit output shape** (`references/audit-output.md`): the Phase 0 baseline template and the ranking rule.
- **anti-bloat test** ("silly vs smart") in the skill body: rejects heavy local installs that duplicate a live hosted route, with the vision-versus-OCR case as the worked example.
- **document tooling phase**: PDF and Markdown text extraction as the base layer, preferring what is already installed before adding anything.
- **honest starting-point framing**: a stock install is thin by default, stated plainly instead of implied.

### Changed
- title and description updated for the guided, Desktop-first model
- README restructured around the guided loop, the phase table, Desktop surfaces, and the anti-bloat test
- lane table extended with the full canonical task slot list (`goal_judge`, `moa_aggregator`, `review`, `background_review`, `memory_query_rewrite`, `tts_audio_tags`, `monitor`, `moa_reference`)
- `references/platform-skill-trim.md` renamed to `references/skill-trim.md` and rewritten around the enabled-versus-installed accounting rule
- the same user's live routing removed from public examples; generic profiles only
- `llms.txt` and `llms-full.txt` rewritten for the phase model

### Verified
- Desktop routes confirmed against the app source: `/settings?tab=config:model`, `/capabilities?tab=plugins`, `/capabilities?tab=skills`, `/capabilities?tab=connectors`, and the per-plugin settings pages
- CLI subcommands confirmed present: `plugins search|install|list|doctor|capabilities|check-updates`, `skills search|install|list`, `fallback list`, `doctor`, `security audit`, `tools list`, `memory status`

## 1.4.1

### Changed
- removed remaining em-dashes from `SKILL.md`

## 1.4.0

### Changed
- removed personal references, generic community footer

## 1.3.0

### Fixed
- clarified auxiliary config shape and the enabled-versus-installed cleanup semantics

## 1.2.0

### Added
- zero-manual optimization: autodetect subscribed models instead of asking the user to list them

## 0.1.0 - 2026-07-24

### Added
- initial GitHub-ready repository structure
- `PUBLISHING.md` with the GitHub and Hermes Ecosystem publication flow
- `LICENSE` (MIT)
- provenance references: `imported-skills.md`, `synthesis-map.md`, `provider-autodiscovery.md`
- repo-local `KANBAN.md` board for publication work

### Changed
- refactored the skill from a user-specific routing snapshot into a provider-agnostic optimizer
- added provider autodiscovery as a core rule