---
title: "Claude Code Agent Teams 教學：什麼時候該多開 AI？"
date: 2026-03-02
tags: [ai/claude, ai/agent, type/how-to]
source: https://brickverse.com.tw/blog/claude-code-agent-teams-tutorial
type: article
status: processed
asset_value: 64
---

## T（核心論點）

**Agent Teams 不是加速鍵，是路線的改變。** Claude Code 官方提供 `TeamCreate`／`Task`（帶 `team_name`）／`TaskCreate`／`SendMessage`／`TeamDelete` 等工具，讓多個 Claude 並行協作；但多開 AI 只在「任務真的能同時進行」時才划算，用錯情境反而增加重工與協調成本。判斷關鍵不是人數多寡，而是任務彼此是否真的獨立。

⚠️ **脈絡區隔**：本篇聚焦官方 Agent Teams 工具本身的使用時機判準；同批次另有 EMil Wu「Agent Team 實戰」系列談的是 GM/Teammate 身份持久化與 A7 稽核方法論，兩者主題關鍵字相似但脈絡不同，本庫不建立跨系列連結。

## B（適用場景）

**適用：**
- 前後端同步開發、文案與切版同步進行等「真正互不依賴」的並行任務
- 大範圍掃描與研究（同時探索多市場資料、多競品分析、多份訪談摘要）
- 大規模重構（多個互不依賴的模組分工）

**不適用 / 注意：**
- 線性流程（如「訪談稿整理→需求文件更新→功能排序」這種前後依賴鏈）：同時派多 Agent 只會互相等待，或用錯誤前提工作，一個 Claude 按順序做完反而更快更穩
- 任務太小太短（改措辭、修錯字）：建立 Team、召喚 Teammates、整合、關閉的 overhead 比任務本身還貴
- 每一步都需要人審查決策：並行優勢會消失，Teams 最適合「交代清楚方向後讓 Agents 自主完成」的任務
- 不確定時，可直接把任務描述給 Claude 問「這個案例需不需要用 team agent？」，讓 Claude 依成本效益判斷並協助拆分

## R（操作步驟）

**快速判斷表**（三個 YES 才值得建 Team）：
1. 任務可以拆成 2 個以上互不依賴的部分嗎？→ NO 就用單一 Claude
2. 每個部分的工作量夠大嗎（幾分鐘以上）？→ NO 就 overhead 不划算
3. 各部分完成後，能清楚整合結果嗎？→ NO 就先釐清整合方式

**完整生命週期**（以「文案 + 切版並行開發」為例）：
1. `TeamCreate`（team_name、description）建立 Team
2. `TaskCreate` 建立共享任務清單（各任務含明確描述）
3. `Task`（subagent_type、team_name、name、prompt）召喚 Teammates，兩者同時開始工作，不需等一個完成才召喚下一個
4. 協調進行中的工作：Teammate 進入 `idle` 是正常等待狀態（等你下一步指示或等其他 Agent 輸出），不是出錯；完成時訊息自動投遞到對話中，可用 `SendMessage` 轉發資訊給其他 Teammate
5. 關閉：**必須先對每個 Teammate 送 `shutdown_request` 並等其回應 `approve: true`，才能執行 `TeamDelete`**——順序顛倒會失敗，沒收到回應就重送一次

## C（關鍵行動）

- [ ] 拿到任務先套三個 YES 判斷表，拿不準就直接問 Claude 需不需要開 team
- [ ] 確認任務是否為線性依賴鏈；線性流程不要多開 Agent
- [ ] 關閉 Team 前務必先完成所有 Teammate 的 shutdown 交握，再執行 TeamDelete

## 原始摘要

Claude Code Agent Teams 是官方多 Agent 協作功能：Team Lead（你）分配工作、整合成果，Teammates 各自獨立工作、完成後自動通知。文章給出三種不適合情境（線性流程、任務太小、每步需人審查）與三種適合情境（真正獨立並行、大範圍掃描研究、多模組重構），並附完整操作流程範例與關閉順序注意事項。本文基於 Claude Code Agent SDK（2026）規格撰寫。

## 相關連結

- [[claude-code-agent-teams-practical-examples]] —— 官方 Agent Teams「怎麼用」的 4 個實戰範例（本條目談「何時該用」，兩者互補）
- [[claude-code-projects-redesign-2026-09]] —— 2026-09-17 Projects 重構把同一套「何時該平行」的判斷交給雲端 Coordinator，本條目的判準在那裡變成「該不該讓它開新 Thread」
