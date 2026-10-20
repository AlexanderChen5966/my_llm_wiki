---
title: "【圖解】將工程師的「隨手畫的 Mermaid」轉換成能打動商業端的圖解"
source: "https://codelove.tw/@tony/post/3kklB3"
author:
published:
created: 2026-06-08
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------（30 秒讀懂結論）- 用 Mermaid 只寫結構 → 丟給專門做「資訊圖表化」的 Gem → 就會回傳白底、日..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月07日](https://codelove.tw/@tony/post/3kklB3 "2026-06-07 17:03:19")

## 【圖解】將工程師的「隨手畫的 Mermaid」轉換成能打動商業端的圖解

## 前言

（30 秒讀懂結論）

- 用 Mermaid 只寫結構 → 丟給專門做「資訊圖表化」的 Gem → 就會回傳白底、日文字體為哥德體的商務圖解
- 關鍵在於 **不要一開始就丟長文** 。先用 Mermaid 固定結構，再讓它幫你清稿
- 如果想把日文文字和官方圖示漂亮地呈現出來，Nano Banana 建議選「Pro」或「2」

這篇文章的目標讀者是這些人。

> - 會快速畫架構圖或流程圖，但要整理成簡報用圖就會覺得很麻煩的工程師
> - 開會時常被交代「這個機制之後幫我畫成圖分享一下」的人
> - 想了解 Gemini 圖像生成（Nano Banana）在工作上可以怎麼用的人

你是不是也曾經在工程師看得懂的圖和商務簡報用的圖之間，每次都花很多時間手動重畫、整理？

所以我做了一個 Gem：只要把 Mermaid 直接丟進去，它就會幫你清稿成「白底、日文字型為哥德體、帶官方圖示」的資訊圖表。提示詞只有 4 句。以下全文直接附上。

## 機制：為什麼要特地先經過 Mermaid

一開始我也是直接把想畫成圖的內容，用文章形式丟給 Nano Banana。但效果其實不太好。只要給它長篇說明，模型就會自作主張地把內容「這邊不重要吧」然後摘要掉，導致關鍵元素和箭頭方向跑掉。

這時候 Mermaid 就很有用。Mermaid 可以當作嚴格描述元素與關係的「結構中介語言」。先把結構固定好，Nano Banana 要做的就只剩清稿。這就是分工的概念。

```
腦中的模糊想法
   ↓ 結構化（人類 + Mermaid）
Mermaid（骨架、關係已確定）
   ↓ 清稿（Nano Banana）
資訊圖表（外觀整理完成）
```

把 Mermaid 想成設計者腦中的構想，Nano Banana 想成負責清稿的設計師——這樣分工就很清楚了。結構設計的責任由我們承擔，外觀呈現交給對方。先由人類把最容易失真的「摘要」步驟做完，這就是訣竅。

## 系統提示詞（全文公開）

只要把這段貼到 Gem 的自訂指示裡就行了。

gem-custom-instruction.txt

```
將輸入內容摘要並結構化，以日文哥德體生成資訊圖表圖片。請使用官方圖示進行視覺化。若輸入內容超過 30 個字，請先摘要並結構化，再進行圖片生成。背景為白色，不要標題。
```

雖然短，但每一句都有作用。依序來看：

- 「將輸入內容摘要並結構化」——不管是 Mermaid 還是長文，先轉成結構再畫。這是讓雜亂輸入也不容易崩壞的基礎
- 「以日文哥德體」——如果跑成明體或可愛字體，會很不符合簡報風格。這裡是往商務文件的標準靠攏
- 「請使用官方圖示進行視覺化」——指定要用 AWS 或各服務的官方圖示。不過後面會提到，這裡不要期待過高
- 「若輸入內容超過 30 個字」——防止你一不小心貼了長文。這裡也一樣先做摘要→結構化
- 「背景為白色，不要標題」——為了貼到簡報或 Slack 時不突兀。標題我想自己加，所以不讓它處理

這不是那種看起來很厲害的提示詞，但能穩定回傳相同品質，這點其實很有用。

## 模型選擇一定要注意（Nano Banana / Pro / 2）

這裡要補充一下，因為 2026 年上半年情況已經變了。「Nano Banana」現在已經不是單一模型。

通稱模型適合用途Nano BananaGemini 2.5 Flash Image高速、輕量。日文文字容易崩壞Nano Banana ProGemini 3 Pro Image資訊圖表、圖解、多語文字Nano Banana 2Gemini 3.1 Flash Image以 Flash 的速度達到接近 Pro 的品質這次的用途重點是「日文字能讀」以及「圖表不能破圖」。初代（2.5 Flash Image）很容易把日文弄壞，所以我現在都用 Pro 或 2。Google 也把 Nano Banana Pro 定位為適合用來視覺化點子、將資料做成資訊圖表、把手寫筆記圖解化的工具。這正好就是我想做的事。

Gem 內是否能生成圖片，以及可選的模型（Fast / Thinking / Pro），會依照方案與釋出狀況而有所不同。本文資訊以 2026 年 6 月為準。最新支援情況請以官方說明為準。

## 使用方法只有 3 步

1. 用 Mermaid 寫出你想圖解的內容（照平常做就好）
2. 把那段 Mermaid 貼到設定好上面提示詞的 Gem 裡送出
3. 直接把回傳的圖片貼到簡報、Slack 或會議記錄裡

就這樣而已。我現在只是把在 VS Code 裡寫好的 Mermaid 複製貼上，幾乎不用再花時間清稿。老實說，第一次試的時候，我還有點「這樣真的可以嗎」的錯愕感。

## 實例 3 個：Mermaid → Nano Banana

「那最後到底會出現什麼圖？」應該是大家最在意的部分。我用三種風格差很多的圖試過。每個例子都是上方為 Qiita 直接渲染的 Mermaid（= 前），下方為丟進 Gem 後的輸出（= 後）。

後面的圖片請換成你自己用 Gem 生成的版本。只要把 `![...](圖片URL)` 的 URL 部分替換成生成圖片連結即可。

### 例 1：AWS 構成圖（3 層 Web 應用）

前（Mermaid）:

後（Nano Banana）:  
[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3937496%2F63f377f5-52fe-4746-bd45-0e704bb41453.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=85dec6cbdff032d709fafb82392c14c3)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3937496%2F63f377f5-52fe-4746-bd45-0e704bb41453.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=85dec6cbdff032d709fafb82392c14c3)

