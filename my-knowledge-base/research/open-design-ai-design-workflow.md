---
title: Open Design：把 Claude Code / Codex 變成 AI 設計工具
date: 2026-05-22
tags: [ai/workflow, ai/agent]
source: https://www.knightli.com/zh-tw/2026/05/18/open-design-open-source-claude-design-alternative/
type: article
status: processed
asset_value: 27
---

Open Design（nexu-io/open-design）是 local-first 開源 AI 設計工作台，定位為 Claude Design / Figma 的開源替代方向。透過 Web UI + 本地 daemon + Skills/Design Systems，把已有的 coding agent CLI（Claude Code、Codex、Cursor、Gemini CLI 等）接入設計工作流，生成網頁、PPT、海報、影片等製品，存入本地 SQLite，沙盒 iframe 預覽。

**觀察中**：目前版本 0.8.0-preview，快速迭代中。使用者需熟悉 Node/pnpm/daemon/CLI，適合技術型設計師/開發者。核心價值是「把設計生成從單次 prompt 推向結構化工作流」。等生態更成熟後可重新評估建立完整 wiki 條目。
