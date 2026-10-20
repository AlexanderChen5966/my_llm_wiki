---
title: "GPT-6 三個模型怎麼選？Sol、Luna 價格砍半上線，跟 Astra 的分工與設計師用法"
source: "https://rar.design/posts/gpt-6-sol-luna-guide"
author:
  - "[[設計師 Riven]]"
published: 2026-09-23
created: 2026-09-24
description: "GPT-6 Sol 與 Luna 是 OpenAI 於 2026 年 9 月 22 日推出的新模型，與旗艦 Astra 用相近方法訓練。Sol 每百萬 token $2／$10，主攻複雜 coding 與專業工作；Luna $0.10／$0.50，負責大量日常任務，都比 GPT-5.6 促銷價便宜一..."
tags:
  - "clippings"
status: processed
---
OpenAI 把價目表畫成三張像素角色卡，看懂 GPT-6 宇宙的新成員

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80,format=webp/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年9月23日 上午 4:05

AI 設計

AI MODELS · 2026

![GPT-6 Sol 與 GPT-6 Luna 官方發表圖：左上角是發光的太陽，右下角是新月，中間寫著 GPT-6 Sol and Luna](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/7eece0cb-724c-4432-9082-94455355ad8f/3322d052-2c57-453b-8a96-323e2c2568a1.jpg)

GPT-6 Sol 與 GPT-6 Luna 官方發表圖：左上角是發光的太陽，右下角是新月，中間寫著 GPT-6 Sol and Luna

▲ OpenAI 的官方發表圖，太陽是 Sol，新月是 Luna。圖片來源：OpenAI

QUICK ANSWER

GPT-6 Sol 與 Luna 是 OpenAI 於 2026 年 9 月 22 日推出的新模型，用跟旗艦 Astra 相近的方法訓練。Sol 每百萬 token 輸入 2 美元、輸出 10 美元，主攻複雜 coding 與專業工作；Luna 輸入 0.10、輸出 0.50 美元，負責大量日常任務。兩者 API 價格都比 GPT-5.6 促銷價便宜一半。

台灣時間今天凌晨，OpenAI 在 IG 發了一組不太像 AI 公司會發的貼文：三張像素風的角色卡。一隻銀河做成的小人，一隻頭頂冒火的小戰士，一隻月亮頭的太空人。每張卡最底下標著一行價格，右下角寫著 READY。

這是 GPT-6 家族的全員陣容。月初上場的旗艦 Astra 之外，這次多了兩個新成員：GPT-6 Sol 和 GPT-6 Luna。

設計師的目光很容易先被角色吸走，但這組貼文真正的新聞，藏在卡片最底下那行數字。這篇拆成四件事：Sol 和 Luna 各是什麼、價格砍半省在哪裡、官方跑分該怎麼讀，還有設計師手上的工作，該派哪一隻上場。

CONTENTS

01

GPT-6 宇宙多了兩個成員

02

真正的新聞是那一行價格

03

三個模型怎麼分工

04

官方跑分怎麼讀

05

為什麼把模型畫成角色卡

06

設計師的派工表

07

上場前要知道的限制

08

選角比排名重要

CHAPTER 01 · WHAT IS IT

## GPT-6 宇宙多了兩個成員

