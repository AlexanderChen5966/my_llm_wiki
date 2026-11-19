---
title: Google ADK 2.0：從 Agent 中心到 Graph 中心
date: 2026-05-28
tags: [ai/agent, dev/architecture]
source: https://medium.com/@simon3458/google-adk-2-0-beta-version-2026-757c0e8d6968
type: article
status: processed
asset_value: 48
---

## T（核心論點）

ADK 2.0 把多 Agent 協作的控制流從「LLM 口頭協調」搬進「程式碼圖結構」：流程不再是 prompt，而是 `Workflow(edges=[...])` 決定的有向圖，LLM 只負責節點內的智慧，圖負責流程的紀律。

## B（適用場景）

**適用：** 需要精確控制平行、路由、迴圈、human-in-the-loop 的生產級 Agent pipeline；希望流程可測試、可審核、行為確定性高的場景。

**不適用 / 注意：** ADK 2.0 目前仍是 Beta，且 session 與 ADK 1.x 不相容。純探索型、流程彈性需求高的 demo 仍適合 1.x。

## R（操作步驟）

1. 用 `from google.adk import Agent, Workflow, Event` 取代 1.x 的 `LlmAgent`
2. 在 `Workflow(edges=[...])` 中明確定義節點與邊（平行用 fork，合流用 `JoinNode`，條件分支用 edge condition）
3. 節點可以是純 Python function（`yield Event(...)`），不需要一定是 Agent
4. 迴圈（需多次審查才通過的場景）直接寫在 edges，不靠 prompt 哄 LLM

## C（關鍵行動）

- [ ] 確認是否需要 Beta 穩定性保證，或可接受 ADK 2.0 的不相容風險
- [ ] 把原本靠 prompt 控制的「如果不通過就重試」邏輯，改寫為 Workflow edge condition

## 原始摘要

Google ADK 2.0 Beta 研究（Simon Liu）：ADK 1.x 以 `LlmAgent` 為核心，讓協調者 LLM 自己決定 transfer 給哪個 sub-agent，流程隱式、不可靠；ADK 2.0 改為「圖中心」，`Workflow(edges=[...])` 把平行、路由、迴圈全部寫死在程式碼，LLM 只在節點內決策。節點可以是純 function，是一等公民。

## 補充資料

**2026-11-19 補充（來源：AI 郵報 Philo〈Google ADK2.14.0 穩定化執行介面：多次評測、工具比對與升級限制怎麼看？〉https://www.aiposthub.com/google-adk-python-214-multi-run-evaluation-upgrade/ ，2026-11-09）**

- 2.x 已走到 **2.14.0（2026-11-07）**：本機／E2B／Daytona 執行環境、A2A 整合（CLI 可產生 agent card）轉穩定；檔案工具可分塊串流讀。官方自報 30 節點、200 回合工作流快 23%、尖峰記憶體少 9%（單一流程，未核對）。新增 TeamsRunner 接 Microsoft Teams
- ⚠️ **升級不是零修改**：16 項轉穩定的工具集不再受舊的實驗功能停用旗標阻擋（且無警告）；RunSkillScriptTool 不再收任意 `command` 字串，只收經驗證的腳本路徑＋參數；列舉型工具參數改傳 enum 成員（比較要用成員或 `.value`）
- 🔴 **回退要連資料一起規畫**：Firestore 新序列化寫過的會話，2.13.0 以前讀不了（追加事件也算新寫入）；PostgreSQL 新表 JSON 欄改 `json`、舊 `jsonb` 表照舊
- 多次評測與合併規則的細節見 [[agent-eval-regression-testing]] 補充資料

## 相關連結

- [[agent-framework-stability-comparison]]
- [[agentic-engineering-patterns]]
- [[synthesis-agent-framework-philosophy]]
