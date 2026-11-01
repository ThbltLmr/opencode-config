# opencode configuration

Personal [opencode](https://opencode.ai) configuration, living in opencode's
documented global config directory, `~/.config/opencode`.

This machine uses OpenCode 2.1.4 as of 2026-10-25. The V2 migration guide is
https://dev.opencode.ai/v2/docs/migrate-v1/.

Aligned with the sibling configs so the four coding agents behave the same way:
[claude-config](https://github.com/ThbltLmr/claude-config),
[codex-config](https://github.com/ThbltLmr/codex-config), and pi-config.

## What is here

- `opencode.json` — `anthropic/claude-opus-5` at the `high` reasoning variant,
  Haiku 4.5 as the small model, session sharing disabled, and a read-permission
  deny-list for `.env` files, `secrets/`, and `credentials*`. Also defines
  `pair-programming`, a primary agent that asks before every edit.
- `cli.json` — V2 terminal settings, with Catppuccin Frappe, automatic diff
  layout, notifications enabled and sound disabled. `tui.json` retains the V1
  preferences for reference.
- `AGENTS.md` — global instructions, wired in through `instructions`.
- `skill/` — model-invoked skills: `agent-browser`, `atomic-commit`, `unslop`.
- `command/` — user-invoked slash commands: `/grill-me`, `/teach-me`,
  `/handoff`, `/catchup`. opencode skills have no invocation-control
  frontmatter, so a command is how a user-only entry point is expressed here.
- `plugins/herdr-agent-state.js` — Herdr's dual V1/V2 server entrypoint.
- `herdr-opencode/tui.js` and `herdr-tui-session.js` — Herdr V2 terminal
  integration v12, configured through `cli.json`. Session selection, activity,
  permissions and forms report from the pane's terminal process because a shared
  V2 server cannot use one pane's environment to identify every session.

The native V2 `agents` configuration explicitly selects `#high` for Build,
Plan, General and Pair Programming. Global permissions retain the sensitive-file
denials and explicitly ask before accessing an external directory. Pair
Programming still asks before editing. Amazon Bedrock remains disabled.

## Not `~/.opencode`

`~/.opencode` is the install location for the `opencode` binary. Global settings
belong in `~/.config/opencode`. V2 searches for project configuration from the
current directory to the filesystem root, so do not put global settings in a
home-directory `.opencode` tree.

Verify what is actually loaded:

```bash
opencode debug config    # resolved config
opencode debug agents    # resolved models and permissions
opencode plugin list     # configured plugins
```

## External skills

opencode also auto-scans `~/.claude/skills` and `~/.agents/skills`, and it
ignores their `disable-model-invocation` frontmatter — so the user-invoked-only
skills leak in as model-invocable duplicates. `OPENCODE_DISABLE_EXTERNAL_SKILLS=1`
is exported from `~/.config/zsh/.zshrc` to suppress both scans.

## Notes

The repository uses an allow-list `.gitignore`; `node_modules/`, lockfiles, and
the `@opencode/plugin` SDK dependency stay local. The Herdr integration itself
has no SDK dependency.

Run `opencode reload` after changing server configuration. V2 watches valid edits
to `cli.json`. Reinstall managed Herdr files with
`herdr integration install opencode`; keep `./herdr-opencode` in the V2
`cli.json` plugins array. The installer also maintains V1 `tui.jsonc` settings.
