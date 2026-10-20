---
title: "Android 17 記憶體管理將嚴格管控，App 要注意適配"
source: "https://codelove.tw/@tony/post/3KXbQa"
author:
published:
created: 2026-06-08
description: "Android 17 記憶體管理將嚴格管控，App 要注意適配  =================================    從 Android 17 開始，Google 官方開始對記憶體進行嚴格管控，**App 如果長期占用過多匿名記憶體 / swap，系統會依設備總 RAM ..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月07日](https://codelove.tw/@tony/post/3KXbQa "2026-06-07 16:02:14")



## Android 17 記憶體管理將嚴格管控，App 要注意適配

```
從 Android 17 開始，Google 官方開始對記憶體進行嚴格管控，**App 如果長期占用過多匿名記憶體 / swap，系統會依設備總 RAM 給它加上限制，超限後可能直接殺掉程序，而且沒有一般的崩潰堆疊**。
```

> 也就是說，你繼續占用太多，我就把你「暗殺」掉，然後你只看 crash 日誌還不知道自己被殺過。

以前 Android 系統的記憶體治理主要還是靠 LMK（Low Memory Killer），系統整體記憶體緊張時，會按程序優先級殺掉背景、快取程序，只有最嚴重時才會殺前台，而 Android 17 的新邏輯是：

> **不能讓一個 App 因為記憶體洩漏或占用過大，就把系統流暢度給掀了。**

