---
title: "OpenAI Codex 推出 Record and Replay：做一次給 AI 看，自動學會你的工作流程"
source: "https://www.koc.com.tw/archives/646598"
author:
  - "[[達小編]]"
published: 2026-06-20
created: 2026-06-23
description: "這句話精準描述了 Record and Replay 的定位：不是要你寫 prompt 教 AI 做事，而是直接「示範」一次，AI 自己學。"
tags:
  - "clippings"
status: processed
---
Tags:

4,710 次瀏覽

你有沒有遇過這種情況？某個流程（產品上架、客服處理…）想交由 AI 代勞，但對人類來說幾個反覆機械化的操作想讓 AI 自己跑通流程卻異常困難。OpenAI 想解決這個問題。2026 年 6 月 18 日，OpenAI 在 Codex 應用程式中正式推出「Record and Replay」功能，讓使用者直接在 Mac 上「做一次」給 AI 看，Codex 會自動將整個流程打包成可重複使用的 Skill（技能）。這不是傳統的巨集錄製或 RPA 腳本。Record and Replay 背後是 Computer Use 模型在觀察你的操作，理解每個步驟的意圖，然後生成一份結構化的、可編輯的技能文件。用 OpenAI 的話說：「Show Codex a workflow once. Reuse it as a skill.」  

## 什麼是 Record and Replay

OpenAI 官方直接點出此功能的核心價值：

「You can now show Codex how to do something, teach it your preferences, and turn what it learns into a skill you can use again.」（你現在可以示範給 Codex 看要怎麼做某件事、教它你的偏好，並把它學到的內容轉換成一項可以重複使用的技能。）

![OpenAI Codex 推出 Record and Replay：做一次給 AI 看，自動學會你的工作流程 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260620193034_0_d0f3dd.jpg "OpenAI Codex 推出 Record and Replay：做一次給 AI 看，自動學會你的工作流程 - 電腦王阿達")

這句話精準描述了 Record and Replay 的定位：不是要你寫 prompt 教 AI 做事，而是直接「示範」一次，AI 自己學。

### 實際操作範例：YouTube 影片上傳流程

影片中展示的是一個真實場景：OpenAI 團隊每次發布 YouTube 影片時，都要從發行試算表拉 metadata、找到對應的素材檔、在 YouTube Studio 填寫相同的欄位與設定。這些步驟每次都一樣，但手動操作繁瑣且容易出錯。

