---
title: AI 輔助工程方法論 跨主題概念框架
date: 2026-06-05
tags: [ai/agent, dev/architecture, ai/workflow]
type: meta-synthesis
sources:
  - [[synthesis-ai-dev-workflow-frameworks]]
  - [[synthesis-agentic-engineering]]
  - [[synthesis-agent-framework-philosophy]]
  - [[synthesis-spec-driven-workflow]]
status: processed
---

## 整合來源

| 精粹條目 | 核心論點 |
|---------|---------|
| [[synthesis-ai-dev-workflow-frameworks]] | 工作流架構是瓶頸；gstack 管戰略、Superpowers 管紀律；最強組合是兩者並用 |
| [[synthesis-agentic-engineering]] | 角色轉型（執行者→監督者）是本質；護城河在審查治理層；Understanding 不可外包 |
| [[synthesis-agent-framework-philosophy]] | 框架選擇核心是穩定性哲學；環境比模型影響更大；「最壞情況是什麼」才是正確選擇依據 |
| [[synthesis-spec-driven-workflow]] | 規格文件是 AI 的外部大腦；四層鏈路（Layer 0-3）解決上下文缺口 |

---

## 共同前提

四篇精粹都隱含以下假設，從未被質疑：

1. **AI 執行能力已足夠強**：瓶頸不在「AI 能不能做到」，在「AI 被告知要做正確的事」
2. **工程師的護城河是判斷力，不是實作技能**：可以外包寫程式，不能外包理解邊界
3. **問題的根源在系統設計**：個別 prompt 技巧和模型選擇的影響，遠小於架構/規格/流程的影響
4. **純 Markdown 的持久化是現階段的最佳解**：四個框架都不依賴向量資料庫或外部服務

---

## 核心張力

### 張力 1：速度 vs 治理

**AI 讓執行速度趨近零，但治理缺失讓錯誤累積速度也趨近零（負面的 compound）。**

- synthesis-agentic-engineering：CodeRabbit 數據顯示 AI 代碼有 1.7× major bugs、2.74× XSS 漏洞——更快執行 = 更快累積技術債
- synthesis-agent-framework-philosophy：「正確的 Harness 比更好的模型影響更大」——Manus 重寫 5 次 Harness 的效果 > 換模型
- **未解決**：沒有任何精粹提出「治理成本」的計算方式——加多少治理才算夠，加多少才算過度？

### 張力 2：強制結構 vs 最小原則

同樣主張「先設計後執行」，但實踐路線相反：

| 陣營 | 代表 | 主張 |
|------|------|------|
| 強制結構 | Superpowers Mandatory Workflow、planning-with-files「永遠先建 Plan」 | 流程必須強制，否則 AI 會繞過 |
| 最小原則 | ZeroSpec「不觸發就不建立文件」、Harness「越做越薄」 | 過度結構是認知負擔，最小必要才可持續 |

**未解決**：哪種情況應選強制，哪種選最小？目前四篇都未給出判斷框架。

### 張力 3：人定義邊界 vs AI 自學邊界

所有精粹都說「人必須定義規格與邊界」，但 gstack 的 GBrain（跨 session 記憶）和 Hermes 的學習迴圈（每次任務後自動萃取技能）開始讓 AI 自動修改自己的治理規則——**這個趨勢正在侵蝕「意圖層必須由人擁有」這個前提**。

---

## 上層概念：三層職責分離框架

把四個精粹放在一起，浮現出一個統合概念：

```
┌─────────────────────────────────────────────────────┐
│ 意圖層（Why / What）— 人必須擁有                       │
│ • 需求定義、功能範圍、Out of Scope、架構邊界           │
│ • 工具：ZeroSpec / spec-kit / OpenSpec               │
│ • 核心能力：Understanding（不可外包）                  │
├─────────────────────────────────────────────────────┤
│ 治理層（How to govern）— 人設計、AI 可參與             │
│ • 流程強制、品質關卡、Harness、審查機制               │
│ • 工具：Superpowers Mandatory Workflow / gstack Sprint│
│         Harness Guides×Sensors / 框架穩定性哲學       │
│ • 核心能力：選框架前先問「壞掉最壞是什麼」            │
├─────────────────────────────────────────────────────┤
│ 執行層（Do）— AI 主導                                 │
│ • 寫程式、生成內容、運行測試、執行任務                │
│ • 工具：Claude Code subagent / Codex CLI / Gemini CLI│
│ • 核心能力：規格文件作為持久化外部大腦                │
└─────────────────────────────────────────────────────┘
```

**這個框架解釋了四篇精粹的所有核心主張**：

- 「先想清楚再動工」= 意圖層必須在執行層動工前就位
- 「Understanding 不可外包」= 意圖層的判斷力是人的護城河
- 「Harness 比模型重要」= 治理層的品質決定執行層的天花板
- 「規格文件是外部大腦」= 意圖層的輸出必須能跨 session 持久化給執行層讀取

---

## 應用缺口

**只有把四篇放在一起看，才能發現這些缺口：**

1. **意圖層的 AI 滲透正在發生，無人討論**：gstack /office-hours 幫你「想清楚需求」，brainstorming Skill 協助探索邊界——AI 已在協助生成意圖層的輸出。但「人負責意圖層」這個前提是否仍然成立？沒有任何精粹觸及這個演進，也沒有安全邊界的討論。

2. **三層之間的切換摩擦是未計量的成本**：從意圖層（規格文件）切換到執行層，需要 Context Hygiene（清空 context + 重讀文件）；從執行層回到意圖層（發現規格需更新），需要中斷執行。這個摩擦成本沒有量化，也沒有工具專門解決。

3. **治理層的自進化是最大的未爆風險**：Hermes 學習迴圈讓 AI 自動更新技能規則；GBrain 跨 session 記憶讓 AI 自動調整行為。如果治理層規則可以被 AI 自動修改，「人設計治理層」這個前提就動搖了。這是單看任何一個精粹都無法看到的跨框架風險。

4. ~~**Layer 0-3 工具的銜接沒有指引**~~（已有答案，見 [[research/dev-workflow-optimization-tools]] 和 [[research/claude-pilot-suite-single-agent-workflow]]）：兩份研究筆記分別提供多 Agent 路線（ZeroSpec → spec-kit → planning-with-files → CCB+dotbabel → review）和單 Agent 路線（ZeroSpec → requirements → planning-with-files → claude-pilot-suite → review）的完整整合架構，並說明 ZeroSpec（靜態基底）與 planning-with-files（動態基底）如何互補而不衝突。

---

## 下一步行動

- [ ] 為 ai-pair 開發制定「三層職責分離」的具體分工：哪些決策屬於意圖層（人必須確認）、哪些屬於治理層（SKILL.md 設計）、哪些屬於執行層（Reviewer 的行為）
- [ ] 研究「意圖層 AI 滲透」的邊界：GBrain 和 Hermes 的學習迴圈在實踐中是否已導致意外的治理層漂移
- [ ] 在個人工作流中實驗：強制結構（Superpowers 路線）vs 最小原則（ZeroSpec 路線）在哪類任務各自更合適，建立自己的判斷框架

## 相關連結

- [[concepts/ai-dev-workflow-taxonomy]] — 六個失敗點與對應知識資產的導航地圖，可當三層職責分離的落地檢查表