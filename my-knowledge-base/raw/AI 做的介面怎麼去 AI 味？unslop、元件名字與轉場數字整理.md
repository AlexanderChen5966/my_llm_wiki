---
title: "AI 做的介面怎麼去 AI 味？unslop、元件名字與轉場數字整理"
source: "https://www.aiposthub.com/ai-ui-unslop-four-x-posts-names-numbers-checklist/"
author:
  - "[[Philo]]"
published: 2026-11-10
created: 2026-11-10
description: "Damir Sokolovsky 的 AI 介面改造影片超過 200 萬次觀看，並做成免費 skill unslop。整理四則 X 貼文、九項檢查清單、9 次實驗與轉場數字，標明作者說法與限制。"
tags:
  - "clippings"
status: "inbox"
---
AI 做出來的介面常有固定的樣子：紫色漸層、表情符號當圖示、帶驚嘆號的文案、發光的圖表。X 上有四則貼文在談怎麼把這些去掉（原文用的字是 slop，這裡中文說「AI 味」）。我們把它們整理成同一個問題：怎麼跟 AI 說，它才不會做成那樣？先說結論（資料時間：2026 年 11 月 9–10 日，台北）：

- **看清楚改了什麼。** 設計師 Damir Sokolovsky 10 月 27 日貼的 AI 儀表板改造影片，超過 200 萬次觀看；10 月 30 日他把改法做成免費的 Claude skill「unslop」，GitHub 上有 1.2 萬多顆星，授權 CC BY 4.0。skill 把改法拆成九項，依序是間距、顏色、圖示、文案、字體、版面、細節、視覺、動態。
- **說出元件名字，或把行為寫清楚。** Andrew Gao 說先學元件名字，用 AI 做前端能強十倍（這是他的說法，我們沒找到獨立實驗）。我們用同一個待辦頁、Claude Opus 5.5 測了 9 次：只說「跳出視窗」得 3 分；指名元件，或用白話把行為寫清楚，都是 4 分，差在「點頭像」那一題。
- **寫出毫秒和像素。** Jonathan Wilke 推薦的 transitions.dev 免費版有具體數字，例如彈窗從 96% 放大、開 250 毫秒、關 150 毫秒。這些數字可以直接寫進給 AI 的指令。
- **做一次換名測試。** unslop 檢查表的最後一項：把產品名換成別家的，畫面還說得通，就是模板。

這四個做法來自作者自己的說法，加上我們的小實驗，都不是獨立評測。這篇是 AI郵報同主題 Reel 的完整整理，下面依序放上原文出處、可以直接複製的檢查清單、實驗設計與限制。

## 這四則 X 貼文是什麼？

