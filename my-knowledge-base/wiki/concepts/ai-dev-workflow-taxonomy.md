---
title: AI 輔助開發流程地圖：六個失敗點與對應知識資產
date: 2026-06-24
tags: [ai/agent, ai/claude, dev/architecture, pkm/workflow]
type: concept-synthesis
sources:
  - [[synthesis-spec-driven-workflow]]
  - [[synthesis-ai-dev-workflow-frameworks]]
  - [[synthesis-agentic-engineering]]
  - [[synthesis-practitioner-skills-design]]
  - [[loop-engineering-6-components]]
  - [[claude-code-community-workflow-2026]]
status: processed
asset_value: 88
---

## 概念定義

AI 輔助開發的知識庫條目常以工具名稱分類（Superpowers、Loop Engineering、Harness），但這遮蔽了它們真正要解決的問題。把這些工具用「**AI 開發的哪個失敗點**」來重新分類，會發現知識庫裡的方法論剛好覆蓋了六個核心失敗原因——每一類的工具互補而非競爭。

這份文件是導航地圖，不重複各條目細節；每格連到既有 wiki 條目。

對照你的開發流程：`figma-to-requirements` → `/requirements` → `agent 開發` → `/stack-review`（或 `/ai-pair`）→ `/doc-update`，六類知識分別對應流程的不同環節。

---

## 六大失敗點總表

| 失敗點 | 發生時機 | 根本原因 | 知識庫條目 |
|--------|---------|---------|-----------|
| **① 上下文不完整** | 開工前 | AI 只知道 How，不知道 What 的邊界 | [[synthesis-spec-driven-workflow]]、[[zerospec]]、[[planning-with-files]]、[[persistent-markdown-planning-skill]]、[[ai-sdd-framework-comparison]] |
| **② 沒有工程紀律** | 執行中 | AI 沒有資深工程師的隱性判斷，只有執行能力 | [[synthesis-practitioner-skills-design]]、[[synthesis-ai-dev-workflow-frameworks]]、[[claude-code-community-workflow-2026]]、[[superpowers-framework]]、[[gstack-framework]] |
| **③ 執行迴圈失控** | 執行中 | 迴圈設計有缺陷：無驗收條件、無中斷機制、無 budget | [[loop-engineering-6-components]]、[[loop-engineering-implementation-patterns]]、[[harness-engineering]]、[[claude-code-dynamic-workflows]]、[[claude-code-goal-command]] |
| **④ 輸出品質無保證** | 審查時 | AI 生成速度快但 bug 率 1.7×，缺乏有效驗證層 | [[claude-code-review-harness-principles]]、[[ai-pair-skill]]、[[synthesis-agentic-engineering]]、[[ai-code-review-safeguards]] |
| **⑤ 跨 Session 失憶** | 長任務 | Context window 清空後 AI 失去任務脈絡，重工或跑偏 | [[claude-code-context-management]]、[[claude-code-auto-memory]]、[[claude-code-handoff-memory-strategy]]、[[claude-code-large-codebase]]、[[claude-code-file-management]] |
| **⑥ 人的心智模型錯誤** | 全程 | 把 AI 當工具升級，而非角色轉型；速度優先而品質妥協 | [[synthesis-agentic-engineering]]、[[vibe-vs-agentic-coding-maturity]]、[[loop-engineering-human-roles]]、[[claude-engineering-lead-workflow-principles]]、[[agentic-engineering-mindset]] |

---

## 對應你的開發流程

