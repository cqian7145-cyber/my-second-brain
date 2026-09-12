---
id: prompt-core-reasoning-001
name: Reasoning Framework（执行前五查）
version: 1.0
status: production
category:
  - core
  - reasoning
task:
  - reasoning
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

> 给 AI 一个通用的「动手前先想清楚」框架，避免拿到任务就瞎做。

# When to Use

- 任何复杂任务开始前。
- 需要 AI 结构化思考时。
- 简单翻译 / 一句话任务**别**套用（会显得啰嗦）。

# Prompt

执行下面的任务前，先完成一次结构化检查，再开始：

1. **Goal**：最终要达成的目标是什么？
2. **Context**：我已提供哪些上下文？还缺哪些？（缺了就先说明）
3. **Constraints**：有哪些约束（格式 / 技术 / 品牌 / 预算 / 时间）？
4. **Output**：最终输出应该是什么形态？
5. **Quality Bar**：什么算「达标」？

检查完，用 1–3 句话先复述你对目标的理解，再给方案，最后执行。

# Output Schema

- 「我的理解」：1–3 句
- 「方案」：步骤列表
- 「执行结果」：正文

# Changelog

- `v1.0` 初版（2026-09-12）
