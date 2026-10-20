---
title: "TypeScript 7 改用原生實作：哪些真的改變、哪些沒有改變"
source: "https://codelove.tw/@tony/post/x0nk2a"
author:
published:
created: 2026-08-05
description: "大家現在應該都聽說 TypeScript「原生化」了。我也一直看到同樣的錯誤結論：TypeScript 現在好像可以不經編譯直接執行、build 步驟消失了、你的 `.ts` 檔可以直接跑。這些都沒有發生。你的瀏覽器還是不能直接執行 TypeScript，Node 也還是不能對它做型別檢查。真正..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月05日](https://codelove.tw/@tony/post/x0nk2a "2026-08-05 09:15:12")

## TypeScript 7 改用原生實作：哪些真的改變、哪些沒有改變

大家現在應該都聽說 TypeScript「原生化」了。我也一直看到同樣的錯誤結論：TypeScript 現在好像可以不經編譯直接執行、build 步驟消失了、你的 `.ts` 檔可以直接跑。

這些都沒有發生。你的瀏覽器還是不能直接執行 TypeScript，Node 也還是不能對它做型別檢查。真正「原生化」的不是你的程式碼，而是編譯器。

老實說，這反而是更好的故事。

## 「原生」到底是什麼意思

TypeScript 編譯器從誕生以來，整個生命週期都是用 TypeScript 寫的。你在 CI 裡執行的 `tsc` ，以及替編輯器提供紅色波浪底線的 `tsserver` ，本質上都是在 Node.js 上執行的 JavaScript 程式。你那個上百萬行的程式碼庫每次做型別檢查，背後其實就是另一個 JavaScript 程式在處理一大張充滿指標的型別物件圖，還得忍受單執行緒、JIT 熱身和垃圾回收壓力。