原本只有箭頭的架構，變成有圖示之後，一眼就能看出「請求從哪裡來、資料會存到哪裡」。

### 例 2：資料分析基盤的管線

前（Mermaid）:

後（Nano Banana）:當你要向非工程師說明「資料從哪裡來、怎麼加工、最後誰會看」，這種清稿版非常有用。我的本業是 ID-POS 分析基盤，這個層級的圖最容易讓人理解。

### 例 3：業務流程（序列圖）

前（Mermaid）:

後（Nano Banana）:時序性的互動，如果只看序列圖，讀的人通常需要一些預備知識。清稿之後，就會變成「送出申請後，完成付款，然後收到信」這種直覺流程。要交給商務端看，這種版本更適合。

## 踩雷的地方

雖然好用，但並不是萬能。我實際遇到的 3 個問題也記一下。

第一個是 **不要對官方圖示的精確度期待過高** 。即使指示「使用官方圖示」，也不代表會出現像素級正確的 AWS 圖示，有時候只會生成外觀相近的圖示。若是對外文件，且必須完全符合品牌規範，這部分最好預設之後手動替換。

第二個是日文文字容易壞。前面提過，初代模型有時會出現像亂碼一樣的狀況。我之前就在這裡卡過一次，花了不少時間才發現問題不是提示詞，而是模型選擇（也就是根本原因在模型端）。切到 Pro 或 2 之後就解決了。

第三個是輸入太複雜的時候。如果一次把有 20 到 30 個節點的 Mermaid 整段丟進去，摘要階段可能會把部分元素刪掉。圖很大時，建議改成以子圖（subgraph）單位切開丟，這樣比較不容易崩。

把機密架構圖或系統資訊輸入雲端生成式 AI 時，務必先確認公司內部的資料處理政策。具體的主機名稱、IP、驗證資訊等，最好先改成假資料再送出。

## 總結

- 先用 Mermaid 固定結構，再交給 Nano Banana 清稿，幾乎可以消除商務圖解的手工整理成本
- 提示詞只有 4 句，直接複製貼到 Gem 就能用
- 想要日文與圖解品質，模型請選 Pro 或 2
- 官方圖示的嚴格精準度，以及機密資訊處理，這兩點要特別注意

老實說，我沒想到清稿這件事可以這麼輕鬆外包出去。跟我一樣「Mermaid 會畫，但整理成簡報很麻煩」的人，不妨試著做一個這樣的 Gem。若你有更好的提示詞改良方案，也歡迎留言告訴我。接下來我想寫的是，把這個 Gem 跟排程執行結合，讓定期簡報自動更新的做法。

---

原文出處： [https://qiita.com/ktdatascience/items/4b35eb4e157becfac073](https://qiita.com/ktdatascience/items/4b35eb4e157becfac073)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3kklB3) [305瀏覽](https://codelove.tw/@tony/post/3kklB3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3kklB3)

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

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝6 💬2

337

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780890299&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3kklB3&amp;pra=5&amp;asro=0&amp;itsi=5&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780890298974&amp;bpp=2&amp;bdt=533&amp;idt=48&amp;shv=r20260604&amp;mjsv=m202606020101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780889738%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780889738%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780889738%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1337364769413&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390667%2C95393281&amp;oid=2&amp;pvsid=7376857386760795&amp;tmod=473573085&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=88" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=2&amp;uci=a!2&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>