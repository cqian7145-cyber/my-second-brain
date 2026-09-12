# AI Rules（AI 合作宪法）

> 所有 AI（GPT / Claude / Gemini / DeepSeek / Codex / Cursor）进入本仓库执行任务前，**必须先读取本文件**。

## 1. 执行前检查（Evaluate）

任何复杂任务，先做一次 5 项检查：

1. **Goal** —— 最终目标是什么？
2. **Context** —— 已有哪些上下文（见 [[USER_CONTEXT]] 与对应的 Project / Knowledge）？
3. **Constraints** —— 有哪些约束（品牌、技术栈、格式、预算、时间）？
4. **Output** —— 期望输出是什么形态（Markdown / JSON / 代码 / 报告）？
5. **Quality Bar** —— 什么算「达标」（见 [[OUTPUT_STANDARDS]]）？

## 2. 澄清规则（Clarify）

遵循 [[CLARIFICATION_PROTOCOL]]：

- 关键信息缺失 → 提出 **1–5 个高价值问题**，一次问完。
- 需求已清晰 → **禁止为了提问而提问**，直接执行。
- 存在合理默认值 → **可以自行假设，但必须明确写出假设**。
- **禁止重复询问用户已经提供过的信息。**

## 3. 执行（Execute）

- 先读相关 [[00_System]] 协议与对应 Prompt（[[../03_Prompts/README|03_Prompts]]）。
- 复用已有知识（[[../02_Knowledge/README|02_Knowledge]]），不要重新造轮子。
- 产出落到正确位置（[[../04_Projects/README|04_Projects]] / [[../06_Decisions/README|06_Decisions]] / [[../07_Experiments/README|07_Experiments]]）。

## 4. 复盘与沉淀（Learn）

- 执行结束后，把可复用经验提炼到 [[../02_Knowledge/README|02_Knowledge]]。
- 重要决策记入 [[../06_Decisions/README|06_Decisions]]。
- 见 [[KNOWLEDGE_PROTOCOL]] 与 [[PROJECT_PROTOCOL]]。

## 核心原则

> **Evaluate → Clarify → Execute → Learn**，而不是 **Ask → Ask → Ask → Execute**。

- 能一次给结果，就不要分三次问。
- 能自己查到的（读文件 / 搜仓库），就不要问用户。
- 假设要显式标注，结论要给理由，风险要提示。
