---
title: "真正的跨平台 AI 自動化框架，甚至還支援鴻蒙"
source: "https://codelove.tw/@tony/post/xNQ16q#google_vignette"
author:
published:
created: 2026-06-09
description: "真正的跨平台 AI 自動化框架，甚至還支援鴻蒙  ==========================    之前介紹過最好用的 [Flutter AI 測試框架](https://juejin.cn/post/7646696467931807807)，然後有很多人在問有沒有原生平台的，剛好就..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月08日](https://codelove.tw/@tony/post/xNQ16q "2026-06-08 19:03:00")

## 真正的跨平台 AI 自動化框架，甚至還支援鴻蒙

## 真正的跨平台 AI 自動化框架，甚至還支援鴻蒙

```
之前介紹過最好用的 [Flutter AI 測試框架](https://juejin.cn/post/7646696467931807807)，然後有很多人在問有沒有原生平台的，剛好就看到了一个原生平台的開源測試框架 \`agent-device\`，甚至它還支援鴻蒙：
```

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/200632b0e34845318920291fde333d4c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1781485363&x-signature=4o87RKKDNc6XD9kXHnXsB2gYghw%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/200632b0e34845318920291fde333d4c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1781485363&x-signature=4o87RKKDNc6XD9kXHnXsB2gYghw%3D)

**`agent-device` 是專門為 AI Agent 設計的裝置自動化 CLI 工具** ，可以類比 Vercel 的 `agent-browser` 是給 AI 用的瀏覽器自動化工具那樣， `agent-device` 是給 AI 用的 App 準備的標準化通道，例如：

```
sql 体验AI代码助手 代码解读复制代码agent-device apps --platform ios
agent-device open <app> --platform ios
agent-device snapshot -i
agent-device press @e12
agent-device diff snapshot -i
agent-device close
```

> 比如這裡的 `snapshot -i` ，它會把目前 UI 轉成 Agent 更容易理解的結構化樹，同時給可互動元素生成 `@e1` 、 `@e2` 這種引用。

**它最核心的能力是不需要透過截圖去辨識控制項** ， `agent-device` 是透過檢查真實 App UI，輸出 **無障礙存取樹（accessibility snapshot）** ，然後將 UI 中的可互動元素映射為短引用（ `@e1`, `@e2`, `@e3` ），讓 AI 知道畫面上有什麼：

```
java 体验AI代码助手 代码解读复制代码agent-device snapshot -i
# @e1 [heading] "Settings"
# @e2 [button] "Sign In"
# @e3 [text-field] "Email"
```

這樣 AI 就可以直接用 `@e3` 操作「 `[text-field] "Email"` 」，不需要自己去讀座標，也不需要每次截圖分析，同時功能上支援：

- 截圖：用於 debug／回放
- 錄影（record）：錄製操作流程
- 日誌（logs）：app 日誌流
- 網路流量（network）：HTTP 請求封包擷取
- 效能採樣（perf）：CPU／記憶體
- 當機上下文：crash 時的現場資訊

甚至它還支援工作流回放，將 AI 探索過的互動錄製成 `.ad` 腳本，然後可以在 CI、本地、重複場景中回放執行，等效於 e2e 測試案例。

> `agent-device` 的場景其實就是：Agent 寫程式，之後安裝到對應裝置，然後 agent-device 打開 App，透過 snapshot 讀取目前 UI 結構，之後 Agent 根據 `@e1` / `@e2` / `selector` 操作 UI，然後得到「截圖／錄影／日誌／網路／效能」等證據存檔，最後再讓 Agent 根據測試結果修復問題。

