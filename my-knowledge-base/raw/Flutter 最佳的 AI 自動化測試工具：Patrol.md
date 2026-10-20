---
title: "Flutter 最佳的 AI 自動化測試工具：Patrol"
source: "https://codelove.tw/@tony/post/3Qjv7a"
author:
published:
created: 2026-06-08
description: "Flutter 最好的 AI 自動化測試工具：Patrol================================    Patrol 是一個**專為 Flutter 實現的跨平台 E2E 測試開源框架**，主要由 LeanCode 開發與維護。它最大的作用就是補足 Flutter 官..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月05日](https://codelove.tw/@tony/post/3Qjv7a "2026-06-05 19:15:10")

## Flutter 最好的 AI 自動化測試工具：Patrol

```
Patrol 是一個**專為 Flutter 實現的跨平台 E2E 測試開源框架**，主要由 LeanCode 開發與維護。它最大的作用就是補足 Flutter 官方 integration_test 的短板，特別是在原生互動方面，以及對 AI 友善。
```

[![](https://i.imgur.com/go77Wat.jpeg)](https://i.imgur.com/go77Wat.jpeg)

開發者可以用 Flutter 風格來寫 Patrol 的測試用例， **測試過程除了支援操作 Flutter Widget 之外，最重要的是可以深入控制 iOS/Android 的原生元件** 。

> 簡單來說，Patrol 透過在 Flutter 測試程序和原生自動化服務之間建立雙向 gRPC 通訊，來實現 Flutter 的全面自動化測試。

[![](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/8ab1ad2aba94400eb0264895a7638f46~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1781053324&x-signature=fJCqa884whzJ67rzgZZ4qXUd9bk%3D)](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/8ab1ad2aba94400eb0264895a7638f46~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1781053324&x-signature=fJCqa884whzJ67rzgZZ4qXUd9bk%3D)

除了 Flutter 控件之外，它在原生自動化上還可以實現：

- 處理權限彈窗（位置、相機等）
- 操作通知（打開通知欄、點擊特定通知）
- 與 WebView 互動
- 控制裝置設定：開關 Wi‑Fi、行動數據、深色模式
- 模擬網路條件、背景/前景切換、App 生命週期等
- 瀏覽器相關操作（Web 支援）

[![](https://i.imgur.com/1aaZBj5.jpeg)](https://i.imgur.com/1aaZBj5.jpeg)

[![](https://i.imgur.com/CxAp2aP.jpeg)](https://i.imgur.com/CxAp2aP.jpeg)

**這對於真實 Flutter E2E 測試來說是非常不錯的補強，另外它對 Flutter 也很友善** ，例如：

- 自訂 Finder 系統，語法很簡單（如 $(#emailTextField).enterText('xxx')），比官方 tester.enterText(find.byKey(...)) 可讀性高得多
- 支援 Hot Restart，寫測試時迭代更快
- Patrol DevTools 擴充功能，可直接查看 Android/iOS 原生視圖屬性
- 全測試隔離 + 分片（Sharding），適合大規模 CI/CD
- 多平台支援

相較於另一個知名測試框架 Maestro，Patrol 更貼近 Flutter，而且 LeanCode 本身也是專業的 Flutter 顧問公司，專案生態已在許多真實產品中驗證過， **最重要的是開源且免費** 。配合 AI 可以對專案進行全流程腳本自動化測試，這就是我認為 Patrol 是目前最好的 Flutter 測試框架的原因。

接著看到原始碼，目前 Patrol 專案是透過 Melos 管理的 monorepo，核心主要包括：

| 包 | 作用 |
| --- | --- |
| `patrol` | Flutter 端核心函式庫（Dart） |
| `patrol_cli` | 命令列工具，負責建構、打包、編排測試執行 |
| `patrol_finders` | 可單獨使用的自訂查找器（finders） |
| `patrol_gen` | 程式碼產生器，從 `schema.dart` 生成跨平台協定程式碼 |
| `patrol_log` | 結構化日誌 |
| `adb` | ADB 封裝（用於 Android 裝置操作） |
| `patrol_devtools_extension` | DevTools 擴充功能 |
| `patrol_mcp` | MCP 服務支援 |

[![](https://i.imgur.com/wI0IVZ9.jpeg)](https://i.imgur.com/wI0IVZ9.jpeg)

**這裡面最重要的就是雙向 gRPC 通訊架構** ，這是 Patrol 的核心，整個系統存在兩個獨立的 gRPC 服務，分別在不同端運行以實現互動：

- PatrolAppService（Dart 端作為 Server，原生端作為 Client）
	- Dart/Flutter 測試程序作為伺服器，提供兩個介面：列出所有測試、執行某個特定測試
		- 原生端（Android JUnit Runner / iOS XCTest）作為用戶端，主動向 Dart 端請求要執行哪個測試
- MobileAutomator / AndroidAutomator / IosAutomator（原生端作為 Server，Dart 端作為 Client）
	- 原生端啟動一個 HTTP/gRPC 伺服器，監聽來自 Dart 測試程式碼的指令
		- Dart 測試程式碼在需要做系統級操作時，向本機這個服務發送 RPC 請求

整個簡要流程如下所示：

[![](https://i.imgur.com/n66PhXH.jpeg)](https://i.imgur.com/n66PhXH.jpeg)

[![](https://i.imgur.com/0Os8ygS.jpeg)](https://i.imgur.com/0Os8ygS.jpeg)

而所有跨程序通訊的資料結構和服務介面都定義在根目錄的 `schema.dart` ，然後 `patrol_gen` 會解析這份 schema，分別生成程式碼：

- Dart 端：gRPC client stub、server stub、訊息類別
- Kotlin/Android 端：對應的 Kotlin 資料類別和服務實作
- Swift/iOS 端：對應的 Swift 資料類別和服務實作

[![](https://i.imgur.com/rAPxb9w.jpeg)](https://i.imgur.com/rAPxb9w.jpeg)

接著就是 `PatrolBinding` ，作為整個框架執行時的核心，它主要負責 gRPC 綁定支援：

```
class PatrolBinding extends LiveTestWidgetsFlutterBinding {
  // 1. 啟動時註冊 setUp/tearDown 鉤子，監聽原生端的測試執行請求
  // 2. 透過 patrolAppService.testExecutionRequested 掛起等待
  // 3. 測試完成後主動通知原生端結果（pass/fail + details）
  // 4. 收集 FlutterError，序列化後傳回原生，讓 JUnit/XCTest 報告更準確
  // 5. 註冊 DevTools Service Extension 支援 UI 樹檢查
}
```

> 這裡最重要的是， **測試不是按順序全部執行，而是每個測試體都會先 `await patrolAppService.waitForExecutionRequest(testName)` 掛起，然後等到原生 Runner 透過 gRPC 發出 `runDartTest` 指令時才真正執行** 。也就是說，原生端可以精確控制哪個測試在什麼時機執行，並在 JUnit/XCTest 報告中單獨呈現每個測試的結果。

然後來到原生端， **Android 原生層主要有** ：

- `Automator.kt` ：封裝 UIAutomator2 API，實作所有系統級操作（點擊通知、處理權限彈窗、切換 Wi‑Fi 等）
- `AutomatorServer.kt` ：基於 Ktor 啟動 HTTP 服務，將 gRPC 呼叫路由到 `Automator`
- `PatrolJUnitRunner.java` ：
	- 先透過 `PatrolAppServiceClient` 呼叫 Dart 端 `listDartTests()` 取得測試列表
		- 為每個測試動態註冊一個 JUnit test case
		- 逐一呼叫 `runDartTest()` 觸發 Dart 執行
		- 收集結果，輸出標準 JUnit XML

> 也就是說，最終 Android CI 系統（Firebase Test Lab、BrowserStack、LambdaTest 等）可以直接識別每個測試用例的結果。

**而對於 iOS 原生層，則是使用 XCTest + XCUIApplication 框架** ，原理類似，XCTest runner 和 Dart 程序之間透過 gRPC 完成握手和測試編排；對系統的操作（權限彈窗、通知、深色模式等）則透過 `XCUIApplication` 和 `XCUIDevice` API 完成，基本上都是官方原生支援的測試自動化介面。

**最後不得不提 `patrol_cli`** ，它是整個執行機制的編排器，主要包括：

- 解析 `patrol.yaml` / `pubspec.yaml` 設定
- 使用 `test_bundler.dart` 將多個測試檔案打包成單一入口（ `_bundled_test.dart` ），搭配 Dart `group/test` 機制建立完整測試樹
- 透過 ADB（Android）或 `xcodebuild` / `xcrun simctl` （iOS）安裝、啟動測試
- **支援 Hot Restart 開發模式**
- 彙整各裝置的測試結果

[![](https://i.imgur.com/UBqvrSG.jpeg)](https://i.imgur.com/UBqvrSG.jpeg)

也就是說，原生 Runner 會動態將每個 Dart 測試註冊為獨立的 JUnit/XCTest 用例，而 CI 系統（Firebase Test Lab、Bitrise、BrowserStack 等）可以按單一測試維度展示結果，因此在結果呈現上也能有更好的測試粒度。整個能力核心主要包括：

| 類型 | 能力 |
| --- | --- |
| Flutter UI | 點擊、輸入文字、滑動、等待 widget 出現、查找 widget |
| 系統權限 | 檢測彈窗、授權（使用期間/僅此一次/拒絕）、精確/模糊位置 |
| 通知 | 打開通知欄、讀取通知列表、點擊通知 |
| 網路 | Wi‑Fi 開關、行動網路開關、飛航模式、藍牙開關 |
| 裝置狀態 | 回到主畫面、最近使用的 App、音量鍵、深色模式 |
| 跨 App | 打開指定 App（套件名稱/Bundle ID）、跳轉 URL |
| 相機/相簿 | 拍照、從相簿選擇單張/多張圖片 |
| 位置 | 設定 Mock GPS 座標 |
| 系統資訊 | 取得 OS 版本、判斷是否為虛擬裝置 |

更具體的 Flutter Method 有：

| Method | Purpose | Example |
| --- | --- | --- |
| `$(pattern)` | 查找並串接 widgets | `$(Scaffold).$(TextField).at(1)` |
| `.tap()` | 點擊 widget | `$(FloatingActionButton).tap()` |
| `.enterText(text)` | 在欄位中輸入文字 | `$(#emailField).enterText('[email protected]')` |
| `.scrollTo()` | 滾動到 widget | `$(#submitButton).scrollTo()` |
| `.waitUntilVisible()` | 等待可見 | `$(Dialog).waitUntilVisible()` |
| `.waitUntilExists()` | 等待存在 | `$(LoadingSpinner).waitUntilExists()` |
| `.containing(pattern)` | 依後代篩選 | `$(ListTile).containing($('Premium'))` |
| `.which(condition)` | 自訂篩選 | `$(TextField).which((w) => w.enabled)` |
| `.visible` | 檢查是否可見 | `if ($(Dialog).visible) { ... }` |
| `.exists` | 檢查是否存在 | `if ($(#errorMessage).exists) { ... }` |

[![](https://i.imgur.com/CA8k2h2.jpeg)](https://i.imgur.com/CA8k2h2.jpeg)

另外對應的 Finder 行為可以透過 `PatrolTesterConfig` 進行配置，你可以根據情境自訂腳本的事件設定，例如超過多久沒出現就代表是 UX 的 Bug：

| Property | Default | Purpose |
| --- | --- | --- |
| `existsTimeout` | 10s | `waitUntilExists()` 超時 |
| `visibleTimeout` | 10s | `waitUntilVisible()` 超時 |
| `settleTimeout` | 10s | `pumpAndSettle()` 超時 |
| `settlePolicy` | `trySettle` | 動作在什麼時候進行結算 |
| `dragDuration` | 100ms | 拖曳手勢持續時間 |
| `settleBetweenScrollsTimeout` | 5s | 滾動過程中的超時設定 |

同樣地，在原生層也可以直接透過 Dart 進行操作管理，整個實作都有相同的抽象適配：

[![](https://i.imgur.com/NSYinH0.jpeg)](https://i.imgur.com/NSYinH0.jpeg)

[![](https://i.imgur.com/rULCavB.jpeg)](https://i.imgur.com/rULCavB.jpeg)

基本上原生的大多數功能都支援，除了 iOS 的無障礙標籤還不支援之外，基本能力都已經對齊：

| Property | Android | iOS | Description |
| --- | --- | --- | --- |
| `text` / `label` | ✓ | ✓ | 可見文字內容 |
| `resourceId` / `identifier` | ✓ | ✓ | 唯一元素 ID |
| `className` / `elementType` | ✓ | ✓ | View/element type |
| `contentDescription` | ✓ | ✗ | 無障礙標籤 |
| `isClickable`, `isEnabled` | ✓ | ✓ | 互動狀態 |
| `instance` | ✓ | ✓ | 多個匹配時的索引 |

[![](https://i.imgur.com/tgNaugv.jpeg)](https://i.imgur.com/tgNaugv.jpeg)

之後， **目前 Patrol 除了 Android、iOS、Web 之外，也開始支援 macOS，不過還沒支援 Windows，確實是一大遺憾** ：

[![](https://i.imgur.com/7VXn9xC.jpeg)](https://i.imgur.com/7VXn9xC.jpeg)

| Feature | Android | iOS | macOS |
| --- | --- | --- | --- |
| **Test Runner** | Gradle + JUnit | xcodebuild + XCTest | xcodebuild + XCTest |
| **UI Automation** | UiAutomator 2 | XCTest | XCTest |
| **Min API/Version** | API 21+ | iOS 13.0+ | macOS 12.0+ |
| **Back Button** | ✓ | ✗ | ✗ |
| **Control Center** | Quick Settings | ✓ | ✓ |
| **Permission Dialogs** | ✓ | ✓ | ✓ |
| **Notifications** | ✓ | ✓ | Limited |
| **Camera/Gallery** | ✓ | ✓ | Partial |
| **Build Flavors** | ✓ | ✓ Schemes | ✓ Schemes |

> 這裡 iOS 和 macOS 不支援 Back Button，是因為 iOS 和 macOS 從設計上就沒有全域 Back Button，XCTest 框架本身也沒有提供模擬「系統返回」的方法。在 Patrol 測試中，針對 iOS/macOS 需要 **明確操作 UI 元素** 來實現返回，也就是 Android 可以直接用 `await $.native.pressBack();`，但是 iOS 需要 `await $('Back').tap();`，或者 `await $.tap(find.byTooltip('Back'));`。

當然，也有一些限制，例如：

- Web 支援仍不完整，部分原生能力在 Web 上不適用
- 真實 Android 裝置上 Mock Location 不可用（僅模擬器）
- Bluetooth 開關在 Android 12 以下不可用
- iOS 控制中心操作在模擬器上不可用

不過這些限制都不算大問題，所以如果你在做 Flutter，尤其是需要測試複雜使用者流程（權限、支付、推播、OAuth 等），我個人非常推薦直接用 Patrol，特別是透過 AI 完成自動化測試驗收時，真的比直接對接 Flutter MCP 效果更好。

因為 Patrol 同樣支援 MCP 服務，可以讓 AI 直接在 Flutter 專案中執行與管理 Patrol 測試，AI 能夠直接 **執行測試、看裝置截圖、讀取原生 UI 樹、查看日誌狀態、重跑測試並修復測試，然後 Hot Restart 後直接執行** 。

[![](https://i.imgur.com/9WRdbiq.jpeg)](https://i.imgur.com/9WRdbiq.jpeg)

所以，如果你還沒試過，真的可以試試 Patrol，AI 時代，你值得擁有。

---

原文出處： [https://juejin.cn/post/7646696467931807807](https://juejin.cn/post/7646696467931807807)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3Qjv7a) [309瀏覽](https://codelove.tw/@tony/post/3Qjv7a)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3Qjv7a)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780888551&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3Qjv7a&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780888550505&amp;bpp=1&amp;bdt=259&amp;idt=859&amp;shv=r20260604&amp;mjsv=m202606020101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780888487%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780888487%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780888487%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1340754998087&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=42532523%2C95390668%2C95390788%2C42533293%2C95390278&amp;oid=2&amp;pvsid=7695822216434823&amp;tmod=473573085&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=887" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>