TypeScript 7 把這一切換成了用 Go 寫、並預先編譯成原生二進位檔的編譯器。Microsoft 在 [2025 年 3 月宣布這個移植計畫](https://devblogs.microsoft.com/typescript/typescript-native-port/) ，由 Anders Hejlsberg 領軍推進，並在 [2026 年 7 月 8 日以 TypeScript 7.0 釋出](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/) 。

所以當你看到「native TypeScript」時，最好把它理解成「原生編譯的 TypeScript 工具鏈」。整個流程長這樣：

- **之前：** 你的 `.ts` 檔送進一個用 JavaScript 寫、跑在 Node 上的編譯器，輸出型別錯誤與產生的 `.js` 。
- **之後：** 你的 `.ts` 檔送進一個原生 Go 二進位編譯器，輸出同樣的型別錯誤與同樣產生的 `.js` 。

第一個框和最後一個框都沒變，只有中間那個變了。這就是整個公告的核心，而且已經夠重要了。

[![比較圖，標題為 What Actually Went Native：TypeScript 6 流程中 tsc 是在 Node.js 上執行的 JavaScript，而 TypeScript 7 流程中 tsc 是原生的多執行緒 Go 二進位檔，輸入與輸出框保持不變](https://www.nazarboyko.com/assets/imgs/articles/typescript-7-native-compiler-what-changes/what-actually-went-native.webp)](https://www.nazarboyko.com/assets/imgs/articles/typescript-7-native-compiler-what-changes/what-actually-went-native.webp)

## 10 倍速度從哪裡來

這個標題數字是站得住腳的。以下是 Microsoft 在 [7.0 版本公告](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/) 中公布的完整建置基準測試：

| 程式碼庫 | TypeScript 6 | TypeScript 7 | 加速比 |
| --- | --- | --- | --- |
| VS Code | 125.7s | 10.6s | 11.9x |
| Sentry | 139.8s | 15.7s | 8.9x |
| Playwright | 12.8s | 1.47s | 8.7x |

最初的公告在單純型別檢查上的基準也講的是同一件事：VS Code 150 萬行程式碼從 77.8 秒降到 7.5 秒，TypeORM 從 17.5 秒降到 1.3 秒，tRPC 從 5.5 秒降到 0.6 秒。這個倍數在不同規模的專案中都相當一致，代表這不是只對大型儲存庫有用的某種快取技巧，而是整體下限被往下拉了。

但這裡有個多數報導都跳過的重點：單靠原生編譯本身，拿不到 10 倍。從 JIT 編譯的 JavaScript 變成 AOT 的 Go，能先拿到其中一部分。剩下的來自 **共享記憶體多執行緒** ，這是舊編譯器在結構上做不到的。JavaScript 的 worker threads 沒辦法共享物件圖，它們只能傳訊息、複製資料。而型別檢查器的工作本來就是在遍歷一整張巨大的共享型別圖，結果就被卡在單核心上。Go 版本則可以把工作切分給多個平行 worker，大家一起讀同一份記憶體。

TypeScript 7 直接把這件事暴露出來。型別檢查預設會用 4 個 worker，你也可以自己調整：

```bash
# 預設：4 個型別檢查 worker
npx tsc -p tsconfig.json

# 在硬體強一點的 CI 機器上拉高
npx tsc -p tsconfig.json --checkers 8
```

使用 `--checkers 8` 時，Microsoft 的 VS Code 基準可降到 7.51 秒，相較 TypeScript 6 提升 16.7 倍。這就是疊加效果：原生程式碼讓工作更快，而並行處理讓工作可同時展開。

記憶體也往正確方向前進了：7.0 公告顯示 VS Code 的整體建置記憶體下降 18%，Bluesky 的程式碼庫下降 26%。跟 10 倍速度相比這不是最吸睛的數字，但如果你曾在 CI 裡看過 `tsc` 一口氣吃掉 4GB 記憶體，你一定會想要這個改善。

## 你日常工作中會改變什麼

Microsoft 基準機器上的數字很漂亮，但真正重要的是這些時間會回到你一週工作的哪裡。它主要會出現在三個地方。

**編輯器。** 這會是你最先感受到的，因為你一天會感受到幾百次。原始基準中，VS Code 程式碼庫的專案載入時間從 9.6 秒降到 1.2 秒。7.0 版本也測到，開啟一個有錯誤的檔案從 17.5 秒降到不到 1.3 秒。如果你在大型 monorepo 工作過，你一定知道那種儀式感：打開檔案，等一下，看著「正在初始化 JS/TS 語言功能」轉圈圈，去泡杯咖啡，再回來看波浪底線。這個儀式在這裡終於要死掉了。語言服務也已經基於 Language Server Protocol 重新打造，Microsoft 報告相較 6.0 伺服器，失敗指令減少超過 80%，當機次數減少超過 60%。少一點「重啟 TS server」的時刻，本身就是一種生活品質提升。

**CI。** 型別檢查一直是很多 pipeline 裡安靜但又卡人的瓶頸：重要到不能跳過，慢到讓人不想面對。這次版本公告裡有真實的生產環境數字。Slack 把 CI 型別檢查從 7.5 分鐘縮短到 1.25 分鐘，並消除了 40% 的 merge queue 時間。Canva 的錯誤偵測從 58 秒降到 4.8 秒。Microsoft 自家的 News Services 團隊表示，每個月大約省下 400 小時不再耗在等 CI 建置上。當型別檢查不再是最慢的那根木樁時，merge queue 會更快清空，而「再跑一次 CI 就好」也不再意味著要多喝完一杯咖啡。

**你不再需要的 workaround。** 這一點比較隱晦，我認為也最有意思。慢速編譯器不只會花時間，它還會扭曲架構。 `skipLibCheck: true` 之所以出現在 GitHub 上一半的 tsconfig 裡，不是因為大家想少做檢查，而是因為檢查 `node_modules` 的型別太貴了。Project references、它們的 composite builds 以及 `.tsbuildinfo` 那套協調機制，大幅上其實都是性能逃生門；很多 monorepo 會沿著為了讓 `tsc` 能忍受而選出的界線來切，而不是依照領域上真正合理的界線來切。當一個 150 萬行程式碼庫的完整檢查只要 10 秒，這些壓力就會大幅緩解。你不需要明天就推翻你的 project references。你只是下一次不會再那麼快去用那些花招，而這會改變未來程式碼庫成長的方式。

## 哪些不會改變

接下來是拆穿迷思的部分，因為這份清單一樣重要：

- **你輸出的 JavaScript。** 輸入一樣，輸出也一樣。你的使用者下載到的建置產物，在行為上不會多一個位元組的差異。
- **你的執行階段。** 你的程式怎麼執行，完全沒有不同。這就是一個編譯期的故事，僅此而已。
- **型別系統規則。** TypeScript 7 的設計目標是對齊 TypeScript 6.0 的型別檢查行為。在 6.0 上能乾淨編譯的程式碼，到了 7.0 應該也會以相同方式通過。你的型別沒有變得更嚴格、更寬鬆，也沒有變得更聰明，只是檢查得更快了。

這不是運氣好，而是團隊 [一開始就選 Go 的原因](https://github.com/microsoft/typescript-go/discussions/411) 。既有編譯器累積了十年的行為：大量指標型樹狀遍歷、共享可變狀態，以及內建在結構裡的無數細微決策。團隊明確把這個專案定位成 **移植，而不是重寫** ，幾乎逐函式地翻譯既有程式碼庫，以保留完全相同的行為。Go 之所以勝出，是因為慣用的 Go 寫法可以對應原本程式碼的結構；如果選 Rust，他們就得從頭重新思考記憶體與可變性，讓移植變成重寫，也讓相容性承諾變成祈禱。

我會說，這大概是整個專案裡最被低估的一個工程決策。那個看起來最無聊、但能證明行為完全一致的選擇，正是你能把這次編譯器翻新當成類似小版本升級風險的原因。

## 這不是 Node 的型別剝除

很多人一直把這兩件事混在一起，但它們其實在解決相反的問題。

從 Node 22.6.0 開始，並在 23.6.0 之後預設啟用， [Node 可以直接執行 TypeScript 檔案](https://nodejs.org/api/typescript.html) ，方法是把型別剝掉。這裡的「剝除」非常字面：Node 會把你的型別註記換成空白，然後執行剩下的程式，所以行號與欄位號仍然對得上原始碼。它不會做型別檢查，一點都不會。你甚至可以宣告 `const port: number = "絕對不是數字"` ，Node 也會照跑不誤。

這也就是為什麼只有 **可被消去的** 語法才行，也就是那些刪掉之後不會改變執行行為的東西：

```typescript
// 在 Node 的型別剝除模式下可以正常執行：
interface User {
  id: number;
  name: string;
}
const greet = (user: User): string => \`Hi, ${user.name}\`;

// 會丟出 ERR_UNSUPPORTED_TYPESCRIPT_SYNTAX，因為 enum
// 不是可消去語法：它會產生真正的執行期程式碼
enum Role {
  Admin,
  Member,
}
```

Enum、帶有執行期程式碼的 namespace、以及建構子參數屬性，都會產生 JavaScript，所以如果把它們直接刪掉，行為就會改變，因此 Node 的預設模式不接受它們。

所以這兩者的界線是：

- **Node 的型別剝除** 回答的是：「我能不能在沒有 build 步驟的情況下直接跑這個 `.ts` 檔？」它會執行你的程式，但什麼都不檢查。
- **原生 TypeScript 編譯器** 回答的是：「我能不能在程式送出前先檢查這段程式？」它會驗證一切，而且現在快了 10 倍。

它們是互補，不是競爭關係。到了 2026 年，一個很現代的設定會是：開發時讓 Node 直接執行你的 `.ts` 檔，而原生 `tsc` 則在你的編輯器和 CI 中執行真正的型別檢查。一個拿掉 build 步驟；另一個讓安全網快到你幾乎不會感覺到它存在。

## 時間線與遷移，老實說

截至 2026 年中，現況如下：

**TypeScript 7.0 已正式 GA。** npm 上的套件名稱就是 `typescript` ，二進位檔還是叫 `tsc` 。如果你之前玩過預覽版，那時候叫 `@typescript/native-preview` ，二進位檔是 `tsgo` ；那個階段已經結束，現在原生編譯器就只是……TypeScript。

**6.x 系列仍然存在。** 以 JavaScript 為基礎的編譯器會繼續以 TypeScript 6 的形式存在，並與原生移植版平行維護，直到原生版本完全接手。甚至還有一個相容套件會把它安裝成 `tsc6` ，這很重要，因為下一點就是關鍵。

**程式化 API 才是誠實的但書。** TypeScript 7 目前還沒有提供給把編譯器當成函式庫使用的工具一個穩定 API。這代表 typescript-eslint 的型別感知規則，以及 Vue、Svelte、Astro 背後的 template 型別檢查，目前底層還是需要 TypeScript 6。穩定 API 預計會在 7.1 提供。在那之前，工具鏈重的環境還是會同時跑兩個版本：

**`package.json`**

```json
{
  "devDependencies": {
    "typescript": "npm:@typescript/typescript6@^6.0.2",
    "@typescript/native": "npm:typescript@^7.0.2"
  }
}
```

你的快速建置與編輯器體驗來自 7；你的 lint 工具鏈則保留它所需要的 6 API。笨重、暫時，但值得。

**有些預設值也變嚴格了。** 7.0 預設開啟 `strict` ， `module` 預設為 `esnext` ，並移除長期已棄用的目標，例如 ES5 與 AMD/UMD 輸出。如果你的 tsconfig 本來就已經明確寫出自己的意思，而它本來就應該這樣，那你幾乎不會注意到變化。如果你的程式碼庫從沒開過 `strict` ，那編譯器沒有把你的程式弄壞，只是終於不再假裝舊預設值還可以。

> **注意**  
> 如果你維護的是一個普通的 `tsc` 建置專案，這次升級會和大多數升級一樣無聊：更新套件、執行 build、看幾條設定警告。真正該稍微等一下的是那些工具鏈會深入編譯器 API 的團隊。切換前先檢查你的 lint 設定與框架工具。

## 這是一個工具鏈故事，不是語言故事

TypeScript 7 沒有為語言新增任何東西，而這正是它重要的原因。前面每一個主要版本都會給你新的型別系統玩具；這一版則是把你過去在每次按鍵、每次儲存、每次推送時默默付出的時間還給你，並且淘汰掉一整類其實從來不是架構、只是慢速編譯器代償機制的設計決策。

10 倍速度會成為頭條；真正的報酬，會是大型 TypeScript 團隊因此開始不再做的那些事。

---

*P.S. 感謝你花時間閱讀這篇文章！文中表達的想法與觀點都屬於我本人。英文不是我的母語，所以我會使用 AI 協助修正文法，並讓文字更清楚、更容易閱讀。如果仍有些地方讀起來稍微不自然，還請見諒！*

*原文最初發表於 [nazarboyko.com](https://www.nazarboyko.com/articles/typescript-7-native-compiler-what-changes) 。*

*如果你喜歡這篇文章，歡迎保持聯絡——我的 [LinkedIn](https://www.linkedin.com/in/nazar-boyko) 在這裡，很樂意聊天、交換想法，或只是打聲招呼。👋*

---

原文出處： [https://dev.to/nazar-boyko/typescript-7-went-native-what-actually-changes-and-what-doesnt-6b3](https://dev.to/nazar-boyko/typescript-7-went-native-what-actually-changes-and-what-doesnt-6b3)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x0nk2a) [8瀏覽](https://codelove.tw/@tony/post/x0nk2a)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x0nk2a)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 ❤️4

310

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 💬4 ❤️1

58

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785899713&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0nk2a&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785899712887&amp;bpp=7&amp;bdt=116&amp;idt=134&amp;shv=r20260804&amp;mjsv=m202607300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785899710%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785899710%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785899710%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2033503158720&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95396138&amp;oid=2&amp;pvsid=4451648941623374&amp;tmod=159034781&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C908&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=192" title="Advertisement" aria-label="Advertisement"></iframe>