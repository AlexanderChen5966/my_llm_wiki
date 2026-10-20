---
title: Codex Plugin：可重用 AI 工作流模組系統
date: 2026-05-20
tags: [ai/agent, ai/workflow, pkm/automation]
source: https://tools.wingzero.tw/article/sn/3816
type: article
status: processed
asset_value: 50
---

Codex Plugin 把 AI 工作流能力封裝成可安裝的模組（Skills + Apps/MCP Servers），讓工具鏈和規則可以像套件一樣共享、版本化、跨專案復用。

## T（核心論點）
Plugin 的本質不是「功能擴充」，而是「工作流資產化」——把反覆使用的 Agent 工作邏輯封裝成可安裝的標準單元，讓團隊不必每個人重造輪子，也讓個人的 AI 工作流可以跨專案帶著走。

## B（適用場景）
**適用：** 想跨專案重用 AI 工作流、需要在團隊內共享 Agent 設定的場景
**不適用：** 一次性任務、只在單一專案使用的簡單 Skill（直接用 Skill 即可，不需包成 Plugin）

## R（Plugin 架構）

### 最小結構
```
my-plugin/
└── .codex-plugin/
    ├── plugin.json          # Plugin 元資料（名稱、版本、作者）
    └── skills/              # Skill 定義
        └── my-skill/
            └── SKILL.md
```

### 完整結構（含可選元件）
```
my-plugin/
└── .codex-plugin/
    ├── plugin.json
    ├── skills/              # Skill 定義
    ├── .app.json            # 自訂 App 設定（選填）
    └── .mcp.json            # MCP Server 設定（選填）
```

### plugin.json 核心欄位
```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "做什麼、適合誰用",
  "author": "your-name"
}
```

### 三種安裝來源（marketplace.json）
| 來源 | 路徑 | 說明 |
|------|------|------|
| 官方 Marketplace | 系統預設 | Anthropic 審核的高品質 Plugin |
| 專案層級 | `$REPO_ROOT/.agents/plugins/` | 只在這個 Repo 生效 |
| 個人層級 | `~/.agents/plugins/` | 跨所有專案可用 |

## C（關鍵行動）
- [ ] **先做 Skill，確認有效再升級 Plugin**：Skill 驗證工作流→打包成 Plugin→發布到個人 marketplace
- [ ] `.app.json` 用於自訂 App 行為（UI、工具白名單），`.mcp.json` 用於連接外部 API
- [ ] 用 `/plugin marketplace add` 加入來源，用 `/plugin install name@marketplace` 安裝

## 相關連結
- [[claude-skills-guide]] — Skill 設計模式（Plugin 的 Skills 層建構在此）
- [[claude-code-commands-reference]] — Plugin 管理指令速查（`/plugins`、`claude plugins`）
- [[agent-skills-vs-mcp]] — 何時用 Skill，何時需要 MCP（決定 Plugin 是否需要 .mcp.json）
