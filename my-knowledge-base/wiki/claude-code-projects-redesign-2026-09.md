---
title: Claude Code Projects 重構（2026-09-17）— 從資料夾到協調者 + 平行 Threads
date: 2026-09-23
tags: [ai/claude, ai/agent, type/concept]
source: https://www.bnext.com.tw/article/92300/claude-code-projects-redesign-parallel-threads; https://www.aiposthub.com/claude-projects-redesigned-folder-to-conversation-parallel-threads/
type: article
status: processed
asset_value: 60
---

Anthropic 於 2026-09-17（台灣時間 09-18）改版 Claude Code 的 Projects，官方標題
《Projects redesigned: from folder to conversation》。從「放檔案的資料夾」變成
**一個永不阻塞的協調者（Coordinator）＋多條隔離雲端 Threads** 的分層多代理體系。

⚠️ **來源可信度差異（請連同事實一起記）**：本條目的**事實基準**是數位時代報導（新聞體，引 Anthropic 官方公告與 VentureBeat，
資格與限制交代最清楚）；aiposthub 深度解析僅取其**架構拆解與競品對比**，
因該篇帶明顯行銷語氣，🔴 且**文中舉例 Coordinator 用「Claude 3.7 / 3.5 Sonnet」為舊型號誤植，屬作者臆測而非官方說法——
該篇的所有型號敘述本條目一律不引用**。

## T（核心論點）

🔑 **這次改版把「拆工作」這件事本身從人手上收走了。**
過去一個案子要同時改好幾個地方，得由人自己切分、分別開多個對話、最後把結果拼回來；
新版的官方說法是「**工作線做事，Claude 指揮**」。

🔑 **VentureBeat 一句話點出與 2024 年 6 月舊版 Claude Projects 的差別：
舊版幫 Claude 記住「你給它的脈絡」，新版記住並協調「它自己正在做的工作」。**
這也是為什麼它不只是功能更新——記憶的對象從靜態輸入變成了動態工作狀態。

🔑 **可遷移的架構要點（不限於 Claude Code，自建 Agent 編排同樣適用）：
頂層是一個不參與執行、因此永不阻塞的協調者；底層是一群在獨立分支上作業的隔離 worker；
衝突不在黑箱裡覆蓋，而是退回既有的 Git merge conflict 流程由人仲裁。**
最後那一點是關鍵設計選擇：**它沒有發明新的協調機制，而是把協調責任交還給既有的工程流程。**

## B（適用場景）

**適用：** 複雜重構、跨系統 API 升級、指標導向的優化任務（如「把結帳 P75 延遲降 30%」）、可拆成多條獨立工作線的團隊協作。

**不適用 / 注意（截至 2026-09-17 的官方說明）：**
- 🔴 **Thread 只能在雲端執行**——碰不到本機檔案與工具，也連不進公司內部網路。
  對倚賴內網與本機工具的企業團隊，本機執行才是關鍵，而 Anthropic 只說「很快」，未給時間表
- 🔴 **beta 範圍有限**：9/17 起先開放部分 Claude Pro / Max 用戶，條件是使用 Claude Code **雲端 session**、
  且網頁版或桌面版**沒有既有專案**；公告後一週內擴大至更多 Pro/Max 用戶，未取得資格者可登記候補
- 之後才推至 Claude 全線產品（含聊天介面與 Cowork）與 Team / Enterprise，**官方未給日期**；
  Pro/Max 既有專案維持現狀，屆時一併升級
- ⚠️ **用量**：不另外收費，直接扣既有訂閱用量；每條 thread 都是完整的 Claude Code session，
  **多條同時跑會更快觸及用量上限**

## R（操作步驟）

### 架構：雙層解耦

**Coordinator（協調者 / 幕僚長）**
- **不親自寫程式**，只負責理解需求範疇（scoping）、制定計畫、切分工作邊界、指派任務
- 綁定程式碼倉庫後會**主動建議**可以立刻接手的工作
- **永不阻塞**：因為不參與編譯與執行，隨時可以接新指令，動態調整既有 Threads 的優先順序或開新的

**Thread（雲端執行緒）**
- 每條都是 Anthropic 隔離容器中的**獨立 Claude Code 實例**，擁有獨立運算環境
- 各自建立獨立 Git 分支與 workspace 副本，在裡面改程式、裝相依套件、跑測試
- **內部還能再啟動 Subagents / Loops / Workflows**——Thread 不是單線程的葉節點
- 兩條 Thread 改到同一段程式碼時，**不在黑箱中覆蓋**，走一般 Git merge conflict 流程仲裁

### 官方兩個範例

1. **跨 Repo 廢棄 v1 API**：API、Web、Mobile 三個倉庫掛進同一專案，各開一條 Thread 改接新版、跑測試、送 PR；
   最後由 Coordinator 給出**依賴排序**指引：「先合併 API repo 的 PR #104，待後端發布完畢，再合 Web 的 PR #58 與 Mobile 的 PR #212。」
2. **降低結帳流程 P75 延遲 30%**：對不同服務呼叫點各開 Thread，分別做 profile、試快取策略、減少 N+1 查詢，
   各自跑 benchmark 驗證有效後才拉 PR。

### 共享記憶與資源庫

跨 Thread 的共享記憶會記三類內容（三類的區分本身就是可借用的分類法）：

