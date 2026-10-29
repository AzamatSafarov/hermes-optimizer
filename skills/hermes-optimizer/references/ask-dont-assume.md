# Ask, do not assume

The single most common failure of this skill is deciding *for* the user that a phase is
unnecessary. An audit shows a populated stack and the optimizer concludes "phase clean, skip it" -
and the user never learns that a connector, a plugin, or a workflow exists.

**A populated stack is not a calibrated stack.** 15 plugins installed says nothing about whether
the *right* 15 are installed. 107 skills enabled with 0 disabled is not "clean" - it is untouched.

## The rule

Three distinct verdicts, not interchangeable:

| Verdict | Meaning | Required action |
|---|---|---|
| **Clean** | Audit proves the phase is already optimal | May skip. Say what proved it. |
| **Populated, unverified** | Something is there, but nobody asked if it fits | **Ask.** This is the default verdict. |
| **Dirty** | Audit found a concrete defect | Ask, then fix. |

Default to **populated, unverified**. Prove "clean" before claiming it.

## What "clean" must prove

- **Phase 1 (provider)** - clean only if the route answers a real call and the user's intent is
  satisfied. A working provider does not mean the right one.
- **Phase 2 (routing)** - clean only if lanes are split off the main model **and** the fallback
  resolves to a genuinely different route. A fallback on the same provider is never clean.
- **Phase 3 (plugins/connectors)** - clean only if every category the user needs is covered.
  Count is not coverage. Ask about connectors and automation surfaces explicitly.
- **Phase 4 (documents)** - almost never clean on first contact, because the formats question has
  not been asked. Ask it.
- **Phase 5 (skills)** - clean only if the enabled set was deliberately chosen. A large enabled
  count with nothing disabled is uncalibrated, not clean.
- **Phase 6 (guardrails)** - clean only if hard stop, checkpoints, and the health sweep were
  checked and reported.

## Never assume

- do not skip a phase because a count looks healthy
- do not treat "already configured" as "correctly configured"
- do not decide what the user does not need
- do not write a bare "skipped" with no evidence

## Reporting a skip

When a phase is genuinely clean, say what proved it, in one line:

- "Phase 1 clean: the route answers a live call and you asked for this provider."
- "Phase 2 clean: lanes split, fallback on a different provider, verified."

A skip without a reason reads as an oversight, and the user is right to question it.
