# Hermes Optimizer

A GitHub-ready skill project for **optimizing Hermes Agent** across different provider stacks.

This project is designed to become a **publishable Hermes Ecosystem skill** that helps users reduce spend, lower prompt bloat, improve routing, and add safer defaults without overfitting to one person's config.

## Status

- **Scope:** provider-agnostic Hermes optimization
- **Maturity:** synthesis draft with live-tested patterns
- **Source basis:** multiple imported optimization skills + Hermes-specific verification
- **Target:** publishable skill for the Hermes Ecosystem

## What the skill does

`hermes-optimizer` helps with:

- auxiliary model routing
- fallback chain setup
- loop / runaway spend protection
- checkpoints and safer defaults
- skill-bloat reduction
- provider-specific optimization without hardcoding one user's catalog

## Design principle

This skill separates two layers:

1. **Universal optimization logic**
   - heavy / medium / light lane split
   - live verification before trusting cached model catalogs
   - safer defaults for cost control and stability

2. **Provider-profile examples**
   - concrete mappings for Azure-only, Ollama Cloud, mixed-stack users
   - kept in notes/references, not in the core skill body

That split is the main reason this repo exists.

## Repository layout

- `SKILL.md` — the publishable skill draft
- `references/imported-skills.md` — upstream skills imported and used as idea sources
- `references/synthesis-map.md` — explains how the final skill was synthesized
- `notes/current-routing.md` — one user-specific routing snapshot, explicitly **not** a global default
- `PUBLISHING.md` — GitHub + Hermes Ecosystem publication workflow

## Provenance

This repo is not a blind clone of one upstream skill.
It synthesizes ideas from several optimization-focused skills and then filters them through Hermes-specific runtime checks.

See:
- `references/imported-skills.md`
- `references/synthesis-map.md`

## Installed upstream source skills used in synthesis

- `token-budget-guard`
- `cost-control`
- `context-compression`
- `prompt-caching`
- `token-budget-advisor`
- `cost-aware-llm-pipeline`
- `token-optimization`

## Publishing goals

### GitHub
This repo should be understandable on its own:
- clear scope
- clear provenance
- clear distinction between universal logic and user-specific examples

### Hermes Ecosystem
The final skill should be usable by:
- Azure-first users
- Ollama Cloud users
- mixed-stack users
- users with different model catalogs and budgets

## Current publication commands

Hermes exposes the following CLI surfaces relevant to publication:

```bash
hermes skills publish <skill_path>
hermes skills publish --to github <skill_path>
hermes skills publish --to clawhub <skill_path>
hermes skills publish --to github --repo owner/repo <skill_path>
```

And for GitHub-backed discovery sources:

```bash
hermes skills tap add owner/repo
```

## Next steps

- tighten the core `SKILL.md` into a final official-style version
- optionally add provider-profile reference files (`azure-only.md`, `ollama-cloud.md`, `mixed-stack.md`)
- publish to GitHub first
- then publish or submit through the Hermes skill ecosystem flow
