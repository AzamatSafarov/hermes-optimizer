# Current routing snapshot — user-specific example

This file is **not a universal default**.
It is one concrete routing snapshot for one Hermes setup:
- main GPT on Azure
- auxiliary split across Ollama Cloud
- vision kept on Azure because a verified live VL route was not available in the alternate provider at the time

## Snapshot

Main:
- `azure-foundry / gpt-5.4`

Fallback:
- `ollama-cloud / kimi-k2.6`

Auxiliary:
- `vision` → `azure-foundry / gpt-5.4`
- `compression` → `ollama-cloud / kimi-k2.5`
- `web_extract` → `ollama-cloud / kimi-k2.5`
- `kanban_decomposer` → `ollama-cloud / kimi-k2.5`
- `curator` → `ollama-cloud / kimi-k2.5`
- `approval` → `ollama-cloud / deepseek-v4-flash`
- `triage_specifier` → `ollama-cloud / deepseek-v4-flash`
- `profile_describer` → `ollama-cloud / deepseek-v4-flash`
- `session_search` → `ollama-cloud / gpt-oss:20b`
- `skills_hub` → `ollama-cloud / gpt-oss:20b`
- `mcp` → `ollama-cloud / gpt-oss:20b`
- `title_generation` → `ollama-cloud / gpt-oss:20b`

## Generalization rule

When adapting this to another user/provider stack:
- keep the lane split
- replace the exact models with live verified equivalents from that user's provider catalog
- do not assume Ollama Cloud availability unless it is actually present
