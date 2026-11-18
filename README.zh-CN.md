# Planner–Executor Workflow · 规划与执行工作流

[English](README.md) · 简体中文

**Skill（技能）是一套 AI 助手可以按需读取的工作说明。** 这个 Skill 用于编码任务。你可以给助手一个需求，也可以交给它一份已有计划。它会用当前平台已有的工具和权限完成修改、测试和审查。

## 什么时候用

例如，修改解析器时，你既要让它接受空字段，又要保证旧数据仍能读取。这个 Skill 会引导助手找到受影响的文件和调用方，明确怎样才算改好，按需分派子任务，最后检查实际改动和测试输出。你能从结果里看到改了什么、跑过哪些检查、还有哪些情况未验证。改一个拼写之类的小任务可以直接动手。

如果已经在网页会话里写好了计划，把计划贴过来，或提供助手能读取的文件。助手会核对计划里的路径和依赖，从已具备条件的任务开始执行。[计划交接模板](references/web-plan-handoff.md)

## 三个平台怎么分工

| 从哪里发起 | 谁来完成 |
| --- | --- |
| Codex | Codex 负责规划和最终验收。它可以调原生子智能体，也可以通过 `agy` 让 Antigravity 调研一个具体问题，或通过 `dsh headless` 让 DeepSeek Harness 从指定材料中提取信息、整理摘要。 |
| DeepSeek Harness 或 OpenClaw | 当前平台自己规划、执行和审查。平台支持且任务允许时，可以调用自己的原生子智能体。 |
| 由 Codex 通过 CLI 调起的助手 | 助手完成被分配的子任务，把结果和证据交回 Codex。派工到这一层结束。 |

项目自身的规则和正式任务单优先。要使用可选的 CLI 助手，当前环境需要能运行相应命令、访问所选模型，并具备执行子任务所需的权限。[工作规则与 CLI 示例](SKILL.md)

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
