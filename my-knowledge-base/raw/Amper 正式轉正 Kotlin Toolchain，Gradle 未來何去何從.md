---
title: "Amper 正式轉正 Kotlin Toolchain，Gradle 未來何去何從"
source: "https://codelove.tw/@tony/post/qP5DAx"
author:
published:
created: 2026-07-07
description: "Amper 正式併入 Kotlin Toolchain，Gradle 未來何去何從=============================================    最近 Amper 發布了[《*Kotlin Toolchain 0.11: The Next Step for A..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月03日](https://codelove.tw/@tony/post/qP5DAx "2026-07-03 19:05:27")

## Amper 正式併入 Kotlin Toolchain，Gradle 未來何去何從

```
最近 Amper 發布了[《*Kotlin Toolchain 0.11: The Next Step for Amper*》](https://link.juejin.cn?target=https%3A%2F%2Fblog.jetbrains.com%2Famper%2F2026%2F06%2Fkotlin-toolchain-0-11%2F) ，核心就是 *Amper has evolved into the Kotlin Toolchain and is now Alpha* ，**也就是 JetBrains 正式把 Amper 併入 Kotlin Toolchain，並且從實驗階段推進到 Alpha** 。
```

> 感覺就是，JetBrains 想給 Kotlin 生態做一個類似 `cargo` / `dotnet` / `go` 那種統一入口。

從 Kotlin Toolchain 0.11 開始，Amper 這個名字在產品定位上就被降級了，核心能力被整體遷移進 Kotlin Toolchain，以後也沒有 `amper` 命令入口了，全部統一成 `kotlin` ：

> **原來的 `amper` / `amper.bat` wrapper 要替換成新的 `kotlin` / `kotlin.bat` wrapper，IDE 外掛也從 Amper 外掛切換為 Kotlin Toolchain 外掛** 。

從產品上看，以前 Amper 的定位是「幫你用 YAML 設定 Kotlin/KMP 工程」，而現在變成了 Kotlin Toolchain 的一部分，成了 Kotlin 專案的統一入口，可以透過一個 `kotlin` 命令建立、建置、執行、測試、發布專案。

JetBrains 之所以這麼做，主要是覺得現在的 Kotlin 生態太割裂了，現在做一個 Kotlin 專案需要處理一堆東西：

> Kotlin compiler、Gradle、Kotlin Gradle Plugin、Android Gradle Plugin、Kotlin/Native、Xcode/iOS 產物、Maven 發布、Maven Central 簽章、source set、平台差異設定……

而現在 Kotlin Toolchain 把 Kotlin 的工程入口從 Gradle 腳本、外掛版本、模板碎片，收斂成一個統一的 Kotlin 官方入口，目的就是：

> **Kotlin 語言很現代，但工程體驗還是 JVM 清朝那一套，改變了很多，卻又像沒變。**

所以 0.11 開始 `kotlin` 就是統一入口了，比如 `kotlin build` / `kotlin run` / `kotlin test` / `kotlin publish` ，同時全域 `kotlin` 命令不會強行讓所有專案使用同一個工具鏈版本，它會自動找到專案裡的 wrapper，然後執行相符版本。

另外 0.11 也終於支援發佈 JVM 函式庫到 Maven 儲存庫，包括 Maven Central，Maven Central 之前發佈很麻煩：

> sources jar、javadoc jar、PGP 簽章、POM 中繼資料、checksum、deployment bundle 等等，這些東西 Kotlin Toolchain 現在會把這些自動處理掉，你在 `module.yaml` 裡宣告 publishing 設定，然後執行 `kotlin publish mavenCentral` ，它就會建置、簽章、打包、上傳並等待 Maven Central 驗證。

不過 **目前 Kotlin Multiplatform 函式庫還不能發布** ，KMP library publishing 還在做，暫時只能用在 `product: jvm/lib` ：

```
yaml 代码解读复制代码product: jvm/lib
​
description: A meaningful description for this specific module
​
settings:
  publishing:
    enabled: true
    group: com.example # the group has to match your Maven Central namespace
    version: 1.0.0
    # artifactId is optional, and defaults to your module's name
    mavenCentral: enabled
    signArtifacts: true # automatically sign your artifacts, no external GPG binary required
    publishSources: true
    pom:
      url: https://example.com
      scm: https://github.com/my-org/example.git # the SCM connection and dev connection are automatically derived from this
      developers:
        - name: John Doe
      licenses:
        - name: MIT
          url: https://opensource.org/license/mit
```

**其次還有 Cinterop 支援增強** ，Kotlin Toolchain 現在可以根據模組裡 `cinterop` 資料夾下的 `.def` 檔生成 C 函式庫 binding，同時 IDE sync 階段也會生成 binding 並提供輔助。

> 這個對 KMP / Kotlin Native 很關鍵，因為 Cinterop 如果能被 Toolchain 統一管理，就能減少 Kotlin/Native 工程裡大量手動設定。

