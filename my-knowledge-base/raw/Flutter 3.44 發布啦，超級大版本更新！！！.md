---
title: "Flutter 3.44 發布啦，超級大版本更新！！！"
source: "https://codelove.tw/@tony/post/aYQeVa"
author:
published:
created: 2026-05-25
description: "Flutter 3.44 發布啦，超級大版本更新！！！  ==============================    雖然 Flutter 3.44 的版本已經發布兩天了，但是今天才是真正的官宣 Flutter 3.44 發布哦，3.44 雖然看起來版本號不大，但是這確實是一個超級大版..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月22日](https://codelove.tw/@tony/post/aYQeVa "2026-05-22 18:04:12")

```
雖然 Flutter 3.44 的版本已經發布兩天了，但是今天才是真正的官宣 Flutter 3.44 發布哦，3.44 雖然看起來版本號不大，但是這確實是一個超級大版本更新。
```

> 也不知道為什麼 Flutter 對於 3.x 這個版本號這麼熱愛，就和 RN 熱愛 0.x 一樣。

這次 3.44 核心包括 **Android HC++ 功能落地、iOS/MacOS 預設使用 Swift PM、多視窗功能落地、Material 和 Cupertino 解耦、各種 AI 場景支援、Widget Previewer 更新和各種功能優化** 。

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0e044262372c40d38b908121e5d12558~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=3PCSXwaHtfTw6u0Gg9wZrjzcWso%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0e044262372c40d38b908121e5d12558~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=3PCSXwaHtfTw6u0Gg9wZrjzcWso%3D)

首先還是按照慣例，Flutter 先公布了近期的一些數據情況，其中 **[pub.dev](http://pub.dev/) 在過去 30 天內， Package 下載量超過 13 億次 ，Flutter 兩大應用商店下月活開發者超過 150 萬 ，一年內成長了 50%** 。

> 這算是慣例對於「Flutter 涼了沒」的回覆。

## 工具更新

### Widget Previewer

雖然我基本沒怎麼用過，但是 Flutter 官方一直很熱衷 Widget Previewer 的優化，這次主要有兩個內容：

- 預覽偵測的邏輯重寫了，使用 analysis server 做 widget preview detection， **降低了 Flutter 工具的記憶體占用，對於 IDE 使用者而言，整體記憶體占用最多可降低 50%**
- **自訂 preview annotation 支援 collections 和 records** ，現在可以按群組、名稱、腳本和套件 URI 篩選預覽

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/035d197d338649c892fde36df84dc64e~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=yNMvfbIlDu01yNjKxb%2BsOCAr1EY%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/035d197d338649c892fde36df84dc64e~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=yNMvfbIlDu01yNjKxb%2BsOCAr1EY%3D)

3.44 的 Previewer 其實就是接入了更底層的 analysis server 偵測鏈路，主要涉及：

- IDE 能準確知道哪些函式、建構子、靜態方法可以 preview
- Previewer 要能處理真實專案裡的 wrapper、theme、localization、狀態注入
- 依賴真實 analyzer 級別的語意資訊

> 可以看得出來官方對於 Widget Previewer 很重視，大概在 AI 時代，Widget Previewer 可以在開發過程中，有不錯的即時預覽效果。

### 不需要 Rosetta

