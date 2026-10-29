# Hermes optimization skill synthesis map

This document explains how the local `hermes-optimizer` skill was derived, and what changed in
version 2.0.0.

## Core claim

`hermes-optimizer` is **not** a copy of any one source. It is a synthesis layer that:

1. imports useful optimization ideas from several hub skills,
2. rejects unsafe or overfitted advice,
3. re-expresses the result as provider-agnostic Hermes guidance,
4. adds Hermes-specific runtime mechanics that no upstream skill knows about,
5. validates concrete choices against live config and real model calls.

## Installed upstream inputs

| Upstream skill | Source | Status | Main contribution |
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
| `hermes-agent-health-check` | blocked | scanner caution verdict |
| `context-budget` | blocked | dangerous verdict, tied to agent-config / CLAUDE-style persistence assumptions |
| `context-degradation` | blocked | scanner caution verdict |
| `token-efficiency` | blocked | caution verdict, plus Claude-specific assumptions unsafe to import blindly |

## What version 2.0.0 added

Version 1.x was an audit-and-apply optimizer. It read live state, inferred the provider stack,
and applied lane routing and guardrail changes. Useful, but it had two gaps.

**Gap 1 - it did not ask.** Autodiscovery is right for facts the machine can read. It is wrong for
intent. What a user does all day, what hurts, and what budget posture they want are not in any
config file. Version 2 opens every phase with questions, and distinguishes what inspection
answers from what only the user can answer.

**Gap 2 - it ignored the Desktop app.** The optimizer told users what to change but pushed them
toward config files and CLI commands. The app owns most of these surfaces. Version 2 routes every
phase through the real Desktop pages and reserves the CLI for reading state, verifying, and the
few things with no app field.

It also added the parts a stock install most obviously lacks:

- **the plugin layer** - a stock install has no third-party plugins and therefore no plugin
  toolsets. The baseline and its fit test are new.
- **the document tooling layer** - text extraction for PDF and Markdown, with the rule to prefer
  what is already installed.
- **the anti-bloat test** - the explicit rule that a heavy local model duplicating a live hosted
  route is a downgrade, using local OCR versus a live vision route as the worked case.
- **the honest framing** - a plain statement of what a stock install is missing, ranked by return,
  rather than an implied "your install is fine, here are tweaks".

## How the synthesis works

### Universal logic kept
- heavy / medium / light lane split
- verify models with real calls
- cheap models for internal routing, stronger models for heavy auxiliary work
- use fallback chains
- enable spend and safety guardrails
- reduce prompt bloat from irrelevant skills

### Overfitted advice removed
- fixed model names as defaults for everyone
- Claude Code specific assumptions as global Hermes policy
- any single vendor's cache or routing as the one true recipe
- the author's own routing snapshot as a public example

### Hermes-specific verification added
This part comes from live work, not imported skills:
- read the live config file
- confirm Desktop routes against the app source rather than guessing
- confirm CLI subcommands exist before documenting them
- test specific models with real calls
- reject stale cache entries, retired models, and 404 deployments
- treat provider-specific mappings as examples, not universal defaults

## The two layers

The skill answers two different questions, and keeping them separate is the main design goal:

1. **Universal layer** - how to think about Hermes optimization for any user: the phases,
   the questions, the anti-bloat test, the lane model.
2. **Provider-profile layer** - concrete model mappings for a specific live catalog, kept in
   `notes/current-routing.md` as generic shapes.

That separation is what distinguishes this from the raw upstream hub skills.