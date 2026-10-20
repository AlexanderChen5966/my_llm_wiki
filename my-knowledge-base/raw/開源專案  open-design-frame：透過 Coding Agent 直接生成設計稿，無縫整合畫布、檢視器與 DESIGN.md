---
title: "[ 開源專案 ] open-design-frame：透過 Coding Agent 直接生成設計稿，無縫整合畫布、檢視器與 DESIGN.md"
source: "https://medium.com/@simon3458/open-design-frame-32bcd0bd2e1f"
author:
  - "[[Simon Liu]]"
published: 2026-09-10
created: 2026-10-02
description: "不囉唆，先給 Github 連結，但請大家給專案一顆星星，並且努力用它，幫助你的工作上能夠做得更好，有任何 bug issue 請發 Github，感謝大家！"
tags:
  - "clippings"
status: "inbox"
---
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*TfaKnzSMfKWSpsdwkJEFGw.png)

\[ 開源專案 \] open-design-frame：透過 Coding Agent 直接生成設計稿，無縫整合畫布、檢視器與 DESIGN.md

## 懶人包：Github 專案 — open-design-frame

不囉唆，先給 Github 連結，但請大家給專案一顆星星，並且努力用它，幫助你的工作上能夠做得更好，有任何 bug issue 請發 Github，感謝大家！

## [GitHub - simonliu-ai-product/open-design-frame: Design frames, written as components. A canvas that…](https://github.com/simonliu-ai-product/open-design-frame?source=post_page-----32bcd0bd2e1f-----------------------------------------)

### Design frames, written as components. A canvas that draws React at true size, an inspector that edits the source, and…

github.com

## 一、open-design-frame 是什麼

**一句話：它是一個設計畫布，但畫布上的每一張設計稿都是一個透過 Coding Agent 所製作出來的 React 元件。**

你在 `frames/` 底下寫一個元件、給它一個尺寸，它就成為畫布上的一張設計稿。開一個指令就有工作區：左邊是檔案與資料夾、中間是等比例的畫布、右邊是可以改東西的檢視器。

```cs
pnpm add -D @open-design-frame/core
open-design-frame dev
```
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*I4kpvdAWjBck2Tj8.png)

因為設計稿本身就是程式碼，所以它天生具備三件別的設計工具做不到的事： **Coding Agent 可以直接寫它** 、 **git 看得懂它的差異** 、而且 **畫布上看到的就是會出貨的東西** — — 因為那就是會出貨的東西。

它適合誰：需要一張說得清楚的畫面，但不想開 Figma 慢慢畫的人。產品經理畫產品介面、工程師畫提案示意圖，或者只是想確認「這個想法長出來會是什麼樣子」。

## 二、它想解決的問題

要一張設計稿，過去通常是這三條路，而三條都有各自的痛：

- **開 Figma 自己畫** ：畫得出來，但它跟最後會出貨的程式碼是兩份東西，改了一邊另一邊就開始走鐘。
- **請 Agent 直接寫一個網頁** ：這個快，但它是「網頁」不是「設計稿」，沒有固定尺寸的概念，你在 27 吋螢幕上看很好，別人用筆電打開就整個垮掉。
- **把 Figma 截圖丟給 Agent 說照這個做** ：Agent 打不開 Figma 檔，它只能看圖猜，猜出來的間距、字級、顏色跟你檔案裡的值不會一樣。

共同的形狀是： **我們把 Agent 真正擅長的東西（寫元件）跟它做不到的東西（讀設計檔）綁在一起了。** open-design-frame 就是把這兩件事拆開 — — 設計稿改用 Agent 本來就會寫的語言，而畫布負責把它擺成一張有尺寸、能點、能交件的設計稿。

## 三、三分鐘上手

裝好之後，一個 frame 長這樣：

```cs
// frames/dashboard/index.tsx
import { type Frame, SIZES, Layer } from '@open-design-frame/core';

const Overview: Frame = () => (
  <Layer name="Summary" kind="section">
    <h1>本月營收</h1>
  </Layer>
);
Overview.frameName = 'Overview · Desktop';
Overview.size = SIZES.DESKTOP;   // 1440 × 1024
export default [Overview];
```

三個重點： `frameName` 是它在畫布上的名字、 `size` 是它的尺寸、包在 `<Layer>` 裡的東西會出現在檢視器的圖層樹（沒包的照樣會畫出來，只是不能從圖層樹點選）。

`SIZES` 內建五個常見尺寸，你也可以直接寫數字：

![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*-kYBDz8yMD3YjIElfv2-1Q.png)

尺寸是 **每一張 frame 自己的屬性** ，不是專案設定。文件是 A4、簡報是 16:9，因為媒介決定了尺寸；但設計稿沒有固定媒介 — — 桌機版、手機版、社群圖都是「這個設計」，沒有哪一個比較正統。所以同一個檔案裡可以同時放 1440 的桌機版跟 390 的手機版，匯出的時候各自用自己的尺寸重畫。

## 四、能做什麼

### 1\. 點畫布上的元素做修改

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*ZzSEgPDSDE2PZvYR.png)

點任何一個元素 — — 不限於你特別包起來的部分 — — 右邊會填入 **瀏覽器實際解析出來的值** ，不是另外存的一份狀態。字級、字重、顏色、對齊、間距拉一拉，畫布當下就變了；那只是暫存，按 Save 才會寫回 `frames/<id>/index.tsx` 。

## Get Simon Liu’s stories in your inbox

Join Medium for free to get updates from this writer.

