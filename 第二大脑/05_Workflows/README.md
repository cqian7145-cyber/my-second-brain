# 05_Workflows（工作流）

> 可复用的「多步流程」，把多个 Prompt / 动作串起来，固定成标准流程。

## 与 Prompt 的区别

- **Prompt**：单次输入 → 输出。
- **Workflow**：多步、有顺序、可能有分支，可以串联多个 Prompt。

## 示例工作流

### 课题研究工作流

```
1. 收集（丢入 01_Inbox）
2. 用 Tasks/market-research 做初步调研
3. 用 Roles/startup-advisor 做评估
4. 用 Evaluators/evaluate-business-idea 打分
5. 结论写入 04_Projects/<项目>/02_Research
6. 关键取舍写入 04_Decisions
```

### Prompt Router（未来阶段）

```
用户请求 → 意图分类 → 自动挑选 Role + Task + Context + Output + Evaluator → 组合成最终 Prompt
```

## 新建工作流

复制 [[_workflow-template|工作流模板]]，按步骤写清楚「输入 → 动作 → 输出」。
