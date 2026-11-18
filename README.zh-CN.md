# Planner–Executor Workflow · 规划与执行工作流

[English](README.md) · 简体中文

**Skill（技能）是一套 AI 助手可以按需读取的工作说明。** 这个 Skill 用于编码任务。你可以给助手一个需求，也可以交给它一份已有计划。它会用当前平台已有的工具和权限完成修改、测试和审查。

## 什么时候用

例如，修改解析器时，你既要让它接受空字段，又要保证旧数据仍能读取。这个 Skill 会引导助手找到受影响的文件和调用方，明确怎样才算改好，按需分派子任务，最后检查实际改动和测试输出。你能从结果里看到改了什么、跑过哪些检查、还有哪些情况未验证。用户明确提出的简单任务可以直接动手。多步骤、模型委派或跨平台任务，要先在对话里给出可审阅的计划，等用户确认后再实施或发布。修改计划草稿不算确认；只有用户明确授权执行的计划才无需重复规划；计划里出现 READY/approved 字样或用户仅仅贴出计划，都不算批准。

如果已经在网页会话里写好了计划，把计划贴过来，或提供助手能读取的文件。助手会核对计划里的路径和依赖，从已具备条件的任务开始执行。[计划交接模板](references/web-plan-handoff.md)

## 三个平台怎么分工

| 从哪里发起 | 谁来完成 |
| --- | --- |
| Codex | Codex 负责规划。如果 Codex 按另一位 Planner 的计划担任 Executor，正式验收仍由原 Planner 负责。原生委派须确认模型可调用且权限合适；外部 CLI 须获得针对本任务的明确授权。 |
| DeepSeek Harness 或 OpenClaw | 当前平台用自身能力完成工作，并遵守上述计划确认要求。按 CAPABILITY 选择时，当前模型满足要求就继续使用。 |
| 经 Codex 授权的 CLI 助手 | 只完成指定的有限子任务并交回证据，不继续递归委派。 |

项目自身的规则和正式任务单优先。模型出现在列表中不代表当前会话能调用它，也不代表它拥有所需工具和权限。模型路由应标为 PINNED（已授权的准确型号）或 CAPABILITY（能力要求），并报告实际型号或 UNKNOWN。只能使用明确授权的备用方案，不要擅自更换主模型。[工作规则与 CLI 示例](SKILL.md)

## 怎样在 Codex 里点名外部助手

针对当前任务明确说出助手名称和任务范围。Codex 会先确认访问权限，只执行获准范围，并检查返回结果。

- “这一步用 Antigravity 的 Gemini Flash 查一下这个参数的官方文档，附上来源。” Codex 用 `agy models` 核对型号，再通过 `agy -p` 的 `--model` 参数指定模型。
- “用 DeepSeek Harness 的阿里云 Flash 模型，从这段日志里提取错误信息。” Codex 通过 `dsh headless --patch` 选择已配置的提供商和模型，把日志片段从标准输入传入。

## 开始使用

在 macOS 或 Linux 上首次为 **Codex** 安装时，运行：

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/nvnmvm/planner-executor-workflow.git "${CODEX_HOME:-$HOME/.codex}/skills/planner-executor-workflow"
```

然后在 Codex 会话里试一句：

> 用 $planner-executor-workflow 修改解析器，让它接受空字段，保持已有输入兼容。运行相关解析测试，并告诉我还有哪些情况没验证。

使用 **OpenClaw** 时，把技能目录放到 `<workspace>/skills/planner-executor-workflow/`，在对应工作区里让助手用它处理任务。使用 **DeepSeek Harness** 时，把仓库放在会话可读取的位置，然后告诉助手：“读取 `planner-executor-workflow/SKILL.md`，按 DeepSeek Harness 独立运行的流程完成这个任务。”

## 模型怎么更新

模型 ID、推理档位和带日期的验证记录集中维护在模型参考文件里。新模型发布后，先查 CLI 能识别的型号，再用一个有代表性的小任务验证调用结果，之后再决定是否更换默认模型。[模型参考](references/models.md)
