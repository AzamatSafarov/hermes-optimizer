---
name: hermes-optimizer
description: "Use when optimizing or bringing up a Hermes Agent install. Guided Desktop-driven phases for providers, auxiliary routing, plugins, document tooling, skills, and guardrails. Asks by phase, acts in the app, verifies live."
version: 2.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [hermes, optimization, cost-control, routing, plugins, skills, guardrails, onboarding, setup]
    related_skills: [hermes-agent, azure-foundry-models, kanban-model-routing]
---

# Hermes Optimizer

## Overview

Bring a Hermes Agent install into shape: the right providers, sane auxiliary routing, the plugins
that should have been there from day one, document tooling that makes PDFs and Markdown readable,
skills without bloat, and guardrails that stop runaway spend.

Two modes:

- **Bring-up** - a fresh or messy install where little is configured. Walk the phases and install
  and set up as you go.
- **Tune** - a working install that is expensive, noisy, or drifting. Audit, then fix only the
  weak links.

Both modes share one phase plan. Bring-up runs all phases; tune runs the ones that audit dirty.

## The honest framing

A stock Hermes install is **thin by default**. Not broken, but it starts minimal:

- one main model and no fallback chain
- most auxiliary task slots pinned to the main model, so background work costs main-model money
- no third-party plugins, so no plugin toolsets at all
- a large bundled skill set where most entries are irrelevant to any one user
- `tool_loop_guardrails.hard_stop_enabled` off, so a looping agent can burn budget
- no local document or data tooling

Say this plainly at the start. Do not imply the install was already good and you are only
polishing it. The user should know what a phase buys before agreeing to it.

## Core rules

1. **Ask first, act second.** Every phase opens with questions. Never bulk-apply a preset.
2. **Work in the Desktop app, not in the terminal.** The user's hands are on the app. Send them
   to the app surface that owns the setting. Real routes are in `references/desktop-surfaces.md`.
3. **The CLI is for reading state, verifying, and for what the app has no field for.** Do not make
   the user live in a terminal.
4. **Infer before asking.** Read the live config first. Ask only what inspection cannot answer:
   intent, preferences, budget posture. Never ask a user to list models you can enumerate.
5. **Verify models with real calls.** Reject 404s, retired models, and broken routes before
   recommending them. A picker entry is not proof of a live route.
6. **Preserve intent.** If the user wants a particular provider as main, optimize around it. Do
   not silently swap their main model.
7. **One phase, one checkpoint.** Do not start the next phase before the user confirms this one.
8. **Back up config before edits, and re-read to verify after.**
9. **Do not recommend a heavy local model that duplicates a capability already live.** See below.

## Silly vs smart: the anti-bloat test

Run every proposed install through this before recommending it.

**Reject by default:**

- a multi-gigabyte local OCR model when the configured provider already has a working vision
  route - the local stack is a second, worse copy of a live capability you now have to maintain
- a local embedding or reranker model when semantic recall is already served by the configured
  context engine, or when plain retrieval is enough
- any local model that duplicates a hosted route the user already pays for
- bundled skills for platforms the host does not run (Apple/macOS-only skills on Windows)
- narrow single-purpose integrations the user never mentioned

**Recommend by default:**

- a document and data loader for PDF and Markdown - structured text extraction is the base layer
  other capabilities sit on, and hosted vision does not replace it
- guardrails, checkpoints, and a real fallback chain - these are config, not downloads
- auxiliary routing that moves non-critical work off the premium main model
- plugins that fill a capability gap the install genuinely has, not plugins that merely impress

Say the reasoning out loud when you reject something. "You already have vision on your main
provider, so a local OCR model would be a downgrade you maintain yourself" is a useful sentence.

## The phase plan

Run in order. Each phase is **questions -> actions -> checkpoint**. Per-phase detail, question
sets, and checkpoint criteria live in `references/phase-plan.md`.

