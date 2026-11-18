# Planner–Executor Workflow

Use this skill to route coding work through the current Codex project's configured
`explorer` and `executor` agents. Keep model selection in the effective Codex
configuration; do not hardcode model IDs in task instructions.

## Before starting

Read the nearest applicable `AGENTS.md` and preserve its project-specific rules,
commands, and constraints. Inspect the repository only as much as needed to
classify the task and discover its existing validation commands.

## When a plan was prepared on the web

If the user pastes or attaches a web plan, supplies an accessible link, or points
to a local plan file, read that handoff first. The local Codex task cannot see an
unrelated web conversation automatically. Use the plan's task IDs, agent roles,
file ownership, dependencies, acceptance criteria, and tests as the dispatch
queue. See [web plan handoff](web-plan-handoff.md) for a compact
format to request from the web planner.

Verify only the live repository facts needed to execute safely. When the plan
is complete and current, dispatch its ready tasks without repeating Explorer
or planning. Use Explorer for a specific missing fact or stale path; revise only
the affected task if the repository conflicts with the plan. Map requested
roles and models to agents actually available here, keep shared-file edits
serial, wait for results, and review the diff and tests before accepting a task.
The plan does not grant extra permissions or authorize unrelated changes.

## Route by the active model

Before delegating, determine the current primary model and its role from runtime
metadata or effective configuration. Verify candidate native models are callable
in this session and that the required tools and permissions are available; catalog
listing is not proof. Do not infer capability from a guessed model name or invent
model IDs. Preserve the current primary model unless the user explicitly requests
a change. Report the actual model, or `UNKNOWN` when the runtime does not expose it.

- **High-capability primary:** keep the current model as Planner, Architect, and
  Reviewer. For Level 2–3 work, use the platform's supported delegation mechanism
  to create or select lower-cost, coding-capable agents for repository discovery,
  implementation, and tests. Default to one behaviorally read-only Explorer and
  one Executor that may edit only when the parent session's effective permissions
  allow it; role instructions constrain behavior and do not grant tool access.
  Give each agent a bounded task and retain planning,
  difficult diagnosis, acceptance decisions, and final review in the primary
  model.
- **Lower-cost primary:** do not claim that a high-capability review occurred. If
  the environment exposes a configured high-capability planner, escalate the
  planning or final review to it. Otherwise execute the smallest safe workflow
  directly, keep changes bounded, and disclose that the high-capability review
  stage was unavailable.
- **Unknown capability:** preserve the effective configuration and use the normal
  project defaults. Ask for clarification only when choosing a model would cause
  material cost or behavior differences that cannot be resolved from the
  environment.

Mark model routing as **PINNED** when the user authorizes an exact model, and use
that model only if callable with required tools/permissions. Mark it **CAPABILITY**
when the user specifies capability and cost constraints but no exact model; choose
a callable model that meets them. State the actual chosen model or `UNKNOWN`. For
a requested model unavailable in-session, use only the user-authorized fallback;
otherwise pause that subtask and report the block. If overrides are unsupported,
use configured native roles only when callable and permitted; do not silently
change the primary model.

For a bounded task that needs visual browser or desktop interaction, assign the
UI steps to the configured `computer_operator` role when it is available. Keep
planning and review with the primary agent when the request also involves code
or difficult judgment. The UI agent inherits available tools and app permissions;
its role does not enable computer use by itself. Prefer a suitable connector or
API for structured operations, and do not spawn a UI agent for tasks without
screen interaction.

Classify the request before delegating:

- **Level 0 — trivial:** copy, formatting, one variable, or an obvious one-to-five-line edit. The primary agent works directly; do not spawn an agent.
- **Level 1 — simple:** a small single-file feature or obvious bug. The primary agent may send one precise task to `executor`, then perform a quick review.
- **Level 2 — normal:** a multi-file feature, API, data operation, integration, or ordinary refactor. Default to the full flow below. If current evidence confirms the relevant files, call chain, and validation commands and none has changed, reuse it and explain why exploration can be skipped; fill only the evidence gaps that remain.
- **Level 3 — complex:** work with actual risk to architecture, permission boundaries, security, consistency, data migrations, or concurrency semantics. Assign this level based on the risk, not keywords such as cache, protocol, or authentication. Small changes following an established pattern stay at the level their actual complexity warrants. For genuine Level 3 work, use deeper planning and conservative execution, with Astra for difficult planning and review.

## Level 2–3 flow

### 1. Explore

When no sufficient supplied plan exists and exploration is needed, create
or select one lower-cost `explorer` for bounded, read-only discovery. It may search
and read files but must not edit files, make architecture decisions, or spawn other
agents. Reuse current evidence and explore only missing or changed areas. Ask for a
concise report with only these sections:

```text
Relevant files
Important symbols
Execution flow
Dependencies
Risks
Likely modifications
```

Prefer paths, symbol names, short excerpts, and evidence over full-file copies.

### 2. Plan

