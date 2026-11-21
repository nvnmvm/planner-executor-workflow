# Web plan handoff

Follow the [approval rules](../SKILL.md#plan-and-approval). For a GitHub handoff, confirmation to execute the presented plan authorizes posting its task sheet and executing its listed steps. Include the confirmation and its scope so the receiver can proceed directly.

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
  Preferred model: <exact ID, reason, known availability>
  Optional fallback: <model and switching condition; omit if unnecessary>
  Cost: <estimate or unknown>
  Tool/host: <native agent, CLI, connector, or UI tool>
  Files/ownership: <relevant paths; sole writer for shared files>
  Depends on: <IDs or none>
  Steps: <stable step ID | concrete action | expected observation>
  Execution record: <same step ID | actual result | evidence or unrun reason | next step if needed>
  Acceptance: <observable conditions>
  Validation: <discovered commands and expected evidence>

Final review: <exact GPT model ID, evidence to check, and GPT handoff if unavailable>
External actions: <publishing or other externally visible changes>
```

Example subtask table for any host:

| Subtask | Difficulty and reason | Model (PINNED/CAPABILITY; actual or UNKNOWN) | Cost | Tool/host | Depends on | Validation |
| --- | --- | --- | --- | --- | --- | --- |
| T1: inspect parser callers | Level 2: several files | CAPABILITY: coding; `gpt-6-luna` for bounded repository inspection (availability unverified) | Low/unknown | Native workspace | None | Parser tests and diff review |

Keep the plan short. Separate tasks only when ownership or dependencies justify
it. Verify requested models are callable with required tools and permissions; a
model catalog alone is not proof. In CAPABILITY mode, use the current host/model
when it meets the need. Use only authorized fallbacks, preserve the current
primary model, and do not recursively delegate.
For UI work, name the app, required result, and available UI permissions.

## Worked example: One optional exporter behavior

T1 delivers one outcome: the exporter can skip blank lines when requested and preserves existing output by default. One owner handles the whole change. Recommended GPT executor: `gpt-6-luna` (availability unverified).

| Step | Action | Expected observation |
| --- | --- | --- |
| T1.1 | Locate the export entry point, option handling and existing fixtures. Capture default output for a mixed input. | The plan now has verified paths and a baseline to compare against. |
| T1.2 | Add the opt-in filter at the identified boundary. | With the option enabled, whitespace-only lines disappear and nonblank entries retain their formatting. |
| T1.3 | Compare default output with the baseline, then check enabled output on the same fixture. | Both checks have recorded results, with any skipped check identified. |

Final review: `gpt-6.1-sol` (availability unverified) checks the approved behavior and regression evidence.

## Worked example: Reject a mismatched task result

One owner delivers a single outcome: the workspace checker reports a RESULT
whose task ID differs from its TASK. Model selection applies to this whole
subtask. The following observations are expectations for the plan.

| Step | Action | Expected observation |
| --- | --- | --- |
| T1.1 | In a disposable project copy, change only the current RESULT ID. Run the existing checker against the copy. | The old checker passes, reproducing the missed mismatch. If it already rejects the input, record that and reassess. |
| T1.2 | Compare the IDs from the dedicated task headings. Exercise the changed check with the same copy. | A failing check identifies the conflicting record. |
| T1.3 | Check a matching pair, an idle project, a task awaiting a RESULT and a record mentioning an old ID in prose. | Each valid case passes. |
| T1.4 | Record the commands, observed outcomes and revision for T1.1–T1.3. | The next host can locate the tested change and distinguish verified cases from any still open. |
