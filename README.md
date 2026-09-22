# Planner–Executor Workflow

Reusable Codex skill for routing non-trivial coding tasks through concise
exploration, explicit planning, bounded implementation, tests, and a final diff
review.

## Invoke

Use `$planner-executor-workflow` explicitly, or let Codex invoke it when a task
matches its description. Trivial one-to-five-line changes should stay direct.

## Workflow

```text
User → Explorer → Planner → Executor → Tests → Reviewer → Repair if necessary
```

The skill uses the current project's effective Codex configuration for model
routing. When the active primary model is confirmed to be high-capability, it
keeps planning and review while delegating search, ordinary implementation, and
tests to lower-cost coding agents. It does not embed a model ID, credentials, or
user-specific paths. When delegation or the configured `explorer`/`executor`
roles are unavailable, the primary agent follows the same stages directly and
reports that fallback.

## Install locally

Copy this directory to the user's Codex skills directory:

```sh
cp -R planner-executor-workflow ~/.codex/skills/
```

The exact directory is normally `$CODEX_HOME/skills/planner-executor-workflow`.

## Contents

- `SKILL.md` — the runtime instructions.
- `agents/openai.yaml` — the display name and invocation prompt used by Codex.

This repository contains instructions only. Keep project-specific `AGENTS.md`,
secrets, tokens, backups, and generated files outside it.
