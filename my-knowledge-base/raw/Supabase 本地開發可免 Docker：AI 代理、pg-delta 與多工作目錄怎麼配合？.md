---
title: "Supabase 本地開發可免 Docker：AI 代理、pg-delta 與多工作目錄怎麼配合？"
source: "https://www.aiposthub.com/supabase-local-development-agents-pg-delta/"
author:
  - "[[Philo]]"
published: 2026-10-05
created: 2026-10-06
description: "Supabase Select 2026 公布原生本地開發、pg-delta 宣告式資料結構與程式化設定。本文整理 alpha 限制，說明 AI 代理如何在獨立工作目錄測試後端，以及兩種 MCP 的差別。"
tags:
  - "clippings"
status: "inbox"
---
Supabase 是提供資料庫、登入與其他後端服務的平台。2026 年 10 月 2 日的官方發布，將更多後端設定放進程式專案，並讓本地服務能以原生程序執行，降低 AI 程式代理建立測試環境的門檻。

消息近期由 Y Combinator 在 X 分享，查詢時已有接近 10 萬次瀏覽。需要先看清楚的是，免 Docker 的本地服務仍處於 alpha，預設關閉。同場公布的不同功能，也各有不同的開放狀態。

![Supabase Select 2026 的後端開發工具官方主視覺。](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/x20261005e-19-supabase-local-0.png)

Supabase Select 2026 的後端開發工具官方主視覺。 圖片來源： Supabase 官方文章 。

## 為什麼免 Docker 對代理有幫助

Docker 是常用的容器工具，把服務與相依環境包在可重複啟動的單位中。但部分代理沙盒或測試機器沒有可用的 Docker 常駐服務，會讓後端測試難以開始。

依 [Supabase 官方文章](https://supabase.com/blog/select-2026-build-anything?ref=aiposthub.com) ，本地 Supabase 現在可以用原生程序執行，每個工作目錄也能有自己的服務組合。這讓同一個專案的不同程式副本，可以並行測試不同改動。

原生程序仍需要相應環境與資源，不能把免 Docker 等同於完全不用設定。alpha 功能也應先在測試專案評估。

## pg-delta 如何讓資料結構成為來源

資料庫 schema 是資料表、欄位、索引與其他結構的定義。宣告式 schema 讓開發者用 SQL 檔案描述希望得到的結構，再由工具比較差異，產生遷移。

遷移是把舊資料庫逐步改成新結構的操作。Supabase 的 pg-delta 擔任差異引擎，讓代理可以先編輯 schema 檔案，再取得對應的遷移。

官方表示，新專案使用 supabase init 時預設採用 pg-delta。既有專案則需要在設定中啟用。產生遷移後，仍要檢查資料轉換、相容性與刪除風險，再決定是否部署。

Supabase 官方影片展示後端與代理工作流程；各功能開放狀態依公告為準。 影片來源： [Supabase 官方文章](https://supabase.com/blog/select-2026-build-anything?ref=aiposthub.com) 。

## 設定放進程式，變更才容易追蹤

登入供應商、API 限制與儲存桶等設定，可以放在 config.toml。從管理介面修改的內容，也能透過命令取回檔案。

| 功能 | 對代理工作的幫助 | 開始前要確認 |
| --- | --- | --- |
| 原生本地服務 | 在沒有 Docker 的環境測試 | alpha、預設關閉 |
| 每目錄一組服務 | 不同程式副本分開驗證 | 埠號與資源隔離 |
| 宣告式 schema | 以 SQL 描述目標結構 | 遷移內容與資料影響 |
| 設定檔同步 | 讓後端設定進入版本管理 | 是否包含敏感資訊 |

將設定納入版本管理，能讓審查者看見後端變更。這也減少代理只知道程式碼，卻不知道管理介面實際設定的落差。

## 兩種 MCP 和 Compute 要分清楚

MCP 是讓 AI 代理呼叫外部工具的協定。官方 Supabase MCP 協助開發者建立產品。新的應用 MCP 則讓產品使用者的代理，依登入身分操作應用。

應用 MCP 仍使用資料列層級安全規則，也就是 RLS，決定每位使用者能讀寫哪些資料。串接工具之前，權限規則應先驗證。

同場公布的 Supabase Compute 提供長時間執行服務，但當時是 private alpha，需要等候開放。不能把它和本地開發功能寫成全數已普遍可用。

我的判斷是，這批更新最實用的方向，是讓代理能在自己的程式副本中完成後端測試。先用可丟棄的測試資料驗證 schema 與設定，再進入正式部署，會比較容易維持變更品質。

## 常見問題

### 原本使用 Docker 的專案需要改嗎？

官方表示有 Docker 的機器維持原本方式。新原生流程需要另外啟用。

### pg-delta 產生的遷移可以直接上正式環境嗎？

應先審查與測試，尤其涉及既有資料與結構刪改時。

### Compute 已開放所有人使用嗎？

公告時處於 private alpha，應以後續官方開放狀態為準。

## 結語：把後端變更帶進可驗證的開發流程

Supabase 讓 schema、設定與本地服務更貼近程式專案。對 AI 代理而言，能在獨立環境完整測試，比只產生程式碼更接近可交付的成果。