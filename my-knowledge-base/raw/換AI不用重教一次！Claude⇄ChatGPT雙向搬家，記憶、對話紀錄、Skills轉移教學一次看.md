---
title: "換AI不用重教一次！Claude⇄ChatGPT雙向搬家，記憶、對話紀錄、Skills轉移教學一次看"
source: "https://www.bnext.com.tw/article/92350/claude-chatgpt-migration"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-09-28
created: 2026-09-29
description: "Claude 和 ChatGPT 可以互相搬家嗎？本文整理記憶、對話紀錄與 Skills 的雙向轉移方法，以及不同方案、裝置與帳號的限制。"
tags:
  - "clippings"
status: processed
---
[![](https://pk-img.learnin.tw/prod/photos/2026-09/img-1790646651-30210.gif)](https://edm.bnext.com.tw/martech-trends/?utm_source=web_bn&utm_medium=logo_banner&utm_campaign=2026_martech&utm_content=175749&utm_term=channel_all)

2026.09.28 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

Claude 和 ChatGPT 可以互相搬家嗎？本文整理記憶、對話紀錄與 Skills 的雙向轉移方法，以及不同方案、裝置與帳號的限制。

---

長期使用 AI，轉移 Claude、ChatGPT 等平台往往最費事，因為累積數月的工作背景、寫作偏好、專案脈絡與操作習慣，可能得重新設定，過去花時間建立的使用默契也很難直接快速轉移。

隨著 Claude、ChatGPT 陸續補上資料匯入、匯出與記憶轉移功能，現在轉移 AI 平台，已經有方法把部分記憶、偏好與工作設定一起搬過去，甚至連 Skills 都能轉移。

本文參考 [《How to AI》電子報作者 Ruben Hassid 整理的流程，](https://ruben.substack.com/p/how-to-switch-ai) 並比對 Anthropic、OpenAI 官方文件，實測整理「Claude 轉 ChatGPT」與「ChatGPT 轉 Claude」兩種搬家方式，帶你一次搞懂哪些資料能搬、怎麼搬，以及有哪些限制。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

**文章目錄**

[**一、Claude、ChatGPT 記憶可以互搬嗎？哪些資料能轉移？**](#表格)  
[**二、Claude 轉 ChatGPT 怎麼做？記憶、Skills 搬家 6 步驟教學**](#Claude轉ChatGPT)  
[**三、ChatGPT 轉 Claude 怎麼做？記憶匯入 2 步驟教學**](#ChatGPT轉Claude)  
[**四、Claude、ChatGPT 搬家有哪些限制？手機無法匯出、記憶匯入不完整等 6 項限制**](#限制)  
[**五、Claude 轉 ChatGPT 常見問題（FAQ）**](#常見問題)

## 一、Claude、ChatGPT 記憶可以互搬嗎？哪些資料能轉移？

Claude 與 ChatGPT 的記憶、偏好與 Skills 可以互相轉移，但完整對話紀錄無法直接搬到另一個平台。截至 2026 年 9 月，兩個方向的搬家方式整理如下：

| 要搬的資料 | Claude → ChatGPT | ChatGPT → Claude |
| --- | --- | --- |
| 記憶與偏好 | 匯出 Claude 對話，整理成記憶檔後上傳 ChatGPT | 使用 Claude 內建的記憶匯入工具 |
| Skills 與工作設定 | 使用 ChatGPT 桌面版「匯入」功能 | 將 Skill 資料夾壓縮成 ZIP 後手動上傳 Claude |
| 可操作的裝置 | Claude 網頁版或桌面版；ChatGPT 網頁版（記憶檔）、桌面版（Skills 匯入） | 可登入 Claude 設定頁的裝置 |

換句話說，AI 搬家主要是把個人背景、偏好與工作設定複製到新平台，原本的對話紀錄通常不會出現在另一個 AI 裡。

## 二、Claude 轉 ChatGPT 怎麼做？記憶、Skills 搬家 6 步驟教學

### Claude 轉 ChatGPT 第一步：怎麼匯出 Claude 對話紀錄？

Claude 的對話資料可以從「設定 → 隱私 → 匯出資料」下載，目前只能透過網頁版或桌面版操作，iOS、Android App 都無法匯出。

操作方式如下：

1. 點選左下角帳號縮寫，進入「設定」（Settings），打開「隱私」（Privacy）分頁。

![#1 Claude 轉 ChatGPT](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/qxzg-1790126115.png?w=2000&output=webp)

圖／ 數位時代

2. 選擇「匯出資料」（Export data），可選擇對話起始時間，最後點選「匯出」（Export），系統完成資料整理後，會將下載連結寄送到帳號信箱，並另存新檔。

![#0 Claude 轉 ChatGPT](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/y1r4-1790126114.png?w=2000&output=webp)

圖／ 數位時代

如果使用的是公司帳號，還要留意組織權限，Team 或 Enterprise 方案的一般成員無法自行匯出資料，必須由組織的 Primary Owner 操作。

### Claude 轉 ChatGPT 第二步：怎麼下載 Claude 完整對話檔？

信件附的下載檔不是對話本身，而是一份列出下載連結的索引檔，須照以下步驟才能拿到完整對話：

1. 點選信件中的下載按鈕，在跳出的「另存新檔」視窗選一個好找的位置（例如桌面），檔名與副檔名.json 維持原樣，按「存檔」。

![#0 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/sxa3-1790214320.png?w=2000&output=webp)

圖／ 數位時代

2. 到剛才的儲存位置找到.json 檔，Windows 右鍵選「開啟檔案 → 記事本」；Mac 選「打開檔案的應用程式 → 文字編輯」。

檔案裡會有兩個以「https://」開頭的網址，內容較多時，可以按 Ctrl＋F（Mac 為 Command＋F）搜尋「https」快速找到。選取第一個網址，從「https」開始複製到網址結束為止，前後的引號不要選進去。

![#1 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/us2d-1790214321.png?w=2000&output=webp)

圖／ 數位時代

要留意的有兩點，第一，這份從信箱下載的.json 檔只是一份索引清單，檔案中出現的是下載連結，不含任何對話內容。打開或上傳它並不等於拿到資料， **真正的內容要透過清單裡的連結另外下載才能取得** 。

第二，檔案中的 2 組連結直接指向你的個人資料下載入口，等同於一次性通行密鑰，請勿轉傳給他人，也不建議貼進任何 AI 對話裡，以免資料外洩。

3. 打開瀏覽器，把第一個網址貼到網址列按 Enter，瀏覽器會自動下載一個 zip 檔，也就是 conversations-000.zip，並將其解壓縮取得 conversations.json 檔，因為主要的對話內容在裡頭。

![#2 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/ck97-1790214321.png?w=2000&output=webp)

圖／ 數位時代

本次教學只需要對話紀錄，因此第二個連結 light\_metadata-000 可先跳過。

### Claude 轉 ChatGPT 第三步：怎麼把大量對話整理成「記憶檔」？

原始對話檔動輒數百則，直接丟給 ChatGPT 效果有限。比較好的做法是先請 Claude 把對話濃縮成一份 Markdown 記憶檔。

1. 開啟新的 Claude 對話框，上傳剛才解壓縮的 conversations.json 檔，並貼上以下提示詞。以下版本依 Ruben Hassid 提供的架構，由《數位時代》精簡改寫：
```
我要從 Claude 轉到 ChatGPT。請讀取我上傳的對話紀錄，並結合你目前能存取的關於我的記憶，整理成一份名為 Claude-to-ChatGPT.md 的 Markdown 檔。

請依以下章節整理：
1. 核心背景：我的身分、工作、團隊與長期目標
2. 回應指令：一段 1,500 字元以內、以第一人稱撰寫的指令，可直接貼進 ChatGPT 自訂指令
3. 長期偏好：寫作風格、格式要求、禁用詞與我明確糾正過的地方
4. 進行中的專案：目的、最新狀態與日期、已確定的決策
5. 衝突與過時資訊：互相矛盾或可能已過期的內容

規則：
- 只把我明確同意的內容列為決策，不要把你的建議當成我的決定
- 數字、價格、專案狀態都要標註日期
- 排除密碼、API 金鑰與任何下載連結
- 如果有檔案讀不到，請在文末說明，不要假裝已讀取
```

![Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/wt7m-1790214985.png?w=2000&output=webp)

圖／ 數位時代

2. 記憶檔產出後，點選下載「Claude-to-ChatGPT.md」記憶檔文件。建議先自行檢查一次，移除不希望帶到 ChatGPT 的個人或專案資訊。

![#0 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/jxpm-1790215869.png?w=2000&output=webp)

圖／ 數位時代

如果資料量很大，也可以要求 Claude 先整理，再針對資訊不足的地方反問幾個問題，補齊重要背景。

### Claude 轉 ChatGPT 第四步：怎麼把記憶檔放進 ChatGPT？

接下來，整理完成的記憶檔，可以先存進 ChatGPT 的檔案庫（Library），再從對話中加入檔案，請 ChatGPT 擷取適合長期保留的內容。

檔案庫目前開放給 Free、Go、Plus、Pro、Business 等方案，需透過網頁版使用。操作方式如下：

1. 從 ChatGPT 左側欄開啟檔案庫，點選右上角「新增 → 上傳」，上傳剛剛的「 `Claude-to-ChatGPT.md` 」檔案。

![#1 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/ixmf-1790215869.png?w=2000&output=webp)

圖／ 數位時代

2. 上傳後，勾選該檔案，下方選擇「開始對話」開啟新的 ChatGPT 對話。

![#2 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/v4ej-1790215870.png?w=2000&output=webp)

圖／ 數位時代

3. 在新對話框中，貼上以下提示詞範例：
```
附件是我從 Claude 匯出的個人背景檔。請先完整讀過，接著：
1. 把「核心背景」與「長期偏好」中對未來對話有用的資訊存進記憶，一則一個事實。
2. 標記為過時或有衝突的內容不要存。
3. 完成後列出你存了哪些記憶，讓我檢查。
```

![Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/gbsi-1790216265.png?w=2000&output=webp)

圖／ 數位時代

ChatGPT 列出清單後，逐條確認有沒有漏存或誤存。記憶開關位在「設定 → 個人化 → 記憶」，也可以在那裡檢查結果。

### Claude 轉 ChatGPT 第五步：寫作語氣、格式偏好怎麼轉移到ChatGPT？

說話語氣、格式偏好這類明確規則，比較適合放在 ChatGPT 的自訂指令。OpenAI 建議，明確的資訊或指示放進自訂指令，對話中提到的細節則交給記憶處理。

因此，可以回到第三步建立的記憶檔，操作方式如下：

1. 打開「Claude-to-ChatGPT.md」記憶檔文件，找到「回應指令」段落，直接複製到 ChatGPT 的自訂指令欄位。

![#0 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/bft2-1790217417.png?w=2000&output=webp)

圖／ 數位時代

2. 點擊 ChatGPT 左下角的個人頭像或帳戶選單，進入「設定」（Settings），選擇「個人化」（Personalization）。

![#1 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/8whm-1790217417.png?w=2000&output=webp)

圖／ 數位時代

3. 找到「自訂指令」（Custom Instructions）區塊，貼入剛剛複製的回應指令，然後儲存或完成設定。

![#2 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/2anu-1790217417.png?w=2000&output=webp)

圖／ 數位時代

簡單來說，個人背景與長期資訊可以交給記憶處理；寫作格式、回答方式與固定規則，則適合放進自訂指令。

### Claude 轉 ChatGPT 第六步：Claude Code、Cowork 的 Skills 怎麼搬？

如果平常有使用 Claude Code 或 Claude Cowork，ChatGPT 桌面版也提供「匯入」功能，可以直接帶入部分代理工具的設定。

依 OpenAI 說明，目前支援匯入的來源包括 Claude Code、Claude Cowork 與 Cursor，可匯入指令、設定、Skills、專案與近期工作等內容。

操作步驟如下：

1. 開啟 ChatGPT 桌面版，進入「設定 → 匯入」。如果沒有看到獨立的「匯入」分頁，也可以到「一般」尋找「Import other agent setup」。

![#0 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/27gu-1790218432.png?w=2000&output=webp)

圖／ 數位時代

2. 在「從其他 AI 應用程式匯入」區塊中，找到偵測到的來源（例如 Claude Cowork），點選該項目右側的「匯入」按鈕。接著會出現可勾選的項目清單（如指令、設定、Skills、專案等），依需求勾選後按下「繼續」即可完成匯入。

![#1 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/bwt3-1790218433.png?w=2000&output=webp)

圖／ 數位時代

![#2 Claude 轉 Chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/cj61-1790218434.png?w=2000&output=webp)

圖／ 數位時代

如果畫面顯示部分項目仍需要設定，點選「Finish」，依照提示完成後續操作。

匯入功能會在 ChatGPT 端建立一份設定，不會刪除原本工具裡的內容。部分設定也支援自動更新，讓 ChatGPT 與原始工具維持同步。

不過，Anthropic 在 2026 年 9 月 16 日宣布調整 Claude 產品介面，逐步將 Chat、Cowork 與 Artifacts 整合至統一介面，由 Claude 自動判斷任務所需能力，不再要求使用者手動切換 Cowork 分頁；新版介面先從 Pro、Max 方案分批推出。

## 三、ChatGPT 轉 Claude 怎麼做？記憶匯入 2 步驟教學

Claude 也提供將其他 AI 記憶匯入平台的工具，因此 ChatGPT 轉 Claude 的記憶搬家流程相對直接。

### ChatGPT 轉 Claude 第一步：怎麼把 ChatGPT 記憶整理出來？

Claude 的做法，是先提供一段提示詞，讓 ChatGPT 將現有記憶整理成可匯入格式，再把結果複製回 Claude。

操作方式如下：

1. 打開 Claude，在「設定（Settings）→ 記憶（Memory）」找到記憶設定頁面，點選「Start import」。（若帳號還是舊版介面，記憶區塊會在「設定 → 功能（Capabilities）」裡）。

![#0 claude 轉 chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/jy5h-1790301066.png?w=2000&output=webp)

圖／ 數位時代

2. 複製 Claude 提供的匯出提示詞，貼到 ChatGPT 對話中執行。

![#2 claude 轉 chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/zeqc-1790301068.png?w=2000&output=webp)

圖／ 數位時代

3. ChatGPT 會將記憶整理在程式碼區塊中，再把完整內容複製下來。 **但如果 ChatGPT 回覆的是一堆「找不到記憶」怎麼辦？**

貼上匯出提示詞後，如果 ChatGPT 回覆的是類似這樣的內容：

> Instructions - \[unknown\] 找不到相關記憶  
> Identity - \[unknown\] 找不到相關記憶  
> ⋯（其他分類都是空的）

可依照下列方式依序檢查：

1. 首先確認 ChatGPT「設定 → 個人化 → 記憶」中的記憶功能確實已開啟
2. 進入記憶設定頁，查看「記憶摘要」、「管理」或「已儲存記憶」中的內容，確認帳號目前有哪些已保存的記憶內容。
3. 若清單確有內容、制式提示詞卻問不出結果，先在對話框直接提問：「你記得我哪些事？」
4. 問出內容後，若回答太過濃縮，可以再追問：「不要幫我摘要或濃縮，請把所有內容一條一條列出來，越詳細越好」，最後再請它「整理成一個程式碼區塊，方便我複製貼上」。

![claude 轉 chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/1dw1-1790303175.png?w=2000&output=webp)

圖／ 數位時代

貼回 Claude 前，建議先檢查一次，刪除過時、不正確，或不希望 Claude 保留的條目。

### ChatGPT 轉 Claude 第二步：怎麼把記憶存進 Claude？

取得 ChatGPT 整理好的記憶後，回到 Claude 的記憶設定頁繼續設定：

1. 在記憶設定的「Import memory to Claude」頁面中，將內容貼進下方「Paste results below to add to Claude's memory」文字框，再點選「Add to memory」。

![#3 claude 轉 chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/8r22-1790301068.png?w=2000&output=webp)

圖／ 數位時代

送出後 Claude 會分析內容、拆成多筆記憶條目儲存，畫面通常不會即時更新，可能要等一下才會看到結果。

有兩點要注意：

- 記憶匯入目前仍屬實驗性功能，並非每次都能完整吸收匯入的內容。
- Claude 的記憶設計上比較聚焦於工作相關主題，因此某些與工作無關的個人細節匯入後可能不會被保留。若有特定資訊一定要留住，可以在記憶設定頁用「Tell Claude what to change or remove」補充，或直接在對話中告訴 Claude。
2. 如果想確認匯入是否成功，可以重新開一個 Claude 對話，詢問：「你目前對我有哪些了解？」，根據回答內容檢查重要資訊是否已經被保留。

![#4 claude 轉 chatgpt](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/jhpt-1790301068.png?w=2000&output=webp)

圖／ 數位時代

### ChatGPT 轉 Claude 同場加映：Codex 的 Skills 怎麼搬？

如果想把 Codex 使用的 Skills 搬到 Claude，目前需要手動處理。

Codex 與 Claude 的 Skill 結構相近，都採用「Skill 資料夾＋SKILL.md」作為核心架構，因此主要由指令與參考資料組成的 Skill，通常可以沿用。不過，如果 Skill 使用 Codex 專屬工具、MCP、agents/openai.yaml 或特定執行環境，搬到 Claude 後可能需要另外調整。

操作方式如下：

1. 找到 Codex 的 Skill 資料夾。依 OpenAI 官方規範，個人 Skills 可存放在使用者主目錄下的.agents/skills；專案使用的 Skills 則放在專案內的.agents/skills。Codex 的 Skill 以 SKILL.md 作為主要設定檔。
2. 搬到 Claude 前，先將 Codex Skill 中的 SKILL.md 改名為 Claude 使用的 skill.md，並檢查其中的設定是否相容。接著將每一個 Skill 資料夾個別壓縮成 ZIP 檔。

依 Claude 官方規範，ZIP 最外層應為 Skill 資料夾，skill.md、參考資料與其他檔案再放在該資料夾內，不要直接將 skill.md 放在 ZIP 最外層。

3. 將 ZIP 上傳至 Claude。進入「Customize → Skills」，點選「＋ → Create skill → Upload a skill」，再選擇剛才建立的 ZIP 檔。上傳完成後，Skill 會出現在清單中，可自行開啟或關閉。

Claude Skills 目前開放給 Free、Pro、Max、Team 與 Enterprise 方案，但必須先啟用程式碼執行功能。若是 Enterprise 方案，需要組織管理員先在「Organization settings → Skills」中開啟程式碼執行與 Skills 功能，一般成員才能各自上傳個人 Skill。

## 四、Claude、ChatGPT 搬家有哪些限制？手機無法匯出、記憶匯入不完整

目前 Claude 與 ChatGPT 都無法一鍵完整搬家，也會遇到手機無法匯出 Claude 資料、對話紀錄無法重現等問題。因此匯出、匯入前建議先確認以下 6 項限制：

| 限制 | 說明 |
| --- | --- |
| 手機無法匯出 Claude 資料 | Claude 資料匯出目前只能使用網頁版或桌面版 |
| 公司帳號需管理者權限 | Claude Team、Enterprise 方案只有 Primary Owner 能匯出資料 |
| Claude 記憶匯入會篩選內容 | 匯入後的內容不一定會全部保留 |
| 不同地區的 ChatGPT 記憶介面可能不同 | 新功能採分批推出，設定畫面可能因帳號與地區而異 |
| Claude Cowork 正分批整合進統一 Claude 介面 | 2026 年 9 月 16 日起陸續推出，Claude 相關設定位置未來可能調整 |
| 對話紀錄無法重現 | 匯入僅複製設定，原平台的完整對話不會搬過去 |

## 五、Claude 轉 ChatGPT 常見問題（FAQ）

### Q：手機 App 可以匯出 Claude 資料嗎？

目前無法，Claude 資料匯出只能在網頁版或桌面版操作。截至 2026 年 9 月，iOS、Android App 都不支援匯出，需改用電腦登入 Claude，從「設定 → 隱私」執行匯出。

### Q：Claude Team 公司帳號可以自己匯出資料嗎？

一般成員無法自行匯出。Claude Team 與 Enterprise 方案目前只有組織的 Primary Owner 能匯出資料，一般成員需請組織管理者協助。

### Q：Claude 資料匯出的下載連結多久會失效？

24 小時。匯出完成後，下載連結會寄到帳號綁定的信箱，須登入帳號才能下載，連結在寄出 24 小時後失效，過期可重新申請。

### Q：Claude 設定匯入 ChatGPT 後，原本的 Claude 資料會被刪除嗎？

不會。依 OpenAI 說明，匯入功能會在 ChatGPT 端新增一份設定，不會修改或刪除原本工具中的內容。

### Q：ChatGPT 的記憶匯入 Claude 後，會全部保留嗎？

不一定。Claude 記憶以工作相關主題為主，與工作無關的個人細節可能不會保留；官方也表示記憶匯入仍屬實驗功能，不一定每次都能成功併入。匯入後可到「設定 → 記憶」確認（舊版介面在「設定 → 功能」）確認實際內容。

### Q：Claude Code 設定可以搬到 Codex CLI 嗎？

可以，在 Codex CLI 輸入 /import 指令即可匯入。依 OpenAI 說明，/import 可匯入 Claude Code 的設定與近期對話，對話最多匯入近 30 天內的 50 則；claude.ai 網頁版的聊天紀錄不在匯入範圍內。

> 延伸閱讀
> 
> - [Claude推出「記憶轉移」功能！一鍵把你在ChatGPT、Gemini的AI偏好全搬走，2步驟搞定](https://www.bnext.com.tw/article/90203/claude-switch-to-claude-without-starting-over)
> - [Claude Skills是什麼、怎麼用？新手零基礎入門教學，不用每次重貼提示詞](https://www.bnext.com.tw/article/90870/claude-skills-guidebook)

資料來源： [Anthropic：匯出 Claude 資料](https://privacy.claude.com/en/articles/9450526-how-can-i-export-my-claude-data) 、 [Anthropic：匯入與匯出記憶](https://support.claude.com/en/articles/12123587-import-and-export-your-memory-from-claude) 、 [OpenAI：從其他 AI 代理匯入](https://learn.chatgpt.com/docs/import) 、 [OpenAI：ChatGPT 檔案儲存與 Library](https://help.openai.com/en/articles/20001052-file-storage-and-library-in-chatgpt) 、 [TechCrunch：Anthropic 合併 Claude 對話與 Cowork](https://techcrunch.com/2026/09/16/anthropic-merges-claude-chat-and-cowork-in-one-interface/) 、 [Ruben Hassid《How to AI》電子報](https://ruben.substack.com/p/how-to-switch-ai)

本文初稿由 AI 協助編撰，整理・編輯／蘇柔瑋

關鍵字： [＃ChatGPT](https://www.bnext.com.tw/tags/ChatGPT) [＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃Claude](https://www.bnext.com.tw/tags/Claude)

往下滑看下一篇文章

tw\_bnext \[Dynamic Article\]-20260925-08:03

00:00

00:00

00:44

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/bnext.com.tw/bnext.com.tw-1790323535.593496.mp4?verify=1790647200-IlGjDmDr6opG82L85sWH%2BUVPvAJyBMwrWISLwpswalk%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/bnext.com.tw/bnext.com.tw-1790323535.593496.mp4?token=zC-Dz_U2WEotOoKYbJuamttxeleDBa65otl8pBUp8hk&amp;expires=1790733600"></video><video title="Advertisement" src="https://rr6---sn-ipoxu-un5ez.googlevideo.com/videoplayback?expire=1790697604&amp;ei=BHC7atKfG4yYsdkPnenAoQI&amp;ip=60.250.84.232&amp;id=86ce0bfa833575e1&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=669&amp;met=1790668804,&amp;mh=x4&amp;mm=18&amp;mn=sn-ipoxu-un5ez&amp;ms=aub&amp;mv=m&amp;mvi=6&amp;pl=21&amp;rms=aub,aub&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=15.069&amp;lmt=1790059396673741&amp;mt=1790668369&amp;cpn=sTncy1YsSi7Y6syW&amp;txp=5532234&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AE0s2JYwRgIhAKA4VOk4fU2-MitwzhJcEkd3ZN7xlVIq0rfohyTvZlcRAiEAkX5vr4YTB-KINbQ8tr6pEGOf6wQb_fy3EuD5ORphwe0=&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRgIhALwt5fZE-UN_ezyLGCZQ8u4JcVAViDddAhuNaKSYFEUJAiEAg09XGYn3yEjzr-2WUzxVhp09bCovXzwrfO7vzs7BJtY=" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.791.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1559464790%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B957164676%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45803173%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95394594%5D%2C%5B95394595%2C%5B%5B45803173%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95401453%5D%2C%5B95401454%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%2C95340252%2C95340254%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=2297200033081925&amp;pvsid=6252245778562919&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F92350%2Fclaude-chatgpt-migration%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="168.75"></iframe>

pause

volume\_mute

AD

即時熱門文章

[1 記憶體真的缺貨？陳俊聖：DDR4、一般DDR5很充裕，真正缺的是高階零組件](https://www.bnext.com.tw/article/92343/acer-memory-ddr4-ddr5-shortage) [2 Claude官方課程有中文版了！22門全免費，涵蓋入門到API開發，完課還能拿證書](https://www.bnext.com.tw/article/91963/claude-academy-chinese-courses) [3 把LINE變成待辦小幫手！4步驟教你用Google Apps Script串接Gemini，一句話自動排入日曆](https://www.bnext.com.tw/article/91909/line-ai-todo-gas-gemini) [4 ChatGPT指令大全！日系美術插畫風、照片轉成羊毛氈、裝滿你的記憶鐵盒⋯140組提示詞一次整理](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete) [5 Gemini完整教學地圖！39篇實測整理，Notebooks、Spark、提示詞架構全打包](https://www.bnext.com.tw/article/91249/gemini-learning-map) [6 Gemini Notebook教學地圖！AI筆記本、簡報製作、財報解讀28篇精華用法一次看](https://www.bnext.com.tw/article/91193/notebooklm-complete-guide-and-use-cases)

![當技術不再是門檻，內容產業如何贏在未來？  2026 亞洲新媒體高峰會「REIMAGINE」前瞻思考十年後的文創生態與賽道](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/2vdj-1789441503.jpg?w=900&output=webp)

2026.09.16 |

當技術不再是門檻，內容產業如何贏在未來？ 2026 亞洲新媒體高峰會「REIMAGINE」前瞻思考十年後的文創生態與賽道

---

分享

近來每場產業論壇都在談AI，大多數談的是「怎麼用AI」。這次，我們想換個角度：「為什麼用AI？用來做什麼？十年後內容生態會是什麼樣貌？我們如何佈局？」今年NMEA（新媒體暨影視音發展協會）以「REIMAGINE 想像新視界」為主題，將於11月25日至26日舉行2026第八屆亞洲新媒體高峰會，號召影視內容產業跳脫短期焦慮，以長遠眼光看待未來，邀請影視音、科技、資本與文化決策者進行前瞻對話。研討的重點不在技術的創新，而是希望邀請大家發揮想像、開拓文化創意的未來新視野。

## REIMAGINE，啟動想像新視界

「REIMAGINE」意即重新想像，不僅指向內容形式的翻新，也意味著既有流程、組織與商業模式的重組；「想像新視界」，則取「世界」、「視野」與「視聽」等複合意涵，呼應新媒體暨影視音發展的無限可能。

「內容產業最近太焦慮了。」NMEA理事長李芃君指出，對內容從業者與企業決策者而言，當下的焦慮已不再僅是「如何使用不斷迭代的新工具」，而是更關鍵的生存命題：當創作技術門檻被弭平，內容的真正價值該如何被重構？台灣的產業優勢是什麼？協會每年透過年度關鍵字呼應產業發展節奏，走過七屆高峰會，從早期的鼓勵擁抱變革，到去年的韌性突圍；第八屆峰會，NMEA理事長李芃君指出，「我們不想再強調現下追逐AI的焦慮，希望將視野拉長至未來十年，邀請產業領袖談談他們心中對未來的想像與願景。十年後這個產業會呈現什麼樣貌？確立中長期目標，能讓我們更有方向性地穩步前行。」NMEA 秘書長袁支翔則表示：「這將是歷屆以來，思維最自由、最具探索性的一次高峰會！」

## 五大主軸匯流，勾勒產業共同目標

「AI帶來結構性的改變，它滲透進整個產銷的價值鏈。」李芃君說明，從前期腳本開發、現場拍攝到後期製作，AI的應用日益深入。同時，消費者的娛樂與觀影行為在改變，在步調緊湊、碎片化時間日益增多的生活趨勢下，敘事策略必須順應變化。

在這兩波大趨勢的交互影響下，無論是內容創作、傳播、資本或技術都在尋求新的機會。藉由技術點燃感動，是文化科技的價值與契機，李芃君以太空望遠鏡為喻：哈伯與韋伯望遠鏡同樣觀測外太空，因科技世代不同，展現出截然不同的星空樣貌。「科技是文化的放大鏡，也是轉譯工具，協助我們更好地傳遞對一個故事或創意的描述。台灣是科技島，也是多元創意的孕育地，文化科技絕對是關鍵的核心競爭力。」

本屆高峰會的議程延續NMEA的優良傳統，由全體理監事與產業專家共創研擬，將邀集超過五十位講者、展開五大主軸：「跨域資本新局、內容跨界匯流、AI創作浪潮、多螢幕時代的競合，以及在地娛樂生態系演化」的精采對話。協會也預告多項亮點議程：以AI創作浪潮為例，今年全聯中元節廣告《歸途》，以AI生成視覺搭配真人實拍，引發市場對科技表現與人文情感平衡的討論，高峰會也將邀請主創團隊，分享第一手的創意策略與成果。

內容跨界匯流方面，三立集團內容營運長王淑娟將剖析現象級節目《請世界吃桌》的創作過程，分享如何整合多元資源將在地文化推向國際；在地娛樂生態系則聚焦台灣主要都會區，探討如何與國際影視、音樂、藝文合作，延伸在地文化特色，自然帶動長期的城市品牌價值。此外，高峰會也邀請貴賓分享體育賽事凝聚全民文化認同與 Team Taiwan 的感動經驗，以及木棉花、尖端出版與巴哈姆特組成的「動漫金三角」，對談動漫產業鏈的跨界協作路徑。

## NMEA 引領產業對話

回顧過去一年，台灣文化內容展現豐沛活力，如：紀錄片《冠軍之路》記錄中華隊在世界棒球12強賽奪冠的歷程，深化了全民的情感共鳴；截至今年8月20日，國片票房累計達12億元，創下疫情後的高峰。要持續延續這股動能，制度端的協力不可或缺。NMEA長期扮演產業與政府之間的溝通橋樑，持續關注各項政策的推動成效。

放眼國際，日本已將動漫、遊戲、影視與音樂等內容產業列為國家重點產業；台灣則在2023年《文化創意產業發展法》修法時，已將以台灣內容為主的影視音產業提升至國家戰略地位。近期，面對政府發布的「AI新十大建設推動方案」，李芃君表示協會將持續建言，期盼將文化內容產業的需求一併納入國家AI藍圖。

李芃君強調，經濟成長若缺少文化底蘊支撐，難以永續強化國際地位與影響力；在產業生命力最旺盛的時刻，更必須同步鍛造屬於台灣獨特的文化價值與靈魂。「文創產業是由一群有使命感的眾人組成，我們希望能讓台灣的文化力持續提升！」隨著協會即將在2027年邁向成立十週年，她也期許，更多想像中的美好將持續從產業對話中發生。

歡迎加入 NMEA 新媒體暨影視音發展協會： [https://www.nmea.asia/join.html](https://www.nmea.asia/join.html)

即時熱門文章

[1 記憶體真的缺貨？陳俊聖：DDR4、一般DDR5很充裕，真正缺的是高階零組件](https://www.bnext.com.tw/article/92343/acer-memory-ddr4-ddr5-shortage) [2 Claude官方課程有中文版了！22門全免費，涵蓋入門到API開發，完課還能拿證書](https://www.bnext.com.tw/article/91963/claude-academy-chinese-courses) [3 把LINE變成待辦小幫手！4步驟教你用Google Apps Script串接Gemini，一句話自動排入日曆](https://www.bnext.com.tw/article/91909/line-ai-todo-gas-gemini) [4 ChatGPT指令大全！日系美術插畫風、照片轉成羊毛氈、裝滿你的記憶鐵盒⋯140組提示詞一次整理](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete) [5 Gemini完整教學地圖！39篇實測整理，Notebooks、Spark、提示詞架構全打包](https://www.bnext.com.tw/article/91249/gemini-learning-map) [6 Gemini Notebook教學地圖！AI筆記本、簡報製作、財報解讀28篇精華用法一次看](https://www.bnext.com.tw/article/91193/notebooklm-complete-guide-and-use-cases)

[![一次搞懂工業AI](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/b4gn-1790235909.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130276)