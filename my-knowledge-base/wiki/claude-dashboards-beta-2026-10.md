---
title: Claude Dashboards（2026-10 Beta）：用自然語言建會更新的儀表板，每張圖可看 SQL 與更新時間
date: 2026-10-19
tags: [ai/claude, ai/workflow]
source: https://www.aiposthub.com/claude-dashboards-live-data-20261009/
official_refs:
  - https://support.claude.com/en/articles/17454700-get-started-with-claude-dashboards
  - https://support.claude.com/en/articles/14503520-available-beta-and-research-preview-features
  - https://claude.com/resources/articles/dashboards-and-motion
  - https://x.com/claudeai/status/2108271552991252810
type: article
status: processed
asset_value: 51
---

Anthropic 於 2026-10-08 宣布 Claude Dashboards 進入 beta：付費使用者在 Claude 對話中接上資料來源，用自然語言要求圖表，Claude 撰寫 SQL 查詢並產生會隨資料更新的儀表板，作品存放於 Artifacts。

> ⚠️ 官方連結未經本次核對（2026-10-19）；內容經 aiposthub（Philo，2026-10-09）單一媒體轉述，原始出處為 Anthropic 官方 support 文件與公告。Beta 規格、方案範圍可能變動，以下方案資訊為「截至 2026-10-09」。作者未實測。
>
> 📎 2026-10-19 Team Lead 核對：已讀官方 support 頁〈Get started with Claude Dashboards〉，**方案（Pro／Max／Team／Enterprise beta）、Enterprise 預設關閉由擁有者在 Organization settings > Artifacts 啟用、可用自訂角色限制群組、五個資料倉儲、預設私人、計入方案用量**皆與本條一致。該頁**未提**雲端程式執行／檔案建立設定（本條該項出處為 Artifacts 指南，維持原標示），也**未說明**資料刷新頻率；官方定位為快速探索用，深入分析建議匯出到其他工具。其餘 official_refs 未核對。

## T（核心論點）

Claude Dashboards 把「提問 → 查資料 → 畫圖」接成一條流程，但真正讓它可用的設計是：**每張圖都能看到 Claude 寫的 SQL 查詢與資料最後更新時間**。圖表做得快與數字算得對是兩件事——查詢攤開，團隊才能指出定義不符（例如「新客戶」算首次註冊還是首次付款）再請 Claude 修改，而不是收到一張無法追問的截圖。這與本庫「AI 產出要可驗收」的主線一致（見 [[ai-era-quality-assurance-test-oracle]]）。

## B（背景）

**適用：**
- 臨時探索問題（官方定位：例如比較本週註冊與上月），需要深入分析時再送到下游工具
- 已有資料平台或已連接 app 的 Pro／Max／Team／Enterprise 帳號

**不適用 / 注意：**
- 🔴 **Free 方案不在開放範圍**（截至 2026-10-09）；能用其他 Artifacts 範本不代表能建資料儀表板
- 🔴 **Enterprise 預設關閉**，需組織擁有者啟用
- Beta：官方 beta 功能說明提醒，可靠性與成熟度可能與正式功能不同
- 方案範圍與 [[claude-cowork-merged-into-chat-2026-09]] 的「先 Pro/Max」合併順序不同，勿混用

## R（關鍵細節）

### 方案與管理（官方，經轉述）

| 項目 | 官方安排 |
|---|---|
| 方案 | Pro、Max、Team、Enterprise（beta）；Free 不含 |
| Enterprise | 預設關閉；組織擁有者在設定中啟用；可用**自訂角色**限制哪些群組能建立 |
| 用量 | 計入方案使用限制 |
| 作品起始狀態 | **私人**；分享受組織設定約束 |
| 可驗證性 | 每張圖可看 Claude 撰寫的 SQL 查詢與資料最後更新時間 |
| 存放位置 | Artifacts；可從 Artifacts 分頁選 **Dashboards 範本** |
| 前置開關 | 依官方 Artifacts 指南，作品功能需啟用**雲端程式執行與檔案建立**；組織方案由管理設定控制 |

