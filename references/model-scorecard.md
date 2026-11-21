# 模型测评与任务分配 · 2026-11-21

覆盖当前 Codex 提供的 GPT 原生代理型号、本机 Antigravity 列出的 Gemini 系列及当前 DeepSeek 工作流，附部分对照型号。不同推理档位单独标明。此表用于选模型，未穷举三家历史 API 快照、专用生成模型和退役版本。

## 同场成绩

Argo-Bench v1.4.3 让模型完成 210 个企业数据任务。下列分数是平均任务得分，满分 100。榜单给出的 95% 置信区间约为 ±2 至 8 分，小差距不足以确定排名。它同时考验分析、工具使用与执行，不能单独量出规划能力。[原始榜单](https://argo-bench.com/)

|模型|推理档位|得分|
|---|---|---:|
|Gemini 4 Neon|High|64.4|
|GPT-6.2 Astra|Extra high|62.6|
|Gemini 4 Carbon|High|62.2|
|GPT-6.2 Sol|Extra high|58.6|
|Gemini 4 Argon|High|53.7|
|GPT-6 Astra|Extra high|50.3|
|GPT-6.1 Sol|Extra high|47.4|
|GPT-6.1 Terra|Extra high|44.0|
|GPT-6 Sol|Extra high|34.7|
|DeepSeek V4.1 Flash 1017|Max|31.3|
|Gemini 3.8 Flash|High|24.4|
|GPT-6 Luna|Extra high|12.8|

## 其他测评和覆盖缺口

- Gemini 4 Hydrogen 在 Agents’ Last Exam v1.1 知识工作测试为 71.6%，GPT-6.1 Terra 为 69.8%。Hydrogen 使用 Antigravity CLI，默认最高思考档。Terminal-Bench 4.1 测试用 High，成绩 57.0%。这些是不同题集的原始成绩。[Google 评测方法和成绩](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_hydrogen_model_evaluation.pdf)
- GPT-5.6 Sol、Terra、Luna 在 SWE-Bench Pro 软件工程测试分别为 64.6%、63.4%、62.7%。适合用来判断旧型号的代码执行能力。[OpenAI 成绩表](https://openai.com/index/gpt-5-6/)
- DeepSeek V4-Pro-0813 是另一公开型号，本机工作流只配置了 1017。旧 V4 Flash 的兼容名称可能映射到新型号。[DeepSeek 更新记录](https://api-docs.deepseek.com/updates/)
- GPT-OSS 120B Medium、Neon 和 Argon 两个 Orchestrator 编排入口缺少本表可直接比较的规划分。编排入口可能调用多个模型，不能继承底层模型的分。Carbon 本机未列出。前次仅对 Neon 编排入口、Gemini 四个普通系列的 Medium 和 GPT-OSS 做过单轮连通测试，没有对其进行项目规划验收。

## 建议采用的分配规则

按任务给适配分。3 表示首选候选，2 表示可用备选，1 表示只承担局部工作。评分是结合公开成绩、工作流和工具条件给出的调度判断。同分时看项目资料是否已齐全、需要哪些工具、实际可用档位和预算。

|任务|3 分|2 分|选择依据|
|---|---|---|---|
|L0–1 摘要、字段提取、整理给定材料|DeepSeek V4.1 Flash 1017、Gemini 3.8 Flash|GPT-6 Luna|按材料和现有入口选，简单改动可由主代理直接完成|
|L1–2 需求清楚，拆步骤、依赖和验收条件|Gemini 4 Hydrogen、GPT-6.2 Sol|Gemini 4 Neon、GPT-6.1 Sol|有完整材料可交 Hydrogen 独立起草；需反复检查本地代码时选有仓库工具的 Sol|
|L2 多约束、跨模块设计或复杂数据工作流|Gemini 4 Neon、GPT-6.2 Sol|Gemini 4 Hydrogen、Gemini 4 Argon|用输入输出、依赖、失败恢复及验证路径判断计划是否可执行|
|L2 按批准方案查仓库、改代码和运行测试|GPT-6 Luna|GPT-6.2 Sol|Luna 已配置执行角色，需结合项目命令验收|
|L3 权限、迁移、并发一致性|项目指定 GPT Astra 规划与终审|Gemini 4 Neon 提供独立方案|普通模型可做 1 分的局部资料整理；遵循项目对 Astra 具体型号的要求|
|必须操作图形界面|已配置的 GPT-6.1 Terra|有对应能力且项目明确授权的模型|能实际操作界面是前提|

Gemini 普通规划可从已验证可调用的 gemini-4-hydrogen-medium 或 gemini-4-neon-medium 开始。需要更高档位时核对 agy models，榜单 High 成绩不能套给 Medium。实际规划效果宜根据项目验收记录修订，关注遗漏依赖、验收条件可执行性、返工次数。GPT 最终验收与计划起草可由不同模型负责。
