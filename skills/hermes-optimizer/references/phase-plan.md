# Phase plan

The optimizer runs seven phases. Bring-up runs all of them. Tune mode runs a phase only when
the audit flagged it dirty.

Every phase has the same shape:

```text
PHASE N - <name>
  Questions  -> ask the user, 2-4 questions, concrete options
  Actions    -> do the work in the Desktop app, CLI only where needed
  Checkpoint -> show what changed, get a yes before moving on
```

Never skip the checkpoint. Never run two phases in one go on a fresh install.

---

## Phase 0 - Baseline audit

**Questions:** none. This phase is read-only, and asking the user to describe a state you can
read yourself wastes their time.

**Actions:**

1. Read the live config: main provider and model, `fallback_providers`, the `auxiliary` block,
   `plugins.enabled`, `tool_loop_guardrails`, `compression`, `context.engine`.
2. Count installed plugins and enabled skills.
3. Note which auxiliary slots are pinned to the main model (those are the expensive ones).
4. Check `hard_stop_enabled` and whether checkpoints are on.

**Checkpoint:** present the baseline report (shape in `audit-output.md`), including the honest
"starts thin" statement and the ranked list of what is worth fixing. Ask which phases to run.

---

## Phase 1 - Provider and main model

**Questions:**

- Which provider do you want as the main one? (list what is already authenticated first)
- Which model should be the default for real work?
- Reasoning level: do you want thinking on by default, or only when asked?
- Context length: do you need the large window, or is it wasted budget?

**Actions:**

1. Desktop route: **Settings -> Models**. Set provider and default model there.
2. If the provider needs a credential and none is configured, ask the user to paste the key
   into the provider field in the app, or tell them the `.env` variable name to fill. Never
   ask them to paste a secret into the chat.
3. Smoke-test the choice with one real call before declaring it set.
4. If the user has a second provider authenticated, note it now - phase 2 needs it.

**Checkpoint:** show the before/after provider and model, and the result of the smoke test.

---

## Phase 2 - Auxiliary and fallback routing

This is usually the highest-ROI phase. A fresh install often has every auxiliary slot on the
premium main model.

**Questions:**

- Cost posture: are you optimizing hard for spend, or for quality first?
- Is there a second provider you are happy to use for background work?
- Do you care about any specific auxiliary task being high quality (compression and
  web_extract are the two that users notice)?

**Actions:**

1. Enumerate what is actually reachable on each authenticated provider. Do not recommend a
   model you have not seen answer.
2. Assign lanes per the lane table in `SKILL.md`. Write the canonical shape from
   `auxiliary-config-shape.md`.
3. Build a fallback chain that does **not** land back on the same provider as the main route.
   A fallback that fails the same way as the primary is decoration.
4. If a user-supplied second provider has no live vision route, keep `vision` on the known-good
   provider rather than guessing.

**Checkpoint:** show the lane table with the model that will serve each slot, the fallback
chain, and which candidates you rejected and why.

---

## Phase 3 - Plugins

A fresh install has zero third-party plugins. That is the single biggest capability gap, and it
is invisible until the user tries to do something and the tool is not there.

**Questions:**

- What do you actually do all day with this agent? (academic work, code, office documents,
  research, automation, multi-agent runs)
- Which specific jobs hurt today?
- Which surfaces do you live in: Word/PowerPoint/Excel files, PDFs, long sessions, delegation
  to sub-agents?

Ask these as concrete options, not open prose. The answers select from
`plugin-baseline.md`.

**Actions:**

1. Desktop route: **Capabilities -> Plugins**. This is the catalog browser with install and
   enable switches.
2. Propose the baseline set matched to their answers. Name what each one buys them.
3. For each install, run the fit test first (see `plugin-baseline.md`). Skip anything that
   duplicates a live capability.
4. Install, then enable.
5. Verify the plugin's tools appear in the toolset list, and that its own settings page shows
   under **Settings -> Plugins** if it declares one.

**Checkpoint:** list installed plugins, the tools each one added, and anything you deliberately
skipped with the reason.

---

## Phase 4 - Local utilities and document tooling

**Questions:**

- Which document formats do you handle: born-digital PDFs, scans, Word, Markdown, spreadsheets?
- Are there scanned documents that need OCR?
- Does any of this need to stay on the machine, or is a hosted route acceptable?

**Actions:**

1. Establish the text-extraction layer first: PDF and Markdown to readable text.
2. Prefer what is already present. If a Python with PyMuPDF is installed, that covers
   born-digital PDF text extraction with no new heavy dependency.
3. Only reach for OCR when the user says there are scans. Run the silly-vs-smart test: a
   working vision route on the main provider already handles images, so a multi-gigabyte local
   OCR stack is justified only by scans at volume or by a local-only requirement.
4. Verify by extracting text from a real file the user names.

**Checkpoint:** show the extraction working on one real file, and state plainly which heavy
option you rejected and why.

---

## Phase 5 - Skills

**Questions:**

- Which domains are real for you? (list the enabled categories you can see)
- Anything in that list you have never used and do not intend to?

**Actions:**

1. Desktop route: **Capabilities -> Skills**, which lists skills with enable/disable.
2. Apply `skill-trim.md` rules. Disable, do not delete.
3. State the enabled-vs-installed distinction explicitly so the user is not surprised by the
   installed count.
4. Keep a provenance note so the trim is reversible.

**Checkpoint:** show enabled count before and after, plus the disable list.

---

## Phase 6 - Guardrails, checkpoints, safety

**Questions:**

- How much autonomous action do you want without being asked?
- Do you want a hard stop when the agent loops, or only a warning?

**Actions:**

1. Enable `tool_loop_guardrails.hard_stop_enabled` on any cost-sensitive install.
2. Enable checkpoints so risky file and config work can be rolled back.
3. Check `approvals.mode`. **An absent key is not a disabled setting** - the default is already
   `smart`, which asks on dangerous commands and passes safe ones. Only change it if the user
   explicitly wants a different posture.
4. Run the **health sweep** in `references/health-sweep.md`: `hermes doctor`, `hermes security
   audit`, `hermes sessions stats`, plugin contract checks, `hermes tools list`, `hermes lsp status`,
   `hermes memory status`. Report per bucket (agent-fixable / user-decision / upstream).

**Checkpoint:** show the guardrail settings and the health sweep findings, ranked, with the
practical risk stated - not just the severity labels.

---

## Phase 7 - Verification and handoff

**Questions:** none.

**Actions:**

1. Re-read the config file and confirm every intended change landed.
2. Smoke-test the main route and at least one fallback with real calls.
3. Confirm the Desktop pages show the new state - do not trust the YAML alone, the app can
   show stale values if the config was written in a shape it does not parse.
4. Produce a short before/after: main model, aux routing, plugins added, skills disabled,
   guardrails flipped, utilities installed.

**Checkpoint:** this is the final handoff. Offer to save the resulting routing as a note so the
next audit has a baseline.