```
figma-to-requirements
  └── 失敗點①：沒有 figma spec → AI 臆測 UI 設計意圖
      關鍵知識：[[zerospec]]（Context Readiness）、[[ai-sdd-framework-comparison]]

/requirements
  └── 失敗點①：規格模糊 → 66% 錯誤源於此
      關鍵知識：[[synthesis-spec-driven-workflow]]、[[persistent-markdown-planning-skill]]

agent 開發（一般）
  └── 失敗點②③：沒有工程紀律 + 迴圈失控
      關鍵知識：[[claude-code-community-workflow-2026]]（社群共識 100 分）、[[loop-engineering-6-components]]

/ai-pair dev（複雜/高風險）
  └── 失敗點②④：需要強制多模型審查
      關鍵知識：[[ai-pair-skill]]、[[harness-engineering]]

/stack-review
  └── 失敗點④：輸出品質驗證
      關鍵知識：[[claude-code-review-harness-principles]]、[[synthesis-agentic-engineering]]

跨 Session / 長專案
  └── 失敗點⑤：脈絡斷裂
      關鍵知識：[[claude-code-handoff-memory-strategy]]、[[claude-code-large-codebase]]
```

---

## 關鍵區分

| 對比 | 差別 |
|------|------|
| **規格工具（①）vs Skills（②）** | 規格解決「AI 不知道要做什麼」；Skills 解決「AI 知道要做什麼但不知道怎麼做才對」。兩者缺一不可，組合使用最強 |
| **Loop Engineering（③）vs Harness（④）** | Loop 設計的是「讓 AI 自動往前跑的機制」；Harness 設計的是「跑完之後怎麼驗證品質」。前者追求自動化，後者追求可靠性 |
| **ai-pair（④）vs /stack-review（④）** | ai-pair 是多模型**同時開發+審查**（異構協作降低同質性偏見）；stack-review 是開發後的**獨立審查**。複雜任務用 ai-pair，一般任務用 stack-review |
| **Superpowers（②）vs gstack（②）** | Superpowers 解決「保證不寫廢 code」（紀律強制）；gstack 解決「判斷值不值得寫」（戰略視角）。最強組合是兩者並用：gstack 管策略，Superpowers 管紀律 |
| **vibe coding vs agentic coding（⑥）** | 成熟度差異，不是工具差異。Vibe 適合 PoC/原型；Agentic 適合要上線的產品。見 [[vibe-vs-agentic-coding-maturity]] |

---

## 選型思考

```
UI 設計有 Figma 稿 → figma-to-requirements（先）→ ① 規格工具
需求文字描述不清楚 → ZeroSpec DRIFT 檢查 → ① 規格工具
AI 產出不符工程標準 → Skills 設計（Superpowers/gstack） → ② 工程紀律
CI 自動修復 / 自動 PR → Loop Engineering → ③ 執行迴圈
高風險功能 / 架構改動 → ai-pair（異構多模型審查）→ ④ 品質驗證
長時間專案 / 多 Session → HANDOFF.md + /clear 策略 → ⑤ 情境管理
AI 開發開始讓人焦慮 → 回到失敗點⑥，先確認心智模型
```

---

## 共同含意

1. **六個失敗點是連動的**：規格不清（①）會讓迴圈失控（③）；迴圈失控讓品質更難驗證（④）；品質問題累積會觸發心智模型崩潰（⑥）。不能只解一個

2. **最高 ROI 的切入點是②（工程紀律）**：[[claude-code-community-workflow-2026]]（100 分）的核心就是「用 Skills 把工程紀律編碼進去」，它比換模型、買更多 token 都更有效。96.7% vs 6.7% 的 pass rate 差距來自這裡

3. **⑥ 心智模型是隱藏的先決條件**：心智模型錯誤（把 AI 當魔法工具而非需要監督的系統），會讓其他五個失敗點的解法全部失效。應最先建立，見 [[synthesis-agentic-engineering]]

---

## 相關概念

- [[concepts/agent-workflow-taxonomy]] — Agent 基礎設施的六大類型（與本文互補：那是系統架構，這是開發流程）
- [[synthesis-agentic-engineering]] — Agentic Engineering 哲學精粹（失敗點⑥的核心閱讀）
- [[claude-code-community-workflow-2026]] — 社群共識工作流（失敗點②的最佳入口）
- [[loop-engineering-6-components]] — Loop Engineering 完整框架（失敗點③的核心）
- [[rules/decision-framework]] — 開發流程的個人決策框架
