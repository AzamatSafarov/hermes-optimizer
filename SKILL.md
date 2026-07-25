---
name: hermes-optimizer
description: "Use when optimizing Hermes Agent cost, routing, skills, and guardrails. Audits live config, infers provider stack, verifies models, and applies safe high-ROI fixes."
version: 1.3.0
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
2. **Infer the provider stack.** Determine which provider/model is main and which auxiliary routes already exist.
3. **Verify models with real calls.** Never trust stale caches, screenshots, or old notes alone.
4. **Preserve user intent.** If the user wants Azure GPT as main, optimize around that instead of replacing it.
5. **Use lane-based routing.** Pick models by task class, not by hype.
6. **Keep examples separate from policy.** User-specific mappings belong in notes/references, not the universal skill body.
7. **Back up config before edits.**
8. **Re-read and verify after every config change.**

## Provider Autodiscovery

Before proposing any optimization, determine:

- current main provider
- current main model
- fallback provider(s), if any
- current auxiliary routing
- whether vision is configured separately
- whether the user is on a single-provider or mixed-provider setup

Preferred evidence order:

1. the active Hermes configuration file and current runtime settings
2. Hermes CLI state (`hermes config`, `hermes fallback list`, related live commands)
3. real one-shot model tests
4. user statement, if the environment is inaccessible

If the config and the user's statement disagree, trust the live config and then ask about intentional overrides.

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

### Step 1 — Audit live configuration
Check:
- main provider/model
- fallback chain
- auxiliary block
- approvals mode
- loop guardrails
- checkpoints
- enabled/disabled skill scope

### Step 2 — Infer provider profile
Classify the user's setup as one of:
- single-provider premium stack
- single-provider budget stack
- mixed stack
- main-on-premium plus aux-on-cheaper-provider

This classification is descriptive only. It helps choose candidates but does not replace real verification.

### Step 3 — Inventory live model options
Gather the real model catalog available to this user.

Then classify candidates by lane:
- heavy
- medium
- light
- vision

### Step 4 — Verify real candidates
Run real one-shot tests.

Reject models that:
- 404 live
- are retired
- fail Hermes minimum context requirements
- have unstable or malformed behavior on simple smoke tests

### Step 5 — Apply high-ROI config changes
Typical wins:
- split auxiliary lanes away from the premium main model
- enable `tool_loop_guardrails.hard_stop_enabled`
- enable `checkpoints.enabled`
- switch `approvals.mode` to `smart`
- add a real fallback chain
- disable obviously irrelevant skills
- enable curator consolidation where appropriate

### Step 6 — Verify after changes
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
- disable unused gaming / server / red-team skills unless the user wants them
- keep a provenance note so the cleanup is reversible
- prefer disabling over deleting

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
- [ ] config backup created before edits
- [ ] main model preserved unless user requested otherwise
- [ ] fallback chain configured and listed
- [ ] heavy/medium/light/vision lanes separated
- [ ] loop hard-stop enabled
- [ ] checkpoints enabled when appropriate
- [ ] irrelevant skills disabled instead of deleted
- [ ] every candidate model verified with a real test call
- [ ] post-change behavior checked after restart

## Provenance

This skill is a synthesis of multiple optimization-oriented skills plus Hermes-specific runtime verification.

See supporting references for:
- imported upstream skills
- synthesis map
- provider autodiscovery guide
- user-specific routing examples