| Phase | Name | One-line goal |
|-------|------|---------------|
| 0 | Baseline audit | Read live state, report honestly what is thin |
| 1 | Provider and main model | Set provider, default model, reasoning, context |
| 2 | Auxiliary and fallback routing | Split lanes off the main model, add a real fallback |
| 3 | Plugins | Install the baseline set matched to the user's work |
| 4 | Local utilities and document tooling | Make PDFs and Markdown readable |
| 5 | Skills | Disable noise, keep the real domains |
| 6 | Guardrails, checkpoints, safety | Hard stop, checkpoints, smart approvals, audits |
| 7 | Verification and handoff | Re-read, smoke-test, before/after summary |

## Lane model

| Lane | Task slots | Selection criteria |
|------|-----------|--------------------|
| Heavy | `compression`, `web_extract`, `kanban_decomposer`, `curator`, `goal_judge`, `moa_aggregator` | strongest verified long-context model at acceptable cost |
| Medium | `approval`, `triage_specifier`, `profile_describer`, `review`, `background_review` | low latency, cheap, adequate judgment |
| Light | `skills_hub`, `mcp`, `title_generation`, `memory_query_rewrite`, `tts_audio_tags`, `monitor`, `moa_reference` | cheapest stable text model with enough context |
| Vision | `vision` | only a verified live multimodal route |

If no genuinely cheaper alternative exists, say so and optimize guardrails and skills instead of
inventing a lane split that is not real.

## Auxiliary config shape

Write auxiliary routing as nested per-task dicts under `auxiliary`, using the canonical slot
names. Do not invent umbrella keys like `heavy`, `medium`, or `light` - the Desktop Models page
does not recognize them and will keep showing stale values. Full shape and the custom-endpoint
case are in `references/auxiliary-config-shape.md`.

## Common pitfalls

1. Hardcoding one provider catalog as universal truth.
2. Asking the user to list models before reading live state.
3. Routing light internal tasks to frontier models.
4. Trusting a cached model list or a picker entry as proof of a live route.
5. Leaving `hard_stop_enabled` off on a cost-sensitive install.
6. Recommending a heavy local model that duplicates an already-live hosted route.
7. Reporting a disabled skill as an uninstalled skill.
8. Driving the user into the CLI when the app owns that surface.
9. Applying a whole phase without its checkpoint.
10. Assuming a vision alternative exists when the alternate provider has no live multimodal route.

## Verification checklist

- [ ] live config inspected before any question was asked
- [ ] honest "starts thin" framing given
- [ ] config backed up before edits
- [ ] main model preserved unless the user asked otherwise
- [ ] auxiliary lanes split off the main model where verified alternatives exist
- [ ] fallback chain configured on a different route than the main where possible
- [ ] every candidate model verified with a real call
- [ ] plugin installs verified to expose their tools
- [ ] heavy duplicates rejected, with the reason stated
- [ ] skills disabled rather than deleted, and the enabled/installed distinction stated
- [ ] loop hard-stop enabled
- [ ] checkpoints enabled where risky file work happens
- [ ] supply-chain audit and doctor run
- [ ] Desktop pages checked to reflect the new state
- [ ] every phase checkpointed with the user

## Provenance

A synthesis of upstream optimization skills plus Hermes-specific runtime verification. Which
inputs were adopted, which were rejected, and why is in `references/synthesis-map.md`.

Supporting references:

- `references/phase-plan.md` - per-phase questions, actions, checkpoints
- `references/desktop-surfaces.md` - real Desktop routes for each phase
- `references/plugin-baseline.md` - the baseline plugin set and the fit test
- `references/auxiliary-config-shape.md` - canonical auxiliary YAML shape
- `references/audit-output.md` - baseline report shape
- `references/skill-trim.md` - platform-aware skill trim rules
- `references/provider-autodiscovery.md` - provider detection rules
- `references/synthesis-map.md` - provenance
- `notes/current-routing.md` - generic provider-profile examples