---
title: Grok Bot 多 bot 分工：接力、平行與跨執行環境交接（何時才拆專職 bot、Cursor／GitHub／Origin 回收 PR、HarnessRouter 統一介面）
date: 2026-11-19
tags: [ai/agent, ai/workflow, type/how-to]
source: https://www.bnext.com.tw/article/92470/grok-bot-tutorial（《數位時代》韋惟珊〈Grok Bot 怎麼分工？多個 AI Bot 接力、平行執行任務實測〉，2026-10-02；作者個人試用）
source_2: https://www.aiposthub.com/grok-bot-cursor-coding-pr-video-update/（aiposthub／Philo〈Grok Bot 強化軟體開發：串接 Cursor、管理 PR，還能分享成果示範影片〉，2026-10-01；查詢日 2026-10-01）
source_3: https://www.aiposthub.com/harnessrouter-ai-news-20261025/（aiposthub／Philo〈HarnessRouter 串起多種程式代理：Grok Bot 如何把工作交給不同執行環境？〉，2026-10-25）
official_refs:
  - https://x.com/bot/status/2105373767568621895（Grok Bot 2026-10-01 官方公告，經 aiposthub 轉述，本庫未核對）
  - https://x.ai/bot/guides/grok-bot-for-engineering（官方工程指南 2026-09-10，經 aiposthub 轉述）
  - https://x.ai/bot/marketplace/engineering（工程 Marketplace，經 aiposthub 轉述）
  - https://cursor.com/docs/origin/pull-requests（Origin PR 說明，經 aiposthub 轉述）
type: article
status: processed
asset_value: 102
---

[[grok-bot-cloud-agent-product-overview]] 已寫了 Grok Bot 的產品架構（Outer／Inner Loop、幕僚長模式、BDK 代理通訊）。本條目合併三篇（raw-pipeline 路徑 ②，各篇 36／30／36，合計 102），只補**多個 bot（或多個代理）之間怎麼分工、怎麼交棒、交到哪裡**：同一帳號內的接力與平行、交給 Cursor 後怎麼把 PR 收回來、以及用 HarnessRouter 把工作送到不同執行環境。

> ⚠️ **來源性質**：《數位時代》篇是作者**個人試用心得**（圖卡為主、無量化數據）；aiposthub 兩篇是**二手整理**——Grok 開發篇整理官方公告與文件，CSV 逾時案例是**官方產品示意圖、不是可重跑的公開實測**；HarnessRouter 篇整理開源專案與 Avi Chawla 的示範，**示範是第三方整合案例，不是 Grok 官方內建功能**。三篇都未經本庫回官方文件核對。
>
> 🔗 **與既有條目的重疊處理**：以下內容已在 [[grok-bot-cloud-agent-product-overview]]，本條**只放連結不重寫**——
> - Grok Bot 當協調者、Cursor Cloud Agent 在沙盒執行（該條 R3「Outer Loop vs Inner Loop」；Grok 開發篇「Grok Bot 與 Cursor 如何分工」與 09-10 工程指南即此架構）
> - 30 分鐘 Notion PR 追蹤閉環、9 大官方指南（該條「原始摘要」）
> - 幕僚長模式：只跟幕僚長講話、它讀職責描述轉派、對外訊息與花錢才找你、用真人職位命名（該條補充資料 ③）
> - 同帳號 Bot 共用一台電腦、不同 Bot 名稱不是安全沙盒、核准規則（該條 B 層與 9-29 補充 ③）
> - BDK cursor-grokbot-agents：**本機**代理詢問 Grok Bot（ask／check／interrupt／list）——方向是「本機 → Bot」；本條的 HarnessRouter 是反方向「Bot 當入口 → 其他執行環境」，兩者不同（該條 11-03 補充）

## T（核心論點）

> **多 bot 的價值不在「bot 越多越好」，而在讓上一棒的產出直接變成下一棒的輸入、讓互不依賴的工作同時跑；人只在重要節點做決定。** 但要先過三個條件才值得拆：**常重複、有固定格式或規則、彼此能獨立進行**——任務短、步驟本來就有先後的，一隻 bot 從頭做到尾就好。交棒交到別的執行環境（Cursor、Codex、Claude Code）時，**統一的是交接方式，不是能力**；驗收要看回收的證據（PR、測試、影片），也要測取消、逾時、部分失敗。

三個支撐點：

