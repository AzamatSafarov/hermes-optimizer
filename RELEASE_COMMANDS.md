# Release Commands

Use these commands when you are ready to publish the repo and then publish the skill.

## 1. Push to GitHub

```bash
cd "C:/Users/akuta/AI Agent/hermes/skill-optimizer-repo"
git branch -M main
git remote add origin <your-github-repo-url>
git push -u origin main
```

If `origin` already exists:

```bash
git remote set-url origin <your-github-repo-url>
git push -u origin main
```

## 2. Optional: test repo-based discovery

```bash
hermes skills tap add owner/repo
```

## 3. Publish the skill

From the repo root or from the skill directory:

```bash
hermes skills publish "C:/Users/akuta/AI Agent/hermes/skill-optimizer-repo"
```

Or choose an explicit target:

```bash
hermes skills publish --to github "C:/Users/akuta/AI Agent/hermes/skill-optimizer-repo"
hermes skills publish --to github --repo owner/repo "C:/Users/akuta/AI Agent/hermes/skill-optimizer-repo"
hermes skills publish --to clawhub "C:/Users/akuta/AI Agent/hermes/skill-optimizer-repo"
```

## 4. Final pre-publish check

Before publishing, confirm:
- `git status --short` is empty
- `SKILL.md` is the provider-agnostic official version
- `README.md` explains autodiscovery and provenance
- `PUBLISHING.md` and `references/` are present
- blocked/skipped upstream skills are disclosed honestly
