# Installation and operations

## Requirements

1. Python 3.9 or newer:

   ```bash
   python3 --version
   ```

2. GitHub CLI authenticated to an account that has access to the private repository:

   ```bash
   gh auth status
   ```

   If needed, run `gh auth login` and ask the repository owner for access. You never need to
   clone the repository.

## macOS and Linux

Download the latest installer:

```bash
mkdir -p "$HOME/.cache/plan-reviewer"
gh release download --repo Himnish/agentic-plan-reviewer --pattern "plan-reviewer.pyz*" --dir "$HOME/.cache/plan-reviewer" --clobber
```

Optionally verify the release checksum:

```bash
cd "$HOME/.cache/plan-reviewer"
shasum -a 256 -c plan-reviewer.pyz.sha256
```

On Linux, use `sha256sum -c plan-reviewer.pyz.sha256` when `shasum` is unavailable.

Install for one host:

```bash
python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" install --host cursor --scope user
python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" install --host claude --scope user
python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" install --host windsurf --scope user
```

Or install for all supported hosts:

```bash
python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" install --host all --scope user
```

## Windows PowerShell

```powershell
$dest = "$env:LOCALAPPDATA\plan-reviewer-download"
New-Item -ItemType Directory -Force -Path $dest | Out-Null
gh release download --repo Himnish/agentic-plan-reviewer --pattern "plan-reviewer.pyz*" --dir $dest --clobber
py -3 "$dest\plan-reviewer.pyz" install --host all --scope user
```

To verify the checksum, compare:

```powershell
(Get-FileHash "$dest\plan-reviewer.pyz" -Algorithm SHA256).Hash.ToLower()
Get-Content "$dest\plan-reviewer.pyz.sha256"
```

## Installation paths

User-scoped skills:

- Cursor: `~/.cursor/skills/plan-reviewer`
- Claude Code: `~/.claude/skills/plan-reviewer`
- Windsurf: `~/.codeium/windsurf/skills/plan-reviewer`

Project-scoped skills:

- Cursor: `<project>/.cursor/skills/plan-reviewer`
- Claude Code: `<project>/.claude/skills/plan-reviewer`
- Windsurf: `<project>/.windsurf/skills/plan-reviewer`

For project scope, `cd` to the project root and use `--scope project`. The runtime is stored
under `<project>/.plan-reviewer/runtime/`.

For user scope, the runtime is stored under the user's application-data directory. Each
skill copy contains `bundle_root.txt`, so the agent can resolve the runtime without guessing
or following a symlink.

## Verify discovery

- Cursor: reload the window if necessary, open Customize → Skills, then invoke
  `/plan-reviewer` or `@plan-reviewer`.
- Claude Code: start a fresh session if necessary, then invoke `/plan-reviewer`.
- Windsurf: reload Cascade if necessary, then invoke `@plan-reviewer`.

Explicit invocation is recommended for reviews that must run. Automatic invocation depends
on the host matching your request to the skill description.

For Cursor Plan Mode, invoke the skill in the request that needs review. A one-message skill
attachment is not documented to persist after Plan Mode switches to Build, so invoke it
again before implementation when needed.

## Update

Download the latest release with `--clobber`, verify it if required, and repeat `install`.
The installer recognizes its ownership marker and replaces only its own skill copy.

## Uninstall

```bash
python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" uninstall --host all --scope user
```

Use the appropriate downloaded path on Windows. For project installs, run from the same
project root and use `--scope project`.

The installer refuses to remove a destination without its package ownership marker.

## Troubleshooting

**`gh` says repository not found**

Confirm `gh auth status` uses the invited GitHub account and that the repository owner has
granted access.

**The skill asks for the bundle path**

Re-run `install`. A correct install has `bundle_root.txt` beside `SKILL.md`; do not guess or
paste somebody else's absolute path.

**The skill is not listed**

Confirm the host-specific path above, then reload the editor or start a new agent session.

**The plan cannot be found**

Attach it, save it, or provide its absolute path. Native and free-form Markdown plans do not
need special headings.

**`prepare` or `finalize` fails**

Read the complete error. The reviewer accepts flexible input plans but deliberately keeps
its evidence, repair, and result contracts strict.

**An existing skill blocks installation**

The directory is not marked as owned by this package. Rename or remove it yourself only
after inspecting it; the installer will not overwrite it.

## Agent-neutral fallback

The installed skill includes `entry_prompt.md`. Hosts that support file reading and shell
commands but not Agent Skills can follow that prompt directly. They still need the
downloaded runtime and must not execute the reviewed plan.
