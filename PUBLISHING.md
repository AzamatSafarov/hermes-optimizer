# Publishing Hermes Optimizer

How to turn this repo into a public GitHub project and publish the skill.

## Publication philosophy

Publish `hermes-optimizer` as:

- a **universal guided optimization skill** in `SKILL.md` - phases, questions, anti-bloat test, lane model
- with **provider autodiscovery** for everything the machine can read
- with **provider-specific examples** kept in references and notes
- with **provenance documentation** showing how it was synthesized
- with **honest disclosure** of what was rejected and why

Do **not** present this project as a raw mirror of upstream skills. Present it as a reviewed
synthesis, adapted for Hermes Agent, with Desktop routes and CLI subcommands confirmed against
the real install.

## Repo-local paths

Keep all paths in this repo **relative** or use a placeholder. Do not commit an absolute path
that contains a username - it leaks local layout and breaks for every other reader.

```bash
cd hermes-optimizer      # from where you cloned it
```

## 1. GitHub publication

### Recommended repo contents

At minimum:

- `README.md`
- `SKILL.md`
- `LICENSE`
- `CHANGELOG.md`
- `KANBAN.md`
- `references/` - all of them
- `notes/current-routing.md`
- `PUBLISHING.md`
- `llms.txt`, `llms-full.txt`

### GitHub preflight checklist

Before pushing:

- [ ] `SKILL.md` has valid frontmatter
- [ ] description is trigger-focused and not user-specific
- [ ] the phase plan is described in the core skill, with per-phase detail in `references/phase-plan.md`
- [ ] Desktop surfaces are documented and were verified against the app, not guessed
- [ ] CLI subcommands named in the docs were confirmed to exist
- [ ] no absolute path containing a username anywhere in the repo
- [ ] provider-specific mappings live only in notes/references
- [ ] provenance docs explain which upstream skills were used and which were rejected
- [ ] all internal `references/` links resolve
- [ ] local git status is clean

### Suggested GitHub flow

```bash
cd hermes-optimizer
git branch -M main
git remote add origin <your-github-repo-url>
git push -u origin main
```

## 2. Hermes Ecosystem publication

Hermes exposes a publish command:

```bash
hermes skills publish <skill_path>
```

With explicit targets:

```bash
hermes skills publish --to github <skill_path>
hermes skills publish --to clawhub <skill_path>
hermes skills publish --to github --repo owner/repo <skill_path>
```

### Practical ecosystem flow

1. keep the skill in a clean directory with a valid `SKILL.md`
2. validate frontmatter and structure
3. publish to GitHub first
4. publish via the Hermes CLI or the registry path you want
5. verify the published skill still reads as provider-agnostic and not tied to one user's subscription

## 3. Discovery via GitHub taps

A GitHub repo works as a discovery source even before registry promotion:

```bash
hermes skills tap add owner/repo
```

## 4. Ecosystem-ready criteria

For publication, the skill should:

- have clean frontmatter
- have a short trigger-focused description
- infer from live state what the machine can answer, and ask only what it cannot
- avoid user-specific assumptions in the main body
- keep provider-specific examples in references
- state verification steps clearly
- avoid unsafe or unverified instructions
- name Desktop surfaces that actually exist and CLI subcommands that actually run

## 5. Recommended release order

### Phase A - GitHub
- push the repo
- verify the README renders cleanly
- verify the references are understandable to a stranger

### Phase B - Discovery
- optionally test repo-based discovery with `hermes skills tap add owner/repo`

### Phase C - Registry publication
- publish from the skill directory or repo path
- verify published metadata
- verify installability from a clean environment