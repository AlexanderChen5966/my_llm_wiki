---
title: "Claude code 新手篇 9： Artifacts — 把產出變成一個會更新的網址 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10423413"
author:
  - "[[iThome]]"
published:
created: 2026-10-13
description: "1. 傳出去的那一份，不會再更新 你應該有遇到過這種事：一份報告傳給同事，十分鐘後發現有個數字錯了，改完再傳一次。檔名從「報告.html」變成「報告_最終版.html」，再來是「報告_最終版_V2.h..."
tags:
  - "clippings"
status: "inbox"
---
1

## Claude code 新手篇 9： Artifacts — 把產出變成一個會更新的網址

- 分享至

## 1\. 傳出去的那一份，不會再更新

你應該有遇到過這種事：一份報告傳給同事，十分鐘後發現有個數字錯了，改完再傳一次。檔名從「報告.html」變成「報告\_最終版.html」，再來是「報告\_最終版\_V2.html」。對方手上同時躺著三份，哪一份才是最新的，只有你知道。

我自己也沒好到哪裡去。寫這篇之前，我回頭翻了自己做過的東西：同一份系統交接書，就有三份，名字一模一樣。（這三份第 5 節會再講。）

G6 教你讓 Claude 用 HTML 把東西畫給你看。那篇提到：東西產完後要怎麼看？直接用瀏覽器打開那個檔案就好，點兩下打開、看完就關掉。

給自己看，這樣就夠了。但只要東西要給別人看，問題就來了：檔案只在你的電腦裡，要給人只能傳檔案；傳出去之後你又改了，對方手上那份就是舊的，你只能再傳一次。

這篇講的 artifact，就是把這件事換掉： **不傳檔案，傳一個網址。你改了，對方打開就是最新的。**

---

## 2\. Artifact 是什麼

一句話： **Claude Code 把它做出來的東西，發佈成 claude.ai 上的一個網址。**

