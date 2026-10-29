# Desktop surfaces

The optimizer drives the Desktop app first. These are the real routes, verified against the
app source. Use them instead of sending the user to a terminal.

## Settings -> Models

Route: `/settings?tab=config:model`

Owns the main provider and default model. Also where a custom endpoint gets declared.

Use in **Phase 1**.

## Settings -> Auxiliary rows / fallback models

Route: `/settings` (sidebar)

The auxiliary task rows and the fallback-model field live in Settings, not on the Models page.
Each row is one auxiliary slot. Use it to point a slot at a different provider or model.

The fallback-model field is where the chain that is tried when the primary fails gets edited.

Use in **Phase 2**.

Note: the Model page is the *main* route. Auxiliary and fallback are separate rows. Users often
edit the main model and assume auxiliary moved with it. It did not.

## Capabilities -> Plugins

Route: `/capabilities?tab=plugins`

The catalog browser. Install, enable, and disable plugins here. Rows show the plugin, its state,
and whether it exposes a settings page.

A deep link to one plugin is `/capabilities?tab=plugins&plugin=<name>`, where `<name>` may be the
agent key, the plugin name, or the desktop record id.

Use in **Phase 3**.

## Settings -> Plugins

Route: `/settings` -> Plugins

Each plugin that declares its own settings page contributes a page here. This is where plugin
config lives, not in the catalog browser. After installing a plugin with settings, check here to
confirm its page appeared.

Use in **Phase 3**, during verification.

## Capabilities -> Skills

Route: `/capabilities?tab=skills`

The installed skill list with enable and disable controls. A skill detail view shows provenance.

Use in **Phase 5**.

## Capabilities -> Connectors / MCP

Route: `/capabilities?tab=connectors`

MCP servers and connector state. Relevant if the user wants a server-backed capability and the
install has no MCP server configured yet.

Use in **Phase 3** when the user's need maps to an MCP server rather than a plugin.

## What the app does not cover

Some work has no Desktop field and belongs to the CLI. Use these, and tell the user what you are
running and why:

| Need | Command |
|------|---------|
| Read the whole live config | read `config.yaml` directly |
| Confirm the fallback chain | `hermes fallback list` |
| Confirm the plugin list and state | `hermes plugins list` |
| Search the plugin catalog as text | `hermes plugins search <term>` |
| Validate a plugin against real runtime contracts | `hermes plugins doctor <name>` |
| Show declared vs granted capabilities for a plugin | `hermes plugins capabilities <name>` |
| Check for plugin updates | `hermes plugins check-updates` |
| Search skill registries | `hermes skills search <query>` |
| Confirm installed vs enabled skills | `hermes skills list` |
| Diagnose the setup | `hermes doctor` |
| Supply-chain audit | `hermes security audit` |
| Show enabled tools per platform | `hermes tools list` |
| External memory provider state | `hermes memory status` |

## Rule

If the app owns the surface, drive the app. If it does not, run the command and say what it is
for. Never tell the user "just run this in your terminal" when the app has the field.