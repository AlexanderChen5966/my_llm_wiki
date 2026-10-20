---
title: Claude 大改版：Cowork 併入 Chat，介面從三分頁縮成兩入口
date: 2026-09-17
tags: [ai/claude, type/concept]
source: https://www.bnext.com.tw/article/92279/claude-cowork-merged-claude-docs-slides
type: article
status: processed
asset_value: 60
---

Anthropic 於 2026-09-16 宣布將上線僅 8 個月的 Claude Cowork 併入一般對話 Chat：介面由 Chat／Cowork／Code 三分頁縮減為 Claude 與 Claude Code 兩個入口，使用者不再需要自行判斷任務該用哪個介面，改由 Claude 自行判斷該給文字回答、文件、簡報，還是長時間代理工作。

## T（核心論點）

架構級變化的本質是**從「使用者自選模式」改為「Claude 自行判斷該用哪種能力」**：Cowork 獨立分頁消失，相關能力直接由 Claude 對話承接。Anthropic 給的理由是一項工作經常橫跨多種類型（研究 → 報告 → 簡報），分散在不同工具反而要不斷重複交代背景，因此從「先選工具」改成「直接交代工作」。

同日並推出 beta 的 **Claude Docs**（對話中共同撰寫、編輯文件）與 **Claude Slides**（依對話與資料生成簡報，可下載 PPT/PDF），Claude Design 也整合進一般對話。

> 📎 2026-10-19 後續：Claude Design 獨立網站將於 2026-12-14 關閉、作品改存為 Artifacts，見 [[claude-design-tool]] 文末補充段。

**注意區分**：Claude Slides（對話中直接生成簡報）與 Claude for PowerPoint（裝在 PowerPoint 裡的增益集，修改既有簡報檔）是兩個不同工具，不要混用。

## B（適用場景）

**適用：** 所有原本使用 Cowork 獨立分頁的使用者；正在依既有 Cowork 教學操作、卻找不到入口的人；評估是否需要調整既有 Cowork 排程/記憶/技能工作流程的人。

**不適用 / 注意：**
- 本次為介面合併，**不是功能砍除**：既有對話紀錄、Projects、Artifacts、Connectors、Skills 全部保留，不需要重新設定。
- Claude Docs／Claude Slides 皆為 beta，格式相容性與穩定度未有官方細節說明，納入既有工作流前仍需自行測試。

## R（合併前後對照）

| 項目 | 合併前 | 合併後 |
|---|---|---|
| 入口 | Chat、Cowork 各自獨立分頁 | 單一 Claude 對話（另有 Claude Code） |
| 任務判斷 | 使用者自行選擇該用哪個介面 | Claude 自行判斷該用哪種能力 |
| 工作銜接 | 不同介面各自進行，需重複交代背景 | 同一段對話延續 |
| 產出 | 依不同工具處理 | 文件、簡報、設計都能在對話內完成 |

**推送時程**：先 Pro/Max（未來幾週，涵蓋網頁/桌面/行動版），Team/Free 隨後，Enterprise 由管理員決定啟用時程（企業版變動會提前至少 30 天通知）。

## C（關鍵行動）

- [ ] 若原本依賴 Cowork 獨立分頁操作，重新確認新架構下功能的實際入口位置（尤其排程任務、Record a skill、記憶設定等原本掛在 Cowork 分頁下的功能）
- [ ] 既有 Cowork 教學類筆記，優先檢查是否還以「獨立分頁」為前提描述操作步驟
- [ ] 想用共同編輯文件/簡報功能的，留意 Claude Docs／Claude Slides 目前仍是 beta，先小範圍測試再依賴

## 原始摘要

Anthropic 2026-09-16 宣布將 Claude Cowork 併入一般對話 Chat，介面由 Chat/Cowork/Code 三分頁縮減為 Claude 與 Claude Code 兩入口，使用者不再需要自行判斷任務該用哪個介面。同步推出 beta 的 Claude Docs（共同編輯文件）與 Claude Slides（對話生成簡報，可下載 PPT/PDF），Claude Design 也整合進對話。既有對話紀錄、Projects、Artifacts、Connectors、Skills 全部保留不需重新設定。先開放 Pro/Max，Team/Free 隨後，Enterprise 由管理員決定。