1. 🔑 **接力 vs 平行看依賴**：有先後（選題 → 撰稿）就接力，上一棒產出直接交下一棒，不用人當傳話筒；互不依賴（會前查客戶、與會者背景、競品）就平行，**等的是最慢那一隻的時間，不是三件加總**——與 [[agent-multi-source-query-parallel-sequential-scheduling]] 的「下一步輸入是否依賴上一步輸出」同一判準
2. 🔑 **交給別的環境後，要把結果收回可檢查的地方**：Grok 開發篇的方向是 Bot 協調 → Cursor 改程式 → 修改進 GitHub／Origin 的 PR → 影片提供介面操作證據；影片只證明某條路徑、某組資料下的結果
3. 🔑 **模型 ≠ 執行環境**：HarnessRouter 把不同代理的工作階段、串流進度、檔案、取消與失敗狀態整理成同一個介面，但工具能力、授權、費用、失敗方式仍各自不同，**不能把清單上的名稱當成可互換零件**

## B（適用場景）

**適用：**
- 一個人要同時推進幾件固定格式的工作（選題／撰稿、會前情報、PR 追蹤）
- 已用 Grok Bot＋Cursor，想讓程式修改回到團隊既有的 PR 審查流程
- 產品要接多種程式代理後端，不想每次重寫進度串流與檔案取得

**不適用 / 注意：**
- 短任務、步驟有嚴格先後：拆成多隻 bot 只增加交棒成本
- 《數位時代》作者實測：撰稿 bot **中文語感不太好**，寫得沒有期待的好，但拿來快速判斷題目值不值得發展已夠用（單人主觀感受）
- 🔴 示範影片 ≠ 驗收通過：大量資料、網路中斷、不同權限、其他操作路徑都沒被影片涵蓋
- 🔴 HarnessRouter 社群版可自行部署，但仍需模型來源與設定；開源 ≠ 所有代理免費可用

## R（操作步驟）

### 1. 同帳號內：接力與平行（《數位時代》作者試用）

- 第一隻 bot 當「幕僚長」管理與新建 bot（架構見 [[grok-bot-cloud-agent-product-overview]] 補充 ③）。作者請它寫的自我介紹可直接當職責描述範本：「你把想法丟給我（一句也行）。**我先問清楚目標、截止、給誰看**，再決定是我直接做、建新 Bot，還是交給現有隊友。專職工作（選題、撰稿、整理信箱、查流量）盡量讓專 Bot 扛，我負責分派與彙整。**對外寄信、刪除、花錢這類會先跟你確認**；例行可做成排程。」
- **接力**：選題 bot 找題目 → 作者看完說「交給撰稿 bot」→ 它自己交棒；上一棒產出直接當下一棒輸入，不用複製貼上、不用重新解釋背景。作者角色從「AI 之間的傳話筒」變成「只在重要節點做決定的主管」
- **平行**：互不依賴的情報工作一次丟給不同 bot 同時做
- **何時多建一隻 bot（三條件，全部符合才拆）**：常常重複出現／每次有固定格式或規則／能和其他工作彼此獨立進行
- 方案（作者 2026-10-02 寫）：Grok Bot 需符合資格的付費方案（例如 SuperGrok），有桌面版與手機版。同類產品：OpenAI Dots 陸續開放給符合資格市場的 ChatGPT Pro、Business Premium（方案內含第一隻 dot）；Meta Muse 有免費額度但依帳號與地區開放。⚠️ Grok Bot 方案細節與歷次衝突見 [[grok-bot-cloud-agent-product-overview]]，以官網為準

### 2. 交給 Cursor 後怎麼收回：GitHub／Origin PR 與示範影片（2026-10-01 官方更新）

- 官方公告三項能力：Bots 把程式任務交給 Cursor／透過 **GitHub 與 Origin 外掛管理 PR**／**分享成果示範影片**。⚠️ 交接本身不是新能力（09-10 工程指南已有 Bot 啟動 Cursor 雲端代理、看執行紀錄、卡住時追加指示），這次是延伸
- **官方示意案例**：要求修復 CSV 匯出逾時 → Bot 轉交 Cursor → 回傳**兩個 PR**（分批匯出資料、加入匯出進度提示）＋一段示範影片 → 使用者表示可交付 → Bot 回報已合併到主要分支。🔴 圖中**沒有測試報告、效能數字或完整程式內容**，無法判定修復品質
- **GitHub vs Origin**：從 GitHub 同步到 Origin 的專案仍以 **GitHub 為主要來源**，雲端代理建 GitHub PR；直接建在 Origin 的專案用 Origin PR。支援 Origin **不代表必須搬家**。10-01 查詢時 Origin 仍是早期測試、帳戶逐步開放（Origin 本身見 [[cursor-origin-git-forge]]）
- **影片的用途與邊界**：讓 PM、設計師不讀程式也能看到按鈕可不可用、進度提示怎麼呈現（作者依官方能力推導的用途）；但程式差異與測試結果才回答「修改是否可靠」，匯出案例還要查資料完整性與是否影響其他功能
- **工程 Bot 範本**：同串後續貼文分享工程團隊用的範本（技能、例行工作、工具連接設定）。工程 Marketplace（10-01 查詢）列有 **Projects Manager**（專案協調）、**QA bot**（驗收）、**Engineering Lead**（追蹤開發進度）、**Nightly Audit Engineer**（夜間程式檢查）等角色。範本只是起點，仍要指定程式庫、驗收標準與可操作範圍
- **權限**：管理員可停用「把程式任務委派給 Cursor 雲端代理」；正式環境變更等動作可設先詢問規則；自動審查仍需搭配明確權限邊界（官方團隊管理與核准文件，經轉述）
- 作者建議的成效指標：每個任務需要**多少人工追問、多少次退回重做、完成後留下哪些可檢查的證據**

