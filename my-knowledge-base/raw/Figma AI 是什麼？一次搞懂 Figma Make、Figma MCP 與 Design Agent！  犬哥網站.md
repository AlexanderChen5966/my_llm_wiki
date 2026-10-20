---
title: "Figma AI 是什麼？一次搞懂 Figma Make、Figma MCP 與 Design Agent！ | 犬哥網站"
source: "https://frankknow.com/figma-ai/#figma-mcp"
author:
  - "[[犬哥網站 - Audrey]]"
published: 2026-05-29
created: 2026-07-20
description: "Figma AI 是 Figma 推出的一系列 AI 功能統稱，涵蓋從畫布內的 AI 輔助，到用一句話生成可互動原型的 Figma Make、讓 AI 開發工具直接讀懂設計稿的 Figma MCP，以及最新推出、可以在畫布上直接動手操作的 Design Agent。這篇文章讓你一次了解它們能做什麼、又該怎麼用：）"
tags:
  - "clippings"
status: processed
---
==[**Figma AI**](https://www.figma.com/ai/) 是 Figma 推出的一系列 AI 功能統稱== ，涵蓋從畫布內的 AI 輔助，到用一句話生成可互動原型的 Figma Make、讓 AI 開發工具直接讀懂設計稿的 Figma MCP，以及最新推出、可以在畫布上直接動手操作的 Design Agent。

這次 AI 化不是在 Figma 裡加幾個按鈕那麼簡單，而是直接 ==串連了「 **設計 → 原型 → 程式碼** 」的整條流程== ，讓設計師、工程師、PM 都能以不同的方式增加工作效率。

如果還不知道 Figma，可以先閱讀《 [Figma 是什麼？](https://frankknow.com/introduce-figma/) 》；若是對 Figma 的功能還不熟悉，則可以先閱讀《 [【 Figma 教學】完整功能分享！](https://frankknow.com/figma-tutorial/) 》，對於 AI 新功能會更有概念哦～

如果你已經聽過 Figma Make 或 Figma MCP 的名字，但還不太確定它們到底能做什麼、又該怎麼用，這篇文章會讓你一次搞清楚：）

## Figma 的 AI 時代已經來了

Figma 近期密集推出的 AI 功能，已經不只是讓你在設計稿裡自動填圖或快速改色那麼基本，而是開始串連發想、設計到開發的整個流程。

### Figma AI 功能分兩個層次

Figma AI 的功能可以分成兩個層次，深淺不同、使用方式也不一樣：

**第一層： ==內建畫布 AI 輔助功能==**

直接整合在 Figma 設計介面裡，不需要額外設定，開啟設計檔就能用。

包含日常的圖片編輯、First Draft 設計初稿生成、圖層自動命名、AI 文案翻譯等基礎輔助工具，以及 2026 年推出，可以用對話指令直接在畫布上執行設計任務的 **Design Agent** 。

**第二層： ==進階 AI 工具==**

需要另外進入、獨立入口或完成設定才能使用，包含 **Figma Make（Prompt 生成互動原型與可運行程式碼）** 和 **Figma MCP（把 Figma 設計稿接入 AI 開發工具）** 。

---

## Figma AI 內建功能：開啟就能用的畫布 AI 輔助

只要你有 Figma 帳號，開啟設計檔就能直接使用。它們分散在畫布的不同地方，針對設計流程中最常見的重複性工作做自動化輔助。

![Figma AI 的內建功能，只要你有 Figma 帳號，開啟設計檔就能直接使用](https://frankknow.com/wp-content/uploads/2026/05/Built-in-Figma-AI-features.jpg)

Figma AI 的內建功能，只要你有 Figma 帳號，開啟設計檔就能直接使用

### Design Agent：AI 直接在畫布上工作

==[**Figma Design Agent**](https://www.figma.com/blog/the-figma-agent-is-here/) 是一個「 在 Figma 畫布裡 」的 AI Agent== ，是 2026 年 5 月正式推出的新功能。

Design Agent 解決的就是 ==在已有設計稿的情況下，讓 AI 直接幫你在畫布上執行修改== ，而不是自己一個一個手動調，例如你說「 把這個 Card 改成深色模式 」，它就直接幫你改。

Figma 推出時也特別強調，設計師不該在「 速度 」和「 精準度 」之間做選擇，Design Agent 就是要讓兩者同時成立！

**Design Agent 能做什麼？**

- **生成設計初稿（原 First Draft ）**

==在空白畫布上輸入文字指令，AI 就能快速生成基礎的 UI 線框稿與設計初稿== ，輸出的是可繼續在 Figma 裡編輯的圖層，跳過從零發想的時間。

> First Draft 自 2026 年 5 月起，已整合進 Design Agent，不再是獨立的 First Draft 按鈕。

- **用指令修改設計：** 根據文字指令在畫布上新增、修改、重新排列元素不用自己一個一個拉元件，直接說你要什麼。
- **==以現有設計為基礎快速疊代== ：** 在既有設計稿上繼續發展，而不是每次都從零開始。
- **==理解並套用設計系統規範== ：** 操作時自動參照你的設計系統，套用正確的元件與樣式，用正確的 Component 和 [Token](https://frankknow.com/ai-token/) 執行。

**Figma Design Agent 怎麼用？**

這個功能目前屬於 Limited Beta（限量測試） 階段，官方採分批開放的方式，逐步將功能推送給 Professional、Organization 和 Enterprise 的付費方案用戶。

由於 Design Agent 直接內建在 Figma 畫布裡，不需要另外安裝或切換工具，有兩個進入方式：

- **==畫布即時 Prompt== ：** 在畫布上點選任何圖層後，直接開始輸入指令，從該圖層的設計出發。
- **==左側導覽列的 Agents 圖示== ：** 點擊左側的 Agent 圖示，開啟持續性的對話介面，適合需要來回調整的較長工作流程。
![Design Agent 內建在 Figma 畫布裡，點選任何圖層後，可直接開始輸入指令](https://frankknow.com/wp-content/uploads/2026/05/Figma-AI-Assistant.gif)

Design Agent 內建在 Figma 畫布裡，點選任何圖層後，可直接開始輸入指令

實際使用流程如下：

**Step 1：** 在 Figma 設計檔裡，點選你要操作的圖層或 Frame。

**Step 2：** 輸入自然語言指令，例如：「 用現有的設計系統元件，在這個 Frame 裡生成一個 Pricing 區塊 」或「 對所有 Card 元件套用 hover 狀態 」。

**Step 3：** Agent 開始在畫布上執行。

> 不需要等 Agent 跑完，可以同時對其他圖層下新的指令，多個 Prompt 可以並行進行。執行的同時，你也可以繼續在畫布上做自己的修改，兩者互不干擾。

**Step 4：** 查看 Agent 生成的結果，滿意就直接繼續，不滿意可追加指令調整，或切換到另一個 Agent 生成的版本比較。

### AI 圖片編輯：去背、去物件、延伸、向量化

Figma AI 提供了幾個常用的圖片處理功能，讓你不需要切換到 Photoshop 就能完成基礎的圖片編輯：

- **==Remove Background（一鍵去背）== ：** 選取圖片，AI 自動辨識主體並去除背景。
- **==Erase Object（去除物件）== ：** 用套索圈選圖片裡的雜物或浮水印，AI 自動填補背景，不留痕跡。
- **==Expand Image（延伸圖片）== ：** 向外擴展圖片邊緣，適合需要把方形圖調整成橫幅比例的情境。
- **==Vectorize（向量化）== ：** 把點陣圖（如手繪草圖、PNG/JPG）一鍵轉成可在 Figma 裡編輯的向量圖。

這幾個功能對設計師來說，可以 ==減少在不同工具之間來回切換的時間== ，讓素材處理在 Figma 裡就能完成。

### 圖層自動命名與整理

做設計做到一半回頭看，圖層全是「 Text Link List 」這類無意義的名稱，要找某個元素得一個一個展開確認。

Figma AI 的自動命名功能，可以一鍵 ==根據圖層的視覺內容，自動賦予有意義的名稱== ，整理檔案結構不再是收尾時最痛苦的事。

> 使用這個功能需要 Figma 付費方案，並具備檔案的編輯權限。

![Figma AI 的自動命名功能](https://frankknow.com/wp-content/uploads/2026/05/Figma-AI-Rename-Layout.jpg)

Figma AI 的自動命名功能

**Step 1：** 在畫布或左側圖層面板裡，選取你想重新命名的圖層（可以一次框選多個）。

**Step 2：** 對選取的圖層按右鍵，選擇 **Rename Layers** ；也可以開啟畫布下方 **Actions** 快速指令列，選擇或直接輸入 **「 Rename layers 」** 。

**Step 3：** Figma AI 自動根據圖層的內容、位置，以及與其他圖層的關係來判斷命名，直接套用，不需要確認。

還有幾個注意事項：

- **==已手動命名過的圖層不會被覆蓋== ：** Figma AI 只會重新命名仍使用預設名稱的圖層，你自己改過的名字不會被動到。
- **==隱藏或鎖定的圖層會被跳過== ：** 如果有不想被重新命名的圖層，直接鎖定或隱藏就好。
- **==支援批次跨 Frame 同步命名== ：** 如果同一個未命名圖層出現在多個頂層 Frame 裡，Figma 會一次把所有相符的圖層都重新命名，避免 Smart Animate 等 Prototype 設定因為命名不一致而失效。

### AI 文案撰寫與翻譯

![Figma AI 可以直接針對文案調整語氣、精簡字數、發想替代文案，以及將文字翻譯成多種語系](https://frankknow.com/wp-content/uploads/2026/05/Figma-AI-Rewrite-Translation.jpg)

Figma AI 可以直接針對文案調整語氣、精簡字數、發想替代文案，以及將文字翻譯成多種語系

選取設計稿裡的文字之後，Figma AI 可以直接 ==針對文案調整語氣、精簡字數、發想替代文案，以及將文字翻譯成多種語系== 。

對需要製作多語言版本設計稿的團隊來說，這個功能省去了手動複製貼上翻譯結果的麻煩，直接在設計稿裡完成。

### 視覺搜尋

在大型設計系統裡找某個元件，有時候你記得它「 長什麼樣子 」，卻想不起來叫什麼名字。

Figma 的視覺搜尋功能讓你 ==用圖片或選取區域，在專案與設計系統中搜尋外觀相似的元件與資源== ，比用關鍵字找更直觀。

視覺搜尋有三種使用方式，依照你手邊有什麼資料來選擇：

**方式一： ==Find More like 用畫布上的圖層搜尋==**

![Find More like 用畫布上的圖層搜尋](https://frankknow.com/wp-content/uploads/2026/05/Figma-AI-Find-More-like.jpg)

Find More like 用畫布上的圖層搜尋

**Step 1：** 在畫布上選取你想找相似元素的圖層。

**Step 2：** 點擊上方工具列的 **Actions** ，選擇 **Find more like** 。

**Step 3：** 搜尋結果會出現，用 Figma 分頁找你團隊檔案裡的設計，或用 **Community** 分頁搜尋 Figma 社群的公開資源。

**Step 4：** 找到喜歡的直接點擊插入畫布，不用離開當前檔案。

**方式二： ==Search with image or selection 上傳圖片搜尋==**

![Search with image or selection 上傳圖片搜尋](https://frankknow.com/wp-content/uploads/2026/05/Figma-AI-Search-with-Image-or-Selection.jpg)

Search with image or selection 上傳圖片搜尋

**Step 1：** 點擊工具列的 **Actions** ，選擇 **Search with image or selection** 。

**Step 2：** 點擊畫布中的任何圖層，或點擊右方 **Upload** 從電腦上傳任何圖片。

**Step 3：** Figma AI 自動搜尋視覺上相似的設計與元件。

**Step 4：** 從結果中找到目標，點擊右下角 **Insert** 直接插入畫布。

**方式三： ==用文字描述搜尋==**

![直接用文字描述搜尋](https://frankknow.com/wp-content/uploads/2026/05/Figma-AI-Search-in-Community.jpg)

*直接用文字描述搜尋*

**Step 1：** 點擊工具列的 **Actions** ，在搜尋欄輸入關鍵字或描述（例如「black button」、「navigation bar with logo on left」）。

**Step 2：** 點選下方跳出的 **Search XX** ‘ **s team / Community** 選項（或直接按下 Enter 鍵）。

**Step 3：** 進入結果頁面後，可在頂部切換 **「 XX’s team 」** 或 **「 Community 」** 分頁標籤，來縮小搜尋範圍。

**Step 4：** 畫面上會出現帶有 AI 標籤 的搜尋結果，在下方的視覺預覽中找到符合特徵的元件，點擊右下角 **Insert** 即可直接拉進畫布。

---

## Figma Make：Prompt 生成可互動原型

Figma Make 是其中最讓設計師和非工程師都眼睛一亮的一個，因為它解決了一件花時間的事：讓你不用寫程式，就能做出真正可以互動的原型。

### Figma Make 是什麼？

==[**Figma Make**](https://www.figma.com/make/) 是一個用自然語言 Prompt 直接生成互動原型和可運行程式碼的工具== ，支援中英文輸入。

![Figma Make 只需要 Prompt 就能生成可互動的原型](https://frankknow.com/wp-content/uploads/2026/05/Figma-Make.jpg)

Figma Make 只需要 Prompt 就能生成可互動的原型

Figma 官方定位是「 Prompt to code anything you can imagine 」，重點在「 code 」。 ==輸入一段描述，AI 就幫你把整個東西做出來，是 **真正可運行的 HTML / CSS / React 程式碼結構**== ，不是靜態設計稿。

Figma 另外有一個獨立產品 Figma Sites（目前 Beta），專門負責把設計稿發布為完整的響應式網站。不過兩者定位不同， ==**Make** 的輸出是可運行的互動原型， **Sites** 負責的是正式上線的網頁發布== 。

### 用 Figma Make 可以做什麼？

Figma Make 特別適合這幾種使用情境：

- **==Onboarding Flow== ：** 快速做出一個新用戶引導流程，讓團隊或客戶直接操作體驗。
- **==Data Dashboard== ：** 生成一個有圖表、有資料卡的儀表板原型。
- **==互動式介面== ：** 有表單、有頁面切換邏輯的完整 UI。
- **==工具型 App== ：** 例如計算機、待辦清單、簡單的資料查詢介面。

對非工程師的人來說，Figma Make 特別友善。

像是 PM 想驗證一個新功能的邏輯、設計師想快速呈現互動概念、創業者想做一個 Demo 給投資人看，Figma Make 都能在幾分鐘內給你一個可以展示、可以點擊的東西。

### Figma Make 怎麼用？

1. 前往 [figma.com/make](http://figma.com/make) 。
![在 Figma Make 中輸入一段描述，AI 就幫你把整個東西做出來](https://frankknow.com/wp-content/uploads/2026/05/Figma-Make-Example.jpg)

在 Figma Make 中輸入一段描述，AI 就幫你把整個東西做出來

2. 在 Prompt 欄輸入你想做的東西，例如：「 幫我做一個訂單追蹤 Dashboard，左側有導航列，右側顯示最近 7 筆訂單狀態 」。
![在 Figma Make 中用對話微調設計細節，在畫布上直接預覽效果](https://frankknow.com/wp-content/uploads/2026/05/Figma-Make-Edit-Design.jpg)

在 Figma Make 中用對話微調設計細節，在畫布上直接預覽效果

3. AI 自動生成可互動的原型，在畫布上直接預覽效果。
4. 用追加 Prompt 繼續調整細節，例如「 把主色改成深藍色 」、「 在右上角加一個通知鈴鐺 」。
![在 Figma Make 中管理權限與分享設計](https://frankknow.com/wp-content/uploads/2026/05/Figma-Make-Share.jpg)

在 Figma Make 中管理權限與分享設計

5. 完成後，點擊右上角「 Share 」複製分享連結，讓團隊或客戶直接在瀏覽器裡操作互動原型。

==Prompt 寫得越具體，生成的結果越貼近你的想像。== 建議第一次先把功能、版面配置、使用情境都說清楚，不要只寫「 做一個 App 」。

### 網站也可用 Figma 設計，正式架站交給 WordPress

Figma Make 非常適合用來快速驗證設計方向和功能邏輯，但它的定位終究是「 原型工具 」，而不是正式網站的終點。

如果你需要一個可以長期維護、SEO 友善、品牌形象完整的網站，就用 [WordPress](https://frankknow.com/what-is-wordpress/) 來架設，它在靈活性、擴充性和 SEO 基礎上是目前最成熟的選擇！

犬哥提供的「 [**WordPress 客製化網頁設計**](https://frankknow.com/web-design-service/) 」服務， ==從設計到上線都有人協助把關，兼顧品牌視覺、使用者體驗與行銷效益== ，讓我們協助您進行更全面的規劃與執行。

![犬哥網站網設設計案例——「 杏子中醫診所 」
跳脫傳統診所框架，溫暖質感設計強化品牌信任感](https://frankknow.com/wp-content/uploads/2026/05/Web-Design-Showcase-of-xingzitcm.jpg)

犬哥網站網頁設計案例——「 杏子中醫診所 」 跳脫傳統診所框架，溫暖質感設計強化品牌信任感

以犬哥協助設計的 **「** [**杏子中醫診所**](https://frankknow.com/portfolio/xingzitcm/) **」** 網站為例，我們融合傳統與現代風格，提供溫暖確跳脫傳統診所的制式設計，營造家的溫馨感與歷史質感，讓訪客能快速瀏覽服務與醫師背景資訊。

犬哥網站的設計特色：

- **==多元網站類型==**
	- **形象網站：** 展現品牌風格，強化專業形象。
		- **部落格／內容型網站：** 方便長期經營內容行銷。
		- **型錄網站：** 適合需要展示大量產品或服務的企業。
		- **電商網站：** 整合購物車與金流系統，創造銷售機會。
- **==專屬客製化設計==**
	- 依品牌調性設計頁面風格，避免落入制式化模版。
		- 強調視覺與內容並重，確保網站能吸引並留住訪客。
- **==技術與效能優化==**
	- **RWD 響應式設計：** 在手機、平板、桌機上皆有良好瀏覽體驗。
		- **SEO 架構優化：** 打好基礎，提升網站被搜尋引擎收錄的效率。
		- **網站速度優化：** 縮短載入時間，降低訪客流失率。
- **==完整配套服務==**
	- 專業主機配置，確保網站穩定與安全。
		- 維運監控，避免突發狀況影響營運。
		- 教育訓練，讓客戶能輕鬆上手，後續自行管理網站。

如果你也需要一個正式的 WordPress 網站，歡迎聯絡犬哥網站，讓我們為你量身規劃，從品牌策略出發，整合設計美感、使用者體驗與行銷導向，為你的網站打造一個有價值、可長期經營的平台：）

[**免費諮詢犬哥網站，立即前往！**](https://frankknow.com/contact/)

---

## Figma MCP：讓 AI 開發工具讀懂你的設計稿

Figma Make 解決的是「 非工程師快速產出原型 」的問題，Figma MCP 面對的則是 ==工程師在用 AI 工具寫程式時，要怎麼 **讓 AI 直接參照設計稿** ，而不是靠截圖或口頭描述== 。

### MCP 是什麼？

**MCP（Model Context Protocol）** 是由 Anthropic 提出的一套 ==**「 讓 AI 工具跟外部資料源對話的標準協議 」** ，讓不同的 AI 工具都能用同一種方式，去讀取、存取外部的資料或服務== 。

[**Figma MCP Server**](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Figma-MCP-server) 就是建立在這個協議上的橋梁， ==讓 Cursor、VS Code、Claude Code 等 AI 開發工具，能夠直接讀取你的 Figma 設計稿內容== ，而不是只能靠工程師口述或截圖說明。

### Figma MCP 能幫你做哪些事？

![Figma MCP 可以幫你做什麼？](https://frankknow.com/wp-content/uploads/2026/05/How-Figma-MCP-Help.jpg)

Figma MCP 可以幫你做什麼？

*Figma MCP 可以幫你做什麼？*

- **==從設計稿直接生成程式碼== ：** 在 AI 工具裡指定 Figma 的 Frame，AI 自動產出對應的前端代碼。沒有設計規範約束時，AI 生出的 UI 常常跟品牌風格對不上；接了 MCP 之後，AI 是看著你的設計稿在寫，不是自己猜。
- **==讀取設計變數與元件== ：** 把 Design Token、元件規範、Code Connect 傳給 AI，讓 AI 以你的設計系統為依據生成程式碼，而不是自己發明一套。
- **==讓 AI Agent 直接寫入 Figma 畫布== ：** 透過文字指令，讓 AI 在 Figma 裡新增或修改設計元素，AI 可以反向操作你的設計稿（需 Remote Server）。
- **==把網頁擷取成設計圖層== ：** 將 localhost 或線上頁面的 UI，轉成可在 Figma 裡編輯的設計層，快速同步設計與開發的現況。

### Figma MCP 支援哪些工具？

目前 Figma MCP 支援的主流 AI 開發工具，幾乎涵蓋現在開發者最常用的 AI Coding 環境，以下列出幾個較常見的工具：

- **==Claude Code== ：** 支援串接 Figma Design、Figma Make 和 FigJam，可直接在命令列環境中根據設計稿生成與修改程式碼。
- **==Claude== ：** 支援串接 Figma Design、Figma Make 和 FigJam，適合在對話介面中搭配 Figma 設計內容進行開發討論與程式碼生成。
- **==Cursor== ：** 支援串接 Figma Design、Figma Make 和 FigJam，在編輯器裡直接讀取設計稿並生成對應程式碼，是目前最多開發者使用的。
- **==VS Code== ：** 透過 GitHub Copilot 整合 Figma MCP，支援串接 Figma Design 和 FigJam。
- **==GitHub Copilot CLI== ：** 支援串接 Figma Design 和 Figma Make，適合習慣在終端機環境工作的開發者。
- **==Codex by OpenAI== ：** 支援串接 Figma Design 和 FigJam，可讀取設計稿內容輔助程式碼生成。
- **==ChatGPT== ：** 支援串接 Figma Design、FigJam、Figma Slides 和 Figma Make，是目前支援 Figma 產品範圍最廣的工具之一。

除了以上，Figma MCP 還支援其他多種開發工具，完整工具列表與各工具的安裝說明，可以到 [Figma MCP Catalog](http://figma.com/mcp-catalog) 查看。

### Figma MCP 要如何串接？

串接方式上，Figma 提供兩種 Server 類型，選哪一種取決於你的使用情境：

- **==Remote Server== ：** Figma 官方主要推薦的版本，功能最完整，絕大多數使用者都是選這個。透過網路連線，不需要在本機另外安裝任何東西，完成授權後即可使用。
- **==Desktop Server== ：** 僅針對特定的組織和企業使用情境設計， ==Figma 官方明確說明 **「 強烈建議使用 Remote Server 」**== ，一般使用者不需要考慮這個選項。

> 另外要注意的是， **「 寫入畫布（Write to canvas）」** 這個功能需要 Full seat 或 Dev seat 才能使用；Starter 方案每月只有 6 次的額度。

如果你還不知道怎麼在 **安裝** [**Claude Code**](https://frankknow.com/claude-code/) ，犬哥之前有拍一支新手教學影片：

![](https://www.youtube.com/watch?v=vPXXD7W56Bw)

【Claude Code 新手必學】從安裝流程、MCP＋SKills 功能強化、報表分析等實戰技巧，一次全學會！

接下來，我們以 **Claude Code** 串接 Figma MCP 為例，官方提供兩種安裝方式：

**方式一： ==安裝 Figma Plugin== （官方推薦）**

這是最快速的方式，安裝完成後除了 MCP Server 設定，還會一併附上幾個常見工作流程的 Agent Skills。

**Step 1** ：以終端機開啟 Claude Code 後，在輸入以下指令，從 [Anthropic 官方 Plugin](https://frankknow.com/claude-code-plugins/) 市集安裝 Figma Plugin：

==claude plugin install figma@claude-plugins-official==

**Step 2** ：安裝完成後即可開始使用，不需要額外設定。

**方式二：** **手動設定**

![Step 1 在終端機輸入指令，將 Figma MCP 加入 Claude Code](https://frankknow.com/wp-content/uploads/2026/05/Claude-Code-Install-Figma-MCP.jpg)

Step 1 在終端機輸入指令，將 Figma MCP 加入 Claude Code

**Step 1** ：在終端機輸入以下指令，將 Figma MCP 加入 Claude Code：

==claude mcp add –scope user –transport http figma https://mcp.figma.com/mcp==

> 💡 此處加上 ==–scope user== 是為了讓 Figma MCP 在所有專案都能使用，不需要每個專案重新安裝。

**Step 2** ：關閉終端機後，重新啟動 Claude Code。

![Step 3 在 Claude Code 裡輸入 /mcp，選擇 figma](https://frankknow.com/wp-content/uploads/2026/05/Claude-Code-Figma-MCP.jpg)

Step 3 在 Claude Code 裡輸入 /mcp，選擇 figma

**Step 3** ：在 Claude Code 裡輸入 ==/mcp== ，選擇 **figma** 。

**Step 4** ：選擇 **Authenticate** ，點擊 **Allow Access** 完成 Figma 帳號授權

![Step 5 Figma 授權完成，Claude Code 會顯示 Authentication successful. Connected to figma](https://frankknow.com/wp-content/uploads/2026/05/Claude-Code-Figma-Figma-Auth-Successful.jpg)

Step 5 Figma 授權完成，Claude Code 會顯示 Authentication successful. Connected to figma

**Step 5** ：授權完成後，Claude Code 會顯示 **Authentication successful. Connected to figma** 。

**Step 6** ：再次輸入 ==/mcp== 確認連線狀態，就可以開始下指令了。

設定完成後， ==把 Figma 裡的 **Frame 連結** 貼給 Claude Code== ，直接 ==下指令讓 AI **「 根據設計稿生成對應的程式碼 」**== ，或是 ==讓 AI 直接 **「 在 Figma 畫布上新增、修改設計元素 」**== 。

如果這邊步驟看得不是很理解，犬哥之前有拍過一支串接 Figma MCP 的影片，以 **「 方式二：手動設定 」** 為例，透過畫面你會更好上手，大家可以邊看邊自己動手試試看：）

![](https://www.youtube.com/watch?v=Br-tCRGioyE)

*Claude Code + Figma MCP：傳統 UI 開發流程已死？*

至於其他工具（Claude Code、VS Code、ChatGPT 等）的串接方式各有不同，建議大家直接看各工具的 [Figma 官方說明](http://figma.com/mcp-catalog) ，依照各工具的官方安裝說明操作！

---

## 誰最適合用 Figma AI？不同角色如何運用？

Figma AI 的功能不少，但不是每個人都需要從頭到尾全部用上。搞清楚各工具的定位，再對照自己的角色，會更容易知道從哪裡切入。

### 三個進階工具，各自解決不同層次的問題

| **************************工具************************** | ******************************核心用途****************************** | ******************************最適合的情境****************************** |
| --- | --- | --- |
| ****Figma Make**** | Prompt 生成可運行的互動原型 | 從零開始，快速驗證想法、產出可展示的成品。 |
| ****Figma MCP**** | 把 Figma 設計接入 AI 開發工具 | 工程師讓 AI coding 工具讀設計稿，精準生成程式碼。 |
| ****Design Agent**** | AI 直接在 Figma 畫布上操作 | 在既有設計中，用對話指令快速疊代修改。 |

三者是互補的，你可以 ==用 Make 快速驗證想法，確認方向後在 Figma 裡精修，再接上 MCP 讓工程師的 AI 工具讀設計稿來開發，疊代過程中用 Design Agent 在畫布上快速調整== 。

### 不同角色如何運用 Figma AI？

![設計師 / 工程師 / 非技術人員如何運用 Figma AI](https://frankknow.com/wp-content/uploads/2026/05/Different-Roles-Using-Figma-AI.jpg)

設計師 / 工程師 / 非技術人員如何運用 Figma AI

- **==設計師==**

從內建的 Design Agent 和圖片編輯功能熟悉 AI 輔助，日常設計效率會明顯提升！需要快速驗證想法時用 Figma Make 產出原型，既有設計稿的修改則善用 Design Agent 加速疊代。

- **==前端 / 全端工程師==**

優先把 Figma MCP 接好。接上之後，AI Coding 工具可以直接讀設計稿，不需要靠肉眼比對截圖或等設計師口頭說明，設計與開發的溝通誤差會明顯減少。

- **==PM / 非技術人員==**

Figma Make 是最低門檻的入口。不需要寫程式，也不需要懂 Figma 操作，只要能描述你要做什麼，就能做出一個可以展示、讓人實際點擊的產品雛形。

---

## 常見問題

### Figma AI 的內建功能需要付費才能用嗎？

部分功能需要 Figma 付費方案才能使用，詳細功能限制可以在 Figma 官方定價頁面確認。

### Figma Make 生成的原型可以直接拿去開發嗎？

可以。Make 輸出的是真正可運行的程式碼，可以直接分享連結讓人操作，或把程式碼導出後交給工程師繼續開發。

### Figma MCP 需要付費方案才能用嗎？

使用官方的 Figma MCP（Dev Mode MCP Server）需要付費方案。

因為 MCP 的運作仰賴「 開發模式（Dev Mode）」的資料串接，而該功能僅限於付費的 Figma 方案（如 Professional 或 Organization）中使用。

### Design Agent 跟 First Draft 有什麼不同？

First Draft 是在空白畫布上生成初始設計稿，輸出的是 Figma 圖層，適合從零開始發想。

Design Agent 則是在既有設計稿上接收指令直接操作，適合在現有設計基礎上疊代修改，兩者使用情境不同。

### Figma MCP 支援哪些程式語言或框架？

MCP 本身是設計稿的橋梁，生成什麼語言和框架，取決於你使用的 AI 開發工具，例如：Claude Code、Cursor、VS Code 等，大部分主流工具支援 React、Vue、HTML / CSS 等。

---

## 聯絡犬哥網站

在網頁設計＋AI SEO 行銷的過程中，或許會遇到一些解決不了問題，可以在下方留言給我，我會盡力協助解答！

接下來，額外分享 2 個實用資源，可有效幫助大家解決問題。

**資源一：犬哥網站（專業 WordPress 網頁設計＆ AI SEO 行銷公司）**

如果你沒時間自己架站，或是遇到網站沒有流量、Google 網站排名不理想，或是沒有被 AI 提及你的品牌。歡迎洽詢 [**犬哥網站（WordPress 網頁設計＆ AI SEO 行銷公司）**](https://frankknow.com/contact/) ，請專家協助你會更容易！

我們有多年網頁設計、AI SEO 行銷經驗，加上上百個實戰案例，絕對能滿足您的需求！

![犬哥網站｜網頁設計＆ AI SEO 行銷公司](https://frankknow.com/wp-content/uploads/2021/03/frankknow-homepage.jpg)

犬哥網站｜網頁設計＆ AI SEO 行銷公司

[**立即免費諮詢**](https://frankknow.com/contact/)

**資源二：WordPress 高手架站課＋SEO 排名飆升課（線上課程）**

如果想學習更深入的 WordPress 架站技巧，或是想透過 SEO 關鍵字提升網站流量？

可以參考犬哥近期推出的這兩門課程，課程非常的扎實，都是犬哥自己實戰出來的經驗分享，不打高空只教你做出成績！

![WordPress 高手架站課＆SEO 排名飆升課](https://frankknow.com/wp-content/uploads/2025/04/course-show.jpg)

WordPress 高手架站課＆SEO 排名飆升課

- **[WordPress 高手架站課](https://frankknow.net/wp-course/) ：  
	**從網頁設計觀念、WordPress / WooCommerce 後台操作、熱門主題.外掛教學、品牌網站 / 部落格 / 電商網站，3 大類型網站建置方法等豐富內容，一次教會你。  
	課程內容非常的紮實（長度超過 20 小時）。如果你剛學習自架網站，這門課程可以讓你用最快的速度，就學會這項技能！
- **[SEO 排名飆升課](https://frankknow.net/seo-course/) ：  
	**教你掌握關鍵字策略、SEO 文章撰寫、多種實戰 SEO 技巧、透視對手 SEO 策略、SEO 必備軟體教學、最新 AI SEO 應用等，大幅提升行銷能力。  
	帶你衝上 Google 排名第一頁，大量提升網站營收和訂單！