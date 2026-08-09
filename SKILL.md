---
name: hermes-optimizer
description: "Use when optimizing Hermes Agent cost, routing, skills, and guardrails. Audits live config, detects available providers/models from active subscriptions and credentials, verifies routes, and applies safe high-ROI fixes."
version: 1.4.1
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [hermes, optimization, cost-control, routing, skills, guardrails, finops]
    related_skills: [hermes-agent, hermes-auxiliary-optimization, token-budget-guard, cost-control]
---

# Hermes Optimizer

## Overview

Optimize Hermes Agent for lower spend, better routing, safer autonomous behavior, and lower prompt overhead.

This skill is intentionally **provider-agnostic**. It does not assume one fixed model catalog or one user's subscription. Instead, it teaches an optimization method that starts from **live autodiscovery** of the user's provider stack and then applies lane-based routing and guardrail improvements.

The user should not have to manually list their models. The skill should inspect the configured providers, authenticated subscriptions, fallback routes, and auxiliary lanes; discover what is actually available; and only ask the user if the environment cannot be inspected directly.

The highest-ROI optimization order is usually:

1. auxiliary routing
2. runaway-loop protection
3. checkpoints
4. fallback chain
5. skills prompt-bloat reduction

## When to Use

Use this skill when:

- the user says "optimize Hermes"
- the user is burning too many tokens or provider credits
- auxiliary tasks are all using the main model
- the system feels expensive, noisy, or unstable
- there are too many enabled skills and prompt overhead is growing
- the user wants a reusable optimization baseline for their Hermes setup

Do **not** use this skill as a hardcoded provider recipe for every user. Infer the provider stack first.

## Core Rules

1. **Inspect the active Hermes configuration first.** Start with the current Hermes config exposed in this environment.
2. **Infer the provider stack and accessible subscriptions.** Determine which providers, deployments, and authenticated routes the user already has.
3. **Inventory models automatically from live state.** Prefer configured providers, authenticated subscriptions, fallback chains, auxiliary routes, and provider/model commands over asking the user to list models manually.
4. **Verify models with real calls.** Never trust stale caches, screenshots, or old notes alone.
5. **Preserve user intent.** If the user wants Azure GPT as main, optimize around that instead of replacing it.
6. **Use lane-based routing.** Pick models by task class, not by hype.
7. **Keep examples separate from policy.** User-specific mappings belong in notes/references, not the universal skill body.
8. **Back up config before edits.**
9. **Re-read and verify after every config change.**

## Provider Autodiscovery

Before proposing any optimization, determine:

- current main provider
- current main model
- fallback provider(s), if any
- current auxiliary routing
- whether vision is configured separately
- whether the user is on a single-provider or mixed-provider setup
- which providers are actually configured or authenticated for this Hermes install
- which model routes are actually reachable through those subscriptions / credentials

Preferred evidence order:

1. the active Hermes configuration file and current runtime settings
2. Hermes CLI state (`hermes config`, `hermes fallback list`, related live commands)
3. real one-shot model tests
4. user statement, if the environment is inaccessible

If the config and the user's statement disagree, trust the live config and then ask about intentional overrides.

### Automatic subscription and model discovery

The skill should prefer automatic discovery over questionnaires.

Default discovery flow:
1. inspect which providers are present in `config.yaml`
2. inspect fallback and auxiliary provider assignments
3. inspect available authentication / credential state exposed through Hermes
4. use provider/model commands available in the current install to discover reachable models
5. if no model-list command exists for a provider, probe likely routes with real one-shot Hermes calls
6. build heavy / medium / light / vision candidates only from models that are actually reachable

The user should **not** have to type out "I have subscription X and models Y/Z" unless live inspection is blocked.

## Lane Model

### Heavy lane
Use a stronger long-context text model for:
- `compression`
- `web_extract`
- `kanban_decomposer`
- `curator`

Selection criteria:
- best verified long-context behavior
- strong summarization / reasoning quality
- acceptable cost relative to the user's stack

### Medium lane
Use a fast, cheaper reasoning/classifier model for:
- `approval`
- `triage_specifier`
- `profile_describer`

Selection criteria:
- low latency
- acceptable judgment quality
- much cheaper than the main model

### Light / internal-search lane
Use the cheapest stable text model for:
- `session_search`
- `skills_hub`
- `mcp`
- `title_generation`

Selection criteria:
- cheap
- stable short outputs
- enough context window for Hermes
- good at summarizing already-retrieved material

### Vision lane
Use a **verified live multimodal** route only.

If the alternate provider has no proven live vision model:
- keep `auxiliary.vision` on the known-good main provider

## Optimization Workflow

### Step 1 - Audit live configuration
Check:
- main provider/model
- fallback chain
- auxiliary block
- approvals mode
- loop guardrails
- checkpoints
- enabled/disabled skill scope

### Step 2 - Infer provider profile
Classify the user's setup as one of:
- single-provider premium stack
- single-provider budget stack
- mixed stack
- main-on-premium plus aux-on-cheaper-provider

This classification is descriptive only. It helps choose candidates but does not replace real verification.

