# Web plan handoff

Ask the web planner for a compact plan that Codex can execute directly. Paste
the plan into the Codex task, attach it, or provide an accessible path or link.
The web planner should not claim it has inspected local files unless it had
access to the same repository revision.

```text
Goal: <observable outcome>
Repository/revision: <path and commit or "unverified">
Decisions and constraints: <only decisions Codex must preserve>

Tasks:
- ID: T1
  Role/model: <Explorer, Executor, or specialist; preferred model if known>
  Objective: <one bounded outcome>
  Files/ownership: <relevant paths; sole editor for each shared file>
  Depends on: <IDs or none>
  Steps: <concrete actions and error behavior>
  Acceptance: <observable conditions>
  Tests: <existing commands, or "discover from project">

Final review: <what Codex must check across all tasks>
```

Keep the task list short. Separate tasks only when ownership or dependencies
make delegation useful. State uncertain paths as hypotheses. Codex checks live
repository facts, dispatches ready tasks, and reports any material deviation
from the handoff instead of replanning the whole request.

For a task that must click or inspect a browser or desktop app, use
`computer_operator / gpt-6.1-terra` as its preferred role/model and name the
target app plus the UI result to verify. Codex must still confirm that the
computer-use tool and app permission are available in the execution session.
