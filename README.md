# stack-profiles

Per-stack rule addenda plus the SessionStart hook that picks the right one.

Eleven profiles: Swift/Apple, bash scripts, Python/uv, Node/TypeScript,
Postgres/SQL, Rust, Go, React/Vite, Rails, Terraform, Kubernetes.

## Why a detector

Eleven profiles loaded unconditionally is eleven profiles' worth of context to
say something about one repo. `stack-detect.py` runs at session start, works out
what the repo is, and loads only the matching profile — so the cost is one
profile, not the set.

## Enabling takes more than installing

`stack-detect.py` reads profiles from `~/.claude/stacks/` (or
`$CLAUDE_CONFIG_DIR/stacks/`), not from the plugin directory. Installing the
plugin wires the hook; the profiles in `profiles/` still have to be deployed to
`~/.claude/stacks/` or the detector finds nothing to load.

The script is byte-identical to source and has not been rewritten to read from
`${CLAUDE_PLUGIN_ROOT}` — that would change behaviour, and these were approved
as-is.

## Status

**Not enabled.** Registered in this marketplace, installed nowhere, and no
profiles have been deployed.

## Provenance

From `Standard Configs/claude-core/stack-profiles/templates/` at `d36d794`.
Hook and all eleven profiles byte-identical.

---

## Story

Approved 2026-09-13: "pending list so far is approved" (Zach). The plugin's
verbatim row from the
[ai-additions approval ledger](https://github.com/locriani/ai-additions/blob/main/SETUP-LIST.md),
Plugins table, as of extraction:

```
| `stack-profiles` | 2026-09-13 | "pending list so far is approved" | No |
```

## Extraction

Extracted 2026-10-10 from [locriani/ai-additions](https://github.com/locriani/ai-additions)
at commit `a01d7eb87d3d24b1cd65bcf509eed82000f7fa21` (path `plugins/stack-profiles`),
history carried with `git filter-repo --subdirectory-filter`. Plugin content is carried
verbatim; the only edits are this README's added sections and the four added files
(`.claude-plugin/marketplace.json`, `LICENSE`, `.gitignore`,
`.github/workflows/validate.yml`).

## Install

```bash
claude plugin marketplace add locriani/stack-profiles
claude plugin install stack-profiles@stack-profiles
```

Update: `claude plugin marketplace update stack-profiles`, then reinstall.

## Layout

```
.claude-plugin/plugin.json   plugin manifest (name, version, author, MIT)
hooks/hooks.json             SessionStart hook wiring
hooks/stack-detect.py        the detector: works out the repo's stack, loads one profile
profiles/                    the eleven per-stack rule addenda (Markdown)
```

The deploy target `~/.claude/stacks/` is machine state, not repo content — see
"Enabling takes more than installing".

## Validation

No test framework exists or is implied. CI (`.github/workflows/validate.yml`) runs
on every push: the three JSON manifests must parse and the Python hook must
byte-compile. Validation only — no suite.
