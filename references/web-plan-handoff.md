# Web plan handoff

Present the proposal to the user on the planning host. Revisions stay in chat
until the user confirms execution. For a repository handoff, publish the
confirmed plan with a short record of the user's approval and its scope. The
receiving Executor verifies live prerequisites and proceeds directly from that
record. A READY label by itself does not establish authorization. A clear,
explicitly requested simple task can proceed directly.

The planner must not claim it inspected local files unless it had access to the
same repository revision. Use `unknown` for unverified repository facts, models,
costs, and permissions.

```text
Goal: <observable outcome>
Repository/revision: <path and commit or unverified>
Decisions and constraints: <only decisions that must be preserved>
Approval state: <proposal, or user explicitly authorized execution>

Tasks:
- ID: T1
  Objective: <one bounded outcome>
  Difficulty and reason: <level and brief rationale>
  Model routing: <PINNED exact model, or CAPABILITY requirements; actual model or UNKNOWN>
  Execution models: <GPT exact ID; Gemini exact ID; DeepSeek exact ID; state scope and unverified availability>
  Cost: <estimate or unknown>
  Tool/host: <native agent, CLI, connector, or UI tool>
  Files/ownership: <relevant paths; sole writer for shared files>
  Depends on: <IDs or none>
  Steps: <concrete actions and error behavior>
  Acceptance: <observable conditions>
  Validation: <discovered commands and expected evidence>

Final review: <exact GPT model ID, evidence to check, and GPT handoff if unavailable>
External actions: <publishing or other externally visible changes>
```

Example subtask table for any host:

| Subtask | Difficulty and reason | Model (PINNED/CAPABILITY; actual or UNKNOWN) | Cost | Tool/host | Depends on | Validation |
| --- | --- | --- | --- | --- | --- | --- |
| T1: inspect parser callers | Level 2: several files | CAPABILITY: coding; GPT `gpt-6-luna`, Gemini `gemini-3.8-flash-medium`, DeepSeek `deepseek-v4.1-flash-1017` for selected excerpts | Low/unknown | Native workspace | None | Parser tests and diff review |

Keep the plan short. Separate tasks only when ownership or dependencies justify
it. Verify requested models are callable with required tools and permissions; a
model catalog alone is not proof. In CAPABILITY mode, use the current host/model
when it meets the need. Use only authorized fallbacks, preserve the current
primary model, and do not recursively delegate.
For UI work, name the app, required result, and available UI permissions.