| 類別 | 官方舉例 |
|---|---|
| 業務決策 | 發布日延至週五；當初為什麼砍掉匯出功能 |
| 團隊規範 | 動 Billing 模組前要抄送資深工程師 Alex 審查 |
| 技術偏好 | 測試用 Vitest 而非 Jest |

Claude 也會學使用者的溝通習慣（多久回報一次、多常開新 Thread、更新寫多細）。
新增的 **Library（資源庫）** 聚合兩類資產：使用者上傳的檔案（PRD、架構圖、API 文件）與 Claude 產出的成品。

### 算力配置

可**分別**為 Coordinator 與 Thread 設定模型與推理深度（effort），並查看專案專屬用量。
典型分配是協調者開高思考深度負責規劃、機械型 Thread 配輕量快速的模型。
（🔴 aiposthub 在此處舉的具體型號為舊型號誤植，本庫不引用；實際可選模型以產品內為準——
配置原則見 [[claude-code-model-and-thinking-depth]]。）

## C（關鍵行動）

- [ ] 尚未取得資格者可登記候補（Beta 表單 <https://claude.com/form/projects>）
- [ ] 取得資格後，**先確認任務是否真的能平行**再開多條 Thread——多開不等於更快，只是更快燒用量
      （判準見 [[claude-code-agent-teams-when-to-multiply]]）
- [ ] 把交辦方式從「打開 auth.ts 在第 45 行加一段」改成目標導向（要消除什麼問題、要達到什麼指標、要開 PR）
- [ ] ⚠️ 預留 **PR 審查頻寬**：半小時內跨三個倉庫開出 6 個 PR 時，瓶頸從「寫不出來」變成「來不及審」
- [ ] 需要碰本機檔案或內網的工作，短期內仍走原本的本機 Claude Code

## 競品定位（aiposthub 整理，取架構面）

| 維度 | 新版 Claude Projects | Cursor Projects (Anysphere) | Devin (Cognition) |
|---|---|---|---|
| 核心哲學 | 對話式協調者＋平行 Threads | 持久在線協調代理＋Subagent 矩陣 | 獨立自駕工程師（全自動工作站）|
| 並行架構 | 平行 Cloud Threads（各自獨立 Branch）| 數百至數千 Subagents 雲端並行 | 單一 Agent 內部子任務循環 |
| 多倉庫 | 原生支援跨 Repo | 透過 Context 支援關聯倉庫 | 支援連接多個 GitHub 倉庫 |
| PR 依賴排序 | **具備**（自動指示合併先後）| 透過 GitHub 整合追蹤 PR 狀態 | 自動開 PR，具 CI 修復能力 |
| 本地 / 雲端 | 雲端先行，官方預告支援本地 | 本地 IDE 與雲端虛擬機切換 | 純雲端沙盒 |

🔑 **值得記的不是誰贏，是兩家在同一個月獨立收斂到同一個形狀**：
Cursor 在 Anthropic 前一週推出「Projects 單一持久協調代理」，兩者都走
**頂層永不阻塞的協調代理 ＋ 底層獨立分支 worker ＋ 雲端非同步**。
⚠️ aiposthub 由此推出「逐次對話已死」的斷言帶行銷語氣，本庫記錄其論據（兩家架構趨同）而不採納其結論強度。
其他平行、非同步長時間開發工具還有 OpenAI Codex app、GitHub 的代理工具、Google Jules；
數位時代指出 Anthropic 的差別在於**把「一條持續進行的對話」放在最上層當協調者，開發者只跟這一個對話溝通**。

## 相關連結

- [[claude-code-agents-md-loading-rules]] — 同為 Claude Code 主題的 2026-09 變更（AGENTS.md 判定規則）
- [[claude-code-agent-teams-when-to-multiply]] — 何時該多開 AI：多開只在任務真的能同時進行時才划算
- [[claude-code-agent-teams-practical-examples]] — Agent Teams 的四個實戰模式（本機／會話層的對應物）
- [[agent-router-vs-coordinator-state-tracking]] — Router 與 Coordinator 的差別與狀態追蹤
- [[git-worktree-parallel-agents]] — 用 worktree 在本機達成「各自一份程式碼副本」的同一件事
- [[claude-code-model-and-thinking-depth]] — 分別為協調者與 worker 配置模型與 effort 的原則
- [[claude-cowork-merged-into-chat-2026-09]] — 同期的另一項 Claude 產品線架構變化

## 原始摘要

事實基準＝《Claude Code 推出新版「專案」功能》（數位時代，李先泰，2026-09-18，
資料來源含 Anthropic 官方公告、Anthropic Projects 功能說明與 VentureBeat）。
補充＝《Claude 專案大重構！》（aiposthub，Philo，2026-09-18，取 Coordinator/Thread 架構拆解、
跨 Repo 依賴排序範例、共享記憶三類、Library、競品對比表）。
⚠️ 本條目 asset_value 60 為門檻邊界案例：純新聞報導依 `rules/research-protocol.md` 本應排除，
之所以仍以 wiki 條目收錄，是因為第二篇提供了**可遷移到自建 Agent 編排的分層架構解析**（協調者不執行故不阻塞、
worker 隔離、衝突退回既有工程流程、記憶分三類），而不只是功能清單。
