# Optional CLI model examples

These are examples from prior configurations, not availability guarantees or defaults. Provider access, model names, aliases, and reasoning settings change. Check the installed CLI help and the user's current configuration before use; do not silently change credentials, routes, settings, or model selection. `AGENTS.md` and formal task routing override these examples.

| Helper | Example configuration | How to use |
| --- | --- | --- |
| Antigravity (`agy`) | `gemini-3.8-flash-medium` | Bounded search or planning; verify exact accepted model identifier and access in the active CLI. |
| DeepSeek Harness (`dsh`) | Aliyun `deepseek-v4.1-flash-1017`, max | Selected-input summaries, extraction, triage, or drafts; verify route and reasoning option in the active CLI. |

After any provider/model update, record the date, exact CLI version/help evidence, configured identifier, access check, and task-appropriate output verification here. Do not claim a route works based only on this example table.

On 2026-11-18, the Antigravity CLI listed the Gemini identifier above via `agy models`. The DeepSeek identifier comes from a configured provider route. Neither observation verifies an inference run. Refresh the list on model releases and verify a bounded task before promoting a new default.