GPT-6 Sol 和 GPT-6 Luna 是 OpenAI 美國時間 9 月 22 日發表的兩個模型。官方的說法是，它們跟 [GPT-6 Astra](https://rar.design/posts/gpt-6-astra-complete-guide) 用相近的方法訓練，把 Astra 在專業工作、事實準確度、coding、電腦操作與對齊上的進步，帶進更快、更便宜的模型裡。

名字聽起來眼熟很正常。今年夏天的 GPT-5.6 就分成 Sol、Terra、Luna 三個等級（ [當時的三模型解析在這](https://rar.design/posts/chatgpt-5-6-sol-terra-luna-guide) ），那時 Sol 是旗艦。到了 GPT-6，旗艦換成 Astra，Sol 往下一格，負責「用更低的成本做複雜 coding 與專業工作」，Luna 還是最輕、最便宜的那一顆。這次的公告沒有提到 Terra。

![GPT-6 Astra 像素角色卡：身上繞著星環的銀河小人，卡片文字 Our most intelligent model for the best results，價格 $10.00 input、$50.00 output](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/6d9e4ada-96df-4e3e-ba12-569c01a545f5/cb94c9e5-3331-4f50-a4b1-07b41f9e13c3.jpg)

GPT-6 Astra 像素角色卡：身上繞著星環的銀河小人，卡片文字 Our most intelligent model for the best results，價格 $10.00 input、$50.00 output

![GPT-6 Sol 像素角色卡：頭頂冒火、腰上有顆太陽寶石的小戰士，卡片文字 Complex coding and professional work at a lower cost，價格 $2.00 input、$10.00 output](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/47b7193f-5f4e-4408-ba0d-1699ea4bdc52/4828b724-edb5-489c-9a4a-b5a888a72798.jpg)

GPT-6 Sol 像素角色卡：頭頂冒火、腰上有顆太陽寶石的小戰士，卡片文字 Complex coding and professional work at a lower cost，價格 $2.00 input、$10.00 output

![GPT-6 Luna 像素角色卡：月牙頭、穿太空衣比 YA 的角色，卡片文字 Fast and efficient everyday work at scale，價格 $0.10 input、$0.50 output](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/fe8f1a1a-7bb3-4d2b-a016-989751b832f1/4fc95f42-2d9c-48c3-bc3f-38ba94ca521a.jpg)

GPT-6 Luna 像素角色卡：月牙頭、穿太空衣比 YA 的角色，卡片文字 Fast and efficient everyday work at scale，價格 $0.10 input、$0.50 output

▲ OpenAI 官方 IG 的三張角色卡：Astra 是銀河，Sol 是火焰，Luna 是穿太空衣的月亮，每張都寫著一句定位和 API 價格。原貼文是三段像素動畫，這裡是擷取畫面。圖片來源：OpenAI Instagram

API 的模型名稱是 gpt-6-sol 和 gpt-6-luna。兩者的 context window 都是 1,050,000 tokens，單次最多輸出 128,000 tokens，推理強度從 none 到 max 共六檔，預設是 medium。

CHAPTER 02 · THE PRICE

## 真正的新聞是那一行價格

角色卡最底下那行字，是這次發表的重點。OpenAI 把 Sol 和 Luna 的 API 價格，直接砍到 GPT-5.6 促銷價的一半。

API PRICE · 每百萬 token 輸入 / 輸出（美元）

GPT-5.6 SOL

$4 / $20

→

GPT-6 SOL

$2 / $10

GPT-5.6 LUNA

$0.20 / $1.20

→

GPT-6 LUNA

$0.10 / $0.50

GPT-5.6 為促銷價。資料來源：OpenAI 官方發表文

細算的話，Luna 的輸出價從 1.20 降到 0.50，降幅接近六成。

錢省在哪裡？OpenAI 把原因歸給兩件事：快取和推論效率。GPT-6 的 prompt caching 預設命中率更高，重複使用的內容從快取讀取時只收一折；開發者也可以在對話中途調整推理強度、開關工具，前面存好的快取不會因此失效。GitHub 的數據是，過去幾個月這些改進讓需要重新處理的 prompt token 比例降了一半以上。

翻成設計師的日常：用 Codex 或 API 跟 AI 來回改同一個網站時，前面累積的脈絡會被快取起來，之後每一輪讀取那一段只付一折。改得越多輪，省得越多。

CHAPTER 03 · THE LINEUP

## 三個模型怎麼分工

OpenAI 給三個模型的定位很清楚。Astra 是整個家族最強的一顆，要最好的結果、不想妥協時用它；Sol 為複雜 coding 與 agent 工作流打造；Luna 是最有效率的一顆，適合目標明確、數量又大的工作。

SPEC

Astra

Sol

Luna

定位

最強，要最好的結果

複雜 coding、專業工作

大量、目標明確的任務

API 價格

$10 / $50

$2 / $10

$0.10 / $0.50

快取讀取

$1.00

$0.20

$0.01

Context window

105 萬 tokens

105 萬 tokens

105 萬 tokens

推理強度

low～max

none～max

none～max

免費方案

—

—

桌面版可用

▲ 價格單位為美元，每百萬 token 輸入 / 輸出。資料來源：OpenAI 官方文件與發表文

表格看下來，Sol 跟 Luna 的差距比名字給人的感覺小很多。Luna 的價格是 Sol 的二十分之一，但在官方的軟體工程測驗 DeepSWE 上，Luna 開到最高推理強度拿到 66.6%，Sol 是 68.8%，只差 2.2 個百分點。

另一組數字更直接。在電腦操作測驗 OSWorld 2.0 上，最高推理強度的 Luna 已經超過中等強度的 GPT-5.6 Sol，成本只要十分之一。兩個多月前的旗艦開中等強度，在這項測驗上已經被最便宜的那隻追過。

CHAPTER 04 · BENCHMARKS

## 官方跑分怎麼讀

這次 OpenAI 的跑分有個明顯的習慣：每個分數旁邊，都附上做完一個任務要花多少錢。它要你同時看兩個數字，分數和帳單。

DEEPSWE v1.1 · 真實程式庫裡的長時程工程任務

Claude Fable 5（xhigh）

69.9%

GPT-6 Sol（max）

68.8%

每個任務的成本比 Fable 5 低約 80%

GPT-6 Luna（max）

66.6%

表現與 Opus 5、Fable 5 的 medium 檔相當，每任務成本低 93%～96%

資料來源：OpenAI 官方發表文，對手分數取自公開報告

AUTOMATIONBENCH

跨 App 的商務流程

33.2%

Sol（xhigh）；Claude Opus 5（max）26.9%

每任務成本約為 Opus 5 的 9%

AGENTS' LAST EXAM

長時程專業工作

56.4%

Sol（max）；高於 Claude Opus 5 的最佳成績

每任務成本低 60%

OSWORLD 2.0

電腦操作

60.5%

Sol（xhigh）；Claude Opus 5（medium）60.3%

每任務成本低約 80%

FACTUALITY

OpenAI 內部事實評測

錯誤少一半

Sol 對比 GPT-5.6 Sol

Luna 開高推理強度可追平 GPT-5.6 Sol，成本約百分之一

讀這些數字時，有三件事要放在心上。第一，全部是 OpenAI 自己的測試，對手的成績取自公開報告。第二，比較的推理強度不一定對等，像 OSWorld 是 Sol 的 xhigh 對上 Opus 5 的 medium。第三，對照組是 Claude Opus 5 與 Fable 5／5.1，而就在同一天稍早，Anthropic 推出了新的 Opus 5.5。

### 跟 Claude 比，要看哪一代

Opus 5.5 比 OpenAI 早大約一個半小時上線，API 價格是輸入 4 美元、輸出 20 美元，Anthropic 說它在多數工作上跟 [Fable 5.1](https://rar.design/posts/claude-fable-5-1-guide) 同級。也就是說，OpenAI 這份對照表發出來的當下，就少了最新的那個對手。

單看標價，Sol 是 Opus 5.5 的一半。不過兩家現在都在強調「每個任務的成本」，同一件事各花多少 token，最後的帳單才算數。真要比，拿你最常做的三個任務各跑一次，帳單跟成品一起看。

CHAPTER 05 · THE DESIGN

## 為什麼把模型畫成角色卡

回到那三張卡。站在設計師的角度，這是這次發表最值得多看兩眼的地方。

AI 模型的命名，這幾年一直在跟「數字加後綴」搏鬥：mini、nano、pro、turbo 一路疊上去，連重度使用者都得查表。GPT-5.6 那一代，OpenAI 改用 Sol、Terra、Luna 這些天體名當等級；到了 GPT-6，官方乾脆把整個家族叫做「GPT-6 宇宙」。

有了宇宙，角色就好做了。Astra 是星，造型是一團繞著星環的銀河；Sol 是太陽，頭頂整團火；Luna 是月亮，乾脆讓它穿上太空衣。三個名字各對到一個一眼就懂的造型，再用像素風統一畫風，連右下角 READY 的字色都跟著角色走：Astra 偏紫、Sol 偏橘、Luna 是白。

CARD ANATOMY · 一張角色卡的五個部件

01 · 家族標籤

左上角的 GPT-6

三張卡都一樣，一眼看出是同一代

02 · 名字

Astra、Sol、Luna

全卡最大的字級，取代過去的數字加後綴

03 · 角色

名字變成的造型

星是銀河、太陽是火焰、月亮穿上太空衣

04 · 一句定位

這隻擅長什麼

一行講完，不用讀規格表

05 · 價格與狀態

價格＋READY

每百萬 token 的輸入輸出價，READY 字色跟著角色走

“

一張價目表，被畫成了一個選角畫面。

這個轉換很聰明。價目表讓人想問「哪一個最強」，答案永遠是最貴的那個；選角畫面讓人想問「這一關派誰」，答案會跟著關卡改變。後面這個問題，才是 OpenAI 希望你開始問的。

這招設計師可以直接帶走。下次做方案頁、做產品分級，先別急著把功能排成一長串打勾表，先問三件事：每個方案能不能有一個記得住的名字？名字能不能對到一個一眼就懂的形象？能不能一句話講完它適合誰？三件事對齊，使用者就不用查表。

CHAPTER 06 · IN PRACTICE

## 設計師的派工表

角色選好，接下來是派工。先看 OpenAI 自己放出來的兩個例子。

### 例一：Figma 給 Astra 的登月任務

OpenAI 的 YouTube 頻道昨天發了一支跟 Figma 合作的影片。Figma 團隊給 Astra 一個「登月任務」：替一架新的個人飛行器，設計整套飛行控制介面。

Astra 先把問題拆過一輪：誰會用、要完成哪些工作、駕駛和乘客的需求怎麼取捨、哪些關鍵畫面狀態不能漏；接著透過 Figma MCP Server 接上真實的設計系統，做出一份符合品牌規範的原型。

![](https://www.youtube.com/watch?v=rRm3Vnb7UPY)

▲ OpenAI 官方影片〈Figma Gave GPT-6 Astra a Moonshot〉，英文

影片的主角是 Astra，設計師該記下來的是那條路徑：先想清楚系統，再接進設計系統出圖。Sol 也能走同一條路，而且更適合接後半段，想清楚之後大量地做、反覆地改。Codex 接 Figma MCP 的完整設定，可以看 [Codex × Figma 雙向工作流](https://rar.design/posts/openai-codex-figma-designer-guide) 。

### 例二：一句話改掉網站排版

OpenAI 的發表文裡還放了一段對話：使用者請 AI 把網站改成 Bento Box 風格，右上角再加一個切換頁面的滑桿。GPT-5.6 Sol 和 GPT-6 Sol 都做完了，差別在回報的方式。GPT-6 Sol 交代自己在桌機和窄版手機畫面都檢查過，連瀏覽器的上一頁都測了；舊版則多講了一段內建生圖工具用的 prompt。

這個差別對設計師很實際。你要的是一份交代清楚的交件說明：改了什麼、測了哪些斷點、哪裡還沒測。OpenAI 說 Sol 和 Luna 都繼承了 Astra 的溝通方式：更清楚、更少術語、答案稍短，內容卻沒有縮水。

DISPATCH · 設計師的派工流程

STEP 01

Astra

想清楚

需求拆解、資訊架構、設計系統規劃、提案論述

↓

STEP 02

Sol

做出來

切版、改版、接 Figma、多輪迭代，日常的主力

↓

STEP 03

Luna

批次整理

alt text、介面字串翻譯、圖層命名、訪談摘要

我的派工建議是：日常先派 Sol；遇到要從零想清楚的題目，才請出 Astra；一旦工作變成重複又明確的批次，就交給 Luna。

CHAPTER 07 · LIMITS

## 上場前要知道的限制

還不在一般聊天裡

Sol 和 Luna 今天起開放在 [ChatGPT Work](https://rar.design/posts/chatgpt-work-gpt-5-6-guide) 與 Codex，對象是 Plus、Pro、Business、Enterprise 和 Edu 用戶，一般的 ChatGPT 聊天介面還沒有。免費和 Go 方案只能在桌面版 App 用 Luna。OpenAI 會在一天內分批開放，看不到新模型就晚點再試。

超過 272K 會變貴

API 有個價格門檻：單次輸入超過 272K tokens，整筆請求的輸入與快取費用變成 2 倍、輸出變成 1.5 倍。整包設計文件或長逐字稿丟進去之前，先想想要不要拆開。

跑分是自家考卷

文中的數字都來自 OpenAI 的測試，對手成績引用公開報告，推理強度也不一定對等。拿來判斷方向可以，拿來做採購決定還不夠。

Terra 沒有出現

GPT-5.6 的三個等級裡，這次只有 Sol 和 Luna 換上 GPT-6，公告沒有提到 Terra。已經在 API 用 GPT-5.6 Terra 的團隊，可以拿 GPT-6 Sol 和 Luna 各跑一輪比較。

CHAPTER 08 · TAKEAWAYS

## 選角比排名重要

TAKEAWAYS · 一分鐘懶人包

→

GPT-6 家族目前三隻：Astra 最強，Sol 做複雜 coding 與專業工作，Luna 做大量、目標明確的日常任務。

→

Sol 每百萬 token $2／$10，Luna $0.10／$0.50，都比 GPT-5.6 促銷價便宜一半，快取讀取再打一折。

→

Luna 的價格是 Sol 的二十分之一，DeepSWE 只差 2.2 個百分點，別只拿它做雜事。

→

官方跑分全是自家測試，對照組還沒有同一天上線的 Claude Opus 5.5，自己的任務跑一輪最準。

→

三張像素角色卡把價目表變成選角畫面，做方案頁的設計師可以直接借用這個思路。

這兩年，大家都在追「哪個模型最強」。排行榜幾乎每週換一次冠軍，追到最後，記得住的只剩一個分數。

OpenAI 用三張像素卡換了一個問法。最強的那隻一直都在，只是大部分的關卡，需要的是剛好的那一隻。接下來設計師的基本功，是知道每一關該派誰上場。

RELATED READS · 延伸閱讀[Codex 為什麼突然紅了？500 萬週活、併入 ChatGPT 桌面版與 7 月最新更新全解析](https://rar.design/posts/codex-why-popular-2026)

[

Sol 和 Luna 今天就能在 Codex 用，先搞懂 Codex 是什麼

→](https://rar.design/posts/codex-why-popular-2026)[

不會 Blender 也能建 3D？GPT-6 Astra 建模實戰：從一句 prompt 到可編輯的完整模型

旗艦 Astra 的實戰：一句 prompt 在 Blender 建出完整模型

→](https://rar.design/posts/gpt-6-astra-3d-blender-guide)[

AI 怎麼直接畫進 Figma？Claude Code 寫入 Figma 的設定、規則檔與元件庫整理

影片裡那條 Figma MCP 路徑，元件庫要整理到什麼程度

→](https://rar.design/posts/figma-mcp-write-to-canvas-setup-guide)[

Figma Weave 工具接上 MCP 是什麼？在 Claude、ChatGPT 一句話跑完 AI 創作管線

ChatGPT 接上 Figma MCP 之後，還能直接呼叫 Weave 工具

→](https://rar.design/posts/figma-weave-tools-mcp-guide)[

Adobe for ChatGPT 是什麼？70+ 個 Adobe 工具進駐 ChatGPT 完整解析

ChatGPT Work 與 Codex 裡，70 多個 Adobe 工具怎麼叫

→](https://rar.design/posts/adobe-for-chatgpt-70-tools-guide)[

Claude Opus 5 是什麼？跑分、Effort 旋鈕與設計師的實際用法

OpenAI 跑分圖裡的主要對照組，effort 檔位怎麼選

→](https://rar.design/posts/claude-opus-5-guide)[

只會答選擇題的 AI 有什麼用？Jev 模型白話解析：速度、價格、弱點與設計師該懂的事

另一種省錢思路：只做判斷題、回應不到半秒的模型

→](https://rar.design/posts/jev-typesafe-system-one-model-guide)![AI 覺醒設計應用攻略](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/63e96ac9-97ce-4dfc-be50-154f731f1cc9/e5ca45f0-206a-478f-af1a-96960be47d6a.jpg)

AI 覺醒設計應用攻略

[

SUBSCRIPTION

AI 覺醒設計應用攻略

AI 工具每週都在更新，我幫你過濾雜訊，只留對設計師真正有用的應用。

了解訂閱 →](https://rar.design/courses/aiwaken)

FAQ

## 常見問題

### Q：GPT-6 Sol 和 GPT-6 Luna 是什麼？

GPT-6 Sol 和 GPT-6 Luna 是 OpenAI 在 2026 年 9 月 22 日推出的兩個 GPT-6 模型，用跟旗艦 GPT-6 Astra 相近的方法訓練。Sol 主攻複雜 coding 與專業工作，Luna 是最有效率的輕量款，適合目標明確、數量大的日常任務。

### Q：GPT-6 Sol 和 Luna 多少錢？

API 以每百萬 token 計價：Sol 輸入 2 美元、輸出 10 美元；Luna 輸入 0.10 美元、輸出 0.50 美元，都比 GPT-5.6 促銷價便宜 50%，快取讀取打一折。單次輸入超過 272K tokens 時，輸入與快取費用變 2 倍、輸出變 1.5 倍。

### Q：Sol、Luna 跟 Astra 差在哪？該用哪一個？

Astra 是 GPT-6 家族最強的模型，API 價格為輸入 10 美元、輸出 50 美元。Sol 的價格是它的五分之一，適合日常的 coding、切版與專業工作；Luna 再便宜二十倍，適合批次、重複又明確的任務。多數工作可以先用 Sol，要從零想清楚的題目再交給 Astra。

### Q：免費版 ChatGPT 能用 GPT-6 Sol 或 Luna 嗎？

免費和 Go 方案可以在 ChatGPT 桌面版 App 使用 GPT-6 Luna。Sol 與 Luna 開放給 Plus、Pro、Business、Enterprise 和 Edu 用戶，在 ChatGPT Work 與 Codex 使用，一般聊天介面目前還沒有。

### Q：GPT-6 Sol 比 Claude 好嗎？

OpenAI 的官方數據裡，Sol 在多項測驗接近或超過 Claude Opus 5、Fable 5，每個任務的成本也低很多。不過這些是 OpenAI 自己的測試，對照組不含同一天推出的 Claude Opus 5.5（API 價格輸入 4 美元、輸出 20 美元）。最準的比較方式，是拿自己常做的任務各跑一次。

### Q：OpenAI 為什麼把模型畫成像素角色？

OpenAI 把 GPT-6 家族稱為「GPT-6 宇宙」，三個模型名稱各對應一個天體：Astra 是星、Sol 是太陽、Luna 是月亮。官方 IG 把它們畫成像素風角色卡，搭配一句定位和價格，效果就像遊戲的選角畫面，讓人從「哪個最強」改問「這件事派誰」。