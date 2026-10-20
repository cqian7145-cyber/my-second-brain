---
title: Anime.js本地接入Codex
source: https://chatgpt.com/c/6aa3dfc5-8194-83ea-bfeb-faaaceb0c228
author:
published:
created: 2026-09-11
description: ChatGPT conversation with 2 messages
tags:
  - clippings
---
animejs使用教程 本地使用 接入codex应用

---

可以。你现在这种 **Next.js / React + Codex 本地开发** 的场景，我建议直接学 **Anime.js V4 + npm 安装** ，不要跟着很多旧教程写 `anime({...})` ，因为 V4 的核心 API 已经变成 `animate()` 、 `stagger()` 等模块化导入。官方文档当前也是按 V4 来写的。 [^1]

### 1\. Anime.js 是干嘛的

Anime.js 是一个前端动画库，适合做：

- 元素进入/退出动画
- 卡片 hover
- 数字、进度条动画
- Hero 区域动效
- SVG 动画
- 多元素 stagger 错峰动画
- 时间轴 Timeline
- 页面高级微交互

它可以直接控制 CSS transform、opacity、SVG、DOM 属性甚至普通 JS 对象。 [^2]

你现在做 SaaS UI，最值得学的是：

```
animate()
stagger()
timeline
scroll
text animation
hover / interaction
```

---

进入你的项目：

```bash
cd 你的项目路径
```

比如：

```bash
cd D:\AI-Product-Data-Normalizer
```

安装：

```bash
npm install animejs
```

官方当前推荐 npm + ES Modules。 [^3]

安装完成后：

```bash
npm run dev
```

---

## 3\. 最简单 Anime.js 示例

普通 JavaScript：

```javascript
import { animate } from 'animejs';

animate('.box', {
  x: 300,
  rotate: 360,
  duration: 1200,
});
```

HTML：

```html
<div class="box"></div>
```

CSS：

```
.box {
  width: 100px;
  height: 100px;
  background: black;
}
```

效果就是：

```
□
↓
向右移动 300px
+
旋转一圈
```

Anime.js V4 官方用法也是这种：

```javascript
import { animate } from 'animejs';
```

而不是很多旧教程里的：

```javascript
import anime from 'animejs';

anime({
  targets: '.box'
});
```

后者主要是 V3 风格。 [^4]

---

## 4\. Next.js / React 中怎么用

这才是你最需要的。

例如：

```typescript
"use client";

import { useEffect, useRef } from "react";
import { animate } from "animejs";

export default function AnimatedCard() {
  const cardRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!cardRef.current) return;

    animate(cardRef.current, {
      opacity: [0, 1],
      y: [30, 0],
      scale: [0.95, 1],
      duration: 800,
      ease: "outExpo",
    });
  }, []);

  return (
    <div
      ref={cardRef}
      className="rounded-2xl border p-6 shadow-lg"
    >
      <h2 className="text-xl font-semibold">
        AI Product Normalizer
      </h2>

      <p className="mt-2 text-gray-500">
        Normalize product data automatically.
      </p>
    </div>
  );
}
```

关键结构：

```
"use client"

↓
useRef()
↓
拿到 DOM
↓
useEffect()
↓
animate()
```

因为 Anime.js 操作浏览器 DOM，而 Next.js 默认 Server Component，所以这种组件一般需要：

```typescript
"use client";
```

---

## 5\. 最常用的动画属性

你先记这几个：

```javascript
animate('.card', {
  x: 100,
  y: 20,
  scale: 1.1,
  rotate: 5,
  opacity: 1,
  duration: 800,
});
```

可以理解成：

| 属性 | 含义 |
| --- | --- |
| `x` | 横向移动 |
| `y` | 纵向移动 |
| `scale` | 缩放 |
| `rotate` | 旋转 |
| `opacity` | 透明度 |
| `duration` | 持续时间 |
| `delay` | 延迟 |
| `ease` | 动画曲线 |

---

## 6\. SaaS 最实用：页面进入动画

例如 Dashboard 卡片：

```typescript
"use client";

import { useEffect } from "react";
import { animate, stagger } from "animejs";

export default function Dashboard() {
  useEffect(() => {
    animate(".dashboard-card", {
      opacity: [0, 1],
      y: [30, 0],
      scale: [0.96, 1],

      delay: stagger(120),

      duration: 700,
      ease: "outExpo",
    });
  }, []);

  return (
    <div className="grid grid-cols-3 gap-4">
      <div className="dashboard-card rounded-xl border p-6">
        Products
      </div>

      <div className="dashboard-card rounded-xl border p-6">
        Orders
      </div>

      <div className="dashboard-card rounded-xl border p-6">
        Revenue
      </div>
    </div>
  );
}
```

关键就是：

```javascript
delay: stagger(120)
```

它会变成：

```
Card 1 ↑

    Card 2 ↑

        Card 3 ↑
```

而不是三个一起突然出现。

官方 V4 也推荐这种 `animate + stagger` 的组合。 [^4]

---

## 7\. Hover 动画

例如高级 SaaS 卡片：

