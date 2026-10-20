---
title: Open Design vs Claude Design：完整比較與選擇指南
date: 2026-06-15
tags: [ai/claude, ai/agent]
source: https://bnext.com.tw
type: article
status: processed
asset_value: 72
---

Open Design（開源本地化）與 Claude Design（雲端訂閱）在 2026 年同期存在；選擇的關鍵不是功能比拼，而是工作流成本結構——已有 Claude Code Max 訂閱者，選 Open Design 邊際成本趨近於零。

## T（核心論點）

**Claude Design vs Open Design 的本質差異是「雲端訂閱 vs 本地 BYOK」**，功能上 Open Design 在 HyperFrames（HTML→MP4 輸出）等方面反超。選擇框架：

| 情境 | 建議 |
|------|------|
| 已付費 Claude Code CLI Max | → Open Design（邊際成本 ≈ 0）|
| 無 agent CLI、重設計功能 | → Claude Design |
| 醫療 / 金融機密設計 | → Open Design（本地資料不外傳）|
| 創辦人 / Pitch Deck | → Claude Design（快速上手）|

## B（適用場景）

- 已使用 Claude Code CLI 且需要 UI 設計功能
- 需要 HTML→MP4 影片輸出（HyperFrames）
- 使用多套 Agent CLI（Claude Code / Cursor / Gemini CLI）的獨立開發者
- 對資料隱私要求高的設計工作

## R（完整內容）

### Claude Design

- 平台：`claude.ai/design`，Pro+ 訂閱
- 模型：Opus 4.7
- 特點：雲端，快速上手，不需本地安裝
- 限制：工具鎖定、雲端依賴、訂閱費用

### Open Design

- 授權：Apache-2.0，GitHub 60.4K stars
- 時間軸：Claude Design 上線後 11 天開源
- 架構：Web UI（Next.js 16）+ Local Daemon（Express）+ SQLite + BYOK Proxy + Coding Agent CLI
- 特色功能：
  - **HyperFrames**：HTML → MP4 影片輸出（Claude Design 沒有）
    - ⚠️ 2026-10-19：2026-10-08 起 Claude Motion（Team／Enterprise beta）可原生匯出 MP4，並可用「Send to」交接到 HyperFrames（單一來源轉述，見 [[claude-motion-beta-2026-10]]）。Motion ≠ Claude Design，原句仍成立；但「Claude 家族沒有 MP4 輸出」的印象已不準確。Motion 的 Send to 交接也和本條 Open Design 的 HyperFrames 安裝方式不同
  - **259+ Skills** / **142+ Design Systems** / **261+ Plugins**
  - **DESIGN.md 九段格式**：color / typography / spacing / layout / components / motion / voice / brand / anti-patterns
  - 支援 MCP stdio server 整合
  - 支援 Claude Code / Codex / Cursor Agent / Gemini CLI / OpenCode / Qwen CLI / GitHub Copilot / Hermes / Kimi CLI

### 安裝

```bash
git clone https://github.com/nexu-io/open-design.git
corepack enable && pnpm install
pnpm tools-dev run web

# MCP 接入
od mcp install claude-code
od system use linear
od skill prototype --prompt "..."
od export html/mp4
```

### 未來預測

- Anthropic：Claude Design + Claude Code 整合
- Open Design：分拆為桌面 App + CI SDK

## C（關鍵行動）

- [ ] 若已有 Claude Code Max，直接 `git clone` 試用 Open Design
- [ ] 設計規格存在 DESIGN.md（9段格式）而非散在 prompt
- [ ] HyperFrames：設計展示用 MP4 輸出，取代截圖流程

## 相關連結

- [[claude-design-tool]] — Claude Design 基礎介紹
- [[claude-code-newbie-guide]] — Claude Code 入口差異
- [[api-cli-skills-agent-architecture]] — CLI 三層架構
- 📎 2026-10-19：[[claude-motion-beta-2026-10]] — Claude Motion：原生 MP4 匯出，可交接 HyperFrames（與本條開源路線不同）
- 📎 2026-10-19：[[hyperframes-heygen-html-to-video]] — HyperFrames 專篇（README 一手：Apache 2.0、Node.js 22+＋FFmpeg、三條安裝路徑、Studio）
