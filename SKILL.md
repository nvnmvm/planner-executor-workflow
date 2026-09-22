---
name: planner-executor-workflow
description: Route non-trivial coding tasks through high-capability planning and review plus lower-cost exploration, implementation, and tests; skip trivial edits.
---

# Planner–Executor Workflow

Use this skill to route coding work through the current Codex project's configured
`explorer` and `executor` agents. Keep model selection in the effective Codex
configuration; do not hardcode model IDs in task instructions.

## Before starting

Read the nearest applicable `AGENTS.md` and preserve its project-specific rules,
commands, and constraints. Inspect the repository only as much as needed to
classify the task and discover its existing validation commands.

## Route by the active model

Before delegating, determine the current primary model's role from runtime
metadata, the effective configuration, or documented model capabilities. Do not
infer capability from a guessed model name and do not invent model IDs.

- **High-capability primary:** keep the current model as Planner, Architect, and
  Reviewer. For Level 2–3 work, use the platform's supported delegation mechanism
  to create or select lower-cost, coding-capable agents for repository discovery,
  implementation, and tests. Default to one read-only Explorer and one
  workspace-writing Executor. Give each agent a bounded task and retain planning,
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

When assigning a child model, choose only from models confirmed available in the
current environment. Prefer the cheapest model that is adequate for code search,
ordinary edits, and test execution. If model overrides are unsupported, use the
configured `explorer` and `executor` roles without fabricating an override.

Classify the request before delegating:

- **Level 0 — trivial:** copy, formatting, one variable, or an obvious one-to-five-line edit. The primary agent works directly; do not spawn an agent.
- **Level 1 — simple:** a small single-file feature or obvious bug. The primary agent may send one precise task to `executor`, then perform a quick review.
- **Level 2 — normal:** a multi-file feature, API, data operation, integration, or ordinary refactor. Use the full flow below.
- **Level 3 — complex:** architecture, authentication, authorization, security, migrations, consistency, concurrency, protocol, caching, or large refactor. Use the full flow with deeper planning and conservative execution.

## Level 2–3 flow

### 1. Explore

When the high-capability primary route is active, create or select exactly one
lower-cost `explorer` to perform bounded, read-only discovery. It may search and
read files but must not edit files, make architecture decisions, or spawn other
agents. Ask for a concise report with only these sections:

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

The primary agent is Planner, Architect, and Reviewer. Use the user request and
the Explorer report to produce a mechanical handoff for `executor` containing:

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

### 3. Execute

Send the approved handoff to one lower-cost, coding-capable `executor`. It has
workspace-write permission and may create or modify only the files required by
the plan. It should make the smallest compatible change, add focused tests where
needed, and run the existing validation commands. It must not redesign the
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
- Preserve unrelated files and user data. Do not print or commit `.env` values or
  API keys; do not delete databases, run dangerous shell commands, or make broad
  destructive changes without an explicit plan and impact check.

If configured custom agents or platform delegation are unavailable, follow the
same stages directly with the primary agent or report the limitation clearly;
never claim that a delegated or high-capability review stage ran when it did not.

## Completion

Report the files changed, tests actually run and their results, remaining issues,
and any version-specific limitation. Keep the final response concise and
evidence-based.
