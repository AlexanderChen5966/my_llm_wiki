---
title: AI Coding Agent Session 管理：新的工作階段紀律
date: 2026-06-15
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10400115
type: article
status: processed
asset_value: 62
---

GitHub Copilot App 和 Codex Mobile 把 Agent 帶進桌面和手機，但真正的挑戰不是「可以用手機 review」，而是：**當 Agent 變成一個有狀態的工作單位，我們的開發流程必須學會管理多個半自主的工作階段。**

## T（核心論點）

以前我們管理 issue / branch / commit / PR，接下來更像是在管理**一串還沒落地的 Agent session**。

- Session = 新的工作容器，有自己的 branch、檔案、對話和任務狀態
- 上下文不是治理：Agent 可以保存上下文，但團隊不能把責任交給它的上下文
- PR 是結果，session 是過程，**風險在 PR 出現之前就已經發生**

## B（適用場景）

團隊導入 Claude Code / Codex / GitHub Copilot App 等 Agentic 工具後的工作流設計。

## R（完整內容）

### Session 應該回答的問題

一個健康的 session 要能清楚回答：
- 這個 Agent 現在到底在做哪件事？
- 它改了哪個 branch？
- 狀態是卡住 / 等待批准 / 已完成 / 準備開 PR？
- 要接手 review，看哪份 diff / 哪段輸出 / 哪個測試結果？
- 方向不對，是叫它修，還是整個 session 作廢？

### 手機批准的風險邊界

**適合手機批准（低中風險）：**
- 允許 Agent 讀取特定明確檔案
- 讓它跑專案既有測試
- 接受只影響文件或註解的小修正
- 依照 review comment 重新整理 commit

**手機上要非常保守（高風險）：**
- 修改資料庫、部署設定或憑證相關檔案
- 合併跨模組的大型 diff
- 接受沒有足夠測試證據的重構
- 只看摘要就同意 Agent 自動修掉 failing checks
- 涉及安全、付款、權限、資料刪除的變更

原因：螢幕、注意力和上下文都不一樣。工程 review 不只是「看見資訊」，是要能在腦中重建系統行為。

### 健康 Session 的 5 個規則

**1. 任務要小**
不要把「重做整個 dashboard」丟給 Agent。
切成可以 review 的單位：「新增匯出篩選條件」「補上錯誤狀態 UI」「把 API client 換成既有 wrapper」。
Session 越小，branch 越乾淨，review 越像工程不像考古。

**2. 每個 Session 要有明確的允許範圍**
至少寫清楚：
- 可以修改哪些目錄或檔案
- 不要修改哪些邊界
- 既有行為哪些不能變
- 必須跑哪些測試
- 如果測試失敗，可以嘗試修到什麼程度

**3. Session 要留下 review 證據**
不只看最後 diff，還要看跑過哪些命令、哪些測試失敗過、怎麼修、哪裡需要人工判斷。

**4. Branch 命名要有規則**
一個人同時開多個 session，命名混亂很快出事：
`agent/<date>/<topic>` 或類似規則，讓人一眼知道誰開的、為了哪件事、是否還活著。

**5. 定義什麼時候作廢 Session**
Agent 做歪時，不一定要救。直接關掉 session，用更清楚的規格重開，成本更低。
**沉沒成本在 AI 開發裡會變得很貴。** 它改了三十個檔案也不要覺得 diff 可惜。

### Human-in-the-loop 分級

真正的 HITL 不是把人放在 approve 按鈕旁邊，而是設計清楚哪些決策在哪裡做：

| 等級 | 操作 | 適合媒介 |
|------|------|---------|
| 低風險 | 讀檔、跑測試、文件小修 | 手機可快速批准 |
| 中風險 | 局部程式碼修改、測試補強 | 需要看 diff 和測試輸出 |
| 高風險 | 部署、資料、權限、安全、跨模組重構 | 必須完整本機 review |

沒有定義這些界線，人類最終只是橡皮圖章：Agent 說測試過了就按 approve，Agent 說可以 merge 就按 approve。**這不是人在迴圈裡，這是人在流程旁邊點按鈕。**

## C（關鍵行動）

- [ ] 每個 Agent session 開始前定義：允許範圍、不能碰的邊界、必跑測試
- [ ] 建立 branch 命名規則：`agent/<date>/<topic>`
- [ ] 建立 approval 分級制度：低/中/高風險各自的審查要求

## 相關連結

- [[ai-agent-forbidden-tasks-agents-md]] — AI Agent 不能交辦的 6 件事（forbidden tasks 完整清單）
- [[agentic-engineering-sop]] — Agentic Engineering SOP（六階段實戰 SOP）
- [[vibe-vs-agentic-coding-maturity]] — Vibe/Agentic 成熟度框架
- [[claude-squad]] — Claude Squad（多 Agent Session 管理工具）
