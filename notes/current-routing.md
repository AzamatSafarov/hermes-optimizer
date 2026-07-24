# Current routing snapshot

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

Safety:
- `approvals.mode = smart`
- `checkpoints.enabled = true`
- `tool_loop_guardrails.hard_stop_enabled = true`
- `curator.consolidate = true`
