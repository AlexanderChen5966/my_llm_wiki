---
title: Claude Code Artifacts：把產出變成一個會更新的網址（發佈／更新／分享／樣板／限制）
date: 2026-10-30
tags: [ai/claude, ai/workflow]
source: https://ithelp.ithome.com.tw/articles/10423413
type: article
status: processed
asset_value: 36
---

Claude Code 可把產出發佈成 claude.ai 的私人網址（artifact）：改內容時網址不變、對方打開就是最新版，取代「報告_最終版_V2」式傳檔。本條整理發佈前提、更新機制、分享權限、四種樣板與限制。

> ⚠️ **時效與性質**：來源為 iT 邦幫忙 Tim Wei（EY 顧問）「Claude Code 新手篇」系列 G9（2026-10-13 建立；published 欄空白）。**作者自註**細節依 **2026 年 10 月官方文件與 Claude Code v2.1.287**，並說「artifact 和樣板改得非常快（樣板還在 beta）」，之後請以官方文件為準。內容為個人經驗＋官方文件整理，作者自述 7～10 月做過約 37 個 artifact；本庫未獨立核對官方文件。

## T（核心論點）

**不傳檔案，傳一個網址。** 要給別人看的產出，用 artifact 發佈到 claude.ai 私人網址；它一開始只有你看得到、原地更新、每次發佈存成一個版本。官方判準（作者引述）：終端機文字不適合呈現產出時才用 artifact；作者自己的判準是「這份東西要給別人看，或我會回頭看很多次」。承接系列 G6：只給自己看用本機 HTML，要給別人看就做成 artifact。

## B（適用場景）

**適用：**
- 做完要給人看的報告（事故調查、進度會報、交接書）、方案並排比較、進度看板（長任務邊跑邊更新）、可互動小工具（試算、記帳、行程）、教學圖解（作者自己的分類）

**不適用 / 注意：**
- 🔴 **方案前提**：發佈需 **Pro 以上＋用 `/login` 登入 claude.ai 帳號**；用 **API key、Bedrock、Vertex** 使用 Claude Code 都**不能發佈**（會改存本機 HTML 或直接說不行）
- 範圍區分（本庫整理，非原文）：這是「Claude Code 發佈 artifact」的前提；[[claude-design-tool]] 依 10/23 官方說明頁為 Docs／Slides／Design 全方案（含 Free）可用，[[claude-dashboards-beta-2026-10]] 不含 Free——三者描述不同產品面，不矛盾，但**勿互相套用方案範圍**
- 只有單頁、沒有後端；外部圖片會被擋（圖須內嵌，整頁最多 16 MiB）；比純文字更花 token（官方建議圖表用 SVG、不用的互動不做、大量資料只放摘要）
- 「知道連結的任何人」＝任何人（免登入）；公司資料勿選

## R（操作步驟）

### 1. 發佈與開啟
- 一句話叫它做：「把這三個方案做成一頁比較，並排放」；做好會印出網址、瀏覽器自動開啟
- 終端機版：輸入框下方有 `⧉` 小標列出此對話做過的 artifact；`Ctrl+]` 開啟此對話最近一個
- Claude 判斷適合時可能**自己發佈**（官方說明，作者引述）
- 成品集中在 claude.ai「成品／Artifacts」頁（claude.ai 與 Claude Code 做的在同處）
- 與權限模式的關係（接 G8.5）：自己叫它做的新 artifact 發佈時**不跳詢問、不經 auto mode 審核**（因初始僅自己可見）；**Plan Mode 第一次發佈會先問**。見 [[claude-code-auto-mode-permissions]]、[[claude-code-plan-mode-permission-rules-layering]]
- 設定：`CLAUDE_CODE_ARTIFACT_AUTO_OPEN=0` 關閉自動開啟；`/config` 可整個關閉 Artifacts；專案 `CLAUDE.md` 的設計設定（顏色、字型、間距）優先於 Claude 自己的選擇

### 2. 更新（最容易踩雷）
- 同一對話內直接說「加一個細項，重新發佈」→ **同一網址**更新，對方開著頁面會直接看到新內容
- 每次發佈存成版本；頁面上方 **Share** 選單可指定別人看哪一版（改到一半可讓對方停在上一版）
- 🔴 **換了對話，沒給網址也沒用 `/artifacts` 附加，會建立新的 artifact 而非更新**（官方原文）。作者自己因此留下三份同名交接書（前兩份隔 6 天、內容 98% 相同，第三份多四成）
- 兩個解法：①把網址貼給它；②`/artifacts` 列出帳號下所有 artifact（從 claude.ai 帳號讀，`/clear` 後仍找得到），`Enter` 附加到目前對話、`o` 瀏覽器開啟、`c` 複製連結
- VS Code 內找不到 `/artifacts`（作者實測），改到「成品」頁找，把網址貼給 Claude
- 與交接檔不同（見 [[claude-code-session-handoff]]）：交接檔給下個對話的 Claude 看「做到哪」；artifact 網址讓它知道「要改哪一頁」

