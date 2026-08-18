# opencode configuration

Personal [opencode](https://opencode.ai) configuration: models, TUI theme,
agents, and commands.

Aligned with the sibling configs so the four coding agents behave the same way:
[claude-config](https://github.com/ThbltLmr/claude-config),
[codex-config](https://github.com/ThbltLmr/codex-config), and pi-config.

## What is here

- `opencode.json` — `anthropic/claude-opus-5` at the `high` reasoning variant,
  Haiku 4.5 as the small model, session sharing disabled, and a read-permission
  deny-list for `.env` files, `secrets/`, and `credentials*`.
- `tui.json` — Catppuccin Frappe, matching Codex and Pi.
- `AGENTS.md` — global instructions: which subagent tier to reach for, and no
  tool attribution in commits. Wired in through `instructions`.
- `agent/code-reviewer.md` — the read-only, high-effort reviewer that also
  exists as a Claude Code subagent and a Codex agent.
- `command/` — the shared slash commands:
  - `/atomic-commit`
  - `/catchup`
  - `/start-code-reviewer`

## Agent tiers

`opencode`'s `task` tool picks a subagent, not a model, so the cost/quality
tiering lives on the agents themselves:

| Tier    | Agent           | Model                       |
| ------- | --------------- | --------------------------- |
| Cheap   | `explore`       | `anthropic/claude-haiku-4-5` |
| Default | `general`       | session model, `high`       |
| Highest | `code-reviewer` | `anthropic/claude-opus-5`, `high`, read-only |

`pair-programming` is a primary agent that asks before every edit.

## Shared skills

opencode scans `~/.claude/skills` and `~/.agents/skills` on its own, so the
skills versioned in the Claude and Codex configs (`grill-me`, `teach-me`,
`handoff`, `pr-comments-triage`, `writing-great-skills`, `agent-browser`) are
already available here. Nothing to symlink. Verify with:

```bash
opencode debug skill
```

## Two directories

opencode's documented global config directory is `~/.config/opencode`, but it
also walks up from the cwd collecting `.opencode` directories, so this
repository is loaded for every project under `$HOME`. Confirm with
`opencode debug config` from inside a project.

`~/.config/opencode` is left to tool-managed state that should not be versioned:
the `peon-ping` sound plugin and, if installed, the herdr agent-state plugin
(`herdr integration install opencode`).

`~/.opencode/bin/opencode` is the installed binary and is deliberately ignored.

## Notes

The repository intentionally uses an allow-list `.gitignore`. Auth, sessions,
caches, the binary, and `node_modules` remain local and must never be committed.

Config is read once at startup and is not hot-reloaded — restart opencode after
editing anything here.
