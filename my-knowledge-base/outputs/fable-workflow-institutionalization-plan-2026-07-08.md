---
title: 用 Fable 5 將開發流程與工作流程制度化：可行性評估與執行計畫
date: 2026-07-08
tags: [ai/claude, ai/workflow]
source: 對話分析（基於 claude-fable-5-usage-guide / claude-fable-5-power-tasks / loop-engineering-6-components）
type: reference
status: processed
---

# 用 Fable 5 將開發流程與工作流程制度化

> 背景：[[claude-fable-5-usage-guide]] 第八節「社群進階玩法」提出：不拿 Fable 5 做一次性任務，而是請它把「判斷力」轉寫成一套之後能沿用的制度與檔案，讓價值留在硬碟上，後續由便宜模型照制度長期運作。本文件評估此玩法套用到個人開發流程與知識庫工作流程的可行性，並列出執行計畫。

## 結論

可行，且基礎比一般情況好——制度的骨架（Skills、CLAUDE.md）已經存在，Fable 5 要做的不是從零建立，而是**補上四個缺口**：路由判斷明文化、Skills 一致性健檢、驗證迴圈原則化、經驗累積規則。

## 現況盤點：已有的制度資產

### 開發流程（三條明文路徑）

| 複雜度 | 路徑 | 對應 Skill |
|--------|------|-----------|
| 簡單 | 純手寫修改 | — |
| 一般 | 需求驅動開發 | `/requirements` → agent 開發 → `/stack-review` → `/doc-update` |
| 複雜 | AI Pair 多模型協作 | `/requirements` → `/ai-pair dev`（含 review）→ `/doc-update` |

### 知識庫工作流程

九個 Skill（knowledge-organizer / raw-pipeline / knowledge-synthesizer / knowledge-elevation / wiki-health-check / daily-brief-generator / alignment-check / obsidian-second-brain / scientific-brainstorming）+ CLAUDE.md 規則 + rules/ 協議文件。

這些檔案本身就是「留在硬碟上的制度」。

## 四個缺口與對應做法

### 缺口 1：路由判斷仍在腦中

「改動複雜度決定走哪條路」目前是隱性判斷。Fable 5 擅長把隱性判斷寫成可執行的 decision rules。

**做法**：明文化路由訊號 → 路徑的對應表，例如：

- 影響檔案數（1 檔 vs 跨多檔）
- 是否跨模組 / 跨層（UI + API + DB）
- 有無 UI 變更（需要 figma-to-requirements 前置？）
- 風險等級（可回滾 vs 不可逆）

寫進 CLAUDE.md 任務路由表或獨立 routing 規則檔，之後任何模型接手都能做出同樣的選擇。

### 缺口 2：Skills 未做過全庫一致性健檢

官方指出：替前代模型寫的 skills 對 Fable 5 常「過度規範」，逐步指揮反而拖累品質。既有 Skill 累積自不同時期，可能存在互相矛盾、過期、過度規範的段落。

**做法**：讓 Fable 5 一次讀完所有 Skill（開發流程 4 個 + 知識庫 9 個），輸出三類問題清單：

1. 互相矛盾的指令（同一情境兩個 Skill 給出不同做法）
2. 過期內容（引用已不存在的檔案、指令、模型行為）
3. 過度規範（逐步指揮可簡化為目標描述的段落）

這是長上下文一致性比對，正是 Fable 5 的核心優勢（見 [[claude-fable-5-power-tasks]] 第 2 項「知識庫健康掃描」）。

### 缺口 3：驗證迴圈未原則化

stack-review 和 ai-pair 已有 reviewer 角色，但「fresh-context verifier」原則未明文化——驗證者不應共享開發者的 context，否則會繼承其盲點（[[loop-engineering-6-components]] 的 Worker/Verifier 分離）。

**做法**：在相關 Skill 中明文寫入：驗證用獨立子代理、context 全新、對照原始規格（requirements 文件）而非開發過程的中間產物。

### 缺口 4：經驗未沉澱成規則

Review 發現的問題目前散在對話裡，session 結束即消失。

**做法**：照官方 memory 建議建立 lessons 檔案維護規則：

