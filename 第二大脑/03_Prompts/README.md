# 03_Prompts（Prompt 资产库）

> 不是「Prompt 收藏夹」，而是**可组合、可版本化、可评估**的 Prompt 资产库。
> 协议见 [[../00_System/PROMPT_PROTOCOL|Prompt Protocol]]。

## 目录结构

| 目录 | 放什么 |
| --- | --- |
| [[Core/README\|Core]] | 底层推理 / 澄清框架（任何任务都能套） |
| [[Roles/README\|Roles]] | 角色设定（产品经理、创业顾问、AI 工程师…） |
| [[Tasks/README\|Tasks]] | 具体任务（市场调研、竞品分析、PRD、代码审查…） |
| [[Context/README\|Context]] | 可复用的背景/约束片段 |
| [[Output/README\|Output]] | 输出格式定义（JSON / 报告 / Markdown） |
| [[Evaluators/README\|Evaluators]] | 评估 Prompt 质量的「评分器」 |
| [[Meta/README\|Meta]] | 生成 / 改进 Prompt 的 Prompt |
| [[Production/README\|Production]] | 验证通过、日常使用的 Prompt |
| [[Deprecated/README\|Deprecated]] | 已废弃（保留历史，不删除） |

## 怎么组合

一个 Prompt 不是「一段万能的字」，而是模块组合：

```
Roles/startup-advisor   （角色）
  + Tasks/market-research + Tasks/competitor-analysis   （任务）
  + Context/…            （背景约束）
  + Output/report-output  （输出格式）
  + Evaluators/evaluate-business-idea （评估）
```

## 新建 Prompt

复制 [[_PROMPT_TEMPLATE|Prompt 模板]]。

## 生命周期

```
draft → 试用 → 配 Evaluator 评估 → production →（不再用）→ deprecated
```
