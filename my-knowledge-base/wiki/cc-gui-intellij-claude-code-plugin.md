---
title: CC GUI：把 Claude Code 搬進 IntelliJ IDEA 的外掛，直接接 Claude Agent SDK
date: 2026-07-09
tags: [ai/claude, dev/architecture, type/how-to]
source: https://codelove.tw/@tony/post/3DREjq（原文：https://juejin.cn/post/7657794899358973986）
type: article
status: processed
asset_value: 54
---

`zhukunpenglinyutong/idea-claude-code-gui`（3.1k star）：IntelliJ IDEA 外掛，讓 Claude Code 直接嵌入 IDE 側邊欄使用，支援圖片解析/Skill 命令/MCP 伺服器，可配置 DeepSeek V4/GLM/Kimi 等第三方 API key（不強制 Anthropic 帳號登入）。技術上不是「終端包了個殼」，而是直接載入官方 `@anthropic-ai/claude-agent-sdk`，透過常駐 Node.js daemon + NDJSON 協定與 IDE 通訊。

## T（核心論點）

CC GUI 的差異化價值不是「又一個 Claude Code 封裝殼」，而是把 Agent SDK 直接嵌進 IDE，帶來三個終端版做不到的體驗：①能跟著改動即時審查程式碼（終端/Web 版做不到逐行追蹤）②讀取既有 `~/.claude/settings.json`，CLI 與外掛共用同一份設定/MCP/Skills，零重複配置 ③常駐 daemon 預先載入 SDK，省掉每次請求的冷啟動開銷。

## B（適用場景）

後端開發重度依賴 IntelliJ IDEA（Java/Kotlin/Spring 等）、想在 IDE 內直接用 Claude Code 而非切到終端；想用第三方便宜模型（DeepSeek V4/GLM/Kimi）取代官方 Anthropic 額度；需要 Agent 操作瀏覽器做前端自動化測試/除錯（配合 Chrome DevTools MCP）。

## R（重點整理）

**安裝與設定**：IDEA 外掛市集搜尋 CC GUI 安裝，首次啟動引導裝 Claude Code SDK（裝在 `~/.codemoss/dependencies/claude-sdk/node_modules/`）；供應商設定可直接讀取本地 `settings.json`（既有 Claude Code 設定直接沿用），或新增供應商填入第三方模型 ID 對應到 Sonnet/Opus/Haiku 三個槽位，本質是自動改寫 `settings.json`；統計面板可見每輪/累計 token 消耗。

**工作原理三層架構**：Java 端（IDEA 外掛）→ 常駐 daemon.js（背景 Node.js 子程序，預載 Claude Agent SDK，一次初始化重用，省 2-5 秒冷啟動）→ Claude Agent SDK。通訊走 NDJSON 協定（stdin/stdout 逐行 JSON），每條訊息帶請求 ID 支援平行多對話路由。daemon 每 15 秒心跳、每 10 秒檢查父程序，IDE 關閉自動退出不留殭屍程序；daemon 崩潰則 Java 端自動重啟（最多 3 次）。

**MCP/Skills 共用機制**：daemon.js 直接從 `settings.json` 讀取 MCP 伺服器配置，執行時把工具列表傳給 SDK——在 CC GUI 裝的 MCP 和終端 Claude Code 用的是同一套，無需重複設定。作者推薦必裝：Chrome DevTools MCP（前端自動化測試/除錯，尤其需要登入態才能重現的 bug）、idea MCP（與 IntelliJ IDEA 本體建立連線）、git commit MCP（自動寫 commit 訊息）；Skill 推薦 web-access（打通瀏覽器通道）與 frontend-design（需求描述→HTML/CSS 生成）。

**實戰案例佐證**（技術派開源專案）：① 把文章閱讀統計從直連資料庫改成 Redis 快取+定時同步，Agent 自動用 `incr` 做原子遞增、加過期時間、批量同步減少連線開銷，並透過 Chrome DevTools MCP 跑瀏覽器驗證 ② 留言系統從一次查全部改成一級留言分頁+二級回覆懶加載，橫跨前後端 9 個子任務，Agent 列出完整檔案異動清單，IDE 內可逐檔審查。

## C（關鍵行動）

- [ ] IntelliJ IDEA 重度使用者可評估導入，取代「終端切視窗」的工作流
- [ ] 有既有 Claude Code `settings.json` 設定可直接複用，不必重新配置
- [ ] 想省官方額度費用時，可透過供應商設定接入 DeepSeek V4/GLM/Kimi 等第三方 API
- [ ] 前端頁面自動化測試/除錯場景優先掛 Chrome DevTools MCP

## 相關連結

- [[claude-code-large-codebase]] — 大型專案 Claude Code 配置策略，可與 IDE 內用法互補
- [[fable5-subagent-model-routing-design]] — 同期社群關於模型分配/成本控制的另一種手法
