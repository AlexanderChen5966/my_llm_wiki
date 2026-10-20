---
title: agent-device：AI Agent 專用跨平台裝置自動化框架（含鴻蒙）
date: 2026-06-17
tags: [ai/agent, dev/flutter]
source: https://codelove.tw/@tony/post/xNQ16q
source_2: https://www.aiposthub.com/mobile-next-mcp-ios-android-automation/（〈Mobile Next MCP 是什麼？用 Claude、Codex 自動操作 iOS 與 Android 的安裝、功能與風險〉，2026-09-29，對應文末「同類工具：Mobile Next Mobile MCP」補充章節）
type: article
status: processed
asset_value: 60
---

## T（核心論點）

傳統 mobile 自動化工具（Appium/Detox/Maestro）是為人寫腳本設計的；`agent-device` 是為 AI Agent 優化的標準化裝置通道。核心差異：不依賴截圖座標辨識，而是透過**無障礙存取樹（accessibility snapshot）**輸出結構化 UI 描述，讓 AI 用 `@e1` / `@e2` 語義引用操作 UI，最大幅降低 token 用量和誤操作率。MIT 授權，callstack 開源。

## B（適用場景）

**適用：**
- Agent 寫完程式後，需要讓 Agent 自己安裝→開啟→操作→截圖→抓日誌→根據結果修復
- 需要跨平台（iOS/Android/HarmonyOS）的 AI 測試流程統一化
- CI 上的 Agent 驅動 e2e 測試回放

**不適用：**
- 人工維護的穩定 e2e 回歸測試（這是 Appium/Detox/Maestro 的強項）
- 複雜 UI 的高穩定性要求場景（目前仍有不穩定問題）
- 截圖座標為主的舊有測試腳本遷移

## R（完整方案）

### 核心差異：accessibility snapshot vs 截圖

```bash
agent-device snapshot -i
# 輸出：
# @e1 [heading] "Settings"
# @e2 [button] "Sign In"
# @e3 [text-field] "Email"
```

- `-i`（interactive only）只回傳可互動元素，進一步減少 token
- `@e1`~`@eN` 是語義引用，不是像素座標（捲動後座標失效，ref 不會）
- AI 用 `agent-device press @e2` 操作按鈕，不需要讀座標

### 支援能力清單

| 功能 | 說明 |
|------|------|
| `snapshot -i` | UI 無障礙樹（可互動元素 + @ref 語義引用）|
| `screenshot` | 截圖（用於 debug 和回放存證）|
| `record` | 錄製操作流程 → `.ad` 腳本 |
| `logs` | App 日誌流 |
| `network` | HTTP 請求封包擷取 |
| `perf` | CPU/記憶體效能採樣 |
| crash context | 當機時的現場資訊 |

### 工作流程（Agent 驅動測試）

```
Agent 生成/修改程式碼
    → agent-device open <app>
    → snapshot -i → AI 讀取 @refs
    → press @e3 / type "hello"
    → diff snapshot → 驗證 UI 狀態變化
    → logs / network → 採集證據
    → AI 根據結果修復程式碼 → 重複
    → 錄製成 .ad 腳本 → CI 回放
```

### 平台支援與架構

| 平台 | 驅動方式 | 限制 |
|------|----------|------|
| **iOS Simulator/真機** | XCTest Runner（動態建構 `.xctestrun`）+ TCP 通訊 | — |
| **Android Emulator/真機** | ADB + android-snapshot-helper APK | — |
| **macOS App** | 與 iOS 共用後端 | — |
| **Linux** | AT-SPI2 + Python3 橋接 | — |
| **HarmonyOS** | HDC + uitest dumpLayout | pinch/rotate、logs/network/perf 不支援 |
| tvOS/Android TV | 同 iOS/Android | — |

### 架構設計亮點

**Session + Daemon 模型：**
- `open` 建立裝置 session，後續命令在 session 上下文執行（鎖住裝置資源）
- 背景常駐 daemon，CLI 透過 IPC 通訊

