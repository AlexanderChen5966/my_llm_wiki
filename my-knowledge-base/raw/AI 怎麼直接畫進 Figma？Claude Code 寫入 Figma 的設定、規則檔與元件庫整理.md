---
title: "AI 怎麼直接畫進 Figma？Claude Code 寫入 Figma 的設定、規則檔與元件庫整理"
source: "https://rar.design/posts/figma-mcp-write-to-canvas-setup-guide"
author:
  - "[[設計師 Riven]]"
published: 2026-09-14
created: 2026-09-15
description: "Claude Code 透過 Figma MCP 的 use_figma 工具，可以用你的元件庫和 Variables 直接在畫布上生成畫面。本文整理設定步驟、CLAUDE.md 與 DESIGN.md 規則檔起手版、元件庫要整理到什麼程度 AI 才畫得對，以及 seat、額度與 beta 限制。"
tags:
  - "clippings"
status: processed
---
把 use\_figma 接進你自己的元件庫：安裝、CLAUDE.md 與 DESIGN.md 起手版、元件庫整理清單與門檻一次講完

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80,format=webp/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年9月14日 下午 12:58

UI/UX 設計

FIGMA × CLAUDE CODE · 2026

![Figma 官方示意圖：終端機裡的 Claude Code 載入 Figma skills 後，直接在 Figma 畫布建出 72 個變體的 Button 元件](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/6090df78-762b-4651-a8d1-6b47922e04ea/e0593f6b-89cf-4077-b390-47ea55d2fadd.png)

Figma 官方示意圖：終端機裡的 Claude Code 載入 Figma skills 後，直接在 Figma 畫布建出 72 個變體的 Button 元件

▲ Figma 官方示意：在終端機下一句指令，Claude Code 載入 figma-use 與 figma-generate-library 兩個 skill、讀了三個檔案，然後在畫布上建出 72 個變體的 Button 元件。

QUICK ANSWER

Claude Code 透過 Figma MCP 的 use\_figma 工具，可以用你的元件庫和 Variables 直接在 Figma 畫布上生成畫面。做法是裝官方 plugin 接上 MCP，開一個資料夾放 CLAUDE.md 與 DESIGN.md 規則檔，把 Figma 檔案連結、頁面規則、命名與元件規則寫進去，再讓 AI 先讀規則、查元件、建一個畫面、截圖自檢。門檻：Full seat 才能寫入，Starter 方案每月只有 20 次讀取額度，目前 beta 免費。

同一句 prompt 丟給 Claude Code 兩次，會拿到兩個都能看、但都不像你家產品的畫面。按鈕圓角多了 2px，主色差半階，間距用的是 AI 自己順手的數字。這種畫面拿去簡報沒問題，拿去接手改稿，比從空白畫布開始還累。

