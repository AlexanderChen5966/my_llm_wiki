---
title: AI Skills / MCP / Plugins / Agents 四概念完整比較（2026）
date: 2026-06-15
tags: [ai/agent, ai/mcp]
source: https://codelove.tw/@tony/post/3Kp5Ya
type: article
status: processed
asset_value: 52
---

AI 工具生態有四個經常混用的術語——Skills、MCP、Plugins、Agents——實際上各司其職，指向不同層次的事物。

## T（核心論點）

這四個概念不是競爭關係，而是**不同抽象層次**的組件：

| 概念 | 本質 | 比喻 |
|------|------|------|
| **Skills** | 方法論 / 工作流程 | 食譜（怎麼做）|
| **MCP** | 通訊協議 | USB 規格（插口標準）|
| **Plugins** | 套裝功能包 | 現成的器具組 |
| **Agents** | 執行者 | 廚師（實際動手做）|

**組合公式：** `Agent + MCP + Skills + Plugins = 自動化工作流程`

## B（適用場景）

- 看到「幫我裝 X plugin」→ 記住是在安裝**套裝能力**
- 看到「設定 MCP server」→ 是在建立**通訊管道**
- 看到「寫 Skill」→ 是在定義**工作方法論**
- 看到「跑 Agent」→ 是在指定**執行者**

## R（完整內容）

### Skills：方法論

Skills 是告訴 AI「如何做某件事」的結構化指令。

**特徵：**
- 以 Markdown 文件（SKILL.md）定義
- 包含觸發條件、執行步驟、輸出格式
- 可在不同 Agent 中重用

**例子：** `knowledge-organizer` Skill → 定義如何把 raw/ 素材整理成 wiki 條目

### MCP（Model Context Protocol）

Anthropic 推出的開放協議，定義 AI 和工具之間的溝通格式。

**特徵：**
- 協議層，不是工具本身
- Server/Client 架構（AI 是 Client，工具提供 Server）
- 任何工具只要實作 MCP Server，AI 就能使用它

**例子：** Obsidian MCP Server → Claude 透過 MCP 讀寫 Obsidian Vault

### Plugins：套裝功能包

打包好的能力組合，通常包含 Skills + Connectors + 工作流程。

**特徵：**
- 一次安裝多個能力
- 針對特定工作角色優化（Figma Plugin、Spreadsheets Plugin）
- Codex Plugins 格式：`skills/` + Connectors + `plugin.json`

**例子：** Codex 的 Product Design Plugin → 打包了設計流程 Skills + Figma Connector

### Agents：執行者

實際執行任務的 AI 實體，可以使用 Skills 和 MCP 工具。

**特徵：**
- 有記憶（短期 context + 長期 memory）
- 可以規劃多步驟任務
- 可以呼叫 tools / 使用 MCP / 執行 Skills

**例子：** Claude Code = Agent（使用 Bash/Edit/Read 等 MCP tools，執行 knowledge-organizer Skill）

### 組合使用示例

```
Claude Code（Agent）
  + knowledge-organizer（Skill）
  + Obsidian MCP Server（MCP）
  + raw-pipeline（自訂 Plugin）
= 自動知識庫整理系統
```

## C（關鍵行動）

- [ ] 建立自己常用的 Skills（SKILL.md），把重複工作流程結構化
- [ ] 評估哪些工具需要設定 MCP Server（優先：Obsidian、GitHub）
- [ ] 參考 Codex Plugin 格式，把現有 Skills 包成可重用的 Plugin

## 相關連結

- [[skill-md-9-types-and-gotchas]] — SKILL.md 九種類型（Skills 怎麼寫）
- [[loop-engineering-6-components]] — Loop Engineering 六元件（Skills + MCP + Agents 組合框架）
- [[codex-plugins-12-guide]] — Codex 12 個插件指南（Plugins 實際案例）
- [[claude-cowork-setup-guide]] — Claude Cowork 設定（Agent 自動化實作）
- [[custom-gpt-gems-to-skills-migration]] — 自訂 GPT／Gems 遷移到 Skills 的跨廠商對照（2026-10-08 快照）