## 相關連結

以下 6 篇既有 Cowork 相關條目，因本次架構變化而在對應段落加註 ⚠️ 警語（原內容不刪除不改寫）：

- [[claude-ai-guide-cowork-skills-token]] — T 層「三模式生態系（Chat/Cowork/Code）」敘述已過時
- [[claude-2026-beginner-guide]] — 「三種模式」與「Cowork 桌面版限定」敘述已過時
- [[claude-cowork-setup-guide]] — 影響最大：整篇前提是 Cowork 為獨立分頁/模式
- [[cowork-memory-browser-scheduled-tasks]] — 記憶互通「僅限雲端執行的 Cowork 任務」區分前提需重新確認
- [[claude-cowork-record-a-skill]] — 「桌面版『+』選單」入口位置可能已變
- [[claude-cowork-one-on-one-management]] — 影響最小，Cowork 現指能力而非分頁
- 📎 2026-10-19：[[claude-dashboards-beta-2026-10]] — 同屬「作品收進 Artifacts」的後續；⚠️ Dashboards 方案範圍（Pro／Max／Team／Enterprise，Enterprise 預設關閉）與本條「先 Pro/Max」的合併順序不同，勿混用

## 補充資料：2026-10-06 後續——新任務一律雲端執行

> 來源：黃若彤〈Claude Cowork改上雲端！10/6起Pro、Max用戶新任務不能再選本機，注意事項一次看〉，數位時代，2026-10-07，https://www.bnext.com.tw/article/92536/claude-cowork-cloud-pro-max-october-6 （官方說法經數位時代轉述）。原文把這次改動放在「Chat 與 Cowork 正合併為單一 Claude（先 Pro/Max）、輸入框不再有切換鈕」的脈絡下，是 9/16 合併的下一步：使用者連「在哪裡執行」也不再自選。

**一句話**：運算一律在雲端；本機檔案經 Desktop App 橋接、要用時才複製單檔上雲；Desktop 一關，雲端任務照跑，但碰不到本機檔案。

- **生效範圍**：2026-10-06 起，Pro／Max 新啟動的任務移除「Only on your computer」選項，**不能退回舊制**；Team 的雲端 Cowork 仍是 beta；Enterprise 要管理員開啟。
- **既有使用者**：原本設「僅本機」的人，下次開 Desktop 會自動移到雲端並看到通知；**已在本機開始的任務與已在本機執行的排程任務，留在原電腦完成**（畫面有橫幅「只在此電腦上執行」）。
- **本機檔案的存取條件**（三者都要）：① 只能透過 Desktop App 存取**事先連結**的資料夾；② 只在 App 開著時有效；③ 這個雲端 session **必須是從桌面版發起，或是排程任務**。用到時才複製「該檔案的單一複本」上雲；刪 session 時複本依資料保留政策一併刪除；是否拿去訓練依 Settings > Privacy 的「Help improve our AI models」決定。
- **仍需要 Desktop App 的能力**：本機檔案、本機連接器與用到它們的外掛、內建瀏覽器（逐步推出）／Claude in Chrome、Computer use。
- **想完全在本機跑**：官方建議改用 Claude Code（Desktop）。搬法：在 Cowork 任務選「Download task data」→ 解壓後用 Claude Code 開啟 → 執行 `/port-task-data`。要全部備份就到 Desktop 首頁「Export your Cowork data」。

**對既有條目的影響**（已各加附註回連）：[[cowork-memory-browser-scheduled-tasks]]（排程任務需要本機檔案時的機制改變，但「要遠端常駐就別依賴本機檔案」的取捨仍成立）、[[claude-cowork-setup-guide]]（「直接讀寫本機實體檔案」的比較列只適用 10/6 以前）。

⚠️ 數位時代文內夾帶的「圓剛科技」業配長文不收。
