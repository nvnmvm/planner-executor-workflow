# Planner–Executor Workflow · 规划技能

English · [简体中文](README.zh-CN.md)

A **skill** is a set of instructions an AI assistant can load for a particular kind of task. This skill helps a coding assistant take a change from a request or an existing plan through implementation, tests, and review. It uses the tools and permissions already available in the host.

## When to use it

Use it for work where several steps must fit together, such as changing a parser, updating its callers, and verifying compatibility with existing inputs. The assistant identifies the files and acceptance criteria, assigns work where useful, and reviews the actual diff and test output. Its final report tells you what changed, which checks ran, and what remains unverified. A clear, simple task explicitly requested by the user can go straight to implementation. For multi-step work, model delegation, or cross-host execution, the assistant first presents a plan in chat and waits for confirmation before implementation or publishing. Editing the proposal is not approval. A supplied plan skips replanning only when the user explicitly authorizes execution; a `READY`/`approved` label or the plan being pasted alone is not approval.

If you already have a plan from another conversation, supply the plan or a readable file. The assistant checks its assumptions against the repository and picks up the ready tasks. [Plan handoff template](references/web-plan-handoff.md)

## How the hosts divide the work

| Where you start | Who does the work |
| --- | --- |
| Codex | Codex plans and reviews its own work. Native delegation requires a callable model and suitable permissions. External CLI helpers require explicit authorization for this task. |
| DeepSeek Harness or OpenClaw | The active host performs its own workflow using its native capabilities; follow the plan approval gate above. |
| Codex-authorized CLI helper | Handles only its named bounded subtask and returns evidence. It never delegates recursively. |

When Codex acts as Executor under a plan from another Planner, that original Planner retains formal acceptance. Project instructions and formal task plans take priority over the skill's defaults. A model appearing in a catalog does not prove it is callable or has required tools/permissions. State model routing as PINNED (exact authorized model) or CAPABILITY (requirements), and report the actual model or UNKNOWN. Use only an explicitly authorized fallback; do not force a primary-model change. [Workflow and CLI examples](SKILL.md)

## Request an external helper from Codex

Name the helper and job explicitly for this task. Codex verifies access, uses only the authorized scope, and reviews the result.

- “Use Antigravity with Gemini Flash to look up the official documentation for this flag and give me the source.” Codex checks the model ID with `agy models` and passes it to `agy -p` via `--model`.
- “Use DeepSeek Harness with my configured Aliyun Flash model to extract the errors from this excerpt.” Codex selects the provider/model overlay with `dsh headless --patch` and sends the excerpt on standard input.

## Get started

For a first-time Codex installation on macOS or Linux:

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/nvnmvm/planner-executor-workflow.git "${CODEX_HOME:-$HOME/.codex}/skills/planner-executor-workflow"
```

In a Codex conversation, try:

> Use $planner-executor-workflow to update the parser to accept an empty field. Preserve existing inputs, run the relevant parser tests, and report any cases you could not verify.

For **OpenClaw**, place the skill folder at `<workspace>/skills/planner-executor-workflow/` and ask the workspace assistant to use it for the task. For **DeepSeek Harness**, place the repository where the session can read it and ask “Read `planner-executor-workflow/SKILL.md` and follow the standalone DeepSeek Harness workflow for this task.”

## Model updates

Keep provider-specific model IDs, reasoning settings, and dated verification evidence in the model reference. When a model changes, check what the CLI exposes and run a small representative task before changing a default. [Model reference](references/models.md)
