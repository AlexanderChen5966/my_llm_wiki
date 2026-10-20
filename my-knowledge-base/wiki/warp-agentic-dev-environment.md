---
title: Warp：Agentic 開發環境
date: 2026-06-02
tags: [ai/agent, dev/architecture]
source: https://github.com/warpdotdev/warp
type: article
status: processed
asset_value: 36
---

Warp 是一個從終端機演化而來的 agentic 開發環境，由 OpenAI 贊助開源。支援內建 coding agent 或外接 CLI agent（Claude Code、Codex、Gemini CLI 等）。核心特色為 Oz agent 系統，可自動化 issue triage、PR review、社群管理。

## T（核心論點）

Warp 定位為「agentic development environment」，不只是終端機，而是整合了 AI agent 的開發平台。內建 Oz agent 系統（GPT 模型驅動），可在公開 dashboard（build.warp.dev）觀看 agent 即時工作。UI 框架（warpui）MIT 開源，其餘 AGPL v3。

## B（適用場景）

**適用：**
- 想要統一管理多個 CLI agent 的開發者
- 開源專案維護者（Oz for OSS 提供免費 agent credits）
- 需要 agent-native 開發環境的團隊

**不適用 / 注意：**
- Oz agent 基於 GPT，非 Claude 生態
- 與 [[claude-squad]] 定位不同：Warp 是完整 IDE 級環境，claude-squad 是輕量 tmux 管理器

## 原始摘要

warpdotdev/warp GitHub README，介紹 Warp 作為 agentic 開發環境的定位、Oz agent 自動化系統、安裝方式與開源授權。

## 相關連結

- [[claude-squad]] — 輕量級多 Agent 終端管理（tmux + git worktree）
- [[multi-agent-cli-bridge]] — CCB 多 Agent CLI 工作區
- [[google-antigravity-platform]] — Google 的 Agent 開發平台（類似定位）
