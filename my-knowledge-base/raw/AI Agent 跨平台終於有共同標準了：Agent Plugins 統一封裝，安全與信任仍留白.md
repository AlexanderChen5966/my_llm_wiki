---
title: "AI Agent 跨平台終於有共同標準了：Agent Plugins 統一封裝，安全與信任仍留白"
source: "https://techorange.com/2026/08/07/ai-agent-plugins/"
author:
  - "[[廖紹伶]]"
published: 2026-08-07
created: 2026-08-10
description: "這週 AI 產業新動作，發生在少有人注目的底層管線。就在 GPT-5 屆滿一週年前夕，OpenAI 與競爭對手亞馬遜（AWS）、微軟、Cursor 與 Vercel 於美國時間 8/6 共同發布開放標準 Agent Plugins 1.0，Google 隨後以核心維護者身分加入。這是一套開放標準，讓單一的代理擴充套件能跨競爭產品運作。"
tags:
  - "clippings"
status: processed
---
![](https://techorange.com/app/uploads/2026/08/da1a766e72177ed9-scaled.jpg)

這週 AI 產業新動作，發生在少有人注目的底層管線。就在 GPT-5 屆滿一週年前夕，OpenAI 與競爭對手亞馬遜（AWS）、微軟、Cursor 與 Vercel 於美國時間 8/6 共同發布開放標準 Agent Plugins 1.0，Google 隨後以核心維護者身分加入。這是一套開放標準，讓單一的代理擴充套件能跨競爭產品運作。

## 為什麼 AI Agent 需要這個共同標準？

Agent Plugins 要解決的痛點，不在 AI 代理能力本身。Agent Plugins 技術指導委員會撰文指出，AI 代理已變得高度可擴充。Agent Skills 能賦予它們可重複使用的指令、腳本與參考素材；模型脈絡協定（Model Context Protocol，MCP）伺服器則能將它們連接到資料庫、瀏覽器、原始碼控管、雲端平台，以及其他即時系統。但分享這些能力，仍存在一個封裝上的問題。而 Agent Plugins 1.0 以一套開放、不偏向任何廠商的規格來彌補這道缺口，為那些可攜式元件提供單一、可預期的套件格式。

Agent Plugins 1.0 主打訴求是「一次建置，隨處運行」（build once, run anywhere），將開發者早已在使用的兩樣東西包裹在一起。第一是模型脈絡協定（Model Context Protocol）伺服器，第二是 Agent Skills。一個外掛程式，其實就是一個根目錄放著一支小小 plugin.json 檔案的資料夾。

## 刻意做窄，信任與安全留給各平台

1.0 版本只定義兩件事：外掛怎麼打包、怎麼被發現，其餘一概不碰。官方把交由各平台自理的部分明確分成三塊：發行（市集、安裝、更新）、執行體驗（權限、審核流程、沙箱、介面），以及信任（發布者身分、來源證明、簽章、組織政策）。

這個留白正是隱憂所在。判斷一個外掛能不能安全執行，仍是每個客戶端各自的責任；而《TNW》指出，今年稍早就曾有偽造的 Agent Skills 通過資安掃描。

目前發布即支援的客戶端包括 ChatGPT／Codex、Cursor、GitHub Copilot、Kiro 與 VS Code，AWS 與 Google 也已各自在旗下產品導入支援。換言之，同一套 AI Agent 外掛能跨 ChatGPT、GitHub Copilot、Cursor 等產品使用，不必為每個平台各做一份。

## 有人說早該如此，也有人嫌太單薄

對於這套標準，開發者反應兩極。據《TNW》報導，開發者 Angie Jones 樂見其成，因為她一直想要一套能在不同工具間帶著走的方式；打造 SST 開發工具的 Dax Raad 則明確反對，稱這是「一套單薄的標準」，認為真正有用的部分終究會落回各客戶端的專屬擴充。

《TNW》也點出更深一層的競爭張力：統一格式一方面讓小型開發者得以一次觸及所有主流 Agent，是開放生態的正面情境；另一方面，標準會獎勵那些使用者本來就多的客戶端，反而可能鞏固既有龍頭的優勢。

工具連接與打包這一層，如今總算有了共同語言；但市集怎麼經營、外掛能不能被信任、安全又該由誰把關，這些被 1.0 刻意留白的環節，角力才正要開始。

> 【推薦閱讀】
> 
> ◆ [Meta 首款程式代理 Muse Code 上線！改按 token 計費，最低價要拿你的程式碼交換](https://techorange.com/2026/08/06/meta-muse-code-muse-spark-1-2/)
> 
> ◆ [MCP 史上最大更新來了：AI Agent 為何終於更適合大規模部署？](https://techorange.com/2026/07/29/mcp-update-ai-agent/)
> 
> ◆ [人人都會用 AI 後，真正的績效差距從哪來？德州大學、KPMG 實驗點出關鍵](https://techorange.com/2026/07/28/why-some-workers-turn-ai-into-results-and-others-dont/)

＊本文開放合作夥伴轉載，資料來源： [《The Next Web》](https://thenextweb.com/news/openai-agent-plugins-open-standard-skills-mcp) 、 [Agent Plugins](https://agent-plugins.org/) 、 [AWS](https://aws.amazon.com/tw/blogs/opensource/aws-supports-agent-plugins-an-open-standard-for-portable-agent-extensions/) 、 [Google](https://developers.googleblog.com/agent-plugins-package-your-skills-tools-and-more/) 、 [Vercel](https://vercel.com/changelog/introducing-agent-plugins-1-0-0) 、 [Agentic AI Foundation](https://aaif.io/blog/from-skills-and-tools-to-portable-agent-plugins) ，首圖來源： [Google](https://developers.googleblog.com/agent-plugins-package-your-skills-tools-and-more/)

![](https://techorange.com/app/uploads/2026/07/647e9210990aec2f-scaled.jpg)

**一句話生成整部影片，AI Agent 正在搶走導演的工作？AI 不只聽懂你，它開始「自己拍片」了**

想像一下：你只是說了一句「我想做一部關於西施犬獨自在家的短片」，然後去泡了杯咖啡回來，結果畫面、配音、背景音樂全部都好了，這就是 AI Agent（代理人）現在正在做的事。

## 提示工程之後，下一步是什麼？

在目前的生成式 AI 領域，我們正從「提示工程」轉向「創意導向」，所謂提示工程（Prompt Engineering），就是你用文字告訴 AI 你想要什麼，例如「畫一隻坐在草地上的柴犬」，AI 根據這段描述生成結果，這已經很強了，但你還是在手動操作每一個步驟。

不過現在正邁向 AI Agent 的工作流，AI 不只會回應你的單一指令，而是像一位助理一樣，自動規劃並執行一連串複雜的任務，不需要再管理每一個素材，而是由一個 AI Agent 擔任你的編導，管理複雜的流程。

對於缺乏技術背景，或者缺乏靈感的人來說，現在能更容易將一個簡單的想法轉化為完整的作品。這意味著，如果你缺乏靈感，代理人可以構思故事、生成角色並從零開始構建整個敘事，降低創作初期的門檻。

## AI Agent 怎麼讓角色從頭到尾長得一樣？

那麼，這個「編導」背後是什麼？

ADK（Agent Development Kit，代理人開發套件） 是讓你可以快速組裝模型作為 Agent 的大腦、串接工具作為手腳，甚至快速將多個 Agent 組成一個多代理團隊，如此一來就能快速操控 AI Agent，你可以把它想像成一個遙控器，複雜的電路都藏在裡面，只需要按按鈕，就能用網頁介面就能跟代理人互動。

ADK 最厲害的地方，是它讓角色在整部影片裡「認得出來」，很多人以為這單靠代理人的「記憶」，但實際上更有趣：它用的是多模態推理（Multimodal Reasoning）。簡單說，就是它能同時看懂文字、圖像、聲音，而不是只懂其中一種。第一張生成的圖像，會成為整個專案的主視覺，後續每一個畫面、每一段影片，都會跟這張圖對齊，確保同個角色在不同場景裡長得一樣。

而且你不需要寫任何程式碼，如果想調整音量，就直接說「把音樂調大聲一點」，AI Agent 自己會去處理後面的技術細節。

如果不夠好，它會選擇重做，而不是硬著頭皮往下走，這也是它跟傳統自動化腳本最大的差別，傳統腳本是照順序執行，AI Agent 的每一步都是一個有意識的判斷。

## AI Agent 怎麼跟工具說上話？

AI Agent 再聰明，也需要有人幫它接線，MCP（Model Context Protocol，模型上下文協定）伺服器就是扮演這個角色。你可以把它想像成一個翻譯官，AI Agent 說「我需要一段音樂」，MCP 負責把這句話翻譯成 Lyria 聽得懂的格式，拿到結果再帶回來，AI Agent 不需要自己去學每一個工具的語言，這些都交給 MCP 處理。

AI Agent 可以用 MCP 存取三種媒體類型：

- 圖像生成： 視覺化故事並設定參考基準。
- 文字轉語音： 生成旁白與細膩的配音。
- 音樂生成： 創作大氣的背景配樂。

不過可能有人會想問：代理人直接呼叫 Veo 或 Lyria 不就好了，為什麼中間還需要 MCP 這一層？

原因在於「標準化」，每個工具都有自己的格式、參數、錯誤回傳方式，如果 AI Agent 要直接跟每個工具溝通，就必須各自學一套語言，維護成本很高。MCP 的作用是統一這套溝通協定，AI Agent 只說一種語言，MCP 負責轉譯給各個工具，這樣當你要替換或新增工具時，只需要更新 MCP 的設定，AI Agent 本身完全不需要動。

## 創意工具箱：Nano Banana、Veo 與 Lyria

AI Agent 協調著一套專業工具，每一項工具都像虛擬製作工作室中的部門主管。

![](https://techorange.com/app/uploads/2026/07/a99027df72306ac2.png)

## 一句話，到一部成品影片，中間發生了什麼？

還記得開頭那杯咖啡嗎？這就是它泡好之前發生的事，為了製作像《西施犬 Lulu 獨自在家》這樣的故事，AI Agent 會自動跑完一套模擬專業電影工作室的流程：

① 先確定「這部片長什麼樣子」

AI Agent 用主流圖像生成模型）(例如：Nano Banana 2) 生成角色的第一張參考圖，Lulu 長什麼樣、場景是什麼風格，全部在這一步確定，這張圖會成為整個專案的視覺錨點，後續所有畫面都要對齊它。

② 把靜態圖變成會動的畫面

參考圖確立之後，AI Agent 調用 Veo 生成影片片段，Veo 負責確保 Lulu 在每一個場景裡都長得一樣，同一隻狗、同一個蝴蝶結、同一種毛色，不會跑版。

③ 聲音和情緒進來

AI Agent 自動撰寫旁白腳本，再透過 Gemini TTS 配音，這個配音不是機械感的念稿，它會根據場景情緒調整語氣，緊張的場景說話急促，溫柔的場景放慢節奏，同時，Lyria 根據當下畫面的氛圍生成背景音樂，兩者同步進行。

④ 全部縫在一起

最後，AI Agent 用 FFmpeg 把影片、旁白、音樂合併成一個完整的檔案，FFmpeg 是業界廣泛使用的免費影音處理工具，專門負責把不同格式的影像和聲音合併、轉檔、輸出，你幾乎不需要打開任何剪輯軟體。

## 這麼多步驟同時跑，代理人不會亂掉嗎？

### 有兩個關鍵設計讓整個流程維持品質。

第一個是 **模組化技能（Skills）** ，與其給 AI Agent 一份又長又複雜的指令清單，不如把它拆成幾個各司其職的「技能角色」，配音導演、圖像藝術家、場景規劃師。每個角色只專注在自己的領域，不互相干擾，這樣做的好處是代理人不會因為要同時記住太多事情而出錯，而且每個技能都可以帶著非常專門的知識上場，例如「配音導演」這個技能，內建了完整的音訊標籤規則和聲線特徵判斷，這種通常一個指令做不到的。

第二個是 **LLM 作為裁判（LLM as a Judge）** ，LLM 是 Large Language Model（大型語言模型）的縮寫，也就是 ChatGPT、Gemini 這類 AI 的底層技術。這裡的做法是讓一個完全獨立的 AI 模型，專門負責審查其他 AI 的輸出，例如確認 Lulu 的蝴蝶結在每個場景都待在同一個位置，或者旁白的時間長度有沒有超出影片。

不過有人可能會想問：兩個 AI 會不會互相說好話、有盲點？

關鍵在於評估員 Agent 和生成 Agent 是完全隔離的，它們沒有共享任何上下文或目標。評估員不知道生成端「想要」什麼結果，它只有一份死板的評分清單：「角色特徵是否與參考圖一致？」「音訊長度是否符合時間軸？」。

當然，LLM 判斷也不是萬能的，在某些主觀的美學上，它仍然可能出現偏差，這也是為什麼在高品質要求的製作流程裡，人工複審仍然是最後一道必要的關卡。

幾年前，我們還需要想像「一個人做出一部完整影片」這個概念，但現在，你不需要工程學位，不需要剪輯經驗，甚至不需要知道 FFmpeg 是什麼，只需要一個想法，然後去泡杯咖啡，AI 就能替你完成了，那個一直放在心裡、還沒開始做的故事，或許現在是時候開始了。

(本文訊息由 CloudMile 萬里雲提供，內文與標題經 TechOrange 修訂後刊登。新聞稿 / 產品訊息提供，可寄至：pr@fusionmedium.com，經編輯檯審核並評估合宜性後再行刊登。圖片來源：Shutterstock。）

![](https://sslcode.adgeek.com.tw/public/images/popad_close_button.png)