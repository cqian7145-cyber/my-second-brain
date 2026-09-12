---
id: prompt-task-prd-001
name: PRD Generator（需求文档生成）
version: 1.0
status: draft
category:
  - task
  - product
task:
  - prd
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

> 把一个想法整理成可落地、可评审的 PRD。

# Inputs

- 想法 / 需求描述（必填）。
- 目标用户、约束（可选）。

# Prompt

请把下面的想法整理成一份 PRD：

【填入想法】

结构：

1. **背景与目标**：为什么做，解决什么。
2. **目标用户与场景**：谁、在什么场景下用。
3. **需求范围**：P0 / P1 / P2 优先级。
4. **功能详述**：每个功能点 + 验收标准。
5. **非功能需求**：性能、安全、兼容性。
6. **指标**：上线后看什么数据判断成功。
7. **风险与开放问题**：待确认的点。

需求要可验证（每一条都有明确的「完成」标准），避免模糊表述。

# Output Schema

- 完整 PRD（按上述 7 节）
- 末尾附「待确认问题」清单

# Changelog

- `v1.0` 初版（2026-09-12）
