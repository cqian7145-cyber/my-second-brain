---
title: Shopify Veloura Theme设计
source: https://chatgpt.com/c/6a7216c7-9e28-83ea-be41-47a71c85fca9
author:
published:
created: 2026-08-05
description: ChatGPT conversation with 4 messages
tags:
  - clippings
---
shopify商店theme网页实际制作ai提示词 Veloura

---

如果你要让 AI 帮你 **实际制作 Shopify Veloura 美妆个护独立站 Theme（Homepage + Product Page + Collection Page）** ，不要只说“做一个美妆网站”，要给 AI **品牌定位 + 视觉系统 + 页面结构 + 转化逻辑** 。

Shopify Theme 本质是由 **templates → sections → blocks** 组成，AI 生成时最好按模块要求，否则容易生成漂亮但不能卖货的页面。 [^1]

下面这个 Prompt 可以直接给 Claude / ChatGPT / Cursor / Lovable / Shopify AI 使用：

---

## Veloura Shopify Theme Design Master Prompt

```
You are a senior Shopify theme designer, CRO specialist, and luxury beauty brand creative director.

Design and build a complete Shopify ecommerce theme for a premium skincare and beauty brand called "Veloura".

Brand Positioning:
Veloura is a modern luxury skincare brand targeting women aged 25-45 who care about elegant appearance, anti-aging, skin health, and self-confidence.

Brand Personality:
- Minimalist luxury
- Clean beauty
- Premium but approachable
- Scientific skincare meets feminine elegance
- Similar feeling to brands like Augustinus Bader, Glossier, Tatcha, Aesop

Visual Direction:
Create a high-end editorial aesthetic.

Design style:
- Scandinavian minimalism
- Soft luxury
- Clean white space
- Premium beauty magazine feeling
- Calm and trustworthy

Color Palette:
Primary:
- Warm ivory (#F8F5F0)
- Soft beige
- Champagne gold accents
- Dark charcoal typography

Avoid:
- Bright colors
- Cheap ecommerce feeling
- Overloaded banners
- Excessive animations

Typography:
Use elegant serif fonts for headlines.
Use clean modern sans-serif fonts for body text.

Layout Principles:
- Large whitespace
- Strong visual hierarchy
- Mobile-first responsive design
- Premium product storytelling
- Conversion-focused UX

Create the following Shopify pages:

========================
1. HOMEPAGE DESIGN
========================

Hero Section:

Full-width luxury skincare image.

Headline:
"Reveal Your Natural Radiance"

Subheadline:
"Science-backed skincare designed to restore confidence and timeless beauty."

CTA:
"Shop Best Sellers"

Add:
- Trust badges
- Free shipping message
- Customer rating

Section 2:
Featured Products

Display:
3-4 hero products

Each product card:
- Product image
- Product name
- Short benefit
- Rating stars
- Price
- Quick add button

Section 3:
Problem → Solution Story

Create emotional marketing section:

Example:

"Your skin changes.
Your confidence shouldn't."

Explain:
- Aging concerns
- Dryness
- Dull skin
- Uneven texture

Then introduce Veloura solution.

Section 4:
Science & Ingredients

Create premium ingredient storytelling.

Example:

Hyaluronic Acid
Retinol
Peptides
Niacinamide

Show:
Ingredient image
Benefits
Scientific explanation

Section 5:
Before / After Transformation

Create trust-building section.

Include:
- Customer results
- Testimonials
- Lifestyle images

Section 6:
Brand Philosophy

Create "About Veloura"

Message:

"Beauty is not about becoming someone else.
It is about revealing your best self."

Section 7:
Reviews

Create premium testimonial carousel.

Include:
- Customer photo
- Name
- Verified purchase badge
- Review

Section 8:
Newsletter

Luxury email capture.

Headline:

"Join the Veloura Beauty Circle"

Offer:
10% OFF first order

========================
2. PRODUCT PAGE DESIGN
========================

Create a high-converting Shopify product page.

Structure:

Above Fold:

Left:
Large product gallery

Right:

Product name

Star rating

Price

Short benefit statement

Example:

"Advanced hydration serum that restores youthful glow and strengthens skin barrier."

Benefits:

✓ Deep hydration
✓ Supports collagen production
✓ Improves skin texture

Add:

Quantity selector

Add To Cart button

Buy Now button

Trust section:

Free shipping
30-day guarantee
Secure checkout

Below:

Product Story

Explain:
- Why customers need it
- How it works
- Ingredients
- Usage

Ingredient Breakdown:

Create premium cards.

How To Use:

Morning/night routine.

FAQ:

Include:

Is it suitable for sensitive skin?
When will I see results?
How often should I use it?

Reviews:

High-trust review section.

Recommended Products:

Create skincare routine bundle.

========================
3. COLLECTION PAGE
========================

Create luxury ecommerce collection layout.

Features:

- Clean product grid
- Filter system
- Sort options
- Product cards optimized for conversion

Categories:

Cleanser
Serum
Moisturizer
Anti-aging
Sets

========================
4. HEADER & FOOTER
========================

Header:

Minimal luxury navigation.

Logo:
VEL OURA

Menu:

Shop
Best Sellers
Skin Concerns
Ingredients
About

Footer:

Include:

Customer service
Shipping policy
Returns
Contact
Social media
Newsletter

========================
Technical Requirements
========================

Create Shopify 2.0 compatible structure.

Use:

- Modular sections
- Reusable blocks
- Responsive design
- Fast loading
- SEO friendly
- CRO optimized

Avoid:
- Generic dropshipping style
- Cheap templates
- Too many colors
- Excessive popups

The final result should feel like a $200+ luxury skincare brand website.
```

