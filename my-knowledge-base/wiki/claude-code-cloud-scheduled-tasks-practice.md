---
title: Claude Code 雲端 session＋Scheduled tasks（原 Routines）實戰：什麼該丟雲端、排程 prompt 怎麼寫、怎麼驗收——台股盤前整理案例
date: 2026-11-19
tags: [ai/claude, ai/workflow, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10423566（iT 邦幫忙〈Claude Code 雲端實戰篇0：拿送的美金 250 額度做一個每天自動跑收集股票相關資訊的工具(總覽)〉；作者 GitHub timwei0801，開發測試 2026-10-07～10-15，Max 方案）
source_2: https://ithelp.ithome.com.tw/articles/10425144（同系列〈雲端實戰篇1：哪些事該丟雲端、哪些留本機，還有第一個任務的 prompt 該怎麼下〉；官方文件 2026-10-21 查證、額度驗證 2026-10-23）
source_3: https://www.aiposthub.com/claude-routines-workshop/（aiposthub／Philo〈Claude Code Routines 怎麼規劃？官方工作坊的主動工作流與交付驗收〉，2026-11-11；官方文件 2026-11-11 讀取）
official_refs:
  - https://code.claude.com/docs/en/routines
  - https://claude.com/code-with-claude/session/ldn-build-a-proactive-agent-workflow-with-claude-code
  - https://github.com/timwei0801/claude-cloud-premarket-brief
type: article
status: processed
asset_value: 132
---

一位 iT 邦作者拿 Claude Code 雲端 session 上線時送的美金 $250 雲端額度，從零做一個「每個交易日 08:07 自動整理台股盤前資訊、送到 Telegram」的工具，把雲端 session 與排程（Routines）實際跑了一輪；再配一篇 aiposthub 對官方 Scheduled tasks 文件與工作坊的整理。本條目合併三篇。

> ⚠️ **來源性質**：篇 0／篇 1 是**作者第一手實作紀錄**（有程式碼 repo、踩坑檔 `docs/PITFALLS.md`、自寫核對程式），但為單人、單一專案、約一週的觀察；花費是 `/usage` 的 **API 牌價估算，不是帳單**。「排程不扣雲端額度、扣訂閱用量」是作者實測＋官方文件兩句原文的推論（官方文件**沒提**那筆送的額度）。aiposthub 篇為**二手整理**，工作坊影片內容未逐段核對。三篇都未經本庫回官方文件核對。報告內容只整理公開資訊，**不構成投資建議**（作者聲明）。

## T（核心論點）

> **雲端 session 是「做東西的地方」，不是「放東西的地方」。** 適合有頭有尾、材料在 GitHub、你不用在電腦前的工作；要每天跑，靠排程（每次開一個新的雲端 session）。

三個支撐點：

1. **丟哪裡只問兩題**：需不需要你的電腦（本機檔案、本機設定、你這邊的網路）？需不需要你一直在旁邊回答？**兩個都不需要 → 雲端**；有一個需要 → 本機（人不在電腦前再用 Remote Control）
2. **本機規劃、雲端執行**（官方建議 "Plan locally, execute in the cloud"）：規格在本機寫好、推上 GitHub → 雲端照規格做、開 PR 不合併 → 回本機審 PR 再合併
3. 🔑 **數字由程式算，AI 只照抄**：7 份有數字的報告、每份 42 個數字由獨立核對程式比對，**AI 寫出來的數字全對**；被擋時寫「無資料」並註明缺少，沒有自己填——關鍵在設計，不在模型

## B（適用場景）

**適用：**
- 想讓 Claude Code 每天／每週自動跑一件有明確交付物的事（資料整理、文件摘要、檢查部署），且電腦不必開著
- 已有 Pro／Max 訂閱、材料能放 GitHub 的個人或小團隊
- 判斷一件事要用本機、雲端 session、Remote Control 還是排程

**不適用 / 注意：**
- 🔴 **不能當常駐服務**：閒置幾分鐘主機就暫停、之後可能回收；回收後對話紀錄會還原，但**背景子代理、指令、`/loop` 排好的下一次都不會還原**
- 🔴 **每次都是新的 clone**：上次留下的檔案只有推回 GitHub 的才在（作者每跑一次就 commit 一次當日資料，下一版要算「法人連續買賣超」也只能讀 repo）
- 🔴 **本機設定帶不過去**：`~/.claude/CLAUDE.md`、自己裝的 skills、`claude mcp add` 加的 MCP，雲端都看不到——要用就 commit 進 repo。另見 [[claude-code-less-is-more-opus-5-5]]：projects 派出的雲端 session **不套用 repo 的 deny 規則、也不執行 hooks**
- 🔴 **雲端主機的位置會被擋**：台灣證交所擋雲端主機，openapi 擋頁回 **HTTP 200**、www 回 **307 無轉址目標**——只看狀態碼會誤判成功；反覆測試還會擋得更嚴（冷卻約 15 分鐘）
- 🔴 **GitHub 是單點**：10/7 GitHub 出事約半小時，雲端 session 拿不到程式、排程存不回結果，整條流程卡住
- 排程最短間隔一小時；作者撰文時（10 月）仍是 research preview

## R（操作步驟）

### 1. 先分清三種「排程」與三種「跑法」

**名稱**：官方文件（2026-11-11 讀取）標題已是 *Automate work with scheduled tasks*，註明原名 routines；舊網址仍可進入（aiposthub）。

| 排程類型 | 在哪跑 | 電腦關掉 | 備註 |
|---|---|---|---|
| **雲端 Scheduled tasks**（原 Routines） | Anthropic 雲端，每次開新 session | 照跑 | 觸發：**時間排程／API 呼叫／GitHub 事件**（如新 PR）；設定＝提示＋repo＋環境＋連接器 |
| Desktop 排程 | 你的電腦 | 停 | 官方與雲端任務分開列 |
| CLI `/loop` | 你的電腦、目前 session | 停 | 🔴 作者本機排 08:05 的監看，**Mac 睡著計時就停** |

| 跑法 | 程式在哪 | 電腦關掉 | 本機設定 | 要 GitHub | 網路 | 權限模式 |
|---|---|---|---|---|---|---|
| 本機 | 你的電腦 | 停 | 可用 | 不用 | 你的網路 | 全部可選 |
| 雲端 session | Anthropic 主機（Ubuntu、4 核、16 GB） | 繼續跑 | 只有 repo 裡的 | 要 | 預設只連套件庫、GitHub 白名單 | 只有 Auto／Accept edits／Plan |
| Remote Control | 你的電腦 | 電腦與 session 都要開著 | 可用 | 不用 | 你的網路 | 三種＋Manual |

搬移方向：雲端 → 本機用 `--teleport`；終端機 session **不能推上雲端**；桌面版可把本機 session 接到雲端（對話濃縮成摘要帶過去，作者未實際送出）。

### 2. 丟雲端前的四個檢查

1. **材料都在 GitHub 上嗎？** 雲端 clone 的是 GitHub 分支，不是你電腦裡的版本——還沒 push 的先 push
2. **說得出完成條件嗎？**（例：PR 開好、測試全過）說不出來先留本機用 Plan 模式想清楚
3. **中間不用你一直回答嗎？** 要一直問你的留本機
4. **成品有地方放嗎？** 分支、PR、或外送到 Telegram；沒地方放，主機回收就沒了

### 3. 第一個雲端任務的 prompt（作者 v1 原文）

```
請照 docs/SPEC.md 實作這個 repo 的第一版。先讀 CLAUDE.md 和 docs/PITFALLS.md。

1. 先寫 python -m brief check，在這台雲端機器上測每個資料來源和 api.telegram.org 連不連得到，結果（狀態碼、耗時、錯誤訊息）寫進 docs/CONNECTIVITY.md。連不到的照實記錄，不要想辦法繞過。
2. 接著實作 collect 和 send。把第一次抓到的真實回應存成 tests/fixtures，測試不打網路，全部要通過。
3. 實際跑一次 collect（報告日用今天），把 JSON 存進 data/。
4. 用 send 送一則測試訊息到 Telegram：「tw-premarket-brief 測試：<collect 的結束碼>」。環境變數沒設就跳過，並在 PR 說明寫出來。
5. 遇到任何意外，在 docs/PITFALLS.md 加一列。

分支 feature/v1，做完開 PR，不要 merge。commit 不要加 Co-Authored-By。規格沒寫清楚的地方列在 PR 說明裡，不要自己決定。
```

每句的用意：

- **「先讀 CLAUDE.md 和 PITFALLS.md」**——雲端每次新 clone，不知道之前討論過什麼，背景要寫在 repo 裡
- **「在這台雲端機器上測連線、連不到照實記錄、不要繞過」**——本機通不代表雲端通（後來證交所真的擋了）
- **「測試不打網路、真連線只跑一次」**——fixture 用第一次抓到的真實回應
- **「意外寫進 PITFALLS.md」**——雲端看到的事要落檔，主機回收就沒了
- 🔑 **「規格沒寫清楚的列在 PR 說明，不要自己決定」**——作者認為最有用：v1 列了 27 個待決事項、各附暫時做法；審完逐條決定後寫成 v1.1 的 prompt，並把上一輪雲端寫下的 `CONNECTIVITY.md` 加進「先讀」清單——**上一輪的檔案變成下一輪的背景**

開跑前確認：規格已 push、雲端環境網路**在開 session 之前**加好網域（執行中改設定實測不生效，要新 session）、權限模式與模型選好。🔴 **要丟出去就關電腦的任務別選 Plan 模式**——它會停下來等你核准。

### 4. 排程（Scheduled task）怎麼寫、怎麼設

- **提示詞自帶完整背景**：要讀什麼、時間範圍、去重方式、結果放哪、**失敗時怎麼回報**——新的雲端工作不會記得你上一段聊天講過什麼（aiposthub）
- **第一輪只交草稿**：挑一個能核對的小任務（例：讀指定 repo 新文件變動、輸出帶連結的摘要草稿），手動跑一次確認再開排程；確認來源與格式後才授權寫入（aiposthub 建議，非官方規定）
- 🔴 **表單預設值要逐一改**：作者建排程時環境預設帶到舊的「遊戲開發」、連接器預設帶 3 個；官方文件提醒**排程執行時連接器可不經詢問就寫入**——只保留需要的
- 作者的細節：排程主機 `python3` 指到 3.11（沒裝 requests），開發 session 用 3.13，所以在指示裡**寫死** `/usr/bin/python3.13`；改報告規則只需改 repo 檔案並 push，排程隔天就照新規則跑
- **金鑰**：放在標頭／內容的 API 用環境的「API 憑證」（金鑰存在 Anthropic、送出時代加，Claude 看不到）；Telegram token 放在網址裡，所有憑證類型都放不進，只能用環境變數（設定頁寫明「任何使用此環境的人都能看到」）→ 開**可隨時撤銷的專用 bot**

### 5. 驗收：綠燈不代表成功

- 🔴 **執行紀錄的綠燈只代表主機正常啟動與結束**（官方文件）；作者第一次手動執行有網址被擋，綠燈照亮，報告的 ETF 法人寫「無資料」
- 點進執行歷史看：有沒有真的開始、是否卡住、輸出是否符合指定條件；保留失敗回報與執行紀錄——資料格式、網站、權限都會變（aiposthub）
- **獨立核對**：作者另寫核對程式從台灣向證交所重抓一次，比對每份報告 42 個數字；另檢查有無「看好」「目標價」用語與免責聲明
- 連線檢查不能只看狀態碼，要檢查回應內容是不是擋頁

### 6. 費用與額度（作者實測，截至 2026-10-23）

| 開法 | 扣哪裡 |
|---|---|
| 自己開的雲端 session（桌面版／網頁／`claude --cloud`） | 扣送的雲端額度 |
| 排程任務本身、**在排程執行紀錄裡接著問** | **不扣額度，扣訂閱用量** |
| 沒有額度的人 | 都扣訂閱用量，雲端 VM 不另收費（官方） |

- 開發三版合計美金 $22（v1 $11／40 分鐘、118 測試；v1.1 $5／20 分鐘；v2 $6／40 分鐘、181 測試），皆 Opus 5.5；排程用 Sonnet 5.5，每次 1～2 分鐘、約 $0.34～0.50（估算），月約 $10 等值
- 錢大多花在「讀」（快取讀取 47 萬～74 萬 token，輸出才 400～500）
- 想知道單次花多少：在**那次執行裡**輸入 `/usage`（雲端點數只顯示到整數美元）
- 未量到：自己開的 session 扣額度後是否**同時**吃訂閱用量

### 7. 雲端 session 自己做對的事（作者觀察）

- 實作時讀到證交所備註，**回頭指出規格會重複計算**外資自營商
- v1 完成後另派子代理對照規格審查，修了 3 個 bug
- 兩篇新聞同一數字對不上（88.3% vs 99%），查證不了就**兩個都不寫**並記進踩坑
- 被擋時寫「無資料」，不湊數

## C（關鍵行動）

- [ ] 把想自動化的事用「需要我的電腦嗎？需要我一直回答嗎？」分流；電腦不開也要跑的，不要用本機 `/loop` 或 Desktop 排程
- [ ] 雲端任務的 repo 內放 `CLAUDE.md`＋規格＋`PITFALLS.md`；prompt 開頭叫它先讀，結尾寫「開 PR 不合併、規格不清列在 PR 說明」
- [ ] 建排程時逐項檢查環境與連接器預設值，只留需要的寫入權限；第一輪只交草稿
- [ ] 要連國外會擋的網站，先在雲端跑一支小程式測連線（檢查內容而非狀態碼），網域在開 session 前加
- [ ] 每個排程配一個獨立核對（程式或人工抽查），不信綠燈
- [ ] 本庫 `~/notes` 若要試雲端排程：注意 deny 規則與 hooks 在 projects 雲端 session 不生效

## 原始摘要

- **iT 邦〈雲端實戰篇0〉（總覽）**：為什麼測、五點結論、每天收到的 Telegram 盤前整理樣貌（大盤／法人／融資融券／類股／5 檔 ETF／除息／新聞／行事曆，休市只送一行）、給股民的限制（無個股、不推薦不預測、新聞只看三家）；雲端 session vs 本機 vs Remote Control 表；分工（本機寫規格審 PR → 雲端寫程式 → 排程每天跑）；三版本開發表；**11 個坑**（證交所擋雲端且回 200、預設網路連 pypi 都不行且執行中改設定不生效、排程主機 python 3.11、Telegram token 放不進 API 憑證、截圖拍到 token 而 Claude Code 拒絕從截圖讀 token、GitGuardian 誤判假 token、送的額度不能用在排程、`/usage` 看單次費用、表單預設值、綠燈不代表成功、GitHub 單點）；42 數字核對結果；後續系列預告（#2～#6）。文末 iThome 影音推廣不收。
- **iT 邦〈雲端實戰篇1〉**：兩題分流法；官方比較表（2026-10-21）；雲端做不到的三件事（常駐、保留檔案、帶本機設定）；專案 12 件事的落點表（含本機 `/loop` 監看因 Mac 睡眠失敗）；四個檢查；v1 prompt 全文與逐句理由、v1.1 prompt 開頭；權限模式提醒；額度扣法補測表（四種開法）與官方兩句原文。
- **aiposthub〈Claude Code Routines 怎麼規劃？〉（Philo，2026-11-11）**：Routines → Scheduled tasks 改名；觸發方式三種；與 Desktop 排程、CLI `/loop` 分開；官方工作坊頁（London「Build a proactive agent workflow with Claude Code」）當用途入口、設定以文件為準；提示詞自帶背景、第一輪交草稿、連接器可能含寫入、看執行歷史。站內延伸閱讀未收。

## 相關連結

- [[claude-code-less-is-more-opus-5-5]] — 🔴 projects 雲端 session 不套用 repo deny 與 hooks；雲端乾淨機器、network secret 的安全面
- [[claude-code-projects-redesign-2026-09]] — 同樣只在雲端跑的 Threads（協調者＋平行雲端執行緒）
- [[claude-code-billing-usage]] — `/usage` 金額估算與歸因
- [[claude-code-commands-reference]] — `/schedule`（建立雲端排程）等指令
- [[cowork-memory-browser-scheduled-tasks]] — Cowork 側的排程任務：「資料放哪裡」決定能不能電腦關著跑
- [[claude-cowork-stock-tracking-scheduled-tasks-case]] — 同題材（股票追蹤）但用 Cowork 排程＋儀表板，對照本條的 Claude Code＋GitHub＋程式核對路線
- [[grok-bot-routine-automation-ladder]] — 「先做對一次、再固化、最後才排程」與排程五要素（失敗 vs 空結果分開寫），與本條「第一輪只交草稿、被擋寫無資料」同一套紀律
- [[agent-task-system-background-scheduling]] — 工程面：時區、漏跑、重試
- [[boris-cherny-prompt-to-verification]] — 賭注從提示移到驗證：本條「數字由程式算、另寫核對程式」的實例
