---
title: Hermes Agent 完整架構解析：從設計原則到 14 天自建清單
date: 2026-06-24
tags: [ai/agent, dev/architecture]
source: https://codelove.tw/@tony/post/aw0oyq
type: article
status: processed
asset_value: 76
---

Hermes 的差異化在於**封閉式學習迴圈**：解決問題時自動撰寫可重用的 Skill 文件 + 整理持久記憶，越用越強。本文補充現有 `hermes-agent.md` 的記憶層說明，聚焦完整架構設計、工具/Skill 系統，與 14 天自建清單。

## T（核心設計哲學）

**程序式記憶 > 聰明的提示詞**：大部分「智慧型代理人」的行為，不是來自提示詞工程，而是代理人擁有一個 markdown 文件資料夾（Skills + 記憶 + 人格），並能隨時間讀、寫、成長。

六大設計原則：
1. **與平台無關的核心**：Agent 不知道自己在 CLI / Telegram / cron 哪一個介面，平台細節全在 adapter 層
2. **提示詞穩定性**：System prompt 在 Session 開始時一次組裝完成，對話中不變動——命中 Prompt Cache，成本降 10 倍
3. **漸進式揭露**：描述（第 0 層）→ 完整 Skill（第 1 層）→ 參考檔案（第 2 層）；這是能支援 47 個工具的方法
4. **自我註冊勝過中央清單**：工具 import 時自行 `registry.register()`，新增工具不需修改其他檔案
5. **設定檔隔離**：每個 Agent 實例有自己的 `HERMES_HOME` 目錄，支援多實例
6. **Agent 自己擁有自己的學習素材**：Skill 由 Agent 在解決任務後自動撰寫（`skill_manage`），不是人手動加入

## B（三層架構）

```
第 1 層：介面（Surfaces）
CLI / TUI / Gateway（TG/Discord/Slack）/ ACP / Web UI / Cron / Batch

第 2 層：Agent 核心
AIAgent 類別 → build_system_prompt → 呼叫模型 → 派發工具呼叫 → 持久化
    ├── 工具註冊表（Tool Registry）
    ├── Skill 載入器
    ├── 記憶管理器
    └── 模型供應器（4 種 API 模式）

第 3 層：執行後端
本機 / Docker / SSH / Modal / Daytona
```

## R（四個核心子系統）

### 工具系統

- **自我註冊式 Registry**：工具 import 即自動掛載，無需中央清單
- **四類工具集**：Filesystem / Browser / Data / Code execution
- **MCP 整合**：MCP Server 作為工具池的一部分，可動態載入
- **分層安全**（防 Prompt Injection）：
  - Layer 0：危險動作用 XML 呼叫而非文字（強制解析）
  - Layer 1：Tool Approval（由使用者確認）
  - Layer 2：Tool Sandbox（沙箱執行環境）

### Skill 系統（最關鍵功能）

- **Skill 就是 Markdown 文件**，放在 `~/.hermes/skills/` 下，任何工具能讀寫
- **3 層漸進式揭露**：
  - 第 0 層（Synopsis）：`# title` + 1 行描述 → 常駐 System Prompt（極少 Token）
  - 第 1 層（Overview）：完整說明 → Agent 認為需要時拉入
  - 第 2 層（Reference）：附加文件、程式碼 → Skill 本身要求時才載入
- **條件式啟用**：Skill 的 `when:` 欄位定義觸發條件，避免無關 Skill 佔用 Context
- **自我改進**：`skill_manage` 工具讓 Agent 在任務完成後自動建立/更新 Skill，每次解決同類問題變得更快

### 記憶系統（3 種機制）

| 機制 | 實作 | 用途 |
|------|------|------|
| **凍結快照** | `MEMORY.md` + `USER.md` | 跨 Session 的使用者偏好、決策記錄 |
| **SessionDB 回憶** | SQLite WAL + FTS5 | 跨 Session 的對話歷史檢索 |
| **可插拔供應器** | Honcho / mem0 / supermemory | 企業/雲端場景的外部記憶後端 |

### Agent 迴圈（關鍵實作細節）

```
接收輸入 → 組裝 System Prompt（一次）→ 呼叫模型 → 解析回應
    ├── 有工具呼叫 → 逐一派發 → 附加結果 → 回到呼叫模型
    └── 最終文字 → 顯示 → 持久化 → 完成
```

重要細節：
- **迭代預算**：`execute_code` 執行後退還迭代次數，程式迴圈不耗盡預算
- **壓縮 vs 截斷**：Context 滿時做摘要（有資訊損失），不直接丟棄歷史
- **Ctrl-C 中斷**：乾淨取消工具、附加「使用者中斷」結果，不殺整個迴圈

## C（14 天自建清單，9 個階段）

| 階段 | 天數 | 工作 |
|------|------|------|
| **1. 迴圈** | 第 1-2 天 | `while not done` + 工具呼叫派發基礎結構 |
| **2. CLI** | 第 3 天 | 讀 stdin、輸出 stdout，加 `--verbose` |
| **3. 工具 Registry** | 第 4-5 天 | 自我註冊機制 + 基本工具（read/write/bash）|
| **4. 記憶與人格** | 第 6 天 | `MEMORY.md` + `USER.md` + Snapshot 機制 |
| **5. Skill 系統** | 第 7-10 天 | 漸進式揭露 3 層 + `skill_manage` 自我改進 ← **魔法所在** |
| **6. Prompt Caching** | 第 11 天 | 提示詞穩定性設計，命中 Cache 降 90% 成本 |
| **7. Gateway** | 第 12 天起 | Telegram / Discord adapter（薄層接入）|
| **8. MCP** | 第 14 天起 | MCP Client 整合，接入外部工具池 |
| **9. 設定檔與打磨** | 後續 | 多 Agent 實例隔離、Cron 排程、ACP |

## 相關連結

- [[hermes-agent]] — Hermes Agent 記憶層詳解（Episodic/Skill/Honcho）
- [[ideas/hermes-agent-desktop]] — Hermes Desktop GUI 版本
- [[agentmemory]] — 外部記憶伺服器（可替換 Hermes 內建記憶）
- [[loop-engineering-6-components]] — Loop Engineering 框架（Hermes 的 Skill 系統是最佳範例）
- [[agent-framework-stability-comparison]] — 四個框架穩定性哲學比較
- [[synthesis/synthesis-agent-framework-philosophy]] — Agent 框架設計哲學知識精粹（本主題 4 篇整合）
- [[synthesis/synthesis-agent-engineering-architecture]] — Agent 工程架構知識精粹（8 篇整合，本篇同時服務兩個精粹的不同角度）
