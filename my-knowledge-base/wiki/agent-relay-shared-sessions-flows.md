---
title: Agent Relay：跨機器代理訊息、共享會話歷史與 Flows，歷史只是證據不是現況
date: 2026-10-30
tags: [ai/agent, ai/workflow]
source: https://www.aiposthub.com/agent-relay-shared-sessions-team-coding-agents/
type: article
status: processed
asset_value: 36
---

## T（核心論點）

Agent Relay（2026-10-23 發表）在既有編碼工具之外加上三層：**跨機器訊息**（頻道／對話串／DM）、**可搜尋的共享會話歷史**（Shared Sessions）、**以 TypeScript 定義的 Flows**（多步流程、固定檢查、人工關卡），讓多人團隊的代理能直接交接，不必由人當傳話者。但**共享歷史只是當時的證據**，不證明程式庫現在仍成立，接手前仍須核對目前程式與權限。

🔴 單一來源（aiposthub／Philo）；官方示範（前端代理問後端欄位）是產品示意，**沒有獨立效率實驗**。

## B（適用場景）
**適用：** 已常為多人代理交接付出時間的團隊（作者判斷）；前後端配合、需要追查「當初為什麼這樣改」的情境。
**不適用 / 注意：**
- 歷史不等於現況：舊會話說欄位叫 user，現分支可能已改 owner，須在目前分支重跑檢查
- 共享歷史不等於授權：套件讀取工作區歷史，不會取得恢復會話、改分享設定或向其他代理傳訊的權限
- 可搜尋哪些內容取決於工作區 History 存取與分享設定；個人測試對話可能變成同事代理的查詢材料，需事先說清楚哪些可共享
- 自架僅替換訊息引擎 Relaycast（單程序＋SQLite、單租戶）；共享會話、雲端沙箱、代管儀表板仍需 Cloud 帳號；未設定時節點預設連代管服務；核心專案 Apache 2.0

## R（操作步驟）
導入前驗收：
1. 確認工作區 History 存取、CLI 與 Cloud 版本相容（原文指出當時已發布的 CLI 可能尚不支援共享會話參數）
2. 挑一個需前後端配合的變更做驗收
3. 交接流程：先找到歷史中的決策原因 → 再確認目前檔案與測試 → 在目前分支執行相關檢查 → 搜尋結果保留可回查原會話的來源
4. 重要步驟（如「改完必須先跑測試才給合併選項」）用 Flows 寫成程式，比在技能說明裡請代理記得更能強制順序；規則仍要作者正確設定
5. 分享權限與程式整合分開驗收

## C（關鍵行動）
- [ ] 評估前先盤點團隊目前花在代理交接上的時間
- [ ] 訂定哪些任務可共享、哪些保留個人範圍
- [ ] 試用時檢查搜尋結果是否能回到原會話而非僅有代理摘要

## 原始摘要

2026-10-25 發布於 aiposthub，說明 Agent Relay 發表內容、共享會話搜尋的背景價值、歷史需與現況核對、Flows 與技能說明的差異，以及自架與 Cloud 功能邊界。

## 相關連結

- [[claude-code-cross-session-messaging]] — Claude Code 官方原生跨 session 傳訊（僅傳訊，不含共享歷史與 Flows）
- [[ai-memory-cross-agent-handoff]] — 開發工具之間的自動交接與記憶搬家
- [[agent-memory-lifecycle]] — 歷史檢索需要的 Selection／Consolidation 概念
