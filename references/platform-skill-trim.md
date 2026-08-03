# Platform-aware skill trim

`hermes-optimizer` should not only optimize models and guardrails. It should also reduce **skill-layer noise**.

## Core principle

Disable irrelevant skills instead of deleting them by default.

That preserves reversibility, avoids damaging bundled installs, and still removes prompt overhead and accidental false triggers.

Important distinction:
- updating `skills.disabled` reduces the number of **enabled** skills
- it does **not** reduce the number of **installed** skills shown by `hermes skills list`
- if the user wants the installed count itself to go down, that is a separate uninstall/delete step

## Windows-first safe trim

On a Windows machine, these are safe **default disable candidates** unless the user explicitly says they need them:

### Apple / macOS-only
- `apple-notes`
- `apple-reminders`
- `findmy`
- `imessage`
- `macos-computer-use`

### Clearly irrelevant leisure / side integrations
- `minecraft-modpack-server`
- `pokemon-player`
- `spotify`
- `openhue`
- `himalaya`

### Narrow or risky specialist skills unless explicitly needed
- `godmode`
- `dogfood`
- `yuanbao`
- `dermatology-trichology-reviews`

## Imported source-skills cleanup

If optimization source-skills were installed only as research inputs for building a better umbrella skill, they should usually be disabled after synthesis.

Safe examples:
- `token-budget-guard`
- `cost-control`
- `context-compression`
- `prompt-caching`
- `token-budget-advisor`
- `cost-aware-llm-pipeline`
- `token-optimization`

## Workflow

1. detect host platform
2. inspect `skills.disabled`
3. identify safe irrelevant candidates
4. propose the trim list to the user when the request is narrow
5. apply directly when the user asks for broad Hermes optimization / cleanup
6. prefer `disabled` list updates over file deletion
7. tell the user explicitly when a requested installed-count reduction needs manual uninstall / separate removal
8. verify config after changes

## What not to do

- do not physically remove bundled skills from the Hermes install by default
- do not claim that disabling skills lowered the installed skill count
- do not delete user-authored skills just because they are idle
- do not disable a skill the user actively relies on without checking recent relevance
