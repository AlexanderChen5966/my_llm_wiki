---
title: Google Android CLI + AI Agent：完整 Android 開發自動化實驗
date: 2026-06-15
tags: [dev/flutter, ai/agent]
source: https://medium.com/ddsakura-blog/用-android-cli-讓-ai-agent-跑完整-android-開發流程-c09023a292c8
type: article
status: processed
asset_value: 68
---

Google 推出的 Android CLI 提供統一介面整合過去分散的 `adb` / `gradle` / `emulator` 指令，搭配官方 Android Skills，讓 AI Agent 能操作完整 Android 開發流程。實驗結論：**不開 Android Studio 就能 build → deploy → 畫面互動 → UI 驗證**。

## T（核心論點）

三工具分工：
- **Gradle**：build app（Android CLI 尚未取代）
- **Android CLI**：emulator 管理 / deploy / 畫面結構讀取 / screenshot
- **AI Agent**：判斷流程、解析結果、決定下一步

## B（適用場景）

**適用：**
- CI/CD 無頭模式（無 GUI 環境）測試 Android App
- AI Agent 驅動的自動化 UI 測試
- Flutter/Android 開發者希望在不開 Android Studio 的情況下執行完整流程

**不適用（目前）：**
- 需要精細 UI 調整或 Layout Inspector 深度分析
- App 有動態輪播（carousel）時 `android layout` 會失敗

## R（詳細內容）

### 安裝與初始化

```bash
curl -fsSL https://dl.google.com/android/cli/latest/darwin_arm64/install.sh | bash
android init  # 自動安裝 Android Skills 到支援的 AI Agent（Claude Code / Gemini CLI / Codex）
```

### 核心指令對照表

| 任務 | 舊做法 | 新做法（Android CLI）|
|------|--------|---------------------|
| 啟動 emulator | `emulator @Pixel_9` | `android emulator start Pixel_9` |
| 部署 APK | `adb install app.apk` | `android run --device=emulator-5554 --apks=...` |
| 讀取畫面結構 | adb + UIAutomator | `android layout --device=emulator-5554 --pretty` |
| 截圖 | `adb shell screencap` | `android screen capture --output=ui.png` |

### Build 流程（仍需 Gradle）

```bash
./gradlew assembleDebug                                    # build
android run --device=emulator-5554 --apks=app/build/outputs/apk/debug/app-debug.apk  # deploy
```

### 最重要的坑：Emulator Process Lifecycle

**問題**：`android emulator start Pixel_9` 在 AI agent 環境下 emulator 視窗開了又馬上關。
**原因**：Emulator process 綁在 agent command runner 的生命週期，指令結束 emulator 就被清掉。
**解法**：使用 detached screen：

```bash
screen -dmS android-pixel9 android emulator start Pixel_9
```

### `android layout` 是 Agent UI 操作的關鍵

```bash
android layout --device=emulator-5554 --pretty
# 輸出：畫面元素文字 + resource id + bounds（座標）
```

Agent 可從 layout 資訊計算精確點擊座標，不需要靠 screenshot 猜測：

```bash
adb shell input tap 336 1543           # 點擊按鈕
adb shell input keyevent KEYCODE_BACK  # 返回上一頁
adb shell input swipe 500 1600 500 600 400  # 上下捲動
```

### 官方 Android Skills

```
android-cli               # 核心 CLI 操作
edge-to-edge              # Edge-to-edge 遷移
migrate-xml-views-to-jetpack-compose  # XML → Compose 遷移
navigation-3              # Navigation 3 升級
r8-analyzer               # R8 混淆分析
agp-9-upgrade             # AGP 9 升級
play-billing-library-version-upgrade  # 計費庫升級
```

### 把踩過的坑寫進 AGENTS.md

```
請 AI agent 記錄：
- emulator 需用 detached screen 啟動
- 何時 fallback 到 -no-snapshot-load
- deploy 的完整指令格式
```

下次 agent 進到 repo 就不需要重新試錯。

## C（關鍵行動）

- [ ] 在 Flutter/Android 專案的 AGENTS.md 加入 Android CLI 操作記錄（emulator detached screen 坑）
- [ ] `android init` 安裝 Skills 到 Claude Code
- [ ] 評估是否用 `android layout` 取代截圖分析，提高 UI 操作精準度

## 相關連結
- [[kotlin-toolchain-amper-merger]] — 建置工具鏈動態：Amper 併入 Kotlin Toolchain，Gradle 之外的統一入口
- [[flutter-google-io-2026]] — Flutter 3.44 Google IO 2026（A2UI / Android AI 整合趨勢）
- [[research/android-ai-integration-appfunctions]] — Android AppFunctions（Android MCP）+ A2UI Compose Renderer + ADK
- [[agents-md-guide]] — AGENTS.md 設計原則（記錄踩坑知識的標準格式）
- [[zerospec]] — ZeroSpec：Layer 0 AI 導覽基礎（含 AGENTS.md）
