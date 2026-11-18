# Planner–Executor Workflow

English · [简体中文](README.zh-CN.md)

A **skill** is a set of instructions an AI assistant can load for a particular kind of task. This skill helps a coding assistant take a change from a request or an existing plan through implementation, tests, and review. It uses the tools and permissions already available in the host.

## When to use it

Use it for work where several steps must fit together, such as changing a parser, updating its callers, and verifying compatibility with existing inputs. The assistant identifies the files and acceptance criteria, assigns work where useful, and reviews the actual diff and test output. Its final report tells you what changed, which checks ran, and what remains unverified. A small edit can go straight to implementation.

If you already have a plan from another conversation, supply the plan or a readable file. The assistant checks its assumptions against the repository and picks up the ready tasks. [Plan handoff template](references/web-plan-handoff.md)

## How the hosts divide the work

| Where you start | Who does the work |
| --- | --- |
| Codex | Codex owns planning and final review. Execution stays within Codex by default. The primary can assign bounded work to an available lower-cost native model such as Luna. |
| DeepSeek Harness or OpenClaw | The host plans, executes, and reviews its own task. It may delegate to native subagents when available and authorized. |
| An assistant called by Codex via CLI | The assistant handles the assigned subtask and returns evidence to Codex. Delegation stops with that assistant. |

Project instructions and formal task plans take priority over the skill's defaults. Optional CLI helpers require a working CLI, an accessible model, and permissions appropriate to the subtask. [Workflow and CLI examples](SKILL.md)

## Request an external helper from Codex

Name the helper and the job in your prompt. Codex handles the CLI invocation and reviews the result.

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
