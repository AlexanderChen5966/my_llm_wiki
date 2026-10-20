---
title: Flutter Patrol：以 gRPC 雙向通訊打通原生層的 E2E 測試框架
date: 2026-06-16
tags: [dev/flutter]
source: https://codelove.tw/@tony/post/3Qjv7a
type: article
status: processed
asset_value: 58
---

Flutter 內建的 `integration_test` 無法直接操作原生 UI（系統權限對話框、推播通知、其他 App），導致跨 App 或涉及系統層級互動的測試場景難以自動化。Patrol 透過 gRPC 雙向通訊把 Flutter 測試程式與原生 Automator 服務串起來，補上這塊空白。

## T（核心論點）

Patrol 是一套 Flutter E2E 測試框架，核心架構是雙向 gRPC 通訊：Flutter 端的 `PatrolAppService` 與原生端（iOS/Android）的 Automator 服務互相呼叫，讓測試程式碼能直接操作系統層 UI（權限對話框、通知中心、設定頁面），而不只是 App 內部 Widget。v2.9+ 提供 `patrol_mcp`，讓 AI 工具（如 Claude Code）可透過 MCP 直接驅動與除錯測試執行。

## B（適用場景）

**適用：**
- 測試流程涉及系統權限請求（相機、定位、通知）需要自動點擊原生對話框
- 需要驗證推播通知、深層連結（deep link）等跨 App 邊界行為
- 想用 AI Agent 自動執行/除錯 E2E 測試（搭配 patrol_mcp）

**不適用：**
- 純 Widget 層級的單元/元件測試（用標準 `flutter_test` 即可，不需要 Patrol 的原生互動能力）
- 專案沒有任何跨原生層互動需求，導入 Patrol 增加的設定成本大於收益

## R（架構與能力）

### 雙向 gRPC 架構

```
Flutter Test Code  <—gRPC—>  PatrolAppService（Flutter 端）
                                      ↕
                              Native Automator（iOS/Android 原生服務）
                                      ↕
                          系統 UI（權限對話框/通知/設定）
```

測試程式碼透過 `PatrolAppService` 發出指令，由原生 Automator 服務實際執行對系統 UI 的操作（例如點擊「允許」權限按鈕），執行結果再透過 gRPC 回傳給 Flutter 測試端，形成雙向通訊而非單向呼叫。

### 核心套件對照

| 套件 | 角色 |
|------|------|
| patrol | 測試框架主體，提供 `patrol()` 測試函式與斷言 API |
| patrol_cli | 編譯/執行測試的命令列工具，封裝原生建置流程 |
| patrol_finders | 增強版 Widget Finder，支援原生 UI 元素查找 |
| patrol_mcp | MCP Server，讓 AI 工具可直接驅動/除錯測試 |

### 能力對照表

| 能力 | integration_test（內建） | Patrol |
|------|------------------------|--------|
| Widget 互動 | ✅ | ✅ |
| 系統權限對話框操作 | ❌ | ✅ |
| 推播通知互動 | ❌ | ✅ |
| 跨 App 操作 | ❌ | ✅ |
| AI 驅動測試執行（MCP）| ❌ | ✅（patrol_mcp）|

### 平台支援

| 平台 | 支援狀態 |
|------|---------|
| Android | 完整支援（含系統 UI 操作）|
| iOS | 完整支援（含系統 UI 操作）|
| Web/Desktop | 不支援原生 Automator 互動，僅能做 Widget 層測試 |

### patrol_mcp：AI 驅動測試的新切入點

透過 MCP 把 Patrol 測試能力暴露給 Claude Code 等 AI 工具，讓 AI 可以直接觸發測試執行、讀取失敗結果並協助除錯，把「寫測試」與「跑測試除錯」的迴圈整合進 AI 對話流程中，而非人工切換到終端機執行再貼回結果。

## C（關鍵行動）

- [ ] 若專案測試流程卡在系統權限對話框或推播通知，評估導入 Patrol 取代純 `integration_test`
- [ ] 純 Widget 層測試維持用標準 `flutter_test`/`integration_test`，避免不必要的原生整合複雜度
- [ ] 評估用 `patrol_mcp` 把測試執行/除錯整合進 Claude Code 工作流，減少人工切換終端機的摩擦

## 相關連結

- [[flutter-google-io-2026]] — Flutter 生態動態與 AI 工具鏈發展（可一併追蹤）