### Step 3 - Inventory live model options
Gather the real model catalog available to this user.

Default method:
- inspect configured providers and authenticated subscriptions first
- inspect fallback and auxiliary providers already present in the config
- use Hermes provider/model commands available in the current install to enumerate reachable models
- if enumeration is unavailable, probe likely candidates with real one-shot calls
- avoid asking the user to manually list models unless the environment is inaccessible

Then classify candidates by lane:
- heavy
- medium
- light
- vision

### Step 4 - Verify real candidates
Run real one-shot tests.

Reject models that:
- 404 live
- are retired
- fail Hermes minimum context requirements
- have unstable or malformed behavior on simple smoke tests

### Step 5 - Apply high-ROI config changes
Typical wins:
- split auxiliary task slots away from the premium main model
- enable `tool_loop_guardrails.hard_stop_enabled`
- enable `checkpoints.enabled`
- switch `approvals.mode` to `smart`
- add a real fallback chain
- disable obviously irrelevant skills
- enable curator consolidation where appropriate

### Step 5a - Write auxiliary routing in the exact config shape Hermes Desktop expects
Do **not** invent umbrella keys like `heavy`, `medium`, or `light` under `auxiliary`.
The desktop Models page only recognizes the canonical task slots:
- `vision`
- `web_extract`
- `compression`
- `skills_hub`
- `approval`
- `mcp`
- `title_generation`
- `triage_specifier`
- `kanban_decomposer`
- `profile_describer`
- `curator`

Write auxiliary routing as nested per-task dicts under `auxiliary`, for example:

```yaml
auxiliary:
  vision:
    provider: azure-foundry
    model: gpt-5.4
  web_extract:
    provider: ollama-cloud
    model: kimi-k2.5
  compression:
    provider: ollama-cloud
    model: kimi-k2.5
  approval:
    provider: ollama-cloud
    model: deepseek-v4-flash
  skills_hub:
    provider: ollama-cloud
    model: gpt-oss:20b
```

For custom/local endpoints, persist the endpoint on the task itself, not only on the main model:

```yaml
auxiliary:
  compression:
    provider: custom
    model: my-model
    base_url: http://127.0.0.1:1234/v1
    api_key: dummy
```

If the config is written in any other shape, Hermes Desktop may keep showing the old settings even though the YAML changed.
### Step 5b - Trim irrelevant skills by platform
Use platform-aware skill cleanup as part of Hermes optimization.

Rules:
- prefer updating `skills.disabled` over deleting skill files
- on Windows, auto-disable Apple/macOS-only skills unless the user explicitly needs them
- also disable clearly irrelevant leisure / smart-home / narrow specialist integrations when they are not part of the user's workflow
- if imported source-skills were installed only to build a stronger umbrella skill, disable them after synthesis so they stop inflating prompt overhead

See supporting references for concrete Windows-safe default candidates.

### Step 6 - Verify after changes
After editing:
1. re-read `config.yaml`
2. verify fallback with `hermes fallback list`
3. smoke-test the main model
4. restart Hermes when needed
5. confirm the new session behavior instead of trusting UI alone

## Skills Overhead Guidance

Too many enabled skills can increase prompt overhead and increase the chance of irrelevant skill activation.

Safe cleanup pattern:
- disable obviously irrelevant platform-specific skills
- on Windows, treat Apple/macOS-only skills as default disable candidates
- disable unused gaming / server / red-team skills unless the user wants them
- disable imported source-skills once their logic has been absorbed into a stronger umbrella skill
- keep a provenance note so the cleanup is reversible
- prefer disabling over deleting
- tell the user plainly that disabling changes the **enabled** count, not the **installed** count
- if the user wants fewer installed skills in `hermes skills list`, treat that as a separate uninstall/delete step

## Common Pitfalls

1. **Hardcoding one provider catalog as universal truth**
2. **Skipping provider autodiscovery and optimizing the wrong stack**
3. **Routing light tasks to expensive frontier models**
4. **Trusting cached model lists without real tests**
5. **Leaving `hard_stop_enabled` off on a cost-sensitive setup**
6. **Treating user-specific routing snapshots as official defaults**
7. **Assuming vision alternatives exist when they are retired or broken**

## Verification Checklist

- [ ] live `config.yaml` inspected
- [ ] provider stack inferred from live state
- [ ] accessible providers / subscriptions detected automatically where possible
- [ ] config backup created before edits
- [ ] main model preserved unless user requested otherwise
- [ ] fallback chain configured and listed
- [ ] heavy/medium/light/vision lanes separated
- [ ] loop hard-stop enabled
- [ ] checkpoints enabled when appropriate
- [ ] irrelevant skills disabled instead of deleted
- [ ] every candidate model verified with a real test call
- [ ] the user was not asked to enumerate models unless live inspection was blocked
- [ ] post-change behavior checked after restart

## Provenance

This skill is a synthesis of multiple optimization-oriented skills plus Hermes-specific runtime verification.

See supporting references for:
- imported upstream skills
- synthesis map
- provider autodiscovery guide
- platform-aware skill trim guide
- provider-profile examples
