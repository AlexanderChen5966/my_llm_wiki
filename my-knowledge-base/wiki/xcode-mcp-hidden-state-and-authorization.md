---
title: Xcode MCP 的狀態問題：agent 卡在它看不見的 run destination 前面
date: 2026-09-23
tags: [ai/mcp, ai/agent, type/concept]
source: https://medium.com/@n913239/agent-%E5%8D%A1%E5%9C%A8%E4%B8%80%E5%80%8B%E5%AE%83%E7%9C%8B%E4%B8%8D%E8%A6%8B%E7%9A%84%E4%B8%8B%E6%8B%89%E9%81%B8%E5%96%AE%E5%89%8D%E9%9D%A2-xcode-mcp-%E7%9A%84%E7%8B%80%E6%85%8B%E5%95%8F%E9%A1%8C-3b75666a3d26
type: article
status: processed
asset_value: 60
---

MikeWang（2026-09-18）Xcode 內建 MCP 實測系列的續篇。前篇 [[xcode-mcp-test-report-fabrication]] 談**失敗怎麼被報告**（工具拿舊結果或假訊號充數），本篇談**失敗為什麼發生**：它不是一個 server，是一條接到 IDE 上的橋，而 IDE 裡有一半的狀態 agent 看不到、也改不動。兩篇共用同一個實測場景（163 個測試、模擬器選錯），但論點不同，所以各自成條。

> ⏳ 時效：Xcode 27 的觀察全部來自 build `27A5237l`（beta 5），正式版可能再變；26.3、26.6 是正式版行為。

## T（核心論點）

**Xcode 內建 MCP 不是無狀態 server，而是經 XPC 接到活 Xcode 行程的 bridge——決定成敗的狀態不在 agent 那一側。** 26→27 最重要的變化不是工具從 21 支增加到 53 支，而是 agent 能讀寫 run destination，從「卡住」變成「自救」；再加上授權從「認 PID、每次要人按」改成可預先核准，**這兩件事合起來才決定 MCP 能不能進 CI**。

## B（適用場景）

**適用：**
- 用 agent 驅動 Xcode 建置／測試（含 Flutter 專案的 iOS 端）
- 評估任何「接到活應用程式」的 MCP（IDE、瀏覽器、桌面 App）：先問它有哪些 agent 看不到的狀態
- 決定 agent 工具授權流程時

**不適用 / 注意：**
- 要進 CI、不想依賴開著的 Xcode：替代方案 **XcodeBuildMCP**（走 `xcodebuild` CLI、無狀態、不需開 Xcode）
- 本篇 27 的行為都是 beta，升級前逐條自己量

## R（四種隱藏狀態＋授權）

### (1) GUI 的 run destination 與 headless 各自的 scheme

- 同一專案只把 toolbar destination 從實機改成模擬器，`BuildProject` 就從「No Accounts／No profiles」失敗變成 26.10 秒建置成功。`BuildProject` 只有 `tabIdentifier` 與可選的 `buildForTesting`，**沒有 destination 參數**；agent 看到簽章錯誤，會走進一條完全正確也完全沒用的路（去查 `DEVELOPMENT_TEAM`）。
- 26.3 共 20 支工具、18 支需要 `tabIdentifier`（例外：`XcodeListWindows`、`DocumentationSearch`）；26.6（`17F113`）是 21 支中 19 支，且沒有 `xcrun mcp-server`、沒有 `XcodeService.app`、沒有 `XcodeListRunDestinations`／`XcodeSwitchRunDestination`。**整個 26 正式版線都沒有自救能力。**
- 27 beta 5 工具 53 支（新增 36、移除 4）。`XcodeListRunDestinations` 回傳每個 destination 的 `isActive`／`isEligible`，`XcodeSwitchRunDestination` 可切換。實測 headless 預設 `iPhone 17 Pro (27.0)` 下 `RunAllTests` 63.6s、189 個全部 notRun；切到 `iPhone 16 Pro (18.6)` 後 Build 7.1s、`RunAllTests` 9.2s、163 passed。
- 27 的 headless 由 `XcodeService.app` 提供，有自己的 workspace、scheme、destination，**與 GUI 互不知情**——而 headless 正是 CI 會用的模式。參數名也跟著模式變（headless schema 只公告 `workspaceIdentifier`）。

### (2) 多版本並存：你到底在跟哪一個 Xcode 講話

- `xcrun mcpbridge` 預設連 `xcode-select -p` 指的那一版，不是你呼叫的那個 bundle。
- `initialize` 回的 `serverInfo.version` 是 bridge 自己 bundle 的版本號（26.6 為 `24952`、27 為 `25280.8`），**與實際連到誰無關**。接錯時 `tools/list` 永遠沒回應、不報錯、對話框也不跳。
- 解法：`MCP_XCODE_PID=$(pgrep -f 'Xcode_266.app/Contents/MacOS/Xcode') xcrun mcpbridge`。這個變數只寫在 27 的 `--help`，但 26.6 也吃。

