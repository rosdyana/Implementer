# Implementer

A Claude Code plugin that splits planning from typing. The main model plans and reviews. A fast, cheap subagent does the code edits. It runs on **Haiku** by default, and you can switch it to Sonnet or Opus.

It includes:

| Component | What it does |
|---|---|
| `implementer` agent | Carries out an approved plan exactly as written, runs the verification steps, and reports failures instead of guessing. Runs at medium effort. |
| `/implementer:implement` skill | The workflow: delegate the plan to the implementer on the model you chose, then read the diff and run the checks on the main model. |

## Install

The same commands work on **macOS, Linux and Windows**. No scripts or extra dependencies; you only need Claude Code.

### Option A: inside Claude Code

```
/plugin marketplace add rosdyana/Implementer
/plugin install implementer@implementer
```

When the plugin is enabled, Claude Code asks which **implementer model** you want (`haiku`, `sonnet` or `opus`). Then restart Claude Code, or run `/reload-plugins`.

### Option B: from a terminal

Choose the model with `--config model=<haiku|sonnet|opus>`. Leave it out to get `haiku`.

macOS / Linux (bash, zsh):

```bash
claude plugin marketplace add rosdyana/Implementer && claude plugin install implementer@implementer --config model=haiku
```

Windows (PowerShell):

```powershell
claude plugin marketplace add rosdyana/Implementer; claude plugin install implementer@implementer --config model=haiku
```

Windows (Command Prompt):

```bat
claude plugin marketplace add rosdyana/Implementer && claude plugin install implementer@implementer --config model=haiku
```

## Change the model later

You don't need to edit any files:

- **In Claude Code:** open `/config`, find **Implementer model**, and pick a value. Or run `/plugin configure implementer@implementer`.
- **From a terminal:** `claude plugin configure implementer@implementer`.

Restart Claude Code, or run `/reload-plugins`, for the change to take effect.

> **How it works:** Claude Code doesn't fill plugin options into an agent's `model:` frontmatter, so the agent file keeps `model: haiku` as a fallback. The `/implementer:implement` skill passes your chosen model as an override every time it delegates. Your choice applies when work goes through the skill. That's why the CLAUDE.md snippet below points Claude at the skill.

### Check that it worked

Run `/agents` in Claude Code. `implementer` should be listed under the plugin agents.

## Update / uninstall

```
claude plugin marketplace update implementer
claude plugin update implementer@implementer
claude plugin uninstall implementer@implementer
```

## Usage

After a plan is approved, run:

```
/implementer:implement
```

Or just say "implement the plan with the implementer."

### Optional: delegate automatically

To have Claude use the implementer for every approved plan without being asked, add this to your user `CLAUDE.md`. That's `~/.claude/CLAUDE.md` on macOS/Linux and `%USERPROFILE%\.claude\CLAUDE.md` on Windows.

```markdown
# Implementation
After a plan is approved, carry it out with the `implementer:implement` skill: it delegates the code edits to the `implementer` subagent on the configured model, then reviews the diff and runs the final checks on the main model.
Do the edits on the main model instead when the project's CLAUDE.md marks an area as high-risk, or when the change is a one-line fix that isn't worth delegating.
```

## Notes

`haiku`, `sonnet` and `opus` are aliases for the newest model in each family.

If you already have a personal `~/.claude/agents/implementer.md`, remove it after installing so the two don't conflict.

## License

MIT
