---
id: prompt-eval-prompt-001
name: Evaluate Prompt（评估 Prompt 本身）
version: 1.0
status: draft
category:
  - evaluator
  - meta
task:
  - evaluate
models:
  - GPT
  - Claude
  - Gemini
  - DeepSeek
created: 2026-09-12
updated: 2026-09-12
quality_score: 
tags:
  - prompt
  - evaluator
---

# Goal

> 评估一个 Prompt 写得好不好，给出改进方向。

# When to Use

- 迭代 Prompt 时，判断当前版本质量。

# Prompt

请评估下面这个 Prompt 的质量，从以下维度打分（每项 1–10）：

1. **目标清晰**：是否一句话说清要什么。
2. **上下文充分**：是否给了足够背景与约束。
3. **结构合理**：是否拆分了角色 / 任务 / 输出等模块。
4. **可验证**：输出标准是否明确、可判断。
5. **精简**：有无冗余、含糊表述。

然后给出：
- 总分
- 最需要改的 1 处
- 改写建议（给出具体改法，直接给出优化后的版本）

【被评估的 Prompt】

# Output Schema

- 五维打分
- 总分
- 最需改进处 + 优化后的 Prompt 全文

# Changelog

- `v1.0` 初版（2026-09-12）
