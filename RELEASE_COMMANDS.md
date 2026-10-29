# Release Commands

Commands for publishing the repo, then the skill. All paths are relative to the repo root - do
not commit or use an absolute path containing a username.

## 1. Push to GitHub

```bash
cd hermes-optimizer
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

From the repo root:

```bash
hermes skills publish .
```

Or choose an explicit target:

```bash
hermes skills publish --to github .
hermes skills publish --to github --repo owner/repo .
hermes skills publish --to clawhub .
```

## 4. Final pre-publish check

Before publishing, confirm:

- `git status --short` is empty
- `SKILL.md` is the provider-agnostic guided version (version 2.0.0 or later)
- `README.md` explains the phase plan, Desktop surfaces, and the anti-bloat test
- all `references/` files are present and internal links resolve
- no absolute path containing a username anywhere in the repo
- blocked and skipped upstream skills are disclosed honestly in `references/synthesis-map.md`
- the author's own routing is not present in any public example