[![](https://i.imgur.com/IqpUJMo.jpeg)](https://i.imgur.com/IqpUJMo.jpeg)

同時 0.11 也改善了終端輸出和 IDE 同步，例如：

- library sources 會在 sync 後自動背景下載，這樣 sync 完你可以先開始寫程式，不用等 sources 下載完成
- IDE 外掛的相依性解析從「專案級」對齊為「模組級」，和 CLI 行為一致，以前 IDE 可能給某個模組解析出錯誤的相依版本，或者編輯器警告和 CLI 不一致，現在每個 module 有自己的 resolution scope

[![](https://i.imgur.com/vqfX7A7.jpeg)](https://i.imgur.com/vqfX7A7.jpeg)

核心就是 **CLI 和 IDE 行為一致** ，這也是 Amper / Kotlin Toolchain 的核心價值之一，因為 Gradle 生態長期痛點就是各種：

- 命令列能過，IDE 紅字
- IDE 不紅，CI 掛掉
- 同步慢
- 相依性解析不一致
- source set / target / variant 組合複雜

[![](https://i.imgur.com/oHwUKJ6.jpeg)](https://i.imgur.com/oHwUKJ6.jpeg)

所以現在 Kotlin Toolchain 如果能把 IDE model 和 build model 做成同一套宣告式結構，對 KMP 會很有價值。

**最後 0.11 還增加了幾個外掛開發能力** ：

- `checks` ：外掛可以註冊品質檢查，例如 lint，然後透過 `kotlin check` 執行
```
yaml 代码解读复制代码# my-lint-plugin/plugin.yaml
tasks:
  runLinter:
    action: !kotlinJavaLint
      sources: ${module.kotlinJavaSources}
​
checks:
  - name: lint
    performedBy: runLinter
```
- `commands` ：外掛可以公開命令，例如 `kotlin do updateBaseline`
```
yaml 代码解读复制代码# my-lint-plugin/plugin.yaml
tasks:
  updateBaseline:
    action: !runDetektForBaseline
      sources: ${module.kotlinJavaSources}
      outputFile: ${module.rootDir}/detekt/baseline.xml
​
commands:
  # shorthand when the name of the command matches that of the task
  - updateBaseline
```
- `generated` ：統一宣告 generated sources、resources、classes、cinterop definitions，取代舊的 `markOutputAs`
```
yaml 代码解读复制代码tasks:
  generateStuff:
    action: !myGenerateStuffAction
      outputSources: ${taskOutputDir}/src
      outputResources: ${taskOutputDir}/res
      outputDefFiles: ${taskOutputDir}/cinterop
​
generated:
  sources:
    - directory: ${tasks.generateStuff.action.outputSources}
      language: kotlin
  resources:
    - directory: ${tasks.generateStuff.action.outputResources}
  cinteropDefinitions:
    - directory: ${tasks.generateStuff.action.outputDefFiles}
```

> **關於這個 JetBrains 還特別強調，這樣的生成物可以被人、AI agent 和其他工具更容易識別** 。

這一點很有意思，也就是 Kotlin Toolchain 明顯不只是給人寫設定用的，它也在為 AI coding / agent 場景做準備，而 YAML 宣告式設定明顯比 Gradle Kotlin DSL 更容易被工具理解。

> Gradle 腳本本質上是可執行程式碼，裡面可以有條件判斷、函式呼叫、動態任務註冊、外掛魔法嗎，這些對 IDE、CI、AI agent 來說都很難靜態分析。

從目前看， **Amper 進入 Kotlin Toolchain 之後，也代表著它正式成為未來的主角，kotlin 統一入口也意味著未來 gradlew 等命令會慢慢邊緣化** 。

> **短期看 Gradle 還會在，但是現在在 Kotlin-first / KMP-first 的新專案裡，Gradle 的「直接可見度」會被逐步降低** 。

想想，JetBrains 2023 年介紹 Amper 時還說，Amper 是作為 Gradle plugin 實作的，使用 YAML 做專案設定，目的是先驗證使用者體驗，同時也強調他們會承諾支援 Maven 和 Gradle，不會改變對這些技術的支援。

但是 2026 之後，Amper 就不再和 Gradle 有關係，更多強調的是 “ *ecosystem doesn’t just need another build tool – it needs a unified entry point into all of Kotlin* ”，也就是 JetBrains 的已經從「改善 Gradle 設定體驗」的目標，升級成了「建立 Kotlin 官方工具鏈入口」。

所以 0.11 這次不是一個小版本，它同時也是一個訊號： JetBrains 在告訴社群：

> **Kotlin 不想再只是「語言 + Gradle 外掛」，它已經變成一個完整的工具鏈生態。**

## 連結

[blog.jetbrains.com/amper/2026/…](https://link.juejin.cn/?target=https%3A%2F%2Fblog.jetbrains.com%2Famper%2F2026%2F06%2Fkotlin-toolchain-0-11%2F%23terminal-ui-improvements)

---

原文出處： [https://juejin.cn/post/7656202263124082730](https://juejin.cn/post/7656202263124082730)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qP5DAx) [21瀏覽](https://codelove.tw/@tony/post/qP5DAx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qP5DAx)

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

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️4

689

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

137

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783386023&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqP5DAx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783386022975&amp;bpp=1&amp;bdt=296&amp;idt=49&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783385939%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=7694947957840&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395661%2C42533293%2C95393485&amp;oid=2&amp;pvsid=6265906715372247&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=99" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>