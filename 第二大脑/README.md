# 第二大脑 · AI Knowledge OS

> 一套 **Obsidian × 多 AI × Prompt Engineering × Project Memory** 的个人 AI 操作系统。

**模型可以换，知识资产不能换。** 这个仓库是所有 AI 的共同记忆与工作台。

## 目录地图

| 目录                                        | 作用                     | 谁在读写          |
| ----------------------------------------- | ---------------------- | ------------- |
| [[00_System/AI_RULES\|00_System]]         | AI 的「宪法」：规则、上下文、协议     | 所有 AI 进入本仓库先读 |
| [[01_Inbox/README\|01_Inbox]]             | 未整理信息的临时入口             | 你随手丢，之后清空     |
| [[02_Knowledge/README\|02_Knowledge]]     | 可复用知识（按领域分）            | AI 读 + 你沉淀    |
| [[03_Prompts/README\|03_Prompts]]         | Prompt 资产库（可组合/版本化/评估） | AI 读 + 你迭代    |
| [[04_Projects/README\|04_Projects]]       | 项目全过程（问题→复盘）           | 你 + AI        |
| [[05_Workflows/README\|05_Workflows]]     | 可复用的多步工作流              | AI 读          |
| [[06_Decisions/README\|06_Decisions]]     | 关键决策记录（含 Why）          | 你 + AI        |
| [[07_Experiments/README\|07_Experiments]] | Prompt / 方案 A-B 实验     | 你 + AI        |
| [[08_Reviews/README\|08_Reviews]]         | 周期性复盘                  | 你             |
| [[09_Archive/README\|09_Archive]]         | 归档（不删除，只归档）            | 你             |

## 运行闭环

```
Capture（收集）→ Organize（整理）→ Connect（链接）→ Prompt（提问）
   → Execute（执行）→ Evaluate（评估）→ Learn（学习）→ Update Knowledge（沉淀）
```

最终形成飞轮：

> **Knowledge → Prompt → AI → Action → Result → Review → Knowledge**

## 快速上手

1. **先填** [[00_System/USER_CONTEXT]]（让 AI 认识你）。
2. **告诉 AI**：进入本仓库先读 [[00_System/AI_RULES]]。
3. **随手记**丢进 [[01_Inbox/README\|01_Inbox]]，定期清空。
4. **项目**按 [[04_Projects/_project-template/00_Context\|模板]] 新建。
5. **Prompt** 按 [[03_Prompts/_PROMPT_TEMPLATE\|模板]] 沉淀，不再收集「万能 Prompt」。

## 阶段路线

- **阶段 1（现在）**：Obsidian + Markdown + AI Rules —— 结构化知识库。
- **阶段 2**：Codex / Claude Code / Cursor 直接读取本文件夹。
- **阶段 3**：MCP。
- **阶段 4**：RAG / 向量检索（笔记量和检索需求上来后再上）。



可以。你这 10 个模块其实不是“10 个分类文件夹”，而是一条完整的信息流：

**00 规则 → 01 捕获 → 02 沉淀知识 → 03 调用 AI → 04 做项目 → 05 固化流程 → 06 记录决策 → 07 做实验 → 08 复盘 → 09 归档。**

你可以先记住这个总图：

```text
00_System
   ↓ 规定怎么玩

01_Inbox
   ↓ 收集原始信息

02_Knowledge ←──────────────┐
   ↓ 提供知识              │
03_Prompts                 │
   ↓ 指挥 AI               │
04_Projects                │
   ↓ 真正执行              │
05_Workflows               │
   ↓ 固化方法              │
06_Decisions               │
   ↓ 记录为什么            │
07_Experiments             │
   ↓ 验证假设              │
08_Reviews ────────────────┘
   ↓ 提炼经验

09_Archive
   ↓
历史记录
```

## 00_System：系统规则层

**一句话：规定整个知识库和 AI 应该怎么工作。**

这里不是放你学到的 AI 知识，而是放“规则”。

例如：

```text
00_System/
├── AI_RULES.md
├── USER_CONTEXT.md
├── CLARIFICATION_PROTOCOL.md
├── OUTPUT_STANDARDS.md
├── KNOWLEDGE_PROTOCOL.md
├── PROMPT_PROTOCOL.md
└── PROJECT_PROTOCOL.md
```

比如你告诉 Claude：

> 帮我分析 AI Product Data Normalizer。

Claude 应该先通过 `00_System` 知道：

- 是否需要追问
    
- 应该读取什么上下文
    
- 输出标准是什么
    
- 如何处理项目资料
    