[![](https://i.imgur.com/9Lu1Gj2.jpeg)](https://i.imgur.com/9Lu1Gj2.jpeg)

當然，最最最最重要的是它是多平台支援：

- iOS 真機／Simulator（XCTest 驅動）
- Android Emulator／真機（ADB 驅動）
- tvOS／Android TV
- macOS App
- Linux（AT-SPI）
- HarmonyOS（HDC 驅動）

整個流程大概如下圖所示：

[![](https://i.imgur.com/KXGYClw.jpeg)](https://i.imgur.com/KXGYClw.jpeg)

如果拆開來看，整個架構大概如下：

- **Session 模型** ：先 `open` 一個裝置工作階段，後續所有命令在這個 session 上下文內執行（鎖住裝置資源、保持狀態）
- **Daemon 架構** ：背景執行一個常駐程序（daemon），CLI 命令透過 IPC 與 daemon 通訊
- **提升效率** ：snapshot 使用無障礙樹而不是視覺截圖，讓 AI 用最少 token 取得 UI 資訊； `-i` （interactive only）進一步只回傳可互動元素，而且用截圖做測試的方式真的超慢
- **語義引用系統** （Refs）： `@e1~@eN` 是每次 snapshot 後分配的臨時引用，命令可直接用 ref 而不是座標，降低 AI 犯錯機率，比如座標在捲動後就失效，但 ref 是具語義感知的
- **Interactor 模式** ：每個平台實作一個統一的 `Interactor` 介面（ `tap`, `swipe`, `snapshot`, `screenshot`...），核心調度層對介面編程，平台差異被隔離在各自的 `platforms/` 目錄
- **可控範圍** ： `src/core/capabilities.ts` 為每條命令定義哪些平台／裝置類型支援，防止 AI 呼叫不支援的命令，會在 admission 階段直接報錯而不是無意義地送到裝置

當然，也存在一些平台限制，例如：

局限具體表現 **pinch/rotate 手勢** 僅支援 iOS Simulator + Android，HarmonyOS 不支援 pinch/rotate **`read` （文字識別）** 只支援 iOS/Android **`logs` / `network` / `perf`** HarmonyOS 均不支援 **`clipboard`** HarmonyOS 需要 app-level 授權，讀寫均可能失敗 **權限管理** HarmonyOS 僅支援 grant，deny/reset 未實作然後在平台鏈路上，每個平台都有自己一套實作，從程式碼看，你會看到一套非常複雜的工作流程。

## iOS 體系

首先是 iOS， **iOS 核心用的還是我們熟悉的 XCTest Runner** ，iOS 本身沒有對外開放的 UI 自動化 API 給外部程序使用，這裡 `agent-device` 的做法是：

> **在目標裝置上動態建構並執行一個 XCTest bundle（`.xctestrun` ），透過 XCTest 框架的 `XCUITest` 能力操控 UI** ，然後在這個 Runner 程序和 CLI 之間建立 TCP 通訊通道。

[![](https://i.imgur.com/lkGPn2G.jpeg)](https://i.imgur.com/lkGPn2G.jpeg)

更具體的結構是：

- `runner-xctestrun.ts` ：動態生成 `.xctestrun` 設定檔（相當於 Xcode 的測試 scheme）、建構參數、signing 設定
- `runner-session.ts` ：管理 Runner 程序的生命週期，啟動 `xcodebuild test-without-building` 、等待 ready、保持 session 熱啟動
- `runner-transport.ts` ：TCP 通訊層，發送 JSON 命令到 Runner，接收回應，逾時／重連處理
- `runner-client.ts` ：命令執行的上層入口， **完整的故障復原邏輯**
- `runner-contract.ts` ：Runner 通訊協議定義，命令類型列舉
- `interactions.ts` ：將 `tap/swipe/type` 等抽象命令翻譯為 Runner 命令
- `xml.ts` ：解析 XCTest 回傳的 Accessibility XML 快照
- `simctl.ts` ：呼叫 `xcrun simctl` 操作模擬器（boot/shutdown/install）
- `devicectl.ts` ：呼叫 Xcode 15+ 的 `xcrun devicectl` 操作真機
- `perf.ts` ：透過 `xcodebuild` + Instruments XML 擷取效能資料（CPU／記憶體／幀率）

**這裡最有意思的就是 Runner 故障復原機制** ，由於 XCTest Runner 是一個獨立程序，TCP 通訊可能在命令執行過程中中斷（網路抖動、Runner 當掉），所以專案設計了一套 **lifecycle 狀態機** ：

- 每個命令送出前打上唯一 `commandId`
- 如果傳輸中斷，送出 `status` 命令查詢該 `commandId` 的狀態
- 根據 `lifecycleState` 決策：
	- `completed` （已完成）：從 `lifecycleResponseJson` 取結果，session 保留
		- `failed` （已失敗）：丟出 Runner 回報的錯誤，session 保留
		- `accepted/started` （仍在飛行中）：丟出 in-flight 錯誤，session 保留
		- 未知狀態：保守路徑，invalidate session，重新啟動

> 比如 CLI 發出了一個 `tap` 命令，TCP 連線中途斷了，CLI 根本不知道 Runner 是否已經執行了這個 tap，如果重試可能會 double-tap，但如果不重試，可能 tap 根本沒發生，這時候就需要透過 lifecycle 判斷。

[![](https://i.imgur.com/7J7EXgb.jpeg)](https://i.imgur.com/7J7EXgb.jpeg)

另外 macOS 和 iOS 共用了一套後端，所以邏輯基本可以複用，另外截圖功能上也是做了很多細節處理，比如：

> 狀態列內容覆蓋，將時間／電量偽造為固定值，讓截圖更容易區分。

## Android

**Android 平台就是 ADB + 自訂 Snapshot Helper APK** ，類似 iOS 的 Runner transport，只是 Android 走的是 `adb` 命令（ `adb shell` 、 `adb exec-out` 、 `adb install` 等）：

- 本地 `adb -s <serial>` 直連
- Provider 注入模式（可以支援雲端／遠端 Android 裝置）
- 文字注入 provider 覆蓋（用於非 ASCII 字元）

**Android 平台最有意思的就是雙引擎架構快照實作** ：

[![](https://i.imgur.com/YwvZPGt.jpeg)](https://i.imgur.com/YwvZPGt.jpeg)

為什麼需要自訂 Helper APK？因為 Android 原生的 `uiautomator dump` 有嚴重缺陷：

- 經常逾時（UI 繁忙時 hang 住）
- 回傳的節點數有限，無法取得完整 `can-scroll-forward/backward` 屬性
- 不支援多視窗

所以 `agent-device` 開發了一個 **`android-snapshot-helper`** APK（專案根目錄有獨立子包），透過 Android Instrumentation 框架執行，能回傳更完整和穩定的 UI 樹 XML，同時它還維護了一個 **persistent session** （常駐背景程序），避免每次快照都 cold start，例如：

- `snapshot-helper-install.ts` ：檢測版本／安裝 APK
- `snapshot-helper-session.ts` ：常駐 session 管理
- `snapshot-helper-capture.ts` ：透過 instrumentation 呼叫取得 XML

另外一個就是捲動內容提示，這個也挺有意思，Android 快照時， `agent-device` 會額外執行 `adb shell dumpsys activity top` ，從 Activity 狀態中推斷每個 ScrollView/RecyclerView 是否還有隱藏內容（上方／下方），然後標註到節點上，讓 AI 知道「這個列表下面還有更多內容」。

## Linux

**Linux 平台用的就是 AT-SPI2（Assistive Technology Service Provider Interface）+ Python 橋接** ，之前 `agent-device` 也嘗試過 `node-gtk` （Node.js 原生綁定），但 ABI 不相容、CI 編譯問題太多，所以現在改用 Python3 + `python3-gi` （PyGObject）呼叫 AT-SPI2，Node.js 負責啟動 Python 子程序、讀取 JSON 輸出。

> AT-SPI2 是 Linux 桌面輔助功能標準（GNOME/KDE 等都支援），類似 macOS 的 Accessibility API，每個 GTK/Qt 視窗會把自己的 UI 樹註冊到 D-Bus 上，透過 AT-SPI2 可以遍歷所有應用程式的 UI 節點。

## 鴻蒙

**最後鴻蒙平台主要透過 `HDC` 鏈路來適配** ，可以類比 Android 的 ADB，HarmonyOS 使用的是 `hdc` 命令列工具，核心是 `runHarmonyHdc(device, args)` 會自動帶上 `-t <serial>` 裝置序號，與 ADB 的用法完全鏡像。

**UI 快照層 ArkUI Hierarchy（等價於 iOS 的 XCTest / Android 的 ViewHierarchy）** ，比如 `hdc shell uitest dumpLayout -p /data/local/tmp/xxx.json` ，最終得到 `@e1, @e2...` 等節點，最重要的是它還處理了鴻蒙本身的一個問題相容：

> **uitest uiRecord 卡死時 dumpLayout 會 hang** ，所以專案加了預檢，發現 stuck 程序時直接報錯提示 reboot。

所以鴻蒙的 snapshot 是透過 `uitest dumpLayout` 生成 ArkUI component tree，然後支援 `interactiveOnly` 、 `compact` 、 `depth` 、 `scope` 、 `raw` 模式，同時透過 `snapshot_display` 進行截圖。

還有一些有趣的，比如 Alert 辨識（純 ArkUI 節點分析），不像 iOS 有原生的 alert API，HarmonyOS 的彈窗是普通 ArkUI 元件，所以 `agent-device` 是透過 **關鍵字匹配辨識彈窗並擷取按鈕座標** ，同時支援中英文按鈕文字（ `確定/取消/允許/拒絕/知道了`...）：

```
less 体验AI代码助手 代码解读复制代码if (type.includes('dialog') || type.includes('alert') || 
    label.includes('暂无可用打开方式') || label.includes('权限') ...) {
  // 识别为弹窗，提取 buttons
}
```

## 最後

對比 Appium、Detox、Maestro 這些傳統 mobile automation framework， `agent-device` 是為 AI Agent 優化的，更適合 AI 自動化測試，當然它對人寫的場景就不是很友好了，如果真要對比，我理解是：

- Appium / Detox / Maestro：支援人寫，重點是穩定、可維護、CI 回歸
- agent-device：面向 AI Agent 現場探索、驗證、debug、採證、沉澱 replay

目前看起來它還不是特別適合大規模的穩定回歸測試，一些複雜 UI 的測試也還不夠穩定，所以它更適合作為 Agent 開發過程中的即時回饋和修復支援， **讓 Agent 改完程式後自己跑起來看、自己點、自己截圖、自己抓日誌，然後基於真實結果繼續修，而且支援原生跨平台** ，最重要的是，開源、免費、MIT。

## 連結

[github.com/callstack/a…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fcallstack%2Fagent-device)

---

原文出處： [https://juejin.cn/post/7648520470531473471](https://juejin.cn/post/7648520470531473471)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xNQ16q) [20瀏覽](https://codelove.tw/@tony/post/xNQ16q)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xNQ16q)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780973929&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxNQ16q&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780973929296&amp;bpp=2&amp;bdt=49&amp;idt=43&amp;shv=r20260604&amp;mjsv=m202606030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780973912%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780973912%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780973912%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=7501802543869&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=8&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390668&amp;oid=2&amp;pvsid=7149021569318097&amp;tmod=173356463&amp;uas=1&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=1920%2C80%2C1920%2C80%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=52" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>