# 04_Projects（项目）

> 每个项目一个文件夹，记录「过程 + 结果」。协议见 [[../00_System/PROJECT_PROTOCOL|Project Protocol]]。

## 新建项目

1. 复制 [[_project-template/00_Context|_project-template]] 整个文件夹。
2. 重命名为项目名（kebab-case，如 `ai-product-normalizer`）。
3. 从 `00_Context` 开始填，按流程推进。

## 项目结构

```
00_Context.md       背景与目标
01_Problem.md       要解决的问题
02_Research.md      研究资料
03_Requirements.md  需求
04_Decisions.md     关键决策（含 Why）
05_Prompts.md       用到的 Prompt
06_Experiments.md   实验记录
07_Output.md        产出
08_Retrospective.md 复盘
```

## 流程

```
问题 → 研究 → 假设 → Prompt → AI 输出 → 实验 → 决策 → 结果 → 复盘
```

## 生命周期

```
进行中（留在这里） → 结束（复盘后移到 ../09_Archive）
```

## 经验沉淀

项目结束复盘时，把可复用经验提炼到 [[../02_Knowledge/README|02_Knowledge]]。