也因為它改的是原始碼，所以有些情況它會 **明確拒絕** ：如果那個元素的樣式來自一個共用常數（ `style={CARD}` ），改下去會把其他十幾個用到它的地方一起改掉，而你拉那一下滑桿的意思顯然不是這樣。它會直接告訴你原因，請你去改那個常數、或者給這個元素自己的樣式。

### 按 Play，設計稿變成可以點的原型

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*8SVXoBhfEKLPbFlJ.png)

在 frame 裡宣告一個 frame 連到另一個 frame：

```cs
<Layer name="Transactions" to="Transactions · Desktop">…</Layer>
```

按下 Play，它會被放進一個模擬的瀏覽器外殼（手機尺寸就換成手機外殼），可以實際點著走一遍流程，也支援全螢幕。跟一般原型工具不同的是，這些連結是寫在 frame 裡、跟著 git 走的，不是存在某個工具的資料庫裡。按到沒有連結的地方，它會把畫面上所有熱點閃一下，而不是把那一下吞掉。

### 主題 = tokens + 一份 DESIGN.md

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*6DdmXq-Zylgx5A41.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*Za4bJlk2fxLi4l5H.png)

一個主題是一個資料夾，裡面兩個檔案：

```cs
themes/aurora/
  index.ts      ← tokens（會變成 --odf-* 變數，frame 讀它）
  DESIGN.md     ← token 裝不下的另一半（人跟 Agent 讀它）
```

`DESIGN.md` 寫的是氛圍、間距階、元件規則、該做與不該做，還有可以直接交給 Agent 的提示語——頁面上有一顆按鈕，一鍵複製整份 markdown 丟給你的 Agent。而左邊那張規格表是 **生成的、不是畫的** ：每一格色階、每一個 Aa、每一顆按鈕都從該主題自己的 token 算出來，所以它不可能承諾一個 frame 做不到的事情。

> *DESIGN.md 這個做法是 Google Labs 的 Stitch 團隊提出來的：* [*2026 年 3 月在 Google Stitch 新版中提到*](https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-ai-ui-design/) *DESIGN.md 是一個對 agent 友善的 markdown 檔案，讓設計規則能在不同的設計與 coding 工具之間匯出匯入；* [*4 月 21 日他們把草案規格開源了*](https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-design-md/) *，放在* [*google-labs-code/design.md*](https://github.com/google-labs-code/design.md) *。*

### 留言直接寫進原始碼

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*LuGv9MJsLd0okH3f.png)

與其在對話框裡描述「左邊那張卡片下面那行小字」，不如點它、留一則字條。字條會變成一段 JSX 註解，就寫在它所講的那段標記旁邊 — — 重新整理還在、跟著 commit 走、進得了 PR 被 review，元素被刪掉的時候字條也一起消失。回頭跟 Agent 說一句「處理一下這些註解」就行。

### 匯出是一個 zip，每種格式一個資料夾

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*t91yf5Lt_QyJYF76.png)

設計稿交出去的時候從來不是一個檔案，是一組檔案。所以匯出產生的是單一 zip： `html/` 、 `png/` 、 `svg/` 各一個資料夾。其中 HTML 是可以獨立打開的一頁（標記加 token，不用另外附 CSS），直接丟給不裝任何工具的人看最方便。

另外 `open-design-frame build` 可以把整個工作區打包成靜態網站，丟到任何地方就是一個可以分享的設計稿網站。

### 內建 MCP server，任何 Agent 都能操作

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*G_GQgD_TKay8L7W5.png)

```cs
pnpm add -D @open-design-frame/mcp
open-design-frame dev --mcp
#   ➜  MCP:     http://localhost:5274/mcp
#   ➜  Local:   http://localhost:5274/
```

**25 個工具** ，涵蓋 frame 的增刪改查、單一元素的樣式與文字修改、留言、資料夾、主題、DESIGN.md 與素材。無狀態的 Streamable HTTP，client 直接呼叫，不需要 session handshake，Google Antigravity、Claude Code 或你自己寫的 Agent 都接得上。

值得一提的是 `write_frame` 會帶著你上次讀到的 revision，如果磁碟上的內容已經變了就回 `409` 而不是覆蓋——因為人跟 Agent 同時改到同一個檔案，在有 Agent 參與的系統裡是常態、不是例外。

## 五、結語

open-doc 那篇的結論是「做給 Agent 用的工具，難的不是功能，而是讓 Agent 知道怎麼用」。做完這一個，我想補一句：

**也要讓人知道 Agent 做了什麼。**

這個專案裡我自己最喜歡的三個設計，其實都在處理同一件事：畫布上點一下就能看到那個元素在原始碼的哪一行、留一則字條會變成原始碼裡的一段註解、寫入的時候要帶著上次讀到的版本。它們都不是為了讓 Agent 更聰明，而是為了讓 **人跟 Agent 在同一份檔案上工作的時候，彼此看得見對方做了什麼** 。

歡迎試用、開 issue、送 PR，也很歡迎告訴我你拿它畫了什麼。

## I am Simon

大家好，我是 Simon 劉育維，是一位 AI 領域解決方案專家，目前也擔任 Google Cloud AI 領域開發者專家 (GDE)，期待能夠幫助企業導入人工智慧相關技術解決問題。如果這篇文章對您有幫助，請在 Medium 上按一下鼓勵，並追蹤我的個人帳號，這樣您就可以隨時閱讀我所撰寫的文章。歡迎在我的 Linkedin 上留言提供意見，並與我一起討論有關人工智慧的主題，期待能夠對大家有所幫助！

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*U2bNGuucMkNobhm_DAeEEg.png)