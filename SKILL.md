---
name: hermes-optimizer
description: "Use when optimizing Hermes Agent cost, routing, skills, and guardrails. Audits config, reduces token waste, and applies safe high-ROI fixes."
version: 1.1.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [hermes, optimization, cost-control, routing, skills, guardrails]
    related_skills: [hermes-agent, hermes-auxiliary-optimization, token-budget-guard, cost-control]
---

# Hermes Optimizer

## Overview

Use this skill when the user wants Hermes itself optimized for lower cost, better routing, safer autonomous operation, and lower prompt overhead.

This skill is opinionated toward high-ROI, low-risk changes first:
1. auxiliary routing
2. runaway-loop protection
3. checkpoints
4. fallback chain
5. skills prompt bloat reduction

## Recommended lane split

Do **not** hardcode one user's provider catalog as the universal answer.
Choose by lane and by live availability.

- Heavy: `compression`, `web_extract`, `kanban_decomposer`, `curator`
  - choose the strongest affordable long-context text model that actually works in that user's stack
- Medium: `approval`, `triage_specifier`, `profile_describer`
  - choose a fast, cheap reasoning/classifier model
- Light/internal-search: `session_search`, `skills_hub`, `mcp`, `title_generation`
  - choose the cheapest stable model with clean short outputs and enough context
- Vision:
  - choose a verified live multimodal model; if the alternate provider has none, keep vision on the known-good main provider

## Rules

1. Read live `~/.hermes/config.yaml` first.
2. Verify candidate models with real test calls.
3. Keep the user's preferred main model/provider unless they ask to change it.
4. Use cheap models for internal search/routing tasks.
5. Use stronger models for heavy auxiliary tasks.
6. Backup config before edits.
7. Re-read config and verify after edits.

## Provider-profile examples

Provider-specific mappings should live in `references/` or `notes/`, not in the core body of the skill.
This repo's `notes/current-routing.md` is one user-specific snapshot, not a global default.
