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

### Automatic delegation

You don't have to ask. Two hooks in `hooks/hooks.json` handle it:

- A `SessionStart` hook adds a short rule to every session, including after `/clear` and compaction: once a plan is approved, Claude delegates through `/implementer:implement` by itself, and any plan it writes gets an "Execution: delegate via `implementer:implement`" line.
- A `PostToolUse` hook on `ExitPlanMode` repeats the reminder at the moment you approve a plan, when it matters most.

No `CLAUDE.md` changes are needed.

Both hooks only print a file with `cat` (`hooks/session-context.md` and `hooks/plan-approved.json`), which works the same in `sh` (macOS/Linux), Git Bash and PowerShell (Windows). No scripts, no extra dependencies.

There are only two exceptions, which stay on the main model: single-line changes, and any area your project's `CLAUDE.md` marks as high-risk. When Claude uses one, it tells you which.

If you already have a similar delegation rule in your own `CLAUDE.md`, remove it so the two don't conflict. In particular, remove any "don't override its model" line, because the skill now passes your chosen model.

To stop automatic delegation but keep the command, disable the plugin's hooks by forking and deleting `hooks/hooks.json`, or uninstall the plugin.

## Notes

`haiku`, `sonnet` and `opus` are aliases for the newest model in each family.

If you already have a personal `~/.claude/agents/implementer.md`, remove it after installing so the two don't conflict.

## License

MIT
