---
title: "Claude Skills 教學：一次搞懂 Skills 是什麼？怎麼用？以及如何打造自己的 AI 工作流 | 犬哥網站"
source: "https://frankknow.com/claude-skills/"
author:
  - "[[犬哥網站 - Audrey]]"
published: 2026-04-24
created: 2026-07-20
description: "Claude Skills 是一種把工作流程、規則與知識「 打包 」給 AI 使用的 技能模組，當 Claude 遇到相關任務時，就會自動載入這份手冊，按照你設定的方式執行。很多人在用 AI 工具時都有同樣的困擾，每次都要把流程、規則重新說一遍，品質還不穩定，前後落差很大，Skills 就是解決這件事的方法。這次我們就先從最核心的概念開始，一路到怎麼建立第一個 Skill、如何寫，讓你一次搞懂 Claude Skills ：）"
tags:
  - "clippings"
status: processed
---
==**Claude Skills** 是一種把工作流程、規則與知識「 打包 」給 AI 使用的 **技能模組** ，當 [Claude](https://frankknow.com/claude-ai/) 遇到相關任務時，就會自動載入這份手冊，按照你設定的方式執行。==

很多人在用 AI 工具時都有同樣的困惑，每次都要把流程、規則重新說一遍，品質還不穩定，前後落差很大，Skills 就是解決這件事的方法。 ==它讓 Claude 有能力「 **記住你的做事方式** 」，而且是可以重複使用的那種記住== 。

如果你想讓 Claude 變成能長期配合的工作助手，那你一定要了解 Skills！這次我們就先從最核心的概念開始，一路到怎麼建立第一個 Skill、如何寫，讓你一次搞懂 Claude Skills ：）

## Claude Skills 是什麼？

Skills 這個概念聽起來有點抽象，但只要你用過 **SOP 標準作業流程** ，就很容易理解它在做什麼。

### Skills 的核心概念

==**Skills** 就是「 一份寫給 Claude 看的工作手冊 」，把你的流程、知識、標準打包起來。==

你可以把它想成平常你怎麼做某件事、有哪些規則要遵守、什麼情況下要怎麼處理，全部整理成一份文件， ==讓 Claude 每次執行相關任務時，都有這份手冊可以參考== 。

它不是一個聊天記錄，也不是你每次都要貼上去的指令。它是一個 ==**可以被重複載入、可以跨對話使用的模組**== ，等於是讓 Claude 擁有了「 你的工作習慣 」。

### Skills 解決了哪兩件事？

Skills 設計出來，主要是為了解決使用 AI 工作時兩個最常見的痛點：

![Skills 解決使用 AI 工作時兩個最常見的痛點](https://frankknow.com/wp-content/uploads/2026/04/2-Major-AI-Pain-Points-Solved-by-Skills.jpg)

Skills 解決使用 AI 工作時兩個最常見的痛點

**一、省 Context**

Claude 和所有大型語言模型一樣，都有所謂的 ==「 **Context Window（上下文視窗）** 」限制，也就是一次能讀取和記憶的資訊量是有上限的== 。

如果你把所有規則、流程、參考資料都一次塞進去，不但很快就會超出限制，還會讓 Claude 分不清什麼才是重點。

==Skills 的設計是「 **需要時才載入** 」== 。當你的問題跟某個 Skills 有關，Claude 才會把那份手冊讀進來， **不相關的任務就完全不占空間** ，讓 Context Window 的使用更有效率，也能讓 Claude 在有限空間裡更專注。

**二、穩定品質**

==有了 Skills 等於你有了一份標準 SOP，Claude 每次都會按同一套流程走== ，不會因為你說法不同、遺漏了哪個細節，就跑出截然不同的結果。

這對需要品質一致的工作特別重要，像是固定格式的內容產出、Code Review、定期報告等。

### Claude Skills 適合哪些人用？

Skills 的使用場景很廣，幾乎任何有重複性工作流程的人都能受惠。以下列出幾個比較典型的使用族群：

- **==工程師== ：** 把開發流程、測試規範、部署步驟模組化，讓 Claude 在協助你的過程中，一直都按照同一套技術標準走。
- **==內容工作者== ：** 把寫作格式、品牌語氣、研究流程固定下來，不管是寫部落格、社群貼文還是文案，都能有一致的輸出水準。
- **==一般工作者== ：** 把重複性任務整理成可重用流程，例如每週的工作規劃、固定的整理格式、資料彙整的邏輯等，都可以用 Skill 來承接。

### Skills 跟 CLAUDE.md 有什麼不同？

如果你有用過 [Claude Code](https://frankknow.com/claude-code/) ，可能聽過 **CLAUDE.md** 這個東西，它跟 Skills 雖然有些像，但在設計邏輯上其實差很多。

![SKILL.md 跟 CLAUDE.md 的差異](https://frankknow.com/wp-content/uploads/2026/04/CLAUDE.md-vs-SKILL.md_.jpg)

SKILL.md 跟 CLAUDE.md 的差異

**CLAUDE.md 是什麼？**

==**CLAUDE.md** 是一個放在專案目錄的設定檔，用來給 Claude Code 提供專案層級的指示。== 你可以在裡面寫專案背景、使用規則、常見指令等，Claude Code 啟動時就會自動讀取。

**CLAUDE.md 的限制**

==CLAUDE.md 是「 **每次全部載入** 」的設計== ，也就是說 ==只要 Claude Code 進入專案時，不管這次的任務有沒有用到，所有內容都會佔用 context== 。

當你的專案規模越來越大、需要整理的規則越來越多，CLAUDE.md 就會變得越來越難維護，也越來越容易讓 context 被塞滿不必要的內容。

**Skills 的優勢：模組化、按需載入、跨專案重複用**

==Skills 是 **模組化** 的設計，每一個 Skill 都是獨立的單元，只有在任務跟它 **相關時才會被觸發和載入** 。==

而且 ==Skills 可以 **跨專案重複使用** ，你只需要做一次，就能在不同的工作場景裡反覆調用== 。這讓維護成本大幅降低，也讓你的工作知識更容易累積和沿用。

### Skills 與 Claude、Claude Code 的關係

==Skills 是 **Claude** 生態系統裡的一個功能模組== ， ==要用 Skills，就要先有 Claude 的使用環境== 。而 Claude 是 Anthropic 開發的 AI 助理，可以用來寫作、分析、寫程式、回答問題，應用範圍很廣。

如果你對 Claude 本身還不熟悉，可以先看這篇《 [Claude 怎麼用？新手到進階流程，1 篇搞懂ChatGPT 最強勁敵！](https://frankknow.com/claude-ai/) 》，了解它的基本定位與使用方式～

而 ==Skills 目前最常見的使用情境，是搭配 **Claude Code** 一起運作== ，你可以把整個開發流程、測試規範、部署步驟全部模組化，讓 Claude Code 每次都按照你設定的方式走。

Claude Code 是專為開發者設計的 AI 編程工具，可以直接在終端機裡操作、讀寫檔案、執行指令，Skills 在這個環境下可以發揮最完整的效果。

如果你還沒用過 Claude Code，也可以參考《 [Claude Code 零基礎教學：不會寫程式，也能讓AI 幫你搞定電腦大小事！](https://frankknow.com/claude-code/) 》 裡面有更詳細的使用說明哦！

另外，犬哥也有錄製一支實戰影片，示範如何把 Claude Code + Claude Skills + Nano Banana 組合在一起，做出有一定規模的網站 ：）

![](https://www.youtube.com/watch?v=qvEjFXmQpXQ)

*【保姆級教學】Claude Code＋Claude Skills＋Nano Banana 組合，做出價值 10 萬的網站！*

---

## 一個 Skill 會長什麼樣子？

接下來，我們就來看 Claude Skills 實際的結構長什麼樣子。知道結構，才能知道每個部分要放什麼、怎麼寫。

### 最基本的結構：SKILL.md

![Claude Skills 實際的結構](https://frankknow.com/wp-content/uploads/2026/04/The-Actual-Structure-of-Skills.jpg)

Claude Skills 實際的結構

==每個 Skill 的核心是一個叫做 **SKILL.md** 的 Markdown 文件== ，它分成兩個區塊：

**第一區塊：YAML frontmatter**

==文件最上方是 **YAML frontmatter** ，用來描述這個 Skill 的基本資訊== ，讓 Claude 判斷要不要啟動它。最重要的兩個欄位是：

- **==name== ：** Skill 的名稱，建議簡短清楚，讓人一眼知道這是做什麼用的。
- **==description== ：** 最關鍵的欄位，它要 ==清楚說明「 **這個 Skill 在什麼情況下應該被使用** 」== ，Claude 就是根據這個描述來判斷相關性的。  
	  
	==description 寫得準不準，直接決定這個 Skill 會不會在對的時機被觸發。==

**第二區塊：SOP 主體內容**

frontmatter 之後，就是寫給 Claude 看的實際指示內容。

這裡你可以 ==用 **清楚的步驟說明流程** 、 **列出判斷規則** 、 **提供範例格式** ，甚至 **加入條件判斷邏輯**== （如果是 A 情況就做 X，如果是 B 情況就做 Y）。

這一區塊的 ==內容越清楚、越有結構，Claude 執行時就越穩定== 。

### 完整資料夾結構

一個完整的 Skill ==除了 SKILL.md 主體，通常還會包含幾個子資料夾== ，讓內容可以分層管理：

![完整的 Skill 資料夾結構，除了 SKILL.md 主體，通常還會包含幾個子資料夾](https://frankknow.com/wp-content/uploads/2026/04/Complete-Skills-Folder.jpg)

完整的 Skill 資料夾結構，除了 SKILL.md 主體，通常還會包含幾個子資料夾

- **==scripts/== ：** 放可執行的 **腳本** 或 **命令** ，這些內容不會直接佔用 context，需要時才被呼叫執行。
- **==references/== ：** 放 **參考文件** 、 **知識庫** 、 **規格說明** 等較長的資料，同樣是按需載入，避免浪費 context 空間。
- **==assets/== ：** 放 **模板** 、 **範例素材** ，讓每次輸出的格式能保持一致。

這樣的結構設計讓 Skill 的主體保持輕量，同時又能在需要時調用豐富的補充資料。

---

## Skills 怎麼知道要不要啟動？

那我們做了這個 Skill，Claude 怎麼知道什麼時候要用它呢？答案在於它的 **三層載入機制** 。

### 三層載入機制

==Skills 採用的是一種 **Progressive Disclosure** 的設計，也就是你可以把它理解成「 **分層載入** 」。==

Claude 不會一開始就完整讀取整個 Skill，而是先看最前面的 metadata， ==判斷這個 Skill 和當前任務有沒有關係；只有在它覺得相關時，才會進一步把完整內容載入== 。

Skills 的設計採用分層讀取的邏輯，讓每一次的 context 使用都盡可能精準：

- **第一層： ==Metadata== （約 100 tokens）**

這是 Claude 啟動時就會預先載入的部分，主要就是 **name 和 description 的內容** ，大約只佔 **100 tokens** 。Claude 用這層來快速 ==判斷「 這個 Skill 跟現在的任務有沒有關係 」== 。

- **第二層： ==Instructions== （SKILL.md 主體）**

確認相關之後，Claude 才會把 **SKILL.md 的主體內容** 讀進來。這一層是 ==完整的 SOP 指示== ，建議控制在 **5000 tokens 以內** ，讓 Claude 能清楚消化又不佔太多空間。

- **第三層： ==Resources== （scripts/references/assets）**

只有在 SOP 主體明確指示需要的時候，Claude 才會去讀取 **scripts** 、 **references** 或 **assets** 裡的內容。這些資料 ==需要時才載入，不用的時候完全不佔 context== 。

### 為什麼這樣設計這麼重要？

**Context Window（上下文視窗）** 是使用 AI 工具時最珍貴的有限資源，你塞進去的每一行文字都會消耗它。

==Skills 的分層設計讓 Claude「 **用多少拿多少** 」== ，而不是一開始就把所有東西全部讀進來，這讓 context 的使用效率大幅提升。

另一個關鍵點是， ==description 寫得好不好，直接決定 Skill 會不會被正確觸發== 。

因為 ==第一層的 Metadata 就是 Claude 做判斷的依據== ，description 太模糊，Claude 不知道要不要用；description 寫得太廣泛，又可能什麼任務都觸發。

所以花時間把 description 寫好，是整個 Skill 品質的關鍵。

---

## 怎麼建立自己的第一個 Skill？

要怎麼做出自己的第一個 Skill？你不一定要先成為很會寫規格文件的人，重點是在動手之前，有三件事要先想清楚，之後你可以選兩種方法其中一種來執行：

### 建立 Claude Skills 前，先規劃這 3 件事

不少人一開始就急著打開編輯器開始寫，結果做出來的 Skill 又模糊又難用。建議在動手前，先花幾分鐘想清楚這三件事：

![建立 Claude Skills 前，先規劃這 3 件事](https://frankknow.com/wp-content/uploads/2026/04/3-Things-to-Plan-Before-Building-Skills.jpg)

建立 Claude Skills 前，先規劃這 3 件事

**1\. 你想解決的是哪種重複性任務？**

越具體越好。是每次都要重新說明的寫作格式？還是固定的程式碼審查邏輯？把這件事想清楚，description 就好寫了。

**2\. 這個流程裡有哪些固定規則？**

把那些「 **每次都要注意的事** 」列出來，這些就是 SOP 主體的核心內容。

**3\. 哪些知識該常駐，哪些資料該按需讀取？**

==常用的規則放 **SKILL.md 主體** ，偶爾才用到的參考資料放 **references/**== ，這樣 context 才不會被塞滿。

### 方法一：用官方的「 Skill Creator 」

如果你已經知道自己要做什麼，不想從頭手寫，可以直接用官方提供的 Skill Creator：

![用官方提供的 Skill Creator 建立 Skill](https://frankknow.com/wp-content/uploads/2026/04/Skill-Creator-.jpg)

用官方提供的 Claude Skill Creator 建立 Skill

1. 在 Claude 介面進入 **Customize** → **Skills**
2. 找到並 **點擊 skill-creator** → **＋** → **Create with Claude**
3. 開一個新對話後，告訴 Claude **「 幫我做技能包 」** ，接著照引導一步步走，Claude 會幫你產出 SKILL.md 的內容

這個方法適合已經 ==知道自己要什麼、但不想自己手寫格式的人== ，可以省下很多整理的時間。

**方法二：從實際工作流程回推製作**

如果你已經有一套成熟的工作方式，也可以用這個方法：

1. 先把你自己的 SOP 整理出來，不需要很完整， **把步驟和規則列清楚就行**
2. 把整理好的內容貼給 Claude，請它幫你 **轉換成 SKILL.md 的格式**
3. 再自己檢查一遍 description 是否精準， **步驟是否完整**

這個方法適合 ==有成熟工作流程、想把現有知識打包== 起來的人，因為你已經有實際運作過的 SOP，只是需要轉換格式而已，品質通常也比較扎實。

### 安裝、更新與備份 Skill

Skill 做好之後，還包括你能不能持續更新它、管理它，甚至在不同專案之間重複用。

**如何把做好的 Skill 安裝到 Claude？**

做好 SKILL.md 之後，到 Claude 的 Customize → Skills 介面，點「 ＋ 」>「＋ Create skill」新增，會有三種安裝 Skill 的方式。

可以選擇讓 Claude 引導你建立（Create with Claude）、自己直接在介面輸入內容（Write skill instructions），或是把 Skill 資料夾打包成 ZIP 檔上傳（Upload a skill）。

安裝完成後，可以用一個相關的任務測試看看是否正確觸發。

**如何修改已有的 Skill？**

如果你當初是 ==用 **「 Create with Claude 」** 或 **「 Write skill instructions 」** 建立的== ，建議平時就把內容另外備份一份（例如存在記事本或文件裡）。

![修改已有的 Skill](https://frankknow.com/wp-content/uploads/2026/04/Edit-Skill.jpg)

修改已有的 Skill

要修改時，先到 **Customize** → **Skills** 找到該 Skill（如圖：daily-todo），點「 **…** 」 ==選擇「 **Edit** 」直接編輯== ，或 ==「 **Edit with Claude** 」與 Claude 對話來修改== ，以及 ==「 **Replace** 」上傳更新的檔案== 都可以。

如果你當初是 ==用 **「 Upload a skill 」** 上傳.md 或 ZIP 檔建立的== ，直接 ==在本機修改 SKILL.md 的內容，刪除舊的 Skill 再重新上傳就好== 。

這也是為什麼建議養成在本機保留原始檔的習慣，修改起來最方便。

如果你有 ==用 **Claude Code**== ，Skill 是存在本機資料夾裡的實體檔案，可以 ==用任何文字編輯器直接開啟 SKILL.md 修改== ，存檔後重新載入即可。

**Skills 版本備份的基本觀念**

Skills 本質上就是文件， ==最簡單的備份方式就是 **用資料夾搭配日期命名** （例如 skill-v1-20250420），或者用 Git 做版本控制== 。

隨著 Skill 不斷優化，留下歷史版本會讓你在出問題時有東西可以回溯。

---

## 寫出好 Skill 的幾個關鍵技巧

做出 Skill 之後，品質好不好往往就差在幾個細節上。以下整理幾個最實際的技巧，幫你把 Skill 寫得更精準、更好用！

![Skill 寫得更精準、更好用的 3 個技巧](https://frankknow.com/wp-content/uploads/2026/04/3-Tips-for-Developing-Good-Skills.jpg)

Skill 寫得更精準、更好用的 3 個技巧

### 一、description 寫法決定觸發準確度

description 是整個 Skill 最重要的欄位，因為它是 ==Claude 判斷「 **要不要用這個 Skill** 」的唯一依據== 。

寫 description 的核心原則是： ==明確列出「 **什麼情況下觸發** 」== 。可以具體寫出關鍵字、使用場景，甚至反例（哪些情況不適用）。

- 太模糊（例如：幫我處理各種任務）→ Claude 不知道要不要用，容易被忽略
- 太廣泛（例如：任何有關工作的事情）→ 什麼任務都觸發，反而干擾正常對話

好的 description 大概是這樣：「 ==當使用者要求撰寫部落格文章、整理內容大綱或改寫文案時，載入此 Skill，依照品牌語氣與固定格式執行。== 」

### 二、步驟清楚、有判斷邏輯

SOP 主體的內容，步驟越清楚越好。與其用一大段文字敘述，不如 ==拆成 **編號步驟** ，讓 Claude 有明確的順序可以跟隨== 。

另外， ==加入 **判斷條件** 會讓 Skill 更有彈性== 。

例如：「 如果使用者提供了文章長度要求，依照指定長度撰寫；如果沒有，預設 800 字 」，這樣的條件邏輯讓 Claude 在各種情況下都能做出合理的判斷，而不是遇到邊緣狀況就卡住。

### 三、內容分層，並放對地方

一個常見的錯誤是把所有東西都塞進 SKILL.md 主體，結果主體又長又雜，Claude 讀起來效率很差，也容易忽略細節。

正確的做法是：

- **常用的核心規則：** 放 **SKILL.md 主體** ，Claude 每次啟動這個 Skill 都會讀到
- **較長的參考資料：** 放 **references/** ，只有需要深入參考時才載入
- **可執行的腳本或命令：** 放 **scripts/** ，需要執行時才呼叫

這樣的分層讓主體保持精簡，也讓整個 Skill 的 context 使用效率更高。

---

## Skills 實際能做什麼？幾個真實應用場景

Claude Skills 真正有價值的地方，是幫你省下重複交代、反覆確認的時間。理解完前面的觀念後，我們就來看幾個實際的使用場景，看看有哪些真的可操作的應用方式～

### 一、網站開發與架設：讓 Claude 熟悉你的建站流程

如果你有 ==固定的 **建站流程**== ，Skills 可以讓 Claude 每次協助你時，都按照同一套方式走， ==不需要每次重新說明你用的框架、命名規則或部署流程== 。

以 WordPress 為例，你可以 ==把 **慣用的主題設定方式** 、 **外掛配置邏輯** 、 **SEO 基本設定步驟** 全部打包成一個 Skill== ，讓 Claude 在協助你架站或修改時，都清楚你的習慣與標準。

不過前提是，你自己要先有一套成熟的建站流程，Skills 才有東西可以承接。但如果你沒有架站經驗，也沒關係！

**自己架設網站，沒有想像中困難！**

即使完全不會寫程式、沒有設計或工程背景， ==只要跟著 [**《WordPress 高手架站課》**](https://frankknow.net/wp-course/) 一步步學習，就能親手完成一個專業網站== 。

![WordPress 高手架站課](https://frankknow.com/wp-content/uploads/2024/12/wp-course-pic2.png)

WordPress 高手架站課

如果你正打算用 WordPress 架站，這門線上課會手把手帶你從零到做出三種主流網站：形象站、部落格、電商站。

- ==不需要懂程式碼== ，採用視覺化編輯器，拖拉就能完成頁面。
- ==20 小時扎實教學== ，從安裝到上線，全程手把手示範。
- ==不會寫程式也能跟上== ，進階者還能加強進行網站優化。

包含熱門主題、外掛，例如：WooCommerce 及第三方金物流（信用卡、銀行轉帳、超商取貨、貨到付款）的完整串接，看完課程即可打造你的品牌網站、作品集、電商網站。

****《 WordPress 高手架站課 》你會學到：****

- **==多種頁面編輯模式== ：** Elementor 的「 段／欄／元素、Flexbox、Container 」編輯法、Gutenberg 區塊編輯器。
- **==WordPress 後台操作== ：** 文章與頁面管理、媒體庫、權限與留言管理、外觀自訂。
- **==網站實務觀念== ：** 配色與品牌視覺、RWD、常用外掛挑選與設定。
- **==學習時的完整支援== ：** 助教陪跑、客服問題解答、學員私密群組交流與資源共享。

**《 WordPress 高手架站課 》單元一覽**

| **********單元********** | **********時數********** | ****你會做到的事**** |
| --- | --- | --- |
| **形象網站** | 4 小時 | 本機建站、頁首頁尾客製、動畫／hover 效果、RWD、常用區塊儲存。 |
| **部落格網站** | 4.3 小時 | 版型與配色、LOGO 製作、側邊欄優化、閱讀進度條與目錄等實用模組。 |
| **電商網站** | 9.8 小時 | 根據提示（Prompt）創造文字、圖片、音樂等。 |

**《 WordPress 高手架站課 》課程單元**

[**加入課程，架站一次學到位**](https://frankknow.net/shop/wp-course/)

**（現正特惠 ==不到 3 折== ！若與 [「 SEO 排名飆升課 」](https://frankknow.net/seo-course/) 合購， ==再享 8 折== ）**

### 二、每日工作規劃：輸入兩個字，它自動跑所有步驟

你可以做一個 ==**「 每日工作規劃 Skill 」** ，裡面寫好你的規劃邏輯：先整理今日待辦、按優先度排序、預估時間、找出可以批次處理的任務等步驟== 。

設定好之後，只要輸入「 開始今日規劃 」，Claude 就會自動跑完整個流程，輸出一份符合你習慣的規劃清單，完全不需要每次重新說明。

### 三、Code Review 標準化：讓 Claude 按照你的標準審查

如果你的團隊有一套 ==**Code Review 的規則** ，例如：命名規範、安全性檢查、效能標準、文件要求等，可以把這些全部打包成一個 Code Review Skill== 。

往後只要請 Claude 幫你看 code，它就會按照你們的標準來審查，不會每次因為沒有上下文而遺漏重要的審查點，也不用每次重新貼一次規範。

### 四、內容製作流程：格式、語氣、品質標準一次打包

內容工作者可以把完整的 ==**內容製作 SOP** 打包成一個 Skill，包含：文章結構要求、品牌語氣規範、標題寫法、CTA 放置位置、SEO 基本要求等== 。

這樣無論是你自己在用 Claude 寫作，或是你想讓團隊成員也能用 Claude 產出符合品牌標準的內容，都能有一套共同依循的標準，輸出品質自然更穩定。

---

## Skills 核心價值：讓工作流程被複製

Skills 的核心價值，不是讓 AI 更聰明，而是 **讓你的工作流程可以被複製、被沿用** 。

過去累積的做事方式、踩過的坑、建立起來的標準，本來都只在你的腦袋中，每次換工具、換對話就消失了。Skills 讓這些知識有地方落地，成為可以跨對話、跨專案反覆調用的模組。

不管你是工程師、內容工作者，還是需要每天處理大量重複任務的工作者，Skills 都是值得花時間建立的東西！

從一個 Skill 開始做，慢慢累積，你會發現自己的工作方式越來越有系統，AI 的輸出也越來越符合你的預期：）

---

## 常見問題

### Skills 免費方案可以用嗎？

**可以** ，Skills 適用於免費、Pro、Max、Team 和 Enterprise 所有方案 Claude。

不過有一個前提：需要先到 Settings ＞ Capabilities 確認「 Code execution and file creation 」功能已開啟，Skills 才能正常運作。

### Skills 只有 Claude Code 才能用嗎？

**不是。** Skills 是 Claude 整體功能的一部分，並不限定只能在 Claude Code 環境下使用。

不過目前 Skills 的使用情境確實與 Claude Code 的工作流程結合最緊密，許多實際範例也多以 Claude Code 為主。

### 一個專案可以安裝多個 Skills 嗎？

**可以。** 你可以針對同一個專案安裝多個 Skills，每個 Skill 負責不同的功能模組。

Claude 會根據當下的任務類型，判斷哪些 Skill 相關並載入。這也是為什麼每個 Skill 的 description 要寫得精準，才能確保觸發邏輯不會混亂。

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