---
title: "Grok Bot是什麼？怎麼用？Grok Bot教學：4步驟建好「AI代理一人公司」"
source: "https://www.bnext.com.tw/article/91991/grok-bot-getting-started-60"
author:
  - "[[李先泰]]"
published: 2026-09-18
created: 2026-09-18
description: "月繳 60 美元就能組第一支 Grok Bot 團隊。四步驟建幕僚長，實測找題寫選題卡大約 10 分鐘。"
tags:
  - "clippings"
status: processed
---
[Grok Bot](https://www.bnext.com.tw/article/91832/spacexai-grok-bot-ai-agent-team) 是SpaceXAI推出的AI代理工具，每個Bot都能使用持續運行的雲端電腦，操作網站、應用程式與檔案，並透過Routine在使用者離線時繼續工作。它也支援多個Bot彼此分工，例如由幕僚長把研究、寫作與財務任務轉交給不同專責Bot。

換句話說，你只需要負責最終的審閱與決策，將例行公事放心交給 AI，徹底改變團隊處理日常繁瑣工作的方式。

《數位時代》實測，使用者只向幕僚長下達一句指令，3個Bot便自行完成找題、選題與製作字卡，整個過程約10分鐘。本文將說明 [Grok Bot](https://x.ai/bot) 是什麼、價格多少，以及如何用4步驟建立第一支AI代理團隊。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

![Brok Bot教學](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-08/j7ye-1787663982.png?w=2000&output=webp)

Brok Bot教學

圖／ Brok Bot

> ### 文章目錄
> 
> [Grok Bot是什麼？與一般AI聊天機器人有何不同？](#what-is-grok-bot)  
> [Grok Bot價格多少？最低每月60美元](#grok-bot-pricing)  
> [Grok Bot教學：4步驟建立第一個Bot](#grok-bot-tutorial)  
> [\- 第1步：下載桌面版並登入帳號](#download-grok-bot)  
> [\- 第2步：選擇每天使用的工具](#grok-bot-onboarding)  
> [\- 第3步：建立第一個Bot並設定職務](#create-first-bot)  
> [\- 第4步：撰寫職務章程與權限界線](#write-bot-instructions)  
> [Grok Bot幕僚長模式是什麼？多個Bot如何分工？](#chief-of-staff)  
> [Grok Bot實測：3個Bot約10分鐘完成找題與選題卡](#grok-bot-test)  
> [Teach a task、Routine與Plugins怎麼用？](#grok-bot-features)  
> [Grok Bot常見問題](#grok-bot-faq)

## Grok Bot是什麼？與一般AI聊天機器人有何不同？

總共就三個重點：

> 第一，它不是聊天機器人，是一組 AI 代理（agent，能自行拆解任務、代替使用者操作工具的 AI 程式），而且每一個都配一台自己的雲端電腦。
> 
> 第二，SpaceXAI（原 xAI）在美國時間 8 月 11 日推出 [Grok Bot](https://x.ai/bot) 早期測試版（early beta），支援 macOS 桌面版與 iOS。
> 
> 第三，它和 Cursor 深度綁定，用同一組帳號登入。SpaceX 與 Cursor 於 2026 年 6 月 16 日簽署合併協議，並在 8 月 14 日完成交割，Cursor 成為 SpaceX 全資子公司。

「有自己的電腦」是這個產品的關鍵差異。Cursor 員工 [mattyp](https://x.com/mattyp/status/2089758434921160877?s=20) 認為： **技術上是一台持續運行的 Linux 虛擬機，用起來像一台隨時開機的遠端桌面，能瀏覽網頁、開應用程式、存取檔案、截圖，做的事和真人一樣。**

也因為它是直接在畫面上動手，那些沒有現成 API、也沒接上 MCP（讓 AI 串接外部工具的通用介面標準）的平台，它照樣操作得動。跟現行的 Grok 或是 Codex 等產品比起來，Grok Bot 對自然語言的指令接受度更高，你不太需要安裝其他的外掛或工具，就可以透過它完成一些複雜的操作。

## Grok Bot價格多少？最低每月60美元

8 月 21 日之前，Grok Bot 主要綁在較高價方案。SpaceXAI 當天 [宣布把它下放到更多方案](https://x.ai/news/grok-bot-more-plans) ，納入 SuperGrok Plus、Cursor Pro+，以及 Cursor Teams 的 Standard 與 Premium。個人最低是 Cursor Pro+ 每月 60 美元；已有 Teams Standard 座位者，月費 40 美元／座位也含 Grok Bot。

| 方案 | 月費（美元） | 約合新台幣 |
| --- | --- | --- |
| Cursor Teams Standard | 40 元／座位 | 1,270 元／座位 |
| Cursor Pro+ | 60 元 | 1,910 元 |
| SuperGrok Plus | 100 元 | 3,190 元 |
| Cursor Teams Premium | 約 120 元／座位 | 約 3,820 元／座位 |
| Cursor Ultra | 200 元 | 6,370 元 |
| SuperGrok Heavy | 約 300 元 | 約 9,560 元 |

據《數位時代》實測，Grok 免費用戶安裝桌面版後可以選 Free Trial 方案，但必須提供信用卡，且 token 額度十分有限，足夠看懂介面，但不夠跑一週的實際工作。

由於每月 60 美元的門檻仍不算低，因此建議還在摸索 AI 能幫上什麼忙的人，還是先把 Claude 或 Codex 用熟再說，可以學得更多、花得更少；若你是已經同時付好幾個 AI 訂閱的重度使用者，用 Grok Bot 等於拿資料主權換便利（它之所以方便，就代表它在不問你權限的情況下，可以任意存取你的資料），算是值得一試。

## Grok Bot教學：4步驟建立第一個Bot

動手前先建立一個觀念，這是最容易被略過、卻最決定成敗的一點。sairahul1 把它壓成一句話：提示詞是一次請託，Bot 是一個職位。你要為它寫一份說明，是因為你打算讓它在你不在場的時候繼續動，權限的邊界今天不畫，明天就得每天盯著它。

### 第1步：下載桌面版，用同一組帳號登入

到 [x.ai/bot](https://x.ai/bot) 下載桌面版（macOS、Windows）或 iOS 版（需 iOS 18 或以上）。Linux 桌面、Android 與 iPad 在初發時不支援。務必用「持有你方案與用量的那一組 Cursor 帳號」登入，這是 [Cursor 官方說明](https://cursor.com/help/grok-bot/getting-started) 特別點名的一步，用錯帳號會看不到付費買到的額度。從下載到建好第一個 Bot，sairahul1 的紀錄是大約四分鐘。

### 第2步：在onboarding勾選每天使用的工具

第一次啟動時，它會先介紹 Bot、共享電腦與 routine 三個概念，然後問你每天用哪些工具。

畫面給你一整片選項，包括 Google Workspace、Slack、Notion、LinkedIn、GitHub、Salesforce、HubSpot、Figma、Canva、Zoom 等。照實勾選，之後的 Bot 不用你交代就知道該去哪裡動手。

> 圖一：Grok Bot 桌面版主畫面
> 
> ![Grok Bot 桌面版主畫面](https://terrylee-lang.github.io/bnext-visuals/2026/08-grok-bot-fig1.png)
> 
> 圖說：① 左側是已建立的 Bot 列表，名稱即職務，之後在側邊欄分辨誰負責什麼。② 左下角「外掛程式」用來連接 Notion、Gmail、Google Drive 等服務，屬帳戶層級，接一次全部 Bot 共用。圖中為點開後的外掛視窗。

### 第3步：建立第一個Bot，用職務名稱命名

建立時要填名稱、形狀、顏色與職稱。名稱請照「一個真人可以擔任的職位」來取，例如收件匣管理、內容企劃、業務開發、SEO、財務對帳。

名稱是你日後在側邊欄分辨誰負責什麼的依據，也是「幕僚長模式」判斷該把工作轉派給誰的依據。建議不要取成「小幫手一號」之類的籠統名稱，Bot 一多就全糊在一起。

例如《數位時代》編輯部這邊，大部分的任務都是稿件生成或是深度編輯，所以我的幕僚長命名邏輯很簡單，就是「名稱＋職稱」，然後重點是裡面的描述定義要寫清楚。

這邊要強調的是，因為每個人的工作領域不一樣，所以這邊無法針對每種職務類型來說明。如果你不清楚你的幕僚長要怎麼定義，你完全可以跟你的 Grok Bot 討論，然後再填寫。

> 圖二：幕僚總長的設定頁
> 
> ![幕僚總長的設定頁](https://terrylee-lang.github.io/bnext-visuals/2026/08-grok-bot-fig2.png)
> 
> 圖說：建立 Bot 要填名稱、職稱與描述。名稱用來在側邊欄辨識，職稱是這個職位叫什麼，描述才是它怎麼做事。圖中以編輯部幕僚長為例，交辦先判斷產稿、查核或蒐集資料，能轉給專責 Bot 就轉；進度以 Notion 稿單為準，只有衝突、對外發送和定稿才問人。

### 第4步：替Bot撰寫職務章程與權限界線

這步頗關鍵，尤其是先前沒有建立過完整工作流的同學，如果你對 Cloud 的 Claude.me 或是 Codex 的 Agent.me 沒有概念，那你就得像交接新人第一天一樣，把以下三件事寫清楚：

- **職責範圍** ：負責哪一塊、每天或每週該產出什麼、成果要放到哪裡。
- **執行標準** ：格式、長度、語氣，以及你不要的東西。
- **不能做的事情** ：例如花錢、對外發送、碰法律或合約字眼時一律停下來問。

第三項是重點。因為 Grok Bot 的優點就在於讓你敢讓 Bot 無人看管跑下去工作，因此你要事先定義它的權限在哪裡結束。 **什麼都要問的 Bot 太老實，什麼都不問的 Bot 太危險** ，所以你要先幫他把紅線給畫清楚。

以下章程是範例，讀者們完全可以依照自己的需求自己下去改。

```
你是我的內容企劃。

// 你負責什麼
每個工作日早上掃過我追蹤的 [10 個產業媒體] 與 [3 個 YouTube 頻道]，
挑出 5 則和 [AI 工具、企業導入] 有關的題目。
每則附上來源連結、一句話重點，以及為什麼我該關心。
整理成一份 Markdown 檔存到 [資料夾]，並在對話裡貼 5 行摘要給我。

// 好的產出長什麼樣
一則只寫一句重點，不要摘要全文。
五則之間主題不重複。
沒有把握的數字直接標「待查證」，不要自己補。

// 哪裡停下來
不要主動發任何訊息或貼文。
不要幫我回信。
任何要花錢的動作，先問我。
```

（要注意，職務章程寫了「每個工作日早上執行」，不等於已經建立排程。若要讓Grok Bot在指定時間自動開跑，還需要建立Routine，可參考： [Grok Bot排程怎麼用](https://www.bnext.com.tw/article/92295/grok-bot-routine-schedule-ai-news-briefing) ）。

如果看完上面你還是沒頭緒，該怎麼辦？

推薦你反過來讓 AI 訪問你：把一份「 [grill me](https://www.bnext.com.tw/article/91643/matt-pocock-ai-skills-workflow) 」技能檔丟進 Grok Bot，要它照著檔案不停追問，直到雙方對你的業務與這一季目標達成共識，再把訪談結果變成 Bot 的背景知識。你可以在每季換目標或要開新專案時跑一次。

《數位時代》建議優先建立以下六個角色，可以當起手範本：

> 1. 幕僚長（跨 Bot 協調，只有衝突、對外訊息與花錢才找你）
> 2. 研究員（每則結果都附來源、追蹤理由與下一位負責人）
> 3. 內容（只備稿，不自行發布）
> 4. 工程（每次交付都附測試結果或截圖）
> 5. 客戶（信件一律停在草稿，標明「Bot 草稿．尚未寄出．需人工審閱」）
> 6. 財務（寫入財務系統前都要你核准）

## Grok Bot幕僚長模式是什麼？多個Bot如何分工？

### 幕僚長與專責Bot如何配置？

一個 Bot 是助理，一群 Bot 才是團隊。而讓一群 Bot 不變成一團亂的做法，就是設一個「幕僚長」Bot 統籌，底下再配專責 Bot（舉例來說：假設你是總統，那你的幕僚長接近閣揆）。基本上， **你只跟幕僚長講話，它讀每個專責 Bot 的職責描述，判斷該給誰、把工作轉派出去，再把結果帶回來。**

因此若不知道該配哪些專責 Bot ，建議是先讓幕僚長把你的業務與目標問清楚，再問它該補哪些角色，它會根據現有名單指出哪些重複、哪些缺口值得補。

### 從零建立AI團隊的幕僚長提示詞

在你從零開局使用 Grok Bot 的時候，可以直接貼以下指令給幕僚長當開場：

```
先了解我這個人和我在做的事，能查到的資訊都查一遍。
接著看我已經建立的這些 Bot，它們是你的團隊。
告訴我：這個編制需要調整嗎？該補哪些 Bot？
管理這支 Bot 團隊最好的方式是什麼？
還有什麼能讓我的產出最大化？
```

### 如何讓多個Bot協作與交接？

Grok 跟其他 AI 工具不一樣的地方，在於 **可以把多個 Bot 放進同一個聊天室，它們會自己協調、彼此交接、分配負責人，只在需要判斷時把你拉進來。** 這種「一支 Bot 判斷另一支更適合、把工作交出去」的交接，是過去的 AI 工具沒有的。

因此對 Grok Bot 交辦時，要給目標而不是任務清單，因為清單等於你已經先把工作拆好了，但你不用這麼累，因為你已經有幕僚長了。實作上，人只待在幕僚長這一個視窗：在對話框打「@」就能點名不同專責 Bot，讓它們做各自的事，不用分別打開每一支的對話。

## Grok Bot實測：3個Bot約10分鐘完成找題與選題卡

據《數位時代》以 Cursor Pro+ 實測，我們要求幕僚長去找隔天可能要跟進報導的科技題，找完做成一張可以丟在群組的字卡。我只跟幕僚長講下面這一段提示，然後看它怎麼把工作分出去。

> 圖四：幕僚長視窗
> 
> ![幕僚長視窗](https://terrylee-lang.github.io/bnext-visuals/2026/08-grok-bot-fig4.png)
> 
> 圖說：左邊是幕僚長、主編、研究員。我只在幕僚長視窗講話，找題先給研究員，主編等清單回來再寫卡。

結果，幕僚長下令後，研究員先去負責翻過去 24 小時的新聞，視角如下：

> 圖五：研究員回報
> 
> ![研究員回報](https://terrylee-lang.github.io/bnext-visuals/2026/08-grok-bot-fig5.png)
> 
> 圖說：三則都有連結，還會標這則有多急、下一手該給誰。到這裡都還沒動筆寫稿。

研究員找出題目之後，再回報給幕僚長，幕僚長再把選題的工作交辦給主編。視角如下：

> 圖六：主編交回選題卡的對話
> 
> ![主編交回選題卡的對話](https://terrylee-lang.github.io/bnext-visuals/2026/08-grok-bot-fig6.png)
> 
> 圖說：主編把卡交回幕僚長，自己註明沒發、也沒寫進 Notion。

要先說的是，上述這一輪是模擬編輯交辦、看分工，不是把新聞做出來。但從過程中可以發現，找題、寫卡這種備稿的活，丟出去真的接得上。我只講一句，研究員幾分鐘交回三則，主編做成選題卡，來回大約 10 分鐘，中間也沒打斷。

## Teach a task、Routine與Plugins怎麼用？

前面把第一個團隊建起來就夠開工了。剩下幾個功能，讀者們可以斟酌參考使用。

一大亮點是 **Teach a task。** 畫面有這個按鈕，打開 Bot 的雲端電腦，把流程做一遍給它看（上限 10 分鐘），它會存成可重複用的技能。還沒出現就先寫進職務章程。挑每週都會做、步驟不太變的來教。

**Routine。** 技能要有開跑的理由：時間（例如每個工作日早上 7 點）或事件（新郵件、Slack、Git）。跑在雲端，電腦關掉也照跑。交辦完直接跟它說「以後每週跑一次」就行。

如果想實際設定排程，可以參考延伸教學 [〈Grok Bot排程怎麼用？3步驟設定Routine，每天早上8點自動整理AI新聞晨報〉](https://www.bnext.com.tw/article/92295/grok-bot-routine-schedule-ai-news-briefing) ，《數位時代》實測從建立Skill到設定Routine，讓Grok Bot每周一至周五自動搜尋、整理AI新聞。

> 圖七：右側邊欄介面
> 
> ![右側邊欄介面](https://terrylee-lang.github.io/bnext-visuals/2026/08-grok-bot-fig7.png)  
> 圖說：① 上面是這支 Bot 的雲端電腦，它在幹嘛你看得到。② 下面是例行任務，按「建立例行任務」就能排時間或事件。

另外是 **Plugins** 一定要先串好，包括 Notion、Slack、Gmail 這些接一次，就全部 Bot 共用。至於沒外掛的網站，它會自己開瀏覽器，碰到登入就把畫面交給你。要注意，帳號底下所有 Bot 共用同一台雲端電腦，因此就算拆很多支，也不等於資安隔離。

**記憶分三層。** 使用者層是你這個人（名字、時區、偏好），代理層是這支 Bot 自己的職務與紀錄，專案層是這個專案的慣例。全隊該知道的、只留在這支 Bot 的，講清楚就好。

多數跑不動，先完全退出再開一次。Mac 要從選單列選「退出」，不是只關視窗；Windows 從系統匣結束。

> 延伸閱讀： [SpaceXAI設計師公開Grok Bot工作流：2張截圖、一段語音就能改Figma](https://www.bnext.com.tw/article/92273/grok-bot-figma-screenshot-workflow)

## Grok Bot常見問題

### Grok Bot和一般Grok有什麼不同？

一般Grok主要用來對話、搜尋、分析檔案與生成內容；Grok Bot則能實際操作工具、網站及雲端電腦，持續完成多步驟任務。簡單來說，Grok負責回答問題，Grok Bot更接近能動手執行工作的AI同事。

### Grok Bot可以免費使用嗎？

符合資格的帳號可能可以取得Grok Bot試用額度，但它不是不限量的免費方案。官方說明指出，試用額度會依Bot執行的步驟與token用量扣除，並設有7天使用期限；執行大型任務可能很快用完額度。

### Grok Bot最低價格是多少？

個人用戶可透過每月60美元的Cursor Pro+使用Grok Bot；SuperGrok Plus、Cursor Ultra及部分團隊方案也包含使用資格。若以團隊席次計算，Cursor Teams Standard為每月40美元一席，但方案價格與用量限制仍應以官方最新公告為準。

### 電腦關機後，Grok Bot還會繼續工作嗎？

可以。Grok Bot的工作是在雲端電腦執行，因此關閉桌面程式、筆電或手機，不會自動中止已在背景執行的任務或Routine。不過，涉及登入、驗證碼或人工核准的步驟，仍可能暫停並等待使用者操作。

### Grok Bot支援哪些裝置？

Grok Bot目前支援macOS、Windows與iPhone，其中iPhone需要iOS 18或以上版本。初期尚未提供Linux桌面版、Android與iPad版本，實際支援狀況仍應以官方下載頁面為準。

### Grok Bot可以操作所有網站嗎？

Grok Bot可以透過瀏覽器操作許多沒有Plugins、API或MCP的網站，但並非所有網站都能順利執行。遇到登入驗證、CAPTCHA、自動化阻擋或付款等敏感操作時，Bot可能需要把畫面交回使用者確認。

### 使用Grok Bot有哪些資安風險？

Grok Bot可能接觸帳號登入狀態、檔案與企業系統，因此不宜直接授予不必要的權限。建立Bot時，應明確規定付款、對外寄信、發布內容、修改帳號與處理合約等動作必須先取得人工核准，並定期移除不再使用的檔案、Plugins與登入授權。

資料來源： [SpaceXAI 官方方案公告](https://x.ai/news/grok-bot-more-plans) 、 [Grok Bot 官方文件](https://docs.x.ai/grok-bot/faq) 、 [Grok Bot 雲端電腦與應用程式說明](https://docs.x.ai/grok-bot/computer-and-apps) 、 [Cursor 官方 Grok Bot 入門說明](https://cursor.com/help/grok-bot/getting-started) 、 [Grok Bot 產品頁](https://x.ai/bot) 、 [《數位時代》Grok Bot 發表報導](https://www.bnext.com.tw/article/91832/spacexai-grok-bot-ai-agent-team) 、 [sairahul1 教學貼文](https://x.com/sairahul1/status/2089995692874068433) 、 [milesdeutscher 終極指南](https://x.com/milesdeutscher/status/2089724781449052255) 、 [Nate Herk 九個 Grok Bot 技巧](https://www.youtube.com/watch?v=TMPUUyQC5aM) 、 [mattyp 使用心得](https://x.com/mattyp/status/2087252657589412119) 、 [Composio](https://composio.dev/) 、 [臺灣銀行牌告匯率](https://rate.bot.com.tw/xrt?Lang=zh-tw)

本文初稿為AI編撰，整理．編輯/ 李先泰