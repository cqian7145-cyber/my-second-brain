---
id: prompt-output-markdown-001
name: Markdown Output（Markdown 格式规范）
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

> 规范 AI 的 Markdown 输出，保证可读、可维护。

# When to Use

- 输出会进 Obsidian / 文档系统时。

# Prompt

请用规范的 Markdown 输出，遵循：

- 标题层级正确（`#` → `##` → `###`），不跳级。
- 列表、代码块、表格使用得当；代码块标注语言。
- 结论写在最前面，推导放在后面。
- 重点用 `**加粗**`，不用全大写或表情符号堆砌。
- 引用外部内容注明来源；引用本仓库笔记用 `[[wikilink]]`。

# Output Schema

- 规范的 Markdown。

# Changelog

- `v1.0` 初版（2026-09-12）
