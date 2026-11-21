---
name: planner-executor-workflow
description: Plan, execute, and verify coding tasks across Codex, DeepSeek Harness, or OpenClaw. Present multi-step plans for confirmation before implementation or publishing; use a supplied approved READY plan directly.
---

# Planner–Executor Workflow

Follow the active host’s rules, the nearest applicable `AGENTS.md` and the approved project task. Preserve scope, unrelated data, secrets and Git internals. Helpers must not delegate or invoke another AI helper.

## Plan and approval

Handle a clear, explicitly requested simple task directly. For multi-step work, delegation or cross-host execution, present the plan in chat and wait for explicit approval before implementation or publication. Editing the draft, selecting a model or marking a task READY does not authorize execution. Carry the approved revision, scope and confirmation into the handoff. The recipient proceeds after checking live prerequisites. If evidence conflicts with the plan, pause the affected step and report the smallest adjustment.

State the goal, affected files, constraints, risks and intended external actions. Use one row and one owner per independently acceptable outcome. Keep its discovery, implementation and validation together. Use the [handoff template](references/web-plan-handoff.md).

| Task / owner | Difficulty and reason | Preferred model and reason | Cost | Tool / host | Dependencies | Acceptance and validation |
| --- | --- | --- | --- | --- | --- | --- |

Give each step an ID, action and expected observation. Use bounded discovery for unknown files or test commands. Give one exact recommended model per task and state its known availability. Add a fallback only when useful, with its switching condition. Compare all three providers when requested. Mark unverified costs, access or runtime IDs as unknown. Use PINNED for an explicitly selected model and CAPABILITY for a capability requirement. Before dispatch, verify tools and access and use only authorized routes.

## Codex

For routine plans, choose one author by task. Hydrogen (`gemini-4-hydrogen-medium`) suits clear requirements, dependencies and acceptance checks. Neon (`gemini-4-neon-medium`) suits cross-module decisions and complex data workflows. Sol (`gpt-6.2-sol`) suits either, especially when planning requires repeated repository inspection. The selected author can draft the complete plan. The primary verifies repository assumptions and presents it. For Level 3, the designated GPT Astra handles difficult planning and review. An authorized Neon can offer a second proposal. See the [model scorecard](references/model-scorecard.md).

Prefer suitable native tools. Use an external CLI only with explicit authorization for that helper and task. For CLI commands, permission checks and timeout handling, consult the [model guide](references/models.md). Preserve the session’s primary model. Use a suitable connector for structured operations and a permitted computer operator for UI work. Follow the [Codex workflow](references/codex-agents.md) for delegation and repair.

Name a separate final GPT reviewer by exact model ID. If the reviewer is unavailable, return evidence to the designated GPT Planner and mark acceptance pending. When Codex is the Executor for another Planner, return results to that Planner for acceptance.

## Standalone DeepSeek Harness or OpenClaw

The host owns planning, implementation, validation and final review using its native models and agents. For capability-based tasks, use the current model when suitable. Cross-platform CLI dispatch belongs to Codex.

## Complexity and verification

- **Level 0:** clear, trivial copy or one-to-five-line edit; act directly.
- **Level 1:** small single-file change; use the smallest suitable workflow.
- **Level 2:** ordinary multi-file change; plan, execute, validate, and review against project evidence.
- **Level 3:** genuine architecture, security, permission, consistency, migration, or concurrency risk; use the host's strongest supported planning/review path and conservative execution.

At each task boundary, record observed results, evidence and outstanding work. Use existing project validation commands. For bug fixes, compare the same reproducer before and after when feasible. Add intermediate GPT review only for concrete high risk. At final review, check approved scope and requirements, then the focused diff, tests and regressions. Report changed files, actual results, skipped checks and remaining risks.

Lead with the result and verification, explain unfamiliar terms on first use and make the final report self-contained.
