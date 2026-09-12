---
id: prompt-task-market-research-001
name: Market Research（市场调研）
version: 1.0
status: draft
category:
  - task
  - research
task:
  - market-analysis
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
  - task
---

# Goal

> 对一个市场做结构化调研：规模、趋势、人群、机会。

# Inputs

- 目标市场 / 产品 / 行业（必填）。
- 地域、预算等约束（可选）。

# Prompt

请对【填入主题】做一次市场调研，输出结构化报告：

1. **市场规模与增长**：TAM / SAM / SOM，增长趋势，数据来源。
2. **目标人群**：画像、需求、痛点、付费能力与意愿。
3. **趋势**：正在发生的结构性变化（技术 / 政策 / 消费习惯）。
4. **机会点**：3 个最值得切入的空隙。
5. **不确定性**：哪些结论是假设，需要进一步验证。

所有数据尽量给来源；没有可靠数据就标注「估算 / 待验证」。

# Output Schema

- 执行摘要（3 句）
- 五个分节（如上）
- 待验证假设清单

# Changelog

- `v1.0` 初版（2026-09-12）