**iOS 故障復原狀態機：**
每個命令打上唯一 `commandId`；TCP 中斷後送 `status` 查詢 lifecycle 狀態（`completed/failed/accepted/started`），決定是重試還是 invalidate session，避免 double-tap 問題。

**Android 雙引擎快照：**
原生 `uiautomator dump` 有嚴重缺陷（超時/節點數限制/不支援多視窗），agent-device 開發了 `android-snapshot-helper` APK 作為替代，常駐 session 避免每次 cold start，另外透過 `dumpsys activity top` 推斷捲動列表是否有隱藏內容（標註「下方還有更多」）。

**Interactor 統一介面：**
`src/core/capabilities.ts` 定義每條命令支援哪些平台，AI 呼叫不支援的命令會在 admission 階段直接報錯，不會無意義地送到裝置。

### 與 Flutter Patrol 的關係

| | Flutter Patrol | agent-device |
|---|---|---|
| 定位 | Flutter 層 AI 測試框架 | **原生層**跨平台 AI 自動化通道 |
| 操作方式 | Flutter Widget tree | 原生 accessibility tree |
| 適合 | Flutter 專案 | iOS/Android/鴻蒙 原生 App |
| 互補 | Flutter App 用 Patrol | 原生 App 用 agent-device |

## C（關鍵行動）

- [ ] 在 Flutter 以外的原生 App 測試場景，評估 agent-device 取代截圖辨識方式
- [ ] 使用 `record` 功能讓 AI 探索過的互動生成 `.ad` 腳本，沉澱為 CI 回歸案例
- [ ] 跨平台測試時，用 `capabilities.ts` 定義每條命令的平台支援範圍，避免 AI 呼叫不支援功能

## 補充資料：同類工具 Mobile Next Mobile MCP（2026-09-29 併入）

來源：aiposthub〈Mobile Next MCP 是什麼？用 Claude、Codex 自動操作 iOS 與 Android 的安裝、功能與風險〉（2026-09-29）。核心論點與本條目同構：**優先讀原生 accessibility tree，讀不到才退回截圖座標**；Agent 驅動「重現 → 修正」迴圈，探索成熟後轉成固定回歸測試。本節只收 MCP 形式與安全面的增量。

**① 兩者差異**

| 項目 | agent-device（本條目） | Mobile Next Mobile MCP |
|---|---|---|
| 形式 | CLI＋daemon | MCP server |
| 授權 | MIT | Apache 2.0 |
| 平台 | iOS／Android／鴻蒙／Linux | iOS Simulator、iOS 真機、Android Emulator、Android 真機；另有付費 Mobile Next Cloud 遠端真機 |
| 客戶端 | — | 相容 MCP 的客戶端（Claude Code、Codex、Gemini、GitHub Copilot、Cursor 等） |
| 固化方式 | `.ad` 錄製回放 | 成熟後轉 mobilewright 或原生測試框架 |
| 遠端 | — | `--listen` HTTP 模式 |

**② 支援與工具**

- 前置條件：iOS Simulator 要 Xcode command line tools 與已啟動的模擬器；iOS 真機要 USB 連接＋信任裝置；Android 要 SDK／ADB，真機另需 USB debugging 與裝置授權
- 工具：列裝置、螢幕尺寸與方向、讀元素、點擊、長按、滑動、輸入、啟動／停止 App、deep link、錄影、crash log

**③ 安裝**

- Node.js 20 以上＋Xcode command line tools 或 Android Platform Tools
- MCP 設定執行 `npx -y @mobilenext/mobile-mcp@latest`；Claude Code 用 `claude mcp add`、Codex 用 `codex mcp add`
- 裝好後先啟動模擬裝置，再請 Agent「list available devices」；列不出來先查 Xcode、ADB、USB 授權與模擬器狀態
- 官方最新 release 為 1.0.5（2026-09-23）。`@latest` 只適合入門，團隊驗證後要**固定版本、讀 changelog**，在隔離裝置重跑核心流程後再升級