差別從來都在 AI 動筆前讀到了什麼。今年 3 月 Figma 開放 [use\_figma 工具](https://rar.design/posts/figma-use-figma-agents-canvas-skills) 之後，AI 已經能用你的元件與 Variables 在畫布上蓋東西，能不能蓋對，就看你有沒有先把脈絡準備好。這篇把整套做法攤開：怎麼把 Claude Code 接上 Figma、規則檔要寫什麼、元件庫要整理到什麼程度、現在還做不到什麼。規則檔的起手版整段貼在文章裡，直接複製就能改。

我在設計圈快十年，帶過的 Figma 學員上萬，元件庫整理這件事教了很多年。這一次，同一套整理方法多了一個讀者，而且是最會照字面執行的那一個。

CONTENTS

01

[今年 AI 真的能寫進 Figma 了](#chapter-1)

02

[先確認你能不能用：seat、額度、費用](#chapter-2)

03

[十分鐘把 Claude Code 接上 Figma](#chapter-3)

04

[開一個資料夾，讓 AI 每次都從同一份脈絡出發](#chapter-4)

05

[元件庫要整理到什麼程度，AI 才畫得對](#chapter-5)

06

[實際跑一輪：從讀檔到設計師接手](#chapter-6)

07

[現在還做不到的事](#chapter-7)

08

[重點整理](#chapter-8)

FAQ

[常見問題](#faq)

CHAPTER 01 · WHAT CHANGED

## 今年 AI 真的能寫進 Figma 了

Figma MCP 前兩年做的事都是「讀」：把畫面、Variables、元件結構抓出來給 AI 寫程式。今年 2 月先開了反方向的門， [Claude Code to Figma](https://rar.design/posts/claude-code-to-figma-guide) 能把跑起來的網頁 UI 轉成可編輯的圖層。3 月 24 日 use\_figma 上線，才是真正的寫入：agent 直接在你的檔案裡建 frame、元件、Variables 和 auto layout，落地的是原生圖層，你可以接著改。

TIMELINE · FIGMA MCP 從讀到寫

2 月 18 日

Code to Canvas

活的網頁 UI 轉成可編輯圖層，第一次從 code 回到畫布

3 月 24 日

use\_figma ＋ Skills

agent 直接寫入畫布，用 markdown skill 規範它怎麼做

6 月 16 日

字型、Slides、FigJam

官方補上自訂字型，寫入範圍擴到簡報、白板與 Make

6 月 24 日

Config 2026

Figma 自己的畫布 agent 升級，跟 MCP 這條線並行

現在

Beta，免費

官方明說之後改用量計費，趁現在把流程建起來

機制講白了很簡單：agent 在你的檔案脈絡裡執行 Plugin API 的 JavaScript。Figma plugin 能做的事，它幾乎都能做，差別是寫程式的人換成了 AI。所以它需要三樣東西：一個檔案連結（遠端 server 靠連結定位）、對那個檔案的編輯權、還有一份告訴它「這個檔案怎麼用」的說明。前兩樣是設定，第三樣是這篇文章的重點。

支援的 client 包含 Claude Code、Claude Desktop、Codex、Cursor、Copilot 等。這篇用 Claude Code 示範，因為它的 CLAUDE.md 與 plugin 機制最適合把規則固定下來；如果你不想開終端機，Claude Desktop 接上 Figma connector 之後也走同一套規則。

![](https://www.youtube.com/watch?v=vQMeJMgxRQY)

▲ Figma 官方影片〈Agents, meet the Figma canvas〉，3 月 24 日 use\_figma 上線時發布。

CHAPTER 02 · BEFORE YOU START

## 先確認你能不能用：seat、額度、費用

很多人裝好 plugin、下第一個指令就撞牆，問題出在帳號，跟設定無關。先把三件事對一下。

Full

寫入需要的 SEAT

20 次／月

STARTER 免費方案

200 次／天

PROFESSIONAL FULL SEAT

$0

BETA 期間

**Seat。** 寫入畫布要 Full seat，Dev seat 只能讀，而且你要有那個檔案的編輯權限。這一條擋掉最多人：很多設計師拿的是團隊給的 Dev seat 或 View seat。

**額度。** Figma 官方的額度表：Starter 免費方案每月 20 次工具呼叫；Professional 的 Full 或 Dev seat 每天 200 次、每分鐘 10 次；Organization 每天 200 次、Enterprise 每天 600 次；Organization 以上的 View 與 Collab seat 每月只有 6 次。額度算的是讀取類工具，寫入類工具不計。20 次一個月，只夠你確認「真的能動」，要認真跑流程，至少要 Professional 的 Full seat。

**費用。** 寫入功能目前是 beta，不另外收費。Figma 在官方部落格講得很直接：這會是用量計費的付費功能，免費期是他們在研究 agent 行為該怎麼算進付費座位。現在建的流程，之後不用重做，只是要開始付錢。

NOTE

接上之後先叫它跑一次 whoami 工具。它會回你登入的 email、所屬方案與 seat 類型。權限錯誤十次有九次是登錯帳號或 seat 不對，先驗這個，不要先改設定。

CHAPTER 03 · SETUP

## 十分鐘把 Claude Code 接上 Figma

Figma 官方推薦的路線是裝 plugin，一次拿到 MCP 設定和一整組 skills。手動加 MCP server 也行，但 skills 要另外抓，沒必要。

1

裝 Figma plugin

終端機輸入 claude plugin install figma@claude-plugins-official，這是 Anthropic 官方 marketplace 上的 Figma plugin，MCP server 設定和 figma-use、figma-generate-design、figma-generate-library 這些 skill 一起進來。

2

授權 Figma 帳號

重開 Claude Code，輸入 /plugin，切到 Installed 分頁，選 figma 按 Enter，瀏覽器會跳出 Figma 的授權頁，按 Allow access。

3

確認連線

回到終端機輸入 /mcp，figma 後面顯示 connected 就完成了。打一個 / 可以看到所有 Figma skill 的指令清單。

4

跑一個最小測試

開一個空白的 Figma 檔，把連結貼給它：/figma-use 在這個檔案新增一頁叫 Skill test，做一張訂閱方案卡：標題、三行功能、一顆主要按鈕，全部用 auto layout。畫布上長出東西，設定就通了。

![Figma 官方教學畫面：終端機執行 claude plugin install figma@claude-plugins-official 後顯示安裝成功，Claude Code 的 /plugin 指令清單出現 figma-generate-library skill](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/8ca17e7f-dac3-4b63-a4e3-6d654210ca4e/18eccffe-8fa6-40e9-a23b-7929ace17d7a.png)

Figma 官方教學畫面：終端機執行 claude plugin install figma@claude-plugins-official 後顯示安裝成功，Claude Code 的 /plugin 指令清單出現 figma-generate-library skill

▲ Figma 官方教學畫面：plugin 裝好之後，Claude Code 的指令清單會直接出現 figma-generate-library 這類 skill。

![Figma 開發者文件截圖：Claude Code 輸入 /mcp 後，figma server 顯示 connected 狀態](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/42af5a87-f6a6-4363-bee2-6a3febdaedbd/965644b7-6ea6-4ab8-b435-c4362e977913.png)

Figma 開發者文件截圖：Claude Code 輸入 /mcp 後，figma server 顯示 connected 狀態

▲ Figma 開發者文件的截圖：/mcp 畫面裡 figma 顯示 connected，就是可以開工的狀態。

如果你的公司擋了 plugin 安裝，手動路線是 claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp，加了 --scope user 之後所有專案都能用，不必每個資料夾重加；然後一樣用 /mcp 選 figma 做授權。這條路 skills 不會自動進來，要到 Figma 的 GitHub（figma/mcp-server-guide）自己抓。

兩件事要記得。寫入只有遠端 server 支援，桌面版 MCP server 是讀取用的，別裝錯。還有 plugin 這個機制本身值得認識一下，它是 Anthropic 把 MCP 設定、skill 和指令打包的方式，我之前整理過 [設計師該先裝的三個 Claude plugins](https://rar.design/posts/claude-plugins-designer-guide) ，Figma 是其中一個。

CHAPTER 04 · THE WORKSPACE

## 開一個資料夾，讓 AI 每次都從同一份脈絡出發

接上之後，第一個想法通常是直接在對話裡打需求。可以，但每一次都要重講元件庫在哪、要用哪一頁、命名怎麼取，講漏一次結果就歪一次。解法是把這些話寫成檔案，放在一個資料夾裡，從那個資料夾啟動 Claude Code。它啟動時會自動讀資料夾裡的 CLAUDE.md，你在裡面用 @DESIGN.md 把規則檔匯進來，之後每一次任務都從同一份脈絡出發。

要不要開 git 隨你。Claude Code 只認資料夾，版本控制是為了規則檔改壞了能退回去，跟 AI 能不能動沒有關係。

WORKSPACE ANATOMY · 設計工作區的四個部件

CLAUDE.md

入口，自動載入

只寫工作方式與匯入指令，短，穩定。每次啟動一定被讀到。

DESIGN.md

Figma 怎麼用

檔案連結、頁面規則、命名、元件與變數、每次任務的流程。設計師維護的那一份。

.claude/

Skills 與分主題規則

plugin 裝的 Figma skills 之外，自己的 skill 或 rules/ 分檔放這裡。選配。

references/

給它看的東西

匯出的 token JSON、範本畫面截圖、競品參考。AI 讀圖比讀形容詞準。

### CLAUDE.md 起手版

這一份的原則是短。它是入口，規則細節丟給 DESIGN.md，這裡只放「怎麼工作」的四條鐵則和匯入指令。

CLAUDE.md

\# 專案：產品名稱 設計工作區

這個資料夾是 Figma 設計工作的脈絡來源。動 Figma 之前先讀完 @DESIGN.md。

\## 工作方式

\- 每次任務先讀 DESIGN.md 指定的檔案，用 get\_metadata 確認現有頁面與結構，再動手。

\- 元件一律先用 search\_design\_system 查，找不到就回報，不要自己畫替代品。

\- 一次只做一個畫面，做完用 get\_screenshot 比對，回報沒對到 variable 的地方。

\- 只在 DESIGN.md 指定的頁面建立東西，其他頁一律不碰。

### DESIGN.md 起手版

這一份才是設計師的主戰場。它回答五個問題：檔案在哪、哪一頁能動、東西怎麼命名、元件和變數怎麼用、每次任務照什麼順序做。Figma 開發者文件裡有一頁專門講怎麼給 agent 加規則，還附了一段請 AI 分析專案後自動產出規則檔的 prompt，但那份偏向寫程式端。下面這份是給「AI 要進 Figma 畫東西」用的，欄位換成你的就能跑。

DESIGN.md

\# DESIGN.md：Figma 規則

\## 檔案入口

\- 主檔（工作用）：https://www.figma.com/design/主檔ID/產品名稱

\- 元件庫（唯讀，只查不改）：https://www.figma.com/design/元件庫ID/DS

\- 工作頁：AI Sandbox。所有新畫面只能建在這一頁。

\## 頁面結構

\- Cover：封面與版本紀錄，不動。

\- Flows：正式流程，只有設計師會改。

\- Components：本檔專用元件，不動。

\- AI Sandbox：AI 的工作區。每個任務開一個 section，section 名用任務名。

\## 命名規則

\- 畫面 frame 命名：flow/screen/state，例：checkout/payment/error

\- 圖層名用內容命名：Header、Price、CTA，不留 Frame 123、Rectangle 45。

\- 不改任何既有元件或圖層的名字。

\## 元件與變數

\- 顏色、間距、圓角、字級一律綁 variables。集合：Color、Space、Radius、Type。不寫死任何色碼或數字。

\- 元件只用元件庫已發佈的版本，不 detach、不建新的主元件、不改變體屬性。

\- 找不到對應元件時先回報缺什麼，不要用矩形拼一個假的。

\## 排版

\- 全部 auto layout。間距只用 4 的倍數：4、8、12、16、24、32、48。

\- 手機畫面寬 390，桌面 1440。畫面之間左右間隔 160。

\## 每次任務的流程

1\. 讀完這份檔案。

2\. get\_metadata 看 AI Sandbox 頁現況，確認沒有同名 section。

3\. search\_design\_system 列出這個畫面會用到的元件，先貼清單給我看。

4\. 建一個畫面。一次呼叫的輸出上限 20KB，畫面大就分區塊建。

5\. get\_screenshot 截圖，跟 references/ 裡的範本比對。

6\. 回報：用了哪些元件、哪些值沒對到 variable、哪裡需要我接手。

DESIGN.md 這個檔名，用過 [Claude Design](https://rar.design/posts/claude-design-web-guide) 的人會覺得眼熟，那邊也是用一份 DESIGN.md 描述設計系統再讓 AI 生成介面。思路一樣，內容不同：那份講的是視覺長什麼樣，這份講的是 Figma 檔案怎麼用。兩份可以並存，互不衝突。

寫這份檔案的手感，可以參考社群裡把設計規範寫成機器可讀規格的做法，像 [mono-color 這支 skill](https://rar.design/posts/mono-color-skill-editorial-print-guide) 把單色印刷的規則寫到連留白比例都有數字。原則相同：形容詞越少、數字和清單越多，AI 照做的準確率越高。

CHAPTER 05 · THE LIBRARY

## 元件庫要整理到什麼程度，AI 才畫得對

規則檔寫得再好，AI 真正動手時讀的是你的 Figma 檔案。這一章是整篇最重要的部分，因為它決定了輸出是「可以接手改」還是「重做比較快」。

先講一個容易被忽略的事實： **agent 找元件的方式是搜尋名字** 。它呼叫 search\_design\_system，用文字比對你元件庫裡的名稱，名字就是它唯一的查詢介面。「Button/Primary/Large」找得到，「Rectangle 41 copy 3」永遠找不到。這句話換個說法：你的元件命名就是 prompt 的一部分，而且是 AI 每次都會讀的那一部分。

ITEM

AI 讀得懂

AI 會亂猜

命名

類別／變體／狀態三層：Button/Primary/Hover，圖層用內容命名

Frame 128、btn\_new\_final2、同一個元件有三種寫法

Variables

顏色、間距、圓角、字級都綁 variable，集合有 light／dark 模式

色碼直接填在圖層上，或只有 style 沒有 variable

Auto layout

每個容器都是 auto layout，間距與 padding 綁 Space 變數

元素用絕對位置排，看起來對齊、結構上沒對齊

元件

主元件有變體屬性，已發佈到元件庫，實例不 detach

半數實例已 detach，本地複製一份改過的「元件」滿地都是

頁面與範本

有專用工作頁，頁上放兩個做好的範本畫面

一頁塞所有東西，新舊版本並排，沒有可以照抄的對象

**Variables 沒綁，等於給 AI 一盒蠟筆。** 它只能把色碼寫死在圖層上，你之後切 dark mode、換品牌色，那些畫面全部不動。反過來，元件庫的顏色、間距、圓角都綁在 variable 上，AI 生出來的畫面就跟你的系統活在同一套值裡，改一個變數，畫面跟著動。這一點在 Figma 官方文件的建議裡講得很明確：agent 是「用你的 variables 和元件建東西」，前提是那些東西存在。

**範本畫面比規則文字更有效。** Figma 官方建議 agent 動手前先讀檔案的現有結構，所以在工作頁上放兩個你親手做好的畫面，命名、間距、元件用法都對，AI 會先看再畫，照抄的準確率遠高於照文字描述去想像。這是我測下來最省力的一招：與其在 DESIGN.md 多寫十條，不如多放一個範本。

最後一個重點跟 AI 無關。上面那張表的左欄，就是一個新同事進來第一天能不能看懂你檔案的標準。間距系統怎麼定，我在 [8pt 網格與四層間距](https://rar.design/posts/web-spacing-system-8pt-guide) 那篇寫過；元件與變體的整理法，是 Figma 課程裡整整一個章節。這些功課以前是為了團隊協作做的，現在多了一個理由。

“

元件庫整理到 AI 讀得懂的那一天，它也就整理到新同事讀得懂了。

CHAPTER 06 · THE LOOP

## 實際跑一輪：從讀檔到設計師接手

規則檔和元件庫都到位之後，每一次任務長這樣。

WORKFLOW · 一次任務的五步

STEP 01 →

讀規則

CLAUDE.md 帶入 DESIGN.md

STEP 02 →

查元件

get\_metadata 與 search\_design\_system

STEP 03 →

建一個畫面

use\_figma 分區塊寫入

STEP 04 →

截圖自檢

get\_screenshot 比對範本、回報缺口

STEP 05

設計師接手

修細節、搬進 Flows

Prompt 不用長，因為脈絡已經在檔案裡。一個典型的任務長這樣：

PROMPT

照 DESIGN.md 的流程，在 AI Sandbox 頁做「訂閱方案」畫面的手機版：三個方案卡並排改成直排、年繳方案要有推薦標籤、底部固定一顆主要按鈕。先列出你要用的元件清單，等我說 OK 再建。建完截圖比對 references/pricing-desktop.png，列出沒對到 variable 的值。

要對第一次的結果有正確預期。Figma 自家產品經理在 6 月的官方部落格分享過一個案例：把一個只存在於程式碼裡的畫面推進 Figma，agent 讀了元件庫、決定哪些重用哪些新建，第一版很強，但 auto layout、字型和幾個沒對到 variable 的顏色還是要人修。這是官方自己的用法，也是你該預期的品質： **前 80% 它做，最後 20% 你做** ，而且那 20% 正是設計師的判斷所在。

截圖自檢那一步是 Figma 官方特別強調的機制，他們叫它 self-healing loop：agent 建完就截圖，跟目標比對，不符合的地方自己再改一輪。因為它動的是真實的結構，那些修正會落回元件與 variables，而不只是把畫面調到看起來像。你在 DESIGN.md 裡指定比對對象，這個迴圈才有目標。

CHAPTER 07 · LIMITS

## 現在還做不到的事

這一段照官方文件寫，不加修飾。

20KB PER CALL

每次呼叫的輸出上限 20KB。一個完整的畫面常常超過，所以規則檔裡要寫「分區塊建」，讓它 header、內容、底部分開寫，再組起來。

NO IMAGE ASSETS

圖片素材還塞不進去。照片、插圖的位置它只能放佔位方塊，你之後自己換。官方說圖片支援在路線圖上，排在自訂字型之後。

FONTS

6 月的官方部落格說已經支援上傳到 Figma 的自訂字型，但開發者文件到現在還把自訂字型列在限制清單。實際用之前先當作可能 fallback，第一次跑完檢查字型有沒有對。

BETA QUALITY

同一個 prompt 兩次結果不同，這是模型的本質。規則檔和範本能把變異壓小，壓不到零。官方自己的說法是輸出是 beta 等級，要人工清理。

REMOTE ONLY

寫入只有遠端 server 支援，而且只有 Figma MCP 目錄上列出的 client 能連。桌面版 server 是讀取用的，Dev seat 也是讀取用的。

還有一件事要分清楚。Config 2026 之後，Figma 自己在畫布裡的 agent 也升級了，一樣吃 skills、也能接外部工具。那是另一扇門：你在 Figma 裡面跟它講話。MCP 這一條是把你已經在終端機用的 coding agent 接進來，適合跟程式碼、外部資料、其他 MCP 一起工作。兩條線並行，不衝突，只是這篇講的是後者。Config 那一波的其他東西，像 [Generative Plugins](https://rar.design/posts/figma-config-2026-generative-plugins) 和 [Code Layers](https://rar.design/posts/figma-code-layers-guide) ，之前分別寫過。

CHAPTER 08 · TAKEAWAYS

## 重點整理

TAKEAWAYS

→

use\_figma 讓 Claude Code 用你的元件庫和 Variables 直接在畫布上建原生圖層，3 月 24 日上線，目前 beta 免費，之後用量計費。

→

先確認帳號：寫入要 Full seat 加檔案編輯權；Starter 每月 20 次讀取額度只夠試，認真用至少 Professional。

→

設定走官方 plugin：claude plugin install figma@claude-plugins-official，/plugin 授權，/mcp 看到 connected。

→

開一個資料夾，CLAUDE.md 當入口、DESIGN.md 寫檔案連結、頁面規則、命名、元件與變數、任務流程，AI 每次從同一份脈絡出發。

→

元件名稱是 AI 唯一的查詢介面，Variables 沒綁它只能寫死色碼，工作頁上放兩個範本畫面比多寫十條規則有效。

→

預期是前 80% 它做、後 20% 你做；每次 20KB、沒有圖片素材、beta 品質，這些現在都還在。

設計系統一直都是寫給人看的文件，元件命名、變數、間距規則，每一條都是為了讓下一個接手的人不用問你。AI 沒有改變這件事的定義，它只是第一個會逐字照做的讀者。你把檔案整理到它讀得懂，它就把畫面蓋在對的頁、用對的元件；你把規則寫得含糊，它就用自己的方式填空。這幾年設計師被問「AI 來了你要學什麼」，這篇的答案很老派：把你本來就該整理好的東西整理好。

RELATED READS · 延伸閱讀[Figma 開放 AI Agent 直接操作畫布：use\_figma 工具與 Skills 技能系統完整介紹](https://rar.design/posts/figma-use-figma-agents-canvas-skills)

[

3 月 24 日當天的第一手整理，這篇的前置閱讀

→](https://rar.design/posts/figma-use-figma-agents-canvas-skills)[

Figma MCP 進階教學：七大工具解析、Code Connect 與雙向工作流完整攻略

讀取端的完整地圖，Code Connect 與檔案優化

→](https://rar.design/posts/figma-mcp-advanced-guide)[

Claude Code to Figma 教學：把 AI 生成的介面直接變成可編輯設計稿

2 月的 Code to Canvas，另一個方向的寫入

→](https://rar.design/posts/claude-code-to-figma-guide)[

Claude Plugins 是什麼？設計師先裝這三個，讓 Claude 變成你的設計團隊

plugin 機制怎麼運作，Figma 只是其中一個

→](https://rar.design/posts/claude-plugins-designer-guide)[

Claude Code 怎麼做出沒有 AI 感的網站？三步驟把 AI 調教成懂品味的設計師

同一套「先給脈絡再動手」的思路，用在網站上

→](https://rar.design/posts/claude-code-design-taste-workflow)[

網頁排版的間距系統：8pt 網格與四層間距，讓版面看起來有人在管

DESIGN.md 排版那一段的完整版

→](https://rar.design/posts/web-spacing-system-8pt-guide)[

Figma Config 2026 有什麼新功能？Generative Plugins 用一句話做出專屬外掛

Figma 原生 agent 那一條線的近況

→](https://rar.design/posts/figma-config-2026-generative-plugins)[

Figma Code Layers 是什麼？程式碼變成設計素材的完整教學

程式碼在畫布上的另一種存在方式

→](https://rar.design/posts/figma-code-layers-guide)

EXTRA · 官方資源

→ [Figma 部落格：Agents, Meet the Figma Canvas](https://www.figma.com/blog/the-figma-canvas-is-now-open-to-agents/)

→ [開發者文件：Write to canvas（seat 需求、限制清單）](https://developers.figma.com/docs/figma-mcp-server/write-to-canvas/)

→ [開發者文件：Add custom rules and instructions](https://developers.figma.com/docs/figma-mcp-server/add-custom-rules/)

→ [開發者文件：Rate limits & access（各方案額度表）](https://developers.figma.com/docs/figma-mcp-server/rate-limits-access/)

→ [Figma Help Center：Claude Code and Figma 設定步驟](https://help.figma.com/hc/en-us/articles/39888612464151-Claude-Code-and-Figma-Set-up-the-MCP-server)

→ [GitHub：figma/mcp-server-guide（官方 skills 原始碼）](https://github.com/figma/mcp-server-guide)

→ [Anthropic 文件：CLAUDE.md 與 @ 匯入語法](https://docs.anthropic.com/en/docs/claude-code/memory)![100 種 Figma 設計的方法](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/bc8c4056-d9e6-443e-b7fd-6c5f75178b89/66d668a8-e692-4241-bfb0-e513f1319241.png)

100 種 Figma 設計的方法

[

COURSE

100 種 Figma 設計的方法

元件、變體、Variables、Auto Layout，這些讓 AI 讀得懂你檔案的基本功，課裡從零帶到能建整套系統。

了解課程 →](https://rar.design/courses/figma)[![AI 覺醒設計應用攻略](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/63e96ac9-97ce-4dfc-be50-154f731f1cc9/e5ca45f0-206a-478f-af1a-96960be47d6a.jpg)

AI 覺醒設計應用攻略

SUBSCRIPTION

AI 覺醒設計應用攻略

AI 工具每週都在更新，我幫你過濾雜訊，只留對設計師真正有用的應用。

了解訂閱 →](https://rar.design/courses/aiwaken)

## 常見問題

### Q：一定要會寫程式、會用終端機嗎？

A：不用寫程式，規則檔是 markdown，prompt 是中文。終端機只在安裝 plugin 和授權時用到，之後都是對話。完全不想開終端機的話，Claude Desktop 接 Figma connector 也支援寫入畫布，規則檔的寫法一樣。

### Q：免費方案能用嗎？

A：Starter 方案每月只有 20 次工具呼叫，夠你確認流程能動，不夠日常使用。寫入畫布需要 Full seat 和檔案的編輯權限，等於要付費方案。beta 免費指的是寫入功能本身目前不另外計費，跟方案等級無關。

### Q：桌面版 MCP server 可以寫入嗎？

A：不行。寫入畫布只有遠端 server 支援，桌面版是讀取當前選取圖層用的。Figma 官方也建議多數人直接用遠端版，功能最完整。

### Q：AI 會不會把我的元件庫改壞？

A：它只能動你有編輯權的檔案，所以元件庫檔案不要給它編輯權，只在 DESIGN.md 放連結讓它查。工作檔裡用規則限制它只在 AI Sandbox 頁建東西、不改既有圖層，加上 Figma 的版本紀錄可以整頁還原，風險控制得住。

### Q：DESIGN.md 跟 Claude Design 用的 DESIGN.md 是同一個東西嗎？

A：同一種思路，內容不同。Claude Design 的 DESIGN.md 描述視覺系統長什麼樣，讓 AI 生成介面；這篇的 DESIGN.md 描述 Figma 檔案怎麼用，讓 AI 在正確的頁用正確的元件建畫面。兩份可以並存。

### Q：跟 Figma 自己的 agent 差在哪？

A：Figma 原生 agent 在畫布裡運作，你在 Figma 介面跟它對話；MCP 這條線是把 Claude Code 這類外部 agent 接進畫布，適合跟程式碼、外部資料和其他工具一起工作。兩者都吃 skills，可以並用。

### Q：之後會收費嗎？

A：會。Figma 官方明說寫入畫布會變成用量計費的付費功能，目前 beta 免費是為了研究 agent 行為該怎麼計入付費座位。現在建好的規則檔和流程之後照用，只是要開始算錢。