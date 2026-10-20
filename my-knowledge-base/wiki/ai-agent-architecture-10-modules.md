---
title: AI Agent 架構設計：10 個核心模組
date: 2026-06-15
tags: [ai/agent, dev/architecture]
source: https://juejin.cn/post/7648894966207152180
type: article
status: processed
asset_value: 72
---

以 TypeScript + LangChain + LangGraph 重構的開源 AI Agent「智語」為案例，系統性說明現代 AI Agent 的 10 個核心模組設計。**LLM 是大腦，Tools 是手腳，Skills 是技能手冊**，三者加上剩餘 7 個模組才構成完整的 Agent。

## T（核心論點）

一個完整的 AI Agent 不只是「LLM + 聊天介面」，而是由 10 個相互依存的模組組成的系統。Claude Code、Cursor、OpenClaw 等主流 Agent 工具都包含這些模組，理解它們的設計邏輯等同於理解所有主流 Agent 的底層架構。

**ReAct（Reasoning + Action）是所有模組的基礎流程**：LLM 推理 → 判斷是否呼叫 Tool → 執行 Tool → 再推理 → 直到最終答案。

## B（適用場景）

**適用：**
- 評估 AI Agent 工具（Claude Code / Cursor / Codex）的功能完整度
- 設計自己的 Agent 流程時，對照確認是否遺漏核心能力
- 理解 Skill 在 Agent 架構中的位置（Skills = 技能手冊，影響 LLM 回覆品質與工作流）
- 學習 AI Agent 開發的技術棧選擇（TypeScript + LangChain + LangGraph）

**不適用 / 注意：**
- 文章以「自製 Agent」視角撰寫，部分設計取捨（如降低 RAG 重要性）可能與企業場景不同
- LangChain/LangGraph 框架本身有學習曲線，且版本更新頻繁

## R（10 個核心模組）

| 模組 | 說明 | 關鍵設計 |
|------|------|---------|
| **LLM** | 所有決策的基礎，一切推理通過 LLM | 需支援多種 LLM API（含本地 LLM）|
| **Query Engine** | 封裝每次 LLM 呼叫 | 串流輸出 / 取消 / 錯誤處理 / 自動重試 / Rate limit |
| **ReAct Agent** | 核心流程：推理→執行→再推理 | LLM + Tools + 迴圈判斷即為 Agent 基礎 |
| **Tools** | Agent 的手腳 | read_file / write_file / exec / web_search / web_fetch / run_python |
| **Skills** | 技能手冊，引導 LLM 回覆品質 | Markdown 格式的工作流定義，比流程圖更靈活 |
| **Session** | 會話管理（類 Claude Code /new）| /new / /sessions / /rewind，管理歷史對話 |
| **Context（4層壓縮）** | 防止 Context 臃腫 | ① Tool 輸入壓縮到磁碟 ② 壓縮冗餘 tool message ③ AI 總結（有模板）④ 裁剪保底 |
| **Memory（3層）** | 跨 session 記憶 | 短期（當前 session）/ 長期（跨 session，時間衰減）/ Profile（使用者畫像）|
| **Permission** | 安全防護 | 4 階段：Bash 預檢 → Deny 規則 → Allow 規則 → Ask 確認 |
| **Hook** | 使用者自訂規則 | 在 tool 呼叫前後插入自訂邏輯（如禁止操作 .env 檔）|
| **SubAgent** | 子 Agent 隔離 context | 獨立 context，不影響主 Agent，完成後回報結果 |

**注意**：MCP Server 在文章中被視為 tools 的擴展方式，但文章也提到 "MCP server vs Skills+CLI" 的選擇爭議。

## C（關鍵行動）

- [ ] 對照 10 個模組，評估現有 Claude Code Skill 設計是否覆蓋 Permission（disable-model-invocation）和 SubAgent（context: fork）
- [ ] Context 4 層壓縮機制 → 對應 Claude Code 的 auto-compaction：了解 compaction 發生後哪些 Skills 會保留（每 Skill ≤ 5k tokens）

## 相關連結

- [[claude-skills-deep-dive]] — Claude Code Skills 的 Progressive Disclosure 三層機制（對應文章的 Skills 模組）
- [[agentic-engineering-patterns]] — Simon Willison 的實用 Agent 工程模式
- [[context-engineering-fundamentals]] — Context 壓縮與記憶機制的理論基礎
- [[harness-engineering]] — Guides×Sensors 框架（對應文章的 Permission/Hook 設計）
- [[hermes-agent]] — Hermes 的三層記憶設計（與文章的 3 層 Memory 對比）
- [[awesome-agent-architecture-from-scratch]] — 更完整的 22 章八層拆解，本篇 10 模組可視為其精簡版對照
