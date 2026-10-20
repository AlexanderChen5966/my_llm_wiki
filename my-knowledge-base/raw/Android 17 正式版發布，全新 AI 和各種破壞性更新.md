---
title: "Android 17 正式版發布，全新 AI 和各種破壞性更新"
source: "https://codelove.tw/@tony/post/37lYva"
author:
published:
created: 2026-06-22
description: "Android 17 正式版發布，全新 AI 和各種破壞性更新  ==================================    說了這麼久的 Android 17 終於來正式版了，這也是 Android 系統上第一個 AI 升級，同時也是隱私又進一步收緊，需要各種適配的版本。..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月19日](https://codelove.tw/@tony/post/37lYva "2026-06-19 12:11:00")

## Android 17 正式版發布，全新 AI 和各種破壞性更新

## Android 17 正式版發布，全新 AI 和各種破壞性更新

```
說了這麼久的 Android 17 終於來正式版了，這也是 Android 系統上第一個 AI 升級，同時也是隱私又進一步收緊，需要各種適配的版本。
```

[![](https://i.imgur.com/LcQ4gI4.jpeg)](https://i.imgur.com/LcQ4gI4.jpeg)

對開發者來說這次主要變化應該有：

- **App 支援被 AI Agent 呼叫**
- **要適配任意視窗和大螢幕**
- **要控制記憶體**
- **要處理更嚴格的本機網路**
- **動態程式碼載入**
- **背景音訊**
- **OTP**
- **憑證透明度**
- **NPU 聲明等行為變化**

> 這次 Android 17 正式發布後，目前 Pixel 已經開放更新，同時 AOSP 也同步開放。
> 
> Android作業系統

## AI

這次更新最重要的就是 *An intelligence system* ，基本 Android 未來也是往這方面轉變，就像前段時間的 WWDC 裡蘋果也是， **這一次 Android 17 是 Android 從傳統作業系統轉向「智慧系統」的開端** ，官方強調的是硬體、軟體和 AI 的深度整合，讓系統可以主動理解使用者需求，把 App 放在 AI 工作流中心。

說人話就是，現在支援了 **AppFunctions** ，這個我們已經聊過好多次了，簡單來說，以前 App 只是一個介面，AI 最多開啟你的 App 進行操作，而現在 App 可以把自己的能力暴露成「工具」，讓系統級 AI Agent 能發現和執行：

這個在之前的 [《Android 官方正式官宣 AI 支援 AppFunctions ，Android 官方 MCP 和系統級 OpenClaw 雛形》](https://juejin.cn/post/7611069767160840246) 我們就詳細聊過，例如：

> 一個筆記 App，可以把「建立筆記」「搜尋筆記」「整理待辦」等能力做成 AppFunction，未來 AI 助理就可以直接呼叫這些能力，而不是只跳轉 App 頁面。

AppFunctions 就是 Android MCP 的可編排工具，AI Agent 可以發現並執行這些 AppFunctions，並且能存取 App 的本地狀態。

目前 Jetpack AppFunctions 函式庫目前是 alpha，開發方式是透過 Kotlin 註解 `@AppFunction` 和 KDoc 註解描述函式，讓 LLM 更好理解這個工具怎麼呼叫：

```
kotlin 代码解读复制代码/**
 * A note app's [AppFunction]s.
 */
class NoteFunctions(
    private val noteRepository: NoteRepository
) {
    /**
     * Adds a new note to the app.
     *
     * @param appFunctionContext The execution context.
     * @param title The title of the note.
     * @param content The note's content.
     */
    @AppFunction(isDescribedByKDoc = true)
    suspend fun createNote(
        appFunctionContext: AppFunctionContext,
        title: String,
        content: String
    ): Note {
        return noteRepository.createNote(title, content)
    }
}
```

> Google 還發布了一個 **AppFunctions agent skill** ，可以分析 App 的關鍵流程、自動生成 Kotlin 程式碼、最佳化 KDoc、並提供 ADB 指令做測試和除錯。
> 
> 軟體

## Adaptive-first

Android 17 官方也宣布明確 **adaptive-first development standard** ，其實在此之前，官方也官宣了 Compose First，也就是 XML 模式已經成為歷史，不再更新，所以「自適應優先」也是 Compose 這種回應式框架的基礎。

目前使用者已經不只在手機上用 App，而是在手機、摺疊螢幕、平板、筆記型電腦形態、車機、XR 等裝置之間切換， **官方統計目前已有超過 5.8 億大螢幕 Android 裝置** ，而且未來還有基於 Android stack 的下一代 ChromeOS「Googlebooks」。

說這麼多，核心就是想告訴大家：

> **當 App target Android 17 / API 37 後，在大螢幕裝置上不能再拒絕橫豎螢幕、縮放、自由視窗和比例變化。**

具體說就是 Android 17 對 sw > 600dp 的大螢幕裝置，會移除開發者對方向和尺寸限制的 opt-out，系統會忽略舊的 manifest 屬性和執行階段 API，包括：

- `screenOrientation`
- `setRequestedOrientation()`
- `resizeableActivity=false`
- `minAspectRatio / maxAspectRatio`

也就是說你以前可以強行鎖直向、禁止 resize、限制視窗比例，但是到了 targetSdk 37，在大螢幕上這些限制會被系統無視。

> 遊戲按 Google Play app category 還可以豁免。

實際上這個之前我們也聊過了，核心就是讓你盡早做好適配，目前還是有大量還在鎖直向、強依賴單 Activity 固定尺寸版面的 App。

## 新多工形態

在 Android 17 目前加了幾類新視窗能力，其實也是進一步強化「App 適配動態尺寸」的能力：

軟體

- 第一是 **App Bubbles** ，它不再只是聊天氣泡 API，使用者可以長按 launcher 圖示，把任意 App 轉成浮動氣泡，這個能力涵蓋手機、摺疊螢幕和平板
- 第二是 **Bubble Bar** ，在平板和摺疊螢幕等大螢幕上，系統工作列會有專門的 Bubble Bar，用來組織、切換和停靠這些浮動 App 氣泡
- 第三是 **Desktop interactive PiP** ，傳統 PiP 通常只是唯讀小窗，比如影片播放，而 Android 17 的桌面互動式 PiP 是可互動的，而且可以置頂在其他視窗上

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/28f617fb5e0b445583893765f9c79e8f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1782268040&x-signature=6vIajXa7ECNM86EUaw0J60fQFW4%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/28f617fb5e0b445583893765f9c79e8f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1782268040&x-signature=6vIajXa7ECNM86EUaw0J60fQFW4%3D)

## Activity 變化

從 Android 17 開始，官方修改了 Activity 對部分設定變化的預設處理。

Android作業系統

為了減少狀態遺失和卡頓，系統對一些「不需要完整 UI 重繪」的設定變化，不再預設重啟 Activity，而是透過 `onConfigurationChanged()` 通知正在執行的 Activity， **涉及的設定包括鍵盤、鍵盤隱藏、導覽、觸控螢幕、色彩模式等** 。

> 如果你的 App 明確依賴 Activity 重啟來重新載入資源，那麼現在需要用新的 manifest 屬性 `android:recreateOnConfigChanges` 明確宣告

這個變化的實際影響其實有點抽象：

> 以前很多 App 靠 Activity recreate 偷懶刷新資源，現在需要檢查設定變化後的 UI 狀態、資源切換、輸入法/鍵盤場景、外接裝置狀態等場景是否正確適配。

## Continue On：

Android 17 新增的 **Continue On** 這個之前我們也詳細聊過，主要就是用來讓使用者在 Android 裝置之間繼續任務。

比如使用者剛在手機上打開某個 App，到了平板上，平板工作列可以出現一個接續建議，使用者點一下就能打開同一個 App，並透過 deep link 回到上次停留的位置。

> 這個目前一些場景也有類似的了。

[![](https://i.imgur.com/N88HJiL.jpeg)](https://i.imgur.com/N88HJiL.jpeg)

不過 Continue On 還支援 **App-to-Web** ，也就是如果平板上沒裝 App，可以 fallback 到網頁繼續，官方範例裡，Activity 需要呼叫 `setHandoffEnabled(true, null)` 開啟 handoff，並實作 `onHandoffActivityDataRequested()` 回傳接續資料。

軟體

```
kotlin 代码解读复制代码class MyHandoffActivity : Activity() {
​
    ...
​
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // Do stuff
    ...
    // Enable handoff
    setHandoffEnabled(true, null)
  }
​
  // Override and implement onHandoffActivityDataRequested
  override fun onHandoffActivityDataRequested(handoffRequestInfo: HandoffActivityDataRequestInfo) : HandoffActivityData {
    // Create and return handoff data
  }
}
```

也就是現在官方覺得 Android 生態要做跨裝置連續體驗，類似「手機到平板/桌面」的任務遷移，而對於 App 來說，業務頁面需要有可靠 deep link、狀態恢復、帳號同步和跨端 fallback。

## Compose-first

官方再一次官宣 *Android development is now Compose-first* ， **Google 表示，新的 Android API、函式庫、工具和開發者指引都會優先圍繞 Jetpack Compose 建構** 。

> 傳統 View 元件和 View-based Jetpack 函式庫，比如 Fragment、RecyclerView、ViewPager，進入 maintenance mode，不再獲得新功能支援。

另外谷歌也發布了 *Jetpack Compose adaptive skill* ，這個 AI workflow 可以輔助實作自適應導覽、多面板版面、FlexBox/Grid、非觸控輸入、動態視窗狀態等。

## 效能

Android 17 在效能側有不少「會影響 App 生死」的變化，這個之前我們在 [《Android 17 記憶體管理將嚴格管控，App 要注意適配》](https://juejin.cn/post/7647356541462806580) 也聊過。

Android作業系統

首先是 **App memory limits** ，Android 17 開始會根據裝置總 RAM 執行更嚴格的 App 記憶體限制，如果前台 App 或前台服務記憶體沒管理好，系統可以直接終止違規進程，而且你還不會收到 crash 訊號。

谷歌建議可以使用幾個新的工具來輔助：

- R8 full mode 和新的 R8 configuration analyzer，用來減少位元碼記憶體占用 [![](https://i.imgur.com/6PlftHX.jpeg)](https://i.imgur.com/6PlftHX.jpeg)
- 如果 App 因記憶體限制被殺， `ApplicationExitInfo.getDescription()` 會回傳 `"MemoryLimiter:AnonSwap"`
- `` `ProfilingManager `` 支援 `TRIGGER_TYPE_ANOMALY` ，可以在觸發記憶體限制時自動抓 heap dump
```
scss 代码解读复制代码val profilingManager = applicationContext
   .getSystemService(ProfilingManager::class.java)
​
val triggers = ArrayList<ProfilingTrigger>().apply {
  add(ProfilingTrigger.Builder(
    ProfilingTrigger.TRIGGER_TYPE_ANOMALY).build())
}
profilingManager.addProfilingTriggers(triggers)
```
- Android Studio Panda Profiler 整合 LeakCanary，作為 IDE 內建任務。

其次是 **Generational GC** ，Android 17 給 ART 的 Concurrent Mark-Compact GC 引入更頻繁、更輕量的 young-generation collections，把短生命週期物件和長期穩定物件分開處理，減少全堆掃描，從而降低 CPU、耗電和 UI 卡頓。

> 這些 ART 改進會透過 Google Play System updates 支援到舊裝置。

然後是 Lock-Free MessageQueue，這個我們也聊過，對於 target SDK 37 以上的 App，核心 `android.os.MessageQueue` 改為 lock-free 架構，目標是減少掉幀、改善啟動時間，並提升多執行緒高負載佇列效能。

軟體

> 但如果 App 透過反射存取 MessageQueue 私有欄位和方法，那就會異常了。

接著 **static final 欄位 truly final** ，從 Android 17 開始，target SDK 37 以上 App 不能再修改 `static final` 欄位，透過反射或 deep reflection 修改會拋 `IllegalAccessException` ，透過 JNI 的 `SetStatic<Type>Field` 修改會直接 crash。

最後是 **自訂通知視圖限制更嚴格** ，為了降低記憶體占用，Android 17 進一步限制 custom notification views 的尺寸，並關閉透過 URI 繞過現有限制的漏洞。

## 隱私安全

隱私的其實之前我們也聊過，Android 17 的隱私方向很明確：

> 減少長期、寬泛的權限，改成系統選擇器、臨時授權、會話級授權。

第一是 **系統級聯絡人選擇器** ，透過 `ACTION_PICK_CONTACTS` ，App 可以只請求使用者選擇的具體欄位，比如 email 或電話號碼，不再需要寬泛的 `READ_CONTACTS` 權限，同時支援 work/personal profile 隔離。

第二是 **Photo Picker 可自訂縮圖比例** ，透過 `PhotoPickerUiCustomizationParams` ，App 可以讓系統照片選擇器以直向縮圖顯示，適合短影片、直向內容社交類應用。

第三是 **系統渲染 Location Button** ，App 可以嵌入一個系統渲染的位置按鈕，使用者點擊後只給目前 session 的精確位置存取，而不是長期位置權限。

第四是 **EyeDropper API** ，透過 `ACTION_OPEN_EYE_DROPPER` ，App 可以呼叫系統級取色器，從螢幕任意像素取色，不再需要申請螢幕錄製、MediaProjection 或寬泛截圖權限。

```
kotlin 代码解读复制代码val eyeDropperLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
   if (result.resultCode == Activity.RESULT_OK) {
       val color = result.data?.getIntExtra(Intent.EXTRA_COLOR, Color.BLACK)
       // Use the picked color in your app
   }
}
fun launchColorPicker() {
   val intent = Intent(Intent.ACTION_OPEN_EYE_DROPPER)
   eyeDropperLauncher.launch(intent)
}
```

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/d5bea4dcebea4236a79c6d31aa07d82e~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1782268040&x-signature=5njapaUCprRFhJ%2ByKoiKUWtovVo%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/d5bea4dcebea4236a79c6d31aa07d82e~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1782268040&x-signature=5njapaUCprRFhJ%2ByKoiKUWtovVo%3D)

## 本機網路存取

這個對於物聯網影響比較大，Android 17 對本機網路存取加了新限制，target Android 17 的 App，如果要存取區域網路裝置，比如智慧家居裝置、投放接收器等，要麼申請新的 `ACCESS_LOCAL_NETWORK` runtime permission，要麼使用系統中介的隱私保護裝置選擇器。

Android作業系統

> `ACCESS_LOCAL_NETWORK` 屬於現有的 `NEARBY_DEVICES` 權限組，如果使用者已經授予了其他 `NEARBY_DEVICES` 權限，系統不會再次彈窗。

這對物聯網場景的 App 影響比較大，例如：

- 智慧家居
- 投放/區域網路投放
- 印表機/掃描器
- NAS/SMB/DLNA
- IoT 配網
- 區域網路除錯工具
- 區域網路發現服務

以前 App 都預設覺得可以掃區域網，現在 target 37 後要重新梳理權限和系統 picker 了。

## SMS OTP 保護

這個也是比較討厭的，我們之前也聊過，Android 17 擴大了 SMS OTP 保護，會延遲 App 對簡訊驗證碼的存取，延遲時間為 3 小時。

> WebOTP 格式下，如果 App 不是目標接收方，也就是 domain mismatch，會被延遲

標準 SMS OTP 對 target SDK 37+ 的 App 延遲，預設簡訊 App、assistant、connected companion apps 豁免，Google 建議遷移到 SMS Retriever 或 SMS User Consent APIs。

這個變化主要是為了防止惡意 App 讀取驗證碼，對開發者來說， **重點是不要再依賴直接讀簡訊做驗證碼自動填入，要趕緊遷移到官方 API** 。

## Post-Quantum Cryptography

Android 17 加入 Post-Quantum Cryptography (PQC) 相關能力，支援的裝置可以在安全硬體中生成 ML-DSA 金鑰，並透過標準 JCA API 生成抗量子簽章。

Android作業系統

APK 簽章側，Android 17 引入 **v3.2 APK Signature Scheme** ，結合傳統簽章和 ML-DSA 簽章，用於提升 App 發行安全。

> **說人話就是又多了一種簽章方式。**

## Native 動態程式碼

這個以前也聊過了，Android 14 已經對 DEX/JAR 的 safer dynamic code loading 做了保護，Android 17 把這個保護擴展到 native libraries。

對於 target SDK 37 或更高的 App，所有透過 `System.load()` 載入的 native 檔案都必須標記為 read-only，否則系統會拋 `UnsatisfiedLinkError` 。

這會主要影響 *熱更新、外掛化、下載 so、解壓 so、動態載入 native 模組* 的方案，特別是一些自研外掛框架、遊戲引擎、跨平台 runtime、加固殼、動態演算法包，可能要重點檢查檔案權限。

## 物理鍵盤密碼輸入更安全

Android 17 在使用物理鍵盤輸入密碼、PIN 等敏感內容時，預設不再顯示最後輸入的字元。

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/c42978949adb47aaae5998824ab4a846~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1782268040&x-signature=lxPWcQxnGyHzt3oalu6N535bkn8%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/c42978949adb47aaae5998824ab4a846~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1782268040&x-signature=lxPWcQxnGyHzt3oalu6N535bkn8%3D)

使用者還是可以根據裝置廠商支援情況自行調整顯示設定，而 Android 內建 SDK 元件會自動支援這些增強保護，Compose 1.12 的 `SecureTextFields` 也會支援。

Android作業系統

## 媒體和相機

這個主要就是一些零散更新了：

- **Eclipsa Video** ：基於 SMPTE ST 2094-50 的 HDR 影片標準，使用新中繼資料幫助裝置根據顯示器亮度能力和環境光適配內容，也改善 SDR/HDR 同屏顯示
- **RAW14 image format** ：支援 RAW14，讓專業相機 App 可以從相容感光元件擷取更高細節和色深。
- **Vendor-defined camera extensions** ：允許硬體廠商定義和實作自訂相機擴充模式，讓 App 能存取更具體的新相機特性。
- **Extended HE-AAC software encoder** ：系統提供 Extended HE-AAC 軟體編碼器，支援低/高位元率和 loudness metadata，改善低頻寬語音訊息品質。
- **VVC/H.266** ：OEM 可以透過 `video/vvc` MIME type、VVC profiles、MediaExtractor 等整合 VVC 支援。
- **Camera device type：新 API 可以查詢攝影機是內建硬體、外接 USB 攝影機，還是虛擬攝影機** 。
- **Constant Quality for Video Recording** ： `MediaRecorder` 的 `setVideoEncodingQuality` 可以配置影片編碼 CQ 模式，讓整段影片視覺品質更穩定。

不過 CameraX 和 Media3 也針對 Android 17 進行了更新適配，Google 還發布了一個 agent skill，可輔助把舊 Camera1 或裸 Camera2 實作遷移到 CameraX。

> 這裡有一個非常具體的坑： **需要把 CameraX 更新到 1.5.2 或 1.6.0+，否則可能因為 Android 17 新增 dynamic range mode 導致崩潰。**

## 助聽器支援

Android 17 增加了 BLE Audio hearing aid 的裝置類型常量 `AudioDeviceInfo.TYPE_BLE_HEARING_AID` ，App 可以區分助聽器和普通耳機。

Android作業系統

> 系統支援使用者更細粒度管理系統聲音路由，比如通知、鈴聲、鬧鐘是走助聽器還是走裝置揚聲器

## 盡快適配

官方這次還特別強調現在就要準備更新，這次 Android 17 變化還挺大的，不適配容易有問題，例如：

- 大螢幕 resizability：target 37 後不能再在大螢幕上 opt out 方向、resize、比例限制
- 動態程式碼載入：target 37 後，透過 `System.load()` 載入的 native 檔案必須 read-only，否則 `UnsatisfiedLinkError`
- 憑證透明度 CT：Android 17 預設啟用 Certificate Transparency，Android 16 中 CT 可用但需要 App opt in
- 本機網路保護：target 37 後，本機網路存取預設阻止，需要優先使用隱私保護 picker，確實需要長期廣泛存取再申請 `ACCESS_LOCAL_NETWORK`
- 背景音訊加固：Android 17 對背景音訊互動加強限制，包括音訊播放、audio focus 請求、音量修改 API
- NPU 存取聲明：target Android 17 後，如果 App 需要直接存取 NPU，必須在 manifest 聲明 `FEATURE_NEURAL_PROCESSING_UNIT` ，否則可能被阻止存取 NPU
- 有沒有依賴修改 `static final` 欄位、是否反射存取 MessageQueue 私有實作
- 有沒有直接讀 SMS OTP、是否使用過大的 custom notification view
- 記憶體有沒有管理好，不然會被靜默殺死

> 更多可見： [Android 17 新適配要求，各大權限進一步收緊，適配難度提升](https://juejin.cn/post/7621551215504228395)

## 連結

[android-developers.googleblog.com/2026/06/And…](https://link.juejin.cn/?target=https%3A%2F%2Fandroid-developers.googleblog.com%2F2026%2F06%2FAndroid-17.html)

---

原文出處： [https://juejin.cn/post/7651930056574517289](https://juejin.cn/post/7651930056574517289)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/37lYva) [61瀏覽](https://codelove.tw/@tony/post/37lYva)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/37lYva)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝16 💬1 ❤️1

507

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

30

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782108480&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37lYva&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782108479674&amp;bpp=1&amp;bdt=271&amp;idt=654&amp;shv=r20260617&amp;mjsv=m202606170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782108370%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782108370%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782108370%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2290514926850&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099218%2C42533294%2C95386338&amp;oid=2&amp;pvsid=1321403697019271&amp;tmod=1256732551&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=671" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260617/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>

行動電話