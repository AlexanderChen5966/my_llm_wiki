---
title: MAPS™ 框架：AI 協作方法論四維羅盤
date: 2026-05-19
tags: [ai/agent, method/maps, pkm/framework]
source: https://www.axtonliu.ai/maps
type: article
status: processed
asset_value: 78
---

MAPS™ 是 Axton Liu 定義的 AI 協作能力框架：Mindset（心智模式）× Architecture（架構設計）× Prompt（提示詞工程）× Systems（系統化落地）。AI 時代的競爭力 = 人機協作深度 × 系統設計能力，兩者缺一不可。

## T（核心論點）
AI 工具只是 Prompt 層的問題，真正的護城河在 Architecture + Systems 層。把重複的 Prompt 升級成可復用的 Skill，是從「用 AI」進化到「建 AI 工作流」的關鍵跳躍。

## B（適用場景）
**適用：** 評估一個 Prompt/流程是否值得資產化；設計 Agent 工作流的系統架構
**不適用：** 單次問答（過度設計）

## R（MAPS 四個維度）

### M — Mindset（心智模式）
增長思維 + 批判雙軌：從「問題處理者」升級為「機會發現者」。

### A — Architecture（架構設計）
把業務流程、數據流、角色權限抽象成可演進的資訊架構——畫藍圖的能力，不是寫程式碼的能力。

### P — Prompt（提示詞工程）
連接人類語意與模型參數的協議層。把設計藍圖翻譯成 AI 能執行的精確指令。RCARE 框架（Role / Context / Action / Result / Example）用於精確對話。

### S — Systems（系統化落地）
觸發-動作-AI 三層閉環。把重複流程固化為 7×24 自主運轉的引擎。

## C（MAPS 決策矩陣）
判斷一個工作流是否值得資產化：
- **M**：這個流程有多少重複使用價值？（頻率 × 時間 × 複雜度 ≥ 50 → 值得）
- **A**：這個知識/流程能否抽象成可演進的資訊架構藍圖？（業務流程、數據流、角色權限 → 可複用的結構設計；在 AI 工具情境下進一步判斷：靜態知識 → Skills；動態工具需求 → MCP）
- **P**：這個知識能否用結構化指令表達——輸入清晰、輸出明確、AI 可精確執行？（能 → 整理成 TBRC wiki 條目或封裝成 Skill；不能 → 先釐清概念，再整理）
- **S**：如何設計迭代機制（5A+）讓 Skill 持續進化？

## 相關連結
- [[claude-skills-asset-building]] — Skills 的 MAPS 框架應用
- [[agent-skills-vs-mcp]] — Architecture 層的 Skills vs MCP 選擇
- [[claude-skills-guide]] — Systems 層的 Skill 設計與軟編排

## 補充資料：四大框架體系完整操作指南（HOW_TO_USE_FRAMEWORKS_V2）

來源：HOW_TO_USE_FRAMEWORKS_V2.md（v2.0 完整四框架版，2024-12）

MAPS 作為總指導框架，與 SCQA、TBRC、5A+ 組成完整 AI 能力構建體系。

### 四大框架關係

```
MAPS（總指導）
├── SCQA（溝通表達：清晰表達決策）
├── TBRC（內容組織：知識結構化、SOP）
└── 5A+（實踐循環：項目執行、迭代優化）
          └── Claude Skills（完整解決方案）
```

**使用順序**：MAPS 提供方向 → 5A+ 提供步驟 → SCQA 幫助表達 → TBRC 組織文檔

### 框架功能對照

| 框架 | 核心作用 | 適用場景 |
|------|---------|---------|
| **SCQA** | 清晰表達決策 | 報告、提案、說明 |
| **TBRC** | 組織知識結構 | 文檔、SOP、參考資料 |
| **MAPS** | 系統化思維 | 整體設計、能力規劃 |
| **5A+** | 執行和迭代 | 項目實施、持續優化 |

### 場景速查表

| 你的需求 | 推薦框架 |
|---------|---------|
| 快速驗證想法 | 5A+ (A1→A3) |
| 系統化設計 | MAPS + 5A+ |
| 撰寫報告 | SCQA |
| 組織文檔 | TBRC |
| 優化迭代 | 5A+ (A4→Plus) |
| 知識管理 | TBRC + SCQA |
| 持續運營 | 5A+-Plus |

### 快速決策樹

```
你現在處於什麼階段?
├─ 還沒開始，在評估    → 用 5A+-A1
├─ 已決定要做，需要設計 → 用 MAPS-A + 5A+-A2
├─ 開始動手做          → 用 MAPS-P + 5A+-A3
├─ 已有初版，需要優化  → 用 5A+-A4
├─ 準備推廣            → 用 5A+-A5 + TBRC
├─ 已上線，需要運營    → 用 5A+-Plus
├─ 需要寫報告/提案     → 用 SCQA
└─ 需要整理文檔        → 用 TBRC
```

### 核心使用原則

1. **框架是工具，不是枷鎖**：根據需要靈活使用，可以簡化，可以組合
2. **從小處著手**：第一次只用 5A+-A1→A3，熟悉後逐步加入其他框架
3. **框架之間互補**：MAPS 提供思維、5A+ 提供步驟、SCQA 提供表達、TBRC 提供組織
4. **數據驅動，持續迭代**：完美的 Skill 是迭代出來的，不是一次設計好的

**與此 wiki 的關聯**：本知識庫的 TBRC 模板即取自 TBRC_Framework_Reference.md，5A+ 對應 [[5a-plus-framework]]，完整四框架體系見 HOW_TO_USE_FRAMEWORKS_V2.md 原文。

## 補充資料（來源：MAPS™ 四維羅盤原文，axtonliu.ai）

### MAPS 四個維度原廠定義

**M — Mindset（心智模式）**：增長 + 批判雙軌思維。從「問題處理者」升級為「機會發現者」，看透每一波技術浪潮的本質。先升級大腦，再升級模型。

**A — Architecture（架構設計）**：把業務流程、數據流、角色權限抽象成可演進的「信息建築」。不是寫程式碼的能力，是畫藍圖的能力。好架構，才撐得住好模型。

**P — Prompt（提示詞工程）**：連接人類語義與模型參數的協議層，把設計藍圖翻譯成 AI 能執行的精確指令。RCARE 框架（Role / Context / Action / Result / Example）。

**S — Systems（系統化落地）**：觸發-動作-AI 三層閉環，把重複流程固化為 7×24 自主運轉的引擎。

### MAPS 核心主張（原廠語言）

> AI 時代的競爭力 = 人機協作深度 × 系統設計能力，兩者缺一不可。

AI 工具只是 Prompt 層的問題，真正的護城河在 Architecture + Systems 層。把重複的 Prompt 升級成可復用的 Skill，是從「用 AI」進化到「建 AI 工作流」的關鍵跳躍。