### 3. 分享與權限
| 方案 | 一般存取權可選 |
|---|---|
| Pro、Max | 「僅限您」或「知道連結的任何人」 |
| Team、Enterprise | 「只有受邀的人」「整個組織」「知道連結的任何人」（須管理員開啟對外分享） |

- 「知道連結的任何人」**對方免登入**（作者用無痕視窗驗證）
- 邀請特定人：輸入 email（**beta**），權限三級：可檢視（看、看留言）／留言者（加留言、下載頁面檔案）／可編輯；被邀請者須用該 email 登入 Claude
- 公開後，Claude 在每個對話第一次要改它之前會先問你
- Team／Enterprise：組織內留言可用 **Send to Claude** 或 `@claude`，Claude 會讀留言、回覆、必要時改頁面；個人方案受邀者可留言但不能在留言裡叫 Claude

### 4. 四個樣板（2026-09 起，作者稱 Dashboard 仍 beta）
| | 一般 artifact | Docs | Slides | Design |
|---|---|---|---|---|
| 怎麼改 | 請 Claude 改 | 直接打字、可多人同改 | 逐張直接改、可直接報告 | 畫布上直接改 |
| 帶走 | 傳網址 | Word、PDF、Markdown、Google Docs | PowerPoint、PDF、Google Slides | PDF、PowerPoint、HTML，單畫板 PNG |
| Claude Code 叫法 | 「做成 artifact」 | 「寫成一份文件」 | `/slides` | `/design` |

- 分辨：只要看→一般 artifact；要一起改、要帶走→樣板（人讀→Docs、上台→Slides、先看長相→Design）
- Docs 在 Claude Code 是 connector（`/mcp` 看得到 `claude.ai Claude Docs`）；**Docs 不能用 email 邀請（任何方案）**，Pro／Max 的 Docs 可連結分享、對方須有 Claude 帳號
- Dashboard 需先接資料來源（已連接的 app），詳見 [[claude-dashboards-beta-2026-10]]；動畫另見 [[claude-motion-beta-2026-10]]
- 作者實測（桌面版，單次）：Docs 截圖時已跑 4 分多鐘、Design 約 8 分鐘；Slides 完成後會列出「我自己決定的幾點」（如把系列地圖上待寫的 G8 當成已寫完）；Slides 與 Design 各留下一個空白 `Untitled`，需回成品頁刪除
- `/design` 是 G6 wireframe 的升級版：一次多方向畫板、瀏覽器內點元素即可改字／移位置（自動存檔）、畫板可匯出 PNG／PDF；官方流程「挑一個、調一下、再讓 Claude 實作」。作者帳號沒設定設計系統，配色是 prompt 裡指名的；樣板可接設計系統，React 元件可用 `/design-sync` 同步（作者標為進階、未細講）。工具細節見 [[claude-design-tool]]
- 作者事後重分類：規格書／驗收清單／交接書→Docs、進度會報→Slides、視覺方向／配色→Design、事故調查／試算／圖解→留在一般 artifact

### 5. 即時資料
- artifact 可在每次有人開啟時，透過**看的人自己的** connector 抓最新資料（例：GitHub 未處理 PR）；connector 細節作者留待 G11

## C（關鍵行動）
- [ ] 換對話要改既有 artifact 時，先貼網址或用 `/artifacts` 附加，避免產生同名多份
- [ ] 分享前確認方案可用的存取選項；公司／客戶資料不選「知道連結的任何人」，改用 email 邀請
- [ ] 把重複打的「做成 artifact，用我的配色，放這幾個區塊」寫進 `CLAUDE.md` 或變成 skill（作者下一篇 G10 的主題）
- [ ] 試過 Slides／Design 後回成品頁清掉空白 `Untitled`
- [ ] 以官方文件〈Share session output as artifacts〉核對方案與樣板最新狀態（本條為 2026-10 快照）

## 原始摘要

iT 邦幫忙 Tim Wei，Claude Code 新手篇 9（G9）。重點：artifact＝Claude Code 發佈到 claude.ai 私人網址的網頁，原地更新、版本化；需 Pro 以上＋claude.ai 登入；換對話要給網址否則新建；分享選項因方案而異；四個樣板（Docs／Slides／Design／Dashboard beta）；限制為單頁無後端、外部圖片被擋、較耗 token。來源頁尾的 iThome／iTplus 活動影音廣告（5G 資安、零信任、CipherTech、Nutanix KubeSummit）與作者簡介與主題無關，不收錄。

## 相關連結

- [[claude-code-newbie-guide]] — 同系列 G1（Claude Code 從零開始）
- [[claude-code-session-handoff]] — 同系列 G5：交接檔 vs artifact 網址的分工
- [[claude-code-plan-mode-permission-rules-layering]] — 同系列 G8：Plan Mode 第一次發佈會先問
- [[claude-code-auto-mode-permissions]] — 同系列 G8.5：自建新 artifact 不經 auto mode 審核
- [[claude-design-tool]] — `/design` 與 Claude Design 樣板
- [[claude-dashboards-beta-2026-10]] — Dashboard 樣板（需接資料來源，不含 Free）
- [[claude-motion-beta-2026-10]] — 同批 Artifacts 動畫範本（Team／Enterprise beta）
