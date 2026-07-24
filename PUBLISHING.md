# Publishing Hermes Optimizer

This file explains how to turn this repo into a public GitHub project and then publish the skill into the Hermes Ecosystem.

## Publication philosophy

`hermes-optimizer` should be published as:
- a **universal optimization skill** in `SKILL.md`
- with **provider autodiscovery** as a core behavior
- with **provider-specific examples** in references/notes
- with **provenance documentation** proving how it was synthesized

Do **not** present this project as a raw mirror of upstream skills.
Present it as:
- a reviewed synthesis
- adapted for Hermes Agent
- validated with live config and model tests

## 1. GitHub publication

### Recommended repo contents

At minimum:
- `README.md`
- `SKILL.md`
- `LICENSE`
- `CHANGELOG.md`
- `KANBAN.md`
- `references/imported-skills.md`
- `references/synthesis-map.md`
- `references/provider-autodiscovery.md`
- `PUBLISHING.md`

### GitHub preflight checklist

Before pushing:
- [ ] `SKILL.md` has valid frontmatter
- [ ] description is trigger-focused and not user-specific
- [ ] provider autodiscovery is described in the core skill
- [ ] provider-specific mappings live only in notes/references
- [ ] provenance docs explain which upstream skills were used
- [ ] blocked/skipped skills are disclosed honestly
- [ ] local git status is clean

### Suggested GitHub flow

```bash
cd "C:/Users/akuta/AI Agent/hermes/skill-optimizer-repo"
git branch -M main
git remote add origin <your-github-repo-url>
git push -u origin main
```

## 2. Hermes Ecosystem publication

Hermes exposes a publish command:

```bash
hermes skills publish <skill_path>
```

The current CLI also advertises target selection:

```bash
hermes skills publish --to github <skill_path>
hermes skills publish --to clawhub <skill_path>
hermes skills publish --to github --repo owner/repo <skill_path>
```

### Practical ecosystem flow

1. keep the skill in a clean directory with a valid `SKILL.md`
2. validate frontmatter and structure
3. publish to GitHub first
4. publish via Hermes CLI or through the desired registry path
5. verify the published skill still reads as provider-agnostic and not tied to one user's subscription

## 3. Discovery via GitHub taps

A GitHub repo can also be used as a discovery source:

```bash
hermes skills tap add owner/repo
```

That means a public GitHub repo is useful even before full registry promotion.

## 4. Ecosystem-ready criteria

For Hermes Ecosystem publication, the skill should:
- have clean frontmatter
- have a short trigger-focused description
- infer the user's provider stack from live state when possible
- avoid user-specific assumptions in the main body
- keep provider-specific examples in references
- state verification steps clearly
- avoid unsafe or unverified instructions

## 5. Recommended release order

### Phase A — GitHub
- push repo
- verify README renders cleanly
- verify references are understandable to a stranger

### Phase B — Discovery
- optionally test repo-based discovery with `hermes skills tap add owner/repo`

### Phase C — Registry publication
- publish from the skill directory or repo path
- verify published metadata
- verify installability from a clean environment
