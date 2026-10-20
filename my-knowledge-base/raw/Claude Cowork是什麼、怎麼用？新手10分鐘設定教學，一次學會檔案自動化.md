---
title: "Claude Cowork是什麼、怎麼用？新手10分鐘設定教學，一次學會檔案自動化"
source: "https://www.bnext.com.tw/article/91158/from-chat-to-cowork-local-claude-workflow-guide"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-08-17
created: 2026-08-17
description: "Claude Cowork 是什麼、怎麼用？教新手 10 分鐘設好環境，把 AI 工作流從雲端搬下本機、自動處理檔案。"
tags:
  - "clippings"
status: "processed"
---
如果你用Claude還停留在「開對話框、貼問題、複製答案」，本文會幫你跨過一道門檻：把AI從雲端的聊天視窗，搬到你電腦本機，直接讀寫整個資料夾的檔案。我們推薦的是Cowork，它能幫你把一份英文研究報告變成可編輯的中文簡報、把一整批檔案整理成報表——這些都是傳統對話模式做不到的。

這是《數位時代》Claude 系列的第二站。第一篇 [Claude Skills 零基礎入門：把每天重複的工作包成自動啟動的小工具〉](https://www.bnext.com.tw/article/90870/claude-skills-guidebook) 教你把每天重複的工作包成會自動啟動的小工具、再把「你是誰」的個人偏好設定一次寫好，省下每次重貼指令的力氣；如果你還沒讀過，沒關係，那些設定在這期會再用到、本文也會帶你回顧重點。而這一篇要做的，是把那套工作流從雲端的網頁，整個搬到你電腦本機。

如果你兩期都想完整收藏， [這邊可以免費下載全文PDF](https://www.bnext.com.tw/member/special-issue/list) （需免費加入會員）。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## Claude Cowork 是什麼？先搞懂 Claude 的三種工作入口

本期的主角 Cowork，是 Anthropic 目前三種常見工作入口的其中一個。在進入主題前，先用一張全景圖把這個生態交代清楚，不同入口各有適用情境，多數仍計入同一訂閱用量：

| 產品 | 解決的需求 | 適合誰 |
| --- | --- | --- |
| Claude（對話 + Cowork） | 對話、文件處理、本機檔案自動化 | 所有工作者 |
| Claude Code | 程式開發、跑指令、寫腳本 | 工程師與技術愛好者 |
| Claude Design | 視覺設計（pitch deck、landing page、行動版 mockup、品牌指引） | 設計工作者與創業者 |

本期教的 Cowork 適合「快速、可編輯、跟既有資料連動」的場景；至於要做品牌級視覺輸出，那是 Claude Design 的場景，有興趣的讀者可參考《數位時代》文章 [〈省 token 心法：用 Claude Design 與 Refero 做品牌簡報〉](https://www.bnext.com.tw/article/90873/save-tokens-with-claude-design-and-refero-brand-templates) 。

## 單元一｜Claude Cowork 跟 Chat 差在哪？為什麼要切過去？

5 月最後一頁我們說，「下期把雲端的東西搬下本機。」本期就是這場搬遷的開始。但搬下本機不只是換載體，Claude Cowork 跟 Chat 是兩種完全不同的工作模式。先理解差在哪，你才知道什麼時候該切過去。

### 三個本質差異

| 差異 | Chat | Cowork |
| --- | --- | --- |
| 處理檔案的方式 | 上傳上限 20 檔 / 單檔 500MB | 不受 Chat 上傳件數 / 單檔大小限制 |
| 累積記憶的方式 | 雲端 Memory、有總量上限 | 寫進本機 CLAUDE.md (資料夾規則檔) |
| 下指令的方式 | 一步一步陪它走 | 給結果與約束、走開讓它做 |

**第一，本機檔案存取。** Chat 一次最多上傳 20 個檔案、單檔不能超過 500MB。而 Cowork 卻能直接讀寫你電腦上的實體檔案，不受 Chat 那種上傳件數／單檔大小的限制。舉例來說，你有 100 張收據資料夾要做成費用報告。Chat 處理不了，光是上傳就卡住；而且它回給你的也只是對話框裡的一段文字，你還得自己複製到 Excel。

但是 Cowork 就不一樣了，你只要把資料夾路徑給它，幾分鐘後通常就可以直接在電腦上看到一份格式化的 Excel 初版，模糊看不清的還會自動標 VERIFY 提醒你複核。如果說 Chat 像是需要頻繁寫入的黑板，那 Cowork 就是可以堆疊資料的辦公桌。

**第二，持久化記憶。** Chat 的雲端 Memory 鎖在 Claude 內部、有總量上限。Cowork 則可以把規則跟偏好放進你電腦上的 CLAUDE.md（資料夾規則檔，Cowork 對話前會自動讀），你能隨時打開來看、改、備份。

因此對於大部分讀者來說，Cowork 的優勢是「規則寫一次就在那」，下次對話不必再重貼；就算換電腦也只要整個資料夾複製過去，整個工作環境可以輕鬆帶走。

**第三，下指令的方式不同。** Chat 期待你一步一步陪它走（先讀這個、然後抓那個、最後寫成段落）。Cowork 期待你給結果與約束、走開讓它做（例如：產出一份 5 段、每段 100 字、存到 outputs 資料夾，不確定的標 TBD）。這條最關鍵，接下來單元三會深講。

要澄清的是，你 5 月建的雲端 Skill 不需要重做。因為 Skill 跟你的 Claude 帳號綁定，在 Chat、Cowork、Claude Code 三個介面都能呼叫，差別只在 Cowork 還能呼叫本機檔案。

### 何時用 Chat、何時用 Cowork？

簡單對答、改一句話、單檔分析，用 Chat，省 token、回應快；多步驟任務、要處理多個檔案、有可重複的工作流，切到 Cowork。但要注意，Cowork 比 Chat 更吃 token，使用上要注意限額（這部份要看你的付費方案）。判斷標準很簡單：你需要它「做完一整件事」還是「陪你討論」？整理一批檔案、處理一份報告、產出一份簡報這種要它跑幾分鐘才能完成的事，交給 Cowork、給完指令就走開；改一句話、問一個問題、邊看邊調這種一來一往的事，繼續用 Chat。

## 單元二｜10 分鐘把環境設好

這節純操作。7 個步驟、約 10 分鐘設完，之後就一勞永逸。

### 第 1 步：下載 Claude Desktop、切到 Cowork tab

![#0 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/5ew3-1780629611.jpg?w=1200&output=webp)

打開App後，可以在左上標籤切換Chat、Cowork、Code模式。

圖／ 數位時代

### 第 2 步：設定全域指示

全域指示（Global Instructions）是所有 Cowork 對話都會先讀取的常駐指令，位置在 Settings → Cowork → Edit。對全域指示該寫沒有內容沒頭緒的讀者，可以直接複製下面這段簡易版貼上去：

```
動任何檔案前先列計畫，等我確認再執行。刪除、覆寫、改名檔案前一律問我。預設輸出格式：docx 或 xlsx。不確定的地方明說，不要猜。需要圖的地方留空白給我，不要自動補圖。
```

貼上之後，之後不管你 prompt 寫得多隨便，這五條指示都默默把關。要注意的是，全域指示跟上一期設的 Profile Instructions（位置在 Settings → General → Instructions for Claude）是不同的方框，分別管 Cowork 跟 Chat。

### 第 3 步：Capabilities 設定

到 Settings → Capabilities，做四件事：

1. 開啟「Generate memory from chat history」開啟 Cowork 模式的雲端記憶
2. 「Location metadata」可考慮關閉（隱私）
3. Tool access 設為「Load tools when needed」（需要時載入）
4. 下方其他所有功能全部開啟。若想在 Claude 繼承其他 AI 產品的記憶，可以在「Import memory from other AI providers」處點擊「Start import」上傳

這裡開啟的是 Cowork 的雲端記憶：由系統自動從你跟它的對話中提取你的個人偏好、寫作習慣、常用工具，下次新對話就會自動套用。整個過程不需要你動手寫檔案，用得越久、Cowork 就越貼近你的口吻。

（資料夾內的本地記憶檔屬於進階用法，後面第 9 頁會專門教。）

![#4 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/ihg7-1780629613.jpg?w=1200&output=webp)

切記，最重要的是允許AI從對話紀錄中存取雲端記憶。

圖／ 數位時代

### 第 4 步：建一個 Cowork-Playground 空資料夾

到桌面或 Documents 下，右鍵新增資料夾，取名 Cowork-Playground（或任何你喜歡的名稱）。只建一個空資料夾就好，子資料夾和檔案讓 Cowork 稍晚幫你建（第 6 步的事）。這邊要注意的是，不要直接指向 Documents 整層或桌面整層，這會讓 Cowork 看到太多無關檔案，因此給它一個獨立資料夾，將所有跟 Cowork 相關的工作都鎖在這個資料夾內。

### 第 5 步：開新對話、指向 Cowork-Playground

回到 Cowork ，按「新對話」，左下方有一個資料夾選擇器，點開選 Cowork-Playground。Cowork 會問是否允許存取，選「永遠允許」。從現在起，Cowork 原則上只處理你授權的資料夾、不會去翻整層 Documents 或桌面。

![#2 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/icis-1780629612.jpg?w=1200&output=webp)

不管你的資料夾名稱叫什麼，使用 Cowork 的時候，最好在一個獨立的資料夾裡面運作，避免讓 Cowork 讀取到電腦中其他地方的檔案。

圖／ 數位時代

### 第 6 步：複製貼上「環境初始化 prompt」

由於這是你第一次體驗 Cowork，因此請複製下面這段 prompt，貼到對話框送出（堅持手工建立也沒問題）：

```jsx
請在這個資料夾建立 Cowork 工作環境：

1. 建三個子資料夾：inputs、outputs、needs-review
2. 建一個 CLAUDE.md 檔案，內容用以下結構（每段先放占位文字，我之後會親自編輯）：

   # 工作環境總則

   ## 我是誰
   （請放占位文字「待貼入個人化設定」）

   ## 資料夾用途
   - inputs/：要 Cowork 處理的素材
   - outputs/：Cowork 產出的成品
   - needs-review/：Cowork 不確定的、要我人工檢查的

   ## 工作流規則
   - 收到任務後先列計畫，等我確認再執行
   - 產出存到 outputs/
   - 不確定或多歸屬的丟 needs-review/

3. 完成後列出整個目錄結構讓我確認
```

接下來 Cowork 會先列出計畫（因為全域指示 設了「動手前先列計畫」），你按確認後它 1 分鐘左右就建好。然後打開 Finder，就會看到資料夾結構出現了：

```jsx
Cowork-Playground/
├── CLAUDE.md
├── inputs/
├── outputs/
└── needs-review/
```

為什麼要這樣分？沒有這幾個子資料夾的話，Cowork 跑完任務的成品會散落在 Playground 根目錄、模糊資料也會混進 inputs/，幾週後資料夾就變得難以維護。

![#9 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/2ru7-1780629614.jpg?w=1200&output=webp)

這個資料夾結構只是簡易版，目的是讓你的 Cowork 可以在一個有明確定義的結構中運作。 這樣一來，不管進行哪一種類型的工作，資料都可以順利歸檔。因此你也可以按照圖四：資料夾結構 圖說：這個資料夾結構只是簡易版，目的是讓你的 Cowork 可以在一個有明確定義的結構中運作。 這樣一來，不管進行哪一種類型的工作，資料都可以順利歸檔。因此你也可以按照自己的需求調整資料夾結構，重點是你的 Cowork 跟你自己都知道這些資料夾的定義是什麼。

圖／ 數位時代

### 第 7 步：填寫 CLAUDE.md

若直接打開 CLAUDE.md，你會看到「## 我是誰」段落是占位文字，把它換成你的個人化設定。最簡單的做法是複製下面這段 prompt 給 Cowork，它會用 5 題快速訪談跟你互動、自動寫進 CLAUDE.md：

```jsx
我要為自己建立一份精簡的 AI 工作偏好設定。請用一題一題互動問答的方式跟我跑 5 題訪談，
回答完後把答案整理成 250 字內的 Markdown，直接寫進 CLAUDE.md 的「## 我是誰」段落（覆蓋現有占位文字）。

訪談題目：
1. 你的職業／角色／產業是什麼?
2. 你一週大部分時間在做什麼?(具體任務)
3. 你希望 Claude 給你的東西看起來像什麼?(語氣、長度、格式偏好)
4. 你最受不了 AI 寫東西時哪些習慣?
5. 有沒有絕對不要 Claude 做的事?(硬底線)

回答完之後請：
- 把答案整理成 5 個小段、每段對應一題、用「## 我是誰」當小節標題的子段落
- 字數壓在 250 字以內
- 直接寫進這個資料夾的 CLAUDE.md、覆蓋占位文字
```

當然，如果你已經有現成的個人化設定（例如 5 月做過的 Profile Instructions 存檔），也可以直接貼進來覆蓋占位文字、跳過上面這段訪談，讀者們可隨喜。

### 全域指示與 CLAUDE.md 怎麼分工

設好之後，Cowork 對話前其實會讀兩個地方的指示，分工很重要：

| 名稱 | 位置 | 內容性質 | 範例 |
| --- | --- | --- | --- |
| 全域指示 | Settings 面板 | 跨所有 Cowork 對話的普世規則 | 不刪檔、動作前列計畫、不要自動補圖 |
| **CLAUDE.md** | 資料夾根目錄 | 跟這個工作環境綁定的規則 | 你是誰、這個資料夾結構、產出該放哪個子資料夾 |

邏輯上，全域指示是最頂層的規範、跨所有資料夾都不變；CLAUDE.md 則是依資料夾不同而有差異。

舉個具體例子：你今天要做兩份 PPTX，一份的工作環境是「蘋果風」資料夾、一份是「輝達風」資料夾。蘋果風的 CLAUDE.md 寫「配色白底黑字、字型無襯線、版面極簡」；輝達風的 CLAUDE.md 寫「配色綠黑強對比、字型有科技感、版面強調力量」。同一個 PPTX 任務，因為兩個資料夾的 CLAUDE.md 不同，產出風格就會完全不同。

但有一類規則是跨資料夾都該守的，這就放進全域指示。例如「不要用中國用語『視頻』」這種文字風格規範，寫進全域指示後，不管你在蘋果風還是輝達風資料夾做 PPTX、都不會出現「視頻」；「動任何檔案前先問我」、「不要自動補圖」這種安全規則也是同類，寫一次就涵蓋所有資料夾，不必每個 CLAUDE.md 重複寫。

### 進階用法：memory.md

以下是進階做法，新手可以先跳過、等熟悉 CLAUDE.md 工作流之後再回來啟用。

除了上面講的「全域指示／CLAUDE.md」兩種規則來源，Cowork 還有另一種跟記憶有關的進階用法：本地記憶檔（memory.md）。跟 CLAUDE.md 不同的是，它不會自動載入。要讓它生效，先在 Cowork-Playground 根目錄建一個空白的 memory.md，然後在 CLAUDE.md 加一行：「開始任何任務前，先讀 memory.md，套用裡面記錄的偏好與決策。」這樣 Cowork 每次讀 CLAUDE.md 就會自動翻 memory.md。

接著只要你在每次收工或踩到雷後請 Cowork 把這次學到的東西記進 memory.md，你的 AI 會越來越懂你。

![#5 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/izrx-1780629613.jpg?w=1200&output=webp)

多加一句設定，就可以讓AI做事前先讀取記憶檔，這可以讓高頻任務的品質增加。

圖／ 數位時代

## 不想寫 Skill？錄一段螢幕給它看

Anthropic 在 7 月 給 Cowork 加了一條捷徑：Record a skill。你不用自己描述工組流程，直接做一次給 Cowork 看就好。在桌面版的「＋」選單點 Record a skill，按下開始錄製，然後照平常的方式把工作做一遍。Claude 會同時錄下你的螢幕畫面、滑鼠點擊、鍵盤輸入，以及你的語音講解，錄完之後反推出這件事的步驟，整理成一份 Skill 草稿。這項功能開放給 Pro、Max、Team 訂閱者，目前只在桌面版有，網頁與手機版還看不到。

想看這項功能的完整發布內容，可參考《數位時代》這篇〈 [Claude Cowork推「Record a skill」新功能！3步驟複製你的工作流程，把SOP變AI技能庫](https://www.bnext.com.tw/article/91585/claude-cowork-record-a-skill) 〉。

## 單元三｜給 Cowork 下 prompt 的三件事

Cowork 跑得順的人，不是每次都下 prompt 的人。他們把工作分兩層：例行的事丟給 Skill 自動跑、其餘臨時任務才下 prompt。5 月你已經做過第一個 Skill，每週都要做、輸出格式又固定的事就該包成 Skill。本單元教的是另一條路：還沒被 Skill 化的臨時任務該怎麼下指令。

開始用 Cowork 跑真實工作會發現，prompt 只要說清楚下面這三件事，產出品質就穩定。

### 第一件：成品長什麼樣

要交代清楚成品是什麼格式（docx、xlsx、pdf、新資料夾結構）、長度多少（字數、頁數）、檔案要存在哪裡（哪個資料夾、檔名怎麼命名）。先花幾分鐘寫清楚成品標準，可以省下後面 5 輪「不對、這不是我要的」的來回。

### 第二件：約束條件是什麼

你以為的「常識」對 Cowork 不一定是常識，所以所有硬性的限制，你都要寫進 prompt，例如哪些事不能做（不刪檔、不蓋掉、不改 A 欄資料）；要用哪個範本、哪種配色、哪種字型；有哪些忌諱（不要 AI 風格示意圖、不要太多 emoji、不用「值得注意的是」這類開場白）。這些約束寫一次寫進 CLAUDE.md，之後每次對話自動套用，不必每個 prompt 重講。

### 第三件：告訴它不確定時怎麼辦

這是最容易漏的一條。一般人下 prompt 時通常只想「完美狀況」，但實作時十次有八次 Cowork 會遇到模糊情境。例如，你丟一份英文 PDF 給 Cowork、請它做成中文 PPT。如果你沒交代「遇到看不清的數字怎麼辦」，它可能就自己估個數字填進去、繼續往下做，最後你打開 PPT 才發現有幾頁數字錯了。

但如果你在 prompt 最後加一句「看不清楚的數字請集中列在最末頁的待補清單、不要自己估」，Cowork 就會把這些有疑慮的地方主動回報、由你決定怎麼處理。寫進這條規則之後，Cowork 就會從「偶爾犯錯、要你回頭找問題的工具」變成「會主動告訴你哪些地方需要人工判斷的助理」。

### 對照範例：壞 prompt vs 好 prompt

| 對照 | prompt 內容 |
| --- | --- |
| ❌ 壞 prompt | 幫我整理檔案 |
| ✅ 好 prompt | 把這個資料夾的檔案按客戶名分子資料夾，檔名用 YYYY-MM-DD 格式，產一份變動 log 存到根目錄，不刪任何檔，多歸屬的丟 /needs-review |

兩段差別 30 秒，但 Cowork 跑出來的結果差兩個小時。

## 單元四｜實戰：把一份英文研究報告變成中文 PPTX

讀到這裡你已經學會：把 Cowork 環境設好、把「你是誰」搬進 CLAUDE.md、寫好 prompt 的三件事。現在把這些放在一起，跑一個完整任務：把一份 Anthropic 4 月發布的 15 頁英文研究報告，變成一份 10 頁可編輯的繁體中文 PPTX。

挑這個實戰任務有兩個理由。一是內容剛好夠，這份報告叫《8.1 萬人告訴我們 AI 的經濟意義》，是 Anthropic 4 月 22 日發布、整理自約 8.1 萬名 Claude 使用者回饋的研究。15 頁、6 張數據圖、5 個量化發現、若干受訪者引言，做 10 頁 PPTX 剛好，既不會少到沒料、也不會多到 Cowork 處理不來。

第二，雙重意義。讀者做完學到的不只是「英文長文怎麼變中文簡報」這個工作流，讀完報告本身，也對「AI 對工作的衝擊」有了具體理解。雖然 Cowork 產的 PPTX 在版面精緻度上不如專業設計工具，但結構、字型、配色、可編輯性都到位，足以應付多數內部簡報初稿。

### 完整流程四步驟

**第 1 步：準備素材。** 從 anthropic.com/research/81k-economics 下載 PDF（檔名建議重新命名為 81000-people-AI-economics.pdf），放進 Cowork-Playground/inputs/ 資料夾。Cowork 會自己讀 PDF，不需要另外轉檔。

**第 2 步：下三件式 prompt。** 把單元三的三件式直接套用，複製下面這段給 Cowork：

```jsx
把 inputs/81000-people-AI-economics.pdf 改寫成 10 頁可編輯的繁體中文 PPTX，
存到 outputs/ 資料夾，檔名 AI-economics-deck.pptx。

結構：
- 封面 1 頁
- Key Findings 1 頁
- 誰擔心被取代 2 頁
- 誰從 AI 受益 2 頁
- 生產力提升的型態 2 頁
- 結論 1 頁
- 延伸閱讀 1 頁

約束：
- 每頁字數不超過 80 中文字
- 保留原報告的 6 張數據圖（用截圖嵌入）
- 配色用 CLAUDE.md 裡的品牌色，字型用思源黑體
- 引言保留英文原文加中譯

不確定時：
- 英文術語找不到精準中譯就用 [原文待譯]
- 數字看不清楚就標 [TBD]
- 集中列在延伸閱讀頁下方的 follow-up 清單
```

仔細看這段 prompt，其實就是單元三教的三件式：成品定義（10 頁結構）、約束條件（字數、配色、字型、引言處理方式）、不確定時怎麼辦（標記方式、集中列出）。

**第 3 步：看計畫、再放行。** 因為全域指示設了「動手前先列計畫」，Cowork 會先回類似這樣的訊息：

> 我打算這樣做：讀 81000-people-AI-economics.pdf、抓出 6 個量化發現與 6 張數據圖、用 python-pptx 套件產出可編輯 PPTX、檔名 AI-economics-deck.pptx 存到 outputs/，預估 3 到 5 分鐘，要繼續嗎？

![#7 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/cvja-1780629614.jpg?w=1200&output=webp)

等AI列出製作計畫後，若沒問題就回覆OK，接下來Cowork就會開始製作pptx。

圖／ 數位時代

**第 4 步：先用 Cowork 對話改大方向，最後再開 PowerPoint 做交付前檢查。** 成品出來後，先別急著打開 PowerPoint 手動改，可以直接在 Cowork 對話框繼續說話，例如「第 3 頁的 81,000 改成 8.1 萬」「整體配色太冷，改暖一階」「結論那頁太空，多加一句總結句」。Cowork 改完直接覆寫檔案；等大方向都改好，再打開 PPTX 做最後檢查（版面、字型、圖表截圖、數字是否正確）。

![#1 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/b4wm-1780629612.jpg?w=1200&output=webp)

Claude會進去閱讀報告，並截圖製作簡報。

圖／ 數位時代

![#6 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/sdb6-1780629613.jpg?w=1200&output=webp)

在沒有特別指示的情況下，Claude會去媒合適合的圖表與訪談內文。

圖／ 數位時代

以上從輸入 Prompt 到成品產出，中間耗時大概 8 分鐘。想看完整 PPTX 檔案的讀者 [請點我](https://docs.google.com/presentation/d/1ZOm7UVEMUBYMWkwCYdhvYXyGjzErE1V8/edit?usp=sharing&ouid=102486943050644268563&rtpof=true&sd=true) 。寫到這，Cowork 已經能幫你產出可供檢查與修改的初稿。下一單元教你怎麼讓它不用等你開口、自己按表操課。

## 單元五｜讓 Cowork 自己上班：排程任務與外部連線

前四單元都在教你「怎麼跟 Cowork 對話」：你打字、它工作。本單元換軌：「怎麼讓 Cowork 不用對話、自己做事」。從工具升級為系統。

### 排程任務：用 /schedule 讓 Cowork 按表操課

在任何 Cowork 對話框輸入 /schedule，Cowork 會用問答介面帶你設好頻率（每日、每週、每月、按需）。三個立即能套用、ROI 最高的排程案例：每週一早上 7 點掃 Google Calendar 跟未讀 Slack，整理本週重點與需要準備的會議，你進辦公室時已經有了；每週五下午 4 點從你的專案管理工具撈本週完成事項，產一份週報草稿，週末不必再煩這件事；每天早上 6 點讀 Gmail 分類 + 草擬回信（只產草稿、不自動寄出），你 9 點開電腦時，信箱已經被處理過一輪。

![#3 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/as9v-1780629613.jpg?w=1200&output=webp)

要注意，排程任務只在「電腦開機 + Claude Desktop 應用程式開著」時才會跑。睡眠或關機時錯過的任務，Cowork 會在你回來時補跑通知你。

圖／ 數位時代

### Connectors：把 Cowork 接到外部服務

Connectors（連接器，把 Gmail、Notion、Slack 等外部服務接進 Cowork 的橋樑）。本機檔案只是 Cowork 能讀的「私領域」，Connectors 把它的眼界拓展到「公開／團隊領域」。設定路徑：Customize → Connectors → Browse connectors，數十個整合可選。最低建議起手的 4 個：Gmail、Google Calendar、Google Drive、Notion。

![#8 Claude Cowork工作流](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/ktqt-1780629614.jpg?w=1200&output=webp)

Connectors 是 Claude 可以調用各種工具的關鍵，一定要記得設定。

圖／ 數位時代

一個單獨用 Connector 的小例子是，把 Gmail 連上後，跟 Cowork 說「讀我過去一個月的信件，萃取我的寫作 tone of voice 存進 CLAUDE.md 的『# 我的寫作風格』段落」 下次 Cowork 幫你草擬回信時，會自動套用你的口吻。Connector 也可以跟排程任務搭配，例如每週一早上 7 點自動讀過去一週的 Gmail 跟 Slack、整理出本週重點存進 outputs/ 資料夾，你進辦公室時報告就在桌上。

但必須說排程任務耗費 token 相對多很多，別把不重要的任務也排上去。此外，Cowork 自動讀外部資料時風險也更高，因此敏感操作（轉帳、刪檔、寄信給客戶）要設「動作前需確認」護欄。而且Connectors 的存取範圍也要常檢查，不需要的權限隨時撤掉。

## 兩期讀完，你帶走了什麼？

兩期讀完，用 Claude Cowork 你帶走了哪三樣資產？這三樣不會因為你換電腦、換公司、或 AI 工具換代而失效，它們是你真正帶得走的資產。

第一樣：第一個工作流 Skill。 5 月教你用 skill-creator 包出來的那個 Skill，它不存在某個 App 裡，是綁在你的 Claude 帳號上的能力。你切到 Cowork、切到手機 App，它都跟著你。再多一個 Skill、再多一個，你的工作流就越被自動化。

第二樣：CLAUDE.md（本機的「你是誰」）。 從 5 月的 500 字 Profile Instructions 開始、現在搬下本機變 CLAUDE.md，未來還會越寫越深。這份檔案是你的數位人格切片，換新電腦時一個資料夾就帶得走。

第三樣：給 AI 下指令的肌肉記憶。 你已經知道：寫好一個 prompt 要說清楚三件事（成品長什麼樣、有什麼約束、不確定時怎麼辦）。這個肌肉不只用在 Cowork。未來 AI 工具更新換代，你也不會被淘汰，因為你會的是「怎麼交辦工作」這件事本身。

4 月教你跟 AI 開始對話、5 月教你建立 Skills、6 月教你下本機進 Cowork。每一期都把你從上一站推到下一站。下一期主題先賣個關子，但有件事可以先說：你接下來最該投資的不是「再多學一個新工具」，是把這兩期建立的三樣資產練深練熟。

[登入會員免費收藏專刊、下載全文PDF](https://www.bnext.com.tw/member/special-issue/view/1446)

> 延伸閱讀： [Claude入門學習地圖！38篇教學、8大主題一次收，Cowork、Skills、省Token秘訣全打包](https://www.bnext.com.tw/article/91080/claude-cowork-skills)  
> [Claude Code快捷鍵+指令大全！13大類速查不用背，從Ctrl+C到多Agent協作一次整理](https://www.bnext.com.tw/article/90925/claude-code-slash-commands-shortcuts-complete-guide)

本文不開放轉載