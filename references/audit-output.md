# Audit output shape

Phase 0 produces this report. It is read-only and it is the honest baseline. Keep it short and
keep it true.

## Template

```text
HERMES BASELINE

Main route
  provider   : <provider>
  model      : <model>
  reasoning  : <level>
  context    : <tokens>

Fallback
  chain      : <none | list of provider/model, with a note if it points back at the main route>

Auxiliary routing
  on main model : <count> of <total> slots   <- this is the expensive part
  split out     : <count>
  slots pinned  : <slot names>

Vision
  route      : <provider/model, or "not configured">
  verified   : <yes/no - did you see it answer on an image>

Plugins
  installed  : <count>
  enabled    : <count>

Skills
  enabled    : <count>
  installed  : <count>     <- state both, they differ

Guardrails
  loop hard stop : <on/off>
  checkpoints    : <on/off>
  approvals      : <mode>

Context engine
  engine     : <name>

WHAT MAKES MONEY LEAVE

1. <highest-ROI problem, one line>
2. <next>
3. <next>

WHAT IS MISSING

1. <capability gap, one line>
2. <next>

SUGGESTED PHASES
  Run: <phase numbers that audit dirty>
  Skip: <phase numbers already clean>
```

## The honest framing line

Include one plain sentence about the starting state, in the user's language. It should not be
softened and it should not be alarmist. The content:

- the install starts with one main model and no fallback
- background auxiliary work is running on the main model, so it costs main-model money
- there are no third-party plugins, so plugin toolsets do not exist yet
- the bundled skill set is large and mostly irrelevant to this user
- loop hard stop is off, so a stuck agent can spend without a ceiling

Then the ranked list of what is worth fixing. Rank by return, not by ease.

## Ranking rule

Order the findings by money and pain saved per change:

1. auxiliary slots pinned to a premium main model - usually the largest single saving
2. missing loop hard stop - protects against a large one-off loss
3. no real fallback - a provider outage stops all work
4. missing capability that blocks the user's actual job - plugins and document tooling
5. skill noise - small prompt cost, but easy and reversible
6. cosmetic config drift

Do not present a flat list with no order. The user needs to know what to do first.

## What not to put in the report

- a model list. Enumerate internally, not in the report.
- screenshots or cached catalog dumps as evidence of a live route
- any claim of verification you did not perform. If you did not see a model answer, say
  "configured, not verified" rather than implying it works.