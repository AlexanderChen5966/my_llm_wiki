---
title: Loop Engineering 時代人類工程師的三大職責與 Comprehension Debt
date: 2026-06-24
tags: [ai/agent, pkm/workflow]
source: https://techorange.com/2026/06/10/loop-engineering-ai-agent/
type: article
status: processed
asset_value: 62
---

Loop Engineering 的核心轉變不是「AI 取代人類」，而是**創造價值的槓桿點已經移動**——從寫程式碼，到設計能自動產生程式碼的系統。但 Addy Osmani（Google Cloud AI 部門總監）警告：Loop 運作越順暢，三個問題反而越棘手。

> 「建立你的 Loop，但請以一個打算繼續當工程師的人來建立它，而不是只想按下『開始執行』按鈕的人。」—— Addy Osmani

## T（核心論點：AI 開發典範三次轉移）

| 時代 | 年份 | 核心問題 | 關鍵所在 |
|------|------|---------|---------|
| **提示詞工程** | 2022–2024 | 「我該說什麼？」| 指令的品質 |
| **情境工程** | 2025 | 「我該提供什麼資訊？」| 填入上下文視窗的內容 |
| **Loop 工程** | 2026 | 「我該建立什麼系統？」| 整體系統的架構 |

工程嚴謹性從未消失——它只是**從提示詞搬到情境，再從情境搬到系統架構**。

## B（人類工程師的三大不可外包職責）

### 1. 驗證（Verification）

即使 Loop 長時間自主運作，它仍然可能持續犯錯。

> 「你的工作不是產生程式碼，而是確認程式碼真的可以運作。」

### 2. 理解（Comprehension）—— Comprehension Debt

**Comprehension Debt（理解債）**：當 Agent 產生越來越多程式碼，而開發者閱讀越來越少程式碼時，團隊對系統的理解程度逐漸下降。

短期：專案開發速度提升。  
長期：沒有人真正理解系統如何運作。

這是 Loop Engineering 比 Prompt Engineering 難的根本原因——Prompt Engineering 的風險是單次輸出品質；Loop Engineering 的風險是**累積性的系統理解空洞**。

### 3. 認知不投降（Anti-Cognitive-Surrender）

**Cognitive Surrender（認知投降）**：當 Loop 運作越來越順暢，人們很容易停止思考，長期下來，人類可能逐漸失去形成獨立判斷的能力。

> 「設計迴圈本身並不是答案。當你帶著判斷力去設計它時，它能成為解方；但當你用它來逃避思考時，它反而會成為加速問題惡化的催化劑。同樣的行動，卻可能帶來完全相反的結果。」

## R（Loop 是否值得建立：四個必要條件）

根據《AlphaSignal》，只有以下四個條件**同時成立**，Loop 才有正面效益：

1. **高度重複性**：任務具規律性、不需要每次重新思考
2. **驗證機制已自動化**：有測試/CI/可量化的驗收標準
3. **足夠 Token 預算**：Loop 是高 Token 消耗架構
4. **Agent 具備完整工具存取權限**：無法操作環境的 Agent 只是在猜測

**最適合 Loop 的任務**：CI 故障排查、依賴套件更新、Lint 修正、Issue 轉 PR（高度標準化）

**不適合的任務**：架構重構、產品設計、需要大量判斷的工作——仍應由人主導

## C（關鍵行動）

- [ ] 建立 Loop 前先問：「驗收條件是否已自動化？」——沒有可驗證的結束條件，Loop 只是在浪費 Token
- [ ] 定期「手讀」部分 AI 生成的程式碼，防止 Comprehension Debt 累積
- [ ] 在 Loop 設計中保留人工確認節點（HITL），不要讓 Loop 完全不需要你

## 補充資料

### 日常操作層：從認知卸載到認知投降（Addy Osmani，二手）

來源：aiposthub〈AI 代理接手工作，如何保留批判性思考？〉（2026-11-05，轉述 Osmani 2026-11-03〈Critical thinking when agents do the work〉；**Osmani 原文未讀、未驗證**，且多數觀點為站方引申）。與上文 T 一致：上文講「認知投降」概念，這裡補日常做法。

- **光譜**：認知卸載 → AI 輸出變成下一步的前提 → 認知投降
- **交辦前先寫**：已知情況、假設、目標、範圍、完成條件
- **決策卡**：讀提案時先列出「AI 替我做了哪些決定」
- **檢查亮綠燈要問「它證明了哪件事」**；用自己的話轉述，找出理解缺口
- **意圖負債（Intent Debt）**：目標、限制、決策理由沒有留下。與理解債對偶——理解債是讀的人不懂，意圖負債是寫的當下沒留下為什麼
- **四句交辦模板**：核對假設、列出支持與削弱的證據、寫明前提、列出需由人負責決定的事項

站方原創的活動報名頁情境、YouTube 嵌入連結不收。

## 相關連結

- 📎 2026-11-05：[[boris-cherny-prompt-to-verification]] — 上批已補 Osmani 的「目標＋完成標準＋停止條件」模板（提示詞層驗收）；本篇補充段是人這一側的思考角度，兩者互補
- [[loop-engineering-6-components]] — Loop Engineering 完整框架（六大組件/Worker+Verifier/三護欄）
- [[loop-engineering-implementation-patterns]] — Claude Code 迴圈協議實作（CLAUDE.md + Stop hook）
- [[agentic-engineering-mindset]] — Karpathy 的鬼魂比喻 + 品味不可外包
- [[concepts/meta-ai-engineering-methodology]] — AI 輔助工程三層職責分離（人必須擁有意圖層）
- [[anthropic-claude-ai-perf-sprint-metric-first-loop]] — 人在迴圈的官方實例：人負責推一把（please be braver）、拍板體驗、定方向（2026-09-29 新增）
