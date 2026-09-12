---
id: prompt-eval-business-idea-001
name: Evaluate Business Idea（评估创业想法）
version: 1.0
status: draft
category:
  - evaluator
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

> 对某个创业想法 / 商业分析输出做打分与批判。

# When to Use

- 评估 [[../Roles/startup-advisor|Startup Advisor]] 或其他商业分析的输出质量。

# Prompt

请评估下面这份关于创业想法的分析，按以下维度打分（每项 1–10）：

1. **Accuracy（准确）**：事实与数据是否可靠、有无编造。
2. **Depth（深度）**：是否深入，还是泛泛而谈。
3. **Clarity（清晰）**：结论是否明确、结构是否清楚。
4. **Actionability（可执行）**：是否给出可落地的下一步。
5. **Completeness（完整）**：是否遗漏关键维度（市场/竞争/商业模式/风险）。

然后给出：
- 总分（平均分）
- 最大问题（1 个）
- 缺失的关键信息（列出）
- 改进建议（3 条以内）

【被评估的内容】

# Output Schema

- 五维打分表
- 总分
- 最大问题 / 缺失信息 / 改进建议

# Changelog

- `v1.0` 初版（2026-09-12）
