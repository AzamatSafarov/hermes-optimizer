---
name: hermes-optimizer
description: "Use when optimizing Hermes Agent cost, routing, skills, and guardrails. Audits config, reduces token waste, and applies safe high-ROI fixes."
version: 1.0.0
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

- Heavy: `compression`, `web_extract`, `kanban_decomposer`, `curator` → `kimi-k2.5`
- Medium: `approval`, `triage_specifier`, `profile_describer` → `deepseek-v4-flash`
- Light/internal-search: `session_search`, `skills_hub`, `mcp`, `title_generation` → `gpt-oss:20b`
- Vision: keep on the actually working multimodal provider; for this setup that is Azure GPT.

## Rules

1. Read live `~/.hermes/config.yaml` first.
2. Verify candidate models with real test calls.
3. Keep the user's preferred main model/provider unless they ask to change it.
4. Use cheap models for internal search/routing tasks.
5. Use stronger models for heavy auxiliary tasks.
6. Backup config before edits.
7. Re-read config and verify after edits.
