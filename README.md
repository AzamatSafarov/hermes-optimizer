<div align="center">
  <img src="assets/hermes-optimizer-hero.svg" alt="Hermes Optimizer hero banner" width="100%">

# Hermes Optimizer

[![Hermes Skill](https://img.shields.io/badge/Hermes-Skill-7c3aed?style=flat-square)](#)
[![Status](https://img.shields.io/badge/status-public%20ready-22c55e?style=flat-square)](#)
[![Autodiscovery](https://img.shields.io/badge/autodiscovery-live%20first-f59e0b?style=flat-square)](#)
[![Routing](https://img.shields.io/badge/routing-heavy%20%7C%20medium%20%7C%20light-2563eb?style=flat-square)](#)
[![License](https://img.shields.io/badge/license-MIT-e5e7eb?style=flat-square)](LICENSE)

> *A reusable Hermes skill for discovering the user's stack and cutting wasted spend safely.*

[Quick Start](#quick-start) • [Why Hermes Optimizer](#why-hermes-optimizer) • [Feature Highlights](#feature-highlights) • [Optimization Workflow](#optimization-workflow) • [Project Structure](#project-structure) • [Roadmap](#roadmap)

</div>

---

## Why Hermes Optimizer

Most "optimizer" skills are secretly somebody else's preset.

**Hermes Optimizer** is built around a stricter rule: **infer first, optimize second**. Instead of assuming one provider, one subscription, one model catalog, or one personal setup, it teaches Hermes to:

1. inspect the active Hermes configuration,
2. infer the user's current provider profile,
3. verify real model availability with live checks,
4. split work into heavy / medium / light / vision lanes,
5. apply only safe high-ROI configuration changes.

That makes it useful for:
- Azure-first users
- Ollama Cloud users
- mixed-provider users
- people who just say **"optimize my Hermes"** and expect the skill to figure out the rest

---

## At a Glance

| Signal | Value |
|---|---|
| **Primary purpose** | Reduce Hermes spend and routing mistakes without blindly hardcoding one stack |
| **Best for** | Users with Azure, Ollama Cloud, mixed-provider, or evolving model catalogs |
| **Core mechanism** | Provider autodiscovery → live verification → lane-based routing |
| **Output** | Safer config recommendations, better auxiliary routing, lower prompt waste |
| **Distribution** | Public GitHub repo, raw `SKILL.md`, future ecosystem/hub listing |

---

## Quick Start

Install directly from GitHub:

```bash
hermes skills install https://raw.githubusercontent.com/AzamatSafarov/hermes-optimizer/main/SKILL.md -y
```

Or add the repository as a reusable skill source:

```bash
hermes skills tap add AzamatSafarov/hermes-optimizer
```

Then load it in Hermes:

```text
/skill hermes-optimizer
```

Or simply ask:

```text
Help me optimize Hermes.
```

---

## Feature Highlights

| Feature | Description |
|---|---|
| **Provider autodiscovery** | Reads the live Hermes setup first instead of pushing a hardcoded model preset. |
| **Lane-based routing** | Separates heavy, medium, light, and vision tasks by cost/capability profile. |
| **Live verification** | Rejects stale catalog entries, retired models, and broken routes before recommending them. |
| **Guardrail upgrades** | Pushes high-value defaults like loop hard-stops, checkpoints, and safer approval modes. |
| **Platform-aware skill trim** | Auto-targets irrelevant platform-specific skills like Apple/macOS-only defaults on Windows. |
| **Skill-bloat cleanup** | Helps reduce unnecessary enabled skills and prompt overhead without destructive pruning. |
| **Provenance-aware design** | Documents which upstream optimization skills informed this one and what was intentionally rejected. |
| **GitHub-ready distribution** | Can be shared as a public repo, raw `SKILL.md`, or future ecosystem/hub entry. |

---

## Optimization Workflow

<div align="center">
  <img src="assets/architecture-flow.svg" alt="Hermes Optimizer architecture flow" width="100%">
</div>

```text
User asks to optimize Hermes
            │
            ▼
   Detect active provider stack
            │
            ▼
   Inspect main / fallback / auxiliary
            │
            ▼
    Test real candidate models live
            │
            ▼
 Map candidates into heavy / medium /
      light / vision task lanes
            │
            ▼
 Apply only safe high-ROI changes
            │
            ▼
  Verify config + runtime behavior
```

### Heavy lane
Used for:
- `compression`
- `web_extract`
- `kanban_decomposer`
- `curator`

### Medium lane
Used for:
- `approval`
- `triage_specifier`
- `profile_describer`

### Light lane
Used for:
- `session_search`
- `skills_hub`
- `mcp`
- `title_generation`

### Vision lane
Used only when a **verified live multimodal route** exists.

---

## What Improves

| Area | Typical improvement |
|---|---|
| **Auxiliary routing** | Moves non-critical work off the premium main model when cheaper verified routes exist. |
| **Fallback chains** | Adds real backup routes so the agent degrades gracefully instead of stalling. |
| **Guardrails** | Enables loop protection and safer autonomous behavior on cost-sensitive setups. |
| **Checkpoints** | Makes risky file/config work easier to roll back. |
| **Skill overhead** | Reduces prompt bloat and accidental irrelevant skill activation. |

---

## Example Optimization Output

### Detected environment
```text
Main: premium provider / premium general model
Fallback: cheaper secondary provider / stable backup model
Aux: heavy tasks still pinned to the premium main lane
Vision: verified only on the main provider
```

### Recommended mapping
```text
Heavy lane   → strongest verified long-context budget model
Medium lane  → fast low-cost reasoning/classifier model
Light lane   → cheapest stable text model with enough context
Vision lane  → keep on the verified multimodal provider
```

### Why this is better
- premium main model stays reserved for the work that truly needs it
- internal routing/search moves to cheaper lanes
- broken or retired model candidates are filtered before config changes
- the user gets a provider-specific recommendation without a hardcoded preset

---

## Project Structure

```text
📁 hermes-optimizer/
├── 📖 README.md                         ← public project overview
├── 🧠 SKILL.md                          ← publishable Hermes skill
├── 📜 LICENSE                           ← MIT license
├── 🗂️ CHANGELOG.md                      ← release history
├── 📋 KANBAN.md                         ← publication work board
├── 🚀 PUBLISHING.md                     ← GitHub + ecosystem publication flow
├── 🛠️ RELEASE_COMMANDS.md               ← exact push / publish commands
├── 🎨 assets/
│   ├── 🖼️ hermes-optimizer-mark.svg     ← repo icon mark
│   ├── 🖼️ hermes-optimizer-hero.svg     ← hero banner
│   ├── 🖼️ architecture-flow.svg         ← workflow diagram
│   └── 🖼️ social-preview.png            ← shareable preview image
├── 📁 references/
│   ├── 📄 imported-skills.md            ← upstream skills used as inputs
│   ├── 📄 provider-autodiscovery.md     ← autodiscovery design rules
│   ├── 📄 platform-skill-trim.md        ← safe default disable candidates by platform
│   └── 📄 synthesis-map.md              ← why this is a synthesis, not a clone
└── 📁 notes/
    └── 📄 current-routing.md            ← one example routing snapshot, not a default
```

---

## Sources and Attribution

This repository is **not** a blind copy of one upstream skill.
It is a reviewed synthesis built from several optimization-oriented skill ideas, including work on:

- token budgeting
- prompt caching
- cost-aware routing
- context compression
- token-efficient tool usage
- runaway spend protection

See:
- [`references/imported-skills.md`](references/imported-skills.md)
- [`references/synthesis-map.md`](references/synthesis-map.md)
- [`references/provider-autodiscovery.md`](references/provider-autodiscovery.md)
- [`references/platform-skill-trim.md`](references/platform-skill-trim.md)

---

## Reuse and Distribution

Share with one colleague:

```bash
hermes skills install https://raw.githubusercontent.com/AzamatSafarov/hermes-optimizer/main/SKILL.md -y
```

Share as a reusable source:

```bash
hermes skills tap add AzamatSafarov/hermes-optimizer
```

Move toward ecosystem publication:

See [`PUBLISHING.md`](PUBLISHING.md).

> **Note**
> A ready-to-upload social preview image is included at `assets/social-preview.png` for manual use in GitHub repository settings.

---

## Roadmap

- [x] Public GitHub repository
- [x] Provider-agnostic optimization logic
- [x] Provider autodiscovery design
- [x] Provenance documentation
- [x] Shareable raw install flow
- [ ] Registry / hub listing
- [ ] Additional provider-profile examples
- [ ] Installation validation from a clean external machine

---

<div align="center">

**Built by [Azamat Safarov](https://github.com/AzamatSafarov)**

</div>
