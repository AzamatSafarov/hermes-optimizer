# Run visualization

Numbers in a paragraph are forgettable; a before/after chart and a phase timeline are not. Emit
them as part of the handoff.

## What to build

Two widgets per run, self-contained HTML in the scratch directory, embedded with
`::preview{file="..."}` on its own line. See `interactive-chat-widgets` for the frame rules.

### 1. Before/after chart

One row per metric. Two bars: "before" in `var(--border)`, "after" in `var(--accent)`, plus a
delta label colored by direction. Include the flat rows - a row where nothing changed is
information, not noise.

Metrics worth plotting:

- security findings by severity (HIGH, total)
- loop hard stop, checkpoints, force_ipv4 (off -> on)
- auxiliary slots pinned to the main model (should be 0)
- skills enabled vs installed
- fallback on a distinct route (yes/no)
- vision route verified (yes/no)

### 2. Phase timeline

One node per phase, three states: **done** (green), **found, not changed** (amber), **not run**
(grey). Each node carries one line on what happened.

Statuses that lie are worse than no widget. "Not run" and "found, not changed" must look distinct
from "done", or the chart overstates the work.

### 3. Relationship graph (when routing changed)

Show the topology: main provider -> auxiliary slots -> models, with split-off slots highlighted
and any slot still sharing the main provider called out. This is the picture that makes lane
routing legible.

## Rules

1. **Build from verified numbers only.** Every value comes from a command run this session -
   `hermes config get`, `hermes security audit`, a live model call. A chart drawn from remembered
   numbers is a fabrication with a picture around it.
2. **Never paint everything green.** Include what did not improve and what was deferred. A chart
   of wins only hides the next action.
3. **Style from the frame's CSS variables** - `--foreground`, `--muted-foreground`, `--accent`,
   `--border`, `--card`. No own background, font, or centering wrapper. Content flush left.
4. **No CDN.** Plain HTML/CSS with inline bars; inline SVG is fine. A chart that fails to render in
   the frame is worse than a table.
5. **Save the run** to `~/.hermes/optimizer-runs/<YYYY-MM-DD>/` so successive runs can be compared
   and the history is auditable.

## Placement

Widgets belong to **Phase 7**, after the config re-read and the smoke tests, so the numbers are
final. Do not build them mid-run - a chart built before verification shows intended state, not
actual state.
