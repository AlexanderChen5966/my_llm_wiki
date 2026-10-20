---
title: "AI 時代，Google 在 Android 官方都做了哪些支援？"
source: "https://codelove.tw/@tony/post/xNQodq"
author:
published:
created: 2026-06-01
description: "AI 時代，Google 在 Android 官方都做了哪些支援？  ===============================    AI 時代，Google 在 Android 官方都做了哪些支援？----------------------------之前我們一直在..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月31日](https://codelove.tw/@tony/post/xNQodq "2026-05-31 15:03:44")

## AI 時代，Google 在 Android 官方都做了哪些支援？

## AI 時代，Google 在 Android 官方都做了哪些支援？

```
AI 時代，Google 在 Android 官方都做了哪些支援？
```

---

之前我們一直在聊 Flutter 在 AI 時代官方支援的各種能力，今天就來聊一聊 Android 官方做了哪些 AI 時代的支援。

第一個其實就是 AppFunctions，我們在之前的 [《Android 官方正式官宣 AI 支援 AppFunctions》](https://juejin.cn/post/7611069767160840246) 就聊過，官方正式將它定位成了 Android MCP，也就是你的 App 可以為 AI 提供 MCP 的呼叫服務：

[![](https://i.imgur.com/R6ENyrD.jpeg)](https://i.imgur.com/R6ENyrD.jpeg)

比如使用者只需要告訴 Gemini「從圖庫裡找找我貓的照片」，Gemini 會接收使用者的查詢，智慧辨識並觸發相應的功能，然後就可以直接把圖庫中的照片顯示在 Gemini 裡，這就是 AppFunctions 的支援：

[![](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/114d33ebf2db4eaa9f88d4fdcae22794~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1780534797&x-signature=IULjntx8DFVXrR7MNDgdVbjbwU8%3D)](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/114d33ebf2db4eaa9f88d4fdcae22794~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1780534797&x-signature=IULjntx8DFVXrR7MNDgdVbjbwU8%3D)

AppFunctions 簡單來說就是 **讓你的 App 能被 AI 當成工具呼叫** ，應用程式把一部分能力（資料／動作）以「自我描述函式」的方式暴露出來，AI agent（例如 Gemini）能夠發現並執行這些函式，從而完成跨 App 的任務， **而這個過程不需要打開目標 App** 。

**另外就是 Gemma 4，Gemini Nano（Nano 4）的端側能力也在往可量產推進** ，透過支援 Prefix Caching，利用 prompt API 優化裝置上的推理效能，新的 Prefix Caching 透過儲存和重用處理提示中共享且重複部分的中間 LLM 狀態來縮短推理時間。

> App 可以透過 AIcore 開發者預覽版進行測試，也可以使用 LiteRT-LM 進行自訂化，簡單的本地任務辨識與執行還是可以的，特別還支援結構化 JSON

接著就是 **A2UI Jetpack Compose Renderer** ，是的，之前我們聊過的 [《Flutter GenUI 0.9 和 A2UI 0.9 發布》](https://juejin.cn/post/7640409934489010230) 也即將支援 Android 了， **也就是 Compose 可以支援 AI Agent 動態產生 UI 了** ，當然，這個叫 *A2UI Jetpack Compose Renderer* ，所以肯定沒有 XML 什麼事，畢竟官方也宣布了 [XML 不再更新了](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2Fl2sTkojVxPiu8jcl1AR1Ug) 。

[![](https://i.imgur.com/pxcbe6I.jpeg)](https://i.imgur.com/pxcbe6I.jpeg)

A2UI 協議本身並不直接產出程式碼，因為 A2UI **模型輸出的也不是 Dart、JS、HTML 這種具體程式碼，對應實際輸出的是一種結構化的 UI 描述** ，所以只要你的 SDK 適配了這種描述，就可以支援類似能力：

[![](https://i.imgur.com/WysF4Io.jpeg)](https://i.imgur.com/WysF4Io.jpeg)

比如不久前高德就宣布了首個生成式 UI 開源框架 AGenUI，所以基本上只要你適配了 A2UI，什麼平台都可以實現 Agent 生成能力：

[![](https://i.imgur.com/0u1LMfL.jpeg)](https://i.imgur.com/0u1LMfL.jpeg)

**最後一個就是 ADK for Android** ，這也是 Google 首個官方面向 Android 的 **Agent Development Kit** ，用於開發和執行 AI 代理：

[![](https://i.imgur.com/RQgFKq5.jpeg)](https://i.imgur.com/RQgFKq5.jpeg)

ADK 支援在裝置端和雲端模型上建構多代理工作流程，同時管理代理之間的編排、上下文處理和會話，這裡面 **混合編排** 是主打能力，可以將雲端模型用作主編排器，然後把特定任務放在裝置端的子代理執行，ADK 函式庫負責將通用代理實作適配到正確的雲端或裝置端 API。

> 你說 Koog 不也是 Kotlin 的 Android 支援嗎？那個是 JetBrains 家的，而且支援的是 KMP 場景。

**加上之前聊過的 Android CLI 帶有的 skill、doc 等能力** ，目前 Android 也算是全面跟上 AI 開發時代了，還差的就是一套有效的 Android MCP，可以提供類似 Dart MCP 那樣更好的 AI 除錯能力。

[![](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/e41dc416b56740a58de6a23a5ecddc25~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1780534797&x-signature=TRwP%2FsfxjzHCwLkATiDcbc98DT4%3D)](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/e41dc416b56740a58de6a23a5ecddc25~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1780534797&x-signature=TRwP%2FsfxjzHCwLkATiDcbc98DT4%3D)

只能說，2026 了，沒有什麼框架敢不提供和跟進 AI，畢竟不支援等於放棄了浪潮，你覺得 Android 這波還少了哪些？

---

原文出處： [https://juejin.cn/post/7644480592969023531](https://juejin.cn/post/7644480592969023531)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xNQodq) [5瀏覽](https://codelove.tw/@tony/post/xNQodq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xNQodq)

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

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝12 💬4 ❤️1

465

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

87

🥉

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

#4

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780297285&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxNQodq&amp;pra=5&amp;asro=0&amp;itsi=8&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTgxIixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xODEiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE4MSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780297284796&amp;bpp=1&amp;bdt=277&amp;idt=418&amp;shv=r20260527&amp;mjsv=m202605260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780297054%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780297054%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780297054%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4885900129493&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31098751%2C95390668%2C95390787%2C95391803&amp;oid=2&amp;pvsid=3404977891020931&amp;tmod=1412125679&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=441" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260527/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=2&amp;uci=a!2&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>