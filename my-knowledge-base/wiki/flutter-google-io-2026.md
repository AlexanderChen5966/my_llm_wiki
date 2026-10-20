---
title: Flutter 3.44 / Dart 3.12：Google IO 2026 完整重點
date: 2026-05-22
tags: [dev/flutter]
source: https://hackmd.io/@FatJohn/HkPoZi31Ml
type: reference
status: processed
asset_value: 80
---

Flutter 3.44 / Dart 3.12 在 Google I/O 2026 發布，最重要的架構變革是 Material/Cupertino 從核心 SDK 解耦為獨立套件，以及全面的 Agentic 開發工具整合（Agentic Hot Reload、Dart & Flutter Agent Skills、Genkit Dart）。生態擴大：Toyota RAV4 車機採用、Canonical 成為 Desktop Lead Maintainer、Cloud Functions for Firebase 正式支援 Dart。

## T（核心論點）

Flutter 正在做兩件同時發生的大事：**向下解耦**（Material/Cupertino 拆出核心，讓框架更輕、更可替換）、**向上整合**（AI/Agent 工作流深度支援，Agentic Hot Reload、Agent Skills、GenUI）。前者是架構成熟的標誌，後者是配合 AI 開發時代的主動轉型。

## B（適用場景）

**適用：** 評估 Flutter 升級路徑（3.44 的 Material 解耦是破壞性變更）；規劃 Agentic 開發工具整合；了解 Firebase Dart 後端生態；iOS SPM 遷移時機

**不適用 / 注意：**
- Material/Cupertino 解耦尚未完全完成，暫緩 Material 3 Expressive 和 iOS 26 Liquid Glass 實作，升級前確認現有 Material Widget 相容性
- Genkit Dart 仍為 Preview，不建議生產環境使用
- Dart Primary Constructors 仍為實驗性功能

## R（重點分類）

### 架構大變革：Material/Cupertino 解耦

**最重要的變更，影響所有 Flutter 專案：**

- Material 拆為獨立套件 **`material_ui`**，Cupertino 拆為 **`cupertino_ui`**
- 原有 `import 'package:flutter/material.dart'` 將在下一版開始 deprecated，提供 `dart fix` migration
- 目的：設計語言變更不再綁定 Flutter 核心版本；減少自訂 UI 與框架的衝突
- **副作用**：暫緩 Material 3 Expressive + iOS 26 Liquid Glass 支援，待新架構落地後再跟進
- **Strengthening the Core**：style-neutral 邏輯抽離為 design-agnostic widget primitives

### AI / Agent 開發工具整合

| 功能 | 說明 |
|------|------|
| **Agentic Hot Reload** | Agent（Claude Code、Antigravity、Gemini CLI）改動程式碼後自動觸發 hot reload，無需手動操作 |
| **Dart & Flutter Agent Skills** | 官方 Skills 集合，遵循 Flutter 最佳實踐，採 Progressive Disclosure 節省 Token |
| **Dart & Flutter MCP Server** | 為 Agent 提供 Dart/Flutter 專案的深度 context 與工具（持續演進） |
| **Genkit Dart（Preview）** | 開源 AI/Agentic app 框架，model-agnostic（Google/Anthropic/OpenAI），含 structured output、tool calling、multi-turn、observability |
| **flutter_gemma → LiteRT-LM** | On-device 模型推論，抽象硬體差異，支援 GPU/NPU 加速，跨 6 個 stable 平台 |

### Firebase × Dart：全端 Dart 開發

- **Cloud Functions for Firebase 正式支援 Dart**：純 Dart 撰寫 HTTP + callable functions，AOT 編譯冷啟動最低 **10 ms**
- **Dart Admin SDK**：從特權伺服器端存取 Firebase 服務
- **Firebase AI Logic Server Prompt Templates**：prompt 外部化，不再 hard-coded 在 app 內
- **Firebase Agent Skills for Flutter**：為 AI agent 提供建構全端 Flutter + Firebase 的逐步指引

### Dart 語言新特性

| 特性 | 狀態 |
|------|------|
| Primary Constructors | 實驗性 |
| Named Constructor Parameters 自動初始化 | 實驗性 |
| Dot Shorthands（省略 class/enum 名稱） | 已落地 |
| Dart Build Hooks（FFI 套件直接在 Dart 套件內編譯原生碼） | 已落地 |
| Custom Analyzer Plugins（自訂診斷/quick fix） | 已落地 |
| `pub cache gc`（清理未使用套件） | 已落地 |
| `dart format` 近乎即時、`dart analyze` 快約 50% | AOT snapshots 改進 |

### 平台重點

**Android：**
- Android Hybrid Composition++：原生處理圖層合成，大幅改善混合渲染效能（需 Vulkan + API 34+，opt-in，未來將為預設）
- Android 17 整合：Local Network Protections、安全 Dynamic Code Loading

**iOS：**
- iOS Swift Package Manager 設為預設，CocoaPods 支援走向終結
- `swift-package build` 指令：Flutter module 當成原生 Swift Package 使用
- iOS App Extensions 支援 Flutter Views（如 Share Extension）
- Flutter View 內容自適應大小

**Web：**
- WebAssembly 渲染全面重整：LazyPath + Frame Arenas 消除 jank 與 crash
- Path benchmark 提升 25-40%

**Desktop：**
- 多視窗 API 成熟：跨平台 Tooltips + 獨立 Dialog 視窗，支援 macOS/Windows/Linux
- Widget Previews：沙盒環境針對螢幕尺寸/主題/字型縮放矩陣預覽 widget

### Generative UI（GenUI）

- **A2UI Protocol**：Google 主導開源協定，定義 agent 與 client 如何協作組合 UI + 狀態
- **Flutter GenUI SDK**：2026 年至今下載成長超過 500%
- Google DeepMind Visual Layout Experiment 驗證可行性，已整合進 Gemini app

### 生態合作亮點

| 合作方 | 重點 |
|--------|------|
| Toyota | 2026 RAV4 車機採用 Flutter，2027 Highlander BEV 接續；從設計工具到車機 head unit 可一日內完成 |
| Canonical | 宣布成為 Flutter Desktop Lead Maintainer & Strategic Steward，主導 Linux/Windows/macOS embedders |
| LG | 投資 webOS embedder，webOS SDK 即將推出，支援 Firebase/Video/Gamepad |

## C（關鍵行動）

- [ ] **評估 Material 解耦影響**：現有專案用了哪些 Material Widget？遷移到 `material_ui` 的時機規劃
- [ ] **Agentic Hot Reload**：確認 Claude Code 是否已支援此功能，升級後可大幅加速 UI 開發循環
- [ ] **Dart & Flutter Agent Skills**：下載官方 Skills，整合進 Claude Code 工作流提升 Token 效率
- [ ] **Firebase Dart Functions**：評估是否將後端函式從 Node.js 遷移至 Dart（冷啟動 10ms 優勢）
- [ ] **iOS SPM 遷移**：CocoaPods 走向終結，規劃遷移時間表

## 相關連結

- [[sanlong/architecture]] — Sanlong App 架構（評估 Agentic Hot Reload 和 Agent Skills 整合機會）
- [[b2b-manager/architecture]] — B2B Manager 架構（同上）
- [[sanlong/dev-workflow]] — Flavor 建置與 AI 工具分工
