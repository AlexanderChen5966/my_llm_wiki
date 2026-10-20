---
title: AI 協作 Web 設計系統：14 面向框架與 25 種視覺風格
date: 2026-06-24
tags: [ai/prompt-engineering, dev/architecture]
source: https://qiita.com/yuuuki_ogino/items/33c6e5c52fa5b3dfbb39
type: reference
status: processed
asset_value: 71
---

AI 生成 UI 失敗的原因通常不是「模型不夠好」，而是需求沒有系統化傳遞。此框架把 Web 設計需求拆成 14 個面向，讓 AI（或人類）可以一致地理解和執行設計意圖。

## T（核心論點）

**設計系統 = AI 協作語言**——把品牌/視覺/行為規格結構化，AI 才能跨請求保持一致性。一次定義好設計系統，後續 Prompt 只需要說「根據設計系統做 X」。

## B（適用場景）

- 用 AI 快速搭建 Web 原型（SaaS / 官網 / 活動頁）
- 多次對話中保持視覺一致性
- 向 AI 傳達複雜設計需求（替代繁瑣的口頭描述）

## R（14 面向設計需求框架）

| # | 面向 | 說明 | 範例 |
|---|------|------|------|
| 1 | **目的（Purpose）** | 網站目標用戶與核心功能 | 「B2B SaaS 登陸頁，目標是轉換試用」|
| 2 | **資訊架構（IA）** | 頁面層級與導覽結構 | Hero → Features → Pricing → CTA |
| 3 | **頁面結構（Layout）** | 區塊分割方式 | 全幅 Hero + 三欄特色 + 兩欄比較表 |
| 4 | **版面配置（Grid）** | 欄數、間距、對齊 | 12-column, 24px gutter, max-width 1280px |
| 5 | **視覺風格（Style）** | 設計語言（見下方風格表）| Glassmorphism + 暗色系 |
| 6 | **語氣（Tone）** | 文案與設計氛圍 | 「專業可信賴，不過度正式」|
| 7 | **色彩設計（Color）** | 主色/次色/強調色/中性色 | Primary: #6C63FF, Neutral: #F5F5F5 |
| 8 | **字體排版（Typography）** | 字型、大小比例、行距 | Noto Sans TC, H1=48px/1.2, Body=16px/1.7 |
| 9 | **元件設計（Components）** | 按鈕/卡片/表單/圖示規格 | 圓角 12px，按鈕高度 48px |
| 10 | **UI 狀態（States）** | Hover/Focus/Disabled/Error | Focus: 2px outline, brand color |
| 11 | **留白圓角陰影（Spacing）** | 間距系統、陰影層級 | 8px 基準，陰影：0 4px 24px rgba(0,0,0,0.1) |
| 12 | **響應式（Responsive）** | 斷點與行為 | 1280/1024/768/375px 四個斷點 |
| 13 | **無障礙（A11y）** | WCAG 等級與對比需求 | WCAG AA，最低對比 4.5:1 |
| 14 | **設計系統（DS）** | Token 命名與變數規範 | CSS variables 設計 Token |

### 設計 Token 範例（CSS Variables）

```css
:root {
  --color-primary: #6C63FF;
  --color-primary-light: #A09BE8;
  --color-neutral-100: #F9F9F9;
  --color-neutral-900: #111111;
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 32px;
  --radius-sm: 4px;
  --radius-md: 12px;
  --radius-lg: 24px;
  --shadow-card: 0 4px 24px rgba(0, 0, 0, 0.08);
  --font-heading: 'Noto Sans TC', sans-serif;
  --font-body: 'Noto Sans TC', sans-serif;
}
```

## 25 種視覺風格速查

| 風格 | 特徵 | 適用 |
|------|------|------|
| **Flat** | 無陰影無漸層，純色幾何 | 品牌、教育 |
| **Material** | 卡片+陰影+圖示 | Android App、企業工具 |
| **Glassmorphism** | 毛玻璃背景+高光邊框 | SaaS、科技 |
| **Bento UI** | 不規則大小卡片格布局 | 產品展示、Portfolio |
| **Brutalism** | 粗邊框、高對比、非對稱 | 創意、反傳統 |
| **Neo Brutalism** | Brutalism+柔和色 | 新創、設計工具 |
| **Skeuomorphism** | 仿真實物件紋理 | 工具類 App |
| **Neumorphism** | 軟浮雕（淺色凸起效果）| 個人專案、設定頁 |
| **Swiss/International** | 網格+大字+幾何 | 設計展覽、出版 |
| **Cyberpunk** | 霓虹+暗黑+數位感 | 遊戲、科幻 |
| **Maximalism** | 多色多元素多紋理 | 時尚、藝術 |
| **Organic** | 不規則形狀+自然色 | 健康、植物 |
| **Retro** | 懷舊配色+圓角+噪點 | 品牌、文化 |
| **Japanese Minimal** | 大量留白+細線+禪意 | 高端品牌、日系 |
| **8-bit Pixel** | 像素藝術 | 遊戲、懷舊 |
| **Paper Cutout** | 層疊剪紙插圖感 | 兒童、創意 |

## C（AI 設計需求模板）

直接在 Prompt 開頭附上此模板，AI 即可遵循整個設計系統：

```
Design System Spec:
- Purpose: [目標用戶與核心功能]
- Visual Style: [風格名稱]
- Color: Primary=[HEX], Secondary=[HEX], Background=[HEX]
- Typography: Heading=[字型/size/weight], Body=[字型/size/line-height]
- Spacing: Base unit=8px, Container max-width=[px]
- Corner radius: [sm=Xpx / md=Xpx / lg=Xpx]
- Tone: [一句話形容]

Task: [具體設計需求]
```

### 用途別快速範例

| 用途 | 風格推薦 | 色彩方向 |
|------|---------|---------|
| **SaaS 登陸頁** | Glassmorphism 或 Flat | 深色系 + 亮紫/青 |
| **美妝品牌** | Organic 或 Maximalism | 玫瑰金、奶油白 |
| **徵才頁** | Swiss 或 Material | 深藍、白、橘 |
| **活動報名頁** | Retro 或 Neo Brutalism | 高對比飽和色 |

## 相關連結

- [[taste-skill-anti-slop-frontend]] — Frontend 反 Slop 方法論（品味培養）
- [[screenshot-to-enterprise-frontend-skill]] — 截圖轉 Enterprise Frontend 流程
- [[concepts/agent-workflow-taxonomy]] — Agent 與工具分類（包含設計類工具）
