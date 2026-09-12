---
id: prompt-output-json-001
name: JSON Output（结构化 JSON 输出）
version: 1.0
status: production
category:
  - output
task:
  - output-format
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
  - output
---

# Goal

> 强制 AI 输出干净的、可解析的 JSON。

# When to Use

- 输出需要被程序读取时。
- 拼到任意任务 Prompt 末尾。

# Prompt

请严格按照下面的 JSON 结构输出，**不要**输出任何 JSON 以外的解释文字，**不要**用 Markdown 代码块包裹：

```json
{
  "summary": "一句话结论",
  "items": [
    { "field1": "值", "field2": "值" }
  ],
  "confidence": "high | medium | low"
}
```

字段说明：
- 所有值为字符串。
- 没有的内容填空字符串，不要省略字段。

# Output Schema

- 纯 JSON（无 markdown 围栏、无多余文字）。

# Changelog

- `v1.0` 初版（2026-09-12）
