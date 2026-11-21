# Planner–Executor Workflow · 规划技能

For ordinary plans, Hydrogen or GPT-6.2 Sol can turn clear requirements into steps and acceptance checks. Neon or Sol can plan work with cross-module dependencies. The designated GPT Astra handles high-risk planning, and GPT owns final review. [Model scores and task routing](references/model-scorecard.md)

English · [简体中文](README.zh-CN.md)

A **skill** is a set of instructions an AI assistant can load for a particular kind of task. This skill helps a coding assistant take a change from a request or an existing plan through implementation, tests, and review. It uses the tools and permissions already available in the host.

## Example: Add dark mode to a personal website

Ask Codex to “Use the Planner–Executor Skill to plan dark mode for my website.” It presents a plan for your approval:

| Subtask | Difficulty | Suggested model or capability | Acceptance check |
| --- | --- | --- | --- |
| Locate color definitions and theme entry points | Low | Low-cost native model with repository access | Identify the relevant files and existing theme logic |
| Add a toggle and persist the preference | Medium | Native model that can edit the frontend and run tests | Switching works and survives a reload |
| Check readability in both themes | Medium | Execution host with browser tools | Links, text, and focus states are clearly visible |

Say “Execute this plan” to start in the current session. To hand it off, say “Execute this plan and upload it to 项目闭环” so the next host can read the confirmed task. [Project workspace](https://github.com/nvnmvm/ai-xiangmu-bihuan) · [Plan handoff template](references/web-plan-handoff.md)

For the implementation, recommend `gpt-6-luna` for a bounded edit with existing tests. Add a fallback only when a concrete switching condition applies. GPT reviews the result.

## Installation and quick start

Install Git and the assistant you intend to use. The clone commands below are for a first install. Update an existing Git installation with `git pull --ff-only` from its skill directory.

### Codex

Ask Codex to “Use `$skill-installer` to install the root skill from `nvnmvm/planner-executor-workflow`,” or run the terminal commands below. Reopen Codex after installation and try `$planner-executor-workflow`.

**macOS / Linux**

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/nvnmvm/planner-executor-workflow.git "${CODEX_HOME:-$HOME/.codex}/skills/planner-executor-workflow"
```

**Windows PowerShell**

```powershell
$codexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $HOME '.codex' }
$skillDirectory = Join-Path $codexRoot 'skills'
New-Item -ItemType Directory -Force -Path $skillDirectory | Out-Null
git clone https://github.com/nvnmvm/planner-executor-workflow.git (Join-Path $skillDirectory 'planner-executor-workflow')
```

### OpenClaw

From the active assistant workspace, clone the repository as shown below. Then ask the assistant in that workspace to use `planner-executor-workflow`.

```sh
git clone https://github.com/nvnmvm/planner-executor-workflow.git skills/planner-executor-workflow
```

### DeepSeek Harness

Clone the repository to a location accessible to the session. Ask it to “Read `planner-executor-workflow/SKILL.md` and plan this task using the standalone DeepSeek Harness workflow.”

Installation references: [OpenAI Skills](https://github.com/openai/skills#installing-a-skill) and the [OpenClaw workspace skills guide](https://docs.openclaw.ai/tools/skills).

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

## Model updates

Keep provider-specific model IDs, reasoning settings, and dated verification evidence in the model reference. When a model changes, check what the CLI exposes and run a small representative task before changing a default. [Model reference](references/models.md)
