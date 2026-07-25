# Provider-profile examples

This file contains **generic provider-profile examples**, not any one user's private routing snapshot.

## Rule

Use these as shape examples only.
Do **not** copy the exact mappings blindly.
Always detect the active provider stack, inspect the live catalog, and verify candidate models with real calls.

## Example A — Premium main + cheaper aux

- Main provider: premium frontier model
- Fallback: cheaper secondary provider
- Heavy lane: strongest verified long-context budget model
- Medium lane: fast classifier / reasoning model
- Light lane: cheapest stable text model with enough context
- Vision lane: keep on the only verified multimodal provider

## Example B — Single-provider setup

- Main provider and aux provider are the same
- Heavy lane stays on the strongest available long-context model
- Medium and light lanes use cheaper models from the same provider if available
- If no real cheaper alternatives exist, optimize guardrails, checkpoints, and skills noise first

## Example C — Mixed stack

- Main model stays where the user prefers it
- Auxiliary tasks move to whichever second provider offers cheaper verified routes
- Fallback should ideally sit on a different provider than the main route

## Generalization rule

When adapting to a real user:
- keep the lane split
- replace exact models with live verified equivalents from that user's provider catalog
- do not assume any provider or subscription is available until verified