- 一檔一教訓，開頭一行摘要
- 修正與已驗證做法都記，含 why
- 已記過的更新而非重複存，錯的即刪
- 每次 stack-review / ai-pair 結束時，將發現的通用問題萃取成一條規則存入

## 可行性檢核（Loop Engineering 三關）

| 檢查 | 開發流程 | 判定 |
|------|---------|------|
| 重複性 | 開發流程高頻使用，制度化成本可攤平 | ✓ |
| 可驗收 | review 有明確標準，Skill 修改前後可比對 | ✓ |
| 有價值 | 一次建好、長期由任何模型沿用 | ✓ |

## 建議執行順序

1. **先做缺口 2（Skills 健檢）**——它會順便暴露缺口 1、3 該補在哪裡，且單次長上下文任務最能發揮 Fable 5 優勢
2. 依健檢結果補缺口 1（路由規則）與缺口 3（驗證原則），修改落在具體 Skill 檔案
3. 最後建缺口 4（lessons 規則），因為它依賴前三項定型後的流程

## 注意事項

- 這是社群觀點（Threads @gyozalab 提出、數位時代整理），非 Anthropic 官方能力保證
- 前提假設：後續由其他 Claude 模型（Sonnet / Opus / Haiku）接手長期運作
- 涉及修改既有 Skill 檔案時，先備份或走版本控制再動手

## 後續狀態（2026-11-09）

本文是 07-08 的評估，內文保留原樣。四個缺口到 2026-11-09 的進度（流程 v2、v3，見 [[decision-framework]]、[[dev-workflow-optimization-tools]] 文末）：

| 缺口 | 狀態 | 怎麼補的 |
|---|---|---|
| 1. 路由判斷仍在腦中 | ✅ 部分 | decision-framework 寫明「情境內的條件步驟」與情境 2/3 vs 4/5 的分界（一般任務遇到單一高風險決定用 doubt-check，一開始就知道動多個風險區塊才走 ai-pair）。本文建議的「影響檔案數、是否跨層」等量化訊號沒有採用，改以風險清單判斷 |
| 2. Skills 一致性健檢 | ⏳ 未做 | 沒有做全庫一次性健檢；改成每次改 skill 時同步 Skill存檔、`SKILL_INDEX.md`、`skills-list.md`，並用 workflow-bench 回放比對版本 |
| 3. 驗證迴圈未原則化 | ✅ | `/stack-review` 有任務文件時平行派 spec subagent（新 context、對照任務文件而非開發過程）；新增 `doubt-check`，reviewer 只拿產物＋契約、不拿結論 |
| 4. 經驗未沉澱成規則 | ✅ | `/doc-update` Step 7 沉澱判斷：門檻三條＋選最便宜的形式（測試 → 專案規則 → 三層記憶 → 文件 → 公司知識），寫入前先給使用者看；第一個實例是 SSGS-13687 的清除鍵規則 |

本文的前提「後續由便宜模型照制度長期運作」沒有變；實際做法是把判斷寫進 skill 與 decision-framework，而不是另建 lessons 檔案（三層記憶已承擔這個角色）。

## 相關連結

- [[decision-framework]] — 流程 v3 的權威來源（含本文缺口 1、3、4 的落地）
- [[multi-model-orchestration-expansion-plan-2026-07-08]] — 延伸規劃：三個工具（fox-ai-roundtable / notebooklm-kit Codex 端 / claude-pilot-suite）的具體擴充設計，含成本護欄修正
- [[fable-advisor-subagent-skill]] — 缺口 3（驗證迴圈原則化）的具體實作範例：Opus 統籌 + Sonnet worker + Fable 5 決策點顧問
- [[claude-fable-5-usage-guide]] — 官方使用指南：effort / 驗證迴圈 / 修正提示詞 / memory
- [[claude-fable-5-power-tasks]] — 限時窗口最值得處理的 5 種長上下文任務
- [[loop-engineering-6-components]] — Worker/Verifier 分離與三關檢核的出處
- [[llm-cost-optimization-strategies]] — 貴模型搭架構、便宜模型維護的分層邏輯
