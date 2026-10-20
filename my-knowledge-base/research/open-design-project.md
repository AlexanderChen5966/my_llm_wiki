---
title: Open Design：Local-First 開源 AI 設計平台
date: 2026-05-28
tags: [ai/agent, dev/architecture]
source: https://www.knightli.com/zh-tw/2026/05/18/open-design-open-source-claude-design-alternative/
type: article
status: processed
asset_value: 27
---

## 核心論點

Open Design (nexu-io) 是 local-first、開源的 Claude Design/Figma 替代方案，核心創新是把現有的 coding agent CLI（Claude Code、Codex、Gemini CLI、Cursor 等）當作「設計引擎」，透過 Skills + Design Systems + 本地 daemon 把它們接入設計工作台，生成可在沙盒預覽的 artifact。

## 三大組成

| 元件 | 功能 |
|------|------|
| Web UI | 對話、預覽、管理專案、匯出產物 |
| 本地 daemon | 調度 Agent、管理文件、保存專案、提供 API |
| Skills + Design Systems | 約束 Agent 生成符合設計規範的產物（而非隨手拼湊）|

## 工作流

1. 使用者輸入需求 → AI 補充設計簡報、選擇場景方向
2. 注入上下文：專案元資料 + 設計系統 + Skill 文件 + 模板 + 檢查清單
3. Agent 在真實資料夾裡讀寫文件
4. 沙盒 iframe 即時預覽產物

## 適用場景

- 需要自托管設計 AI 工具（不想依賴 Claude Design 閉源雲端服務）
- 想接入自訂模型或私有設計系統
- 需要把 AI 設計輸出整合進本地工作流程

## 相關連結

- [[claude-code-design-skills-overview]]
- [[claude-design-product]]
- [[agent-skill-design-patterns]]
