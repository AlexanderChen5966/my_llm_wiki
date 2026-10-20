---
title: "CLAUDE.md 是 21 個章節，還是 8 行——官方文件已經定案"
source: "https://codelove.tw/@tony/post/aWKpNa#google_vignette"
author:
published:
created: 2026-07-30
description: "CLAUDE.md 的寫法我查了一下，結果碰上了**完全相反的主張**。- **A**：「準備 20 項以上的模板，應該把角色、規範、測試策略、安全性都涵蓋進去」- **B**：「8 行就夠了。與其增加，不如刪減更有效」兩邊都寫得很有自信。實際都試過之後，我又重讀了官方文件，**到底誰對誰..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月30日](https://codelove.tw/@tony/post/aWKpNa "2026-07-30 17:03:14")

## CLAUDE.md 是 21 個章節，還是 8 行——官方文件已經定案

[CLAUDE.md](http://claude.md/) 的寫法我查了一下，結果碰上了 **完全相反的主張** 。

- **A** ：「準備 20 項以上的模板，應該把角色、規範、測試策略、安全性都涵蓋進去」
- **B** ：「8 行就夠了。與其增加，不如刪減更有效」

兩邊都寫得很有自信。實際都試過之後，我又重讀了官方文件， **到底誰對誰錯，已經非常明確地分出勝負了** 。而且，理由不只是「乾脆寫短一點」這麼簡單。

這篇文章會從機制開始說明，為什麼長版 [CLAUDE.md](http://claude.md/) 沒有效果，並整理出到底該寫些什麼。

**本文重點**

- 官方立場是「保持簡短」。 **一旦膨脹，真正想遵守的指示反而會被忽略**
- 原因有兩個。 **每次請求都會載入到上下文** ，以及 **注意力會被稀釋**
- 以現行模型來說， **有些指示寫了反而有害** 。「要驗證」「CRITICAL:」之類就是典型

---

## CLAUDE.md 是什麼

這是 Claude Code 在 **工作階段開始時自動載入的設定檔** 。可以省去每次都輸入相同前置說明（例如「請用日文」「請用條列式」）的麻煩。

主要有兩個放置位置， **兩者都會被讀取並加總** 。

路徑 範圍 `~/.claude/CLAUDE.md` 所有專案共用（個人專用） `./CLAUDE.md` （專案根目錄） 只對該專案有效。若納入 Git，則可供團隊共用。也就是說， **自己的偏好** 可以寫在共用端一次，全專案生效； **該專案特有的前提** 則寫在專案端，這樣分工最合適。

如果從零開始覺得麻煩，可以用 `/init` 產生草稿。也可以用 `/memory` 進行編輯。

[CLAUDE.md](http://claude.md/) 是 **請求，不是強制命令** 。官方也沒有保證它一定嚴格遵守。若有偏離，就當場指出並修正，這就是使用前提。

---

## 兩種立場

### 立場 A：網羅式地寫

這種想法是把角色、專案概要、程式碼規範、反模式、記憶檔、錯誤日誌、提交規範、測試策略、安全性、文件、部署、效能、日誌、倫理、工具、語氣、思考流程……等，準備成 **20 項以上的章節** 。

其論點是「給 AI 的資訊越多，就越能理解脈絡、提升準確度」。從直覺上看，確實說得通。

### 立場 B：維持最小限度

相對地，是把內容壓到 **10 行以內** 的想法。

## 我的設定

- 回答一定要用日文。
- 對方不是工程師。用了專業術語時，當場用 1 行白話解釋。
- 先講結論。說明以條列為主，省略冗長前言。
- 不要擅自新增檔案。優先修改既有檔案。
- 開始動作前，先用 1～2 行說明你打算做什麼。

判斷基準很明確。 **「把這一行刪掉，Claude 會不會出錯？如果不會，就刪。」**

---

## 驗證：官方支持哪一邊

**官方明確支持立場 B。**

Anthropic 的最佳實務建議把 [CLAUDE.md](http://claude.md/) **保持簡潔** 。而且重點不只是「短一點比較好讀」，而是 **一旦膨脹，真正想要它遵守的指示會開始被忽略** 。

這也和實際體感一致。把東西什麼都寫進去，結果變成 50 行之後，關鍵的「請用日文」反而被淹沒、失去效果——這種情況確實會發生。

那為什麼會這樣？原因有兩個。

---

## 原因 1：CLAUDE.md 會載入每次請求的上下文

這點很容易被忽略。 [CLAUDE.md](http://claude.md/) **不是在工作階段開始時只讀一次就結束** 。

Claude API 是無狀態的，所以要延續對話，就必須 **每次重新送出所有歷史** 。 [CLAUDE.md](http://claude.md/) 會放在這些內容的前面，因此 **每次請求都會被傳送** 。

也就是說， **[CLAUDE.md](http://claude.md/) 越長，所有請求的 token 成本就越高** 。來回 100 次，就是 100 次的成本。這不是一次性的初始化成本。

另外，從提示快取（prompt caching）的角度來看也不利。快取是靠 **前綴完全一致** 來生效，所以一旦修改 [CLAUDE.md](http://claude.md/) ，之後的快取就會需要重新建立（Claude Code 的內部實作未公開，這裡是根據機制做的推測）。

**所以，頻繁變動的資訊不應該寫進 [CLAUDE.md](http://claude.md/) 。**

---

## 原因 2：指示越多，單一指示的權重越低

更本質的原因在這裡。

如果只有 5 個指示，模型還能分別注意到它們。若有 50 個指示， **每一條指示的相對權重就會下降** 。例如「請用日文」這一行，會和「部署是透過 CI/CD 自動化」這一行，以差不多的權重並列在一起。

而且現行模型（Claude Opus 4.5 之後） **對指示非常忠實** 。乍看是優點，但反過來也代表 **不必要的指示所帶來的負面效果，也會被忠實反映** 。

- 模糊的指示 → 會被模糊解讀
- 衝突的指示 → 會被擅自選邊
- 不必要的指示 → 會被老實遵守，造成浪費

**正因為模型變聰明了，更需要挑選要寫什麼。**

---

## 驗證：看看網羅模板的內容

這裡是實害最大的部分。立場 A 的模板裡，混進了 **在現行模型下反而有害的指示** 。

### 「驗證一下」才是應該刪掉的

網羅模板通常會包含「最後加上驗證步驟」「讓另一個代理來審查」之類的項目。

**Claude Opus 5 即使不提醒，也會自己做檢查。** 如果還保留驗證指示，就會 **重複驗證** ，浪費時間與 token。

Anthropic 的遷移指南明確寫的是，這種內容應該 **刪除，而不是改寫** 。刪掉也不會降低品質。

這其實 **和一般提示詞技巧相反** 。

「讓模型自我檢查可以提升準確度」這件事曾經很有效，過去的模型也確實如此。到了 Opus 5，反而會適得其反。如果你們公司的提示規範寫著「一定要加上自我驗證的一句」，那就必須破例處理。

### 「CRITICAL:」「MUST」會引發過度反應

常看到有人說，「如果是非得遵守的行，就加上 `IMPORTANT:`，遵守率會提高」。這是 **比較舊的建議** 。

Opus 4.5 之後的模型變得更忠於指示，因此 `CRITICAL` 、 `MUST` 、 `If in doubt` 這類強烈字眼，官方已明言會引發 **過度反應（overtrigger）** 。

| Before | After |
| --- | --- |
| `CRITICAL: You MUST use this tool when...` | `Use this tool when...` |
| `Default to using [tool]` | `Use [tool] when it would improve X` |
| `If in doubt, use [tool]` | （刪除） |

如果出現工具用太多、指示套用過頭這類症狀， **不是先寫更多防護線，而是先把語氣放弱** ，這才是正確處理方式。

### 「公開思考流程」會讓內容更冗長

Opus 5 本來就有輸出偏長的傾向。再加上「逐步公開思考流程」「比較多個選項再決定」之類的要求，內容只會更長。

在已經能判斷的情況下，讓它立刻開始動作，效率更高。

### 「多分工一點」對不同世代會反過來

關於委派給子代理這件事也要注意。Opus 4.8 的委派不夠，需要刻意催促；但 **Opus 5 放著不管也會主動委派** 。如果把寫給 4.8 的「多委派一點」保留下來，就會分工過度，導致成本與等待時間增加。

現行模型真正有效的是反方向的指示。

`子代理的啟動最多以 3 個為限。`

---

## 結論：該寫什麼

判斷標準可以直接用立場 B 的方法。

### 值得寫的內容

**只有模型無法自行推測的資訊。**

分類範例語言與文體「回答一定要用日文」讀者前提「對方不是工程師。專業術語要用 1 行白話解釋」輸出形式「先講結論。以條列為主」禁止事項「不要擅自新增檔案」「不要使用表情符號」專案特有前提技術堆疊、命名規則、應參考文件的位置範圍規範「只執行被指示的範圍」

最後這個「範圍規範」在現行模型下特別有效。Opus 5 有傾向去做你沒要求的事情，所以明確畫線後，它就會停下來。

### 不建議寫的內容

- **一般論** ——「寫得更有禮貌」「重視可維護性」。大家都知道的事，浪費欄位
- **模型本來就會做的事** ——「要驗證」「要寫測試」
- **經常變動的資訊** ——例如進行中的任務狀態。會破壞快取，也很快就過期
- **很少用到的流程** ——每次都會被讀進來，浪費欄位，應該放到別的機制（例如 skills）裡
- **模糊的表達** ——「做好一點」「自然一點」。無法驗證的指示沒有用

### 變多了就切分

如果內容變多了，可以在 [CLAUDE.md](http://claude.md/) 裡用 `@` 寫入路徑，讓它讀取其他檔案。

`程式碼規範請參考 @docs/coding-rules.md。`

這樣會比塞在同一頁更容易整理。不過 **總讀取量並不會改變** ，所以這是整理用途，不是減量手段。

---

## 運用技巧

**不要一開始就追求完美。** 從 3 行開始，如果覺得「啊，這個又講了一次」，再補 1 行。這樣反覆調整，就會變成適合自己的版本。

如果它沒有照做，排查順序如下。

1. 先用 `/memory` 確認 **到底有沒有被讀進來** （路徑不同就不會讀到）
2. 如果有讀到，再懷疑 **太長、太模糊、互相矛盾**
3. 定期盤點， **把可以刪的行刪掉**

第 3 點最有效。與其增加，不如刪減更有效，這不只和實際體感一致，也和官方說明一致。

---

## 總結

- **官方立場是「保持簡短」。** 一旦膨脹，真正想遵守的指示反而會被忽略
- 原因是 **每次請求都會載入上下文** ，以及 **指示越多，單一指示的權重越低**
- 現行模型有些 **寫了反而有害的指示** 。「要驗證」要刪除，「CRITICAL:」要放弱，「多委派一點」要改成上限指定
- 判斷標準就是 **「把這一行刪掉，Claude 會不會真的因此出錯」**

網羅式模板表面上看起來很周到，也讓人安心。但 [CLAUDE.md](http://claude.md/) 不是讓 Claude 變聰明的魔法， **而是用來省略每次前置說明的備忘錄** 。所以不是寫得越多越有效，而是抓得越準越有效。

先打開你的 [CLAUDE.md](http://claude.md/) ， **找找看有哪些行可以刪。** 我想，大概會找得到。

---

原文出處： [https://qiita.com/jqit\_suwa/items/cea574550613de33a114](https://qiita.com/jqit_suwa/items/cea574550613de33a114)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aWKpNa) [4瀏覽](https://codelove.tw/@tony/post/aWKpNa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aWKpNa)

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

📝11 💬1 ❤️6

340

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

114

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785402984&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaWKpNa&amp;pra=5&amp;aiof=10&amp;asro=0&amp;itsi=27&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTg3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjE4NyJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTg3Il1dLDBd&amp;dt=1785402984208&amp;bpp=2&amp;bdt=66&amp;idt=28&amp;shv=r20260729&amp;mjsv=m202607240101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785401262%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785401262%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785401262%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1213145518199&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95395796%2C95396139%2C95396252&amp;oid=2&amp;pvsid=776136861256533&amp;tmod=923065400&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=46" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260729/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>