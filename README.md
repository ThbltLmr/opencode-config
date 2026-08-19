# opencode configuration

Personal [opencode](https://opencode.ai) configuration, living in opencode's
documented global config directory, `~/.config/opencode`.

Aligned with the sibling configs so the four coding agents behave the same way:
[claude-config](https://github.com/ThbltLmr/claude-config),
[codex-config](https://github.com/ThbltLmr/codex-config), and pi-config.

## What is here

- `opencode.json` — `anthropic/claude-opus-5` at the `high` reasoning variant,
  Haiku 4.5 as the small model, session sharing disabled, and a read-permission
  deny-list for `.env` files, `secrets/`, and `credentials*`. Also defines
  `pair-programming`, a primary agent that asks before every edit.
- `tui.json` — Catppuccin Frappe, matching Codex and Pi.
- `AGENTS.md` — global instructions, wired in through `instructions`.
- `skill/` — model-invoked skills: `agent-browser`, `atomic-commit`, `unslop`.
- `command/` — user-invoked slash commands: `/grill-me`, `/teach-me`,
  `/handoff`, `/catchup`. opencode skills have no invocation-control
  frontmatter, so a command is how a user-only entry point is expressed here.
- `plugins/herdr-agent-state.js` — herdr session-state integration.

## Not `~/.opencode`

opencode resolves project config by walking up from the cwd **to the worktree
root**, not to `$HOME`. A `.opencode` directory in the home directory is
therefore only picked up when the cwd is `$HOME` itself, so config kept there
never applied to real projects. `~/.opencode` is now only the install location
for the `opencode` binary.

Verify what is actually loaded:

```bash
opencode debug config    # resolved config
opencode debug skill     # resolved skills, with the file each came from
```

## External skills

opencode also auto-scans `~/.claude/skills` and `~/.agents/skills`, and it
ignores their `disable-model-invocation` frontmatter — so the user-invoked-only
skills leak in as model-invocable duplicates. `OPENCODE_DISABLE_EXTERNAL_SKILLS=1`
is exported from `~/.config/zsh/.zshrc` to suppress both scans.

## Notes

The repository uses an allow-list `.gitignore`; `node_modules/`, lockfiles, and
the `@opencode-ai/plugin` dependency of the herdr plugin stay local.

Config is read once at startup and is not hot-reloaded — restart opencode after
editing anything here.
