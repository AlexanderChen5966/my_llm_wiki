---
title: Cursor Self-Hosted Machines：Cloud Agents 自架執行環境
date: 2026-09-03
tags: [ai/agent]
source: https://www.aiposthub.com/cursor-self-hosted-machines-cloud-agents/
type: note
status: processed
asset_value: 46
---

Cursor 於 2026-09-02 推出 Self-Hosted Machines，讓 Cloud Agents 的「工具執行環境」（讀寫程式碼、跑測試、連內部服務）可搬到企業自有機器（VM/GPU/Mac/Kubernetes），但模型推論、規劃與 Agent loop 仍在 Cursor 雲端執行。核心可轉用判準（最大誤解澄清）：「Self-Hosted」不等於資料完全不出內網——任務所需的檔案內容、終端輸出、截圖與 MCP 結果仍會傳給 Cursor 處理，資安團隊需逐項確認哪些內容會外流，而非只問 Worker 架在哪裡。My Machines（個人/小規模測試）與 Team Pools（需 Cursor Enterprise，可自動擴縮）兩種模式適用情境不同；導入成本除 Cursor 用量費用外，還需企業自行負擔硬體與 Worker 映像維運。若要深入查原文，看導入決策流程圖與「企業可以怎麼開始」的分階段驗證建議。

## 相關連結
- [[agent-permission-and-os-sandbox]] — Agent 執行環境的權限與沙箱隔離，與 Self-Hosted Worker 的資料邊界問題相關
- [[agent-readiness-cloudflare-audit]] — 企業導入 Agent 前的稽核判準，可與本篇「資安逐項確認清單」交叉參考
