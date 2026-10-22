# Synthesis map

`hermes-optimizer` is a synthesis of multiple upstream optimization skills plus live Hermes-specific verification.

## Upstream ideas imported

- `token-budget-guard` → context budgeting, progressive disclosure
- `cost-control` → runaway spend protection, tiered guardrails
- `context-compression` → optimize tokens per task, not just tokens per request
- `prompt-caching` → caching and invalidation concepts
- `token-budget-advisor` → explicit token-depth tradeoff framing
- `cost-aware-llm-pipeline` → task-complexity lane routing
- `token-optimization` → tool/API token-efficiency patterns

## What makes this skill different

This skill is not a direct copy of any source skill.
It adds Hermes-specific rules:
- inspect the active Hermes configuration first
- verify models with real calls
- keep provider examples separate from universal logic
- reject stale cache entries, retired models, and broken deployments
- treat user/provider-specific mappings as examples only

## Non-imported / blocked candidates

Examples of inspected but not blindly absorbed skills:
- `hermes-agent-health-check`
- `context-budget`
- `context-degradation`
- `token-efficiency`

These were either blocked by the scanner or too tied to another tool/runtime model to import unchanged.
