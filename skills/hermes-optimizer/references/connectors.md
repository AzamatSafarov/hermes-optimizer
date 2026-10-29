# Connectors and automation surfaces

A capability gap users cannot see. Ask about it explicitly, every run.

## Why this needs its own step

"Do you need integrations?" produces a shrug. "GitHub, Vercel, Supabase, Slack, Jira, Notion,
Linear, Sentry, Postgres, Figma - which do you touch weekly?" produces an answer. Most people do
not know what is connectable, so name the options.

## Read the live state first

1. Config: the `mcp_servers` block. Each entry may have `url` (hosted) or `command` (local), and a
   per-server `enabled` flag. Present but `enabled: false` is a decided-off connector, not a
   missing one.
2. Desktop: **Capabilities -> Connectors** (`/capabilities?tab=connectors`) shows connected and
   available connectors, with an add dialog.
3. A server in the config whose toolset never loads is a **broken** connector, not a connected
   one. Check `hermes tools list` before counting it.

## The connector question

Ask as a grouped multi-select, not prose. Groups:

**Code and deploy** - GitHub, GitLab, Bitbucket, Vercel, Netlify, Cloudflare, Railway

**Data and backend** - Supabase, PostgreSQL, SQLite, Redis, MongoDB

**Work tracking** - Jira, Linear, Notion, Asana, Trello, a kanban board

**Comms** - Slack, Discord, Telegram, WhatsApp, Gmail

**Observability** - Sentry, Grafana, Datadog

**Design and content** - Figma, Google Drive, Dropbox

Plus the generic ones most hosts want: filesystem, fetch/web, browser automation (Playwright or
Puppeteer), memory.

Present the groups, let the user pick, then install and verify each pick. Never install a
connector the user did not choose.

## Automation surfaces

Separate question, same run. These change how much the install can do unattended.

| Surface | What it buys | Ask |
|---|---|---|
| Chat platform | Reach the agent from a phone; run away from the desktop | "Telegram, Slack, Discord, or WhatsApp?" |
| Task board | Multi-step work with visible state; survives a session | "Want a kanban board for multi-step jobs?" |
| Cron | Recurring jobs: digests, monitors, scheduled reports | "Anything you want run on a schedule?" |
| Webhooks | Inbound events wake the agent | "Anything that should trigger the agent from outside?" |

A user who has never used cron will not ask for it. Name the use case, not the mechanism: "a daily
digest of X", "a monitor that pings you when Y changes".

## Verification after adding

1. The connector's tools appear in `hermes tools list`.
2. Its credentials resolve - a hosted MCP server needing OAuth prompts once.
3. One real call through the connector succeeds. Installed but never answered is not connected.

## Reporting

```text
CONNECTORS
  already connected  : supabase, vercel
  added              : github, jira
  offered, declined  : slack, sentry, figma
  broken             : figma (configured, toolset never loaded)

AUTOMATION SURFACES
  chat platform : telegram - added
  task board    : kanban - already present
  cron          : declined
  webhooks      : already enabled
```

Naming what was *offered and declined* matters as much as what was added: it tells the user the
option exists so they can return to it.
