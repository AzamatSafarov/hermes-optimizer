# Hermes optimization skill synthesis map

This document explains how the local `hermes-optimizer` skill was derived from multiple upstream optimization skills plus live Hermes-specific verification.

## Core claim

`hermes-optimizer` is **not** a copy of any one source.
It is a synthesis layer that:
1. imports useful optimization ideas from several hub skills,
2. rejects unsafe or overfitted advice,
3. re-expresses the result as provider-agnostic Hermes guidance,
4. validates concrete routing choices against live Hermes config and real model calls.

## Installed upstream inputs

| Upstream skill | Source | Status | Main contribution to synthesis |
|---|---|---|---|
| `token-budget-guard` | clawhub/community | installed | progressive disclosure, context budgeting, compression triggers |
| `cost-control` | clawhub/community | installed | runaway-spend protection, kill-switch mindset, tiered cost controls |
| `context-compression` | skills.sh/community | installed | tokens-per-task framing, compression quality vs re-fetch cost |
| `prompt-caching` | skills.sh/community | installed | prompt/response caching concepts and cache invalidation awareness |
| `token-budget-advisor` | skills.sh/community | installed | explicit token-depth tradeoff thinking |
| `cost-aware-llm-pipeline` | skills.sh/community | installed | lane-based model routing by task complexity |
| `token-optimization` | skills.sh/community | installed | API/tool-call efficiency patterns, anti-pattern library |

## Inspected but not adopted as installs

| Skill | Status | Reason |
|---|---|---|
| `hermes-agent-health-check` | blocked | scanner caution/block; not auto-installed |
| `context-budget` | blocked | dangerous verdict tied to agent config / CLAUDE-style persistence assumptions |
| `context-degradation` | blocked | caution verdict from scanner |
| `token-efficiency` | blocked | caution verdict and also contains Claude-specific assumptions that are not safe to import blindly |

## How the synthesis works

### 1. Universal optimization logic kept
Adopted into `hermes-optimizer`:
- heavy / medium / light lane split
- verify models with real calls
- prefer cheap models for internal routing/search
- prefer stronger models for heavy auxiliary work
- use fallback chains
- enable spend/safety guardrails
- reduce prompt bloat from irrelevant skills

### 2. Overfitted provider advice removed
Not kept as universal truth:
- fixed Ollama Cloud model names as defaults for everyone
- Claude Code specific assumptions as global Hermes policy
- any single vendor's cache/model routing as the one true recipe

### 3. Hermes-specific verification added
This part comes from live work, not just imported skills:
- read live `~/.hermes/config.yaml`
- verify current provider catalogs
- test specific models with `hermes chat -q ...`
- reject stale cache entries / retired models / 404 deployments
- treat provider-specific mappings as examples, not universal defaults

## Resulting philosophy of `hermes-optimizer`

The skill now answers two different layers:

1. **Universal layer**
   - how to think about Hermes optimization for any user
2. **Provider-profile layer**
   - concrete model mappings for a specific live catalog

That separation is the main design goal and the main difference from raw imported hub skills.