- 是否需要记录 Decision
    
- 是否应该把经验沉淀进 Knowledge
    

所以它相当于：

> **AI Knowledge OS 的宪法 + 操作系统配置。**

### 什么应该放进去？

长期规则：

```text
AI 如何回答我
AI 如何追问我
AI 如何使用我的知识
Prompt 怎么管理版本
项目怎么记录
什么东西值得存
什么东西不要存
```

### 什么不应该放？

不要把：

```text
React 教程
创业想法
AI 新闻
某项目需求
某 Prompt 实验结果
```

放进去。

---

# 01_Inbox：信息收件箱

**一句话：不知道放哪的东西，先扔这里。**

这是整个系统的入口。

比如今天你看到：

- 一条 Reddit SaaS 帖子
    
- 一个很好的 Prompt
    
- 一个创业 Idea
    
- 一篇 AI Agent 文章
    
- Claude 给你的一个重要回答
    
- 一个 GitHub Repo
    
- 一个突然想到的产品功能
    

不要当场纠结：

> 到底属于哪个分类？

先进入：

```text
01_Inbox
```

例如：

```text
01_Inbox/
├── 2026-09-13-ai-agent-idea.md
├── reddit-ecommerce-pain.md
├── interesting-prompt.md
└── chrome-extension-idea.md
```

然后每隔几天处理一次。

### Inbox 的核心原则

```text
Capture first
Organize later
```

但这里有个危险：

> Inbox 很容易变垃圾场。

所以必须有：

```text
Inbox
↓
处理
↓
删除垃圾 / 进入 Knowledge / Project / Prompt / Experiment
```

比如：

```text
看到 Reddit 一个帖子
↓
01_Inbox
↓
发现是 Product Data Import 痛点
↓
整理
↓
04_Projects/AI-Product-Data-Normalizer/02_RESEARCH.md
```

如果它形成通用认知：

```text
02_Knowledge/Ecommerce/product-import-pain.md
```

---

# 02_Knowledge：长期知识层

**一句话：存放未来还能反复使用的知识。**

这是你的真正“第二大脑”。

例如：

```text
02_Knowledge/
├── AI/
├── Prompt-Engineering/
├── Coding/
├── Product/
├── Business/
├── Marketing/
├── English/
└── Robotics/
```

比如你学会：

> Chrome Extension 的 content script 和 background service worker 如何通信。

这不是只属于某一个项目。

应该进入：

```text
02_Knowledge/Coding/Chrome-Extension/
content-script-communication.md
```

再比如你总结：

> 一个好的 SaaS MVP 应该聚焦 One User + One Pain + One Workflow + One Outcome。

这属于：

```text
02_Knowledge/Product/mvp-principles.md
```

### Knowledge 和 Projects 最大区别

非常重要。

假设：

> AI Product Data Normalizer 使用 Chrome Extension。

项目里的：

```text
为什么这个项目选择 Chrome Extension
```

属于：

```text
04_Projects
```

而：

```text
Chrome Extension Manifest V3 工作原理
```

属于：

```text
02_Knowledge
```

记住：

> **Project = 这一次怎么做。**  
> **Knowledge = 下一次还能怎么用。**

### 不建议直接存 AI 原回答

错误：

```text
把 Claude 的 4000 字回答全部复制进 Knowledge
```

正确：

```text
Claude 输出
↓
提炼
↓
核心原理
↓
案例
↓
应用方式
↓
相关笔记
```

所以 Knowledge 应该越来越“浓缩”。

---

# 03_Prompts：AI 能力层

**一句话：存放你经过设计、测试、版本管理的 AI 指令资产。**

这个目录非常重要，因为这是你的 Prompt Engineering 实验室。

不是：

```text
万能Prompt.md
超级Prompt.md
神级Prompt.md
```

而是：

```text
03_Prompts/
├── Core/
├── Roles/
├── Tasks/
├── Context/
├── Output/
├── Evaluators/
├── Meta/
├── Routers/
├── Production/
└── Deprecated/
```

比如你以后要：

> 分析一个 SaaS 项目。

不一定需要一个 5000 字万能 Prompt。

可以组合：

```text
Role
startup-advisor

+

Task
market-validation

+

Task
competitor-analysis

+

Output
business-report

+

Evaluator
startup-evaluator
```

### Production 是什么意思？

例如：

```text
status: production
version: 1.4
quality_score: 8.7
```

代表：

> 这个 Prompt 已经实际使用和测试过，可以稳定调用。

而：

```text
status: draft
```

表示还没有验证。

### 什么时候进 03？

比如你发现：

