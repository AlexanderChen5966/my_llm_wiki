---
title: "Claude 專案大重構！告別靜態資料夾：Projects 化身「對話式幕僚長」，並行調度多 Threads、跨 Repo 自動開 PR 與共享記憶體深度解析"
source: "https://www.aiposthub.com/claude-projects-redesigned-folder-to-conversation-parallel-threads/"
author:
  - "[[Philo]]"
published: 2026-09-18
created: 2026-09-22
description: "Anthropic 震撼宣布重構 Claude Projects！徹底拋棄過去「靜態檔案夾」架構，進化為全自主的「對話式幕僚長」。在 Claude Code 中，只需交代最終目標，Claude 即自動拆解需求、在雲端平行調度多條 Threads 執行緒獨立拉分支開發、跨 Repo 執行測試並依依賴性自動開 PR。更具備跨 Thread 共享記憶庫、手機隨時調度與離線雲端狂飆。本文全面拆解全新架構、底層機制、與 Cursor Projects 之對決，以及軟體工程開發範式的重大升級。"
tags:
  - "clippings"
status: processed
---
---

> 「過去，在軟體構建過程中管理多個會話，需要你手動拆分工作、應付任務交接，並費力將結果縫合在一起。現在在 Claude Code 專案中，你只需描述需要完成的目標，Claude 就會為你統籌管理整個工作流程——劃定範圍、委派任務、協調平行執行緒、審查輸出，並組裝出最終成品。  
>   
> 你可以隨時透過手機指導進度，甚至在你離開電腦後，它依然在雲端持續為你工作。」  
> —— Anthropic 官方團隊（2026 年 9 月 17 日）

---

![Claude Projects 官方重構發表：從資料夾走向對話](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/00-og-projects-redesigned.jpg)

Anthropic 正式重構 Claude Projects：告別靜態文件夾概念，進化為能自主調度多個平行 Threads、跨倉庫提 PR 的對話式專案指揮中心。 圖片來源： Claude 官方部落格 。

---

全球生成式 AI 與程式開發領域在 2026 年 9 月迎來了極為罕見的高密度的典範轉移。就在 Cursor 剛推出「Projects 單一持久協調代理」架構一週後， **Anthropic 於 2026 年 9 月 17 日正式投下對應的核彈級更新——全面重構旗下最核心的協同機制「Claude Projects」！**

官方直接以 **《Projects redesigned: from folder to conversation》（專案大重構：從資料夾走向對話）** 為題宣告：過去被定義為「上傳參考文件、貼客製化指令（Custom Instructions）、然後開多個獨立 Chat」的舊版 Projects 模式將正式走入歷史。

取而代之的，是一個 **以目標為導向、由單一「對話式幕僚長（Chief of Staff）」統籌全局，在背景並行調度成群雲端執行緒（Threads）自主編程、跨多倉庫（Multi-Repo）驗證並自動開啟 Pull Requests（PR）的自主工程工廠。**

這項升級目前已在 **Claude Code** 雲端會話中展開 Beta 測試，並預告後續將全面擴展至網頁端、桌面端、Cowork 以及 Team / Enterprise 方案。

究竟為什麼 Anthropic 要親手推翻自己過往引以為傲的「Projects 資料夾」？「幕僚長＋多執行緒」架構底層如何運作？跨多個 GitHub 倉庫時如何智慧計算合併順序？與 Cursor 的 Projects 相比又有何異同？本文將為您帶來業界最詳盡的深度架構拆解與實戰洞察。

---

## 一、問題根源：為什麼傳統「靜態資料夾（Folder）」模式正在扼殺研發效率？

在深入全新架構前，我們必須先回顧過去兩年所有使用 Claude Projects（甚至是 ChatGPT Projects、Gemini Gems）的開發者與團隊，心中揮之不去的四大核心痛點：

![傳統靜態資料夾模式 vs 全新對話式幕僚長架構對比圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/01-arch-comparison.png)

架構演進對比：過去工程師手動拆解任務並充當 PR 縫合怪；全新架構由 Coordinator 協調器統籌全局，調度多個獨立雲端 Threads 並行狂飆。圖片製作：雷司紀科技趨勢研究。

