# Publishing to the skills.sh index

How a Hermes skill becomes discoverable by name search. Verified end to end against a real repo,
not inferred from docs.

## The mechanism

skills.sh (operated by Vercel) is the open directory that Hermes' `skills-sh` source reads. It
does **not** crawl arbitrary GitHub repos on a schedule. From their own About page:

> "We index every public skill that ships through the open skills CLI"

So ingestion is driven by installs through **their** CLI (`npx skills add owner/repo`), plus
anonymous install telemetry. Two separate things: a repo page, and an Algolia search entry.

## What actually gates discovery

| Thing | Gated by | Latency |
|---|---|---|
| Repo page renders real content | repo layout being conventional | minutes |
| Install via `npx skills add owner/repo` | repo layout being conventional | immediate |
| Install via Hermes `skills-sh/owner/repo/skill` id | repo layout | immediate |
| **Search by name** | **Algolia index rebuild** | **not under your control** |

Search-by-name is the last one to arrive and you cannot force it. Say that plainly instead of
promising a timeline.

## Required repo layout

skills.sh and the default Hermes tap both expect the skill in a **subdirectory named after the
skill**, not at the repo root:

```text
repo/
├── README.md
├── skills.sh.json          <- repo-page grouping sidecar
└── skills/
    └── <skill-name>/
        ├── SKILL.md
        ├── references/
        └── notes/
```

`hermes skills tap add owner/repo` registers path `skills/` by default. A root-level `SKILL.md`
will not be found by that tap, even though a direct raw-URL install still works.

## The skills.sh.json sidecar

Place at the **repo root**. It controls how the repo page groups skills. Without it the page still
works; with it the skill lands in a labelled section.

```json
{
  "$schema": "https://skills.sh/schemas/skills.sh.schema.json",
  "notGrouped": "bottom",
  "groupings": [
    { "title": "Section title", "description": "One sentence.", "skills": ["skill-name"] }
  ]
}
```

Schema: `https://skills.sh/schemas/skills.sh.schema.json`. `groupings` is required.

## Verification recipe

Do not trust an HTTP 200 as proof the page is live. **A nonexistent skills.sh path also returns
200** and serves an error shell. Check for real content instead.

```bash
# 1. Repo page: must mention the skill AND the npx command
curl -sL "https://skills.sh/owner/repo" | grep -oE "npx skills add|owner/repo"

# 2. Control test: a fake path returns 200 too, so 200 alone proves nothing
curl -sL -o /dev/null -w "%{http_code}\n" "https://skills.sh/fake/fake/fake"

# 3. The actual CLI path a user takes
npx -y skills add owner/repo

# 4. Hermes hub path
hermes skills inspect "skills-sh/owner/repo/skill-name"
hermes skills install "skills-sh/owner/repo/skill-name" -y --force

# 5. The search index states
curl -sL "https://skills.sh/api/search?q=skill-name"
```

## Pitfalls

1. **HTTP 200 is not liveness.** skills.sh serves 200 for unknown paths. Verify content.
2. **Search is by skill name and description only.** The GitHub owner/author is not indexed, so
   "download X by <author>" will not resolve by the author's name.
3. **Root-level `SKILL.md` breaks the default tap.** Move to `skills/<name>/`.
4. **Install-by-identifier can succeed while search still misses it.** Hermes' skills.sh source
   falls back to a direct GitHub fetch when the detail page is empty, so a working install is not
   evidence the index has caught up.
5. **Install counts dedupe hourly** and need opt-in telemetry, so a fresh install reads 0.
6. **The `official` tier is not self-serve.** It is only what Nous Research places in its own
   repo. Publishing to skills.sh/ClawHub does not confer it.

## Watch for the index

A daily monitor that only wakes the agent on change avoids burning a call per day:

```
monitor: script or URL whose output changes only when the index updates
schedule: every day at 10am
```

Script form: print a stable line when not yet indexed, and something different the day it lands.
