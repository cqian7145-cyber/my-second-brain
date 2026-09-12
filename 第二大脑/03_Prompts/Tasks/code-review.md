---
id: prompt-task-code-review-001
name: Code Review（代码审查）
version: 1.0
status: draft
category:
  - task
  - coding
task:
  - code-review
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

> 对代码做结构化审查：正确性、可维护性、性能、安全。

# Inputs

- 代码（必填）。
- 上下文 / 相关文件（可选）。

# Prompt

请审查下面的代码，按优先级输出：

1. **Bug / 正确性**：会导致错误结果或崩溃的问题（最高优先级）。
2. **安全**：注入、越权、泄露敏感信息等。
3. **可维护性**：命名、重复代码、复杂度过高、可复用机会。
4. **性能**：明显的低效点。
5. **建议**：非必须但值得做的改进。

每条问题给出：`文件:行号`、问题描述、为什么是问题、修复建议。没有足够上下文时标注「待确认」。

【粘贴代码】

# Output Schema

- 问题列表（按优先级，每条含 文件:行号 / 问题 / 为什么 / 修复建议）
- 总体评价（1–2 句）

# Changelog

- `v1.0` 初版（2026-09-12）
