# Hermes health sweep

Optimization changes routing and config. It does not, by itself, tell you whether the install is
healthy. Run this sweep as part of Phase 6, and again in Phase 7 as the before/after proof.

Treat it as a checklist: run each command, read the output, report findings. A command that
prints nothing useful is still a result - say what you checked.

## 1. Diagnose the setup

```bash
hermes doctor
```

Covers configuration and dependencies: state.db size, network routes, missing API keys, config
migration drift, npm vulnerabilities in bundled workspaces. Read the numbered issue list at the
end and re-state it, ranked.

```bash
hermes doctor --fix
```

Attempts automatic repair. Run it only after reading what it will touch, and only with the user's
consent - it edits config.

```bash
hermes doctor --live
```

Opt-in real-call health probe per configured backend (Firecrawl/FAL/browser/MCP/TTS/STT). This
makes real network calls. Use it when a tool is suspected broken, not routinely.

## 2. Supply-chain audit

```bash
hermes security audit
```

Scans the Hermes venv, Python deps declared by plugins, and pinned npx/uvx MCP servers against
OSV.dev. Does NOT scan globally-installed packages or editor extensions.

Report the severity counts, not the raw list. Then name only the HIGH and CRITICAL items with
their package and fix version.

**Do not upgrade venv packages unilaterally.** They are inside the Hermes runtime; a bump can
break it, and `hermes update` may revert it. Report and let the user decide.

### Which Python is the real runtime

On a source/git install there are **several Python environments** on disk, and installing into the
wrong one looks like it worked while changing nothing:

| Path | What it is |
|---|---|
| `~/.hermes/installs/<id>/environments/<id>/venv/` | **the running Hermes runtime** - this is the one `hermes security audit` scans |
| `~/.hermes/hermes-agent/venv/` | a build/dev venv; may exist and may be stale |
| `~/.hermes/tools/python-*/` | tool runtimes (document libs, ffmpeg host); not the app |

Never identify the runtime by path guessing. **Ask the live process:**

```bash
hermes --version >/dev/null 2>&1
python -c "import sys, importlib.metadata as m; print(sys.executable); print(m.version('tornado'))"
```

`sys.executable` names the interpreter that actually serves `hermes`. That is the environment to
target.

That environment is usually **uv-managed** and has no `pip`. If the user decides to upgrade a
flagged dependency, resolve the exact version from the advisory, pin it explicitly, and let the
user run the install themselves - do not bake an unpinned install command into this skill. An
unpinned package-install line is flagged by the skills scanner (`unpinned_pip_install`), and a
flagged line blocks the whole skill from installing.

After any upgrade, verify in this order: import the package, import its dependents
(`telegram`, `huggingface_hub`, `requests_oauthlib`), run one real `hermes chat -q` call, then
re-run `hermes security audit` and confirm the count dropped. A successful package install is not
proof the audit changed - the audit reads a specific interpreter.

## 3. Session store health

```bash
hermes sessions stats
hermes sessions optimize-storage --help
```

`state.db` growing into the gigabyte range means the search index is in the old layout. The fix is
`hermes sessions optimize-storage`, which rebuilds the index compactly and VACUUMs.

**It refuses to run while any process holds state.db** - gateway, Desktop, dashboard, or cron.
The refusal is correct and must not be overridden casually:

```text
Refusing: another process is using state.db (PID N: gateway run)
Rewriting the database under a live writer is how every agent ends up refusing turns
with the retired state.db-wal error.
```

`--force` exists but rewriting the store under a live writer can leave every agent refusing turns.
Stopping the gateway first is the supported path. Warn the user that stopping it also pauses cron,
chat gateways, and live Desktop sessions - including, potentially, the conversation you are in.

The command is safe to interrupt and re-run; it resumes. No conversation data is changed.

Other useful subcommands: `hermes sessions optimize` (FTS merge + VACUUM, no schema change),
`hermes sessions prune` (delete old sessions), `hermes checkpoints status`.

## 4. Skills hub integrity

```bash
hermes skills list
hermes skills check
hermes doctor
```

`list` reports installed vs enabled counts. `check` looks for updates to hub-installed skills.
The hub section of `doctor` confirms the lock file is readable and GitHub auth works.

## 5. Plugin runtime contracts

```bash
hermes plugins doctor <name>
hermes plugins capabilities <name>
hermes plugins check-updates
```

`doctor` validates a plugin against the real runtime contracts, not just its manifest.
`capabilities` shows declared vs actually granted capabilities - the place to catch a plugin
declaring a tool it never receives.

## 6. Tool and platform state

```bash
hermes tools list
hermes tools --summary
hermes lsp status
hermes memory status
```

Confirms which toolsets are enabled per platform, whether language servers run (they power
post-write diagnostics), and which external memory provider is active.

## 7. Storage and checkpoints

```bash
hermes checkpoints status
hermes backup --help
```

`checkpoints status` shows the shadow store size. A store that keeps growing without pruning means
`checkpoints.auto_prune` is off or the retention window is too long.

## Reading the output

Separate findings into three buckets:

1. **Agent-caused and agent-fixable** - config not applied, a lane not split, a skill left enabled.
   Fix it in the current session.
2. **User-decision** - anything that changes spend, autonomy, or removes data. Ask.
3. **Upstream / out of scope** - npm advisories awaiting a lockfile fix, retired providers, a
   broken IPv6 route at the network layer. Report, do not attempt.

Do not dump raw command output into the report. Summarize per bucket, with the count and the one
line that matters for each item.

## Pitfalls

1. **Reporting a green doctor as proof of health.** It checks configuration, not that your chosen
   models actually answer. Verify routes with real calls separately.
2. **Treating a security finding as urgent.** Most OSV findings need local access or a hostile
   peer to matter. State the practical risk, not just the severity label.
3. **Upgrading venv packages to clear advisories.** Breaks the runtime; `hermes update` may undo it.
4. **Installing into the wrong Python.** A source install has several venvs; the audit only sees
   the one `sys.executable` names. Verify by asking the live process, not by path.
5. **Running `optimize-storage` with `--force` under a live gateway.** The refusal is a feature.
5. **Assuming `approvals.mode` is missing because the key is absent.** An absent key means the
   default applies, and the default is already `smart`. Absent is not disabled.
