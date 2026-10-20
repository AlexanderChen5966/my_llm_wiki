---
title: Claude Code 跨對話記憶管理：HANDOFF.md + MEMORY.md 模式
date: 2026-06-23
tags: [ai/claude, pkm/workflow, ai/prompt-engineering]
source: https://www.instagram.com/derlife/ (DerLife Claude Code 教學系列) | https://www.koc.com.tw/archives/646548
type: note
status: processed
---

## T（主題脈絡）

Claude Code Desktop 的「換對話」與網頁版 Claude.ai 根本上不同：它直接跑在電腦上、讀寫資料夾，新對話不需要複製貼上，透過檔案就能完整交接。但前提是要建立對的記憶檔案結構，否則跨 session 還是得重新解釋專案脈絡。

同樣的問題存在於所有 AI 工具（ChatGPT、Gemini）的長對話中：對話太長時，模型早期的決定和限制條件會「掉出」有效上下文範圍，導致方向飄移。

## B（核心主張）

**HANDOFF.md = 給明天的我**，**MEMORY.md = 給未來所有對話的我**。兩個檔案責任不同，不要搞混。

| 檔案 | 生命週期 | 記什麼 |
|------|---------|--------|
| `HANDOFF.md` | 每次覆蓋 | 這次做了什麼、改了哪些檔案、TODO、下次從哪接手 |
| `MEMORY.md` | 累積增加 | 值得記一輩子的事：永遠適用的規則、技術選擇、白名單 |

## R（關鍵洞察）

### DerLife 三步驟工作流程

**步驟 1 — 開工**：每個專案開一個獨立資料夾，把 Claude Code 工作目錄指向它。讀寫範圍鎖在這個資料夾，不會誤動別的專案。資料夾內放 `HANDOFF.md`、`MEMORY.md`、`CLAUDE.md`。

**步驟 2 — 收工**：對 Claude Code 說「收工」。它會跳選單：
1. 更新 HANDOFF.md（這次工作的交接紀錄，下次接用）
2. 更新 MEMORY.md（值得跨對話記住的長期事實/偏好）
3. 兩個都要
4. 都不用，直接收工

**步驟 3 — 繼續**：隔天重開同一個專案資料夾，說「讀一下 HANDOFF，我們繼續」。Claude Code 自動讀取 HANDOFF.md（有 MEMORY.md 也會一起讀），三句話確認上次進度，直接接上。

### 進階：寫進 CLAUDE.md 徹底自動化

把觸發詞規則寫入 `~/.claude/CLAUDE.md`（全域，所有專案都讀）：

```markdown
## 自動觸發規則

當我說「今天到這邊」、「先到這邊」、「先這樣」、「我先停」、「先告一段落」、「收工」等暫停/收工詞語時，自動執行 shougong skill，跳選單讓我決定更新 HANDOFF.md、MEMORY.md、兩個都要、或都不用。

當我說「開工」或「繼續」時：
1. 自動讀取專案根目錄的 HANDOFF.md
2. 用三句話跟我確認上次進度
3. 等我下指令再開始
```

設定一次，之後任何專案只要說「開工」，整個流程自動跑。

### 通用版：適用所有 AI 工具的交接摘要提示詞

對話太長時，比起「幫我摘要 100 字」，更好的提示詞包含四個必要元素：

> 請建立一份我可以貼到新聊天中的交接摘要。請包含：**我們想完成什麼**、**目前已做出的關鍵決定**、**有哪些重要資訊是你如果用猜的可能會猜錯的**，以及**下一步該做什麼**。請保持精簡，讓人能在一分鐘內讀完。

四個元素正好是新對話重建脈絡最需要的：目標、已決定的事、容易猜錯的細節、下一步。太短的摘要只有表面結論，漏掉「為什麼這樣決定」和「哪些方案已排除」。

### ADHD 友善的輕量版做法

每個討論段落結束時說：「幫我記錄我做到哪裡、在想什麼、還有什麼沒解決、下一步要做什麼。」下次把這段貼入新對話，30 秒重建脈絡，不需從頭解釋。

## C（連結與延伸）

- [[claude-code-context-management]] — Context 管控全攻略（六成存檔 + `/clear` 重開）
- [[claude-code-auto-memory]] — Auto Memory 機制與三分流判斷法（哪些該記憶、哪些該進 CLAUDE.md）
- [[claude-code-large-codebase]] — 大型專案的 CLAUDE.md 三層架構
- [[get-shit-done-gsd-framework]] — GSD 的 context freshness 原則（每個任務在全新 200k 視窗中執行）
- [[adhd-ai-productivity]] — ADHD 工作者的 AI 記憶外部化策略（與本條目的輕量版做法互補）
- [[spec-driven-ai-workflow]] — ZeroSpec / AGENTS.md 作為跨 session 的靜態背景知識層
- [[claude-code-session-handoff]] — 同主題新增條目（iThome「Claude Code 新手篇」G5）：把 `/compact`、`/rewind`、`/resume` 三個內建工具的**三個共同限制**講清楚（留什麼由模型決定／跨不出這台機器／給不了乾淨白板），並在本頁四要素之外多一段「⑤ 別碰／注意」
