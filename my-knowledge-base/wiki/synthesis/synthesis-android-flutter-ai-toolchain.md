---
title: Android/Flutter AI Agent 開發全景：從設計到測試的端到端工具鏈
date: 2026-06-17
source_count: 12
synthesis_snapshot_date: 2026-07-24
tags: [dev/flutter, ai/agent, ai/claude]
source: synthesis
type: note
status: processed
asset_value: 85
synthesized_from:
  - flutter-google-io-2026 (80)
  - codegraph-ai-coding-agent (78)
  - flutter-genkit-agent-engineering (74)
  - android-cli-ai-agent-workflow (68)
  - android-adk-agent-tool-calling (65)
  - claude-design-tool (62)
  - android-17-memory-management (62)
  - android-repository-suspend-vs-launch (60)
  - agent-device-cross-platform-testing (60)
  - flutter-patrol-e2e-testing (58)
  - web-to-app-android-apk-builder (52)
  - codegraph-token-optimization (48)
---

## C（核心概念）

2026 年 Android/Flutter 開發正在發生一次結構性轉變：**AI 不再只是「寫程式的助手」，而是整個開發流程的參與者**。Google I/O 2026 官方把這條線命名為 Agentic 開發工具鏈——從設計（Claude Design）→ 導航（CodeGraph）→ 建構部署（Android CLI）→ 測試驗證（Patrol + agent-device）→ App 本身成為 Agent Runtime（Android ADK）。

**兩種根本不同的 AI 角色：**

| 角色 | 代表工具 | 描述 |
|------|---------|------|
| **外部 AI 開發 App** | CodeGraph / Android CLI / Claude Design / agent-device | AI 做為開發工具，協助完成設計、編碼、測試 |
| **內部 AI 成為 App 的一部分** | Android ADK / Genkit Dart / LiteRT-LM | App 本身是 Agent Runtime，AI 在 App 內推理並呼叫 App 的工具 |

這兩條路不互斥，但混淆它們會導致架構設計錯誤。

---

## A（端到端工具鏈應用）

### Layer 1：設計（Claude Design）

```
模糊需求 → Claude 對話釐清 → 結構化 prompt + 截圖參考
    → Claude Design 生成設計稿 → 截圖貼給 Claude Code 實作
```

**關鍵原則**：用 Claude 對話省 Claude Design 用量——Design 只用於最終生成，討論和修改在一般對話中完成。

### Layer 2：Codebase 導航（CodeGraph）

大型 repo（1,000+ 檔案）必備。預建語義地圖讓 AI 從「掃描找路」改為「查表定位」：
- **官方跨 7 專案 benchmark（7 語言、各 4 次 median）：平均 25% cheaper / 57% fewer tokens / 23% faster / 62% fewer tool calls**（[[codegraph-token-optimization]]）
  - ⚠️ 流傳的「省 86% token」是單一專案的作者實測值，非跨專案平均——引用時別混用
  - 規模效應明顯：VS Code（~10k 檔）省 70% token / 80% tool calls；Gin（~110 檔）只省 35% / 47%——**小專案 overhead 可能不划算**
- AST-based（理解關係），非 embedding-based（只知語意相近）
- 核心機制：`codegraph_context` 映射區域 → `codegraph_explore` 取原始碼，架構問題常可 **zero file reads** 回答
- MCP 介面開放給所有 Agent（支援 20+ 語言、8 個 Agent、14 個 Web 框架路由識別），不限 Cursor

### Layer 3：建構與部署（Android CLI + Gradle）

```bash
# 三工具分工
./gradlew assembleDebug                          # Gradle 負責 build
android run --device=emulator-5554 --apks=...   # Android CLI 負責 deploy
android layout --device=emulator-5554 --pretty  # 讀畫面結構給 Agent 操作
```

**最重要的坑**：Emulator 需用 `screen -dmS` 在 detached process 啟動，否則 Agent 環境下 emulator 視窗會在指令結束時被清掉。把這條寫進 AGENTS.md，下次不需重新踩。

### Layer 4：測試驗證

**兩層測試工具，各司其職：**

| 層次 | 工具 | 用途 |
|------|------|------|
| Flutter Widget 層 | Flutter Patrol + patrol_mcp | 系統權限對話框 / 推播通知 / 跨 App 操作，MCP 讓 AI 直接驅動 |
| 原生裝置層（含鴻蒙）| agent-device | accessibility snapshot → @ref 語義引用，不靠截圖座標 |

