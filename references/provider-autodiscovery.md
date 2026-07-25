# Provider autodiscovery for Hermes Optimizer

The main requirement of `hermes-optimizer` is: **infer first, optimize second**.

## Goal

When a user asks to optimize Hermes, the skill should determine the user's real provider stack from live state whenever possible instead of asking them to manually enumerate every model.

The default behavior should be:
- detect which providers are already configured or authenticated
- inspect which model routes are actually available through those subscriptions / credentials
- build recommendations from that live inventory
- ask the user to list models only if the environment is inaccessible or the live state is incomplete

## Evidence order

Use this order of trust:

1. **Active Hermes configuration**
   - current main provider
   - current main model
   - current auxiliary routing
   - current fallback configuration

2. **Hermes CLI state**
   - `hermes config`
   - `hermes fallback list`
   - related provider/model commands available in the current install

3. **Real one-shot tests**
   - test candidate models with Hermes itself
   - reject 404, retired, or malformed routes

4. **User statement**
   - use only when the environment is inaccessible or partially inaccessible

## Questions the skill should answer automatically

- What is the user's main provider?
- What is the user's current main model?
- Is the setup single-provider or mixed-provider?
- Is there a fallback chain?
- Are auxiliary tasks all pinned to the premium main model?
- Does the alternate provider have a live vision route?
- Which providers are actually configured or authenticated in this install?
- Which model routes are actually reachable through the user's subscriptions / credentials?
- Which cheap models are actually usable for light/internal tasks?
- Which stronger models are actually usable for heavy tasks?

## Output pattern

The skill should produce recommendations in this form:

1. **Detected environment**
   - main provider/model
   - fallback
   - aux state
   - provider profile (Azure-only / mixed / Ollama-heavy / etc.)

2. **Recommended lane mapping**
   - heavy
   - medium
   - light
   - vision

3. **Why**
   - cost/capability reasoning
   - any blockers such as no live vision alternative

4. **Verification**
   - which routes were actually tested
   - which routes were rejected and why

## What not to do

- do not ask the user to enumerate models before checking live state
- do not assume one user's routing snapshot applies globally
- do not use cache/UI visibility as proof that a model is live
- do not optimize before confirming the real provider stack
- do not replace the user's preferred main model unless they ask
