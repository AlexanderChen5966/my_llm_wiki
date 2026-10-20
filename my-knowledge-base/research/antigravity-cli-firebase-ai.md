---
title: 用 Antigravity CLI 建置 Firebase AI Logic 影像分析應用（Angular + Gemini）
date: 2026-07-24
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10400438
type: article
status: processed
---

## 概述

iThome 實作教學：用 **Antigravity CLI**（代理型 AI 助理）結合 Angular、Firebase Hybrid & On-device Web SDK 與 Gemini，建置影像分析展示應用。使用者上傳圖片，Gemini 分析產生替代文字（alt text）、標籤、建議與 CSS 提示。

**核心亮點——混合推論降本**：在 Chrome 148+ 用**裝置端 Gemini Nano（Prompt API）零 Token**；其他瀏覽器（Safari/Firefox）降級退回**雲端 Gemini 3.5 Flash**（消耗 Token）。

## 環境配置

**安裝 skills**：`grill-with-docs`（嚴格問答產生規格/ADR）、`domain-modeling`（被 grill-with-docs 引用）、`code-review`（衍生兩個子代理審查）、`angular`（Signals/Signal Forms 最佳實踐）、`firebase`。

**註冊 MCP 伺服器**（Antigravity 無地理圍欄，寫入 `~/.gemini/config/mcp_config.json`）：
- **Stitch MCP**（`@_davideast/stitch-mcp`）：需 Stitch API Key + `STITCH_USE_SYSTEM_GCLOUD` + `GOOGLE_CLOUD_PROJECT`；design-to-code 工具（Stitch 網站受地理圍欄，香港需 VPN）。
- **Angular MCP**（`@angular/cli mcp`）：ai_tutor / get_best_practices / search_documentation / onpush_zoneless_migration / run_target / devserver 等。

## 兩種開發工作流

- **工作流 1（不建肌肉記憶）**：`/grill-with-docs` 產生 ADR 供人工審查 → `/goal implement <ADR>` 讓助理實作並接受變更。
- **工作流 2（建肌肉記憶）**：`/grill-with-docs` 產 ADR → 自己實作並 commit → `/code-review` 抓 git 分支，衍生兩子代理（一偵測 code smell、一對照 ADR 驗證 scope creep/遺漏）→ 修到 0 違規 → merge main → 部署 Firebase App Hosting。

## Stitch → DESIGN.md → Angular 元件

用自然語言讓 Stitch 產生首頁與影像分析頁 → 用 Stitch MCP 擷取畫面、萃取設計 token 產生 **DESIGN.md**（首頁存根目錄、分析頁存子資料夾）→ 審查後 `/goal` 讓助理把 DESIGN.md 對應成 production-ready Angular 元件（提示詞明確要求別把所有 CSS/HTML/邏輯塞進單一元件）。

## 相關連結

- [[design-md-for-ai-ui]] — DESIGN.md 概念與現成收藏庫（本案例用 Stitch 產生 DESIGN.md）
- [[design-md-google-format]] — design.md 格式規範與 CLI 工具
- [[google-antigravity-platform]] — Antigravity 平台/CLI（`agy`）總覽，本篇為其具體實作案例
- [[claude-agent-sdk-human-in-the-loop]] — /code-review 子代理驗證屬 HITL 審核模式
