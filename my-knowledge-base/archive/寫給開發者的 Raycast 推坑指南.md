---
title: "寫給開發者的 Raycast 推坑指南"
source: "https://hackmd.io/@DailyOops/raycast-for-developers"
author:
published:
created: 2026-05-28
description: "# 寫給開發者的 Raycast 推坑指南  ![](https://hackmd.io/_uploads/ByLinzBD5.png)  ## 推坑推推坑  這次我要介紹的是 [Raycast]"
tags:
  - "clippings"
---
## 推坑推推坑

這次我要介紹的是 [Raycast](https://raycast.com/?via=yukai) ，Raycast 是一套「啟動器」(Launcher) 類型的軟體。什麼是啟動器？啟動器提供一個文字輸入框，你只要在裡面輸入關鍵字，就能快速的啟動關鍵字搜尋到的軟體，或是任何找到的工作流程，比如 **播放一首歌** 、 **開啟一份文件** 、 **切換電腦的黑暗模式** 等。啊，是不是聽起來很像流行很久的 ？嘿對沒有錯，Raycast 可說是新一代版 Alfred，預設的界面設計比 Alfred 更為貼近目前 macOS 的設計語言。

如果你用的是 Windows，你也許也用過 [Wox](https://github.com/Wox-launcher/Wox/) 這款啟動器軟體（現被 [Powertoys Run](https://docs.microsoft.com/zh-tw/windows/powertoys/run) 繼承）。這種啟動器類型的界面可以用「類 Spotlight」、「Command Palette」來稱呼，關於它的設計脈絡，可以參考 [這篇文章](https://capiche.com/e/consumer-dev-tools-command-palette) 。近年隨著開發者生產力工具的蓬勃發展，Command Palette 更是成為了各 SaaS 的標配。彷彿少了這個功能就是不順手，不夠潮一樣 XD

至於本篇文章為何要特別「寫給開發者」呢？原因無他， [USP](https://www.wikiwand.com/en/Unique_selling_proposition) 而已，作為一個開發者，不對喜歡的軟體狠狠搗鼓一番實在說不過去（好啦就只是我個人的執念）。雖然我在 [2020 年底就開始使用了 Raycast](https://www.facebook.com/yukaihuangtw/posts/3904306446255611) ，不過還真像我在臉書貼文預告的一般，一直都是偷偷自己用，直到去年年底 [Extension API 推出](https://www.raycast.com/changelog#1.25.0) 後，我才開始全力把 Raycast 和自己的工作流程深度整合。

話不多說，準備好接受推坑嗎 (Y/y)

## 粗茶淡飯

先介紹幾個可能會讓你對 Raycast 有興趣的功能。Raycast 厲害的地方在於它 **內建的功能** 就已經足夠好用了，而且在每次更新，時不時就會再推出讓你幸福感加倍的小功能，比如 [星戰光劍](https://www.facebook.com/yukaihuangtw/posts/5487727191246854) 這種無用但很炫炮的東西（？）

### 程式碼片段（snippet）

程式碼片段讓你快速貼上一段文字，還可以設定 **關鍵字展開** ，輸入指定的關鍵字，就會自動幫你貼上片段內容。

### 剪貼簿管理

界面其實和上面 Snippet 一樣就不貼了 XD。我原本就會裝剪貼簿管理工具，比如 [Maccy](https://github.com/p0deje/Maccy) ，但有了 Raycast 後通通都不用了，一套抵多套，讚！

### 選單項目搜尋 (Menu Item Search)

在 macOS 裡選單項目本身就可以透過「幫助」->「搜尋」項目來做查詢，但 Raycast 在 [1.20.0 版](https://www.raycast.com/changelog#1.20.0) 推出了選單項目搜尋的功能，可以說 **幫所有 App 都加上 Command Palette** ，相當實用。

### 視窗管理 (Window Management)

沒錯！Raycast 還內建了視窗管理的功能，常用的視窗放大縮小排列靠左一半分割靠右分割置中都可以，也可以綁上自己習慣的快速鍵，在涵蓋原本我常用的視窗管理工具 [Phoenix](https://github.com/kasper/phoenix) 80% 功能後，我也刪除 Phoenix 了，把全部的視窗快速鍵都交由 Raycast 管理。

## 是時候展現真正的技術了 - 擴充功能 API

講完了幾個 Raycast 內建的常用功能，接下來就帶各位看一下真正讓我興奮 Raycast 的殺手級功能：擴充功能 API (Extension API）。

有了擴充功能，你幾乎可以讓 Raycast 完成任何事，唯一的限制就是 **使用者介面被 Raycast 規範** ，但想想，有適度的限制才能讓開發者專注在開發功能上：不用考慮使用者體驗，因為這點 Raycast 幫你規範好了，Raycast 的開發者還會給你建議；也不用想如何 Deliver，因為有中心化的擴充功能商店（Extension Store），你只管發 Pull Request 到 Raycast 擴充功能的 [GitHub Repo](https://github.com/raycast/extensions) ，PR 合併之後幾分鐘後擴充功能就會上架到 [商店](https://raycast.com/store) 讓所有人可用。

把擴充功能統一集中放在官方的 Repo 也有不少好處。一來開源授權統一，Raycast 官方運用起來清爽無負擔，自用送人兩相宜。二來生態系集中，所有擴充功能都在一個 Repo 裡面，隨時想 ~~抄抄作業~~ 也十分方便，不用在 GitHub 上漫天亂找（阿不過現在有了 [GitHub Code Search](https://cs.github.com/about) 要跨 repo 抄作業也更方便了）。

是說我還沒講到 Raycast 擴充功能 API 最厲害的地方 － React API。什麼意思？直接來點程式碼吧：

```javascript
import { Detail } from '@raycast/api';

export default function Command() {
    return <Detail markdown="Hello World!" />;
}
```

以上是一段 Raycast 擴充功能的 Hello World 範例，跑起來會像這樣：

對就這麼簡單，才四行而已，連 Markdown 元件都幫你做好了。而且…明明是 macOS 原生桌面應用程式，竟然可以用 React 來寫？

JavaScript 成為最常用的程式語言 [已經很久了](https://insights.stackoverflow.com/survey/2021#section-most-popular-technologies-programming-scripting-and-markup-languages) ，俗話說 [打不贏就加入它](https://www.theplayerstribune.com/articles/kevin-durant-nba-free-agency-announcement) 也是有道理，沒有必要為了做擴充功能再創造一個 DSL 的輪子吧。JavaScript 有著最豐富的生態系，React Native 的 [Learn Once Write Anywhere](https://reactnative.dev/) 哲學，選對語言和生態，馬上有一堆最 [**Hype**](https://blog.daftcode.pl/hype-driven-development-3469fc2e9b22) 的開發者，每天為你的生態系貢獻，何樂而不為。對沒錯，我也是其中一員就是了 XD

熟悉的 React API、豐富的 JavaScript 套件生態系、 **優~~雅~~至~~極~~** （Elegant，エレガント） 的界面，截止今天，已經有了 408 個擴充功能。沒錯，以下這兩張圖也是來自社群裡的開發者做的擴充功能： [raycast-charts-extension](https://github.com/tonka3000/raycast-charts-extension) ：

## 那，我該怎麽開始開發 Raycast 擴充功能呢？

一開始當然是先「決定要幹嘛」啦，如果一時沒頭緒，除了從 [Issues 列表](https://github.com/raycast/extensions/issues) 找靈感外，欸嘿，身為開坑怪，我這裡也有幾個現成的想法，可以讓你試試：

- [Anki](https://apps.ankiweb.net/) 擴充功能：Anki 在本機使用 SQLite3 作為資料庫，資料庫存取的方法可以參考 [Raycast VSCode Recent Projects](https://github.com/raycast/extensions/blob/7f77d2fdafbd877d90bc548f105e101abaed265b/extensions/visual-studio-code-recent-projects/src/db.ts#L24-L31) 的做法
- [Microsoft TODO](https://docs.microsoft.com/en-us/graph/api/resources/todo-overview?view=graph-rest-1.0) 的擴充功能：Raycast 已經有數個 Todo List 相關的擴充功能了，比如 [Todoist](https://www.raycast.com/thomaslombart/todoist) 、 [Things](https://www.raycast.com/loris/things) 、 [Todo List](https://www.raycast.com/maggie/todo-list) 。所謂 Todo App 就是晉升 CRUD 工程師的第一步（？），那補齊 Raycast Todo 擴充功能的最後一角當然也是義不容辭（ ~~我到底在寫啥~~ ）
- 潛伏在 [Raycast Slack 社群](https://raycastcommunity.slack.com/archives/C02HEMAF2SJ) 尋找新想法

有了題目，再來就是照著 [文件](https://developers.raycast.com/) 一步一步做。 [Raycast 的開發文件](https://developers.raycast.com/) 清晰明瞭又有截圖，若有問題，在 Slack 的 [`#extensions`](https://raycastcommunity.slack.com/archives/C02HEMAF2SJ) 或是 [`#help`](https://raycastcommunity.slack.com/archives/C028DTXTW3Y) 頻道發文，回應的速度也都相當快。

### 講個秘訣

最後附上幾個在開發擴充功能時，可能會對你有點幫助的訣竅：

**（2023/2/9 更新）**

- 用 `git sparse-checkout` 以及 `shallow clone` 來存取開發倉庫，具體如下：
	```bash
	​​​​# 雙重 buff，depth 1 + filter
	​​​​git clone --filter=blob:none --sparse --depth=1 git@github.com:username/extensions.git
	​​​​
	​​​​# and cd
	​​​​cd extensions
	​​​​git sparse-checkout add extensions/curator-bio
	```
	- 更多 `sparse-checkout` 用法再自己查一下
- 使用 [@raycast/utils](https://developers.raycast.com/utilities/getting-started) 。除了抓 API 資料外，Raycast utils 也提供各種好用的函式
- ~~使用 [swr](https://swr.vercel.app/) 作為抓 API 資料的輔助。 [`react-query`](https://react-query.tanstack.com/) 因為 API 設定時要另外包一層 Context，而在 Raycast 每個 Command 都可以被視為一個 App，可能是因為這樣設定起來很麻煩，所以沒人在用 XD~~
- 用 [Capture Raycast Metadata](https://www.raycast.com/koinzhang/capture-raycast-metadata) 來製作發佈用截圖。前不久 Raycast 在 Store 加上了截圖，針對截圖還訂定了特別的規範，比如 Raycast 視窗應該在哪個位置上，截圖畫面的大小應該是多少，還推出了 [Figma 的 Template](https://developers.raycast.com/basics/prepare-an-extension-for-store#adding-screenshots) 供大家使用，不過對於一般開發者來說還是有點麻煩。社群的開發者就推出了這個專門製作截圖的擴充功能，讓截圖製作一鍵完成 😍
	- **更新** ：要注意解析度問題。一定要在 MacBook 的螢幕截圖才會輸出高解析度圖片，在外接螢幕截圖如果解析度不夠是會被退件的（aka validator 不會過）
- 盡量遵照原生界面，真的想高度自定界面的話可以使用 [SVG](https://github.com/raycast/extensions/blob/859e8a20a2/extensions/jwt-decode) 。
	- **更新** ：不太建議，會變複雜很多
- 持續關注新 API 更新，缺少的 API 常常許願過後一陣子就會在更新出現，記得常常追蹤 Slack！

預祝各位 Happy Hacking Raycast Extension 啦！

我後附上 [我在 Raycast Store 的個人頁面](https://www.raycast.com/Yukai) ，對我開發的擴充功能有興趣也可以安裝試試~~

也歡迎大家也利用我的 [推薦連結註冊](https://raycast.com/?via=yukai) ，讓我分到你買 Raycast Pro 方案的 $$ XD