---
title: CC Switch：跨 8 種 AI CLI 的桌面 provider 統合管理器
date: 2026-07-20
tags: [ai/agent, ai/workflow]
source: https://github.com/farion1231/cc-switch
type: reference
status: processed
asset_value: 54
---

`farion1231/cc-switch`（官網 ccswitch.io）：跨平台桌面 All-in-One 助手，用單一 App 管理 8 種 AI 工具（Claude Code、Claude Desktop、Codex、Gemini CLI、Grok Build、OpenCode、OpenClaw、Hermes Agent）的 provider、MCP、Prompts、Skills。Tauri 2 + React/TS + Rust，資料存本機 SQLite（原子寫入防設定損毀）。

> ⚠️ **與 [[claude-swap-account-rotation]] 是完全不同的專案**。cc-switch＝跨 8 種 CLI 的 provider 統合桌面 App（換供應商/管 MCP）；claude-swap＝單一 Claude Code 的多帳號限額輪替 CLI（換帳號/避限速）。用途、範圍、形態都不同，勿混淆。

## T（核心論點）

現代 AI coding 依賴的每個工具都有自己的設定格式（JSON/TOML/.env），換 API provider 要手動改設定檔，且沒有統一方式跨工具管理 MCP 與 Skills。cc-switch 把這件事變成視覺化操作：一鍵匯入 provider（50+ 內建預設）、即時切換、跨工具統一管理 MCP/Prompts/Skills 並雙向同步——核心價值是「消除手改設定檔的摩擦」與「跨多工具的統一控制面板」。

## B（適用場景）

**適用：** 同時用多種 AI CLI 且常換 provider（官方/第三方 relay/Bedrock/NIM）的重度用戶；想跨 Claude/Codex/Gemini 統一管 MCP 與 Skills 的人；需要成本/用量儀表板追蹤的人。
**不適用 / 注意：** 只用單一工具單一帳號者用不上；README 含大量 relay 供應商贊助廣告（選 provider 時自行判斷）；只換 Claude Code 帳號避限速請用 claude-swap 而非本工具。

## R（重點整理）

- **8 工具 + 50+ preset**：複製 key 一鍵匯入；Universal provider 一份設定可同步到 Claude Code/Codex/Gemini CLI。系統匣快速切換、拖拉排序、匯入匯出。
- **Proxy & Failover**：本地 proxy 熱切換（格式轉換、自動 failover、斷路器、provider 健康監控）；可對 Claude/Codex/Gemini/Grok Build 個別 app 層級接管。
- **統一 MCP / Prompts / Skills**：單一面板管 MCP server 跨工具雙向同步 + Deep Link 匯入；Prompts 用 Markdown 編輯器跨 app 同步（CLAUDE.md/AGENTS.md/GEMINI.md）；Skills 從 GitHub repo 或 ZIP 一鍵安裝（symlink 或複製）。
- **Usage 儀表板**：追蹤花費/請求/token，趨勢圖 + 請求 log + 自訂每模型計價。
- **Session Manager**：跨支援來源瀏覽/搜尋/還原對話歷史；OpenClaw Workspace 編輯器編 AGENTS.md/SOUL.md。
- **系統/平台**：雲端同步（Dropbox/OneDrive/iCloud/WebDAV）、Deep Link（`ccswitch://`）、深淺色主題、自動更新、原子寫入、自動備份、i18n（含 zh-TW）。跨 Windows/macOS/Linux。
- **設計原則**：SSOT（全存 `~/.cc-switch/cc-switch.db`）、雙層儲存（SQLite 可同步資料 + JSON 裝置設定）、雙向同步（切換時寫入 live 檔、編輯當前 provider 時回填）、最小侵入（即使卸載 App，CLI 仍正常運作，故不能刪除當前啟用的 provider）。macOS 已 code-sign + notarize，`brew install --cask cc-switch` 可裝；Claude Code 支援熱切換不需重啟終端（其他工具需重啟）。

## C（關鍵行動）

- [ ] 同時用 Claude Code + Codex + Gemini CLI 且常換 provider 者可評估導入
- [ ] 用「統一 MCP/Skills 面板」取代逐工具重複設定
- [ ] 需要多帳號避限速輪替請改用 claude-swap（不同工具）

## 原始摘要

GitHub README：cc-switch 跨 8 種 AI CLI 的桌面 provider 統合管理器，50+ provider 預設、統一 MCP/Prompts/Skills 管理、本地 proxy failover、usage 儀表板、雲端同步。Tauri 2 + Rust + React。

## 相關連結

- [[claude-swap-account-rotation]] — ⚠️ 不同工具：單一 Claude Code 多帳號限額輪替 CLI
- [[opensquilla-intelligent-model-router]] — 智慧模型路由，與 provider 切換同屬成本/供應商管理
- [[openrouter-fusion]] — 多模型平行 + 裁判合成，另一種多 provider 使用策略
- [[ai-coding-agent-governance-dotbabel]] — 跨 CLI 治理與同步工具，可對照 cc-switch 的跨工具管理定位