```typescript
"use client";

import { animate } from "animejs";

export default function Card() {

  const handleEnter = (
    e: React.MouseEvent<HTMLDivElement>
  ) => {
    animate(e.currentTarget, {
      y: -8,
      scale: 1.02,
      duration: 300,
      ease: "outExpo",
    });
  };

  const handleLeave = (
    e: React.MouseEvent<HTMLDivElement>
  ) => {
    animate(e.currentTarget, {
      y: 0,
      scale: 1,
      duration: 300,
      ease: "outExpo",
    });
  };

  return (
    <div
      onMouseEnter={handleEnter}
      onMouseLeave={handleLeave}
      className="rounded-2xl border p-6 shadow-sm"
    >
      AI Product Generator
    </div>
  );
}
```

你的网站马上会有明显的产品感。

不过我建议：

```
简单 hover
→ Tailwind transition

复杂动画
→ Anime.js
```

不要所有东西都 Anime.js。

---

## 8\. Codex 怎么接入

这里其实没有一个所谓：

```
Anime.js → Codex API
```

的“接入”。

正确理解是：

```
你的本地项目
├─ Next.js
├─ Tailwind
├─ Anime.js
└─ Codex
     ↓
阅读项目
修改代码
调用 Anime.js API
运行测试
```

也就是说， **Anime.js 是代码依赖，Codex 是开发 Agent** 。

你先：

```bash
npm install animejs
```

然后把整个项目交给 Codex。

---

## 9\. 给 Codex 的第一条指令

比如你现在想优化 SaaS：

```
Inspect this Next.js project.

Anime.js v4 has already been installed.

Your task is to improve the UI motion system using Anime.js.

Requirements:

1. Use Anime.js v4 APIs:
   - animate
   - stagger

2. Do not use deprecated Anime.js v3 syntax.

3. Add subtle animations only:
   - page entrance
   - dashboard card stagger
   - modal entrance
   - loading state
   - success feedback

4. Keep animations professional and minimal.

5. Avoid excessive motion.

6. Preserve the existing Tailwind styling.

7. Respect prefers-reduced-motion.

8. Use React refs where appropriate rather than unnecessarily relying on global selectors.

Before modifying files:
- inspect the project structure
- identify the best components for animation
- explain your implementation plan

Then implement and run:
npm run typecheck
npm test
npm run build
```

这个就已经很好用了。

---

## 10\. 更推荐你建立一个 animation.ts

项目变大后不要：

```
component A
写一套动画

component B
再写一套

component C
继续复制
```

可以创建：

```
lib/
  animations.ts
```

例如：

```typescript
export const motion = {
  fast: 250,
  normal: 450,
  slow: 700,

  enterY: 24,

  cardScale: 1.02,
};
```

组件里：

```typescript
import { motion } from "@/lib/animations";
```

之后所有页面统一。

这叫：

> Motion Design System

比“哪里不好看就加一个 animation”专业很多。

---

## 11\. Codex 项目规则也可以加进去

例如你的：

```
AGENTS.md
```

加入：

```markdown
## Animation

Use Anime.js v4 for complex UI motion.

Rules:

- Import from \`animejs\`.
- Prefer \`animate()\` and \`stagger()\`.
- Do not use Anime.js v3 \`anime({...})\` syntax.
- Prefer CSS/Tailwind transitions for trivial hover effects.
- Use Anime.js for orchestrated, timeline, staggered, SVG, or state-driven animation.
- Keep SaaS motion subtle.
- Avoid animations longer than ~800ms unless justified.
- Respect \`prefers-reduced-motion\`.
- Avoid layout-shifting animations.
- Prefer transform and opacity.
```

这样以后你直接告诉 Codex：

```
优化这个页面的动画
```

它就已经知道你的规则。

---

## 12\. 你应该重点学什么

对于你现在做 AI Web / SaaS，不需要把 Anime.js API 全背下来。

建议顺序：

```
第 1 课
animate()

↓

第 2 课
opacity / x / y / scale

↓

第 3 课
duration / delay / ease

↓

第 4 课
stagger()

↓

第 5 课
React useRef + useEffect

↓

第 6 课
Timeline

↓

第 7 课
Scroll animation

↓

第 8 课
SVG / text animation

↓

第 9 课
完整 SaaS Motion System
```

学到这里就够你做绝大多数产品 UI 了。

### 你的开发链路建议

结合你现在用 Codex 做项目，我建议以后固定成：

```
Next.js
+
Tailwind
+
shadcn/ui / Origin UI
+
Anime.js
+
Codex
```

其中：

```
shadcn / Origin UI
负责组件

Tailwind
负责静态 UI

Anime.js
负责高级动效

Codex
负责实现和重构
```

这几个并不是互相替代，而是组合使用。

官方 Anime.js 文档也明确支持 Vanilla JS、React 和模块化导入，所以非常适合你当前这套前端技术栈。 [^1]

如果你现在就想实际学，我建议下一步直接做一个 **「Anime.js 第 1 课：用 Next.js 做一个高级 SaaS 登录页进入动画」** ，边写边让 Codex 接手，基本 20 行核心代码你就能理解它的工作方式。

[^1]: [animejs.com](https://animejs.com/documentation/getting-started/?utm_source=chatgpt.com)

[^2]: [github.com](https://github.com/athiwatp/anime-js?utm_source=chatgpt.com)

[^3]: [animejs.com](https://animejs.com/documentation/getting-started/module-imports/?utm_source=chatgpt.com)

[^4]: [github.com](https://github.com/iHiD/animejs?utm_source=chatgpt.com)