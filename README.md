# Hermes Optimizer

A GitHub-ready skill project for **optimizing Hermes Agent** across different provider stacks.

This project is designed to become a **publishable Hermes Ecosystem skill** that helps users reduce spend, lower prompt bloat, improve routing, and apply safer defaults **without assuming one fixed provider or one person's subscription**.

## Core idea

`hermes-optimizer` must **discover**, not assume.

When a user says:
- "help me optimize Hermes"
- "help me reduce spend"
- "what should I move off my main model?"

…the skill should first determine:
1. which provider(s) the user is actually using,
2. which model is currently main,
3. which auxiliary routes already exist,
4. which fallback chain exists,
5. which models are really available in that user's live catalog,
6. which of those models actually work under Hermes.

Only then should it recommend routing changes.

## What the skill does

`hermes-optimizer` helps with:

- auxiliary model routing
- fallback chain setup
- loop / runaway spend protection
- checkpoints and safer defaults
- skill-bloat reduction
- provider autodiscovery from live Hermes config
- per-user optimization based on the user's actual catalog and budget

## Design principle

This skill separates two layers:

1. **Universal optimization logic**
   - heavy / medium / light / vision lanes
   - live verification before trusting cached model catalogs
   - safer defaults for cost control and stability
   - provider autodiscovery before proposing changes

2. **Provider-profile examples**
   - concrete mappings for specific stacks
   - kept in notes/references, not in the core skill body

That separation is the main reason this repo exists.

## Autodiscovery principle

The skill should infer the provider from live Hermes state whenever possible.

Primary signals:
- `~/.hermes/config.yaml`
- current `model.provider` and `model.default`
- configured `auxiliary.*` routes
- configured fallback chain
- live model tests through Hermes CLI

This makes the skill usable for:
- Azure-first users
- Ollama Cloud users
- mixed-stack users
- users with completely different subscriptions or provider catalogs

## Repository layout

- `SKILL.md` — the publishable skill draft
- `PUBLISHING.md` — GitHub + Hermes Ecosystem publication workflow
- `references/imported-skills.md` — upstream skills imported and used as idea sources
- `references/synthesis-map.md` — explains how the final skill was synthesized
- `references/provider-autodiscovery.md` — how the skill should infer providers and live catalogs
- `notes/current-routing.md` — one user-specific routing snapshot, explicitly **not** a global default

## Provenance

This repo is not a blind clone of one upstream skill.
It synthesizes ideas from several optimization-focused skills and then filters them through Hermes-specific runtime checks.

See:
- `references/imported-skills.md`
- `references/synthesis-map.md`
- `references/provider-autodiscovery.md`

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
- clear autodiscovery story
- clear distinction between universal logic and user-specific examples

### Hermes Ecosystem
The final skill should be usable by:
- Azure-first users
- Ollama Cloud users
- mixed-stack users
- users with different model catalogs and budgets
- users who only say "optimize Hermes" and expect the skill to infer the rest

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

## Current state

This repo is now GitHub-ready as a public skill project.
The remaining work is publication and optional further refinement of examples.