---

## 如果用 AI 写 Shopify Liquid / Theme代码，再加这一段：

```
Generate Shopify Liquid sections.

Each section must include:

- schema settings
- customizable blocks
- responsive CSS
- mobile optimization
- accessibility best practices

Create:
1. hero.liquid
2. featured-products.liquid
3. ingredient-story.liquid
4. testimonials.liquid
5. product-benefits.liquid
6. faq-section.liquid

Follow Shopify Online Store 2.0 architecture.
```

---

## 我建议 Veloura 实际开发流程：

### 第一步：Theme基础

推荐：

- Dawn（免费）
- Prestige（高级美容风格）
- Impact（转化强）

先不要从0写 Theme。

Shopify 官方 Theme Editor 本身支持 sections、blocks 拖拽调整，所以先用成熟主题，再让 AI 定制效率最高。 [^2]

### 第二步：AI负责

AI生成：

✅ Logo视觉规范  
✅ 首页布局  
✅ 产品文案  
✅ 图片Prompt  
✅ Liquid组件  
✅ CSS优化

### 第三步：统一图片 Prompt

以后所有 Veloura 图片都用：

```
Luxury skincare photography for Veloura beauty brand.

Minimalist aesthetic.
Warm ivory background.
Soft natural sunlight.
Premium glass cosmetic bottle.
Clean beauty editorial style.
Scandinavian luxury design.
High-end beauty magazine photography.
Neutral tones.
Elegant feminine atmosphere.
No text, no logo.

8K realistic photography.
```

这样 Homepage、Product Page、广告素材视觉会统一。

这个方向比较适合你之前想做的 **美妆个护 Shopify 品牌化路线** ，而不是普通一件代发店。品牌感做好后，后续跑 TikTok/Meta 广告承接能力会强很多。

---

我现在有https://www.jonesroadbeauty.com/ homepage长截图帮我生成ai提示词 按照https://www.jonesroadbeauty.com/页面框架进行任务

---

我看了一下 [^3] 的首页结构，它的核心不是单纯卖产品，而是采用 **「品牌故事 + 爆款产品 + 教育消费者 + 信任建立 + 场景化使用」** 的美妆 DTC 框架。它首页主要模块包括：

- Hero 大图 + 主推产品/核心卖点
- Shop by Category 分类入口
- Best Sellers 爆款产品墙
- 产品组合套装（提高客单价）
- 品牌理念（Better Beauty / Clean Beauty）
- Founder Story（创始人背书）
- Get The Look（使用场景教学）
- Newsletter 转化 [^3]

下面这个 Prompt 是给 **Claude / ChatGPT / Cursor / Shopify AI / Lovable** 用的，让 AI 按 Jones Road 的网页逻辑重新设计 **Veloura Homepage** 。

---

## Veloura Shopify Homepage Reverse Engineering Prompt

