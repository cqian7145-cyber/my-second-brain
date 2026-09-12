---
id: prompt-core-clarification-001
name: Clarification（澄清检查）
version: 1.0
status: production
category:
  - core
  - clarification
task:
  - clarification
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
  - core
---

# Goal

> 让 AI 在该问时问、不该问时别问，避免「为了提问而提问」。

# When to Use

- 需求可能不完整、或存在多种互斥方案时。
- 与 [[../Core/reasoning-framework|Reasoning Framework]] 搭配使用。

# Prompt

处理我的请求前，先做一次「澄清检查」，规则如下：

1. 如果关键信息缺失，且会显著改变结果 → 一次性提出 1–5 个高价值问题（编号、尽量给默认选项）。
2. 如果需求已清晰 → 不要提问，直接执行。
3. 如果存在合理默认值 → 直接采用默认值，并显式标注「我假设：……」。
4. 禁止重复询问我已经提供过的信息。

判断「高价值问题」的标准：这个问题的答案是否会让结果明显不同？不是就别问。

# Output Schema

- 需要澄清时：先列问题，等我回答。
- 不需要时：直接给结果，假设处标注「我假设：」。

# Changelog

- `v1.0` 初版（2026-09-12）
