---
title: Claude Code Agent SDK 完整指南：Python 與 TypeScript 4 大核心功能教學
date: 2026-07-16
tags: [ai/agent, ai/claude]
source: https://moksaweb.com/claude-code-agent-sdk/
type: article
status: processed
asset_value: 78
---

Claude Code Agent SDK（原名 Claude Code SDK）是把 Claude Code 的完整能力包成 Python/TypeScript 函式庫的程式化介面，核心是非同步 `query()` 函式，讓開發者不需手動管理 tool_use response loop 就能建立能自主讀檔、跑指令、修改程式碼的 Agent。

## T（核心論點）

Agent SDK 與 Anthropic 原生 Client SDK 的本質差異在於「工具迴圈由誰處理」——Client SDK 給你原料（要自己維護 tool_use → 執行 → 回傳的迴圈），Agent SDK 給你整個廚房（SDK 內建處理完整 agent loop，只需宣告 `allowed_tools` 和 prompt）。四大核心功能是：工具存取控制（allowed_tools/permission_mode）、Session 管理（resume 跨輪對話）、Hooks（PreToolUse/PostToolUse/Stop 等生命週期回呼）、Subagents（多 agent 協作，`allowed_tools` 須含 `"Agent"`）。

## B（適用場景）

**適用：** CI/CD 自動化 PR 審查、批次程式碼品質稽核、多 Agent 協作系統（orchestrator + specialized subagents）、需要 session resume 的長期有狀態任務、需完整稽核日誌的生產環境部署。
**不適用 / 注意：** 唯讀分析任務不要順手加上 Bash/Edit 權限，最小權限原則；生產部署需用 API key 認證，不可用 claude.ai 一般帳號額度對外提供服務。

## R（核心設定與實戰範例）

1. **安裝**：`pip install claude-agent-sdk` 或 `npm install @anthropic-ai/claude-agent-sdk`，設定 `ANTHROPIC_API_KEY`（或改用 Bedrock/Vertex AI/Azure Foundry 三個雲端後端環境變數）
2. **範例一：基本 Bug 修復 Agent**——`allowed_tools=["Read","Edit","Bash"]` + `permission_mode="acceptEdits"` + `max_turns` 限制跑迴圈
3. **範例二：帶稽核日誌的生產 Agent**——用 `PreToolUse`/`PostToolUse` hook 攔截每次工具呼叫，比對 `BLOCKED_COMMANDS` 黑名單並寫入 audit log
4. **範例三：Session Resume 跨輪對話**——第一輪分析模組，用 `resume=sessionId` 讓第二、三輪延續完整前後文，不需重新讀檔
5. **範例四：Multi-agent 協作**——一個 orchestrator（`allowed_tools` 含 `"Agent"`）派遣 security-reviewer / performance-analyzer / test-generator 三個 subagent 平行處理，最後彙整報告
6. **範例五：完整生產環境（GitHub Actions）**——Python script + workflow yaml，PR 觸發時自動跑 code review + security scan，並在發現 critical 漏洞時讓 CI fail

## C（關鍵行動）

- [ ] 生產環境的 hook 函式務必補上完整 try/except，避免例外導致 agent 靜默通過或整個停止
- [ ] 從舊版 `claude-code` 套件遷移時，記得同步改 `ClaudeCodeOptions`→`ClaudeAgentOptions`
- [ ] 需要 Agent 主動詢問而非自行猜測時，在 `allowed_tools` 加入 `AskUserQuestion` 並在 `canUseTool` 攔截處理

## 相關連結
- [[cc-gui-intellij-claude-code-plugin]] — SDK 的實際應用案例：把 Claude Code 搬進 IntelliJ IDEA 的外掛，直接接 Agent SDK
- [[claude-agent-sdk-human-in-the-loop]] — 同主題互補篇：聚焦 `canUseTool` callback 與 Human-in-the-loop 機制的深入解析（本篇提供 5 個完整實戰範例的廣度，該篇提供 HITL 單一機制的深度）
- [[claude-code-tools]] — Claude Code 內建 Tools 使用邏輯，可對照 SDK 版的 `allowed_tools` 清單