The primary agent is Planner, Architect, and Reviewer when it must plan locally.
Use the user request and Explorer report to produce a mechanical handoff for
`executor` containing:

```text
Objective
Relevant files
Steps
Constraints
Acceptance criteria
Tests
```

Name the target files and symbols, inputs and outputs, error behavior, affected
callers, tests, and observable acceptance conditions. Do not repeat a repository-
wide search already covered by Explorer. Select test, lint, typecheck, and build
commands from the project's manifests, task runners, documentation, and CI;
never invent unrelated commands.

Keep handoffs to necessary task context, paths and symbols, short evidence,
constraints, and acceptance criteria. When supported, pass the minimum necessary
inherited context; fork full history only when the task depends on it. For the
same task, prefer reusing the original agent in the same role; do not force reuse
across roles.

### 3. Execute

Send the approved handoff to one lower-cost, coding-capable `executor`. Tool
access inherits the parent session's effective permissions; role instructions
constrain behavior and do not grant permissions. The Executor may edit only when
parent permissions allow it, and only within the plan scope. It should make the
smallest compatible change, add focused tests where needed, and run existing
validation commands. It must not redesign the
architecture, broaden scope, replace the technology stack, alter unrelated
files, expose secrets, or modify `.git` internals.

If the plan cannot be executed as written, Executor stops the affected change
and reports the blocked step, cause, verified project state, and smallest
recommended adjustment instead of improvising a new design.

Executor's completion report must contain:

```text
Files changed
Implementation summary
Tests
Results
Remaining issues
Risks
```

Reports must name commands actually run and their results, plus commands not run
or environmental blockers. A report is evidence, not acceptance; the primary
agent still reviews the result before accepting completion.

### 4. Validate and review

The primary agent reviews the original request, plan, Executor summary, `git
status`, focused `git diff`, reported test output, and key changed files. Check
requirement coverage, omissions, regressions, security, unintended files,
compatibility, error handling, and test adequacy. Do not reread the entire
repository unless the evidence is contradictory or incomplete.

### 5. Repair

When review finds a defect, send a narrow task to the same Executor using:

```text
Defect
Expected
File
Required change
Validation
```

Allow at most two ordinary repair loops. After a second failure, the primary
agent decides whether to replan or take over only the genuinely difficult part.

## Concurrency, cost, and safety

- Default to one Explorer and one Executor. Parallelize only fully independent
  read-only or edit tasks with disjoint file ownership.
- Never let two Executors edit the same file, lockfile, migration, shared
  configuration, or tightly coupled module concurrently. Serial execution is the
  default.
- Keep high-capability reasoning for requirements, architecture, diagnosis,
  planning, and review. Keep search, reading, mechanical edits, ordinary tests,
  and documentation with the lower-cost agents.
- Pass paths, symbols, short evidence, diffs, errors, and test results instead of
  full source files. Do not repeat Explorer's search or Executor's summary.
- For multi-step delegation, track task IDs, one owner per task, dependencies,
  completion conditions, and status in the existing plan. Dispatch only tasks
  whose dependencies are satisfied. An idle/done state or queued-message receipt
  is not acceptance: wait for the result and have the primary agent review it.
  After interruption or timeout, do not transfer file ownership automatically;
  confirm the prior worker stopped, reread the relevant diff and current state,
  then make an explicit handoff. Use event-based waiting when supported instead
  of polling. If waiting is unavailable, record status in the existing plan; do
  not claim a persistent task board, CAS, or file locks. Keep short tasks
  lightweight; shared-file edits remain serial under the existing concurrency
  limit.
- Preserve unrelated files and user data. Do not print or commit `.env` values or
  API keys; do not delete databases, run dangerous shell commands, or make broad
  destructive changes without an explicit plan and impact check.

If configured custom agents or platform delegation are unavailable, follow the
same stages directly with the primary agent or report the limitation clearly;
never claim that a delegated or high-capability review stage ran when it did not.

## Coordination references

Coordination principles only are informed by the
[agent-team profile](https://github.com/deepseek-ai/deepseek-harness/tree/master/packages/experimental/agent-team-profile),
[agent-team README](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/experimental/agent-team/README.md),
and [tool-agent-team README](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/experimental/tool-agent-team/README.md).

## Completion

Report the files changed, tests actually run and their results, remaining issues,
and any version-specific limitation. Keep the final response concise and
evidence-based.


## Plan approval gate

For multi-step work, model delegation, or cross-host dispatch, the primary first
presents a chat plan and waits for user confirmation before implementation or
publishing. A revised proposal is not confirmation. A complete supplied plan skips replanning only when the user explicitly
authorizes execution. A `READY`/`approved` label or plan presence alone is not
approval. Verify current paths, access, and commands, then start authorized ready
tasks. A clear simple task explicitly
requested by the user may proceed directly. The plan includes a subtask table:
objective, difficulty and rationale, required model/capability, cost, tool,
dependencies, and validation, plus affected files, acceptance, risks, and any
external action. Use `UNKNOWN` for unverified model/cost/access.
