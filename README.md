# Implementer

A Claude Code plugin that splits planning from typing. The main model plans and reviews. A fast, cheap **Haiku** subagent does the code edits.

It includes:

| Component | What it does |
|---|---|
| `implementer` agent | Carries out an approved plan exactly as written, runs the verification steps, and reports failures instead of guessing. Runs on `haiku` at medium effort. |
| `/implementer:implement` skill | The workflow: delegate the plan to the implementer, then read the diff and run the checks on the main model. |

## Install

The same commands work on **macOS, Linux and Windows**. No scripts or extra dependencies; you only need Claude Code.

### Option A: inside Claude Code

```
/plugin marketplace add rosdyana/Implementer
/plugin install implementer@implementer
```

Then restart Claude Code, or run `/reload-plugins`.

### Option B: from a terminal

macOS / Linux (bash, zsh):

```bash
claude plugin marketplace add rosdyana/Implementer && claude plugin install implementer@implementer
```

Windows (PowerShell):

```powershell
claude plugin marketplace add rosdyana/Implementer; claude plugin install implementer@implementer
```

Windows (Command Prompt):

```bat
claude plugin marketplace add rosdyana/Implementer && claude plugin install implementer@implementer
```

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
After a plan is approved, delegate the code edits to the `implementer` subagent instead of editing directly. Give it the exact files, changes, and verification commands, since it can't see this conversation. Don't override its model in the Agent call. Then review its diff and run the final checks yourself.
Do the edits on the main model instead when the project's CLAUDE.md marks an area as high-risk, or when the change is a one-line fix that isn't worth delegating.
```

## Changing the model

`haiku` is an alias for the newest Haiku model. To use Sonnet for harder work, fork this repo and change `model:` in `agents/implementer.md` to `sonnet`, or to a full model ID to pin a version.

If you already have a personal `~/.claude/agents/implementer.md`, remove it after installing so the two don't conflict.

## License

MIT
