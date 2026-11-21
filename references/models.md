# Optional CLI model examples

These are examples from prior configurations, not availability guarantees or defaults. Provider access, model names, aliases, and reasoning settings change. Check the installed CLI help and the user's current configuration before use; do not silently change credentials, routes, settings, or model selection. `AGENTS.md` and formal task routing override these examples.

| Helper | Example configuration | How to use |
| --- | --- | --- |
| Antigravity (`agy`) | `gemini-3.8-flash-medium` | Bounded search or planning; verify exact accepted model identifier and access in the active CLI. |
| DeepSeek Harness (`dsh`) | Aliyun `deepseek-v4.1-flash-1017`, max | Selected-input summaries, extraction, triage, or drafts; verify route and reasoning option in the active CLI. |

After any provider/model update, record the date, exact CLI version/help evidence, configured identifier, access check, and task-appropriate output verification here. Do not claim a route works based only on this example table.

On 2026-11-18, the Antigravity CLI listed the Gemini identifier above via `agy models`. The DeepSeek identifier comes from a configured provider route. Neither observation verifies an inference run. Refresh the list on model releases and verify a bounded task before promoting a new default.

The [model scorecard](model-scorecard.md) records planning candidates, benchmark settings and the November 21 connectivity checks. Use the task-specific planning choices in SKILL.md for the plan author.

## Runtime verification and routing labels

A model name in a CLI catalog is not evidence that the active session can call it
or that it has the tools and permissions needed for the task. Verify both at use
time. Record the actual callable model, or `UNKNOWN` if runtime metadata does not
reveal it. Do not change the primary model to satisfy a subtask.

- **PINNED** means the user authorized an exact model. Use it only if callable
  with the necessary permissions.
- **CAPABILITY** means the user specified capability/cost needs without an exact
  model. Select a callable model meeting them, and state which one or `UNKNOWN`.
- If the requested model is unavailable, use only an explicitly authorized
  fallback; otherwise pause the dependent subtask and report the limitation.

External CLI helpers require explicit authorization for the current task. General
preferences, installed tools, or available quota are not authorization. Do not
chain helpers or recursively delegate.


For bounded execution, use `gpt-6-luna` as a GPT example if the host offers it. For final review, name the user-selected or available GPT reviewer, such as `gpt-6.1-sol`. Confirm the runtime before reporting a review as complete. The Gemini and DeepSeek entries above are execution alternatives within the stated scopes.