| 作者與貼文 | 發文時間（台北） | 貼文在說什麼 | 觀看數 |
| --- | --- | --- | --- |
| Damir Sokolovsky（@heydamir） [10/27 貼文](https://x.com/heydamir/status/2115063463063998736?ref=aiposthub.com) | 2026/10/27 20:50 | 一支影片，文字只有一句：Unslopping a vibecoded dashboard（把一個 vibecoded 儀表板去 slop；AI郵報自譯） | 超過 200 萬 |
| Damir Sokolovsky [10/30 貼文](https://x.com/heydamir/status/2116158176336970008?ref=aiposthub.com) | 2026/10/30 21:20 | 說他把自己的 unslop 流程做成免費的 Claude skill，適用落地頁、儀表板和手機 app，並附上 GitHub 連結 | 約 64 萬 |
| Andrew Gao（@itsandrewgao） [2/28 貼文](https://x.com/itsandrewgao/status/2027579200635605058?ref=aiposthub.com) | 2026/2/28 10:58 | 一支影片；說只要學會不同 UI 元件叫什麼，就能讓 vibecoded 前端強十倍（原文 10x your vibecoded frontends）；並說 Opus 做出千篇一律的成品，是因為你會的詞只有 menu 和 button | 約 183 萬 |
| Jonathan Wilke（@jonathan\_wilke） [9/12 貼文](https://x.com/jonathan_wilke/status/2098801944756154391?ref=aiposthub.com) | 2026/9/12 23:52 | 推薦 transitions.dev，附連結卡、沒有影片；說在大家都上線 AI 粗糙成品的時候，這些小細節能讓產品突出 | 約 30.8 萬 |

另外，Andrew Gao 在 3/2 又補了一串更完整的建議（ [原文](https://x.com/itsandrewgao/status/2028296136281522226?ref=aiposthub.com) ，約 6.5 萬次觀看），下面會用到。觀看數取自 X 頁面顯示的數字，會隨時間增加；觀看數與星數只代表關注度，不代表內容正確或品質。

## 做法一：Damir 改了哪些地方？

Damir 在 10 月 27 日的影片裡，把一個 AI 做的儀表板一步步改掉。我們逐格看過影片，把看得到的改動整理成下表，並對到他的 skill 檢查表裡的項目。 **這張對照是我們整理的，作者沒有列出清單。**

![Damir 影片開頭的 AI 儀表板：紫色漸層、表情符號、驚嘆號文案、發光長條圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ui-unslop-d27_before.jpg)

改之前（影片開頭）

![Damir 影片結尾的儀表板：深灰色、線條圖示、細線圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ui-unslop-d27_after.jpg)

改之後（影片結尾）

圖：Damir Sokolovsky 10/27 X 貼文影片的開頭與結尾畫面（已裁切；影片屬作者，AI郵報擷取，原貼文連結見文末）

| 畫面上的改動（改之前 → 改之後） | 對到 unslop 的哪一項 |
| --- | --- |
| 背景與整體色調：紫色漸層 → 深灰色 | 顏色：表面用同一組中性色階，拿掉色相漸層、發光與彩色陰影 |
| 導覽與卡片標題前的表情符號 → 同一套線條圖示 | 圖示：當圖示用的 emoji 都換成同一套線條圖示（專案沒有就用 lucide），同一個尺寸與線寬 |
| 問候標題「Welcome back, Damir! 👋」→「October 2026」 | 文案：不用問候式標題，標題寫頁面或期間 |
| 「+12%!!」「+4.2% this month!!」→「+12% vs last month」「4.2% ↑ vs September」 | 文案：不用驚嘆號；變化要寫是跟什麼比 |
| 「PAID THIS MONTH」「NAVIGATION」全大寫加寬字距 →「Paid this month」「Navigation」 | 字體：句首大寫，不用全大寫加寬字距的標籤 |
| 「Search anything...」加發光的快捷鍵標籤 →「Search」加 ⌘ K 小按鍵框 | 文案：標籤要短；細節：快捷鍵做成小按鍵框 |
| 「PRO」「New ✨」徽章拿掉；收件匣的紅色「12」→ 安靜的灰色數字 | 文案：徽章除非產品真的需要就拿掉；細節：計數用安靜的小字 |
| 方案列表：表情符號加發光的數量膠囊 → 小色點加安靜的數字 | 圖示與細節：需要顏色的類別用小色點；數量用安靜的小字 |
| 發光的圓柱長條圖（週一到週日）→ 細線圖，加座標軸與月份 | 細節：圖表放在安靜的格線上、不用發光長條；趨勢用線 |

影片裡改完的畫面是作者自己的示範，沒有獨立評測，也不保證你的畫面照做會有同樣的結果。

10/30 貼文的影片換成另一個畫面：一個落地頁。改之前的標題是 Supercharge Your Invoicing With AI（用 AI 加速你的開發票作業；AI郵報自譯），改之後是 Invoices that chase themselves（會自己追款的發票；AI郵報自譯），標題下方改成一句說明產品做什麼的話，原本的發光色球換成一張以紙飛機為主角、標出「Paid $4,200」「Sent」「Reminder 2」等狀態的圖。改之前還有「Loved by 10,000+ teams worldwide!」這類無法查證的宣稱，改之後的這一格看不到。這些對應 unslop 的文案項目：不用 supercharge 這類誇飾詞、標題要說產品為誰做什麼、查不到的宣稱要拿掉。

![10/30 貼文影片開頭的落地頁：Supercharge Your Invoicing With AI](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ui-unslop-d30_before.jpg)

改之前（影片開頭）

![10/30 貼文影片結尾的落地頁：Invoices that chase themselves](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ui-unslop-d30_after.jpg)

改之後（影片結尾）

圖：Damir Sokolovsky 10/30 X 貼文影片的開頭與結尾畫面（已裁切；影片屬作者，AI郵報擷取）

## unslop 是什麼？怎麼用？

unslop 是 Damir 做的 skill，倉庫是 [github.com/heydamir/unslop](https://github.com/heydamir/unslop?ref=aiposthub.com) 。README 現在寫它給 Claude Code、Codex 和 Google Antigravity 使用（10/30 的貼文只說 Claude skill）。它處理用 Next.js、React、Vue、Svelte、Tailwind 或純 HTML 與 CSS 做的儀表板、app 和落地頁，只有網址時也能處理上線的網站。

### 安裝與使用（Claude Code）

```
git clone https://github.com/heydamir/unslop
mkdir -p ~/.claude/skills && cp -r unslop/skills/unslop ~/.claude/skills/
```

以上兩行出自 README。只想用在單一專案，就複製到該專案內的 `.claude/skills/` 。接著在 Claude Code 輸入 `/unslop this dashboard` 或 `/unslop the hero of https://example.com` ，也可以用自己的話說，讓 agent 自己挑到這個 skill（Codex 用 `$unslop` ）。安裝任何第三方 skill 之前，建議先自己讀過它的 SKILL.md 和附的腳本，這是我們的建議。

### 它的流程

下面是 [SKILL.md](https://github.com/heydamir/unslop/blob/main/skills/unslop/SKILL.md?ref=aiposthub.com) 的「Run it」章節，用中文整理：

1. **先看畫面。** 找出畫面的原始碼；能跑就截圖，判斷使用者實際看到的樣子。只有網址時，用內附腳本存下頁面再處理；只有截圖時，先重建成一個靜態 HTML 當原件。
2. **審查。** 依 references/signs.md 列出每一個 AI 味徵兆與它在哪裡。
3. **決定改多少。** 徵兆很多就跑完整流程；只剩少數就只改審查出的項目、沿用現有品牌；沒有問題就直接說沒有，最多建議兩三個小調整，你同意才套用。
4. **定方向，然後停下來。** 寫五行（產品、性格、世界、重點時刻、招牌細節），連同審查結果給你看， **等你回答才動手** ；你一開始就說「直接做」才略過等待。性格那一行不能寫 clean、modern、sleek、minimal、premium 這類形容所有模板的字。
5. **依序做九項。** 一項做完整個畫面，再做下一項。
6. **跑檢查表。**
7. **回報** 方向與每一項改了什麼。
8. **你要的話** ，另外做一支不到 10 秒的影片，記錄畫面一步步被改掉（需要 Playwright 與 ffmpeg）。

兩個要先知道的條件：

- **授權與費用：** 授權是 [CC BY 4.0](https://github.com/heydamir/unslop/blob/main/LICENSE?ref=aiposthub.com) ，可商用、可修改，要署名 Damir Sokolovsky 並連回倉庫。生成主視覺要另接圖像服務（README 列了 fal、Replicate、OpenAI images、Higgsfield），每次付費生成前會先顯示提示詞並詢問；README 說儀表板和 app 很少需要。
- **作者另有付費服務：** README 有一節 Hire the unslopper，說 Damir 可以 3 天幫你處理一個關鍵畫面。skill 本身免費。

## 可以直接拿去用的檢查清單（整理自 unslop）

下面是我們把 unslop 的 [SKILL.md](https://github.com/heydamir/unslop/blob/main/skills/unslop/SKILL.md?ref=aiposthub.com) 整理成中文的檢查清單。數字與規則都出自 SKILL.md（作者 Damir Sokolovsky，CC BY 4.0）；中文是我們的意譯，要精確內容請看原文。你可以拿它自己檢查一個畫面，也可以貼給 AI 當驗收條件。

| 項目 | 檢查什麼 |
| --- | --- |
| 1 間距 | 所有間距和尺寸都是 4 的倍數（小控制項內的微調可用 2）；同類的東西用同一個尺寸（列表每一列同高同間距、卡片同樣的內距）；群組內的間距小於群組之間；導覽列每列高 32–36、列與列之間 2–4；內距依密度：側欄與緊湊面板約 16、卡片 20–24、頁面邊緣 24–32。 |
| 2 顏色 | 背景、表面、邊線、文字都來自同一組帶一點色調的中性色階；文字只分主要、次要、弱化三層；只用一個強調色而且少用；強調色上的字要有 4.5 比 1 的對比；拿掉色相漸層、漸層文字、彩色光暈和彩色陰影、背景色塊與霓虹邊框；邊線用細線（文字色約 6–10% 透明度）。 |
| 3 圖示 | 當圖示用的 emoji 全換成同一套線條圖示（專案已有的優先，否則用 lucide）；一個尺寸（16 或 18px）、一種線寬（1.5）；圖示用文字的顏色；沒有資訊量的圖示直接拿掉；需要顏色的類別用小色點。 |
| 4 文案 | 標題用白話說這個產品為誰做什麼，換成別的產品就不成立；介面文字不用驚嘆號；不用「Welcome back, 名字 👋」，標題寫頁名或期間；標籤要短（Search，不是 Search anything...）；變化要寫跟什麼比（+12% vs last month）；短字串結尾不加句號；不用 supercharge、seamless、unlock、magic 這類誇飾詞，也不要處處寫 AI-powered；查不到的宣稱（例如 Loved by 10,000+ teams、10x faster）要先確認屬實才保留，否則拿掉；範例資料要像真的，不用 \[X\] 或 123,456；PRO、New ✨ 這類徽章除非產品真的需要就拿掉。 |
| 5 字體 | 依產品性格挑字體：介面一組，最多再配一個；字重只用 400 和 500（偶爾強調才用 600），階層靠大小與顏色；句首大寫，不用全大寫加寬字距；小字不要負字距；數字用 tabular-nums（等寬數字）、整個畫面格式一致；不用斜體強調。 |
| 6 版面 | 保留原本的區塊與順序，區塊內可以調整對齊與比例；app 和儀表板維持真實比例（側欄約 240–280、內容流動），不要縮成置中的窄欄；落地頁首屏一個主要動作、最多再一個次要動作；導覽保持安靜（Logo、既有連結、一個按鈕）。 |
| 7 細節 | 圓角要成套（內圓角＝外圓角減去它們之間的內距，不小於 4；控制項 8–10、卡片 12–16、面板 14–20）；快捷鍵做成小按鍵框；計數與附註改成安靜的小字；圖表放在安靜的格線上，一張主圖表最大、其他用不同類型且更小；一張圖表一個強調色；數字卡只放標籤、大數字、和跟什麼比的變化；hover 與 focus 狀態留在同一組顏色內，鍵盤使用者要看得到 focus 框。 |
| 8 視覺 | 拿掉只是填版面的裝飾（光滑色球、星雲、浮動方塊、發光格線、全像腦袋與機器人）；落地頁首屏的視覺要能說明這個產品的某件真事；儀表板與 app 通常不需要生成圖片，重點來自產品本身的內容（畫得用心的圖表、排得好看的數字）。 |
| 9 動態 | 拿掉沒有理由的循環動畫（不是即時狀態卻用 animate-pulse、animate-ping、微光、飄浮、視差）；不用 transition-all、彈跳彈簧、卡片 hover 放大；只轉換 color、background、border、opacity、transform，而且要寫明名稱；不要動畫 width、height、top、left、margin；整個畫面一條緩動曲線、少數幾個時間長度；載入時最多一個進場動畫；選單是淡入並從開啟處上升幾個像素。 |
| 最後：換名測試 | 把產品名換成別家的，畫面還說得通嗎？如果還說得通，就要在文案、視覺和招牌細節上把方向推得更明確。 |

SKILL.md 另有五條「不要做」：不改產品名稱（名稱裡有 AI 也不改）；不改變產品是什麼，功能、內容與區塊順序不動；不用新的裝飾取代被拿掉的裝飾，也不拿一種流行換另一種（玻璃、顆粒、雜訊、霓虹、bento 格）；不發明功能、宣稱或資料；不要每個產品都套同一種外觀。

## 做法二：說出元件名字——Andrew Gao 怎麼說？我們測了什麼？

### 他說了什麼

Andrew Gao 在 [2/28 的貼文](https://x.com/itsandrewgao/status/2027579200635605058?ref=aiposthub.com) 說，只要學會不同 UI 元件叫什麼，就能讓 vibecoded 前端「強十倍」（原文 10x your vibecoded frontends；AI郵報自譯），並說 Opus 做出千篇一律的成品，是因為你會的詞只有 menu 和 button。 **「十倍」是他的說法，我們搜尋後沒有找到驗證這個倍數的獨立實驗** （只限我們的搜尋範圍，不代表不存在）。

3/2 他又補了 [一串更完整的貼文](https://x.com/itsandrewgao/status/2028296136281522226?ref=aiposthub.com) ，講原因和六個建議。原因有兩個：一是模型做前端時，會預設走向訓練資料的「中間／平均」樣式，它有能力做出不同的樣式，但你得開口要求；二是多數人不擅長想出設計，也不擅長精準描述，所以要像對待一般工程工作一樣，給詳細的規格。這是他的解釋，沒有附實驗。他的六個建議是：

1. 給 agent 你喜歡的設計截圖（你不一定知道怎麼描述，agent 知道）；他列的靈感來源有 Behance、Dribbble、Mobbin（Mobbin 付費）。
2. 請 agent 先提幾個方案，用來帶出不同方向，來回討論。
3. 禁用某些傾向：不要 Inter／Roboto、不要 shadcn（他自己標註有爭議）、不要漸層、不要 emoji。
4. 鼓勵 agent 大膽、不要保守；他認為模型在強化學習與微調時被教成做保守的選擇（他寫的是 i think，屬於推測）。
5. 接上 Figma MCP；他說先在 Figma 做出想要的樣子，結果最好。
6. 盡量選有視覺能力的 agent。

他的結論是：多數人嚴重低估了 agent 做前端設計的能力。第 3 條和 unslop 的審查項目有重疊（漸層、emoji），這是我們的觀察。

### Component Gallery：中文的「彈窗」可能是哪幾種？

[Component Gallery](https://component.gallery/?ref=aiposthub.com) 是整理各家設計系統 UI 元件的網站，首頁寫著 60 種元件、95 個設計系統、2,671 個範例。中文說的「彈窗」，在 Gallery 的元件頁裡至少對得到四種（這個對照是我們整理的，Gallery 沒有「彈窗」這個詞）。下表是各元件頁的定義（中文為意譯）與頁面列出的別名：

| 英文名 | 別名（Gallery 頁面列出） | Gallery 的定義（意譯） |
| --- | --- | --- |
| [Modal](https://component.gallery/components/modal/?ref=aiposthub.com) | Dialog、Popup、Modal window | 出現在其他內容上方；使用者要先操作它，才能回到下面的內容 |
| [Popover](https://component.gallery/components/popover/?ref=aiposthub.com) | 頁面沒列別名 | 從另一個元素彈出、蓋在內容上方；和 tooltip 不同在於通常用點擊而不是滑過觸發，而且可以放可互動的元素 |
| [Tooltip](https://component.gallery/components/tooltip/?ref=aiposthub.com) | Toggletip | 顯示對某個元素的說明或補充資訊，通常是滑過觸發，也可以用點擊或輕觸 |
| [Toast](https://component.gallery/components/toast/?ref=aiposthub.com) | Snackbar | 一種出現在其他內容上層的提醒，外觀像手機或電腦的推播通知 |

本文把 Toast 統一叫「通知」。X 貼文和一些中文討論會把 Toast 或 Tooltip 都叫「提示」，兩種元件容易混在一起。

### 我們的實驗：9 次，3 分對 4 分

我們想知道：同一個需求，只說「跳出視窗」和指名元件，AI 做出來的差別有多大？設計如下（11 月 8 日完成）：

- **需求：** 做一個「團隊待辦清單」單頁網頁（直式約 540×960、三個示範任務、三位虛構負責人），有四個功能：新增任務、任務說明、儲存後提醒、點頭像看負責人。
- **模型與環境：** Claude Opus 5.5，在 claude.ai 開「無痕」新對話，投入程度「中」、思考「自動」；每次一個全新對話，提示詞逐字照稿、沒有另加指令。（原本計畫用命令列自動執行，但 claude -p 顯示未登入，所以改用網頁版無痕對話。）
- **次數：** 三種寫法各 3 次，共 9 次；每次只取回覆裡的 HTML 程式碼，沒有修改。
- **判分：** 四個功能各 1 分，共 4 分，判準在跑之前就寫死。用 Playwright 在 540×960 視窗依序操作每一頁（滑到說明圖示、點頭像、按新增、儲存、復原），判分以逐次看截圖為準。

| 功能 | 得 1 分的條件 |
| --- | --- |
| ① 新增任務（Modal 型） | 按了之後有東西疊在頁面上、背景有遮罩使其餘頁面點不到、能用儲存或取消回來 |
| ② 說明圖示（Tooltip 型） | 滑過圖示才出現、只有說明文字、滑開就消失 |
| ③ 儲存後提醒（Toast 型） | 儲存後疊在頁面上方出現「已新增」、不擋其他操作、按「復原」真的拿掉新任務 |
| ④ 點頭像（Popover 型） | 用點的才出現（滑過不出現）、出現在頭像附近的小面板而且沒有全頁遮罩、「傳訊息」按鈕可以按 |

三種寫法只有四個功能的描述不同，其他文字（需求、格式、三個示範任務）完全相同。四個功能的原話如下（提示詞是我們自己寫的）：

| 功能 | A：只說「跳出視窗」 | B：指名元件 | C：用白話寫行為（依 Gallery 的定義） |
| --- | --- | --- | --- |
| ① 新增任務 | 按「新增任務」， **跳出視窗** ，可以填任務名稱和期限。 | 按「新增任務」，開一個 **Modal** ，可以填任務名稱和期限。 | 按「新增任務」，出現一個 **疊在其他內容上方的表單，使用者要先操作它（儲存或取消），才能回到下面的頁面** ；欄位是任務名稱和期限。 |
| ② 說明圖示 | 滑到圖示上， **跳出視窗** 顯示任務說明。 | 滑到圖示上，顯示 **Tooltip** ，內容是任務說明。 | 滑到圖示上 **才出現一小塊說明** ，內容是任務說明。 |
| ③ 儲存後 | 儲存新任務後， **跳出視窗** 告訴使用者「已新增」，並且可以按「復原」。 | 儲存新任務後，顯示 **Toast** 「已新增」，並且可以按「復原」。 | 儲存新任務後， **疊在其他內容之上出現一則提醒，外觀像手機或電腦的推播通知** ，內容是「已新增」，並且可以按「復原」。 |
| ④ 點頭像 | 點負責人的頭像， **跳出視窗** 顯示負責人的姓名、職稱，和一個「傳訊息」按鈕。 | 點負責人的頭像，開一個 **Popover** ，內容是負責人的姓名、職稱，和一個「傳訊息」按鈕。 | 點負責人的頭像， **從頭像旁邊冒出一個疊在其他內容上方的小面板（用點的觸發，不是滑過去）** ，裡面是姓名、職稱，和一個可以按的「傳訊息」按鈕。 |

### 結果

| 版本 | 第 1 次 | 第 2 次 | 第 3 次 | 中位數 |
| --- | --- | --- | --- | --- |
| A 只說「跳出視窗」 | 3 | 3 | 3 | 3 |
| B 指名元件 | 4 | 4 | 4 | 4 |
| C 用白話寫行為 | 4 | 4 | 4 | 4 |

四個功能裡，新增、說明、提醒三項九次都做對； **唯一的差別在「點頭像」** ：A 版三次都把點頭像做成蓋住整頁的置中對話窗，並讓其餘頁面變暗（Modal 型）；B、C 兩版的六次都是貼在頭像旁邊的小面板，沒有遮罩（Popover 型）。

![A 版：點頭像後出現置中的對話窗，其餘頁面變暗](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ui-unslop-exp_A.jpg)

A：只說「跳出視窗」，點頭像：置中的對話窗，其餘頁面變暗

![B 版：點頭像後在頭像下方出現小面板](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ui-unslop-exp_B.jpg)

B：指名 Popover，點頭像：貼在頭像下方的小面板

![C 版：點頭像後在頭像旁出現小面板](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ui-unslop-exp_C.jpg)

C：白話描述，點頭像：貼在頭像旁的小面板

圖：本站實驗截圖（2026/11/8；各版第 1 次的生成頁面，Playwright 540×960 視窗，AI郵報自行擷取）

### 怎麼讀這個結果

- 我們跑之前就訂了門檻：B 的中位數要比 A 多 2 分以上，才把兩者的對照當作證明來用；實際只差 1 分， **沒有達到門檻** 。
- 用白話把行為寫清楚（C 版）也是 4 分，和指名元件一樣；所以名字不是唯一的辦法。
- A 版三次的程式碼裡，CSS 類別也寫成 tooltip、toast、dialog，也就是模型本來就會用這些名字；只有點頭像那一題選了 dialog。九次都沒有用 alert()，也沒有 JavaScript 錯誤。
- **限制：** 只有一個需求、一個模型（Claude Opus 5.5）、各 3 次；判準是我們自己訂的四條。A 版三次結果相同，但不能推論「所有 AI 都會這樣」，也不能用它證明或否定「十倍」。

### 你可以怎麼寫

記得元件名字，就像 B 版那樣直接寫（Modal、Tooltip、Toast、Popover）；不確定名字，就像 C 版那樣把行為寫清楚：它疊在哪裡、用什麼觸發（點或滑過）、要不要先處理才能回去、會不會擋住其他操作。這是我們從這次實驗整理的寫法，不是 Andrew Gao 的說法。

## 做法三：寫出毫秒和像素——Jonathan Wilke 推薦的 transitions.dev

Jonathan Wilke 在 [9/12 貼了 transitions.dev 的連結](https://x.com/jonathan_wilke/status/2098801944756154391?ref=aiposthub.com) （連結卡，沒有影片），說這些小動畫讓介面感覺更好；在大家都上線 AI 粗糙成品的時候，這些小細節能讓產品突出，前提是別用紫色漸層、別在每個框外面都加邊框。（原文含粗話，這裡意譯。）

[transitions.dev](https://transitions.dev/?ref=aiposthub.com) 的首頁說明是：一組網頁 app 最基本的 UI 轉場，可以複製貼上，也可以透過 skill 交給你的 coding agent 用；首頁寫「43+ crafted transitions」，作者是 Jakub Antalik。 [授權](https://github.com/Jakubantalik/transitions.dev/blob/main/LICENSE?ref=aiposthub.com) 寫明：免費的轉場與 skill 可以用在個人與商業專案、可以修改；不能把轉場庫轉包成競品；網站設計、品牌與 logo 不在授權內。另有付費的 Pro 與 Business 方案，涵蓋 Pro 轉場與付費的 Motion Agent 功能。

### 免費版有哪些數字

下面是我們從 transitions.dev 首頁的免費版 CSS 讀到的數字（11/9 讀取）。這些是官方範例的設定值，不是業界標準。

| 轉場 | 動作 | 開啟（毫秒） | 關閉（毫秒） |
| --- | --- | --- | --- |
| 彈窗（Modal） | 從 96% 大小（scale 0.96）且透明，放大到 100% 並淡入 | 250 | 150 |
| 選單（Dropdown） | 從 97% 放大到 100%；可設定從哪個點展開（預設上方中央） | 250 | 150 |
| 通知（Toast） | 從定位下方 16 像素處、模糊 2 像素、97% 大小，升到定位並變清楚、淡入 | 350 | 250 |

三組有同一個規律：開啟比關閉慢（250 對 150、350 對 250），而且三組都用同一條緩動曲線 `cubic-bezier(0.22, 1, 0.36, 1)` 。官方的 Modal、選單與 Toast 程式碼片段也都附了系統設定「減少動態」（prefers-reduced-motion）時關掉轉場的寫法。

### 給 AI 的寫法範例

把前面的數字寫進指令，大概像這樣（這是我們用上表的數字組成的範例，不是 transitions.dev 或作者的原文）：

> 彈窗開啟時從 96% 放大並淡入，250 毫秒；關閉 150 毫秒。通知從下方升起 16 像素，加 2 像素模糊，開啟 350 毫秒、關閉 250 毫秒。曲線用 cubic-bezier(0.22, 1, 0.36, 1)。使用者系統開啟「減少動態」時，關掉這些轉場。

unslop 的動態項目也要求整個畫面用一條緩動曲線、少數幾個時間長度（它的具體數值放在自己的 tokens 檔，不是上表）。我們另外把同一個設定頁並排錄成兩種：一邊「直接出現」，一邊「加轉場」（轉場是免費版 CSS 原樣）。 **這只是並排展示：沒有測使用者比較喜歡哪個，也沒有和 Tailwind 或 AI 自己生成的轉場比較。** 所以「直接給毫秒和像素」是我們的建議，目的是讓結果可以重現、可以調整，不是證明比較好看。

## 做法四：做一次換名測試

unslop 檢查表的最後一項是 Template check，問的是 would the screen still fit?（這個畫面還說得通嗎？AI郵報自譯）：把畫面上的產品名換成另一個產品的，如果標題、文案、圖還是說得通，這個畫面就可能是模板。 [SKILL.md](https://github.com/heydamir/unslop/blob/main/skills/unslop/SKILL.md?ref=aiposthub.com) 開頭也寫：一個乾淨、但可以屬於任何產品的畫面，是下一個模板；檢查結果若是「還說得通」，就要在文案、視覺和招牌細節上把方向推得更明確。「換名測試」這個中文名稱是我們取的。

做法（我們的整理）：

1. 把畫面上的產品名，換成另一個同類產品的名字。
2. 逐項看標題、副標、按鈕文字和圖：換了名字還成立的，就是通用的。
3. 把通用的改成只有這個產品才會說的話，例如它具體做的事。

skill 的「方向」那一步，最後一行叫招牌細節：一個只有這個產品才會有的小細節，而且一致地使用。SKILL.md 舉的例子有：監控工具的即時狀態點、開發工具的按鍵框、教學 app 的手繪底線、外送 app 的路線線條。

舉例（我們的舉例，不是作者的說法）：Damir 的落地頁範例，改之前的標題 Supercharge Your Invoicing With AI 是誇飾詞，換成任何開發票的產品名都說得通；改之後的標題和說明（寄出每張發票、追催逾期的、款項入帳時通知你）寫的是產品具體做的事，比誇飾詞更具體。

## 這些做法的限制

- Damir 的前後對照是他自己的示範，unslop 是他自己的工具，沒有獨立評測；觀看數與星數代表關注度，不代表品質。
- 影片裡的改動清單是我們看畫面整理的，作者沒有列清單；畫面中還有表裡沒列的改動。
- 影片和 skill 不完全一致：影片裡產品名從 EmberAI 變成 Ember，而 SKILL.md 的「不要做」寫明不改產品名稱（名稱裡有 AI 也不改）。影片是 10/27 貼的，GitHub 倉庫是 10/29 才建立。
- 「十倍」是 Andrew Gao 的說法；我們沒找到獨立實驗，我們自己的 9 次實驗只差 1 分。
- 我們的實驗只有一個需求、一個模型、各 3 次，判準由我們訂；結果不能推論到其他 AI。
- transitions.dev 的數字是官方免費版的設定值；我們只做並排展示，沒有測使用者喜好。
- 費用與授權：unslop 是 CC BY 4.0、要署名，生成主視覺另外付費，作者另提供付費設計服務；transitions.dev 另有付費方案。
- 影片畫面屬各作者，本文只截取短段畫面並標明出處；「去 AI 味」是中文轉述，原文用 slop 與 unslopping，引用的英文都標了「AI郵報自譯」。
- 資料時間是 2026 年 11 月 9–10 日（台北）；之後 unslop 或 transitions.dev 的內容可能更新。

## 常見問題

### 「去 AI 味」是什麼意思？

這是中文轉述；Damir 的原文用 slop 與 unslopping。unslop 的 SKILL.md 描述的徵兆有：紫色漸層、發光、漸層文字、表情符號當標籤、「New ✨」徽章、光滑的色球、全大寫加寬字距的標籤、不必要的脈動動畫、不均勻的間距。

### unslop 免費嗎？可以商用嗎？

skill 本身免費，授權 CC BY 4.0，可商用、可修改，要署名 Damir Sokolovsky 並連回倉庫。生成主視覺要另接圖像服務，每次付費前會先詢問；作者另外提供付費的設計服務。

### unslop 只能用在 Claude 嗎？

10/30 的貼文說它是 Claude skill；README 現在寫它給 Claude Code、Codex 和 Google Antigravity 使用，各有安裝方式。

### 一定要背元件名字嗎？

我們的 9 次實驗裡，用白話把行為寫清楚（C 版）和指名元件（B 版）都是 4 分，只說「跳出視窗」是 3 分。只有一個需求、一個模型、各 3 次，所以只能代表這一次。

### 動畫的數字一定要照 transitions.dev 嗎？

不用。那些是官方免費版的設定值，不是標準。重點是給具體的毫秒、像素和曲線，而不是「順一點」；這是我們的建議，沒有測過使用者的偏好。

### 這些做法有用嗎？

Damir 的改造是作者的示範；我們的實驗顯示，指名元件或把行為寫清楚，能避開「點頭像被做成全頁對話窗」這個差別；轉場只做了並排展示。整體沒有獨立評測，建議你拿一個自己的畫面，用同一份檢查清單前後比較。

## 資料來源

本文資料時間為 2026 年 11 月 9–10 日（台北）；X 的數字會隨時間變動。影片畫面屬各作者，頁面與截圖屬各網站；英文引述均為短句並標「AI郵報自譯」。本文為整理與查證，不是設計服務或購買建議。

- [Damir Sokolovsky：X 貼文「Unslopping a vibecoded dashboard」（2026/10/27）](https://x.com/heydamir/status/2115063463063998736?ref=aiposthub.com)
- [Damir Sokolovsky：X 貼文「I turned my unslopping process into a free Claude skill」（2026/10/30）](https://x.com/heydamir/status/2116158176336970008?ref=aiposthub.com)
- [GitHub：heydamir/unslop（README）](https://github.com/heydamir/unslop?ref=aiposthub.com)
- [GitHub：unslop 的 SKILL.md](https://github.com/heydamir/unslop/blob/main/skills/unslop/SKILL.md?ref=aiposthub.com)
- [GitHub：unslop 的 LICENSE（CC BY 4.0）](https://github.com/heydamir/unslop/blob/main/LICENSE?ref=aiposthub.com)
- [Andrew Gao：X 貼文（2026/2/28）](https://x.com/itsandrewgao/status/2027579200635605058?ref=aiposthub.com)
- [Andrew Gao：X 貼文，前端 vibecoding 建議（2026/3/2）](https://x.com/itsandrewgao/status/2028296136281522226?ref=aiposthub.com)
- [Component Gallery 首頁](https://component.gallery/?ref=aiposthub.com)
- [Component Gallery：Modal](https://component.gallery/components/modal/?ref=aiposthub.com)
- [Component Gallery：Popover](https://component.gallery/components/popover/?ref=aiposthub.com)
- [Component Gallery：Tooltip](https://component.gallery/components/tooltip/?ref=aiposthub.com)
- [Component Gallery：Toast](https://component.gallery/components/toast/?ref=aiposthub.com)
- [Jonathan Wilke：X 貼文（2026/9/12）](https://x.com/jonathan_wilke/status/2098801944756154391?ref=aiposthub.com)
- [transitions.dev 首頁](https://transitions.dev/?ref=aiposthub.com)
- [GitHub：Jakubantalik/transitions.dev](https://github.com/Jakubantalik/transitions.dev?ref=aiposthub.com)
- [GitHub：transitions.dev 的 LICENSE](https://github.com/Jakubantalik/transitions.dev/blob/main/LICENSE?ref=aiposthub.com)
- AI郵報實驗紀錄：待辦頁三種寫法各 3 次的提示詞、九個生成頁面與逐次判分（2026/11/8）