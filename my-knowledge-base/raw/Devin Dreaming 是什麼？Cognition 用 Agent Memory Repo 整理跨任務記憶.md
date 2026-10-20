---
title: "Devin Dreaming 是什麼？Cognition 用 Agent Memory Repo 整理跨任務記憶"
source: "https://www.aiposthub.com/devin-dreaming-agent-memory-repo/"
author:
  - "[[Philo]]"
published: 2026-10-06
created: 2026-10-07
description: "Cognition 推出 Devin Dreaming，並公開 Agent Memory Repo 構想，以 Markdown 和 Git 儲存與整理代理記憶。本文解釋跨工作階段的運作方式，以及本地試用與完整自動排程功能的差別。"
tags:
  - "clippings"
status: processed
---
程式代理可以完成一次修改，卻常在下次工作時忘記團隊偏好：哪些測試要先跑、哪個決策已經被否決、哪些檔案才是準則。Cognition 發表 Devin Dreaming，嘗試讓記憶跨工作階段累積，並定期清理過時資訊。

這次同時公開的 Agent Memory Repo，讓記憶更接近一套可檢視的專案資料。它對開發團隊的吸引力，在於能追查記憶如何形成與修改，減少「代理記得什麼，只有代理知道」的黑箱。

![Devin Dreaming 記憶整理功能的官方展示（官方影片封面）](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/x20261006-03-dreaming-0.jpg)

Devin Dreaming 記憶整理功能的官方展示（官方影片封面） 圖片來源： Cognition 官方貼文 。

## Agent Memory Repo 把記憶存成可讀檔案

依 [Cognition 官方文章](https://cognition.com/agent-memory-repo?ref=aiposthub.com) ，記憶以 Markdown 檔案儲存，並用 Git 追蹤版本。Markdown 是容易閱讀的純文字格式，Git 則記錄修改歷史，讓團隊能看差異、找來源，也能回復先前版本。

代理開始新工作時，可以讀取這份資料庫；完成工作後，再補入有用的資訊。檔案之間的連結與來源，使它不只是把聊天紀錄全部堆進去，而是整理成下次能使用的知識。

例如某次除錯確認一個相容性限制，理想的記憶應包含適用環境與證據。只留下「這個套件不好用」，會把暫時問題放大成永久偏見，後續任務反而容易被帶歪。

## Dreaming 的重點是整理與淘汰

Dreaming 讓代理在工作之外回頭整理記憶，合併重複紀錄、檢查矛盾、移除過時資訊，並找出尚未明說的關聯。Cognition 的 Devin 展示把這類整理放在背景執行。

這解決的是記憶維護問題。模型本身並不因為儲存一份檔案，就等同重新訓練或永久改變參數。下次做事能否改善，仍取決於讀到的記憶是否正確、相關，而且沒有覆蓋新的要求。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/10/x20261006-03-dreaming.mp4" width="1280" height="720" controls=""></video>

Devin Dreaming 記憶整理功能的官方展示。此為原始示範，功能與實際效果依官方說明為準。 影片來源： Cognition 官方貼文 。

## 本地試用與完整產品功能要分清楚

公開儲存庫提供本地嘗試記憶流程的起點，但本地試用並未包含所有遠端同步、自動啟動或排程整理能力。直接複製範例，不會自然得到和 Devin 展示一模一樣的夜間運作。

團隊若要接入正式專案，我會先選低敏感、易核對的開發規則，讓每筆記憶有來源與適用範圍。工作偏好可以儲存，金鑰、客戶資料與一次性對話則不應因為「可能有用」就全部寫進共享儲存庫。

## Devin Dreaming 常見問題

### Dreaming 代表 AI 在睡覺或產生意識嗎？

這是產品名稱與工作比喻。官方描述的是代理背景整理記憶的流程，沒有據此證明意識或人類式夢境。

### Agent Memory Repo 能讓所有代理自動互通嗎？

可讀檔案與版本管理有利於交換資料，但不同代理仍需實作讀寫流程與權限。共享格式不等於所有產品已經提供完整相容功能。

延伸閱讀： [GitHarness 是什麼？讓 AI 記住需求版本，改規格時不用整份重做](https://www.aiposthub.com/githarness-agent-memory-changing-requirements/)

## 結語：記憶需要版本，也需要有效期限

這個方向值得開發團隊嘗試，因為記憶可以像程式碼一樣被審查。評估時應檢視代理是否引用正確來源、會不會保留已失效的規則，讓「記得更多」同時做到「少記錯一些」。