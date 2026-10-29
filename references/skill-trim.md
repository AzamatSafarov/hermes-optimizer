# Skill trim

Reducing skill-layer noise is the cheapest win in the whole optimization, and the most easily
misreported. Get the accounting right.

## The accounting rule

Disabling a skill and uninstalling a skill are different operations with different numbers.

- Editing the disabled list reduces the number of **enabled** skills.
- It does **not** reduce the number of **installed** skills.

State both numbers when you report, and say which one you changed. A user who asked for "fewer
skills" and gets an enabled-count drop without being told is a user who will think the change did
not work. Say it plainly: "enabled went from N to M; installed is unchanged at K. Reducing the
installed count is a separate uninstall step, tell me if you want that."

## Disable, do not delete

Default to disabling. Disabling is reversible, does not touch bundled files, and removes the
prompt overhead and the false-activation risk just as well. Deleting is a separate, explicit
request.

Never delete a user-authored skill because it looks idle.

## Platform-aware trim

On Windows, these are safe default disable candidates unless the user says they need them:

**Apple / macOS-only** - they can never fire on a Windows host:

- `apple-notes`, `apple-reminders`, `findmy`, `imessage`, `macos-computer-use`

The inverse applies on macOS: Windows-only skills are the disable candidates.

**Leisure and side integrations** - only disable with a nod to the user, some people do want them:

- `minecraft-modpack-server`, `pokemon-player`, `spotify`, `openhue`, `himalaya`

**Narrow or risky specialists** - disable unless the user's work is exactly this:

- `godmode`, `dogfood`, `yuanbao`, `dermatology-trichology-reviews`

## Ask before cutting a whole domain

The trim list above is safe because those skills cannot serve any work the user described. Beyond
it, ask. A skill the user relies on but has not mentioned this session should not be disabled on a
guess. Phase 5 opens with the domain question for this reason.

## Imported source-skills

If optimization source-skills were installed only as research inputs for building a stronger
umbrella skill, disable them once their logic has been absorbed. The umbrella carries the
capability; the inputs are now duplicate prompt surface.

Examples of that shape: token-budget guard, cost-control, context-compression, prompt-caching,
token-budget-advisor, cost-aware-llm-pipeline, token-optimization.

Keep a provenance note so the trim is reversible.

## Bundles as an alternative

If the user has many related skills they want to keep but rarely use together, a bundle (an alias
over several skills) is often better than disabling. Propose it when a whole domain is
occasionally needed rather than never needed.

## Workflow

1. Detect the host platform.
2. Read the current disabled list and the enabled and installed counts.
3. Identify safe irrelevant candidates using the rules above.
4. Propose the trim list. Apply directly only when the user asked for broad cleanup.
5. Update the disabled list. Do not delete files.
6. Report the enabled count before and after, and state that the installed count is unchanged.
7. Re-read the config to confirm the change landed.

## What not to do

- Do not physically remove bundled skills by default.
- Do not claim a disabled skill lowered the installed count.
- Do not disable a skill the user actively relies on without checking.
- Do not delete user-authored skills.