**核心洞見**：截圖辨識讓 AI 猜座標，accessibility snapshot 讓 AI 讀結構。後者 token 少、不受捲動失效，適合作為 Agent 驅動測試的標準通道。

### Layer 5：App 作為 Agent Runtime（Android ADK）

```kotlin
@Tool(name = "search_products", description = "...")
fun searchProducts(@Param("category") category: String): String
// ADK 透過 KSP 自動生成 Tool Wrapper，體驗接近 Retrofit
```

**Model Provider 可替換**：`HttpRemoteModel` 可接 LM Studio（Emulator 用 10.0.2.2）、Gemini API、任何 OpenAI-compatible endpoint。

**適用場景**：本地資料 + LLM 推理組合（自然語言查詢本地購物清單、文件 RAG），而非所有 App 都需要。

**企業級深度：GenKit-Dart 的完整工程實作**（[[flutter-genkit-agent-engineering]]）

ADK 解決「App 能呼叫工具」，但企業 Agent 還要解決「怎麼把 LLM 關進業務邊界」。GenKit-Dart 是 Google 官方唯一 Flutter Agent SDK，能力介於輕量與 LangGraph 之間，以 **Flow**（可型別校驗/除錯/追蹤/部署的函式）為核心：

| 關鍵設計 | 內容 |
|---|---|
| **模型即路由器** | 本地只提供能力快照、schema、權限與隱私邊界；**所有語意決策交給模型，不做關鍵字硬編碼分支** |
| **TurnPlan 三集合** | `required`/`forbidden`/`candidate` 表達工具邊界——模型**看不到**被禁工具，不需在自然語言層猜權限 |
| **Soft plan + hard invariant** | Plan 只是 prompt hint（模型可偏離），真正的「必須呼叫」靠 `forceToolUse` + 後置校驗保證 |
| **信任 × 副作用矩陣** | `sideEffectLevel` × `trustLevel` 決定自動執行／需審批／拒絕；只有 readOnly+parallelSafe 可並行 |
| **不自寫 while 迴圈** | 工具循環交給 GenKit `generateStream(maxTurns:)`，避免重複實作 parsing／cancellation／streaming |
| **Auditor 只校驗不生成** | 模型 B 抽 factual claim 定位 evidence，非 grounded 就產結構化 IssueCode 觸發重生成（最多 3 次）|
| **可追溯三件套** | 21-span Trace + Replay Harness（離線重放）+ EvalDataset（回歸 pass rate 趨勢）|

> 為何企業 Agent 需要 RAG 而 Claude Code 這類程式碼 Agent 不需要：**程式碼庫本身結構化、可搜尋、可執行；企業知識則分散在私有系統，必須有檢索增強層**。

### 跨層整合：Flutter 3.44 的架構準備

Google I/O 2026 的兩個方向為上述工具鏈做底層準備：
1. **Material/Cupertino 解耦**（`material_ui` 獨立套件）：AI 生成 UI 時不再被設計語言版本鎖住
2. **Agentic Hot Reload**：Agent 改完程式碼自動觸發 reload，縮短編碼→驗證迴圈
3. **A2UI Protocol**：Agent 與 Client 協作動態組合 UI（GenUI 的協定層）

---

## V（核心洞見與反直覺之處）

**1. AI coding agent 的瓶頸不在模型，在 context 基礎設施**  
CodeGraph 的 benchmark 說明：同樣的模型，有語義地圖 vs 沒有，tool calls 差 70%。模型更換容易，codebase 知識圖譜建不建是架構決策。

**2. 測試自動化的 AI 最佳切入點是「原生結構」而非「視覺外觀」**  
agent-device 用 accessibility tree、Patrol 用 gRPC 原生通訊——兩者都在說同一件事：讓 AI 讀 UI 結構，而非看圖猜位置。視覺辨識是備選，不是首選。

**3. suspend fun vs launch 不是語法選擇，是架構語義**  
Repository 暴露 `suspend fun` = 定義能力，讓呼叫方決定生命週期。`launch` 在 Repository 內 = 把時序複雜度藏起來，製造隱性耦合。這個判斷在 AI 生成 Repository 程式碼的 Code Review 中尤其重要——AI 很容易寫出 `launch` 版本。

