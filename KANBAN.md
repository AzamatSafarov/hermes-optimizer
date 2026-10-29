# Hermes Optimizer — Kanban

Publication board for the repository. Version 2.0.0 replaced the audit-only optimizer with a
guided, Desktop-first phase plan.

| ID | Status | Card | Output |
|---|---|---|---|
| t_d953ee0a | completed | Polish public repo metadata and board artifact | `KANBAN.md`, `CHANGELOG.md` |
| t_c46d4ae6 | completed | Finalize GitHub publish docs and ecosystem checklist | polished `PUBLISHING.md` |
| t_e8482788 | completed | Verify repo artifacts, commit history, and release readiness | verification evidence recorded |
| t_a788833a | completed | Prepare final GitHub handoff and publish commands | `RELEASE_COMMANDS.md` |
| t_v2_phase_plan | completed | Add guided phase plan (questions, actions, checkpoints) | `references/phase-plan.md` |
| t_v2_desktop | completed | Map real Desktop surfaces per phase | `references/desktop-surfaces.md` |
| t_v2_plugins | completed | Add plugin baseline and fit test | `references/plugin-baseline.md` |
| t_v2_aux_shape | completed | Document canonical auxiliary config shape | `references/auxiliary-config-shape.md` |
| t_v2_audit | completed | Define baseline audit report shape | `references/audit-output.md` |
| t_v2_antibl | completed | Add anti-bloat test for heavy local installs | `SKILL.md`, `README.md` |
| t_v2_trim | completed | Rework skill trim around enabled-vs-installed accounting | `references/skill-trim.md` |
| t_v2_rewrite | completed | Rewrite SKILL.md, README, llms files, CHANGELOG | v2.0.0 published |

## Verification evidence

- Desktop routes confirmed against the app source in `apps/desktop/src/app`
  (`/settings?tab=config:model`, `/capabilities?tab=plugins`, `/capabilities?tab=skills`,
  `/capabilities?tab=connectors`, per-plugin settings pages)
- CLI subcommands confirmed present before documenting: `plugins search|install|list|doctor|capabilities|check-updates`,
  `skills search|install|list`, `fallback list`, `doctor`, `security audit`, `tools list`, `memory status`
- all internal `references/` links resolve to files that exist
- no remaining references to the retired `platform-skill-trim.md` outside the changelog entry
- `SKILL.md` frontmatter validated: name, description, version 2.0.0, tags, related skills
- the author's own routing removed from every public example