### (3) 授權的顆粒度

- 26.3／26.6 認 **PID**：每個新 client 都要按一次 Allow（26.6 雖顯示 `Signed by`，但不拿來授權）。**「要人在旁邊按」才是 26 進不了 CI 的真正原因，不是工具少。**
- 27 改認**執行檔路徑**＋「Allow for 24 Hours」，有效簽章可永久允許；並有需要 sudo 的 `mcp-server approve`、`allow-folder` 預先授權。
- 未文件化的 key `IDEAllowUnauthenticatedAgents` 關閉後：`initialize` 照常成功，但 `tools/list` 回 0 支工具，而且對話框不跳——「連問都不問，直接不給」。

### (4) UI 面板狀態與建置日誌不一致

26.3 建置成功後，`XcodeListNavigatorIssues` 回 0 筆，`GetBuildLog(severity=warning)` 卻回 43,107 字元的警告。前者反映的是 UI 面板當下狀態（建置成功後面板被清空）。27 的解法是直接移除 `XcodeListNavigatorIssues`。

### 🔑 安全結論：授權對話框的資訊不足以判斷身分

對話框標題顯示的是 client 自己填的 `clientInfo.name`（可以填「Xcode Helper」）；路徑欄經 `/usr/bin/python3` shim 可以顯示成 Xcode bundle 內的 Python。**授權一個 MCP agent 和 `npm install` 一個沒聽過的套件是同一類決定——授權判斷應該在寫啟動指令時就完成，不要留給對話框。**

### 其他

- **錯誤訊息的實證**：schema 與實際不符四次，三次靠錯誤訊息一輪修好，`RunSomeTests`（26.3 收 `testIdentifiers`，26.6／27 改 `tests:[{targetName, testIdentifier}]`）只回 "The data couldn't be read because it is missing."。本條目只記事實，論點見 [[tool-description-and-error-message-as-prompt]]。
- **Xcode 27 部署目標最低 15.0**（26.3／26.6 為 12.0）：老專案 `IPHONEOS_DEPLOYMENT_TARGET` 9.0 在 27 上 16 個 error，需改 38 處。

## C（關鍵行動）

- [ ] 用 agent 跑 Xcode 建置前，先確認 run destination 與 `xcode-select -p`；多版本並存時一律帶 `MCP_XCODE_PID`
- [ ] CI 場景優先評估 XcodeBuildMCP，而非依賴開著的 Xcode
- [ ] 升級 Xcode 27 前先檢查專案與 Pods 的 `IPHONEOS_DEPLOYMENT_TARGET`
- [ ] 接任何新 agent 工具時，先列出「它有哪些狀態是我看不到的、失敗時會怎麼講」

## 原始摘要

作者在同一專案、同一腳本下只改 GUI 的 destination，建置結果就從失敗變成功，由此展開：MCP bridge 的定址單位是「某個 Xcode 視窗的某個分頁」；GUI 與 headless 狀態分離；兩支診斷工具給相反答案；`xcode-select` 在外面決定你連到誰；授權對話框從認 PID 到認路徑與簽章；對話框的名稱與路徑都可被偽裝；27 的部署目標門檻讓老專案編不起來。

## 相關連結

- [[xcode-mcp-test-report-fabrication]] — **同作者前篇**：前篇談失敗怎麼被報告（假的 163 passed），本篇談失敗為什麼發生（狀態不在 agent 這一側）
- [[mcp-server-stateless-streamable-http-oauth]] — **對照組**：刻意無狀態的 Streamable HTTP server vs 本篇有狀態的 stdio bridge
- [[mcp-stateless-migration-state-ownership]] — state 沒有消失，只是換人負責；本篇是 state 藏在 IDE 裡、agent 無權過問的實例
- [[tool-description-and-error-message-as-prompt]] — 本篇的 schema／錯誤訊息實測是該條論點的第三方佐證
- [[self-built-mcp-server-237-lines]] — 同作者系列：無狀態 stdio server 的對照組，以及 `.mcp.json` 待核准的供應鏈邊界
- [[agent-permission-and-os-sandbox]] — agent 權限與 OS 沙箱
- [[agent-tool-authorization-gatekeeper]] — 工具授權守門機制
- [[agent-device-cross-platform-testing]]（含 Mobile Next MCP 補充）— 對照：Mobile MCP 繞過 IDE 直接操作裝置層，沒有本篇的 run destination 隱藏狀態問題；但 iOS 端仍依賴 Xcode CLT 與模擬器狀態，「先啟動模擬器、先列裝置」對應本篇「選錯 destination」（2026-09-29 新增）