### 1\. 「被動靜態庫」vs「主動交付體」

過去的 Projects 充其量只是一個「帶有固定上下文的收納盒（Context Box）」。你把 API 文件、架構規範、產品需求丟進去，AI 只是被動地「讀取」。每次遇到複雜工程，你依然得自己把龐大的需求切成五塊，分別在五個 Chat 視窗裡一步步引導 AI，人類反而退化成了「任務切分工人」。

### 2\. 人肉交接與結果縫合地獄（The Stitching Tax）

在舊模式中，Chat 1 改了後端資料庫結構，Chat 2 寫前端頁面時完全不知道 Chat 1 做了什麼決定。工程師必須手動把 Chat 1 的輸出複製貼到 Chat 2，並祈禱兩者命名風格與介面一致；一旦合併時出現衝突，又得開 Chat 3 來除錯。這種「在不同對話視窗之間當搬運工」的過程，耗損了極大的工程專注力。

### 3\. 上下文遺忘與重複交代成本

雖然專案裡有共用自訂指令，但對於開發過程中的「動態決策」（例如：「剛才討論決定先把發布日推遲到週五」、「因為效能問題取消了某個匯出功能」），新的 Chat 一無所知。工程師必須反覆在每個對話中補充最新背景，深怕 AI 依照過時的假設寫出垃圾代碼。

### 4\. 筆電綁架與同步阻塞

傳統的 AI 編程往往綁定在本地終端。當 AI 在跑複雜的測試或改寫大量檔案時，工程師只能乾等（Babysitting），不敢關閉終端，更不敢闔上筆電去搭捷運，否則工作立即中斷。

**「全新 Claude Projects 的核心革命，就是把人類從『微觀管理對話（Chat Babysitter）』的角色中徹底解放，讓專案從『裝檔案的死抽屜』，蛻變成『能指揮大軍的活同僚』。」**

---

## 二、核心架構剖析：解密「幕僚長（Coordinator）」與「雲端執行緒（Threads）」雙層體系

全新設計的 Claude Projects 採用了極為清晰且強悍的雙層解耦架構： **負責高層戰略指揮的「協調器（Coordinator）」與負責底層泥濘工作的「平行執行緒（Threads）」。**

![Claude Projects 專案啟動介面：目標設定與執行緒調度](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/01-project-setup-ui.png)

在全新 Projects 介面中，工程師只需輸入目標與關聯 Context，Claude 就會主動建議可立即承接的工作，並調度底層環境、外掛與模型。 圖片來源：Claude 官方部落格。

