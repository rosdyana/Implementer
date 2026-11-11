# Implementation (from the implementer plugin)
After a plan is approved, carry it out with the `implementer:implement` skill: it delegates the code edits to the `implementer:implementer` subagent on the configured model, then reviews the diff and runs the final checks on the main model. Do this without being asked; the user does not need to mention the implementer.
Do the edits on the main model instead when the project's CLAUDE.md marks an area as high-risk, or when the change is a one-line fix that isn't worth delegating.
