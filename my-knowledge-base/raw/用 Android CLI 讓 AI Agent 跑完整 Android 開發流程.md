---
title: "用 Android CLI 讓 AI Agent 跑完整 Android 開發流程"
source: "https://medium.com/ddsakura-blog/%E7%94%A8-android-cli-%E8%AE%93-ai-agent-%E8%B7%91%E5%AE%8C%E6%95%B4-android-%E9%96%8B%E7%99%BC%E6%B5%81%E7%A8%8B-c09023a292c8"
author:
  - "[[ddsakura]]"
published: 2026-05-10
created: 2026-06-09
description: "最近 Google 推出 Android CLI 與 Android Skills。簡單來說，透過它們可以把 Android 開發流程，整理成比較適合 AI agent 使用的 workflow。以前 AI agent 若要操作 Android project，可能會需要多方嘗試"
tags:
  - "clippings"
status: processed
---
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*15Hr_GGG2fnMk7PKFp4i_Q.jpeg)

最近 Google 推出 Android CLI 與 Android Skills。  
簡單來說，透過它們可以把 Android 開發流程，  
整理成比較適合 AI agent 使用的 workflow。  
以前 AI agent 若要操作 Android project，  
可能會需要多方嘗試類似下方努力：

```c
Gradle
adb
emulator
shell script
Android Studio integration
```

但現在 Android 官方開始提供：

```c
統一的 android CLI
官方 skills
```

讓 AI agent 可以比較容易操作 Android 專案怎麼 build、deploy、inspect UI 等功能。

要使用 Android CLI， 首先先透過下列指令安裝

```c
curl -fsSL https://dl.google.com/android/cli/latest/darwin_arm64/install.sh | bash
```

接著執行 android init 這會把相關的 Android Skills 安裝到本機有支援的 AI agent client。 例如：

```c
Gemini CLI
Codex
Claude Code
```

為了實際理解 Android CLI 怎麼使用，  
最近做了一個小 spike，  
測試的目標很單純，  
讓 AI Agent 可以使用 Android CLI 達到：

```c
不開 Android Studio
build Android app
啟動已存在的 emulator
deploy app
inspect 畫面結構
點擊畫面上的按鈕、切 activity、按 back
用 Android 官方 skills 做專案檢查，例如 edge-to-edge
```

實驗下來，  
Android CLI 確實可以做到上述事項，  
不過應該也還有不少可以進步的空間吧。

### Android CLI 現在是統一入口

以前我們可能會直接下：

```c
emulator @Pixel_9
adb install ...
adb shell input tap ...
```

但現在 Android 官方文件裡，很多操作被包在新的 `android` CLI 底下。例如：

```c
android emulator start Pixel_9
android run --device=emulator-5554 --apks=app/build/outputs/apk/debug/app-debug.apk
android layout --device=emulator-5554 --pretty
android screen capture --output=ui.png
```

我覺得統一介面還蠻合理的，  
不過實測下來，  
即使有 skills 教 AI agent，  
運氣不好時，  
AI 還是要反覆試錯幾次就是。

### Build app 還是 Gradle 的守備範圍

這次也確認了一點，  
目前 `android-cli` 並沒有取代 Gradle。  
build Android app 還是走：

```c
./gradlew assembleDebug
```

AI agent 在使用 gradle build app 成功後，  
接著才用 Android CLI deploy 到 emulator：

```c
android run --device=emulator-5554 --apks=app/build/outputs/apk/debug/app-debug.apk
```

所以比較準確的分工是：

```c
Gradle: build app
Android CLI: emulator / deploy / layout / screenshot / docs workflow
AI Agent: 判斷流程、解析結果、決定下一步
```

### Emulator 啟動：最大的坑

我一開始遇到一個很奇怪的現象。 用 AI agent 跑：

```c
android emulator start Pixel_9
```

我目測會發現 emulator 視窗會開起來，  
但很快就關掉。  
然後 AI agent 真的是會找不到這個 emulator。  
但是同樣指令，我自己在 terminal 跑卻是正常的。

根據 AI agent 自我分析，  
它判斷比較像是 agent command runner 的 process lifecycle 問題。  
也就是 emulator process 被綁在 agent 執行環境裡，  
command 結束後 emulator 也跟著被清掉。

所以它的解法是用 detached session：

```c
screen -dmS android-pixel9 android emulator start Pixel_9
```

這樣 emulator 就不會被 agent runner 的生命週期影響。

### Inspect layout：這是 agent 操作 UI 的關鍵

這次最有趣的地方，不是 deploy，而是 agent 可以理解畫面。

## Get ddsakura’s stories in your inbox

Join Medium for free to get updates from this writer.

Android CLI 可以拿到目前畫面的結構：

```c
android layout --device=emulator-5554 --pretty
```

輸出裡會包含畫面上的文字、resource id、bounds 等資訊。例如：

```c
Home
Open Activity
Open WebView Activity
Bottom navigation
```

以及每個元素的位置。

這代表 agent 不只是看 screenshot 猜按鈕在哪裡，  
而是可以讀畫面結構，  
找到目標元件，  
並知曉點擊座標。

例如要點某個按鈕，  
AI agent 可以從 layout 找到資訊後，執行：

```c
adb shell input tap 336 1543
```

進到下一個 activity 後，再用：

```c
android layout --device=emulator-5554 --pretty
```

確認畫面真的變成目標頁面。

要回上一頁則是透過：

```c
adb shell input keyevent KEYCODE_BACK
```

雖然一切看起來很美好，  
不過還是有踩到一個坑，  
如果 app 中有像 carousel 會輪播的功能模組，

```c
android layout --device=emulator-5554 --pretty
```

會回報失敗，  
然後 AI agent 就會 fallback 以截圖解析的方式進行，  
不過這樣就比較花費的 AI agent 用量應該就比較高了。

### 除了點擊，也可以做其他手勢

這次主要測了 tap 和 back，但理論上 adb input 還可以做更多：

```c
adb shell input tap x y
```

長按可以用同點位 swipe：

```c
adb shell input swipe x y x y 800
```

上下捲動：

```c
adb shell input swipe 500 1600 500 600 400
```

左右滑動：

```c
adb shell input swipe 900 1000 100 1000 400
```

所以如果搭配 `android layout` 的結構資訊，  
AI agent 可以做出滿完整的 UI 操作。

### Android Skills：不是只有 CLI

這次也順便看了 Android 官方 skills。  
目前有以下幾個 skills：

```c
android-cli
edge-to-edge
migrate-xml-views-to-jetpack-compose
navigation-3
r8-analyzer
agp-9-upgrade
play-billing-library-version-upgrade
```

我拿了 `edge-to-edge` 來測試。  
它可以發現我範例程式中 `MainActivity.kt` 有一些問題，  
並協助做出調整。

### 把踩過的坑寫進 AGENTS.md

AI agent 的記憶不應該只停在對話裡。

像 emulator 要用 detached screen 啟動、什麼情況要 fallback 到 `-no-snapshot-load` 、deploy 指令怎麼下，  
這些都可以寫進專案文件。

所以我會請 AI agent把這類操作記到 `AGENTS.md` 。

下次 AI agent 進到這個 repo，  
就不需要重新試錯一輪。

這次 spike 後，  
我對 Android CLI / skills with AI agent 的想像變得比較具體。  
而且確實可以做到不用開 Android studio 就能開發測試 Android app 了。  
眼見今年 Google IO 就要到來了，  
Android Studio … 請加油！