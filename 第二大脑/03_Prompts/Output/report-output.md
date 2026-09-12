---
id: prompt-output-report-001
name: Report Output（报告格式）
version: 1.0
status: production
category:
  - output
task:
  - output-format
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
  - output
---

# Goal

> 让 AI 输出结构化的报告，方便阅读与归档。

# When to Use

- 需要一份可读、可复用的分析报告时。

# Prompt

请按以下结构输出一份报告（Markdown 格式）：

1. **执行摘要**：3 句以内说清结论。
2. **背景**：问题和目标。
3. **分析**：分节展开，用标题、列表、表格增强可读性。
4. **结论**：明确的结论与理由。
5. **建议 / 下一步**：可执行的行动项（编号）。

要求：结论先行，数据给来源，不确定处标注「假设 / 待验证」。

# Output Schema

- Markdown 报告，含上述 5 节。

# Changelog

- `v1.0` 初版（2026-09-12）
