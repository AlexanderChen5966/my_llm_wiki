---
title: "Claude 外掛入口整合：Plugins、Skills、Connectors 改到哪裡，誰已經能用？"
source: "https://www.aiposthub.com/claude-unified-plugins-skills-connectors-rollout-20261104/"
author:
  - "[[Philo]]"
published: 2026-11-04
created: 2026-11-04
description: "Claude 外掛管理正在整合，Customize 改名 Plugins，Skills 與 Connectors 可在同一頁查找。整理各方案時程、操作入口，以及移除外掛和撤銷連線的差別。"
tags:
  - "clippings"
status: processed
---
Claude 外掛管理正在換入口。如果你最近打開 Claude，發現 Customize 變成 Plugins，或找不到原本分開的 Skills 和 Connectors 分頁，可能已進入這次介面更新。

Anthropic 從 2026 年 10 月 29 日分階段推出整合，讓技能、連接器與外掛集中管理。技能是可重複使用的工作方法，連接器則用來接上外部服務。

對一般使用者來說，重點是更容易找到已安裝的工具。對團隊管理者來說，還要確認既有控制設定在新介面下的作用。

截至 11 月 4 日，這項變更仍有方案與時間差異。先確認自己的帳號落在哪個階段，才不會把尚未開放誤判成設定出錯。

## Claude 外掛這次整合了什麼？

新的 Plugins 頁面把已安裝與可瀏覽的工具放在同一個入口。官方說明指出，Your plugins 會優先顯示自己、組織或他人分享給你的項目，也能用篩選器找技能或連接器。

公開目錄另外會顯示 Verified 或 Community 標籤，讓使用者辨識來源類別。這能協助選擇，但實際是否適合公司資料與工作需求，仍要看工具功能與要求的權限。

因為既有安裝項目會保留，所以多數人不需要重新安裝。若你只是要找回常用工具，先到 Plugins，再開啟 Your plugins 的 Show all，通常比重做連接更直接。 [來源：Claude 官方變更說明](https://support.claude.com/en/articles/17740565-manage-plugins-skills-and-connectors-from-one-plugins-page?ref=aiposthub.com) 。

![Claude 官方說明頁，說明 Plugins、Skills 與 Connectors 整合](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/hotnews-claude-plugins-body.jpg)

Claude 官方說明頁截圖，記錄外掛管理入口整合與分階段推出。 圖片來源： 官方原文 。

## 哪些帳號已經能用，企業版何時跟上？

官方列出的時程是，Free、Pro、Max、Ultra 與 Team 從 10 月 29 日逐步推出，預計 11 月 9 日全面提供。Enterprise 則從 11 月 30 日預設啟用。

所以在 11 月 4 日，同事看見新入口、你仍看見舊介面，並不一定代表帳號有問題。先核對方案與推出階段，能減少不必要的移除、重裝與重新授權。

管理者還要留意 11 月 30 日的 Skills 關閉規則：若組織關閉 Skills，Claude Desktop 的插件也會受到影響。需要保留或封鎖哪些工具，應由管理者依組織政策決定，不能只為了讓按鈕出現而開啟所有功能。

## 找到工具之後，怎麼確認它真的能工作？

可以先選一個原本就會使用的工具，做一次範圍清楚的測試。例如請它讀取一份已獲授權的文件，再確認來源與內容是否正確，這比一次測試多個新連接更容易排查問題。

如果要實際使用技能，官方提供的入口是聊天中的「＋」或「／」。若工具需要外部帳號，則還要確認連接的是正確帳號，以及這個帳號本身有沒有目標資料的存取權。

也別把「移除插件」當成完整撤銷權限。官方特別說明，移除包含技能與連接器的插件，不會自動斷開那個連接器，因此不用的連線，需要另外處理。

這個區別很實際，因為清理工作介面的清單，與停止外部資料存取，是兩個不同的操作。

## Claude 外掛整合常見問題

### Customize 不見了，需要重裝 Claude 嗎？

先找側邊欄的 Plugins。這次更新包含名稱與管理入口調整，既有工具原則上會保留。

### Team 與 Enterprise 會在同一天更新嗎？

不會。官方目前列出不同推出時程，管理控制也需要另外核對。

### Verified 代表可以不看權限嗎？

仍應檢查資料範圍與工具用途。目錄標籤不能代替公司對資料使用的決定。

## 先找回常用工具，再整理不需要的連線

這次改版適合順手盤點已安裝的工具：哪些仍在使用、哪些只剩舊連線，以及哪些工作需要組織管理者協助。先確定常用流程正常，再逐項清理，會比全面重設更容易保留工作脈絡。

想追蹤 Claude 官方功能與實用教學，歡迎免費訂閱 AI 郵報，也可從官方說明查看後續時程。

延伸閱讀： [Claude MCP Tasks 與 Elicitations：長任務交給外掛，何時回來問你？](https://www.aiposthub.com/claude-mcp-tasks-elicitations-long-running-plugins/)

延伸閱讀： [Claude 非營利組織外掛上線：從資料地圖到募款提案，小團隊怎麼接續工作？](https://www.aiposthub.com/claude-nonprofits-plugin/)