**4. Android 17 Memory Limiter 沒有 crash stack**  
傳統 Crashlytics 看不到這類崩潰。需要 `ApplicationExitInfo` + `MemoryLimiter:AnonSwap` 字串偵測。R8 full mode 是第一道防線，也是最高 ROI 的適配。

**5. Claude Design 的價值不是「取代設計師」，是「讓開發者能討論設計」**  
對獨立開發者，最大的障礙不是設計能力，是沒有工具把腦袋裡的畫面外化成可討論的稿子。Claude Design 解決的是這個問題。

**6. context 基礎設施的效益與 repo 規模強相關，不是普遍真理**  
CodeGraph 官方 benchmark 顯示：VS Code（~10k 檔）省 70% token，但 Gin（~110 檔）只省 35%。**小專案裝索引工具，overhead 可能吃掉收益**——「AI 開發必裝」這類說法要看規模再決定。

**7. 企業 Agent 的工程重點與程式碼 Agent 相反：不是讓模型更自由，是把邊界寫進型別**  
[[flutter-genkit-agent-engineering]] 的三集合工具邊界、信任×副作用矩陣、Auditor 只校驗不生成，全都在做同一件事：**把權限與可信度變成程式碼結構，而非寄望模型在自然語言層自律**。而語意決策（選不選工具、query 怎麼改寫）反而完全交給模型——**該硬的地方硬、該軟的地方軟**，這個分界比「多信任模型」本身更關鍵。

---

## E（演化方向）

**已在進行的三條路：**

1. **on-device AI 推論**：LiteRT-LM（抽象硬體差異，GPU/NPU 加速）讓 App 在裝置本地跑模型，不依賴雲端 API。Android ADK 是 App Runtime 端的配合層。

2. **GenUI（A2UI Protocol）**：Agent 不再只生成靜態介面，而是動態組合 UI + 狀態。Flutter GenUI SDK 2026 年下載成長 500%+，這條路正在加速。

3. **全端 Dart（Cloud Functions for Firebase）**：後端不再需要 Node.js，Dart AOT 冷啟動 10ms，Android/iOS/後端共用同一語言。Genkit Dart（Preview）提供 model-agnostic AI 框架。

**下一個可能的演化：**
- AGENTS.md 的標準化：目前 Android CLI Skills 用 AGENTS.md 記錄踩坑知識，未來這份文件可能與 agent-device 的 `.ad` 回放腳本合流，形成「Android 專案的機器可讀操作規範」

---

## 相關精粹條目

- [[synthesis-agentic-engineering]] — Agentic Engineering 方法論（本地 Android 工具鏈的上層框架）
- [[synthesis-prompt-engineering-evolution]] — Context 工程（CodeGraph 是 Android Context 基礎設施的具體實作）
- [[synthesis-spec-driven-workflow]] — Spec-driven workflow（Android Agent 開發的規格設計層）

## 來源條目

- [[flutter-google-io-2026]] — Flutter 3.44 架構解耦 + Agentic 工具整合全景
- [[codegraph-ai-coding-agent]] — CodeGraph：AI coding agent 的語義導航層
- [[android-cli-ai-agent-workflow]] — Android CLI + AI Agent：完整開發流程自動化
- [[android-adk-agent-tool-calling]] — Android ADK：App 成為 Agent Runtime
- [[claude-design-tool]] — Claude Design：文字描述→設計稿
- [[android-17-memory-management]] — Android 17 Memory Limiter 適配指南
- [[android-repository-suspend-vs-launch]] — Repository suspend fun vs launch 架構語義
- [[agent-device-cross-platform-testing]] — agent-device：AI Agent 跨平台裝置自動化
- [[flutter-patrol-e2e-testing]] — Flutter Patrol：原生層 E2E 測試 + patrol_mcp
- [[web-to-app-android-apk-builder]] — Web-to-App：手機端 APK 封裝器
- [[codegraph-token-optimization]] — CodeGraph 官方跨 7 專案 benchmark 與規模效應（修正「省 86%」的單一樣本說法）（AV 48）
- [[flutter-genkit-agent-engineering]] — GenKit-Dart 企業級 Agent 完整模組拆解：三集合工具邊界、11 態 FSM、Auditor 邊界（AV 74）

## 相關連結

- [[kotlin-toolchain-amper-merger]] — Amper 正式併入 Kotlin Toolchain：Gradle 統一入口的野心，影響 Android 側建置鏈選型