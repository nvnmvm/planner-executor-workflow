# 规划技能 · Planner–Executor Workflow

[English](README.md) · 简体中文

**Skill（技能）是一套 AI 助手可以按需读取的工作说明。** 这个 Skill 用于编码任务。你可以给助手一个需求，也可以交给它一份已有计划。它会用当前平台已有的工具和权限完成修改、测试和审查。

## 实际场景：给个人网站加深色模式

你在 Codex 里说「用规划技能规划一下给个人网站加深色模式」。助手先给出方案，等你确认：

| 子任务 | 难度 | 建议模型或能力 | 验收 |
| --- | --- | --- | --- |
| 找出颜色定义和主题入口 | 低 | 可读仓库的低成本原生模型 | 列出相关文件和现有主题逻辑 |
| 实现切换并保存偏好 | 中 | 能修改前端并运行测试的原生模型 | 切换生效，刷新后保留选择 |
| 检查两种主题的可读性 | 中 | 能在浏览器验证的执行端 | 链接、文本和焦点状态清晰可见 |

说「按方案执行」可以在当前会话开工。如果想换到另一台电脑执行，就说「按这个方案执行，上传到项目闭环」，把确认版交给相应项目的执行端。[项目闭环](https://github.com/nvnmvm/ai-xiangmu-bihuan) · [计划交接模板](references/web-plan-handoff.md)

这份示例可用 `gpt-6-luna` 承担范围明确的执行项。确认后也可用 `gemini-3.8-flash-medium` 整理主题线索，或用 `deepseek-v4.1-flash-1017` 从选定材料提取验证摘要。浏览器检查由具备相应工具的执行端完成，最终由 GPT 审查改动和证据。

## 安装与快速开始

需要先安装 Git 和你准备使用的助手。下面的克隆命令用于首次安装。已经通过 Git 安装的用户可在对应目录运行 `git pull --ff-only` 更新。

### Codex

在对话里发「用 `$skill-installer` 安装 GitHub 仓库 `nvnmvm/planner-executor-workflow` 根目录的 Skill」。也可以用下面的终端命令。安装后重新打开 Codex，输入 `$planner-executor-workflow` 试用。

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

进入当前助手的工作区，把仓库克隆到下面的位置。安装后，在该工作区发「用 planner-executor-workflow 规划这个任务」。

```sh
git clone https://github.com/nvnmvm/planner-executor-workflow.git skills/planner-executor-workflow
```

### DeepSeek Harness

把仓库克隆到当前会话可读取的位置，然后发「读取 `planner-executor-workflow/SKILL.md`，按 DeepSeek Harness 独立运行的流程规划这个任务」。

安装写法参考 [OpenAI Skills](https://github.com/openai/skills#installing-a-skill) 和 [OpenClaw 工作区技能指南](https://docs.openclaw.ai/tools/skills)。

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

## 模型怎么更新

模型 ID、推理档位和带日期的验证记录集中维护在模型参考文件里。新模型发布后，先查 CLI 能识别的型号，再用一个有代表性的小任务验证调用结果，之后再决定是否更换默认模型。[模型参考](references/models.md)
