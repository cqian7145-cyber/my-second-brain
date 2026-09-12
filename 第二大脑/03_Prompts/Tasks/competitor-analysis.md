---
id: prompt-task-competitor-001
name: Competitor Analysis（竞品分析）
version: 1.0
status: draft
category:
  - task
  - research
task:
  - competitor-analysis
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

> 对竞品做结构化拆解，找到差异化机会。

# Inputs

- 竞品名称 / 链接（必填，可多个）。
- 你的产品定位（可选，用于对比）。

# Prompt

请对【填入竞品】做竞品分析：

1. **定位**：一句话定位，目标用户。
2. **功能**：核心功能与特色功能。
3. **定价**：价格体系、免费/付费策略。
4. **优势**：它们做得好、难以复制的地方。
5. **劣势 / 空白**：用户抱怨、没覆盖的场景。
6. **对我方启示**：可借鉴之处 + 可差异化之处。

用表格对比多个竞品时，列要统一、可读。

# Output Schema

- 对比表（可选）
- 逐竞品拆解
- 差异化机会（3 点）

# Changelog

- `v1.0` 初版（2026-09-12）
