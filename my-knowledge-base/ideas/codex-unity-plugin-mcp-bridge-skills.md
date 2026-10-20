---
title: Codex Unity 官方外掛：31 個 Native Engine Skills 與 MCP Bridge 雙向除錯
date: 2026-09-10
tags: [ai/agent, ai/mcp]
source: https://www.aiposthub.com/openai-codex-unity-plugin-deep-dive-2026/
type: note
status: processed
asset_value: 45
---

OpenAI 於 2026/9/10 推出 Codex Unity 官方外掛（`unity-agent-plugin` v0.1.3-beta），比 Unity 官方 9/9 發布的 Claude Code Unity 外掛（29 skills）晚一天，形成雙雄對決。核心架構是 31 個模組化「原生引擎 Skills」（涵蓋 URP Render Graph、UI Toolkit、2D Tilemap、IAP/LevelPlay、多人連線、TMP 中文字型修復等）＋ Unity 6 原生 MCP Bridge，透過本機 IPC（Unix Domain Socket / Named Pipe）連接 Codex 與 Unity Editor，並支援雙向除錯閉環（自動讀取 Console 錯誤堆疊並自我修正）。可轉用判準是「把 LLM 易產生幻覺的領域（過時 API、場景/資產感知）封裝成確定性 Skills，再用 MCP Bridge 做雙向除錯」這個架構模式，不限於遊戲開發。若要深入查原文中 Codex 版與 Claude Code 版的完整功能對照表。

## 相關連結
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills 模組化＋MCP Bridge 設計模式可對照本篇架構