> “执行任务前做 Clarification Check”非常有效。

你把它做成：

```text
03_Prompts/Core/clarification.md
```

以后：

Claude、GPT、Gemini 都能调用。

这就是：

> Prompt 资产化。

---

# 04_Projects：真正干活的地方

**一句话：所有有目标、有开始、有结束的事情都进入 Project。**

这是你的执行中心。

例如：

```text
04_Projects/
├── AI-Product-Data-Normalizer/
├── CET6-Learning-Workspace/
└── AI-Knowledge-OS/
```

一个标准项目：

```text
00_CONTEXT.md
01_PROBLEM.md
02_RESEARCH.md
03_REQUIREMENTS.md
04_PLAN.md
05_DECISIONS.md
06_PROMPTS.md
07_EXPERIMENTS.md
08_OUTPUT.md
09_RETROSPECTIVE.md
```

例如 Product Data Normalizer：

### 00_CONTEXT

一句话让 AI 明白：

> 这个项目到底是什么，现在做到哪。

### 01_PROBLEM

记录：

> 用户真正有什么问题？

### 02_RESEARCH

记录：

- Reddit
    
- X
    
- 竞品
    
- 用户反馈
    
- 市场研究
    

### 03_REQUIREMENTS

记录：

> MVP 到底做什么。

### 04_PLAN

记录：

> 开发路线。

### 05_DECISIONS

记录：

> 为什么选 Chrome Extension + SaaS，而不是纯 Web。

### 06_PROMPTS

记录：

> 这个项目具体用了什么 Prompt。

### 07_EXPERIMENTS

例如：

```text
AI 自动生成标题
vs
规则 + AI
```

哪个效果更好？

### 08_OUTPUT

真正交付：

- PRD
    
- MVP
    
- 代码
    
- 报告
    
- Demo
    

### 09_RETROSPECTIVE

最后：

> 这个项目让我学到了什么？

然后把通用知识提取到：

```text
02_Knowledge
```

所以 **Projects 是整个系统最核心的发动机。**

---

# 05_Workflows：SOP / 工作流层

**一句话：把反复做的事情变成固定流程。**

假设你第一次做 SaaS 调研：

全靠临场发挥。

第二次：

又重新想一次。

第三次：

还是重新想。

这很浪费。

你应该把它变成：

```text
05_Workflows/Product/
SaaS-Idea-Validation.md
```

例如：

```text
1. Define User
↓
2. Define Pain
↓
3. Reddit Search
↓
4. Competitor Research
↓
5. Existing Alternatives
↓
6. Willingness To Pay
↓
7. Distribution Analysis
↓
8. MVP Scope
↓
9. Go / No-Go
```

以后你说：

> 帮我分析一个新 SaaS。

AI 直接调用这套 Workflow。

### Workflow 和 Prompt 区别

这是非常容易混淆的。

**Prompt：告诉 AI 怎么执行一个动作。**

例如：

```text
分析竞争对手
```

**Workflow：定义一连串动作的顺序。**

例如：

```text
市场调研
↓
竞品分析
↓
用户痛点
↓
定价
↓
MVP
↓
决策
```

所以：

```text
Workflow
包含多个
Prompt
```

---

# 06_Decisions：决策记忆层

**一句话：记录“为什么这样做”。**

这个模块长期非常值钱。

因为几个月以后你经常会问：

> 当初为什么选 Supabase？

如果只有代码，你只知道：

> 用了 Supabase。

不知道为什么。

Decision 应该记录：

```text
Context
Options
Tradeoffs
Decision
Why
Risks
Revisit When
```

比如：

```text
2026-09-13-use-chrome-extension-plus-saas.md
```

内容：

```text
问题：
产品到底做纯 SaaS 还是 Chrome Extension + SaaS？

A:
纯 SaaS

B:
Extension + SaaS

Decision:
Extension + SaaS

Why:
用户本身就在供应商页面工作，
Extension 可以减少复制粘贴。

Risk:
Extension 开发和审核增加复杂度。

Revisit when:
Extension 使用率低于 XX%。
```

以后 Claude 再问：

> 要不要改纯 SaaS？

它可以先读历史 Decision。

这就是：

> **组织记忆。**

---

# 07_Experiments：实验验证层

**一句话：不要只靠感觉，用实验验证。**

这里特别适合你做 Prompt Engineering 和产品。

例如：

### Prompt Experiment

```text
Prompt A
vs
Prompt B
```

测试：

```text
Accuracy
Clarity
Depth
Actionability
```

最后：

```text
Prompt B wins
```

然后：

