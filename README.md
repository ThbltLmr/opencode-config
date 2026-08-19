# opencode configuration

This directory is **only** the opencode install location: `~/.opencode/bin/opencode`
is the binary that `which opencode` resolves to, and it is deliberately ignored
by this repository.

The configuration itself lives in **`~/.config/opencode`**, opencode's documented
global config directory. It is currently *not* under version control.

## Why the move

opencode resolves project config by walking up from the cwd **to the worktree
root**, not to `$HOME`. A `.opencode` directory in the home directory is
therefore only picked up when the cwd is `$HOME` itself, so the config that used
to live here never applied to real projects.

Confirm what is actually loaded with:

```bash
opencode debug config    # resolved config
opencode debug skill     # resolved skills, with the file each came from
```

## What is in ~/.config/opencode

- `opencode.json` — `anthropic/claude-opus-5` at the `high` reasoning variant,
  Haiku 4.5 as the small model, session sharing disabled, and a read-permission
  deny-list for `.env` files, `secrets/`, and `credentials*`.
- `tui.json` — Catppuccin Frappe, matching Codex and Pi.
- `AGENTS.md` — global instructions, wired in through `instructions`.
- `skill/` — the model-invoked skills: `agent-browser`, `atomic-commit`.
- `command/` — the user-invoked slash commands: `/grill-me`, `/teach-me`,
  `/handoff`, `/catchup`. opencode skills have no invocation-control frontmatter,
  so commands are how a user-only entry point is expressed here.
- `plugins/herdr-agent-state.js` — herdr session-state integration.

Aligned with the sibling configs so the four coding agents behave the same way:
[claude-config](https://github.com/ThbltLmr/claude-config),
[codex-config](https://github.com/ThbltLmr/codex-config), and pi-config.

## External skills

opencode also auto-scans `~/.claude/skills` and `~/.agents/skills`, and it
ignores their `disable-model-invocation` frontmatter — so the user-invoked-only
skills leak in as model-invocable duplicates. `OPENCODE_DISABLE_EXTERNAL_SKILLS=1`
is exported from `~/.config/zsh/.zshrc` to suppress both scans.

## Notes

Config is read once at startup and is not hot-reloaded — restart opencode after
editing anything.
