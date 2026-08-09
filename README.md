<div align="center">

<img src="assets/hermes-optimizer-mark.svg" alt="Hermes Optimizer mark" width="92">

# ⚙️ Hermes Optimizer

![Hermes](https://img.shields.io/badge/Hermes-Agent-111827?style=for-the-badge&logo=sparkfun&logoColor=white)
![Optimizer](https://img.shields.io/badge/Optimizer-Provider_Agnostic-7c3aed?style=for-the-badge)
![Routing](https://img.shields.io/badge/Routing-Lane_Based-2563eb?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Public_Ready-22c55e?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge)

![Markdown](https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)
![SVG](https://img.shields.io/badge/SVG-FFB13B?style=for-the-badge&logo=svg&logoColor=black)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Shields.io](https://img.shields.io/badge/Shields.io-000000?style=for-the-badge&logo=shieldsdotio&logoColor=white)

**A reusable Hermes skill for cost-aware routing, safer defaults, automatic model discovery, skill cleanup, and live provider autodiscovery.**

<a href="#-quick-start">Quick Start</a> - <a href="#-zero-manual-optimization">Zero-Manual Optimization</a> - <a href="#-features">Features</a> - <a href="#-skill-package">Skill Package</a> - <a href="#-tech-stack">Tech Stack</a> - <a href="#-workflow">Workflow</a> - <a href="#-roadmap">Roadmap</a>

<img src="assets/hermes-optimizer-hero.svg" alt="Hermes Optimizer hero banner" width="100%">

</div>

> Hermes Optimizer does **not** assume your provider, your subscription, or your model catalog. It inspects the active Hermes setup, verifies live routes, and only then recommends changes.

> [!NOTE]
> Public examples in this repository are intentionally generic. The skill is designed to adapt to each user's real provider stack instead of pushing one person's model choices as a default.

---

## 💡 Concept

Most “optimizer” skills are just somebody else’s preset with new branding.

**Hermes Optimizer** is built around a stricter rule: **infer first, optimize second**.

It teaches Hermes to:

1. inspect the active configuration,
2. detect configured providers and authenticated subscriptions,
3. discover reachable models automatically,
4. verify candidate models with real calls,
5. split work into **heavy / medium / light / vision** lanes,
6. trim irrelevant skills safely,
7. apply only high-ROI changes,
8. verify the result before declaring success.

That makes it useful for:
- Azure-first users
- Ollama Cloud users
- mixed-provider users
- people with messy legacy configs
- anyone who says **“optimize my Hermes”** and expects the agent to figure it out

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **Provider autodiscovery** | Reads the live Hermes setup first instead of pushing a hardcoded model preset. |
| **Automatic model inventory** | Detects configured providers, authenticated subscriptions, and reachable models before asking the user anything. |
| **Lane-based routing** | Separates heavy, medium, light, and vision tasks by cost/capability profile. |
| **Live verification** | Rejects stale catalog entries, retired models, and broken routes before recommending them. |
| **Guardrail upgrades** | Pushes high-value defaults like hard stops, checkpoints, and safer approval modes. |
| **Platform-aware skill trim** | Targets irrelevant platform-specific skills, such as Apple/macOS-only defaults on Windows. |
| **Skill-bloat cleanup** | Disables noise safely instead of deleting bundled skills destructively. |
| **Fallback review** | Flags fake fallback chains that point back to the same weak path. |
| **Desktop-compatible aux config** | Writes auxiliary routing in the exact per-task config shape the Hermes Desktop Models page understands. |
| **GitHub-ready packaging** | Ships as a public repo, raw `SKILL.md`, and reusable reference pack. |

---

## 🚀 Quick Start

Install directly from GitHub:

```bash
hermes skills install https://raw.githubusercontent.com/$OWNER/$REPO/main/SKILL.md -y
```

Load it in Hermes:

```text
/skill hermes-optimizer
```

Or simply ask:

```text
Help me optimize Hermes.
```

The skill should inspect the live Hermes setup and discover available models automatically. The user should not need to type out their model list first.

<details>
<summary>⚙️ Advanced install and sharing modes</summary>

### Add the repository as a skill source

```bash
hermes skills tap add $OWNER/$REPO
```

### Share with a colleague

```bash
hermes skills install https://raw.githubusercontent.com/$OWNER/$REPO/main/SKILL.md -y
```

### Publication handoff

See [`PUBLISHING.md`](PUBLISHING.md) and [`RELEASE_COMMANDS.md`](RELEASE_COMMANDS.md).

</details>

---

## 🪄 Zero-Manual Optimization

The intended user experience is simple:

```text
Optimize my Hermes.
```

Then the skill should do the discovery work itself:

1. inspect the active `config.yaml`
2. detect configured providers and authenticated subscriptions
3. inspect fallback and auxiliary routes already present
4. enumerate reachable models when Hermes exposes provider/model commands
5. probe likely candidates with real one-shot calls when enumeration is unavailable
6. build heavy / medium / light / vision recommendations from the models that are actually reachable

### What the user should *not* have to do

- manually list every model they think they have
- guess which provider is supplying which route
- remember old screenshots or stale catalog snapshots
- pre-sort models into “cheap” and “strong” buckets by hand
- invent custom auxiliary YAML shapes that Hermes Desktop does not understand

### What the skill should return

| Output | Meaning |
|--------|---------|
| **Detected environment** | Which providers, subscriptions, and routes Hermes can actually see |
| **Reachable models** | Which candidate models really answered live checks |
| **Lane mapping** | Which models fit heavy / medium / light / vision work |
| **Weak points** | Bad fallback chains, main-model overuse, stale config, or skill bloat |

### Desktop-compatible auxiliary format

When the skill writes auxiliary routing, it should use the exact nested task format Hermes Desktop understands:

```yaml
auxiliary:
  vision:
    provider: azure-foundry
    model: gpt-5.4
  web_extract:
    provider: ollama-cloud
    model: kimi-k2.5
  compression:
    provider: ollama-cloud
    model: kimi-k2.5
  approval:
    provider: ollama-cloud
    model: deepseek-v4-flash
```

Use canonical task keys such as `vision`, `web_extract`, `compression`, `skills_hub`, `approval`, `mcp`, `title_generation`, `triage_specifier`, `kanban_decomposer`, `profile_describer`, and `curator`.
Do **not** invent umbrella keys like `heavy`, `medium`, or `light` under `auxiliary` if the goal is for the Desktop Models page to reflect the change.

For custom/local endpoints, the task itself must carry `base_url` and (if needed) `api_key`.

---

## 🧩 Skill Package

| File | Purpose |
|------|---------|
| `SKILL.md` | Runtime Hermes skill with the optimization workflow and decision rules. |
| `references/provider-autodiscovery.md` | Rules for detecting the real provider stack before changing anything. |
| `references/platform-skill-trim.md` | Platform-aware disable guidance for irrelevant built-in skills, plus enabled-vs-installed cleanup rules. |
| `notes/current-routing.md` | Generic provider-profile examples rather than personal defaults. |
| `assets/hermes-optimizer-hero.svg` | README hero banner for GitHub presentation. |
| `assets/architecture-flow.svg` | Visual workflow diagram for the optimization flow. |
| `llms.txt` / `llms-full.txt` | Machine-readable project summary for LLM-assisted discovery. |

---

## 🧠 At a Glance

| Signal | Value |
|--------|-------|
| **Primary purpose** | Reduce Hermes spend and routing mistakes without hardcoding one stack |
| **Best for** | Users with Azure, Ollama Cloud, mixed-provider, or evolving model catalogs |
| **Core mechanism** | Provider autodiscovery → automatic model inventory → live verification → lane-based routing |
| **Cleanup rule** | Prefer `skills.disabled` over deleting bundled files |
| **Main promise** | Safer recommendations backed by live checks |

---

## 🏗️ Tech Stack

| Layer | Technology |
|-------|------------|
| Agent runtime | Hermes Agent |
| Skill format | Markdown + YAML frontmatter |
| Reference docs | Markdown |
| Visual assets | SVG + PNG |
| Distribution | GitHub raw install + skill tap |
| Quality gate | GitHub Actions README/link checks |

<details>
<summary>📁 Project Structure</summary>

```text
hermes-optimizer/
├── README.md
├── SKILL.md
├── LICENSE
├── CHANGELOG.md
├── KANBAN.md
├── PUBLISHING.md
├── RELEASE_COMMANDS.md
├── llms.txt
├── llms-full.txt
├── .github/
│   └── workflows/
│       └── repo-readme-quality.yml
├── assets/
│   ├── hermes-optimizer-mark.svg
│   ├── hermes-optimizer-hero.svg
│   ├── architecture-flow.svg
│   └── social-preview.png
├── references/
│   ├── provider-autodiscovery.md
│   ├── platform-skill-trim.md
│   ├── imported-skills.md
│   └── synthesis-map.md
└── notes/
    └── current-routing.md
```

</details>

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
 Trim irrelevant skills by platform
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

## 📊 What It Upgrades

| Area | Typical improvement |
|------|---------------------|
| **Auxiliary routing** | Moves non-critical work off the premium main model when cheaper verified routes exist. |
| **Fallback chains** | Exposes fake fallback paths and pushes for genuinely separate backup routes. |
| **Guardrails** | Enables loop protection and safer autonomous behavior on cost-sensitive setups. |
| **Checkpoints** | Makes risky file/config work easier to roll back. |
| **Skill overhead** | Reduces enabled-skill prompt bloat and accidental irrelevant skill activation. |
| **Platform mismatch** | Disables obviously irrelevant skills like Apple/macOS defaults on Windows. |
| **Config hygiene** | Surfaces version drift and deprecated keys before they become breakage. |

> [!IMPORTANT]
> Skill cleanup is **disable-first**. Updating `skills.disabled` lowers the number of **enabled** skills, but it does **not** automatically lower the number of **installed** skills shown by `hermes skills list`. Installed-count reduction is a separate uninstall/delete step.

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

## 🛡️ Quality Checks Included

- README/link lint workflow on push
- generic public examples instead of personal stack defaults
- structured visual assets for GitHub rendering
- reusable machine-readable project summary for LLM tools

---

## 🗺️ Roadmap

- [x] Public GitHub repository
- [x] Provider-agnostic optimization logic
- [x] Provider autodiscovery design
- [x] Platform-aware skill trim rules
- [x] README hero + architecture visuals
- [x] LLM discovery files
- [x] README quality workflow
- [ ] Registry / hub listing
- [ ] Additional provider-profile examples
- [ ] Installation validation from a clean external machine

---

## 🤝 Contributing

Fork → branch → improve docs / references / workflow → PR.

Good contribution areas:
- clearer provider-profile examples
- safer skill-trim heuristics
- better fallback verification recipes
- more validation coverage for public installation flows

---

## 📄 License

MIT. See [LICENSE](LICENSE).

---

<div align="center">

**Built for the open-source community**

</div>
