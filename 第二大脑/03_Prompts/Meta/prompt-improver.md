---
id: prompt-meta-improver-001
name: Prompt Improver（改进已有 Prompt）
version: 1.0
status: draft
category:
  - meta
task:
  - prompt-improvement
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

> 改进一个已有的 Prompt，让它更清晰、更可执行、输出更稳定。

# Inputs

- 原 Prompt（必填）。
- 遇到的问题（可选，例如「输出太啰嗦」「总是漏掉 X」）。

# Prompt

请改进下面这个 Prompt：

1. 找出它的 3 个主要问题（目标不清 / 上下文不足 / 结构混乱 / 输出不可验证 / 冗余）。
2. 逐一修复，保留原意，不改变任务目标。
3. 输出优化后的完整 Prompt，并在末尾附 Changelog（说明改了什么、为什么）。

我遇到的问题（如有）：【填入】

原 Prompt：
【粘贴原 Prompt】

# Output Schema

- 主要问题（3 点）
- 优化后的完整 Prompt
- Changelog

# Changelog

- `v1.0` 初版（2026-09-12）
