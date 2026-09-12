# Evaluators

> 评估 Prompt 输出质量的「评分器」。核心闭环：

```
Prompt → AI Answer → Evaluator（评分）→ 改进 Prompt
```

## 用法

把「被评估的输出」丢给 Evaluator，让它按五维打分（Accuracy / Depth / Clarity / Actionability / Completeness），并指出最大问题与改进建议。

- [[evaluate-business-idea|Evaluate Business Idea]]
- [[evaluate-prompt|Evaluate Prompt]]

## 通用打分标准

每项 1–10 分，最后给总分与改进建议。标准见 [[../../00_System/OUTPUT_STANDARDS|Output Standards]]。
