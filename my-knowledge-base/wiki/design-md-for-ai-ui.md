---
title: awesome-design-md：即用的品牌 DESIGN.md 收藏庫，讓 AI 生成一致 UI
date: 2026-07-24
tags: [ai/workflow, dev/architecture, type/reference]
source: https://github.com/voltagent/awesome-design-md
type: reference
status: processed
asset_value: 27
---

VoltAgent 維護的 GitHub 收藏庫（全球排名 #150），收錄從 Claude、Stripe、Linear、Apple、Vercel、Figma 等知名品牌網站萃取的**即用 DESIGN.md 檔案**。把一份 DESIGN.md 丟進專案根目錄，叫 AI agent「做一個看起來像這樣的頁面」，就能生成視覺一致的高品質 UI。

## T（核心論點）

DESIGN.md 是 Google Stitch 提出的純文字設計系統文件概念——對應關係是：`AGENTS.md`（給 coding agent 讀「如何建置」）vs `DESIGN.md`（給 design agent 讀「該長什麼樣、什麼感覺」）。它只是一個 Markdown 檔（LLM 最好讀的格式），無需 Figma 匯出、JSON schema 或特殊工具。此 repo 提供的是「現成範例集」，非格式規範本身。

## R（重點）

**每份 DESIGN.md 的 9 大區塊**（Stitch 格式 + 擴充）：① 視覺主題與氛圍 ② 色票與角色（語意名+hex+功能）③ 字體規則 ④ 元件樣式（含狀態）⑤ 佈局原則 ⑥ 深度與立體 ⑦ Do's/Don'ts ⑧ RWD 行為 ⑨ Agent 提示指南。每個站台附 `DESIGN.md` + `preview.html` + `preview-dark.html`。

**收錄分類**：AI/LLM 平台（Claude/Cohere/Mistral/Ollama…）、開發工具/IDE（Cursor/Vercel/Warp…）、後端/DevOps、SaaS、設計工具（Figma/Framer…）、金融加密、電商零售、媒體消費、汽車，甚至「復古網頁」（1996 Dell / 2001 Nintendo）系列。

**用法**：複製某站台的 DESIGN.md 到專案根目錄 → 叫 AI agent 使用它。MIT 授權；宣告不主張任何站台視覺識別的所有權（僅公開可見的 CSS 值）。

## C（關鍵行動）

- [ ] 需要特定風格時，直接從此 repo 挑一份現成 DESIGN.md 而非從零寫
- [ ] Flutter/前端專案可比對 9 區塊結構，建立自己的 DESIGN.md
- [ ] 與 figma-to-requirements skill 對照：DESIGN.md 可當設計 token 的上游來源

## 相關連結

- [[design-md-google-format]] — design.md 格式規範本身（Google Labs 開源、YAML+Markdown 兩層、CLI lint/diff/export/spec），本篇為其「現成範例集」
- [[agents-md-guide]] — AGENTS.md 撰寫指南（DESIGN.md 的姊妹檔）
- [[antigravity-cli-firebase-ai]] — 用 Stitch 產生 DESIGN.md 再對應成 Angular 元件的實作案例
- [[open-design-claude-design-comparison]] — DESIGN.md 九段格式在 Open Design 生態的應用
