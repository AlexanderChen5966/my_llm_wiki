---
title: "Flutter 發表 Skills 官方，Flutter 在 AI 領域再添一助力"
source: "https://codelove.tw/@tony/post/3Vz2y3"
author:
published:
created: 2026-05-25
description: "# Flutter 發布官方 Skills ，Flutter 在 AI 領域再添一助力在之前的 [《Flutter 2026 Roadmap》](https://juejin.cn/post/7609927980681314310) 我們就聊到，如今一個專案要有發展前景，就必須能夠和 AI 持續..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [03月07日](https://codelove.tw/@tony/post/3Vz2y3 "2026-03-07 13:01:18")

## Flutter 發表 Skills 官方，Flutter 在 AI 領域再添一助力

## Flutter 發布官方 Skills ，Flutter 在 AI 領域再添一助力

在之前的 [《Flutter 2026 Roadmap》](https://juejin.cn/post/7609927980681314310) 我們就聊到，如今一個專案要有發展前景，就必須能夠和 AI 持續貼一起，而顯然 Flutter 做到了，除了之前的 Flutter MCP 、GenUI 等 AI 相關支持產品，現在官方又新增了 [flutter/skills](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fflutter%2Fskills) 支持，用於補全 Flutter 在 AI 程式設計缺乏官方 skills 的短板。

`flutter/skills` 作为 **給 AI Agent（Agent / Copilot / Claude / Cursor 等）用的技能庫，核心目標是讓 AI 在 Flutter 專案裡，面對執行複雜開發流程時，AI 有相應的規範和指導流程** ：

- **決策邏輯** ：告訴 AI 什麼時候該選哪種方案
- **詳細指令** ：包含具體的程式碼實現和步驟
- **嚴格約束** ：防止 AI 生成錯誤或過時的程式碼（如禁止使用已廢棄的 API）

當然，這些 skills 所需的資料，其實都來自專案裡的 Skills CLI ，Skills CLI 會調用 Gemini，把官方的 Flutter 文檔“壓縮成可執行的 [SKILL.md](http://skill.md/) ” ，也就是它會 crawl 文檔網站找頁面，然後用 Gemini 轉成結構化 `SKILL.md` ，對此 CLI 的主要有兩個核心命令：

- `generate-skill` ：從 YAML 生成（支持 `--skill` 只生成某一個；支持 `--directory` 輸出到指定目錄）
- `validate-skill` ：重新生成並和已有 skills 比對，用來做一致性/回歸驗證（還支持 `--thinking-budget` ）

具體流程為：

- 透過 `resources/flutter_skills.yaml` 指定技能名稱、描述以及原始文檔的資源連結（HTTPS 或本地檔案）
- `ResourceFetcherService` 抓取網頁內容，並由 `MarkdownConverter` 將 HTML 轉換為乾淨的 Markdown 格式，去除導航欄、頁腳等噪點
- 調用 Gemini 接口，向 AI 發送原始文檔、技能模板指令（ `skill_instructions.dart` ）和特定提示詞（ `Prompts.dart` ），要求 AI 生成符合規範的技能文檔
- 生成的 `SKILL.md` 包含統一的 H1 標題、## Goal（目標）、## Instructions（指令）和 ## Constraints（約束）和程式碼片段
- 提供 `validate-skill` 命令，讓 AI 重新審視已生成的技能，根據準確性、結構和完整性進行打分（0-100分），確保輸出質量

而在目前的專案，主要覆蓋有 20 多個不同技能：

| **類別** | **技能** | **能力** |
| --- | --- | --- |
| **基礎環境** | `linux/macos/windows-setup` | 配置三端的 Flutter 開發環境及工具鏈 |
| **核心架構** | `flutter-architecture` | 實現 clean Flutter 腳手架和單一資料源架構 |
| **狀態與數據** | `flutter-state-management` | 處理全局與局部狀態，實施單向資料流 |
|  | `flutter-databases` | 基於 `sqflite` 的持久化存儲實現 |
|  | `flutter-caching` | 記憶體、檔案系統、圖像等多層快取策略 |
| **UI 與 布局** | `flutter-layout` | 約束系統、回應式與自適應佈局 |
|  | `flutter-animation` | 實現隱式、顯式、物理及 hero 動畫 |
|  | `flutter-theming` | Material 3 主題適配、按鈕遷移與平台常用設計 |
| **網路與並發** | `flutter-http-and-json` | 處理非同步網路請求與結構化 JSON 序列化 |
|  | `flutter-concurrency` | 使用 Isolates 處理背景任務（如大資料解析） |
| **原生整合** | `flutter-native-interop` | FFI 呼叫 C/C++、平台通道（MethodChannel）通訊 |
|  | `flutter-platform-views` | 在 Flutter 中嵌入原生 Android/iOS 視圖 |
|  | `flutter-plugins` | 跨平台插件及嵌入生命週期管理 |
| **功能特性** | `flutter-navigation` | 路由跳轉、動態參數傳遞與深層連結處理 |
|  | `flutter-localization` | 多語言支持 |
|  | `flutter-accessibility` | 輔助技術支持、語義化標籤與無障礙測試 |
| **優化與測試** | `flutter-performance` | 辨識渲染抖動、優化 UI 線程與 Raster 線程性能 |
|  | `flutter-app-size` | 測量並縮減安裝包體積（分析建構產物） |
|  | `flutter-testing` | 單元測試、元件測試與整合測試 |

這些技能在 AI Coding 的時候，可以很好幫助 AI 完成專案管理和迭代，比如 `flutter-plugins` 裡：

- 會明確包含哪些修改 `build.gradle` 或 `Info.plist` 的具體步驟
- **是使用 `ffi` 還是 Method Channels**
- 是不是需要使用 **Package-Separated Federated Plugin**

這對於 AI 在決策和實現 Flutter Plugin 程式碼生成的規範和成功率都有明顯提升，同樣的，對於 `flutter-architecture` ，可以讓 AI 在寫程式碼時遵循「Clean Architecture」 ，讓 AI 輸出單向資料流 + 嚴格分層（UI/Domain/Data）+ 職責隔離的架構，例如：

- **分層約束** ：UI / Data / Domain 的職責邊界
- **資料源拆分** ：Remote Service vs Local Service 的建模方式
- **Service 純封裝** ：Service 必須無狀態、無業務邏輯
- **Repository 單一可信源** ：只有 Repository 能 mutate 資料
- **錯誤顯式化** ：Repository 捕捉異常並返回 `Result Ok/Error`
- **可觀察資料流** ：Repository 可透過 Stream 對外暴露變化
- **狀態管理** ： `ChangeNotifier` 持有 UI state、集中處理互動
- **Command 模式** ：用 `Command0` 封裝非同步動作/互動入口，避免 View 直接寫 async 邏輯
- **View 零業務邏輯** ：View 只能佈局/簡單條件渲染/路由，不做資料變更
- **單向資料流校驗規則** ：Repository → ViewModel → View（資料下行），View → ViewModel → Repository（事件上行），並提供“Validate and Fix”的修復策略

另外 `flutter-performance` 也很實用，它把性能優化定義成三類問題，並且以 **16ms 帧預算** 為基準來判斷 jank：

- UI thread（Dart / build 階段）卡頓
- Raster thread（GPU / paint 階段）卡頓
- Web 端 profiling 的特殊路徑

`flutter-performance` 的決策邏輯是：

- 要不要先做 baseline？
	- 要：寫 integration test，用 `traceAction` + `TimelineSummary`
- 是否 Web？
	- Web：開 profiling flags + Chrome DevTools Performance
		- 非 Web：真機 `--profile` + Flutter DevTools
- 看 DevTools 裡哪條線程紅（>16ms）：
	- UI：優化 build / setState 顆粒度 / const / StringBuffer
		- Raster：減少 saveLayer/Opacity/Clip/ImageFilter，等等
		- 兩邊都紅：先 UI 再 Raster（因為 Dart 端過慢會連鎖影響渲染）

同時，它也提供了一些優化建議給 AI ，例如：

- UI Thread（Build 成本）
	- “localize setState” —— 把狀態下沉，縮小重建範圍
		- “use const” —— 減少重建遍歷
		- “循環裡字串拼接用 StringBuffer”
- Raster Thread（繪製成本）
	- 避免 `Clip.antiAliasWithSaveLayer`
		- 少用 `saveLayer` 、 `ImageFilter`
- Layout / Intrinsics
	- `ListView.builder` / `GridView.builder`
		- 避免不必要的 `shrinkWrap: true`

可以看到，這些技能也許你不用 `flutter/skills` ，大模型也可能會做，但是有了 `flutter/skills` 之後，就可以讓 AI 明確實施的方案， **讓 AI 看起來更像一個專業的 Flutter 高級開發工程師** 。

同時， `flutter/skills` 也是大大降低了開發者使用 AI 輔助開發的門檻，它將原本散落在各處的靜態知識庫，提煉成了 AI 代理可以直接理解並執行的“標準操作”，這也是 AI 時代的必經之路。

## 連結

[github.com/flutter/ski…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fflutter%2Fskills%2Ftree%2Fgenerate-skills)

---

原文出處： [https://juejin.cn/post/7613318336118882323](https://juejin.cn/post/7613318336118882323)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3Vz2y3) [986瀏覽](https://codelove.tw/@tony/post/3Vz2y3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3Vz2y3)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779689730&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3Vz2y3&amp;pra=5&amp;asro=0&amp;itsi=43&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779689730602&amp;bpp=1&amp;bdt=71&amp;idt=87&amp;shv=r20260520&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779686994%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779686994%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779686994%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4769245168224&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95391808&amp;oid=2&amp;pvsid=7010689197396606&amp;tmod=1302172221&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=114" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260520/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>