這個我們提前聊過，在之前的 [《Flutter iOS 又修復了一個建置問題》](https://juejin.cn/post/7637433660883714090) 就提到過，以前 Flutter 本身的建置工具還是需要 Rosetta 進行適配支援，這次所有的 macOS 命令列工具都已經透過 **Fat Binary，從而同時支援 `x86_64` 和 `arm64` 兩種架構** 。

因為下個版本 Rosetta 就要開始退出歷史舞台了，所以這也算是個重要的跟進升級，當然， *這個能力的 PR 的過程很抽象，沒看過的可以了解下，是真的草台* 。。。。

[![](https://i.imgur.com/mIQSvbs.jpeg)](https://i.imgur.com/mIQSvbs.jpeg)

## AI

Flutter 官方真的很熱衷跟進各種 AI 能力，而且跟進得賊快。

### Agentic Hot Reload

3.44 官方推出了全新的 Agentic Hot Reload 功能，Flutter 在 AI 支援能力上一向很熱衷，這次的 Agentic Hot Reload 也是。

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/47feca1959a0490e9dfdc96f3f6b804b~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=G9VgjZx%2BV1jV%2BjXB%2BoAKdca9F7A%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/47feca1959a0490e9dfdc96f3f6b804b~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=G9VgjZx%2BV1jV%2BjXB%2BoAKdca9F7A%3D)

從 3.44 開始， **一些 MCP 伺服器和 Coding Agent 支援自動尋找並連接到正在執行的 Flutter App** ，也就是類似 Antigravity 這樣的 Coding Agent 可以支援使用 Hot Reload， 同時還有配置前面的 Previewer ，整體 AI 開發體驗會更好。

另外，Agent 現在可以透過服務，直接讀取本地 pub 套件依賴，不需要去取搜尋本地 pub 快取，同時 MCP 相關的 Tools 也得到了更新。

> 實際上這個能力以前也能做到，AI 透過命令列的 `r` 輸入也可以，但是這次的是讓 agent 有官方路徑去發現執行中的 Flutter session，agent 可以知道是哪個 App 正在執行，因為是一整套的 MCP 協議支援。

### Skills

關於 skills 在不久前我們也剛聊過， [《新 Flutter & Dart Agent Skills 深度解讀》](https://juejin.cn/post/7637046499474538559) ，現在官方的 skills 都是 **「任務導向型」的 Skills** ，每個 Skill 都不是在描述一個功能或者說明一個文件，而是在提供一套決策邏輯，讓 AI 可以透過明確指令來解決問題，並且提供對應的回饋循環（Feedback Loop）。

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/fc0f53412fbf421092a894c7badcdc51~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=AfnnIylDsvZGQq420VVJGAnM7lk%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/fc0f53412fbf421092a894c7badcdc51~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=AfnnIylDsvZGQq420VVJGAnM7lk%3D)

**每個技 Skill 的結果都需要做檢驗** ，類似 `flutter-fix-layout-issues` 、 `flutter-add-widget-test` 、 `flutter-add-integration-test` 、 `flutter-build-responsive-layout` 、 `flutter-setup-localization` 、 `flutter-implement-json-serialization` 、 `flutter-add-widget-preview` 、 `dart-fix-runtime-errors` 、 `dart-use-pattern-matching` 都是很實用的技能。

### Genkit Dart

關於 GenKit 我們之前也一樣在 [《Google Genkit Dart 正式發布》](https://juejin.cn/post/7615551904538755114) 聊過，基於 Genkit 可以建構全端式 AI 驅動的智慧代理應用：

> **Flutter 使用 Dart 可以直接原生實現 AI 編排框架，支援不同模型和本地場景，提供 agent workflow / tool calling / RAG 等能力** 。

透過 Genkit Dart 你可以實現一整套的 Agent 伺服器端和客戶端能力，支援 Google、Anthropic、OpenAI 甚至本地模型的能力，包括型別安全的結構化輸出、工具呼叫、多輪對話和內建的可觀測性能力等等。

```
dart 体验AI代码助手 代码解读复制代码import 'package:genkit/genkit.dart';
import 'package:genkit_google_genai/genkit_google_genai.dart';

void main() async {
 final ai = Genkit(plugins: [googleAI()]);

 final response = await ai.generate(
   model: googleAI.gemini('gemini-flash-latest'),
   prompt: 'Why is Dart a great language for AI
            applications?',
 );

 print(response.text);
}
```

### Gemma 4

Flutter 也支援直接跑 Gemma 4 本地模型，透過 `flutter_gemma` 外掛，App 可以和 Gemma 深度整合，外掛底層支援 LiteRT-LM，支援 GPU 和 NPU 加速，最重要是支援全平台：

[![](https://i.imgur.com/NQXJYia.jpeg)](https://i.imgur.com/NQXJYia.jpeg)

### GenUI

這個大家應該也不陌生吧，我們在 [《Flutter GenUI 0.9 和 A2UI 0.9 發布》](https://juejin.cn/post/7640409934489010230) 才剛剛聊過，官方表示： `Flutter + A2UI = GenUI`

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/139589c087a24ffb88bbd44dca74f14d~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=YePgwghBc3iA%2F8%2BCQjPL%2FWfBUBU%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/139589c087a24ffb88bbd44dca74f14d~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=YePgwghBc3iA%2F8%2BCQjPL%2FWfBUBU%3D)

作為基於 AI 的生成式 UI 支援，Flutter 算是在 A2UI 領域最早的支援專案之一，它提供了一套完整的協議實作， **可以讓 Agent 在 App 執行中動態生成控制項，客戶端不需要開發一個完整的頁面功能，開發者只需要維護組件庫** ，剩下的都交給 AI 。

> 官方統計，自從推出 Flutter GenUI SDK 以來，軟體包下載量自年初以來成長了 500%。

官方特別提到了 *Catagay Ulusoy* 開發的 *Finnish it* （可在 Google Play 商店和 Apple Store 下載）例子，這款應用不僅能為使用者建立客製化的課程計畫來學習芬蘭語，還能根據每節課的需要動態生成完美的介面：

[![](https://i.imgur.com/WTc0Umc.jpeg)](https://i.imgur.com/WTc0Umc.jpeg)

另外還有官方提供的 Demo，也分享了 DeepMind 使用 Flutter 建構 Gemini App「視覺化佈局」的效果：

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/94499288dc354a6f95aebd43124a50e6~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=w4Q5vhjjYysTv9nQPXjh4JqJMdk%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/94499288dc354a6f95aebd43124a50e6~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=w4Q5vhjjYysTv9nQPXjh4JqJMdk%3D)

## Android

首先就是 Googlebook 支援， **我都還沒用上，Flutter 就宣布了 Googlebook 已經適配完成** ，核心是適配了 Gemini 處理器的全新 Googlebook 筆記型電腦，不過具體支援得如何，還需要看真的發布後才知道。

**另外還有 Android 17 的適配** ， Flutter 3.44 也正在完成 Android 17 的適配，包括本地網路保護和安全動態程式碼載入等場景。

### Hybrid Composition++

另外還有一個重點就是 Hybrid Composition++ 終於落地了，這個我們在去年的 [《Flutter 正在推進全新 PlatformView 實現 HCPP》](https://juejin.cn/post/7471979172115152932) 就提到過，這是一套全新的 PlatformView 實現，可以提供更好的 PlatformView 效能：

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/d115b34ab1b04d5886cd30eaebbbb3e8~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=zm7o80qf7baChahfpfkY9bk9V%2F4%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/d115b34ab1b04d5886cd30eaebbbb3e8~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=zm7o80qf7baChahfpfkY9bk9V%2F4%3D)

**HCPP 不再依賴螢幕外緩衝區或強制 Flutter 引擎處理原生視圖** ，而是將圖層合成直接委託給 Android 原生，然後利用 Vulkan 圖形庫的底層存取能力，透過硬體緩衝區交換鏈和 `SurfaceControl` 交易將 Flutter UI 與原生 Android 視圖同步。

簡單來說就是， **HCPP 主要就是透過 `SurfaceControl` 來構造一個高層級的 `Surface` 從而實現最終繪製時混合覆蓋的問題** ，這和 [《深入 Flutter 和 Compose 的 PlatformView 實現對比》](https://juejin.cn/post/7461597205342928936) 裡 Compose 可以在 PlatformView 裡直接使用 `SurfaceView` 的道理類似，都是 `SurfaceFlinger` 合成時的層級操作。

> 只是 **需要 Vulkan 和 API 34 的環境才支援使用** 。

所以 HCPP 對 [Android API 和硬體有一定要求](https://link.juejin.cn/?target=https%3A%2F%2Fdocs.flutter.dev%2Fplatform-integration%2Fandroid%2Fplatform-views%23hcpp) ，可以透過在 `run` 中添加 `--enable-hcpp` 或者在 `AndroidManifest.xml` 檔案中添加相應的設定標記來啟用：

```
xml 体验AI代码助手 代码解读复制代码<meta-data
  android:name="io.flutter.embedding.android.EnableHcpp"
    android:value="true" />
```

### Android 顯示螢幕圓角

Android 裝置圓角支援， `PredictiveBackPageTransitionsBuilder` 新增對 `MediaQuery.displayCornerRadiiOf(context)` 的使用。

簡單說，Android 裝置真實螢幕圓角可以參與頁面返回轉場的裁剪效果，讓轉場更貼合物理裝置邊界。

如果你的應用自己覆蓋了 Android 頁面轉場，可以顯式使用這個 builder；裝置圓角會由 `MediaQuery.displayCornerRadiiOf(context)` 自動參與裁剪，拿不到裝置圓角時走 fallback：

```
dart 体验AI代码助手 代码解读复制代码MaterialApp(
  theme: ThemeData(
    pageTransitionsTheme: const PageTransitionsTheme(
      builders: <TargetPlatform, PageTransitionsBuilder>{
        TargetPlatform.android: PredictiveBackPageTransitionsBuilder(
          fallbackColor: Colors.black,
        ),
      },
    ),
  ),
  home: const HomePage(),
)
```

### AGP 9 和 built-in Kotlin

在 AGP 9 版本之前，Android 應用和外掛需要手動將 Kotlin Gradle 外掛 (KGP) 加到建置檔案，系統才能識別並編譯 Kotlin 程式碼。

從 AGP 9.0 開始，Android 建置系統已原生支援 Kotlin，由於建置系統已經處理了 Kotlin， **手動新增單獨的 KGP 會導致衝突並造成建置失敗** ，所以這個情況需要 Flutter 進行適配，所以：

- 如果你是開發 Flutter App，這裡需要更新 Android 建置檔案，刪除單獨的 Kotlin Gradle 外掛 (KGP)
- 如果是外掛的話，就需要對 Gradle 進行修改相容

> 設定 AGP 9.0 的外掛必須在 `pubspec.yaml` 裡將 Flutter 的最低版本限制設定為 3.44 。

這個問題主要還是 AGP 和 Kotlin 的破壞性更新帶來的，以前 Flutter Android 專案裡常見的是：

```
groovy 体验AI代码助手 代码解读复制代码plugins {
    id "com.android.application"
    id "kotlin-android"
}
```

或者舊寫法：

```
groovy 体验AI代码助手 代码解读复制代码apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
```

而到了 AGP 9.0，Android Gradle Plugin 自己內建了 Kotlin 支援同時預設啟用，也就是說一般 Android app/library 模組裡不再需要顯式 apply `org.jetbrains.kotlin.android` 或 `kotlin-android` 外掛。

> Google 官方說法是：AGP 9.0 introduces built-in Kotlin support and enables it by default，不再需要 apply `org.jetbrains.kotlin.android` / `kotlin-android` 來編譯 Kotlin 原始碼。

所以遷移後的 Flutter Android app 大致會從：

```
groovy 体验AI代码助手 代码解读复制代码plugins {
    id "com.android.application"
    id "kotlin-android"
    id "dev.flutter.flutter-gradle-plugin"
}

android {
    kotlinOptions {
        jvmTarget = JavaVersion.VERSION_17.toString()
    }
}
```

變成：

```
groovy 体验AI代码助手 代码解读复制代码plugins {
    id "com.android.application"
    id "dev.flutter.flutter-gradle-plugin"
}

android {
    // ...
}

kotlin {
    compilerOptions {
        jvmTarget = org.jetbrains.kotlin.gradle.dsl.JvmTarget.JVM_17
    }
}
```

> 也就是 **刪掉 `kotlin-android` ，刪掉 `android { kotlinOptions {} }` ，改用頂層 `kotlin { compilerOptions {} }` 。**

Flutter 3.44 也不是強行讓所有專案馬上完全遷到 AGP 9 + built-in Kotlin，所以也加了兩個 flag 做緩衝：

```
ini 体验AI代码助手 代码解读复制代码android.newDsl=false
android.builtInKotlin=false
```

> 而為了支援還沒遷移的專案，Flutter 預設使用 legacy Kotlin Gradle Plugin 和 old AGP DSL types，Flutter migrator 會自動把這兩個 flag 加到 `gradle.properties` 。

### ABI

Flutter 之前的做法是幫你按建置類型動態塞 ABI 過濾器，現在改成「在 `defaultConfig` 裡統一塞一次」，這個變化也是遇到 AGP 9 的合併規則後。

以前一般是 `flutter build apk --target-platform android-arm64` 或者 `flutter build apk --split-per-abi` ，而現在為了 AGP9 ， Flutter 把 ABI filter 放到了 `defaultConfig` ：

```
groovy 体验AI代码助手 代码解读复制代码android {
    defaultConfig {
        ndk {
            abiFilters ...
        }
    }
}
```

所以，如果在特定建置類型或產品風格中使用自訂 `abiFilters` ，現在需要在建置或執行應用時傳遞 `-Pdisable-abi-filtering=true` 標誌。

## iOS

### Swift PM 預設

這個我們其實也已經在 [《Flutter 3.44 發布前夕，官方宣布 SwiftPM 將完全取代 CocoaPods》](https://juejin.cn/post/7635969457790648383) 聊過，從 Flutter 3.44 開始， **Swift Package Manager 成為 iOS/macOS 的預設套件管理器，同時這個包含了大量 SwiftPM 的修復** ，例如：

> *CocoaPods 與 SwiftPM 衝突提示、SwiftPM 最低平台版本診斷、SwiftPM 下載太慢的慢提示、SwiftPM cache 集中化處理、SwiftPM Add-to-App 工具等* 。

現在 Flutter CLI 會自動處理遷移。建置或執行應用時，CLI 會將 Xcode 專案更新為使用 SwiftPM，如果依賴暫時不支援 SwiftPM，Flutter 會 fallback 到 CocoaPods，同時發出警告。

如果在 add-to-app 場景下，新的 `flutter build swift-package` 也會支援對應命令，如果 SwiftPM 是在無法相容，也可以透過在 `pubspec.yaml` 檔案中設定 `--enable-swift-package-manager: false` 來回退停用。

[![轉存失敗，建議直接上傳圖片檔案](https://codelove.tw/Users/guoshuyu/Library/Containers/com.tencent.xinWeChat/Data/Documents/xwechat_files/wxid_ori7u1g616jx22_9a0f/temp/InputTemp/881580a2-1a33-40bd-b4d3-0e81464ee33a.png)](https://codelove.tw/Users/guoshuyu/Library/Containers/com.tencent.xinWeChat/Data/Documents/xwechat_files/wxid_ori7u1g616jx22_9a0f/temp/InputTemp/881580a2-1a33-40bd-b4d3-0e81464ee33a.png)

### UIScene

由於 Apple 已經強制了 UIScene 要求， **所以這裡是想提醒你，如果還沒遷移適配，那就要趕緊了** 。

### predictive text

這是 iOS 上一個新的實驗性支援， `TextField` 、 `CupertinoTextField` 、 `EditableText` 和 `TextInputConfiguration` 增加了 `enableInlinePrediction` 相關設定。

也就是 iOS 文字輸入接入 inline prediction 提供了 opt-in 設定，並把設定從高層 `TextField` 傳到底層 text input channel，Material 風格輸入框可以這樣顯式打開：

```
dart 体验AI代码助手 代码解读复制代码TextField(
  controller: titleController,
  enableInlinePrediction: true,
  textInputAction: TextInputAction.done,
  decoration: const InputDecoration(
    labelText: '標題',
  ),
)
```

Cupertino 風格輸入框也有同名參數：

```
dart 体验AI代码助手 代码解读复制代码CupertinoTextField(
  controller: titleController,
  enableInlinePrediction: true,
  placeholder: '標題',
  textInputAction: TextInputAction.done,
)
```

> 預設功能是關閉的，它目前只對 iOS 17 及以上的 inline predictive text 有用。

## Web

這個版本主要都是一些簡單優化，例如：

- 支援 `prefers-reduced-motion` ，也就是現在瀏覽器/系統說要「少動畫」，Flutter Web 能夠適配，然後 Web 還把這個偏好同步到 Flutter engine 的 accessibility 設定裡，讓 Flutter 端的 `disableAnimations` / `reduceMotion` 生效。
- 另外現在 Flutter Web 表單錯誤能更快被螢幕閱讀器讀到，也算是無障礙的一個優化。
- 支援 iOS 26 Safari autofill，Flutter Web 在 iOS 26 Safari 上不會再頻繁銷毀/重建用於 autofill 的隱藏 DOM form，而是盡量重用同一個 form，這樣 Safari/密碼管理器才能識別「這是一組連續的登入/註冊表單欄位」，從而正常自動填入。

## Desktop

Flutter 這次不裝了，直接宣布 **Canonical 將成為 Flutter Desktop 的主要維護者和策略合作夥伴** ，這是什麼意思？就是 Desktop 完全交給 Canonical 維護和主導了。

[![](https://i.imgur.com/hRqQXwR.jpeg)](https://i.imgur.com/hRqQXwR.jpeg)

結果這次還是失望了， **雖然新增加了功能，但是多視窗功能還是只僅在 master 可以用** ，依然沒有合併到 stable 。

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/52ef38ab72ba49209e67843b229b56a9~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=Du0nNkyhMAz3eoNcN3Q4jpHIQik%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/52ef38ab72ba49209e67843b229b56a9~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=Du0nNkyhMAz3eoNcN3Q4jpHIQik%3D)

這次多視窗新增加了 Tooltips、Popups 和 Dialogs 的落地，同時 Linux 現在也支援了 content-sized 視圖，可以根據視窗內容動態調整視窗大小。

另外這次更新裡 showRawDialog / showDialog 可以走原生 dialog window，如果透過 `flutter config --enable-windowing` 啟用了 windowing，那麼 dialog 會透過 windowing system 顯示在自己的視窗中，而不是目前視窗內的 modal overlay，平台不支援時，會 fallback 到普通 dialog route。

其他還有 `_window.dart` 有了多類 controller 和 widget：

- `RegularWindowController` / `RegularWindow`
- `DialogWindowController` / `DialogWindow`
- `TooltipWindowController` / `TooltipWindow`
- `PopupWindowController` / `PopupWindow`
- `SatelliteWindowController` / `SatelliteWindow`

## 嵌入式

這裡官方主要介紹了一些案例，例如豐田的車機，RAV4 在 2025 年是全球最暢銷的車型，而 **2026 款 RAV4 正在使用 Flutter 建構車機系統** ：

[![](https://i.imgur.com/27G4LV1.jpeg)](https://i.imgur.com/27G4LV1.jpeg)

LG 的基於 Flutter 的 WebOS SDK 也要更新發布了，webOS SDK 將包含對 Firebase、影片播放器、遊戲手把等外掛的支援。

[![](https://i.imgur.com/xYIRXwf.jpeg)](https://i.imgur.com/xYIRXwf.jpeg)

## Impeller

3.44 版本包含多個 Vulkan 優化，包括「更好的快取記憶體管理」，還有在掉幀情況下更高效的 GPU/CPU 同步， **使用 SDF 可以實現更清晰的圓圈** ：

[![](https://i.imgur.com/kjr9C66.jpeg)](https://i.imgur.com/kjr9C66.jpeg)

> 這個圈我看了是真頭暈。

同時也改進了 Impeller 處理透視矩陣的方式，修正了陰影和透視投影變換的渲染行為，支援 *Get Uniform by Name API* ，可以在著色器中按名稱綁定 uniform 變數：

```
c 体验AI代码助手 代码解读复制代码    void setUp(ui.FragmentShader shader) {
      shader.getUniformFloat('foobar').set(1.234);
    }
```

## Framework

### 解耦

3.44 開始， **Material 和 Cupertino 就是獨立的 `material_ui` 和 `cupertino_ui` 了，而下個版本發布的時候，框架內目前相容的樣式框就會被完全棄用相容** ，整整 60 多萬行程式碼的 PR：

[![](https://i.imgur.com/yBAsQjh.jpeg)](https://i.imgur.com/yBAsQjh.jpeg)

### 彈框

這次新增的 `CupertinoMenuAnchor` / `CupertinoMenuItem` 就是基於 widgets 層的 `RawMenuAnchor` 實作， Flutter 正在把「選單錨點、開啟/關閉、overlay、焦點和手勢」這類共性邏輯放在更低層，讓 Material 和 Cupertino 分別負責自己的視覺和互動風格。

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/d8503c42d9a047389b21956542e140b7~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=nCWDLYp1g6XXJtOHr17v4TZKCgg%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/d8503c42d9a047389b21956542e140b7~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=nCWDLYp1g6XXJtOHr17v4TZKCgg%3D)

3.44 新增 `CupertinoMenuAnchor` 和 `CupertinoMenuItem` ，它們提供 Cupertino 風格的選單 popup，支援 controller、open/close 回呼、動畫狀態、overlay padding、長按開啟、swipe 選擇、focus/hover、destructive action 等能力：

```
dart 体验AI代码助手 代码解读复制代码CupertinoMenuAnchor(
  menuChildren: <Widget>[
    CupertinoMenuItem(
      leading: const Icon(CupertinoIcons.doc_on_doc),
      subtitle: const Text('建立一個副本'),
      trailing: const Icon(CupertinoIcons.right_chevron),
      onPressed: duplicateItem,
      child: const Text('複製'),
    ),
    const CupertinoMenuDivider(),
    CupertinoMenuItem(
      leading: const Icon(CupertinoIcons.delete),
      isDestructiveAction: true,
      onPressed: deleteItem,
      child: const Text('刪除'),
    ),
  ],
  builder: (BuildContext context, MenuController controller, Widget? child) {
    return CupertinoButton(
      padding: EdgeInsets.zero,
      onPressed: () {
        controller.isOpen ? controller.close() : controller.open();
      },
      child: const Icon(CupertinoIcons.ellipsis_circle),
    );
  },
)
```

如果你不需要自己控制開啟/關閉，也可以傳 `child` 並配合外部 `MenuController` ：

```
dart 体验AI代码助手 代码解读复制代码final MenuController controller = MenuController();

CupertinoMenuAnchor(
  controller: controller,
  menuChildren: <Widget>[
    CupertinoMenuItem(
      onPressed: () => controller.close(),
      child: const Text('完成'),
    ),
  ],
  child: CupertinoButton(
    onPressed: controller.open,
    child: const Text('更多'),
  ),
)
```

> 這裡和 `PlatformMenuBar` 不一樣： `CupertinoMenuAnchor` 是 Flutter 自己繪製的 Cupertino-style popup menu，適合控制項附近的上下文選單； `PlatformMenuBar` 則是交給宿主平台渲染的頂層選單列。

Material 的 `MenuAnchor` 也新增了 Material 3 動畫， `SubmenuButton` 上的新 `hoverOpenDelay` 參數可以控制子選單的互動，預設動畫處於停用狀態，可以透過 animated 設定為 true 來啟用：

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/267d94389119467eba273974d5ec0369~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=gkum5H22wRqxyw0zSkXHQXSoUrs%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/267d94389119467eba273974d5ec0369~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=gkum5H22wRqxyw0zSkXHQXSoUrs%3D)

另外， `CupertinoSheetRoute` 裡面的可捲動內容現在可以和拖曳動畫無縫協作，對於需要自訂拖曳區域的開發者來說，新的 `scrollableBuilder` 支援將託管的 `ScrollController` 傳遞給 body 的可捲動區域：

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/a9a1aa099866468898131f5992975ed8~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=N3I1Y2uifFcgU%2F6RztQ7JSBhpS4%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/a9a1aa099866468898131f5992975ed8~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=N3I1Y2uifFcgU%2F6RztQ7JSBhpS4%3D)

### CarouselView 支援 infinite

CarouselView 這也也進行了大調整，最重要就是支援了 infinite：

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/6276bff9c72e43fca2d7d34443116694~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=SwV01RlAd2j3iDv%2FxFemf9GvBHw%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/6276bff9c72e43fca2d7d34443116694~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1779930330&x-signature=SwV01RlAd2j3iDv%2FxFemf9GvBHw%3D)

Material `CarouselView` 新增 `infinite` 參數，開啟後 Carousel 可以在兩個方向連續循環捲動。

這個能力涵蓋普通 `CarouselView` 、 `CarouselView.weighted` 和 lazy builder 場景，當 `infinite` 為 true 時，carousel 支援無限循環，雙向連續捲動：

```
dart 体验AI代码助手 代码解读复制代码CarouselView(
  itemExtent: 280,
  infinite: true,
  children: List<Widget>.generate(6, (int index) {
    return Card.filled(
      child: Center(child: Text('推薦內容 $index')),
    );
  }),
)
```

### SelectableRegion

不得不說，這個玩意一直都沒完全支援好，這次 3.44 主要解決了兩個問題：

- Web 佈局約束保留：之前 `SelectableRegion` 在網頁上渲染時可能會導致對應子元素意外縮小，現在它會將所有佈局約束原封不動地傳遞給子元素，從而確保一致的尺寸行為
- 多行複製精度，現在 `SelectableRegion` 中的文字選擇更加精確，當使用者選擇並複製跨越多行的文字時，複製的輸出中會正確保留換行符，不會遺失換行符

### Superellipse

另外，Superellipse 現在覆蓋更多 iOS 風格形狀：

- `CupertinoFocusHalo.withRoundedSuperellipse` 用 `RoundedSuperellipseBorder` 繪製更接近 iOS 的平滑圓角 focus halo
- `ShapedInputBorder` 可配合 `RoundedSuperellipseBorder` 做 iOS-style shape
- Cupertino 的 dialog、context menu、sheet、list section、picker、segmented control 等路徑已經基本採用 `ClipRSuperellipse` / `RoundedSuperellipseBorder` 。
```
dart 体验AI代码助手 代码解读复制代码ClipRSuperellipse(
  borderRadius: BorderRadius.circular(28),
  child: Image.asset(
    'assets/cover.jpg',
    width: 160,
    height: 160,
    fit: BoxFit.cover,
  ),
)
```

### ExpansionTile

最後， `ExpansionTileController` 也做了優化，作為 Material 元件 `ExpansionTile` 底層基礎的 `Expansible` 元件， `ExpansibleController` 和 `ExpansibleController` 都新增了切換方法，Material 的列表圖塊 `RadioListTile` 、 `CheckboxListTile` 和 `SwitchListTile` 也支援了 `WidgetStatesController` 。

```
dart 体验AI代码助手 代码解读复制代码import 'package:flutter/material.dart';

class ExpansibleControllerDemo extends StatefulWidget {
  const ExpansibleControllerDemo({super.key});

  @override
  State<ExpansibleControllerDemo> createState() =>
      _ExpansibleControllerDemoState();
}

class _ExpansibleControllerDemoState extends State<ExpansibleControllerDemo> {
  final ExpansibleController _controller = ExpansibleController();

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('ExpansibleController.toggle'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Expansible(
          controller: _controller,
          headerBuilder: (context, animation) {
            return Card(
              child: ListTile(
                title: const Text('自訂 Expansible Header'),
                subtitle: const Text('底層展開元件，不限制 Material 樣式'),
                trailing: RotationTransition(
                  turns: Tween<double>(
                    begin: 0,
                    end: 0.5,
                  ).animate(animation),
                  child: const Icon(Icons.expand_more),
                ),
                onTap: () {
                  _controller.toggle();
                },
              ),
            );
          },
          bodyBuilder: (context, animation) {
            return SizeTransition(
              sizeFactor: animation,
              child: const Card(
                child: Padding(
                  padding: EdgeInsets.all(16),
                  child: Text(
                    '這裡是展開內容。'
                    'Expansible 更適合做完全自訂 UI，'
                    '而 ExpansionTile 是 Material 風格的封裝。',
                  ),
                ),
              ),
            );
          },
        ),
      ),
      floatingActionButton: FloatingActionButton.extended(
        onPressed: () {
          _controller.toggle();
        },
        label: const Text('toggle'),
        icon: const Icon(Icons.swap_vert),
      ),
    );
  }
}
```

## 無障礙

### ProgressIndicator

`ProgressIndicator` 的 `semanticsValue` 現在支援類似 `50%` 的百分比字串，之前 progress bar 語意檢查更偏向數值區間，現在對百分比文字有了明確支援：

```
dart 体验AI代码助手 代码解读复制代码const LinearProgressIndicator(
  value: 0.5,
  semanticsLabel: '檔案上傳進度',
  semanticsValue: '50%',
)
```

同樣適用於 `CircularProgressIndicator` ：

```
dart 体验AI代码助手 代码解读复制代码const CircularProgressIndicator(
  value: 0.75,
  semanticsLabel: '同步進度',
  semanticsValue: '75%',
)
```

### Slider

`Slider` 修復了語意節點尺寸問題， `_RenderSlider.assembleSemanticsNode` 會把語意 rect 設定為以 thumb center 為中心、大小為 `kMinInteractiveDimension` 的區域：

```
dart 体验AI代码助手 代码解读复制代码Slider(
  value: volume,
  min: 0,
  max: 100,
  divisions: 20,
  label: '${volume.round()}',
  onChanged: (double value) {
    setState(() => volume = value);
  },
  semanticFormatterCallback: (double value) {
    return '音量 ${value.round()}%';
  },
)
```

其他還有：

- `PopupMenuButton` 和 `DropdownButton` 的 expanded state 語意更新修復，避免展開狀態沒有正確同步
- `PinnedHeaderSliver` 修復語意焦點捕獲問題
- 不可見 semantics 節點從語意樹中移除時的當機被修復
- 自動播放影片預覽：通知使用者是否已停用影片預覽的自動播放
- 優先使用不閃爍游標：允許 App 為那些覺得閃爍游標分散注意力或難以追蹤的使用者提供穩定的、不閃爍的文字指示器
- 自動播放動畫圖像：偵測使用者何時選擇暫停自動播放 GIF 或其他動畫內容

## 最後

可以看出來，這個版本更新了很多，最核心就是樣式庫已經完成初步解耦落地，後續樣式跟進和液態玻璃效果就能快很多了，同時這也是一個充滿 AI Coidng 場景的更新，當然最可惜的還是桌面的依然不合併到 Stable，雖然我已經用了一段時間了。

> 貌似關注過我的，基本已經提前看到大半的 3.44 更新了吧？

那麼，更新吧騷年，是時候勇敢地吃螃蟹了。

---

原文出處： [https://juejin.cn/post/7641896054782672915](https://juejin.cn/post/7641896054782672915)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aYQeVa) [65瀏覽](https://codelove.tw/@tony/post/aYQeVa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aYQeVa)

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

🔥 [50 多個必備 Telegram 機器人的終極清單](https://codelove.tw/@tony/post/37oZV3)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝18 💬11 ❤️1

550

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

82

🥉

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

#4

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779686332&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYQeVa&amp;pra=5&amp;asro=0&amp;itsi=2&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779686330962&amp;bpp=1&amp;bdt=295&amp;idt=1933&amp;shv=r20260520&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779686067%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779686067%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779686067%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6249059954515&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=42533293%2C95390278%2C95391807%2C95386195&amp;oid=2&amp;pvsid=5436456112053889&amp;tmod=1302172221&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=1963" title="Advertisement" aria-label="Advertisement"></iframe>