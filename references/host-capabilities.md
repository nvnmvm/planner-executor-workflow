# Host capabilities / 宿主能力

**DOCUMENTED** = workflow described here / 本技能定义的用法。  
**REQUIRES_RUNTIME_CHECK** = verify access, tools and permissions on the active host / 以当前宿主实际能力为准。  
**NOT_BUILT_IN** = supplied by the host or external tooling / 由宿主或外部工具提供。

| Capability / 能力 | Codex | OpenClaw | DeepSeek Harness |
| --- | --- | --- | --- |
| Load or read SKILL.md / 读取技能 | DOCUMENTED · installed skill | DOCUMENTED · workspace skill | DOCUMENTED · read the file explicitly |
| Native model delegation and UI / 原生委派与图形操作 | REQUIRES_RUNTIME_CHECK | REQUIRES_RUNTIME_CHECK | REQUIRES_RUNTIME_CHECK |
| Cross-provider CLI dispatch / 跨厂商 CLI | REQUIRES_RUNTIME_CHECK · explicitly authorized helper | NOT_BUILT_IN · standalone workflow uses native tools | NOT_BUILT_IN · standalone workflow uses native tools |
| Transfer approved plan / 交接已批准计划 | DOCUMENTED · receiving session reads plan and checks access | DOCUMENTED · same | DOCUMENTED · same |
| Automatic dispatch or permission grants / 自动调度或授予权限 | NOT_BUILT_IN | NOT_BUILT_IN | NOT_BUILT_IN |

The skill defines instructions. It cannot grant tool access or send work automatically. If the required route fails, use an authorized fallback or report the blocked step. Keep the primary model. This matrix has no end-to-end runtime verification claims.

技能提供工作流程。工具、模型或权限不可用时，采用已授权的备用路线，或报告受阻步骤。保持当前主模型。此表未声明跨宿主实测通过。
