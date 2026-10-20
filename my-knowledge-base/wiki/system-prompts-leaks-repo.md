---
title: system_prompts_leaks：主流 AI 產品系統提示詞蒐集庫
date: 2026-07-09
tags: [ai/prompt-engineering, type/reference]
source: https://github.com/asgeirtj/system_prompts_leaks
type: article
status: processed
asset_value: 42
---

`asgeirtj/system_prompts_leaks`：持續更新的開源 repo，蒐集並公開各家 AI 產品被引導洩漏出的系統提示詞（Claude 全系列、GPT-5.5、Gemini、Grok、Copilot、Cursor、Perplexity 等 20+ 家），2026-05-11 曾被 Washington Post 報導引用。

## T（核心論點）

系統提示詞是模型「行為的隱藏規則」，直接閱讀官方廠商如何約束自家模型（工具描述、安全政策、人格設定、拒答邊界），比看公開文件更接近實際運作方式。此 repo 的價值在於**跨廠商橫向對照**——同時能看到 Anthropic/OpenAI/Google/xAI 對同類問題（工具呼叫格式、安全紅線、人格語氣）的不同解法。

## B（適用場景）

研究 Prompt Engineering 手法、想理解特定產品行為為何如此（如 Claude Code 為何頻繁要求確認高風險操作）、比較不同廠商系統提示詞設計哲學、寫自己的 CLAUDE.md / AGENTS.md 時參考官方如何組織規則優先序。

## R（重點整理）

- **涵蓋範圍**：Anthropic（Fable 5/Opus 4.8/Sonnet 5/Claude Code/Cowork/Design/Chrome 插件等全家族＋歷史版本）、OpenAI（GPT-5.5 全變體+Codex+各項 tool 提示詞）、Google（Gemini 3.5/CLI/Antigravity/NotebookLM）、xAI（Grok Build/Expert）、Perplexity、Microsoft Copilot 全家族、Cursor、Meta、Mistral、Notion、Qwen 等
- **版本追蹤**：同一模型的新舊版本並存（如 Claude Opus 4.6→4.7→4.8 逐版對照），並提供 diff 連結（如 Opus 4.8 → Fable 5 的 diffchecker 連結），可直接看出每次更新新增/移除了哪些規則
- **分層版本**：部分模型同時提供「官方發布版」（`claude_behavior` at release date，不隨後續調整更新）與「即時抓取版」兩種，前者是版本快照、後者反映當下實際運作
- **持續更新特性**：作為觀察 AI 產品規則演進的活史料，而非一次性資料集
- **同類專案補充**：另有同性質蒐集庫涵蓋 AI Coding Agent 陣營（Devin、Manus、Lovable、Windsurf、v0、Bolt、Replit Agent 等），與本篇偏重聊天助理/CLI 工具的收錄範圍互補——研究哪個產品的系統提示詞，先確認該產品屬於聊天助理還是 AI 開發代理陣營，兩類蒐集庫收錄重疊度不高

## C（關鍵行動）

- [ ] 撰寫或優化 CLAUDE.md/AGENTS.md 時，可參考同類官方系統提示詞的規則優先序與措辭方式
- [ ] 理解特定產品「為什麼會這樣回應」時，先查是否有對應系統提示詞可對照
- [ ] 內容為第三方抓取，非官方公開文件，使用時留意可能與當下實際版本有落差

## 相關連結

- [[claude-fable-5-usage-guide]] — Fable 5 官方使用指南（互補：官方文件 vs 系統提示詞原文對照）
- [[agent-era-prompt-structure]] — Prompt 結構方法論（互補：這裡是產品端範例，那篇是撰寫方法論）
- [[claude-code-prompt-library-52]] — Claude Code 官方 52 組提示詞（互補：官方公開的 prompt 範本 vs 本庫洩漏的系統提示詞原文）
