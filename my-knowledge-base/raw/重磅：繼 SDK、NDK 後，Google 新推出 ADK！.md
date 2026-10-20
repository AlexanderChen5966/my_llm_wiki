---
title: "重磅：繼 SDK、NDK 後，Google 新推出 ADK！"
source: "https://codelove.tw/@tony/post/3VzjD3"
author:
published:
created: 2026-06-09
description: "重磅：繼 SDK、NDK 後，Google 新推出 ADK！  ========================    Google 發布了 `ADK for Kotlin` 和 `ADK for Android` 的 `0.1.0` 版本。以後不只有 SDK、NDK，還有 ADK。`..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月08日](https://codelove.tw/@tony/post/3VzjD3 "2026-06-08 18:15:11")

```
Google 發布了 \`ADK for Kotlin\` 和 \`ADK for Android\` 的 \`0.1.0\` 版本。
```

以後不只有 SDK、NDK，還有 ADK。

`ADK for Kotlin` 用在 Kotlin 後端和通用 JVM 場景， `ADK for Android` 用在 Android App 裡，主要處理端側模型和 App 內上下文。

## 添加依賴

在 Android 工程裡先加這個依賴：

```
bash 体验AI代码助手 代码解读复制代码dependencies {
    implementation("com.google.adk:google-adk-kotlin-core-android:0.1.0")
}
```

如果是一般 Kotlin 後端或 JVM 專案，用的是 `google-adk-kotlin-core` 。需要透過註解生成工具程式碼時，還要加 KSP processor：

```
bash 体验AI代码助手 代码解读复制代码dependencies {
    implementation("com.google.adk:google-adk-kotlin-core:0.1.0")
    ksp("com.google.adk:google-adk-kotlin-processor:0.1.0")
}
```

`0.1.0` 還是實驗版本。它現在更像 Demo 專案裡的嘗鮮依賴，不適合直接接正式業務。Agent 呼叫失敗以後回傳什麼、模型不可用時怎麼降級、工具有沒有權限，這些都要在 App 程式碼裡處理。

## Agent 寫法

寫 Agent 時會用到 `LlmAgent` 。一個 Agent 裡會放模型、名稱、描述、系統指令、工具和子 Agent。下面是一個簡化後的旅遊助理。

旅遊助理裡有一個雲端 orchestrator，它先理解使用者的旅遊問題，再把訂票確認、飯店資訊、租車資訊這類任務交給裝置上的子 Agent 處理。訂票確認可能來自使用者本機文件，這一步可以交給 Gemini Nano 這類端側模型解析。程式碼只看核心欄位：

```
bash 体验AI代码助手 代码解读复制代码val orchestrator = LlmAgent(
    name = "trip_orchestrator",
    model = Gemini(apiKey = apiKey, name = MODEL_NAME),
    instruction = Instruction(
        """
        You are a travel assistant.
        Use get_trip_details to read the trip plan.
        Delegate booking checks to local sub agents when needed.
        """.trimIndent()
    ),
    tools = listOf(GetTripDetailsTool(tripId)),
    subAgents = listOf(carRentalPipeline, hotelPipeline),
    // 其他路由限制參數先省略，具體語義要看官方原始碼和 demo
)
```

這段程式碼裡， `model` 決定目前 Agent 要呼叫哪個模型， `instruction` 決定它怎麼理解任務， `tools` 是可呼叫能力， `subAgents` 是它可以轉交任務的下游 Agent。官方範例裡還有一些 transfer 相關參數，這裡不按名稱展開說明，等真的看原始碼或 demo 時再確認。

以前在 App 裡接一個大模型，很多時候只是把使用者輸入送到雲端介面，再把回覆顯示出來。換成 Agent 之後，ViewModel 裡那些「先查行程、再判斷訂單、最後組回覆」的分支會往 Agent 端挪，App 端更像是在等一個最終的 UI State。

放到 Android 裡之後，工具呼叫會碰到執行緒和生命週期。比如工具裡要讀取本地行程檔案，就要處理檔案不存在和讀取耗時；如果訂單資訊來自本地資料庫，也不要在主執行緒裡直接查詢。

## 工具呼叫

