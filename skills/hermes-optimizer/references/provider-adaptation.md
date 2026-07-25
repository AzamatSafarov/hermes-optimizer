# Provider adaptation guide

This skill must not assume one fixed provider catalog.

## Universal lane logic

- Heavy: `compression`, `web_extract`, `kanban_decomposer`, `curator`
- Medium: `approval`, `triage_specifier`, `profile_describer`
- Light/internal-search: `session_search`, `skills_hub`, `mcp`, `title_generation`
- Vision: any verified multimodal route

## Selection rules

1. Verify candidate models with a real one-shot call.
2. Reject stale cache entries, retired models, and 404 deployments.
3. For light/internal-search, prefer the cheapest stable model with enough context.
4. For heavy tasks, prefer long-context quality over lowest price.
5. Store exact provider-specific mappings as examples only.

## Example categories

### Azure-only user
- main can stay on GPT
- heavy lane may stay on Azure if no cheaper verified lane exists
- vision often stays on Azure

### Ollama Cloud user
- heavy lane can move to Kimi / DeepSeek Pro class models
- medium lane can move to DeepSeek Flash / MiniMax class models
- light lane can move to GPT-OSS / other cheap text models

### Mixed stack
- keep the user's preferred main model
- route auxiliary by cost/capability class
- use a fallback on a different provider when possible
