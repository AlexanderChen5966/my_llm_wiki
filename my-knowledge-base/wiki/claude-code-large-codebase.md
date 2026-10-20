---
title: Claude Code 大型專案最佳實踐：配置決定成效
date: 2026-05-28
tags: [ai/claude, dev/architecture]
source: https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start
source_2: https://codelove.tw/@tony/post/aAOnPx（中文轉述版〈Claude Code 在大型專案中的最佳實踐指南〉，raw/ 素材 2026-05-25）
type: article
status: processed
asset_value: 48
---

## T（核心論點）

在大型程式碼庫中，Claude Code 效果好壞的差異不在模型，在配置——結構化 CLAUDE.md、優化 context 輸入、正確使用工具是讓百萬行 monorepo 跑得飛的關鍵；Claude Code 不用 RAG，靠 Grep/Read 即時理解程式碼庫。

## B（適用場景）

**適用：** 大型 monorepo、多人團隊使用 Claude Code、希望減少重複解釋的中大型專案。

**不適用 / 注意：** 幾千行的小專案反而不需要這套配置；過度配置反而降低靈活性。

## R（關鍵配置模式）

1. **不用 RAG，善用工具**：Claude Code 靠 Grep/Glob/Read 即時掃描，embedding 索引跟不上大型活躍 codebase 的提交速度
2. **CLAUDE.md 三層結構**：全域層（架構原則）→ 專案層（repo 地圖）→ 子目錄層（模組規範）
3. **Context 精準輸入**：給路徑（`src/auth/`）而非概念描述（「登入邏輯」），減少不必要的 Glob 搜索
4. **AGENTS.md 放在子目錄**：每個模組放自己的 AGENTS.md，描述該模組的職責邊界與 API 合約
5. **Hooks 自動執行**：PostToolUse Hook 在每次修改後自動跑 lint/test，不靠 Claude 記得要跑

## C（關鍵行動）

- [ ] 檢查 CLAUDE.md 是否有 repo 地圖（主要入口點、核心模組路徑）
- [ ] 大型需求先給具體檔案路徑，不給模糊描述
- [ ] 設定 PostToolUse Hook 自動跑測試，減少 context 被驗證步驟占滿

## 原始摘要

Anthropic Applied AI 團隊觀察大量企業級部署後總結，基於 Anthropic 官方部落格《How Claude Code works in large codebases》整理，補充工程場景深度解讀。

## 相關連結

- [[claude-code-file-management]] — 四層設定架構（Managed/User/Project/Local）
- [[claude-code-context-management]] — Context 管控全攻略
- [[claude-code-advanced-workflow]] — Boris Cherny 15 個進階技巧
- [[harness-engineering]] — Harness 工程（企業落地視角）
