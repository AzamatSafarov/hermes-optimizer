# Publishing Hermes Optimizer

This file explains how to turn this repo into a public GitHub project and then publish the skill into the Hermes Ecosystem.

## 1. GitHub publication

### Recommended GitHub repo contents

At minimum:
- `README.md`
- `SKILL.md`
- `references/imported-skills.md`
- `references/synthesis-map.md`
- `notes/current-routing.md`
- `PUBLISHING.md`

### Suggested GitHub flow

```bash
cd "C:/Users/akuta/AI Agent/hermes/skill-optimizer-repo"
git remote add origin <your-github-repo-url>
git branch -M main
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

### Practical publication flow

1. keep the skill in a clean directory with a valid `SKILL.md`
2. validate frontmatter and structure
3. publish to GitHub first
4. publish via Hermes CLI or submit through the desired registry flow

## 3. Discovery via GitHub taps

A GitHub repo can also be used as a discovery source:

```bash
hermes skills tap add owner/repo
```

That means a public GitHub repo is useful even before full registry promotion.

## 4. What makes a skill ecosystem-ready

For Hermes Ecosystem publication, the skill should:
- have clean frontmatter
- have a short trigger-focused description
- avoid user-specific assumptions in the main body
- keep provider-specific examples in references
- state verification steps clearly
- avoid unsafe or unverified instructions

## 5. Current policy for this project

`hermes-optimizer` should be published as:
- a **universal optimization skill** in `SKILL.md`
- with **provider-profile examples** in references/notes
- with **provenance documentation** proving how it was synthesized

## 6. Caution about third-party sources

Some imported or inspected upstream skills were blocked by the scanner or contained overly tool-specific assumptions.

Do **not** present this project as a raw mirror of upstream skills.
Present it as:
- a reviewed synthesis
- adapted for Hermes Agent
- validated with live config and model tests
