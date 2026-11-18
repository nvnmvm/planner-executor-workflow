---
name: planner-executor-workflow
description: Plan, execute, and verify coding tasks in Codex, DeepSeek Harness, or OpenClaw. Use native subagents when appropriate and coordinate CLI helpers from Codex.
---

# Planner–Executor Workflow

First identify the active host from runtime metadata and its documented skill format. Apply only that host's execution rules below. Keep provider/model choices user-configured; project `AGENTS.md` and formal task files (`TASK.md` or equivalent) take priority over this general workflow.

## Shared rules

Read the nearest applicable `AGENTS.md`; preserve its technology, commands, scope, and constraints. Inspect only what is needed to identify relevant files, dependencies, and existing validation commands. Never invent test commands. For a supplied plan, read it first and verify only live facts needed for safe execution. If repository reality conflicts, stop the affected step and report the actual state and smallest adjustment.

Keep changes bounded, preserve unrelated data, and never expose secrets or modify `.git` internals. A delegation helper must never recursively delegate or invoke another AI helper. Do not claim a model, agent, review, or read-only guarantee that was not verified.

## Codex host

For native role handoffs, review, and repairs, follow [Codex agent workflow](references/codex-agents.md) subject to the current project rules. Codex may use its configured native agent/delegation flow when agents are available and authorized. Keep planning, difficult diagnosis, acceptance, and final review with the primary agent; assign bounded discovery or implementation tasks according to the project plan and configured roles. If agents are unavailable, do the work directly and report that fallback. For browser/desktop work, use the configured computer operator only when available; prefer a suitable connector for structured operations.

Codex alone may invoke the optional `agy` and `dsh` CLI helpers described below. Verify their actual availability, permissions, and configured provider/model before use; otherwise follow the authorized fallback, or report the blocked step if the task requires that helper. CLI helpers perform only their assigned bounded operation and never spawn agents or call other models/tools that invoke models.

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

The active host owns planning, implementation, validation, and final review. Its primary assistant may assign bounded work to native subagents when supported and authorized. Keep one writer per file, honor task dependencies, and review the results. Work directly when delegation is unavailable. Cross-platform CLI dispatch belongs to the Codex host. If the host's skill guidance differs, follow its documented runtime behavior while preserving project constraints.

## Complexity and completion

- **Level 0:** clear, trivial copy or one-to-five-line edit; act directly.
- **Level 1:** small single-file change; use the smallest suitable workflow.
- **Level 2:** ordinary multi-file change; plan, execute, validate, and review against project evidence.
- **Level 3:** genuine architecture, security, permission, consistency, migration, or concurrency risk; use the host's strongest supported planning/review path and conservative execution.

For coding tasks, establish Objective, Relevant files, Steps, Constraints, Acceptance criteria, and Tests before execution. Add focused tests when required by the plan and run only discovered or specified validation commands. Review the focused diff and actual command results. Report changed files, implementation, tests, results, remaining issues, and risks. If a repair is needed, make it narrow and validate it.

See [web plan handoff](references/web-plan-handoff.md) for a compact supplied-plan format and [model examples](references/models.md) for non-binding helper examples.
