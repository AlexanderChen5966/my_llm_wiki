---
title: AI-native 介面模式：當 Agent 開始思考與動手，介面該怎麼告訴人類
date: 2026-09-01
tags: [ai/agent, dev/frontend]
source: https://tools.wingzero.tw/articles/4044（Beautiful UI，2026-09-01）／https://tools.wingzero.tw/articles/4045（beUI，2026-09-01）／https://www.beautifului.dev/
type: reference
status: processed
asset_value: 68
---

## T（核心論點）

傳統設計系統回答的是「網站需要哪些基礎元件」（Button、Modal、Dropdown）。Beautiful UI 回答的是另一個問題：

> 🔑 **當 AI 開始思考、使用工具、修改資料、提出建議甚至準備執行動作時，介面應該如何向人類呈現？**

所以它的範例不是「視覺漂亮」的元件，而是大量圍繞 **Agent 的狀態透明度、Human-in-the-loop 與結果可解釋性**的設計。定位上更接近一套 **AI Product UI Pattern Library**。

## B（適用場景）

**適用：** 開發 AI SaaS／Agent／Copilot／聊天式產品的介面；為 Agent 的中間狀態設計呈現方式；需要在介面上實作人工確認關卡。

**不適用 / 注意：**
- ⚠️ 它是**模式參考**，不是可安裝的元件庫——要的是「該呈現什麼」的答案，不是 npm package。
- ⚠️ 本條目取自二手介紹文，元件清單與定位以官網 `beautifului.dev` 現況為準。
- 🔴 **Thinking pattern 的設計重點不是把內部資訊倒給使用者**——原文明說「重點並不是把所有內部資訊毫無整理地倒給使用者，而是提供一個清楚的視覺層級」。

## R（20 種模式與三個代表性設計）

目前展示 20 種元件／互動模式：**Loading State、Thinking、Streaming Text、Approval Card、Tool Chips、Task Rows、Chat、Prompt Bar、Recommendation Card、Context Cards、Diff Table、Records Table、Filter Table、Sidebar Nav、Search、Flowchart、Insight Cards、Code Block、Fine-tune Card、Selection Actions**。

### ① Thinking——最能代表這個網站的定位

一般 ChatGPT 類產品最簡單的處理是模型工作時顯示 Loading Spinner。但當 Agent 開始搜尋、分析資料、寫程式、呼叫工具時：

> **單純顯示「Loading...」已經無法充分告訴使用者系統正在做什麼。**

Beautiful UI 的解法是**可展開的 Trace**，呈現 Steps、Reasoning、Search、Coding 等不同工作狀態——讓使用者知道 Agent 現在處於什麼階段，而不是傾倒全部內部細節。

### ② Streaming Text——回答本身變成互動物件

除了逐步出現的 Answer，把 **Sources、Actions 與 Follow-up Questions 整合在同一個回答介面**。

> 🔑 **AI Response 不必只是聊天泡泡裡的一大段 Markdown，而可以是帶有資料來源與下一步操作的互動物件。**

### ③ Loading State——用 Pixel-grid Loader、Shimmer 與**經過時間**呈現工作中

（經過時間這一項尤其重要：長時任務沒有計時，使用者無法判斷是慢還是卡住。）

其餘與 Agent 直接相關的：**Approval Card**（人工確認關卡）、**Tool Chips**（工具呼叫的視覺化）、**Task Rows**（Agent 任務進度）、**Diff Table**（修改前後對照）、**Context Cards**（引用來源）、**Fine-tune Card**（讓使用者調整 AI 產生的內容）。

## C（關鍵行動）

- [ ] 產品裡凡是 Agent 會「工作一段時間」的地方，檢查是否只有 Spinner——改成可展開 Trace + 經過時間
- [ ] 有 Action Tool（會改變外部狀態）的流程，一定要有 Approval Card 這類人工確認關卡
- [ ] AI 回答若引用外部資料，把 Sources 做進回答本體而不是附註
- [ ] 設計 Trace 時先決定**視覺層級**，別把內部推理原始輸出直接倒出來

## ⚠️ 易混淆：Beautiful UI 不是 beUI

同一天同一來源還介紹了另一個名字極相近、但**性質完全不同**的專案：

| | **Beautiful UI**（`beautifului.dev`）| **beUI**（`beui.dev`）|
|---|---|---|
| 是什麼 | **AI-native 介面模式參考庫** | **Animated Component Library**（可安裝）|
| 回答的問題 | Agent 在思考／用工具／改資料時，介面該呈現什麼 | 元件的動效怎麼做 |
| 技術 | 模式與範例（非套件）| React／Next.js + Motion（Framer Motion）+ Tailwind CSS |
| 取得方式 | 看範例 | 🔑 透過 **shadcn registry / CLI** 把 TypeScript component source 拉進專案 |
| 代表內容 | Thinking Trace、Approval Card、Streaming Text | Morphing Modal、Animated Toast Stack、Dock、Dynamic Island、Command Palette、Tilt Card、Bottom Sheet、Text/Number Animation、Bouncy Accordion |
| 特色 | Human-in-the-loop 與可解釋性 | **Motion-first**：元件不是做好靜態外觀才加 transition，而是從互動與動畫開始設計 |

**beUI 的關鍵取捨**：它走 shadcn 帶動的 **Source Code Ownership** 模式——程式碼進自己的 codebase，可直接改 Animation Variant、Spring Physics、Layout、Class Name 與 Interaction Logic，不受封裝好的 Component API 限制。⚠️ **代價是元件後續維護由專案自己負責**，而不是等 npm package 升級。官方目前將免費版定位為 Free、Open Source、Fully Customizable。

## 相關連結

- [[concepts/agent-engineering-vocabulary-layers]] — 🔑 本篇補上該地圖標為 🟡 的 **#116 Agent Observability 的「使用者介面」面向**（既有條目都在談開發者側的 trace 與日誌，沒有談終端使用者該看到什麼）
- [[agent-ux-status-transparency-maturity]] — 🔑 **同一問題的成熟度尺**：本篇整理「有哪些介面模式」，該條目給出「進度透明度的五級成熟度」與為什麼轉圈圈不夠（Agent 長時間工作時使用者需要知道**現在在哪一步、還要多久、能不能中斷**）
- [[human-in-the-loop-langchain]] / [[claude-agent-sdk-human-in-the-loop]] — Approval Card 背後的機制層
- [[langgraph-cot-visible]] — 讓推理過程可見的框架實作，與 Thinking pattern 對應
- [[html-as-ai-output-interface]] — AI 產出以 HTML 為介面的既有討論
- [[ai-web-design-system-guide]] — 設計系統的既有整理
- [[claude-code-design-skills-overview]] — Claude Code 設計類 Skill 清單（含 10 大 Skill 補充）
- [[taste-skill-anti-slop-frontend]] — 降低 AI 介面模板感的 Skill
- [[agent-tool-runtime-seven-layers]] — Tool Chips 呈現的是七層裡最外面那一層的結果
- [[frontend-agent-skills-30-picks]] — 前端 Skills 精選
- [[grok-bot-persistent-agent-interface-design]] — 單一產品的一手設計取捨：狀態放進身分、監看三層（2026-09-29 新增）
