# Skill calibration

A large enabled count with nothing disabled is **uncalibrated**, not clean. Report it as a finding,
not as a phase that passed.

## Why the count is the wrong metric

Enabled-equals-installed means the set was never curated for this user. Some entries cannot fire
on this host (Apple-only skills on Windows). Some are research inputs whose logic was absorbed
elsewhere. Some serve domains the user never mentioned. Each one costs prompt surface and competes
for activation.

## Read the state properly

```bash
hermes skills list
```

The summary reports `hub-installed`, `builtin`, `local`, and `enabled` / `disabled`. Do **not**
conflate installed with enabled - a skill can be installed and disabled.

## Calibrate, do not just report

Run a real calibration pass:

1. **Group the enabled set by category.** Show the user their own categories with counts.
2. **Apply the platform filter** - anything that cannot run on this host is a default-off
   candidate. See `skill-trim.md`.
3. **Ask per group, not per skill**: "these 12 are Apple-only and cannot fire here - disable?"
   beats reading 107 names aloud.
4. **Ask about absorbed inputs** - skills installed as research material for a stronger umbrella
   skill go off once absorbed.
5. **Disable, never delete.** See `skill-trim.md` for the accounting rule.
6. **Report both counts** before and after, and say which changed.

## Target shape

A calibrated install has **fewer enabled than installed**, and the disabled list is a stated
decision rather than an accident. If enabled still equals installed after a calibration pass, you
have not calibrated - you have counted.

## Bundles as the middle path

When the user needs a whole domain occasionally rather than never, a **bundle** (an alias over
several skills) keeps the capability without the always-on prompt cost. Offer it before offering
deletion.

## Honest report shape

```text
SKILLS
  enabled before : 107
  enabled after  : 84
  installed      : 107   (unchanged - disabling is not uninstalling)
  disabled       : 23

  why:  9 Apple-only (cannot fire on Windows)
        7 absorbed research inputs
        4 domains you said you do not use
        3 leisure integrations
```

If nothing was disabled because the user declined every group, say that plainly and record it as
their decision - do not report the phase as clean.
