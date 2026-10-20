---
title: Plan / Auto / Dynamic Workflows 三模式定位區辨：非同級的三階段
date: 2026-07-20
tags: [ai/claude, ai/workflow, type/comparison]
source: https://xn--cesq7h2ygv1v901b.com/claude-workflows-plan-mode-auto-mode-guide-2026/
type: article
status: processed
asset_value: 55
---

Anthropic 在 2026-05-28（Opus 4.8 公告同日）把 Dynamic Workflows、Auto Mode 擴大使用與 Workflows research preview 一起推出。很多人把 Plan Mode、Auto Mode、Workflows 當成三個並排按鈕，其實它們不是同級功能，而是三個不同階段——一個偏「先想」、一個偏「自己跑」、一個偏「把大任務拆成系統去跑」。

## T（核心論點）

三模式的區別不是操作風格，而是各自解決不同層次的問題：**Plan Mode 解 decision quality**（動檔前先看清路線）、**Auto Mode 解 execution continuity**（方向已對時減少中斷）、**Dynamic Workflows 解 orchestration scale**（任務大到單一對話難協調時，把計畫寫成可重跑腳本）。關鍵判準是「誰 hold the plan」：subagents/skills/agent teams 的下一步仍由 Claude 互動中決定，唯有 Workflows 是由 script 拿著計畫往下跑。選錯層級會把事情做得更慢——多數人只需要 Plan，卻誤以為需要 Workflows。

## B（適用場景）

**適用：** 判斷該用哪個模式時的任務分級參考。
**不適用 / 注意：** 本篇偏 SEO/內容包裝（原文含大量流量經營討論），數據面以官方文件為準；技術細節深度不如專門條目。

## R（三模式對照 + 任務分級判斷法）

| 功能 | 官方定位 | 最適合的任務 | 不該期待它解決什麼 |
|------|---------|------------|------------------|
| Plan Mode | 先研究與提案，不直接改檔（讀檔/跑 shell 探索/寫 plan 但不改 source） | 陌生專案探索、先拆需求、先看風險 | 直接完成大量修改 |
| Auto Mode | 減少例行授權提示，讓執行更連續（獨立 classifier 擋高風險） | 方向已清楚、低風險步驟多、長任務不中斷 | 當成免審查的全自動保證（仍是 research preview） |
| Dynamic Workflows | Claude 寫 JS orchestration script 協調 dozens-hundreds 個 subagents 的 research preview | 大型遷移、跨來源交叉研究、可重跑流程、codebase-wide audit | 拿來做每個小任務（成本與複雜度都太高） |

> ⚠️ 2026-10-08 更新：2026-08-14 起 auto mode 成為 Pro／Max／Team 新對話預設，9/25（v2.1.283）起不分方案都是。操作與權限分層見 [[claude-code-plan-mode-permission-rules-layering]]。

**任務分級判斷法**（回到任務本身而非追功能名詞）：
- 「還在摸索、要先理解架構風險」→ **Plan Mode**（decision quality）
- 「路線已定、只是受不了一直被打斷」→ **Auto Mode**（execution continuity）
- 「任務大到不是一個人腦中臨場編排就能穩定重做」→ **Dynamic Workflows**（orchestration scale）

**採用順序建議**：先讓日常操作層（Plan + Auto）變穩，再決定是否把大任務升級成可重跑流程。Workflows 是進階編排層，不要一開始就搬最重的功能。

## C（關鍵行動）

- [ ] 動工前先問自己：現在缺的是「先想清楚」「不要被打斷」還是「大到需要流程腳本」
- [ ] 碰陌生專案時把 Plan Mode 當固定第一步，不要直接讓 AI 改檔
- [ ] 只有多代理平行 + 交叉驗證 + 可重跑需求同時成立時才開 Dynamic Workflows

## 原始摘要

房地產阿宥以 Anthropic 官方文件為據，把 Plan/Auto/Workflows 三者定位成非同級的三階段（decision quality / execution continuity / orchestration scale），並提供任務分級判斷法。含 SEO 內容包裝雜訊，故條目精簡取其定位區辨核心。

## 相關連結

- [[claude-code-dynamic-workflows]] — Dynamic Workflows 本體深度條目（本篇是三模式定位區辨角度）
- [[claude-code-auto-mode-permissions]] — Auto Mode 權限機制深挖
- [[claude-code-plan-mode-permission-rules-layering]] — Plan Mode 實際操作、Shift+Tab 循環、規則優先序與設定分層（2026-09 現行版）
- [[claude-opus-4-8-release-notes]] — 三模式與 Opus 4.8 同日發布
