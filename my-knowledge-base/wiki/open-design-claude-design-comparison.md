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

## 補充資料

**2026-11-19 補充（來源：aiposthub〈OpenDesign Motion 開源：用不同 AI 模型做可編輯動畫與 MP4 影片〉，https://www.aiposthub.com/opendesign-motion-open-source-editable-video/ ，2026-10-17）**：依官方 X 發布與更新頁整理，非站方實測。

- **OpenDesign Motion**：選擇器裡的動態設計技能，附五個範例起點（未來感動畫、產品宣傳、作品展示、介面變形、視覺教學）
- **輸出分兩種**：影片請求 → 可編輯來源＋可播放 MP4；互動動畫 → 可執行 HTML。交付前先決定要哪一種，社群貼文要檔案、網頁互動要程式＋素材一起部署
- 匯出失敗會保留來源，但「有來源」≠「成片已輸出」，交付前要實際播放
- **可換模型／代理**：便宜配置先搭架構、難轉場交給強模型——但前一個模型理解的顏色、字級、節奏若沒寫成檔案，換模型後會被重新詮釋；模型自由度要搭配固定素材、版型、驗收條件
- **驗收法**：第一版後指定一個小修改（換日期、縮開場、改直式），看能否只動必要範圍並重新輸出；檢查原始專案換電腦能否接續、MP4 從頭播放、長標題與繁中字型、直式裁切。工作區的開源授權 ≠ 影片內照片／音樂／字型的授權
- 與 Claude 原生路線的對照見 [[claude-motion-beta-2026-10]]

## 相關連結

- [[claude-design-tool]] — Claude Design 基礎介紹
- [[claude-code-newbie-guide]] — Claude Code 入口差異
- [[api-cli-skills-agent-architecture]] — CLI 三層架構
- 📎 2026-10-19：[[claude-motion-beta-2026-10]] — Claude Motion：原生 MP4 匯出，可交接 HyperFrames（與本條開源路線不同）
- 📎 2026-10-19：[[hyperframes-heygen-html-to-video]] — HyperFrames 專篇（README 一手：Apache 2.0、Node.js 22+＋FFmpeg、三條安裝路徑、Studio）
- 📎 2026-10-30：[[artcraft-open-source-adobe-alternatives-timeline]] — 另一條開源設計工具路線：ArtCraft 七款 Adobe 對應應用（Alpha／開發中）的狀態與時間線