```
You are a senior Shopify ecommerce designer, CRO strategist, and luxury beauty brand creative director.

Your task:

Recreate the homepage conversion framework inspired by Jones Road Beauty's DTC ecommerce structure, but create an original premium skincare brand experience for "Veloura".

Do NOT copy branding, wording, images, or content.

Analyze the UX logic:
- emotional storytelling
- product discovery
- trust building
- education
- conversion optimization

Brand:

Name:
Veloura

Category:
Luxury skincare & beauty

Positioning:

A modern skincare brand combining clean beauty, advanced ingredients, and effortless daily rituals.

Target Customer:

Women aged 25-45

Pain points:
- tired looking skin
- dull complexion
- early aging signs
- lack of confidence
- complicated skincare routines

Brand Feeling:

Luxury but approachable.
Minimal.
Elegant.
Scientific.
Modern feminine.

Visual Direction:

Create a premium editorial beauty website.

Reference aesthetics:

- Scandinavian luxury
- French beauty minimalism
- clean skincare brands
- high-end magazine layouts

Color System:

Background:
Warm ivory

Secondary:
Soft beige

Accent:
Champagne gold

Text:
Dark charcoal

Typography:

Headline:
Elegant serif

Body:
Modern sans-serif

==============================
HOMEPAGE STRUCTURE
==============================

SECTION 1 — HERO BANNER

Goal:
Immediately communicate brand identity.

Layout:

Full screen lifestyle beauty image.

Left aligned text.

Headline:

"Reveal Your Natural Radiance"

Subheadline:

"Advanced skincare rituals designed to restore your skin's natural glow."

CTA:

SHOP BESTSELLERS

Add:
- Free shipping message
- 30-day satisfaction guarantee
- Clean ingredient badge

Image style:

Luxury skincare photography.

Woman with natural glowing skin.

Soft daylight.

Minimal background.

Premium editorial feeling.

==============================
SECTION 2 — SHOP BY CATEGORY

==============================

Create category navigation.

Cards:

Best Sellers

Serums

Moisturizers

Anti-Aging

Skin Barrier

Gift Sets

Each card:

Large image

Minimal text

Hover animation

==============================
SECTION 3 — BEST SELLERS

==============================

Create product showcase.

Display 4 products.

Example:

Veloura Radiance Serum

Veloura Barrier Cream

Veloura Glow Mask

Veloura Eye Renewal

Product card:

Image

Product name

Short benefit

Star rating

Price

Quick add button

==============================
SECTION 4 — PRODUCT EDUCATION

==============================

Create emotional problem-solution storytelling.

Headline:

"Your skin changes.
Your confidence doesn't have to."

Explain:

Modern lifestyle affects skin:

Stress
Sleep deprivation
Environmental damage

Introduce Veloura solution:

Science-backed formulas.
Simple routines.
Visible results.

==============================
SECTION 5 — INGREDIENT TECHNOLOGY

==============================

Create premium ingredient section.

Cards:

Hyaluronic Acid

Deep hydration

Peptides

Support youthful appearance

Niacinamide

Improve skin texture

Ceramides

Strengthen skin barrier

Design:

Luxury scientific aesthetic.

==============================
SECTION 6 — BRAND PHILOSOPHY

==============================

Create "About Veloura"

Headline:

"Beauty is not about hiding.
It's about revealing."

Include:

Clean formulas

Thoughtful ingredients

Minimal routines

Long-term skin health

==============================
SECTION 7 — PRODUCT BUNDLES

==============================

Increase average order value.

Create:

The Glow Routine

Morning Essentials

Complete Skin Ritual

Show:

Products included

Discount

Save amount

CTA:

BUILD YOUR ROUTINE

==============================
SECTION 8 — CUSTOMER RESULTS

==============================

Create trust section.

Include:

Before/After style cards

Customer testimonials

Example:

★★★★★

"My skin finally looks rested even after long workdays."

==============================
SECTION 9 — GET THE LOOK / ROUTINE

==============================

Create lifestyle tutorial section.

Examples:

Morning Glow Routine

Night Recovery Routine

5 Minute Skin Ritual

Use:

Lifestyle images

Step-by-step guide

==============================
SECTION 10 — FOUNDER STORY

==============================

Create emotional brand story.

Headline:

"Created from a belief that skincare should be simple and effective."

Include:

Founder image

Brand mission

Why Veloura exists

==============================
SECTION 11 — NEWSLETTER

==============================

Premium email capture.

Headline:

"Join the Veloura Beauty Circle"

Offer:

10% OFF your first order.

==============================
TECHNICAL REQUIREMENTS
==============================

Build Shopify Online Store 2.0 compatible layout.

Requirements:

- Modular sections
- Customizable blocks
- Responsive mobile design
- Fast loading
- SEO optimized
- CRO focused

Generate:

1. Homepage wireframe
2. Shopify section structure
3. Liquid component plan
4. CSS design system
5. Image generation prompts

The final homepage should feel like:

A $150-$300 luxury skincare brand,
not a dropshipping store.
```

---

## 如果你是用 AI 做截图复刻（上传 Jones Road 长截图），再追加：

```
Analyze the uploaded homepage screenshot.

Reverse engineer:

1. Section order
2. Container width
3. Spacing system
4. Typography hierarchy
5. Image ratio
6. Product card layout
7. CTA placement
8. Mobile responsive behavior

Then redesign the same UX framework for Veloura.

Do not copy assets.
Create a unique luxury skincare interpretation.
```

---

## 我建议 Veloura 不要完全复制 Jones Road

Jones Road 的逻辑更偏：

**Bobbi Brown个人品牌 → No Makeup Makeup → 产品教育 → 产品销售**

而 Veloura 更适合：

**年轻女性痛点 → 肌肤问题 → 科学方案 → 产品体系 → Routine → 复购**

因为你未来大概率需要靠 Meta/TikTok 广告冷启动，首页第一屏必须承接广告情绪。

最佳结构：

广告点击  
↓  
痛点共鸣 Hero  
↓  
爆款产品  
↓  
为什么有效  
↓  
成分信任  
↓  
用户证明  
↓  
套装提高客单价

这个比单纯展示产品更适合新品牌。

[^1]: [help.shopify.com](https://help.shopify.com/en/manual/online-store/themes/theme-structure/sections-and-blocks?utm_source=chatgpt.com)

[^2]: [help.shopify.com](https://help.shopify.com/en/manual/online-store/themes/customizing-themes/theme-editor?utm_source=chatgpt.com)

[^3]: [jonesroadbeauty.com](https://www.jonesroadbeauty.com/?utm_source=chatgpt.com)