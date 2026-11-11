---
name: implement
description: Carry out an approved plan by delegating the code edits to the implementer subagent, then review its diff and run the final checks on the main model. Use when a plan has been approved and is ready to execute, or when the user runs /implementer:implement.
---

# Implement an approved plan

1. **Confirm there is an approved plan.** If there isn't one, write a plan and get it approved first.

2. **Decide whether to delegate.** Do the edits yourself on the main model instead when:
   - the project's CLAUDE.md marks the area as high-risk, or
   - the change is a one-line fix that isn't worth delegating.

3. **Delegate to the `implementer` subagent.** It can't see this conversation, so the prompt must stand on its own:
   - the exact files to change and what to change in each,
   - any constraints (style, APIs to use, things not to touch),
   - the verification commands to run (build, tests, lint).
   Don't override its model in the Agent call.

4. **Review its work yourself. Don't rely on its report alone.**
   - Read the actual diff (`git diff`).
   - Check it against the plan: missing steps, edits outside the scope, bugs.
   - Run the verification commands yourself.

5. **Fix or send back.** Fix small problems directly. For larger ones, send the implementer a corrected, specific instruction. Then repeat step 4.

6. **Report** what changed and the real results of the checks, including any failures.
