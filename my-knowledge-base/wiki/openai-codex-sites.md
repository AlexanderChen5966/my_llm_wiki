---
title: OpenAI Codex Sites：從想法到可部署網站的 AI 工具
date: 2026-06-15
tags: [ai/agent]
source: https://openai.com/codex-sites
type: article
status: processed
asset_value: 58
---

Codex Sites 是 OpenAI Codex 的新功能（Preview 階段），允許將想法、分析或專案**直接轉換為可透過 URL 分享的互動式網站、App 或儀表板**。僅限 Business / Enterprise 方案，每週 500 萬+ 用戶使用，且非開發者用戶成長速度是開發者的 3 倍。

## T（核心論點）

Codex Sites 的核心定位：**讓非開發者也能「部署」Web 應用**。這不是靜態生成器，每個部署都是帶 D1 資料庫、R2 物件儲存和 RBAC 的正式 production URL。

## B（適用場景）

**適用：**
- 需要快速把分析/報告變成可互動的 Web 儀表板
- 原型驗證（不需要設定整套開發環境）
- 企業內部工具快速部署

**不適用（目前）：**
- 個人/Pro 方案（僅限 Business/Enterprise）
- 需要複雜後端邏輯的正式產品
- 對延遲和效能有嚴格要求的場景

## R（功能詳細）

### 核心能力

- **自然語言 → 網站**：描述你的需求，Codex Sites 生成並部署
- **資料庫支援**：D1（SQLite-based）
- **物件儲存**：R2（S3-compatible）
- **存取控制**：Workspace auth + RBAC（按角色控制存取）
- **Annotations**：精準標記要修改的元素（非重新生成全部）

### 角色專屬 Plugins

62 個 Apps + 110 個 Skills 依角色分類（如銷售、財務、行銷等），降低非技術用戶的起步門檻。

### 用戶趨勢

- 每週 500 萬+ Codex 用戶中，非開發者比例持續上升
- 非開發者用戶成長速度是開發者的 **3 倍**
- 說明 Codex Sites 的主要增長動力來自業務側用戶，不是工程師

### 與現有工具比較

| 工具 | 定位 | 門檻 |
|------|------|------|
| Codex Sites | 想法→部署，含真實 DB | 低（自然語言）|
| Vercel v0 | UI 組件生成 | 中（需整合）|
| Cursor/Claude Code | 開發者工具 | 高（需寫 code）|

## C（關鍵行動）

- [ ] 若升至 Business 方案，評估 Codex Sites 是否可取代現有的原型工具
- [ ] 關注是否開放 Pro 方案（目前僅 Business/Enterprise）

## 相關連結

- [[codex-advanced-tips-11]] — Codex 11 個進階技巧（Goals / Durable Threads）
- [[codex-plugin-for-claude-code]] — Claude Code 與 Codex 的跨平台協作
- [[codex-plugin-workflow]] — Codex Plugin 可重用工作流架構
