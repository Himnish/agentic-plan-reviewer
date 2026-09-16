# Agentic Plan Reviewer

Review an execution plan before an agent runs it. The reviewer combines deterministic
checks with the AI assistant you already use, reports defects with exact evidence, and
proposes repairs without applying them.

- No account, API key, or separate service
- No plan or tool execution
- No source-plan edits
- Works with native/free-form Markdown and structured JSON plans
- Supports Cursor, Claude Code, and Windsurf

## Install

Prerequisites: Python 3 and an authenticated [GitHub CLI](https://cli.github.com/) account
with access to this private repository.

Choose your host and paste one command:

**Cursor**

```bash
mkdir -p "$HOME/.cache/plan-reviewer" && gh release download --repo Himnish/agentic-plan-reviewer --pattern "plan-reviewer.pyz" --dir "$HOME/.cache/plan-reviewer" --clobber && python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" install --host cursor --scope user
```

**Claude Code**

```bash
mkdir -p "$HOME/.cache/plan-reviewer" && gh release download --repo Himnish/agentic-plan-reviewer --pattern "plan-reviewer.pyz" --dir "$HOME/.cache/plan-reviewer" --clobber && python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" install --host claude --scope user
```

**Windsurf**

```bash
mkdir -p "$HOME/.cache/plan-reviewer" && gh release download --repo Himnish/agentic-plan-reviewer --pattern "plan-reviewer.pyz" --dir "$HOME/.cache/plan-reviewer" --clobber && python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" install --host windsurf --scope user
```

Use `--host all` to install for all three. If the skill is not immediately visible, reload
the editor or start a fresh agent session.

## Use

- Cursor: type `/plan-reviewer` or `@plan-reviewer`, then attach/name the plan.
- Claude Code: type `/plan-reviewer`, then name the plan.
- Windsurf: type `@plan-reviewer`, then attach/name the plan.

Example:

> Review this plan before implementation. Flag defects first, propose repairs, and do not
> execute or edit the plan.

The review artifacts are written under the current workspace's
`.plan-reviewer/reviews/` directory. Start with `report.md`.

## Project-only install

Run the install command from the project root and change `--scope user` to
`--scope project`. This installs the skill only for that repository.

## Update

Run the same install command again. The installer updates only installations it owns and
refuses to overwrite an unrelated `plan-reviewer` skill.

## Uninstall

Use the cached release artifact:

```bash
python3 "$HOME/.cache/plan-reviewer/plan-reviewer.pyz" uninstall --host cursor --scope user
```

Replace `cursor` with `claude`, `windsurf`, or `all`. Use the same scope used during
installation.

## Security and privacy

The package runs locally and makes no network calls. It does not call an AI provider itself;
semantic review runs inside the Cursor, Claude Code, or Windsurf session you already chose.
Your host's normal data-handling policy therefore applies to the plan.

This repository intentionally contains onboarding documentation and release metadata only.
The downloadable local package is inspectable and is not a confidentiality boundary.

See [INSTALL.md](INSTALL.md) for detailed setup, Windows instructions, checksum verification,
host paths, updates, and troubleshooting.