**④ 🔴 安全面**

- **版本安全修正歷程**（官方 changelog）：0.0.49 修正截圖與錄影的 path traversal；0.0.50 開網址限 HTTP／HTTPS（避免觸發撥號、簡訊等 scheme，除非使用者主動允許）；0.0.51 HTTP server 加 Bearer token 驗證（`MOBILEMCP_AUTH`）；0.0.52 強化跨來源請求與未驗證警告
- `--listen` HTTP 模式預設綁 localhost、有 Host header 防護，但**未設驗證時會接受未驗證連線**（只顯示警告）。改成對外介面時要設 `MOBILEMCP_AUTH` Bearer token，並限制防火牆與來源
- 預設透過 PostHog 與 Scarf 收集匿名 telemetry，關閉方式：`MOBILEMCP_DISABLE_TELEMETRY=1`
- SECURITY 文件建議：查看 Agent 正在呼叫哪些工具，並**使用專用裝置**。Agent 拿到的是整台裝置的能力，已登入的 Email、社群、銀行或公司帳號都可能被看到或操作
- 不適合直接自動執行：正式付款、刪除使用者資料、讀取私人訊息、修改雙重驗證、操作個人主要手機

**⑤ 定位與導入評估法**

- 與 XCUITest／Espresso／Appium 相比：強在探索速度（不用先寫完整測試程式），弱在 Agent 行為不穩定（同一提示在不同畫面狀態可能選不同順序）
- 作者建議的評估：五條不同難度的流程（單頁點擊、表單輸入、跨頁導覽、需要鍵盤的流程、含錯誤狀態的流程）× iOS／Android × 各跑三次，記錄成功率、平均步驟、人工介入次數、總耗時、失敗證據（截圖／錄影／crash），以及**哪些畫面只能靠座標**（穩定性風險）
- 起點示範：Miguel Ángel Durán 2026-09-27 在 X 發的 59 秒影片（讀 GitHub issue → 操作 iPhone Simulator → 找出文字與圖片問題 → 回到 Swift 程式碼提修正）。⚠️ **單一示例，不是成功率 benchmark**

🔎 同類工具已有兩個（agent-device、Mobile MCP）。再出現第三篇時，可考慮用 knowledge-synthesizer 做「Agent 驅動行動裝置自動化」精粹。

延伸：別只信工具的結構化回傳見 [[xcode-mcp-test-report-fabrication]]；專用裝置＝隔離見 [[agent-permission-and-os-sandbox]]；HTTP 模式的驗證問題見 [[mcp-server-stateless-streamable-http-oauth]]；Android 端的 CLI 工作流見 [[android-cli-ai-agent-workflow]]；Flutter 專案仍用 [[flutter-patrol-e2e-testing]]。

## 相關連結

- [[flutter-patrol-e2e-testing]] — Flutter 層 AI 測試框架（互補，Flutter 專案用）
- [[android-adk-agent-tool-calling]] — Google ADK for Android（Agent 工具呼叫設計）
- [[seven-agent-software-factory]] — 七代理人軟體工廠（測試驗證代理人設計參考）
- [[vibe-to-agentic-engineering]] — Agentic Engineering SOP（PEV Loop 中 Verify 階段的工具化）
- [[xcode-mcp-hidden-state-and-authorization]] — Xcode MCP 是接到活 IDE 的 bridge；Mobile MCP 繞過 IDE 直接操作裝置層（2026-09-29 新增）
- [[agent-permission-and-os-sandbox]] — Mobile MCP 建議用專用裝置＝隔離（2026-09-29 新增）
- [[fara-computer-use-agent]] — 相反路線：純截圖 VLM（Fara-7B、Apple MintAct），不讀 accessibility tree（2026-09-29 新增）
