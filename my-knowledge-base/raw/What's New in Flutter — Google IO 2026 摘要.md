---
title: "What's New in Flutter — Google I/O 2026 摘要"
source: "https://hackmd.io/@FatJohn/HkPoZi31Ml?fbclid=IwY2xjawR8ElZleHRuA2FlbQIxMQBzcnRjBmFwcF9pZA80MDk5NjI2MjMwODU2MDkAAR5WaiXy7Vq5uWDRJA3gf2gDd5HwA02TOvCACoZGL-6Pn-xdl3sJQRDjFW5hWg_aem_EPcK5VpSTH7uxBGGXmxlAg"
author:
published:
created: 2026-05-22
description: "來源：YouTube — What's new in Flutter主講：Kate（Flutter Framework Team Engineering Manager）、Khanh（Dart & Flutter DevRel Engineer）來賓：Li-Te Cheng（Google DeepMind）版本發布：Flutter 3.44 與 Dart 3.12"
tags:
  - "clippings"
status: processed
---
> 來源： [YouTube — What's new in Flutter](https://www.youtube.com/watch?v=3TfGKugPlpE)  
> 主講：Kate（Flutter Framework Team Engineering Manager）、Khanh（Dart & Flutter DevRel Engineer）  
> 來賓：Li-Te Cheng（Google DeepMind）  
> 版本發布： **Flutter 3.44** 與 **Dart 3.12**

---

## 1\. 生態系概況

- Flutter 貢獻者超過 **1,700 人** ，過去一年合併變更超過 **5,800 次**
- pub.dev 過去 30 天套件下載量超過 **13 億次**
- Flutter 為 App Store 與 Google Play 上第 2 受歡迎的行動 SDK
- 每月活躍 Flutter 開發者超過 **150 萬人**

---

## 2\. 新發表的技術

### Dart 語言與工具

- **Primary Constructors（實驗性）** ：可直接在 class header 宣告 instance 變數
- **Named Constructor Parameters 自動初始化** ：Dart 會自動為私有 instance fields 撰寫顯式 initializer list
- **Dot Shorthands（已落地）** ：可省略多餘的 class 或 enum 名稱
- **Dart Build Hooks（已落地）** ：FFI 套件可直接於 Dart 套件內編譯原生程式碼或下載原生資產，免維護 CocoaPods／CMake／Gradle
- **Custom Analyzer Plugins** ：可擴充 Analysis Server，自訂診斷、quick fix 與 assists
- **`pub cache gc` 指令** ：自動清理 pub cache 中未使用的套件

### Firebase × Dart（重點）

- **Cloud Functions for Firebase 正式支援 Dart** ：可純 Dart 撰寫 HTTP 與 callable functions；AOT 編譯使得冷啟動最低約 **10 ms**
- **Dart Admin SDK** ：從特權伺服器端環境存取 Firebase 服務
- **Firebase AI Logic Server Prompt Templates** ：prompt 不再需要硬寫在 app 內
- **Firebase Agent Skills for Flutter** ：為 AI agent 提供建構全端 Flutter + Firebase app 的逐步指引

### AI／Agent 開發工作流

- **Agentic Hot Reload** ：Hot reload 將自動與所有 coding agent（Claude Code、Antigravity、Gemini CLI 等）整合，agent 改動程式碼後即時重載 UI
- **Dart and Flutter Agent Skills** ：官方提供的 skills 集合，遵循 Flutter 最佳實踐，採 progressive disclosure 節省 token
- **Dart and Flutter MCP Server** （去年夏天推出後持續演進）：為 agent 提供 Dart/Flutter 專案的深度 context 與工具

### AI 應用框架

- **Genkit Dart（Preview 發布）** ：開源全端 AI／Agentic app 框架
	- Model-agnostic API，支援 **Google、Anthropic、OpenAI**
		- 內建 type-safe structured output、tool calling、multi-turn conversations、observability
- **flutter\_gemma 即將支援 LiteRT-LM** ：Google 開源高效能推論框架；抽象掉硬體差異，跨平台執行 on-device 模型並支援 GPU／NPU 加速
	- 支援 Flutter 全部 6 個 stable 平台（Android、iOS、Web、Windows、Linux、macOS）

### Generative UI（GenUI）

- **A2UI Protocol** ：Google 主導的開源協定，定義 agent 與 client 如何協作組合 UI 與狀態
- **Flutter GenUI SDK** （去年底推出，2026 年至今下載成長超過 **500%** ）
- 由 Google DeepMind 的 **Visual Layout Experiment** 驗證可行性，現已整合進 Gemini app

### 平台

- **Android Hybrid Composition++** ：讓 Android OS 原生處理圖層合成，大幅改善與原生 Android view 混合渲染的效能（需 Vulkan、Android API 34+；目前 opt-in，未來將為預設）
- **Android 17 整合** ：Local Network Protections、安全的 Dynamic Code Loading
- **iOS Swift Package Manager 為預設** ：CocoaPods 支援開始走向終結
- **`swift-package build` 指令** ：可將 Flutter module 當成原生 Swift Package 使用
- **iOS App Extensions 支援 Flutter Views** （例如 Share Extension）
- **Flutter View 內容自適應大小** ：嵌入原生 app 的 Flutter view 可依內容自動調整尺寸（行動雙平台均可 opt-in）
- **桌面多視窗 API 進入新階段** ：跨平台支援 Tooltips 與獨立 Dialog 視窗，可建立原生感的多視窗 macOS／Windows／Linux 應用
- **Widget Previews** ：可在沙盒環境針對螢幕尺寸、主題、字型縮放矩陣預覽 widget；本版完整支援 Flutter Inspector

### 框架架構（重大變革）

- **Material／Cupertino Decoupling** ：將 Material 與 Cupertino 從 Flutter 核心 SDK 拆分成獨立套件 **`material_ui`** 與 **`cupertino_ui`**
	- 解耦目的：避免 design 變更綁定核心版本、加速設計語言迭代、減少自訂 UI 與框架對抗
		- 原 library 將於下一版開始 deprecated，並提供 `dart fix` migration
		- 因此 **策略性暫緩** 了 Material 3 Expressive 與 iOS 26 Liquid Glass 的實作，待新架構落地後再支援
- **Strengthening the Core** ：將 style-neutral 邏輯抽離成 design-agnostic widget primitives
- **基礎類別搬出核心** ：例如 `listenable` 等將獨立成純 Dart 套件
- **Pluggable Platforms 願景** ：提升 Flutter Engine 與 CLI 的可擴充性，讓更多平台可接入

---

## 3\. 改善的技術

### 開發者體驗（DX）

- Dart CLI 改用 AOT snapshots： **`dart format` 近乎即時、 `dart analyze` 快約 50%**
- Widget Previews 全面整合 Flutter Inspector

### Impeller 引擎

- 改善 Vulkan 支援，記憶體管理最佳化
- Shader 開發可 **直接以名稱存取 uniforms** ，編譯回饋更清楚
- 採用 **SDF（Signed Distance Functions）** 提高複雜形狀的反鋸齒品質

### Web

- WebAssembly 渲染路徑全面重整，新增 **LazyPath** 物件與 **Frame Arenas** ，消除 jank 與 crash
- Path benchmark 提升 **25–40%**
- 新 dev proxy、簡化的設定檔，移除繁瑣 CLI 參數，build 更快更穩定
- 系統偏好整合、無障礙、文字編輯體驗更貼近原生

### Mobile／Desktop

- 主動整合 Android 17 安全與可用性新功能
- iOS 開發環境改採 SPM，更可靠
- Desktop 視窗 API 由實驗階段進入更成熟的多視窗能力

---

## 4\. 技術合作廠商

### 重點夥伴

| 廠商 | 合作內容 |
| --- | --- |
| **Google DeepMind** | Visual Layout Experiment（GenUI 先驅）、整合進 Gemini app；協助形塑 A2UI 與 Flutter GenUI SDK |
| **Toyota** | **2026 RAV4** 車載資訊娛樂系統採用 Flutter； **2027 Highlander BEV** 接續導入。從設計工具到車機 head unit 可一日內完成從反饋到上線 |
| **Canonical（Ubuntu 發行商）** | 宣布 **擴大合作** ，成為 **Flutter Desktop 的 Lead Maintainer 與 Strategic Steward** ，主導 Linux／Windows／macOS embedders 與桌面 roadmap |
| **LG** | 投資 webOS embedder， **webOS SDK** 即將推出，支援 Firebase、Video player、Gamepad 等 plugin，並整合 Hot Reload 與 Riverpod |

### AI／Agent 生態合作

| 廠商／產品 | 用途 |
| --- | --- |
| **Anthropic（Claude Code）** | Agentic Hot Reload 支援的 agent 之一；Genkit Dart 支援的模型供應商 |
| **OpenAI** | Genkit Dart 支援的模型供應商 |
| **Google Gemini CLI、Antigravity** | Agentic Hot Reload 整合的 coding agent |
| **Firebase** | Cloud Functions、Admin SDK、AI Logic、Agent Skills for Flutter |

### 社群亮點（Gemma 3n Impact Challenge）

- **Tomasso — Gemma Vision** （協助視障者感知世界，第一名）
- **Guido — Vite Vere** （協助認知障礙者獨立完成日常任務，第二名）
- 兩者皆以 Flutter + `flutter_gemma` plugin 實作

---

## 5\. 未來方向（Roadmap 訊號）

- **更開放、可永續的 Flutter 治理模式** ：以 Canonical 為首擴大 partner ecosystem
- **Pluggable Platforms** ：讓更多產業／環境可接入 Flutter
- **核心瘦身** ：更多 framework 工具與基礎類別將拆成獨立 Dart 套件
- 聯絡合作： `partners@flutter.dev`

---

> 整理日期：2026-05-21