![](https://www.youtube.com/watch?v=-EAEc4LiTyo)

Anthropic 官方展示：即時觀看 Claude Projects 如何由技術幕僚長（Coordinator）拆解任務、在右側開闢多條 Threads 雲端平行狂飆代碼。影片來源： [Claude 官方 YouTube](https://www.youtube.com/watch?v=-EAEc4LiTyo&ref=aiposthub.com) 。

### 1\. 專案協調器（Coordinator）：如同技術幕僚長（Chief of Staff）

當你在專案主畫面輸入需求時，與你對話的是 Coordinator。

- **不親自寫代碼** ：協調器專注於「理解需求範疇（Scoping）」、「制定實施計畫」、「切分工作邊界」與「指派任務」。
- **主動工作建議（Proactive Suggestions）** ：只要你綁定程式碼倉庫或專案脈絡，Claude 一開場就會主動分析代碼現狀，向你建議：「我可以立即為您處理以下三項重構或測試工作」。
- **永不阻塞的全局掌控** ：因為協調器不參與具體的編譯與執行，它永遠處於待命狀態。你可以隨時向它交辦新想法，它會動態調整現有 Threads 的優先順序，或開啟新的 Thread。

### 2\. 雲端執行緒（Threads）：獨立隔離的專業工作單元

底層真正幹活的是一條條「Threads」。

- **每個 Thread 都是一個獨立的 Claude Code 雲端實例** ：在 Anthropic 伺服器的隔離容器中運行，擁有獨立的運算環境。
- **獨立分支與代碼副本** ：每個 Thread 在該倉庫中建立自己獨立的 Git 分支（Branch），並在獨立的 Workspace 副本中修改代碼、安裝相依套件、執行單元測試。
- **巢狀子代理體系（Subagents, Loops & Workflows）** ：單一 Thread 並非只能單線程工作。當指派給某個 Thread 的任務極為龐大時，該 Thread 內部能進一步啟動 Subagents（子代理）、邏輯迴圈（Loops）與自動化工作流，以最快速度完成作業。
- **代碼重疊與衝突解決** ：如果兩個 Thread 碰巧修改了同一個模組，系統不會在黑箱中強行覆蓋，而是回歸軟體工程的本質規範—— **像一般工程師團隊拉 PR 一樣，透過 Git Merge Conflict 機制由你或 Claude 進行明確仲裁** 。

---

## 三、兩大殺手級實戰場景：跨 Repo 重構與效能優化

Anthropic 在官方發布文檔中，展示了兩個令人極為震撼的實際研發範例，充分彰顯了「對話式 Projects」在複雜工程場景中的壓倒性威力：

![Claude Projects 跨 API、Web、Mobile 三大倉庫平行重構並自動開啟 PR 時序圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/02-multi-repo-sequence.png)

跨倉庫全鏈路重構時序圖：Claude Coordinator 跨 Repo 解析相依拓撲，平行指派 Threads 獨立編程並自動開 PR，最後給予人類工程師明確的合併先後順序指引。圖片製作：雷司紀科技趨勢研究。

### 場景 1：跨 API、Web、Mobile 三大倉庫廢棄廢棄端點（Deprecated Endpoint）

在過去，一個舊 API 廢棄任務往往是工程團隊的噩夢，涉及多個專案組與跨倉庫協調。現在在 Claude Projects 中：

1. **連接三個倉庫** ：一次性將後端 API、Web 前端與 Mobile App 倉庫掛載至專案。
2. **下達目標** ： `「我們的目標是徹底退役 v1 版本的端點。請幫我遷移所有調用方、跑通測試並開啟 PR。」`
3. **自動平行拆解** ：
	- **Thread 1** 檢視 API 倉庫，標註廢棄警告，編寫遷移腳本；
		- **Thread 2** 檢視 Web 倉庫，將所有 API 呼叫點升級至 v2，並執行前端測試；
		- **Thread 3** 檢視 Mobile 倉庫，修改客戶端網路層代碼並驗證編譯。
4. **智慧依賴排序（Dependency-Aware Ordering）** ： 所有 Threads 跑完後，Claude 不僅在 GitHub 提好三個獨立 PR，還會在主對話框中清晰提示工程師： **「請務必先審核並合併 API Repo 的 PR #104，待後端發布完畢後，再合併 Web 的 PR #58 與 Mobile 的 PR #212。」**

### 場景 2：降低電商結帳流程 P75 延遲（Checkout Latency）

如果你提出一個指標導向的目標： `「將結帳流程的 P75 延遲降低 30%。」`

- Claude Coordinator 會自動分析系統架構，為不同的服務調用點開啟多個平行 Thread；
- 每個 Thread 分別對單一 Endpoint 進行 Profile 分析、嘗試最佳化快取策略、減少 N+1 查詢；
- 各自在沙盒中跑基準測試（Benchmark），驗證有效後獨立拉出 PR 供團隊審查。

---

## 四、核心進化：動態共享記憶庫（Shared Memory）與資產庫（Project Library）

傳統 Agent 最為人詬病的就是「換個視窗就失憶」。全新 Claude Projects 引入了強大的「時間序列記憶機制（Context builds over time）」，徹底翻轉了人機協同的互動體驗。

![Claude Projects 一週演進：動態記憶與決策累積](https://cdn.prod.website-files.com/68a44d4040f98a4adf2207b6/6aac17f18b4babdc667d967c_launch-readiness-under-4mb.gif)

一個專案跨越一週的演進軌跡：每次提問在右側開啟專屬 Thread，而記憶、決策與成果在底層持續沉澱生長。 圖片來源：Claude 官方部落格。

### 1\. 跨 Thread 共享動態記憶（Shared Project Memory）

在同一個 Project 中， **任何一個 Thread 產生的關鍵決策與知識，都會自動寫入專案的「共享記憶庫」** ，供後續所有 Threads 隨時調用。 這意味著你不再需要寫冗長複雜的 Prompt Engineering：

- **業務動態自適應** ：Claude 會自動記住「產品上線日已延至週五」、「為什麼先前放棄了某個資料匯出方案」；
- **團隊組織規範** ：它會記住「改動 Billing 支付模組前，代碼必須抄送資深工程師 Alex 審查」；
- **技術偏好沉澱** ：它會記住你的測試風格偏好使用 Vitest 而非 Jest、你的錯誤處理統一採用自訂的 Result 物件。

### 2\. 人類工作與溝通風格自適應（Personalized Interaction）

你甚至可以直接調教專案幕僚長與你的協同習慣：

- `「請減少日常主動打擾的頻率，除非遇到架構分支決策再來問我。」`
- `「只有在大任務超過 3 個檔案時才開啟新 Thread，小修正直接在同一個 Thread 內完成。」`
- `「每次交付報告時，請先用 3 點 Executive Summary 總結，再列出代碼變更。」`

### 3\. 統一專案資產庫（Project Library）

Projects 不再把檔案與產出物隨意丟在聊天紀錄中。全新的 **Library** 面板會自動聚合兩大資產：

1. **使用者上傳的檔案** ：需求規格書（PRD）、架構圖、API 文件；
2. **Claude 產生的成品（Artifacts）** ：自動產出的技術方案（RFC）、流程圖、數據分析報表、代碼改動清單。 任何新的 Thread 都能直接檢索資產庫，讓每一次新任務都站在歷史工作的成果之上。

---

## 五、算力調度與無縫流轉：獨立模型配置、手機接力與離線狂飆

對於高階工程師與企業而言，多代理並行雖然強大，但隨之而來的是 **算力消耗、成本控制與行動體驗** 的嚴峻考驗。Anthropic 在這次重構中給出了解法：

### 1\. 雙軌算力與努力程度獨立配置（Effort Level & Model Selection）

Projects 允許同時跑多個 Thread，這意味著 Token 與配額消耗速度會顯著加快。為了避免不必要的額度浪費，Anthropic 設計了極度精細的資源控制功能：

- **Coordinator 與 Threads 模型分離** ：你可以讓 Coordinator 使用最具戰略規劃能力的高階模型（例如 Claude 3.7 / 3.5 Sonnet 或未來的 Opus），而將簡單的格式化或機械式測試 Thread 配置給更輕量極速的模型；
- **獨立努力程度設定（Effort Levels）** ：你可以為協調器開啟高深度「思考預算（Extended Thinking Budget）」，讓它深思熟慮架構；而將邊緣修復 Thread 的思考時間縮短，在效能與速度之間取得最佳平衡；
- **專案專屬額度監控（Project-Specific Usage）** ：透明查看各專案消耗的 Session 與 Token 數量。

### 2\. 「離開電腦、闔上筆電」的純雲端非同步執行

由於所有 Threads 均託管於 Anthropic 雲端伺服器：

- 你在辦公室交辦完：「幫我把這 20 個模組加上 TypeScript 嚴格型別檢查並跑過 CI」；
- 你可以立刻闔上 MacBook 放進公事包下班；
- 在通勤的捷運上，打開 iPhone 或 Android 上的 Claude Mobile App，你能即時看見各個 Thread 的進度條推進；
- 如果某個 Thread 遇到邊界分歧（例如：某個 nullable 欄位是否該報錯），它會在手機上跳出通知，你點選「回覆」給予決策，雲端代理立即繼續狂飆。

---

## 六、硬核橫向對決：Claude Projects vs. Cursor Projects vs. Devin

隨著 Anthropic 的加入，AI 軟體工程市場的格局正式進入「專案級代理（Project-Level Agents）」白熱化廝殺階段。我們特別整理了目前全球三大代表性工具的深度對比架構：

| 評比維度 | 舊版 Claude Projects | 全新 Claude Projects (Redesigned) | Cursor Projects (Anysphere) | Devin (Cognition) |
| --- | --- | --- | --- | --- |
| **核心哲學** | 靜態上下文資料夾＋獨立 Chat | **對話式幕僚長＋平行 Threads** | **持久在線協調代理＋Subagent 矩陣** | 獨立自駕工程師（全自動工作站） |
| **交互入口** | 點進資料夾、手動開各個 Chat | **單一專案對話流，隨時深入 Thread** | 單一持久 Conversation，內嵌子代理卡片 | 任務 Dashboard＋雲端桌面/終端串流 |
| **並行架構** | 無（完全靠人類手動平行開 Chat） | **平行 Cloud Threads（獨立 Branch）** | **數百至數千 Subagents 雲端並行** | 單一 Agent 內部子任務循環 |
| **多倉庫支援** | 手動上傳不同 Repo 代碼片段 | **原生支援跨 Repo（API/Web/Mobile）** | 透過 Context 支援關聯倉庫 | 支援連接多個 GitHub 倉庫 |
| **PR 依賴排序** | 無（人類自行判斷） | **具備（自動指示 PR 合併先後順序）** | 透過 GitHub 整合自動追蹤 PR 狀態 | 自動開 PR，具備 CI 修復能力 |
| **長期記憶體** | 靜態 Custom Instructions | **動態共享記憶（業務/風格/決策自動寫入）** | 跨端持久上下文（Shared Memory） | 專案專屬 Knowledge Base 學習 |
| **離線與行動** | 不支援（網頁需保持開啟） | **支援（雲端背景執行、手機即時調度）** | 支援（雲端執行，工程師關機不中斷） | 支援（雲端全天候背景執行） |
| **本地/雲端彈性** | 純雲端網頁 | **雲端先行，即將支援本地內網混合** | 本地 IDE 與雲端虛擬機無縫切換 | 純雲端沙盒環境 |
| **最適合場景** | 文字知識庫、撰寫企劃、簡單代碼問答 | **複雜重構、跨系統 API 升級、團隊協同** | **高強度軟體工程、超大規模大型單一庫重構** | 獨立外包 Bug 修復、全自動化遷移作業 |

### 專家分析：Anthropic 與 Cursor 的「殊途同歸」

比對 Anthropic 與 Cursor 在 2026 年 9 月發布的新架構，你會發現極具戲劇性的共識：

1. **兩大巨頭一致宣告「逐次對話（Per-Task Ephemeral Chat）」已死** ——未來的 AI 開發絕對不是工程師在文字框前伺候一個個短命的對話；
2. **兩大巨頭一致採用「分層代理模式（Hierarchical Multi-Agent）」** ——頂層一定是永不阻塞的協調代理（Coordinator / Chief of Staff），底層是一群在獨立分支上瘋狂寫代碼的 Worker 代理；
3. **兩大巨頭一致將戰場移至「雲端非同步（Asynchronous Cloud）」** ——擺脫本地電腦硬體限制，讓 AI 成為 24 小時在雲端運行的數位工程部隊。

---

## 七、給開發者與技術主管的「專案幕僚長」實戰心法

當 AI 從「代碼自動補全（Copilot）」晉升為「專案協調幕僚長（Coordinator）」，軟體工程師的角色正在發生歷史性的轉向。如果你有幸獲得 Beta 測試資格，請務必掌握以下四大高階實戰心法：

### 1\. 戒掉「微觀管理（Micro-managing）」，改用「交付目標導向（Outcome-oriented）」

在過去，你會告訴 AI：「請打開 `auth.ts` ，在第 45 行加上檢查 token 是否過期的邏輯」。  
在全新 Projects 中，你應該像 CTO 一樣下達目標：

> `「我們的目標是消除所有 JWT 過期未被捕捉造成的 500 錯誤。請全面掃描 auth 模組，重構攔截器，為過期事件加上自動刷新機制，撰寫單元測試覆蓋三種邊界情況，並開啟 PR。」`

把具體拆解與多個執行緒的調度交給 Claude，你只需在頂層審核方案與最終成果。

### 2\. 主動調教你的「專案記憶庫」

在專案初期，花 5 分鐘明確告知 Claude 你們團隊的「地雷與偏好」：

- 告知團隊專屬的命名習慣（例如：資料庫欄位一律使用 snake\_case）；
- 告知不允許觸碰的遺留模組（Legacy Core）；
- 要求它每次在開 Thread 之前，先列出將修改的檔案清單。 這些指引會自動沉澱進共享記憶體，後續產生的 10 個、20 個 Thread 都會嚴格遵守。

### 3\. 善用「獨立模型配置」守護荷包

不要讓所有任務都盲目燃燒最高階旗艦模型的深度思考算力！

- **協調器（Coordinator）** ：開高思考深度，負責精密規劃；
- **機械型 Thread（如更新所有 API 呼叫點的函式名稱）** ：配置速度最快、消耗額度最省的模型；
- **複雜演算法與安全架構 Thread** ：單獨啟用最高算力。

### 4\. 警惕「PR 審查疲勞（Review Fatigue）」

當 Claude 在半小時內跨三個倉庫開出 6 個 PR 時，人類工程師最大的瓶頸不再是「寫不出來」，而是\*\*「來不及審查（Review Bottleneck）」\*\*。

- 善用 Claude 提供的「依賴合併順序指引」，一動一動驗證；
- 要求 Claude 在 PR 描述中詳細列出「改動動機、測試覆蓋率、潛在破壞性變更（Breaking Changes）」，保護你的 Codebase 衛生。

---

## 八、開放時程、資格門檻與未來藍圖（本地執行即將登場）

很多開發者最關心的問題是： **我現在用得到嗎？要如何加入？**

根據 Anthropic 官方說法，本次發布採取漸進式灰度推播（Phased Rollout）：

![Anthropic 企業級代理節點生態](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/03-enterprise-agents-illo.svg)

Anthropic 全新 Projects 架構代表著其企業級 Agent 生態（Enterprise Agents）的關鍵一環。 圖片來源：Claude 官方部落格。

### 1\. 首波 Beta 測試資格

- **對象** ：目前限部分使用 **Claude Code 雲端會話（Cloud Sessions）** ，且在網頁端或桌面端「尚未建立過任何舊版 Projects」的 **Claude Pro 與 Max** 訂閱用戶。
- **候補名單** ：符合 Pro 或 Max 身分但尚未獲得資格者，可至官方頁面填表登記（ [https://claude.com/form/projects](https://claude.com/form/projects?ref=aiposthub.com) ）。

### 2\. 後續擴展路徑

- **未來一週** ：擴展至更多 Claude Code 的 Pro 與 Max 用戶；
- **第二階段** ：全平台推廣至 Claude 網頁版、桌面版、Claude Cowork，並正式開放給 **Team 與 Enterprise** 企業方案；
- **舊版專案升級** ：既有的 Pro / Max 舊專案目前維持原樣正常運作，待推播擴展至常規對話與 Cowork 時，官方將提供平滑升級路徑。

### 3\. 未來最重磅功能：混合本地執行（Local Machine & Behind Firewall）

目前所有 Threads 均在 Anthropic 的雲端容器中執行。但官方在公告結尾特別強調了一句震撼彈：  
**「在你的本機電腦上、與你本地的開發工具及代碼並肩運行，並處於你的企業內網與防火牆後面的能力，即將登場（coming very soon）！」**

這意味著，未來 Claude Projects 將具備「雲端狂飆＋本機私有」的混合雙軌架構。對於嚴格禁止代碼出海的金融機構、半導體大廠與敏感國防專案，這將徹底打通 AI 進入核心內網研發體系的最後一哩路！

---

## 結語：軟體開發的「指揮官時代」正式降臨

從 2023 年的一問一答對話框，到 2024 年的 Artifacts 預覽，再到 2025 年初登場的靜態 Projects 資料夾；如今在 2026 年，我們終於看見了軟體工程未來的終極型態—— **對話即工廠、目標即交付。**

Claude Projects 的這次大重構，不僅是 Anthropic 對「如何用 AI 構建大型系統」給出的最成熟答案，更昭示了一個不可逆的職場現實：

> **未來的頂尖工程師，不再以「打字速度多快、寫了幾行語法」論英雄；  
> 而是以「能否像一位優秀的幕僚長指揮官，定義清晰的戰略邊界、拆解架構依賴，並調度千軍萬馬般的 AI 代理團隊」來決勝負！**

準備好升級你的開發思維，迎接與 Claude「幕僚長」並肩作戰的全新工作時代了嗎？