```text
03_Prompts
v1.3 → v1.4
```

再比如产品：

> 商品标题优化到底应该纯 LLM，还是规则 + LLM？

实验：

```text
方案 A
Pure LLM

方案 B
Rules + LLM

测试 30 个商品

比较：
准确率
错误率
成本
速度
```

最终再做 Decision。

### Experiments 与 Decisions 的关系

通常：

```text
Question
↓
Experiment
↓
Evidence
↓
Decision
```

所以：

> Experiment 管“测试”。

> Decision 管“结论”。

---

# 08_Reviews：复盘系统

**一句话：定期把杂乱经历变成知识。**

如果没有 Review：

```text
Project
Project
Project
Project
```

做了一堆事情，但你的能力提升可能很慢。

有 Review：

```text
Experience
↓
Reflection
↓
Pattern
↓
Knowledge
```

比如每周：

```text
08_Reviews/Weekly/
2026-W37-weekly-review.md
```

你可以回答：

```text
本周完成了什么？

学到了什么？

哪些事情失败了？

哪些 Prompt 好用？

哪些 Prompt 不好用？

哪些知识应该沉淀？

做了哪些重大决策？

哪些项目应该停止？

下周最重要的 3 件事是什么？
```

这一步负责把：

```text
04 Projects
07 Experiments
06 Decisions
```

重新转化成：

```text
02 Knowledge
03 Prompts
05 Workflows
```

所以 Review 是整个系统的：

> **垃圾回收 + 系统升级机制。**

---

# 09_Archive：冷存储

**一句话：不用了，但未来可能需要追溯的东西放这里。**

原则：

> Archive ≠ Delete

例如：

- 已结束项目
    
- 老版本研究
    
- 不再使用的工作流
    
- 已废弃 Prompt
    
- 过期资料
    

不要继续占据主工作区。

比如：

```text
Active Project
↓
完成
↓
Retrospective
↓
提炼 Knowledge
↓
Archive
```

这样主目录永远保持干净。

### 一个重要区别

Prompt 如果废弃，优先：

```text
03_Prompts/Deprecated
```

因为它仍然属于 Prompt System。

真正已经完全退出日常系统的东西，再进入：

```text
09_Archive
```

---

# 用一个真实案例串起来

假设今天你突然想到：

> “能不能做一个 AI 自动优化 Amazon Listing 的 Chrome 插件？”

### 第一步：01 Inbox

先记录：

```text
AI Amazon Listing Idea
```

不急着分类。

### 第二步：04 Projects

发现值得研究：

```text
04_Projects/Incubating/
AI-Amazon-Listing/
```

### 第三步：03 Prompts

调用：

```text
startup-evaluation
market-research
competitor-analysis
```

### 第四步：05 Workflows

运行：

```text
SaaS Idea Validation Workflow
```

### 第五步：07 Experiments

验证：

> 卖家到底需不需要？

采访 / Landing Page / MVP 测试。

### 第六步：06 Decisions

结果：

> 不单独做 Amazon，而并入 Product Data Normalizer。

记录为什么。

### 第七步：08 Reviews

周末发现一个重要认知：

> 卖家真正痛点不是“写文案”，而是多平台商品数据标准化。

### 第八步：02 Knowledge

沉淀：

```text
Ecommerce Product Data Fragmentation.md
```

这条知识以后又帮助你做：

> Product Data Normalizer。

于是形成闭环：

```text
Idea
↓
Inbox
↓
Project
↓
Prompt
↓
Workflow
↓
Experiment
↓
Decision
↓
Review
↓
Knowledge
↓
Better Project
```

## 最简单的判断口诀

以后你不知道一个东西该放哪，可以直接这样判断：

|情况|放哪里|
|---|---|
|不知道是什么，先记下来|`01_Inbox`|
|长期可复用的知识|`02_Knowledge`|
|可以重复调用的 AI 指令|`03_Prompts`|
|有明确目标和结果的事情|`04_Projects`|
|经常重复的一套步骤|`05_Workflows`|
|“为什么这么选”的记录|`06_Decisions`|
|需要测试真假的假设|`07_Experiments`|
|总结一段时间做得怎么样|`08_Reviews`|
|已经不用但要保留|`09_Archive`|
|关于整个系统怎么运行|`00_System`|

你现在这个 Vault 真正应该形成的核心关系不是“把笔记分类好”，而是：

> **Project 产生经验 → Review 提炼经验 → Knowledge 保存经验 → Prompt/Workflow 利用经验 → 下一个 Project 做得更好。**

这才是这套 AI Knowledge OS 的核心价值。