這突然讓我想起了之前那篇 [《抖音“極客”適配 Android 5 ~ 9 等老機型技術解讀，都是騷操作》](https://juejin.cn/post/7625239770092797995) ，基本結論就是，App 為了讓使用者用得更流暢，你不給的我自己想辦法拿，這麼一對比貌似還是反著來的。

而在 Android 17 這裡，官方給了一個例子：

> 如果一個 App 持有前台服務等較高優先級狀態，同時又不斷膨脹記憶體，傳統 LMK 一開始不會優先殺它，這就導致系統只能去殺很多「小而正常」的快取 App 來回收記憶體，結果就是使用者返回其他 App 時本來應該 warm resume，結果反而變成冷啟動，還增加 CPU 壓力和耗電。

所以 Android 17 這次引入新的限制： **按設備總 RAM 給 App 加記憶體上限，有線打擊極端洩漏和離群占用。**

這次調整最有意思的就是， **它不一定表現為 Java/Kotlin OOM，也不一定有 crash stack** ，官方給出的判斷方式是用 `ApplicationExitInfo.getDescription()` ，如果被 Android 17 的 memory limiter 影響，exit reason 會是 `REASON_OTHER` ，description 字串裡會包含 `MemoryLimiter:AnonSwap` 。

> 也就是說，你以後排查線上「無堆疊死亡」、「使用者說 App 突然沒了」、「前台服務 App 莫名被殺」時，不能只看 Crashlytics/Java crash 了，還需要把 `ApplicationExitInfo` 也加入啟動時診斷鏈路。

這個適配真的很重要，因為你不做真的就直接抓瞎，比如 App 下次啟動時讀取歷史退出原因，如果發現 `REASON_OTHER + MemoryLimiter:AnonSwap` ，就打點上報：

```
val activityManager = getSystemService(ActivityManager::class.java)
val exitReasons = activityManager.getHistoricalProcessExitReasons(
    packageName,
    0,
    10
)

for (info in exitReasons) {
    val desc = info.description ?: ""
    if (info.reason == ApplicationExitInfo.REASON_OTHER &&
        desc.contains("MemoryLimiter:AnonSwap")
    ) {
        // 上報：疑似 Android 17 memory limiter 殺掉程序
    }
}
```

> 因為這個不是一般 crash，屬於系統層面的強制結束，所以只靠例外攔截沒用。

這裡官方也給出了幾個適配方案，比如 R8 位元碼最佳化、App 不可見時主動 trim memory 等，在幾個適配建議裡，Google 把 R8 放在第一位，官方建議以下設定全開，同時使用全新的 `proguard-android-optimize.txt` ：

```
isMinifyEnabled = true
isShrinkResources = true
```

[![image-20260604094653901](https://i.imgur.com/jR4JCW5.jpeg)](https://i.imgur.com/jR4JCW5.jpeg)

這是一個大家都不太想動的東西，而官方明確說，舊的 `proguard-android.txt` 會阻止最佳化，並且在 Android Gradle Plugin 9 也不再支援，還要求移除 `android.enableR8.fullMode = false` 。

不過這對很多老專案來說確實很要命，因為不少專案的 ProGuard/R8 設定裡有這種「祖傳保命三件套」：

```
-dontoptimize
-dontshrink
-dontobfuscate
```

不過 Google 也說了，這些全域開關會讓 R8 對整個 codebase 失去最佳化空間，官方還推出了 **R8 Configuration Analyzer** ，它會給出 shrinking、optimization、obfuscation 三類分數，告訴你哪些 keep rule 阻止了多少類別、方法、欄位被最佳化。

[![](https://i.imgur.com/tT1gqm2.jpeg)](https://i.imgur.com/tT1gqm2.jpeg)

> 從 AGP 9.3.0-alpha05 開始，R8 建置時會自動在 `build/outputs/mapping/release/configanalyzer.html` 產生報告。

其次就是圖片，老生常談了，Bitmap 往往是 App 裡最大的一類常駐物件，JPEG/PNG 是壓縮檔，但顯示時會解碼成原始像素資料， **記憶體消耗取決於像素尺寸 × 色彩格式** ，一張 100KB 的壓縮圖，解碼後可能需要占幾 MB，所以 Google 建議：

- 降採樣，不要把一張超大圖解碼後塞進一個小 thumbnail，手寫 Bitmap 載入時用 `inSampleSize` ；Glide / Coil 預設會做 downsample，但也要檢查設定
- **不要把 padding 放到圖片裡** ，比如為了留白，直接做一張大透明邊框 PNG，應該用 View / Composable 的 padding，或者 `InsetDrawable`
- **合理選擇 Bitmap Config** ，不需要透明通道時可以考慮 `RGB_565` ，它比預設 `ARGB_8888` 少一半記憶體，當然代價是色彩品質和透明能力
- **簡單幾何圖形優先用 Vector / ShapeDrawable** ，不要什麼都切 PNG
- **手動管理 Bitmap 時注意重用和釋放**

Android Studio Narwhal 4 的 Profiler 也增加了重複 Bitmap 檢測：Heap Dump 後可以看黃色警告，或者用 Duplicate Bitmaps 篩選器，直接找到重複儲存的圖片。

[![](https://i.imgur.com/DynG1el.jpeg)](https://i.imgur.com/DynG1el.jpeg)

另外記憶體洩漏也是常見問題。典型問題包括：

- **洩漏 Context** ，比如在 Compose 裡把 `LocalContext.current` 傳給 ViewModel，View 系統裡把 Activity 放進 companion object / static 變數，都容易造成洩漏，正確做法是 UI 層用 Context，非 UI 層透過依賴注入或狀態流轉解決，不要持有 Activity
- **洩漏 Listener** ，Compose 裡 `DisposableEffect` 註冊了 listener，但 `onDispose` 裡沒註銷
- **洩漏 View** ，比如 `AndroidView` 包了一套 legacy View，但沒有 release 策略，Fragment 裡持有 view binding，在 `onDestroyView()` 後沒清空

> 實際上 Android Studio Panda 3 裡就已經有專門的 LeakCanary profiler task，可以把洩漏分析從裝置端搬到開發機端，同時和 IDE 原始碼跳轉整合。

[![](https://i.imgur.com/WvssMAH.jpeg)](https://i.imgur.com/WvssMAH.jpeg)

另外就是 `onTrimMemory` ，Android 17 下更應該主動釋放快取， **不要等系統來決定回收什麼記憶體，自己應該在合適時機釋放可以重建的資源** ，比如實作 `ComponentCallbacks2.onTrimMemory()` ，尤其是全域快取可以放在 `Application` 層管理。

這裡主要關注： `TRIM_MEMORY_UI_HIDDEN` 和 `TRIM_MEMORY_BACKGROUND` ，因為從 Android 14 開始，系統已經不再發送其他 legacy constants：

- `TRIM_MEMORY_UI_HIDDEN` ：你的 UI 已經不可見，可以釋放和介面強綁定的大物件，比如 Bitmap 快取、影片播放 buffer、複雜動畫資源
- `TRIM_MEMORY_BACKGROUND` ：程序已經在背景，是系統記憶體緊張時的候選終止對象，這時應該更積極釋放那些恢復時能低成本重建的資源，換取更久的 cached 存活，減少冷啟動
```
class App : Application(), ComponentCallbacks2 {

    override fun onTrimMemory(level: Int) {
        super.onTrimMemory(level)

        if (level >= ComponentCallbacks2.TRIM_MEMORY_UI_HIDDEN) {
            // 清 UI 相關快取：圖片、動畫、影片 buffer、頁面暫存物件
        }

        if (level >= ComponentCallbacks2.TRIM_MEMORY_BACKGROUND) {
            // 更積極清理：可重建的全域 cache、暫時資料、背景非必要資源
        }
    }
}
```

除此之外，官方還推薦 `ProfilingManager` ，本地複現不了的記憶體問題最難搞，所以 Android 15 引入了 `ProfilingManager` ，可以讓 App 程式化收集真實使用者裝置上的 Perfetto profiles，現在 Android 17 又引入了新的事件驅動 trigger `TRIGGER_TYPE_OOM` 和 `TRIGGER_TYPE_ANOMALY` ：

- `TRIGGER_TYPE_OOM` ：在 OutOfMemoryError crash 的精確時刻自動收集 Java heap dump，收集到的結果會在 App 下次啟動並註冊 `registerForAllProfilingResults` 回呼後提供
- `TRIGGER_TYPE_ANOMALY` ：用於嚴重效能異常，例如 excessive binder spam 或突破記憶體閾值，對於 memory anomaly，它會在系統終止 App 前收集 heap dump。

> 這對 Android 17 的這次記憶體限制很關鍵：以前你可能只知道「程序被殺了」，現在可以在接近被殺前拿到 heap dump，知道是誰占了記憶體。

另外官方還推薦用 Perfetto UI 的 Heap Dump Explorer 分析 heap dump，這樣可以看物件配置層級、retained size、到 GC root 的最短路徑，也能透過 flamegraph 找大物件。

[![](https://i.imgur.com/3XGRpFW.jpeg)](https://i.imgur.com/3XGRpFW.jpeg)

最後，Android 17 也提供了 `adb shell am memory-limiter` 命令，它只對啟用了 memory limiter 的裝置有效，比如有：

- `adb shell am memory-limiter status` ：查看目前 memory limiter 狀態，包括 visible / non-visible 程序限制
- `adb shell am memory-limiter ignore <uid>|none|all` ：讓 memory limiter 忽略某個 UID、全部 App，或者恢復不忽略
- `adb shell am memory-limiter manual <pid> <limit>|max|none` ：給某個程序手動設定限制，比如 `adb shell am memory-limiter manual 12345 300` ，把 PID 12345 的程序限制到 300MB

可以看出來，這次 Android 17 的記憶體限制變動會很大，以至於官方都不得不給出一系列建議來督促大家趕緊適配，而且 Android 17 也馬上就要來了，其他都還是次要的，避免 App 被刺殺才是最重要的。

我就問你一句，你開 R8 full 嗎？

## 連結

[android-developers.googleblog.com/2026/06/pri…](https://link.juejin.cn/?target=https%3A%2F%2Fandroid-developers.googleblog.com%2F2026%2F06%2Fprioritizing-memory-efficiency-steps-for-android-17.html)

---

原文出處： [https://juejin.cn/post/7647356541462806580](https://juejin.cn/post/7647356541462806580)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3KXbQa) [298瀏覽](https://codelove.tw/@tony/post/3KXbQa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3KXbQa)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780888487&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KXbQa&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780888487784&amp;bpp=3&amp;bdt=1213&amp;idt=82&amp;shv=r20260604&amp;mjsv=m202606020101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780628452%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780628452%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780628452%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6004613956094&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390667%2C95390787%2C42533294%2C95392418%2C95340253%2C95340255&amp;oid=2&amp;pvsid=8292199932363943&amp;tmod=473573085&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=119" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>