![](https://www.youtube.com/watch?v=ZK3JhU73W18)

操作流程如下：

- **啟動錄製** ：在 Codex 應用中開啟 Plugins，選擇「Record a skill」
- **開始示範** ：Codex 請求錄製權限後，使用者正常執行 YouTube 上傳流程
- **拉取 metadata** ：從發行試算表中取出標題和描述
- **上傳素材** ：加入縮圖和英文字幕
- **儲存設定** ：將影片設為非公開並儲存
- **停止錄製** ：從選單列或覆蓋層停止，或直接告訴 Codex「我做完了」

![OpenAI Codex 推出 Record and Replay：做一次給 AI 看，自動學會你的工作流程 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260620193629_0_3e589f.jpg "OpenAI Codex 推出 Record and Replay：做一次給 AI 看，自動學會你的工作流程 - 電腦王阿達")

錄製過程中，Codex 觀察的是操作的「意圖」而非純粹的像素軌跡。它理解你從試算表拉的是「標題」而不是「某個格子的文字」，這讓生成的技能具有泛化能力。

錄製結束後，Codex 分析錄影內容並自動生成一份 Skill。影片中展示 Codex 依照使用者的操作流程學會了：

- metadata 存放在哪裡（發行試算表的位置）
- 上傳素材包的資料夾結構
- 如何加字幕、儲存、驗證每次上傳

這份 Skill 不是黑箱，根據 OpenAI [開發者文件](https://developers.openai.com/codex/record-and-replay) ，生成的 Skill 遵循標準的 SKILL.md 格式，包含使用時機、所需輸入、執行步驟和驗證方式。使用者可以直接檢視和編輯。

### AI 自動化執行

影片後半段展示「Replay」的實際效果。使用者開啟一個全新的對話串，附上下一部影片的素材包，然後要求 Codex 處理。結果：

- Codex 自動將素材包對應到試算表中的正確列
- 填入 metadata
- 上傳縮圖和英文字幕
- 將影片設為非公開
- 驗證所有設定是否正確儲存

![OpenAI Codex 推出 Record and Replay：做一次給 AI 看，自動學會你的工作流程 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260620193738_0_9367b4.jpg "OpenAI Codex 推出 Record and Replay：做一次給 AI 看，自動學會你的工作流程 - 電腦王阿達")

全程不需要使用者手動介入。使用者只需要提供「這一次不同的值」，比如新的影片檔案、新的標題，其餘全部由 Codex 依 Skill 自動完成。

### 技術架構：Agent Skills 開放標準

Record and Replay 的底層是 OpenAI 採用的 Agent Skills 開放標準（agentskills.io）。一個 Skill 就是一個資料夾，裡面包含：

- **SKILL.md** ：必備，包含名稱、描述和指令
- **scripts/** ：可選，可執行腳本
- **references/** ：可選，參考文件
- **assets/** ：可選，模板和資源

Codex 使用「漸進式披露」（progressive disclosure）管理上下文：一開始只載入每個 Skill 的名稱、描述和檔案路徑（最多佔模型上下文視窗的 2%），只有在決定使用某個 Skill 時才載入完整的 SKILL.md 指令。這確保了即使安裝了大量 Skill，也不會擠爆上下文。

觸發方式有兩種： **明確調用** （在 prompt 中直接指定 Skill）和 **隱式調用** （Codex 根據任務自動匹配 Skill 的 description）。Record and Replay 產生的 Skill 走的就是隱式調用路線，Codex 會根據你提供的素材自動判斷該用哪個 Skill。

### 可用範圍與限制

目前 Record and Replay 有幾個限制：

- **僅限 macOS** ：Windows 和 Linux 暫不支援
- **地區排除** ：歐洲經濟區（EEA）、英國和瑞士暫時無法使用
- **需要啟用 Computer Use** ：若組織透過 requirements.toml 管理 Codex，將 computer\_use 設為 false 會同時關閉 Record and Replay
- **需要 Codex 應用程式** ：CLI 和 IDE 擴充套件目前不支援此功能

執行 Skill 時，Codex 可以透過 Computer Use（桌面操作）、Browser Use（瀏覽器操作）、已連接的外掛程式，或三者組合來完成任務。

### 與其他方案的比較

在自動化工作流程的領域，Record and Replay 並不是唯一的解法，但它的切入角度與眾不同：

- **傳統 RPA（如 UiPath、Automation Anywhere）** ：依賴固定的 UI 元素選擇器，介面一改就壞。Record and Replay 用 AI 理解操作意圖，對 UI 變化的容忍度更高
- **手寫 Prompt / Skill** ：需要使用者具備撰寫精確指令的能力。Record and Replay 把門檻降到「你會做就行」
- **Zapier / Make 等整合平台** ：擅長 API 之間的串接，但對需要操作桌面應用的流程無能為力。Record and Replay 剛好補上這個缺口

Record and Replay 的核心價值在於「讓不懂程式碼的人也能教 AI 做事」。過去你要自動化一個流程，得先把它拆解成機器看得懂的步驟；現在你只要自己做一遍，Codex 會幫你完成拆解和封裝。

### 對開發者和企業的意義

Record and Replay 生成的 Skill 可以在團隊間共享。也就是說，一個人錄製的流程可以變成整個部門的自動化資產。不會再有負責的人不在現場就沒人會使用操作特定功能的窘境。這對企業端的影響有幾個面向：

- **知識保留** ：員工離職時，流程知識不會跟著離開，因為它已經被封裝成 Skill（編按：有點可怕…）
- **一致性** ：團隊所有人執行同一個流程時，品質和步驟一致
- **擴展性** ：新進人員不需要重新學習每個內部工具的操作細節

總而言之 Record and Replay 改變了「教 AI 做事」的介面。過去是文字（prompt），現在是行為（demonstration）。這讓 AI Agent 的能力擴展不再受限於使用者的文字表達能力。

### Codex 的產品策略

從 Codex 的發展軌跡可以看出 OpenAI 的策略：先建立 Plugin 生態系（2026 年 3 月已支援超過 20 個第三方外掛，包含 Slack、Gmail、Notion、Salesforce），再推出角色專屬的 Plugin 套件（針對銷售、分析等職能），現在再加上 Record and Replay 讓使用者自己產生 Skill。

這是一條從「平台提供工具」到「使用者創造工具」的路徑。OpenAI 不只是在賣 AI 模型，而是在建一個讓 AI 學會人類工作流程的基礎設施。

### 結語

Record and Replay 目前還在初期階段，限制不少（macOS only、排除歐洲），但它代表的方向很清楚：AI Agent 的能力擴展不該依賴使用者會寫 prompt，而該依賴使用者會做事。當「示範」取代「描述」成為教 AI 的主要方式，自動化的門檻會大幅降低。

對一般使用者來說，這意味著你不需要成為 prompt engineer 也能讓 AI 幫你處理重複性工作。對企業來說，這意味著組織內部的流程知識可以被標準化、封裝、傳承。這才是 AI Agent 真正該做的事：不是替代人類思考，而是替代人類重複。

Tags:

Tags:

## ChatGPT 語音最大規模升級！OpenAI 籌備推出 GPT-Bidi-1 雙向語音模型

![達小編](https://www.koc.com.tw/wp-content/uploads/2025/03/20250303074017_0_33a53c-150x150.jpg) by

[2026 年 06 月 19 日](https://www.koc.com.tw/archives/646486)

in [最新科技新聞](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/3c%e6%96%b0%e5%93%81%e6%90%b6%e5%85%88%e5%a0%b1)

讀取中...

<iframe frameborder="0" src="https://0e1c5c020232af445b12f9b5880d7432.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="728" height="90" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>

據 X 平台知名 AI 開發者爆料，OpenAI 正在籌備推出代號為 GPT-Bidi-1 的新一代語音模型，這將是 ChatGPT 語音模式有史以來最大規模的升級。根據科技媒體 testingcatalog 的報導，GPT-Bidi-1 採用「雙向」（BiDi）架構，能夠同時聽和說，在對話過程中吸收使用者的打斷，並即時調整回應內容。這項升級被認為將徹底改變 ChatGPT 語音對話的流暢度與自然度，讓 AI 語音對話首次接近真人互動水準，也將使 ChatGPT 在語音助理市場中更具競爭力，與 Google 和 Apple 等巨頭正面對決。  
![ChatGPT 語音最大規模升級！OpenAI 籌備推出 GPT-Bidi-1 雙向語音模型 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260619000756_0_024d6c.jpg "ChatGPT 語音最大規模升級！OpenAI 籌備推出 GPT-Bidi-1 雙向語音模型 - 電腦王阿達")

## OpenAI 籌備推出 GPT-Bidi-1 雙向語音模型

這項消息由開發者 M1Astra 在 X 上率先曝光，貼文中寫道：「OpenAI 新語音模型 GPT-Bidi-1 即將推出，帶來『智慧的重大飛躍』：下一代語音、更自然的對話。」相關跡象已在網頁版和行動版 ChatGPT 中出現，暗示消費者端的正式 rollout 即將到來。開發者社群對此反應熱烈，許多人認為這是 ChatGPT 語音功能最重要的一次更新，將直接挑戰 Google Gemini Live 和 Apple Siri 的語音對話能力，為 AI 語音助理市場注入新變數。

![](https://x.com/i/status/2067017773528617041)

### 雙向架構：語音對話的重大突破

GPT-Bidi-1 的核心技術是「雙向」（BiDi）架構，這是 OpenAI 從 2026 年初開始研發的新一代語音處理方式。與現有語音模式最大的不同在於，BiDi 模型能夠同時處理聽覺輸入與語音輸出，而不是像現行系統那樣必須等使用者說完才能開始回應。這項技術最初由 The Information 在 2026 年初率先報導，當時 OpenAI 內部將其定位為語音技術的下一代核心架構，目標是讓 AI 語音對話達到與真人對話同等的流暢度與自然度，實現真正的雙向交流。  
![ChatGPT 語音最大規模升級！OpenAI 籌備推出 GPT-Bidi-1 雙向語音模型 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260619001123_0_f071f2.jpg "ChatGPT 語音最大規模升級！OpenAI 籌備推出 GPT-Bidi-1 雙向語音模型 - 電腦王阿達")

這意味著使用者可以在 ChatGPT 說話的過程中打斷它、修正方向，或是發出「嗯哼」這類確認訊號，模型都能即時感知並調整回應。相比之下，目前的 ChatGPT 語音模式一旦開始說話就會鎖定回應，使用者的中途打斷往往會讓對話卡住，需要重新開始。BiDi 架構讓對話更像真實的人類交流，而非機器的輪流發言，大幅降低了語音對話的僵硬感，讓整體使用者體驗更加直覺自然。

根據 OpenAI 內部消息，GPT-Bidi-1 上線後，使用者可以根據需求在雙向模式和現有的進階語音模式之間切換，並支援 High、Medium 以及 Instant 三種智慧等級，方便使用者依任務需求調整回應速度與深度。例如需要快速查詢天氣或時間時可選 Instant 模式獲得即時回應，進行複雜討論或頭腦風暴時則可切換到 High 模式獲得更深入、更具洞察力的回答。

### 語音技術的落後與追趕

目前，OpenAI 的文字模型已經快速進化到 GPT-5.5 世代，但語音功能仍停留在較舊的音訊技術路線，導致口語對話能力明顯落後於文字表現。GPT-Bidi-1 的推出正是為了彌補這個差距，讓 ChatGPT 的語音對話能力追上文字水準。這也意味著 ChatGPT 的語音模式將首次採用與文字模型同等的智慧等級，不再只是一個「會說話的文字模型」。

這個差距對 OpenAI 來說至關重要，該公司正押注語音（而非文字）將成為人類與 AI 互動的主要方式。OpenAI 與前 Apple 設計長 Jony Ive 合作開發的 AI 智慧音箱（預計售價 200 至 300 美元，約新台幣 6,500 至 9,750 元，最快 2027 年 2 月推出），正需要 GPT-Bidi-1 這樣的雙向語音引擎作為核心。沒有螢幕的裝置上，自然對話能力不是加分項，而是整個使用介面。這也解釋了為何 OpenAI 如此重視語音技術的升級，不惜投入大量資源追趕 Google 和 Apple 在語音領域的領先地位，確保未來硬體產品擁有足夠的競爭力。

> [OpenAI 與 Jony Ive 攜手打造劃時代無螢幕 AI 裝置，預計 2026 年亮相](https://www.koc.com.tw/archives/600000)

### 開發歷程與上市時程

OpenAI 的 BiDi 語音模型最初目標是在 2026 年第一季推出，但據傳原型在連續對話數分鐘後會出現異常聲音等問題，導致時程推遲到第二季或更晚。如今 GPT-Bidi-1 的相關準備跡象已在 ChatGPT 網頁版和行動版中出現，顯示消費者端的發布已近在咫尺，不過最終命名可能還會調整。外界預期 GPT-Bidi-1 可能在未來幾週內正式亮相，屆時將與 Google 的 Gemini Live 以及 Apple 的 Siri 等競品正面交鋒，掀起新一波 AI 語音大戰，也將重新定義 AI 語音對話的標準。

GPT-Bidi-1 也被認為將大幅提升 ChatGPT 在客服場景中的應用能力。BiDi 架構能夠在說話的同時呼叫外部工具和應用程式，這對於需要即時查詢資料庫或執行操作的客服場景來說是關鍵需求。OpenAI 一直將客服自動化視為語音技術的重要應用場景，GPT-Bidi-1 的雙向對話能力讓 AI 客服可以更自然地處理客戶的提問與需求，同時在背景中查詢訂單資訊或執行退款操作，讓服務流程更加順暢。

### 總結

GPT-Bidi-1 的推出將是 ChatGPT 語音模式自推出以來最大的一次升級。從「輪流說話」到「同時聽與說」的轉變，看似只是技術細節的調整，但實際上將徹底改變人與 AI 語音互動的體驗。對於正在打造無螢幕 AI 硬體的 OpenAI 來說，這項技術更是不可或缺的關鍵拼圖。隨著 GPT-Bidi-1 發布在即，ChatGPT 的語音對話能力可望迎來一次真正的質變，讓使用者與 AI 的對話更加自然流暢，也為 OpenAI 的語音生態系與未來硬體產品奠定更加穩固的基礎。

Tags:

No Result

View All Result

- [最新科技新聞](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/3c%e6%96%b0%e5%93%81%e6%90%b6%e5%85%88%e5%a0%b1)
- [最新科技產品報導](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac)
- [好用App介紹](https://www.koc.com.tw/archives/category/android%e8%bb%9f%e9%ab%94%e5%88%86%e4%ba%ab%e8%88%87%e5%88%b7%e6%a9%9f%e6%95%99%e5%ad%b8)
- [電腦應用與其他教學](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/%e9%9b%bb%e8%85%a6%e6%87%89%e7%94%a8%e8%88%87%e5%85%b6%e4%bb%96%e6%95%99%e5%ad%b8)
- [旅遊與美食](https://www.koc.com.tw/archives/category/%e7%94%9f%e6%b4%bb%e8%88%87%e6%97%85%e9%81%8a/%e6%97%85%e9%81%8a%e8%88%87%e7%be%8e%e9%a3%9f)
- [阿達自製工具](https://www.koc.com.tw/speedtest)
- [電腦王阿達團隊](https://www.koc.com.tw/contact)

No Result

View All Result

- [最新科技新聞](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/3c%e6%96%b0%e5%93%81%e6%90%b6%e5%85%88%e5%a0%b1)
- [最新科技產品報導](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac)
- [好用App介紹](https://www.koc.com.tw/archives/category/android%e8%bb%9f%e9%ab%94%e5%88%86%e4%ba%ab%e8%88%87%e5%88%b7%e6%a9%9f%e6%95%99%e5%ad%b8)
- [電腦應用與其他教學](https://www.koc.com.tw/archives/category/3c%e6%96%b0%e8%81%9e%e8%88%87%e7%94%a2%e5%93%81%e8%a9%95%e6%b8%ac/%e9%9b%bb%e8%85%a6%e6%87%89%e7%94%a8%e8%88%87%e5%85%b6%e4%bb%96%e6%95%99%e5%ad%b8)
- [旅遊與美食](https://www.koc.com.tw/archives/category/%e7%94%9f%e6%b4%bb%e8%88%87%e6%97%85%e9%81%8a/%e6%97%85%e9%81%8a%e8%88%87%e7%be%8e%e9%a3%9f)
- [阿達自製工具](https://www.koc.com.tw/speedtest)
- [電腦王阿達團隊](https://www.koc.com.tw/contact)

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-3801907256397861&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782173929&amp;plat=1%3A16777216%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fwww.koc.com.tw%2Farchives%2F646598&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782178576028&amp;bpp=1&amp;bdt=277&amp;idt=411&amp;shv=r20260622&amp;mjsv=m202606150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D7eb900325fb958cb%3AT%3D1764142964%3ART%3D1782178510%3AS%3DALNI_MbVZE1liWGMfPz_VpdO7wIdzmxt6g&amp;gpic=UID%3D000011bd03f7b5c2%3AT%3D1764142964%3ART%3D1782178510%3AS%3DALNI_MbBGuuzozlOZeftw8VbLcX8blwv4w&amp;eo_id_str=ID%3D5dcf144d7d675b57%3AT%3D1781055859%3ART%3D1782178510%3AS%3DAA-AfjYBnGeiCZMK9bNGnZmpq_-G&amp;nras=1&amp;correlator=2598139187218&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;oid=2&amp;pvsid=5179164546513929&amp;tmod=1182815595&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fwww.koc.com.tw%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;plas=247x636_l%7C247x636_r&amp;bz=0&amp;pgls=CAEaBTYuOC41~CAEQBRoGMy4zMS41&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=438" title="Advertisement" aria-label="Advertisement"></iframe>![Buy Me a Coffee](https://cdn.buymeacoffee.com/widget/assets/coffee%20cup.svg)