---
title: Android 17 正式版完整適配指南（AppFunctions / Adaptive-first / 記憶體 / 隱私）
date: 2026-06-22
tags: [dev/flutter]
source: https://codelove.tw/@tony/post/3KXbQa
type: article
status: processed
asset_value: 80
---

Android 17（API 37）是 Android 轉向「智慧系統」的開端：App 能把自身能力暴露成 AppFunctions 供 AI Agent 呼叫、大螢幕適配成必選項、記憶體超限無 crash 靜默被殺。一批 target SDK 37 後必須適配的 breaking change。

## T（核心論點）

**三個最重要的改變：**
1. **AppFunctions = Android MCP**：App 把能力暴露成工具，系統 AI Agent 可發現並呼叫
2. **Adaptive-first 強制**：target SDK 37 後，大螢幕裝置不能再拒絕橫豎螢幕/resize
3. **Memory Limiter 無聲殺**：單一 App 超出 RAM 比例限制，即使前台也會被殺，沒有 crash stack

## B（適用場景）

**所有 Android App 必須了解（target SDK 37 後強制生效的適配）：**
- 特別是有大量 Bitmap 處理、前台服務、複雜快取的 App
- 有鎖定方向（`screenOrientation`）的 App
- 使用動態程式碼載入、反射修改 `static final` 的 App
- Flutter App 也適用（Dart 端的 Bitmap + Compose 適配）

## R（詳細內容）

### 新功能 1：AppFunctions（Android MCP）

App 把能力暴露成 `@AppFunction`，讓系統級 AI Agent 發現並直接呼叫，不只是開啟 App 介面：

```kotlin
class NoteFunctions(private val noteRepository: NoteRepository) {
    /**
     * Adds a new note to the app.
     * @param title The title of the note.
     * @param content The note's content.
     */
    @AppFunction(isDescribedByKDoc = true)
    suspend fun createNote(
        appFunctionContext: AppFunctionContext,
        title: String,
        content: String
    ): Note = noteRepository.createNote(title, content)
}
```

- KDoc 讓 LLM 理解如何呼叫這個工具
- Google 發布 AppFunctions agent skill：自動分析 App 關鍵流程、生成 Kotlin 程式碼、最佳化 KDoc、提供 ADB 測試指令
- Jetpack AppFunctions 函式庫目前 alpha

### 新功能 2：Adaptive-first（大螢幕強制適配）

官方統計超過 5.8 億大螢幕 Android 裝置（含摺疊螢幕、平板、ChromeOS）。

**target SDK 37 後，在 sw > 600dp 的裝置上，系統會忽略：**
- `screenOrientation`（鎖方向）
- `setRequestedOrientation()`
- `resizeableActivity=false`
- `minAspectRatio / maxAspectRatio`

遊戲按 Google Play app category 可豁免。

**新多工形態：**
- App Bubbles：長按 launcher 圖示轉成浮動氣泡
- Bubble Bar：大螢幕專用工作列
- Desktop interactive PiP：可互動的置頂小窗

**Compose-first 官宣（再次）：**
Fragment、RecyclerView、ViewPager 進入 maintenance mode，不再獲得新功能。

### 新功能 3：Continue On（跨裝置任務連續）

手機上未完成的任務，平板上直接繼續（deep link 回到上次位置）。App 需呼叫 `setHandoffEnabled(true, null)` + 實作 `onHandoffActivityDataRequested()`。

### Activity 設定變化行為改變

對 target SDK 37 以上 App，鍵盤/導覽/觸控/色彩模式等設定變化，**不再預設重啟 Activity**，改用 `onConfigurationChanged()` 通知。如果 App 依賴 Activity recreate 刷新資源，需在 manifest 加 `android:recreateOnConfigChanges`。

### 記憶體管理：Memory Limiter（Breaking Change）

### 診斷被 Memory Limiter 殺掉的方法

傳統 Crashlytics / Java crash log 看不到。需要在 App 啟動時讀取 `ApplicationExitInfo`：

```kotlin
val activityManager = getSystemService(ActivityManager::class.java)
val exitReasons = activityManager.getHistoricalProcessExitReasons(packageName, 0, 10)

for (info in exitReasons) {
    val desc = info.description ?: ""
    if (info.reason == ApplicationExitInfo.REASON_OTHER &&
        desc.contains("MemoryLimiter:AnonSwap")
    ) {
        // 上報：疑似 Android 17 memory limiter 殺掉程序
    }
}
```

### 主要適配建議

#### 1. R8 Full Mode（最重要）

```gradle
isMinifyEnabled = true
isShrinkResources = true
```

- 使用新的 `proguard-android-optimize.txt`（舊版會阻止最佳化）
- 移除 `android.enableR8.fullMode = false`
- 移除祖傳規則 `-dontoptimize`、`-dontshrink`、`-dontobfuscate`
- AGP 9 已不再支援舊 `proguard-android.txt`

**R8 Configuration Analyzer**（AGP 9.3.0-alpha05+）：
自動在 `build/outputs/mapping/release/configanalyzer.html` 產生分析報告。

