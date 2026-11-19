---
title: "Claude Code Routines 怎麼規劃？官方工作坊的主動工作流與交付驗收"
source: "https://www.aiposthub.com/claude-routines-workshop/"
author:
  - "[[Philo]]"
published: 2026-11-11
created: 2026-11-11
description: "Claude Code Routines 怎麼規劃？官方工作坊的主動工作流與交付驗收。固定整理工單、追蹤文件或檢查部署，常常只差一個可靠的啟動方式。但排程一旦開始自動執行，任務內容和權限就必須寫得比一般聊天更完整。"
tags:
  - "clippings"
status: "processed"
---
固定整理工單、追蹤文件或檢查部署，常常只差一個可靠的啟動方式。但排程一旦開始自動執行，任務內容和權限就必須寫得比一般聊天更完整。

Claude Code 的官方文件已把 Routines 改稱 Scheduled tasks。這類雲端排程把提示、程式倉庫和連接器存成工作設定，再由時間或事件觸發。

我會先交一個能產出草稿、能查回紀錄的小任務，再逐步加入工具。對反覆性工作，清楚的完成條件比一句「自動幫我處理」更有用。

## Claude Code Routines 現在叫什麼？

2026 年 11 月 11 日讀取的 [官方文件](https://code.claude.com/docs/en/routines?ref=aiposthub.com) ，標題是 Automate work with scheduled tasks，並說明原名稱為 routines。舊網址仍可作為入口，但功能名稱應跟著文件更新。

雲端任務可以依時間、API 呼叫或 GitHub 事件啟動。API 是程式向服務提出要求的介面，GitHub 事件則例如新 Pull request，也就是新的程式變更審查提案。

官方也把雲端任務和電腦上的 Desktop 排程、CLI `/loop` 分開。它們的執行位置與依賴不同，選擇前先確認關掉筆電之後是否仍需要工作。

![Claude Code Scheduled tasks 官方文件圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/claude-routines-workshop.png)

Claude Code Scheduled tasks 官方文件圖。來源： 官方／作者發布頁 。

## 從官方工作坊可以學到什麼？

[主動工作流工作坊頁面](https://claude.com/code-with-claude/session/ldn-build-a-proactive-agent-workflow-with-claude-code?ref=aiposthub.com) 是認識用途的入口。教材展示可以啟發任務，但實際設定仍應回目前文件確認。

一個適合入門的例子，是讀取指定倉庫的新文件變動，輸出帶連結的摘要草稿。它範圍明確，也容易用原始文件驗證，適合先測排程是否正常。

提示詞需要自己帶齊背景，包括要讀什麼、時間範圍、去重方式、結果位置及失敗時怎麼回報。雲端新工作不應依賴你記得上一段聊天講過什麼。

![](https://www.youtube.com/watch?v=eSP7PLTXNy8)

Anthropic 官方工作坊；功能設定以目前官方文件為準。 來源： [官方／原作者原文](https://claude.com/code-with-claude/session/ldn-build-a-proactive-agent-workflow-with-claude-code?ref=aiposthub.com) 。

## 權限和完成條件怎麼一起設定？

官方文件指出，任務能做什麼取決於選定倉庫、環境及連接器，而且連接器可能包含寫入動作。因此，建立時要逐項保留需要的服務。

本文建議第一輪只交草稿，讓你能確認來源和格式，再決定是否授權後續寫入。這是工作流的具體設計，不代表所有排程都只會讀取。

完成後看執行歷史，核對有沒有真的開始、是否卡住，以及輸出是否符合指定條件。顯示成功時，最好也能開到結果，才方便持續維護。

## 常見問題

### Routines 和 Scheduled tasks 是兩個不同產品嗎？

目前文件把前者列為舊名稱。遇到舊教學時，先對照同一份官方文件。

### 有排程就不用再檢查了嗎？

資料格式、網站與權限可能改變，應保留失敗回報與執行紀錄。

### 可以直接用自然語言設定嗎？

介面支援的路徑可能不同，不論怎麼建立，都要回讀儲存的提示、觸發條件及連接器。

## 先把重複工作縮成一次明確交付

選一個可核對的草稿任務，跑一次後再開排程。當輸出、權限和失敗方式都清楚，自動化才比較容易長期使用。

延伸閱讀： [Claude Skills 是什麼？免費就能用的 AI 客製化功能完整教學](https://www.aiposthub.com/claude-skills-complete-guide/)

延伸閱讀： [2026 Claude 教學：官方 90 個 Use Cases、12 個工作流程與 Prompt 範例](https://www.aiposthub.com/claude-use-cases-90-workflows-2026/)