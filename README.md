<div align="center">

# ⚙️ Hermes Optimizer

![Hermes](https://img.shields.io/badge/Hermes-Skill-7c3aed?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Public_Ready-22c55e?style=for-the-badge)
![Autodiscovery](https://img.shields.io/badge/Autodiscovery-Live_First-f59e0b?style=for-the-badge)
![Routing](https://img.shields.io/badge/Routing-Lane_Based-2563eb?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-e5e7eb?style=for-the-badge)

![Markdown](https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)
![SVG](https://img.shields.io/badge/SVG-FFB13B?style=for-the-badge&logo=svg&logoColor=black)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Shields.io](https://img.shields.io/badge/Shields.io-000000?style=for-the-badge&logo=shieldsdotio&logoColor=white)

**Provider-agnostic Hermes Agent optimization skill with autodiscovery, skill cleanup, and safer routing defaults.**

[Quick Start](#-quick-start) • [Features](#-features) • [Workflow](#-workflow) • [Structure](#-project-structure) • [Roadmap](#-roadmap)

<img src="assets/hermes-optimizer-hero.svg" alt="Hermes Optimizer hero banner" width="100%">

</div>

> Hermes Optimizer does **not** assume your provider, your subscription, or your model catalog. It inspects the active Hermes setup first, verifies real routes, and only then proposes cost and routing changes.

---

## 💡 Concept

Most "optimizer" skills are just somebody else's preset with new branding.

**Hermes Optimizer** is built around a stricter rule: **infer first, optimize second**.

It teaches Hermes to:

1. inspect the active configuration,
2. infer the user's provider profile,
3. verify real candidate models with live checks,
4. separate work into **heavy / medium / light / vision** lanes,
5. trim irrelevant skills safely,
6. apply only high-ROI changes.

That makes it useful for:
- Azure-first users
- Ollama Cloud users
- mixed-provider users
- anyone who says **"optimize my Hermes"** and expects the agent to figure out the rest

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **Provider autodiscovery** | Reads the live Hermes setup first instead of pushing a hardcoded model preset. |
| **Lane-based routing** | Separates heavy, medium, light, and vision tasks by cost/capability profile. |
| **Live verification** | Rejects stale catalog entries, retired models, and broken routes before recommending them. |
| **Guardrail upgrades** | Pushes high-value defaults like hard stops, checkpoints, and safer approval modes. |
| **Platform-aware skill trim** | Targets irrelevant platform-specific skills, such as Apple/macOS-only defaults on Windows. |
| **Skill-bloat cleanup** | Disables noise safely instead of deleting bundled skills destructively. |
| **Provenance-aware design** | Documents which upstream optimization ideas informed the final skill. |
| **GitHub-ready distribution** | Can be shared as a public repo, raw `SKILL.md`, or future ecosystem/hub entry. |

---

## 🚀 Quick Start

Install directly from GitHub:

```bash
hermes skills install https://raw.githubusercontent.com/AzamatSafarov/hermes-optimizer/main/SKILL.md -y
```

Load it in Hermes:

```text
/skill hermes-optimizer
```

Or simply ask:

```text
Help me optimize Hermes.
```

<details>
<summary>Advanced install / sharing modes</summary>

### Add the repository as a skill source

```bash
hermes skills tap add AzamatSafarov/hermes-optimizer
```

### Share with a colleague

```bash
hermes skills install https://raw.githubusercontent.com/AzamatSafarov/hermes-optimizer/main/SKILL.md -y
```

### Move toward ecosystem publication

See [`PUBLISHING.md`](PUBLISHING.md).

</details>

---

## 🧠 At a Glance

| Signal | Value |
|--------|-------|
| **Primary purpose** | Reduce Hermes spend and routing mistakes without blindly hardcoding one stack |
| **Best for** | Users with Azure, Ollama Cloud, mixed-provider, or evolving model catalogs |
| **Core mechanism** | Provider autodiscovery → live verification → lane-based routing |
| **Skill cleanup rule** | Prefer `skills.disabled` updates over deleting bundled files |
| **Output** | Safer config recommendations, better auxiliary routing, lower prompt waste |

---

## 🧭 Workflow

<img src="assets/architecture-flow.svg" alt="Hermes Optimizer architecture flow" width="100%">

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

### Lane model

| Lane | Typical tasks |
|------|---------------|
| **Heavy** | `compression`, `web_extract`, `kanban_decomposer`, `curator` |
| **Medium** | `approval`, `triage_specifier`, `profile_describer` |
| **Light** | `session_search`, `skills_hub`, `mcp`, `title_generation` |
| **Vision** | Only when a verified live multimodal route exists |

---

## 📊 What Improves

| Area | Typical improvement |
|------|---------------------|
| **Auxiliary routing** | Moves non-critical work off the premium main model when cheaper verified routes exist. |
| **Fallback chains** | Adds real backup routes so the agent degrades gracefully instead of stalling. |
| **Guardrails** | Enables loop protection and safer autonomous behavior on cost-sensitive setups. |
| **Checkpoints** | Makes risky file/config work easier to roll back. |
| **Skill overhead** | Reduces prompt bloat and accidental irrelevant skill activation. |
| **Platform mismatch** | Disables obviously irrelevant skills like Apple/macOS defaults on Windows. |

---

## 🧪 Example Output Shape

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
- premium main capacity stays reserved for the work that truly needs it
- internal routing/search moves to cheaper lanes
- broken or retired candidates are filtered before config changes
- users get provider-specific recommendations without a hardcoded preset

---

## 🏗️ Project Structure

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
    └── 📄 current-routing.md            ← generic provider-profile examples
```

---

## 🔗 Sources and Attribution

This repository is **not** a blind copy of one upstream skill.
It is a reviewed synthesis built from several optimization-oriented ideas, including work on:

- token budgeting
- prompt caching
- cost-aware routing
- context compression
- token-efficient tool usage
- runaway spend protection
- platform-aware skill cleanup

Useful references:
- [`references/imported-skills.md`](references/imported-skills.md)
- [`references/synthesis-map.md`](references/synthesis-map.md)
- [`references/provider-autodiscovery.md`](references/provider-autodiscovery.md)
- [`references/platform-skill-trim.md`](references/platform-skill-trim.md)

---

## 🗺️ Roadmap

- [x] Public GitHub repository
- [x] Provider-agnostic optimization logic
- [x] Provider autodiscovery design
- [x] Platform-aware skill trim rules
- [x] Provenance documentation
- [x] Shareable raw install flow
- [ ] Registry / hub listing
- [ ] Additional provider-profile examples
- [ ] Installation validation from a clean external machine

---

## 🤝 Contributing

Fork → branch → improve docs / references / workflow → PR.

Good contribution areas:
- new provider-profile examples
- safer skill-trim heuristics
- clearer verification recipes
- ecosystem publication improvements

---

## 📄 License

MIT. See [LICENSE](LICENSE).

---

<div align="center">

**Built by [Azamat Safarov](https://github.com/AzamatSafarov)**

</div>
