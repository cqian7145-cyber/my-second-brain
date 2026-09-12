---
id: prompt-meta-generator-001
name: Prompt Generator（从任务生成 Prompt）
version: 1.0
status: draft
category:
  - meta
task:
  - prompt-generation
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
  - meta
---

# Goal

> 把一个需求描述，自动生成结构化、可复用的 Prompt 资产。

# Inputs

- 你要完成的任务（必填）。
- 期望输出、约束（可选）。

# Prompt

请根据下面的需求，生成一个结构化的 Prompt，并输出成这个仓库的标准模板：

1. 先判断它是 Role / Task / Output / Evaluator / Meta 里的哪一类。
2. 按对应结构组织：Goal、When to Use、Inputs、Prompt、Output Schema、Evaluation、Changelog。
3. 补上 YAML frontmatter（id / name / version / status / category / task / models / tags）。
4. Prompt 正文要具体、可执行，避免空话。

需求：【填入你的需求】

# Output Schema

- 完整 Prompt 文件内容（含 frontmatter + 正文），可直接保存为 `.md`。

# Changelog

- `v1.0` 初版（2026-09-12）
