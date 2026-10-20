---
title: AI-Ready 設計文件管理（Markdown + MkDocs + GitHub Pages）
date: 2026-06-17
tags: [ai/agent, pkm/workflow]
source: https://codelove.tw/@tony/post/q5n80x
type: idea
status: processed
asset_value: 45
---

透過 GitHub 管理 Markdown 設計文件，用 MkDocs Material 自動生成 HTML 網站並部署到 GitHub Pages，讓設計文件同時對 AI（低 MCP 成本）和人類（PR 差異清晰）都友善。

**核心問題：** Confluence 等傳統工具讓 AI 透過 MCP 存取成本高、圖表與文字難以一起處理、diff 無法用 code review 流程審查。

**技術架構：**
```
docs/**/*.md        ← 設計文件本文（Markdown）
docs/assets/images/ ← 圖片（Mermaid 嵌入 md，截圖放此）
mkdocs.yml          ← 主題/導覽/Markdown 擴充設定
.github/workflows/pages.yml ← GitHub Actions 自動部署
```

**PR Preview 機制：**
- main merge → 更新 production
- PR → 生成 preview 網站 `/{repo}/previews/pr-{PR編號}/`
- workflow 自動把 preview URL 貼到 PR 留言

**圖表選擇：** 優先 Mermaid（GitHub 原生顯示，可追蹤 diff）；截圖/既有資料放 `docs/assets/images/`

**啟發點：** 這個「Markdown 設計文件 → HTML 公開 + AI 讀取」的模式，可以應用於個人知識庫或 Wiki 的 AI 讀取入口設計。

**相關：** [[agents-md-guide]]（AGENTS.md 設計，目的類似）、[[planning-with-files]]（文件驅動 AI 開發方法論）