ADK for Kotlin 支援用 `@Tool` 和 `@Param` 描述函式工具。工具本質上是把本地函式暴露給 Agent，讓模型在合適的時候呼叫它。

比如把一個訂單查詢能力交給 Agent，先看服務類：

```
bash 体验AI代码助手 代码解读复制代码class TripService {
    @Tool
    fun getBookingStatus(
        @Param("Booking id from the user's trip document")
        bookingId: String
    ): String {
        return repository.findBookingStatus(bookingId)
    }
}
```

`@Tool` 和 `@Param` 只是描述工具，還不是完整接入。官方 Kotlin 範例裡會透過 `google-adk-kotlin-processor` 做 KSP 生成，然後才能在 Agent 裡使用類似 `generatedTools()` 的結果。

也就是說，如果程式碼裡寫成下面這樣還不夠：

```
bash 体验AI代码助手 代码解读复制代码dependencies {
    implementation("com.google.adk:google-adk-kotlin-core:0.1.0")
    ksp("com.google.adk:google-adk-kotlin-processor:0.1.0")
}
```

Android 工程還要配置 KSP 插件。實際專案裡一般會在根專案或版本目錄裡宣告 `com.google.devtools.ksp` ，再在模組裡套用插件。KSP 版本要和 Kotlin 版本對齊，這裡不寫死版本，舊專案直接照搬很容易卡在 Gradle Sync。

再配置 Agent 時， `generatedTools()` 不是手寫出來的一般方法，而是註解處理後的產物。沒有 KSP 插件和 processor 這兩步，直接照抄這段程式碼大概率會編譯不過。

```
bash 体验AI代码助手 代码解读复制代码val bookingAgent = LlmAgent(
    name = "booking_agent",
    description = "Checks booking status from local trip data.",
    model = Gemini(apiKey = apiKey, name = "gemini-2.5-flash"),
    instruction = Instruction("Use tools to verify booking status."),
    tools = TripService().generatedTools()
)
```

這裡的 `@Param` 不只是給開發者看的註解，它會影響模型理解參數含義。參數從哪裡來、格式是什麼、失敗時回傳什麼，都要寫清楚。

如果工具函式裡要讀本地行程檔案，這些邏輯仍然走原本的 Android 程式碼。Agent 只拿工具回傳的結果，不應該跳過檔案不存在、讀取失敗和權限拒絕這些分支。

## Session 和 Memory

這裡還有一個容易踩坑的地方：狀態。一個 Agent 問使用者補充資訊，另一個 Agent 要知道前面已經確認過什麼；工具呼叫失敗以後，後面的 Agent 也要知道這一步失敗了。否則上游吐了一個空結果，下游還繼續拼 prompt，最後使用者看到的就是一段像正常回答的幻覺回覆。

ADK 裡有 Session state 和 Memory service。Session state 更像短期狀態，服務於目前會話裡的上下文共享；Memory service 更偏長期記憶，可以保存跨會話的資訊。

在 Android 工程裡使用這些能力時，不要簡單理解成 `SharedPreferences` 或聊天紀錄。Agent 狀態可能包含使用者輸入、模型輸出、工具呼叫參數、本地資料摘要和錯誤資訊。哪些能落盤，哪些只能留在記憶體，哪些不能進日誌，需要先訂好規則。

## 最後

我真的學不動了。

[#Android](https://link.juejin.cn/?target=) [#Kotlin](https://link.juejin.cn/?target=) [#ADK](https://link.juejin.cn/?target=) [#GeminiNano](https://link.juejin.cn/?target=) [#AI](https://link.juejin.cn/?target=)

---

原文出處： [https://juejin.cn/post/7647438301877501978](https://juejin.cn/post/7647438301877501978)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3VzjD3) [17瀏覽](https://codelove.tw/@tony/post/3VzjD3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3VzjD3)

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

339

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780973997&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3VzjD3&amp;pra=5&amp;asro=0&amp;itsi=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780973997686&amp;bpp=1&amp;bdt=70&amp;idt=46&amp;shv=r20260604&amp;mjsv=m202606030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780973912%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780973912%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780973912%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1306653338821&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390668%2C42533294%2C95392291&amp;oid=2&amp;pvsid=8704597867147364&amp;tmod=173356463&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=57" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=2&amp;uci=a!2&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>