> **An artifact is a live, interactive web page that Claude Code publishes from your session to a private URL on claude.ai. You open it in a browser, and it updates in place as the session continues.**
> 
> artifact 是 Claude Code 從你的對話發佈到 claude.ai 私人網址的網頁。用瀏覽器打開，對話繼續進行時，它會原地更新。  
> —— [Claude Code 官方文件〈Share session output as artifacts〉](https://code.claude.com/docs/en/artifacts)

拆開來是三件事：

- **一開始只有你看得到。** 要給別人看，你自己按分享。
- **原地更新。** 它改了內容重新發佈，網址不變，開著網頁的人直接看到新的。
- **每次發佈都存成一個版本。** 你可以決定別人看到的是哪一版。

跟 G6 的本機 HTML 放在一起比：

|  | G6 的 HTML 檔 | artifact |
| --- | --- | --- |
| 放在哪 | 你的電腦 | claude.ai 上的一個網址 |
| 誰看得到 | 拿到檔案的人 | 一開始只有你，你分享給誰，誰就看得到 |
| 內容改了之後 | 要重傳 | 同一個網址，對方直接看到新的 |
| 用手機看 | 要先把檔案傳到手機 | 點網址就好 |
| 需要什麼 | 什麼都不用 | Pro 以上的方案，用 `/login` 登入 claude.ai 帳號 |

> 💡 **用 API key 的人不能用。** artifact 一定要用 claude.ai 帳號登入。用 API key、或透過 Bedrock、Vertex 這類雲端平台用 Claude Code 的，都發佈不了 —— 叫它做的時候，它會改成在你電腦裡存一個 HTML 檔，或直接跟你說沒辦法發佈。

---

## 3\. 它能幫你做什麼

先講好處。

我從 7 月開始用，到 10 月初大約做了 37 個。回頭看，用法大概是這幾種：

| 用法 | 我做過的 | 為什麼做成網頁 |
| --- | --- | --- |
| 做完要給人看的報告 | 一次資料庫鎖死事故的調查報告、模型建置的進度會報、系統交接書 | 有圖、有表、有時間軸，傳網址比貼一大段文字好讀 |
| 把方案攤開來比 | 刷題系統的視覺方向、幾組配色比較、系統的 Wireframe | 三個方案並排，一眼看出差別 |
| 進度看板 | 刷題系統的開發進度 | 長任務跑的時候讓它邊做邊更新，我想確認進度時點開就知道做到哪 |
| 可以動的小工具 | 生活費上的試算、記帳本、旅行行程規劃 | 有輸入框、有滑桿，數字一改結果跟著變 |
| 教學用的圖解 | IV（訊息值）的數學定義圖解 | 公式配圖，比一段推導好懂 |

官方對「什麼時候用」的說法只有一句：

> **Use an artifact when terminal text is the wrong medium for what Claude produced.**
> 
> 終端機的文字不適合呈現它做出來的東西時，就用 artifact。

翻成我的判準： **這份東西要給別人看，或是我會回頭看很多次。** 剛好接得上 G6 —— G6 說「產一次、看很多次」的用 HTML；再加上「要給別人看」，就做成 artifact。

---

## 4\. 怎麼叫它做

不用學指令，一句話就夠。我拿系列地圖試：

```bash
把 medium/claude-code-series-map.md 的 G1 到 G20 做成一頁路線圖 artifact，分四段
```

或直接描述你想看到什麼：

```markdown
把這三個方案做成一頁比較，並排放，每個下面寫一句優缺點
```

做好之後，它會印出網址，點開就是那一頁。

![https://ithelp.ithome.com.tw/upload/images/20261011/201827960ekE8ZE6Rd.png](https://ithelp.ithome.com.tw/upload/images/20261011/201827960ekE8ZE6Rd.png)

在終端機裡用的話，還多了幾個方便的地方：

- 瀏覽器會自動打開那一頁
- 輸入框下面多一個 `⧉` 小標，顯示這個對話做了哪些 artifact
- 之後想再打開，按 `Ctrl+]` ，會打開這個對話最近做的那一個

**它也可能自己決定做。** 官方寫明，產出適合做成網頁時，Claude 可能自己發佈。所以哪天你只是請它整理資料，瀏覽器突然跳出一頁，不用緊張 —— 那就是它幫你做的～

做好的東西都收在 claude.ai 的「成品」頁（英文介面叫 Artifacts，桌面版左邊欄也有）。在 claude.ai 對話裡做的、在 Claude Code 裡做的，都在同一個地方。

> 💡 **接回 G8.5：發佈會不會先問你？** 你自己叫它做的新 artifact，發佈時不會跳詢問，也不經 auto mode 審核 —— 因為一開始只有你看得到。在 Plan Mode 裡，第一次發佈會先問你。

> 💡 **不想讓瀏覽器自己跳出來？** 在環境變數設 `CLAUDE_CODE_ARTIFACT_AUTO_OPEN=0` 。想整個關掉 artifact，到 `/config` 把 **Artifacts** 那一列關掉。

> 💡 **想要每一頁都用你的配色？** 把顏色、字型、間距寫進專案的 `CLAUDE.md` （G3 那個檔案）。官方說 Claude 做 artifact 時會先找專案裡的設計設定，而且你寫的優先於它自己的選擇。

---

## 5\. 更新：同一個網址，但換對話要給它網址

要改內容，直接講就可以：

```markdown
在摘要下面加一個各區域的細項，然後重新發佈
```

它改完會發佈到 **同一個網址** 。對方如果開著網頁，會直接看到新的內容，你不用再傳一次。

每次發佈都會存成一個版本。網頁上方的 **Share** 選單裡，可以選讓別人看哪一版 —— 改到一半還不想給人看，就先讓別人停在上一版。

### 注意的地方：換了對話，它會做一個新的

回到開頭那三份交接書：前兩份隔了 6 天，內容 98% 相同；第三份是同一天稍晚、內容多了四成的版本。

不管當初是不是故意的，結果都一樣：要找最新的那份，得一份一份點開來看。

換了對話，就很容易變成這樣。官方寫得很清楚：

> **Without either, a new session creates a new artifact instead of updating one.**
> 
> 沒有給網址、也沒有用 `/artifacts` 附加，新的對話會建立一個新的 artifact，而不是更新原本那個。

所以換了對話之後要改，有兩個方法：

1. **把網址貼給它：** 「更新 [https://claude.ai/code/artifact/](https://claude.ai/code/artifact/) …，換成今天的數字」
2. **用 `/artifacts` 附加：** 它會列出你所有的 artifact，選一個按 `Enter` ，就接到現在的對話裡。

`/artifacts` 還有兩個鍵： `o` 用瀏覽器打開， `c` 複製連結。清單是從你的 claude.ai 帳號讀的，所以就算 `/clear` 過、網址早就捲不見了，一樣找得到。

![https://ithelp.ithome.com.tw/upload/images/20261011/20182796yAUoFNy2bi.png](https://ithelp.ithome.com.tw/upload/images/20261011/20182796yAUoFNy2bi.png)

> 💡 **用 VS Code 的人：** 我在 VS Code 裡打 `/artifacts` ，選單裡找不到它，這個指令要在終端機版用。在 VS Code 裡，就到「成品」頁找到那一頁，把網址貼給它。

> 💡 **跟 G5 的交接檔不一樣。** G5 的交接檔是留給 **下一個對話裡的 Claude** 看的；artifact 是給 **人** 看的網址。換對話時，交接檔讓 Claude 知道做到哪，artifact 的網址讓它知道要改哪一頁。

---

## 6\. 分享：「知道連結的任何人」就是任何人

要給別人看，打開那一頁，按右上角的 **分享** （Share）。

![https://ithelp.ithome.com.tw/upload/images/20261011/201827968E9IihR0Qi.png](https://ithelp.ithome.com.tw/upload/images/20261011/201827968E9IihR0Qi.png)

選單裡有兩件事要決定。

**第一，一般存取權：除了你邀請的人，其他人能不能打開。**

| 方案 | 可以選 |
| --- | --- |
| Pro、Max | 「僅限您」，或「知道連結的任何人」 |
| Team、Enterprise | 「只有受邀的人」、「整個組織」，或「知道連結的任何人」（要管理員打開對外分享） |

選「知道連結的任何人」， **對方不用登入就打得開** —— 我把路線圖那頁打開分享，用無痕視窗試過，直接就看得到。

**第二，邀請特定的人：** 在最上面的欄位打對方的 email（目前是 beta），再選他能做什麼：

| 權限 | 能做什麼 |
| --- | --- |
| 可檢視 | 打開來看、看留言 |
| 留言者 | 再加上可以留言、下載頁面提供的檔案 |
| 可編輯 | 再加上可以修改 |

被邀請的人要用你填的那個 email 登入 Claude 才打得開。只想給特定幾個人看，用邀請比用連結安全 —— 邀請只對那個人有效，連結則是傳到哪、誰就打得開。

公開之後，Claude 在每個對話裡第一次要改它之前，會先問你一聲。

> ⚠️ **公司的東西，別選「知道連結的任何人」。** 網址一旦轉出去，你就管不到誰在看了。工作上的報告、客戶的資料，分享前先看公司的規定。

> 💡 **Team、Enterprise 多了什麼？** 分享給組織內的人之後，看的人在頁面上留言時，可以按 **Send to Claude** 、或在留言裡寫 `@claude` ，你的 Claude 會讀那則留言、回覆，需要的話直接改頁面。個人方案邀請來的人可以留言，但官方寫明他們不能在留言裡叫 Claude。

---

## 7\. 給人看，還是給人用：四個樣板

到這裡講的，都是 Claude 寫好的一頁網頁：它做、它改，別人負責看。

9 月起，artifact 多了幾個 **樣板** 。打開「成品」頁，上面那排 **Make something new** 就是：Docs、Slides、Design，還有一個還在 beta 的 Dashboard。

![https://ithelp.ithome.com.tw/upload/images/20261011/201827965HE3l5J130.png](https://ithelp.ithome.com.tw/upload/images/20261011/201827965HE3l5J130.png)

它們跟一般 artifact 最大的差別： **會在自己的編輯器裡打開，你可以直接動手改，也可以匯出成 Word、PowerPoint 帶走。**

|  | 一般 artifact | Docs | Slides | Design |
| --- | --- | --- | --- | --- |
| 做什麼 | Claude 寫的一頁網頁 | 文件 | 簡報 | 視覺稿、原型、landing page |
| 怎麼改 | 請 Claude 改 | 直接打字，可以多人同時改 | 一張一張直接改，可以直接報告 | 在畫布上直接改 |
| 怎麼帶走 | 傳網址 | Word、PDF、Markdown、Google Docs | PowerPoint、PDF、Google Slides | PDF、PowerPoint、HTML，單一畫板可存 PNG |
| 在 Claude Code 怎麼叫 | 「做成 artifact」 | 「寫成一份文件」 | `/slides` | `/design` |

一句話分辨：

> **只要看，用一般 artifact；要一起改、要帶走，用樣板。** 要人讀的 → Docs，要上台講的 → Slides，要先看長相的 → Design。

Docs 在 Claude Code 裡不是指令，是一個 connector： `/mcp` 裡看得到 `claude.ai Claude Docs` ，就代表連上了。Dashboard 要先接資料來源（例如 Salesforce 這類已連接的 app），會碰到 G11 的 MCP，到時候再講。

> ⚠️ **Docs 的分享有一點不一樣。** Pro、Max 的 Docs 可以用連結分享，對方要有 Claude 帳號才打得開；而且 Docs **不能用 email 邀請** ，任何方案都不行。

### 我實際跑了一次

寫這篇之前，三個我各跑了一次（在桌面版）：

- **Docs：** 請它把 G9 的寫作計畫寫成文件
- **Slides：** 請它用系列地圖，做一份給學生的 10 分鐘導覽
- **Design：** 請它畫 G9 的社群圖卡，三個方向

幾個觀察：

**1\. 三個都開在自己的編輯器裡。** Docs 像一般的文件編輯器，有表格、圖片、核取清單；Slides 下面一排縮圖，右上角可以直接報告；Design 是一張畫布，每個畫板可以單獨放大、下載。在終端機裡跑，它會給你一個連結，用瀏覽器打開就是同一個編輯器。

![https://ithelp.ithome.com.tw/upload/images/20261011/20182796u1GuJTeriW.png](https://ithelp.ithome.com.tw/upload/images/20261011/20182796u1GuJTeriW.png)

![https://ithelp.ithome.com.tw/upload/images/20261011/20182796kSPYYYnBAh.png](https://ithelp.ithome.com.tw/upload/images/20261011/20182796kSPYYYnBAh.png)

**2\. 要等幾分鐘。** Docs 我截圖的時候已經跑了 4 分多鐘，還在修；Design 大約 8 分鐘。丟給它之後，去做別的事比較實際。

**3\. Slides 會講它自己做了哪些決定。** 做完它列了一段「有幾點是我自己決定的」：系列地圖上 G8 還寫著待寫，它直接當成已經寫完；沒有可靠來源的安裝步驟，它就不放。哪裡是它猜的，一眼就看得到。

**4\. 成品頁多了兩個空白的 `Untitled` 。** Slides 和 Design 各留下一個，打開是空的。不影響使用，但記得回成品頁刪掉。

### /design 是 G6 wireframe 的升級版

G6 最有感的一招，是叫它畫一張 wireframe —— 不是要漂亮，是確認「你想的跟它想的是同一件事」。

`/design` 把這件事往前推了一步。我用的是這句：

```bash
/design G9 的社群圖卡，給 Threads 和 Facebook 用，主題是「把 Claude 做的東西變成一個會更新的網址」。畫三個方向，配色用 medium/drafts 裡 html 預覽的 ink/ember 風格。
```

![https://ithelp.ithome.com.tw/upload/images/20261011/201827962iGfXWiezd.png](https://ithelp.ithome.com.tw/upload/images/20261011/201827962iGfXWiezd.png)

跟 G6 的 wireframe 比，多了三件事：

- **一次好幾個方向。** 不是一張草圖，是同一張畫布上好幾個畫板，並排比。
- **可以直接改。** 在電腦瀏覽器裡點畫板上的元素，就能改字、移位置，會自動存檔。不用每改一個字都回去跟它講。
- **能帶走。** 每個畫板可以匯出成 PNG 或 PDF。

挑好之後，就跟 G6 一樣：跟它說要哪一個，請它照著做出來。官方介紹 `/design` 時，寫的就是這個流程：

> **Pick one, tweak it, then have Claude implement it.**
> 
> 挑一個、調一下，再讓 Claude 實作。  
> —— [Claude Code 官方文件〈What's new, Week 34〉](https://code.claude.com/docs/en/whats-new/2026-w34)

> 💡 **它怎麼知道我的配色？** 是我在 prompt 裡指名的。我的帳號沒有設定任何設計系統，沒指名的話，它會用自己的風格。樣板可以接一套設計系統（顏色、字型、元件），設定好之後新做的都會套用；用 React 寫元件的人，可以用 `/design-sync` 把 repo 裡的元件同步過去。比較進階，先知道有這件事就好。

### 回頭看我那 37 個

如果樣板早點出來，我那 37 個大概會這樣分：

| 原本做成一般 artifact 的 | 現在會改用 | 為什麼 |
| --- | --- | --- |
| 規格書、驗收清單、交接書 | Docs | 會有人想直接改 |
| 進度會報 | Slides | 本來就是要上台講的 |
| 視覺方向、配色比較 | Design | 直接在畫布上改，不用一直叫它重做 |
| 事故調查、試算、圖解 | 留在一般 artifact | 只要看，或要能互動 |

---

## 8\. 限制和代價

artifact 不是萬用的：

- **只有一頁，沒有後端。** 不能拿來架網站或內部系統。官方說，要做那種東西，請放在自己的伺服器上。
- **外部圖片會被擋。** 頁面不能從別的網站抓圖片，圖都要直接嵌在頁面裡；整頁最多 16 MiB。
- **比純文字花 token。** 跟 G6 一樣：有樣式的頁面，比同樣內容的純文字貴，嵌進去的圖片最花。官方的建議是：圖表用 SVG 畫、用不到的互動功能不要做、大量資料讓頁面放摘要就好。
- **要 claude.ai 帳號。** 前面講過，API key 不行。

所以 G6 那條判準還是成立： **只看一次的東西，終端機的文字就夠了。**

> 💡 **它還能抓即時資料。** artifact 可以在每次有人打開時，透過 **看的人自己的** connector 抓最新的資料，例如 GitHub 上還沒處理的 PR。connector 是 G11 MCP 的主題，到時候再細講。

---

## 9\. 本課單字：把產出帶出去

### 「幫我做成 artifact」

一句話把產出變成網址。不用學指令，直接描述你想看到什麼也可以。

### /artifacts

列出你所有的 artifact。 `Enter` 附加到現在的對話（換對話要更新時用）、 `o` 用瀏覽器打開、 `c` 複製連結。

### Ctrl+\]

重新打開這個對話最近做的 artifact。網址捲不見的時候用。

### /design

一句需求，畫出好幾個方向的畫板。G6 wireframe 的升級版。

---

## 10\. 給新手的三個提醒

### a. 「知道連結的任何人」就是任何人

選了它，網址傳到哪、誰就打得開。只想給特定幾個人看，用 email 邀請；工作上的東西，分享前先想一下。

### b. 換了對話，把網址給它

不然它會做一個新的，你的成品清單就會出現好幾份同名的東西 —— 到時候要找最新的那份，就得一份一份點開來看。

### c. 只要看，用網頁；要一起改，用樣板

別人只要看結果，一般 artifact 就好；別人要動手改、要帶走 Word 或 PowerPoint，選 Docs 或 Slides。

---

## 結語

回到開頭那三份「最終版」。用 artifact，你只要傳一次網址；之後改幾次，對方打開都是最新的。只要記得一件事：換了對話，要把網址給它，不然清單裡也會冒出好幾份同名的東西。

G6 解決的是「 **你** 要花多少力氣，才看得懂 Claude 給你的東西」。這篇往前走一步：「 **別人** 要花多少力氣，才拿得到你最新的那一份」。答案是一個網址。

用久了你可能會發現，自己一直在重打同一句話：「做成 artifact，用我的配色，放這幾個區塊」。官方文件在 artifact 那一頁的最後，剛好寫了下一步：

> **Turn an artifact prompt you reuse into a skill so you can invoke it as a command.**
> 
> 把你一直重打的 artifact 指令變成一個 skill，之後一個指令就能叫出來。

下一篇就講這個。

> 📌 **關於本篇的細節：** 方案限制、樣板、畫面上的字，都是依 **2026 年 10 月的官方文件** 和 v2.1.287 整理的。artifact 和樣板 **改得非常快** （樣板還在 beta），如果你是很久以後讀到這篇，請以官方文件為準。

---

下一篇 **G10 — Skills：把重複的工作流打包** 。同一句話打到第三次，就該把它變成一個指令。

---

## 系列文章

- G1 Claude Code 從零開始 — Portfolio v2 重做
- G2 用量與計費 — 搞懂怎麼收費，再決定要不要省
- G3 CLAUDE.md — 讓 Claude 跨對話記得你的專案
- G4 Memory 系統 — 讓 Claude 不只記得專案，還記得你
- G5 換 section 不斷線 — 把當下進度交接給下一個對話
- G6 用 HTML 跟 Claude 溝通 — 讓你一眼看懂它給你的東西
- G7 控制 Claude 怎麼想 — 選模型與思考深度
- G8 先想清楚再動手 — Plan Mode 與權限模式
- G8.5 auto mode — 你的預設模式到底在幫你擋什麼
- **G9（本篇）** Artifacts — 把產出變成一個會更新的網址
- G10 Skills — 把重複的工作流打包（規劃中）

> 完整 20 篇地圖會放在系列首頁。本系列同步發佈於 Medium。

---

**關於我**

Tim Wei / EY 安永 Technology Consulting 顧問（AI/ML），做 RAG 知識系統、機器學習模型與 AI 治理，也擔任數位發展部 AI 人才培育計畫的講師。

歡迎在留言區跟我討論，或在 [LinkedIn](https://www.linkedin.com/in/timwei0801) 上找我。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1791766546_6acc3012761f1.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2610B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/a125b489-b5d6-45d7-9696-7f8992b0e247_small.jpg)

[當我們談論 5G 資安](https://itplus.ithome.com.tw/vod-page/345?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/22/c360c0d8-ffa3-4602-b6ac-a86ba0f35e81_small.png)

[零信任架構帶給企業資安的機會與挑戰](https://itplus.ithome.com.tw/vod-page/627?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

零信任資安講堂 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/10/31/50a8ef99-343c-4fc8-a56d-f24df3780224_small.png)

[2022 CipherTech 亞利安科技 Solution Day 議程 — 資料庫安全防護，金鑰管理、加密、代碼化](https://itplus.ithome.com.tw/vod-page/725?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

CipherTech 亞利安科技 |

34 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/10/1/97c23fc2-e60b-4e3d-bea4-36dfcc7c2c68_small.png)

[全面解放 Agentic AI：以 Nutanix 打造現代化 AI 企業營運](https://itplus.ithome.com.tw/vod-page/1524?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

39 分