#### 2. Bitmap 最佳化

- 降採樣（Glide/Coil 預設會做，但要檢查設定）
- **不要把 padding 放到圖片裡**，用 View/Composable 的 padding
- 不需透明通道用 `RGB_565`（比 `ARGB_8888` 少一半記憶體）
- 簡單幾何圖形用 Vector/ShapeDrawable，不要切 PNG

#### 3. 記憶體洩漏常見問題

- **洩漏 Context**：在 ViewModel 持有 Activity Context → 改用依賴注入/狀態流
- **洩漏 Listener**：Compose `DisposableEffect` 沒在 `onDispose` 裡清除
- **洩漏 View**：Fragment 持有 view binding，在 `onDestroyView()` 後未清空

#### 4. 主動 trim memory（onTrimMemory）

```kotlin
class App : Application(), ComponentCallbacks2 {
    override fun onTrimMemory(level: Int) {
        super.onTrimMemory(level)
        if (level >= ComponentCallbacks2.TRIM_MEMORY_UI_HIDDEN) {
            // 清 UI 相關快取：圖片、動畫、影片 buffer
        }
        if (level >= ComponentCallbacks2.TRIM_MEMORY_BACKGROUND) {
            // 更積極清理：可重建的全域 cache
        }
    }
}
```

從 Android 14 起，只剩 `TRIM_MEMORY_UI_HIDDEN` 和 `TRIM_MEMORY_BACKGROUND` 有效。

#### 5. ProfilingManager（Android 17 新功能）

- `TRIGGER_TYPE_OOM`：OOM crash 發生時自動收集 heap dump
- `TRIGGER_TYPE_ANOMALY`：記憶體異常時收集（在被殺前）

#### 6. adb 工具

```bash
adb shell am memory-limiter status                  # 查看 memory limiter 狀態
adb shell am memory-limiter manual 12345 300        # 設定 PID 12345 上限 300MB
```

### 隱私安全變化（target SDK 37）

- **系統級聯絡人選擇器**：不需寬泛 `READ_CONTACTS` 權限，只取使用者選擇欄位
- **系統渲染 Location Button**：只給當前 session 精確位置，不需長期授權
- **EyeDropper API**：`ACTION_OPEN_EYE_DROPPER` 取色，不需截圖/MediaProjection 權限
- **本機網路存取**：存取 LAN 裝置需申請 `ACCESS_LOCAL_NETWORK` 或使用系統 picker（影響 IoT/NAS/印表機 App）
- **SMS OTP 延遲 3 小時**：target SDK 37+ 的 App，需遷移到 SMS Retriever 或 SMS User Consent APIs

### 其他 Breaking Changes

| 項目 | 影響 |
|------|------|
| `static final` truly final | 反射修改會拋 `IllegalAccessException`；JNI `SetStatic<Type>Field` 直接 crash |
| 動態 native 載入 | `System.load()` 的 native 檔案必須 read-only，否則 `UnsatisfiedLinkError`（影響熱更新/外掛框架）|
| Lock-Free MessageQueue | 反射存取 `MessageQueue` 私有實作的 App 會異常 |
| Certificate Transparency | Android 17 預設啟用（Android 16 需 opt-in）|
| 背景音訊加固 | 音訊播放、audio focus 請求、音量修改 API 新限制 |
| NPU 存取聲明 | 需在 manifest 宣告 `FEATURE_NEURAL_PROCESSING_UNIT` |

**CameraX 注意：**需升級到 1.5.2 或 1.6.0+，否則因 Android 17 新增 dynamic range mode 導致崩潰。

## C（關鍵行動）

- [ ] 啟用 R8 full mode（`isMinifyEnabled = true` + 新 proguard 設定），移除祖傳 `-dontoptimize`
- [ ] 在 App 啟動時加入 `ApplicationExitInfo` 診斷 + 上報（REASON_OTHER + MemoryLimiter:AnonSwap）
- [ ] 實作 `onTrimMemory` 在 App 不可見時主動釋放大物件
- [ ] 跑 R8 Configuration Analyzer 找阻止最佳化的 keep rule
- [ ] 評估 AppFunctions 實作，把 App 核心能力暴露給 AI Agent
- [ ] 移除 `screenOrientation` 強制鎖定，正確實作多視窗/大螢幕適配
- [ ] 確認 `System.load()` 載入的 native 檔案是 read-only
- [ ] 遷移 SMS OTP 讀取到 SMS Retriever API
- [ ] CameraX 升級到 1.5.2+

## 相關連結

- [[flutter-google-io-2026]] — Flutter 3.44（同屆 Google I/O，含 AGP / Dart 變更）
- [[android-adk-agent-tool-calling]] — Android ADK v0.2.0（AppFunctions 相關 AI 整合）
- [[android-cli-ai-agent-workflow]] — Android CLI + AI Agent（Android 開發自動化）
- [[sanlong/bugs]] — Sanlong App Bug 記錄（記憶體問題可參考此指南）