### 資料來源與下游工具（官方公告，經轉述）

- **資料平台**：Amazon Redshift、BigQuery、ClickHouse、Databricks、Snowflake
- **已連接的 app**：例如 Salesforce
- **可送往的後續分析工具**：Amplitude、Grafana、Hex、Mixpanel、Omni、Perplexity、PostHog、Sigma
- **「即將支援」**：Looker、monday.com、Tableau——🔴 預告不等於今天可用
- 官方示範：以一年自行車共享資料建立看板，再把內容轉成動畫（示範資料，非實測）；公告網址為 `dashboards-and-motion`，Motion 的功能細節原文未展開，本條不臆測
  - 📎 2026-10-19：Motion 細節已另立 [[claude-motion-beta-2026-10]]（官方公告頁經 Team Lead 核對）。⚠️ 方案不同：**Motion 只開 Team／Enterprise**，Dashboards 另含 Pro／Max，兩者不能互推；一般 Artifact 則是 Free～Enterprise

### 官方提問建議

- 先描述**想理解什麼**，而不只是指定圖表類型（「比較本月各來源的註冊表現」優於「畫註冊圖」）

## 作者觀點（aiposthub／Philo）

- 最適合承接「臨時探索問題」；穩定的管理指標、固定財務口徑仍留在既有 BI（長期定義、權限、維護責任）
- 官方公告**沒有承諾各連接器統一的更新頻率**（作者依公告推論），團隊需自行確認各來源如何同步；某通路突然歸零可能是同步中斷而非業務變化
- 導入第一步：挑一個**已有可靠答案**的小問題讓 AI 重算，與既有報表比對；提問時交代退款是否扣除、測試帳號是否排除、時區、歸因規則
- 加篩選器可能改變分母，修改後要回頭看查詢，不能只確認按鈕出現
- 成效不看生成了幾張圖，而看問題多久得到確認、錯誤多久被發現

## C（連結與應用）

- [ ] 若要試用：先確認方案與組織是否開放、雲端程式執行與檔案建立是否啟用
- [ ] 第一張圖選已知答案的指標，逐條核對 SQL 的定義（分子、分母、時間範圍、排除條件）
- [ ] 分享前把指標名稱寫完整，並保留更新時間判讀
- [ ] 待追蹤：Looker／monday.com／Tableau 正式支援時間；Beta 結束後方案範圍是否變動

## 原始摘要

- **aiposthub〈Claude Dashboards 上線：一句話做出公司儀表板，為什麼還要檢查背後的查詢？〉（Philo，2026-10-09）**：整理官方使用指南、beta 功能說明與公告；主張查詢與更新時間是可信度關鍵、臨時探索與正式 BI 分工。文末兩段 aiposthub Academy 付費課程推廣未收；同站延伸閱讀未收。剪藏另有一份逐位元相同的重複檔（`… 1.md`），僅標記處理、不另建條目。

## 相關連結

- [[claude-design-tool]] — 同為 Artifacts 範本、同受組織 Artifacts 開關控制；Claude Design 獨立網站 12-14 關閉後作品也改存 Artifacts
- [[claude-cowork-merged-into-chat-2026-09]] — One Claude 整合：作品統一收進 Claude 對話與 Artifacts 的前一階段
- [[claude-2026-beginner-guide]] — Artifacts 基本概念
- [[ai-era-quality-assurance-test-oracle]] — 「每張圖附查詢」＝讓 AI 產出可被驗證的同一原則
- [[claude-platform-opentelemetry-beta]] — 同批（2026-10）Claude 平台 beta：可觀測性那一側
- 📎 2026-10-19：[[claude-motion-beta-2026-10]] — 同一頁公告的 Motion（Team／Enterprise beta，方案與本條不同）
