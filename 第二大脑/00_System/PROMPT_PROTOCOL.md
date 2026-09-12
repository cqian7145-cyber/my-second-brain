# Prompt Protocol（Prompt 管理协议）

> 目标：把 Prompt 从「文本」变成**可版本化、可组合、可评估的资产**。

## 结构

一个 Prompt 由模块组合而成：

```
Role + Task + Context + Constraints + Output Schema + Evaluator
```

## 存放

- 全部在 [[../03_Prompts/README|03_Prompts]]，按类型分目录（Core / Roles / Tasks / Context / Output / Evaluators / Meta / Production / Deprecated）。
- 每个 Prompt 文件 = **Metadata（YAML frontmatter）+ 正文（固定结构）**。
- 模板见 [[../03_Prompts/_PROMPT_TEMPLATE|Prompt 模板]]。

## 版本

- **不要** `v1.md / v2.md / final.md / final-final.md`。
- 同一文件内，用 frontmatter 的 `version` 字段 + 底部 `Changelog` 维护版本。
- 真正的历史交给 Git（本仓库建议 `git init` + 私有仓库）。

## 生命周期

```
Deprecated ← 废弃
Production ← 验证通过，日常使用
Evaluators ← 评估 Prompt 质量
Meta       ← 生成/改进 Prompt 的 Prompt
```

## 评估闭环

每个重要 Prompt 配一个 Evaluator：

```
Prompt → AI Answer → Evaluator（评分）→ 改进 Prompt
```

见 [[../03_Prompts/Evaluators/README|Evaluators]]。