### 3. 交給不同執行環境：HarnessRouter

- **是什麼**：開源專案，把不同程式代理的**工作階段、串流進度、檔案、取消、失敗狀態**整理成統一程式介面（原專案稱 **Unified Harness Protocol**），讓產品用同一種呼叫方式把工作交給 Codex、Claude Code 等後端；各後端保留自己的工具與工作流程
- **Avi Chawla 的示範**：Grok Bot 當協調入口，其他任務在各自的代理工作階段執行。🔴 這是**示範作者的整合案例**，不等於 Grok 官方內建，也不代表 Grok Bot 換成了另一套執行環境
- **模型與執行環境分開看**：模型負責理解與產生下一步；執行環境決定工具、檔案、工作紀錄、任務如何持續——只比模型名稱解釋不了最終表現
- **接入前驗證**（作者建議）：
  1. 挑一個輸入與產出都清楚的工作（如把公開範例資料整理成圖表），比較不同後端時**保留相同資料與驗收條件**
  2. 🔑 **測取消、逾時與部分失敗**——跨系統最難處理的是「已產生檔案卻沒完成」或「前一段失敗、下一段仍繼續執行」
  3. 交接可靠、維護真的減少，才擴充更多後端

## C（關鍵行動）

- [ ] 拆 bot 前先過三條件（重複／固定格式／可獨立）；不符合就一隻做到底
- [ ] 安排多步流程時先標出依賴：有依賴接力、無依賴平行
- [ ] 交給別的代理的工作，要求回收「PR＋測試結果」，影片只當介面證據
- [ ] 評估 HarnessRouter 這類統一介面時，先跑一輪取消／逾時／部分失敗，而不是只跑成功路徑
- [ ] 自己的對照：Claude Code 子代理與 Agent Teams 的分工判準可參照本條三條件

## 原始摘要

- **《數位時代》多 bot 實測（36）**：幕僚長自我介紹（先問目標／截止／給誰看；對外寄信、刪除、花錢先確認）、選題 → 撰稿接力、會前情報平行、拆 bot 三條件、Grok Bot／Dots／Muse 可用方案；撰稿品質與中文語感一般
- **Grok Bot 強化軟體開發（30）**：10-01 官方更新（交給 Cursor、GitHub／Origin PR、示範影片）、09-10 工程指南的延伸、CSV 逾時示意案例、Origin 與 GitHub 主從關係、工程 Marketplace 角色、管理員停用委派與核准規則；與既有條目 Outer／Inner Loop 重疊部分只放連結
- **HarnessRouter（36）**：Unified Harness Protocol 統一工作階段／串流／檔案／取消／失敗；Grok Bot 當協調入口是第三方示範；模型 ≠ 執行環境；先驗證交接再擴充後端

## 相關連結

- [[grok-bot-cloud-agent-product-overview]] — 產品架構本體：Outer／Inner Loop、幕僚長模式、共用電腦風險、BDK 代理通訊（本機 → Bot，方向與 HarnessRouter 相反）
- [[grok-bot-routine-automation-ladder]] — 單一 bot 的「先做對一次 → 固化 Skill → 排程」；本條是多 bot 之間的分工，拆 bot 三條件與「固定格式才固化」同一套判斷
- [[cursor-origin-git-forge]] — Origin 本身：為代理大量分支與差異設計的 git forge
- [[agent-multi-source-query-parallel-sequential-scheduling]] — 平行 vs 循序的判準（下一步輸入是否依賴上一步輸出）
- [[agent-router-vs-coordinator-state-tracking]] — 幕僚長是 Coordinator：分流之外還要追蹤目標是否已滿足
- [[multi-agent-cli-bridge]] — 本機側的多供應商代理分工（CCB／tmux），與 HarnessRouter 的「統一介面送進不同後端」可對照
- [[grok-bot-persistent-agent-interface-design]] — 多 bot 在介面上呈現為 Bot 名單與頭像狀態
