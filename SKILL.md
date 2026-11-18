---
name: planner-executor-workflow
description: Plan, execute, and verify coding tasks across Codex, DeepSeek Harness, or OpenClaw. Present multi-step plans for confirmation before implementation or publishing; use a supplied approved READY plan directly.
---

# Planner–Executor Workflow

First identify the active host from runtime metadata and its documented skill format. Apply only that host's execution rules below. Keep provider/model choices user-configured; project `AGENTS.md` and formal task files (`TASK.md` or equivalent) take priority over this general workflow.

## Shared rules

Read the nearest applicable `AGENTS.md`; preserve its technology, commands, scope, and constraints. Inspect only what is needed to identify relevant files, dependencies, and existing validation commands. Never invent test commands. For a supplied plan, read it first and verify only live facts needed for safe execution. If repository reality conflicts, stop the affected step and report the actual state and smallest adjustment.

Keep changes bounded, preserve unrelated data, and never expose secrets or modify `.git` internals. A delegation helper must never recursively delegate or invoke another AI helper. Do not claim a model, agent, review, or read-only guarantee that was not verified.

## Planning and approval boundary

A clear, simple task explicitly requested by the user can proceed directly. For multi-step work, model delegation, or cross-host execution, prepare a reviewable plan in chat before implementation or publishing and wait for the user's confirmation. Include the objective, affected files/outputs, subtask table, dependencies, acceptance checks, validation commands, risks, and intended external actions. The table should state for each subtask its difficulty and why, required model or capability, estimated cost, tool, dependencies, and validation. Mark uncertain capabilities or costs as unknown instead of guessing. This shared format applies on Codex, DeepSeek Harness, and OpenClaw:

| Subtask | Difficulty and reason | Model routing (PINNED/CAPABILITY; actual or UNKNOWN) | Cost | Tool/host | Depends on | Validation |
| --- | --- | --- | --- | --- | --- | --- |
| T1: inspect affected parser and callers | Level 2: several files and compatibility impact | CAPABILITY: code-capable; actual model or UNKNOWN | Low/unknown | Native workspace tools | None | Existing parser tests and caller review |

Editing a proposal, choosing a model, or returning a revised draft is not approval. Begin implementation or publishing only after an unambiguous confirmation. A supplied, complete plan may proceed without repeating the planning step only when the user explicitly authorizes that plan for execution (for example, says to execute it). A `READY` or `approved` label in a plan, or its presence in the task, is not by itself user approval. Verify only live facts needed for safe execution. Carry the user’s recorded confirmation, approved revision or plan summary, and scope in the handoff. In a GitHub handoff, confirmation to execute the presented plan authorizes posting its task sheet and executing its listed steps. An execution host reading that approval proceeds directly. If authorization is unclear, ask before dependent work. Formal project task routing and `AGENTS.md` remain authoritative within their scope.

## Codex host

For native role handoffs, review, and repairs, follow [Codex agent workflow](references/codex-agents.md) subject to the current project rules. Verify that a native model is callable in this session and has the needed tool/permission access; appearing in a model list alone is insufficient. For locally owned work keep planning and review with the primary agent; when acting as Executor, verify the output and return it to the designated Planner for acceptance; assign bounded tasks only when authorized. If agents are unavailable, follow the approved fallback exactly or report the block. For browser/desktop work, use the configured computer operator only when callable and permitted; prefer a suitable connector for structured operations.

Default to the current host's native tools and models. For CAPABILITY routing, use the current host/model when it meets the requested capability and permissions; do not switch the primary model just to match a label. Invoke an external helper only when the user explicitly names and authorizes it for this task. Do not treat general preferences, installation, quota, or availability as authorization. Use only the named helper and approved scope; never chain helpers or recursively delegate. See [model routing and verification](references/models.md).

### Optional Antigravity planning helper

For a bounded planning/search task only, the documented invocation is:

```sh
agy -p "$task" --model "$model" --mode plan --sandbox --output-format json --print-timeout 60s
```

`--mode plan` and `--sandbox` do not prove read-only behavior. Inspect the CLI's current help and sandbox semantics, constrain the prompt to planning, and do not grant write/delegation steps. Treat output as a draft for Codex to verify.

### Optional DeepSeek Harness helper

For bounded work on selected input, the documented invocation is:

```sh
timeout 60s dsh headless --patch "$patch" --permission-mode read-only --approval-policy never - < task.txt
```

Use a task-specific outer timeout suitable for the job. The example uses GNU timeout. On hosts without it, set the deadline in the process runner. After timeout, verify backend termination before reassigning any writable work. Confirm the installed CLI's flags and route first. `--permission-mode read-only` constrains the helper session; the outer environment remains authoritative. Feed only the minimum selected input, inspect the result, and do not let the helper execute, delegate, or invoke another AI.

## Standalone DeepSeek Harness or OpenClaw host

The active host owns planning, implementation, validation, and final review. Use only native models/subagents actually callable in that session with the required tools and permissions. Presence in a model catalog is not proof of access. Label model selection as PINNED when an exact user-authorized model is specified, or CAPABILITY when choosing among models that meet a stated capability. For CAPABILITY, use the current host/model if it meets the requirement; this applies equally to DeepSeek Harness and OpenClaw. For either, report the actual callable model or `UNKNOWN`; do not force a primary-model change. A lower-capability request may use only an explicitly authorized fallback; otherwise report the limitation. Follow the planning and approval boundary above. Cross-platform CLI dispatch belongs to Codex. Do not invoke external helpers or recursively delegate.

## Complexity and completion

- **Level 0:** clear, trivial copy or one-to-five-line edit; act directly.
- **Level 1:** small single-file change; use the smallest suitable workflow.
- **Level 2:** ordinary multi-file change; plan, execute, validate, and review against project evidence.
- **Level 3:** genuine architecture, security, permission, consistency, migration, or concurrency risk; use the host's strongest supported planning/review path and conservative execution.

For coding tasks, establish Objective, Relevant files, Steps, Constraints, Acceptance criteria, and Tests before execution. Add focused tests when required by the plan and run only discovered or specified validation commands. Review the focused diff and actual command results. Report changed files, implementation, tests, results, remaining issues, and risks. If a repair is needed, make it narrow and validate it.

See [web plan handoff](references/web-plan-handoff.md) for a compact supplied-plan format and [model examples](references/models.md) for non-binding helper examples.
