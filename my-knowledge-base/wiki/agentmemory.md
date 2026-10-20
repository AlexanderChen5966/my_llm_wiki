---
title: agentmemory：AI Coding Agent 的跨 session 持久記憶伺服器
date: 2026-05-28
tags: [ai/agent, ai/claude, dev/architecture]
source: https://tools.wingzero.tw/article/sn/3926
type: article
status: processed
asset_value: 64
---

## T（核心論點）

agentmemory 是放在各種 AI coding agent 背後的「共享記憶後端」——讓 Claude Code、Cursor、Codex CLI 等工具在不同 session 之間共用同一套持久記憶，自動捕捉專案脈絡、技術決策、bug 修復紀錄，解決「每次開新 session 就要重新解釋一切」的根本問題。

## B（適用場景）

**適用：**
- 長期維護同一個專案，且頻繁使用 AI coding agent
- 需要在 Claude Code、Cursor、Gemini CLI 等工具之間切換的開發者
- 多 agent 協作場景（讓不同 agent 共享同一套知識）

**不適用 / 注意：** 需具備 Node.js、CLI、MCP、API 串接工程背景；短期或一次性 AI 輔助開發顯得功能過重。

## R（操作步驟）

1. 快速啟動：`npx @agentmemory/agentmemory` 或全域 npm 安裝
2. 啟動 server 後開啟 `localhost:3113` 查看 memory build 狀態（即時 viewer）
3. 選擇接入方式：**MCP server**（推薦 Claude Code）/ REST API / agent-specific hooks
4. 執行 `demo seed sample sessions` 驗證記憶建立
5. 多工具共用：Claude Code、Cursor、Codex CLI 各自設定接入同一 agentmemory server

## C（關鍵行動）

- [ ] 評估是否符合「重度 AI coding workflow」使用情境（偶爾用 AI 寫小段程式碼不值得）
- [ ] 優先選擇 MCP 接入（比 hooks/REST API 更原生整合進 Claude Code）
- [ ] 確認 `CLAUDE.md` 靜態記憶與 agentmemory 動態記憶的分工：靜態規範放 CLAUDE.md，動態工程知識交給 agentmemory

## 原始摘要

agentmemory 與一般工具的差異：CLAUDE.md / .cursorrules 是靜態規則檔，需人工維護且易過期；agentmemory 在 agent 工作過程中**自動**捕捉操作紀錄並壓縮成可搜尋記憶。與 mem0、Letta/MemGPT 相比，強調混合搜尋（BM25 + 向量 + graph）、多 agent 協調、self-hosted 預設、不綁定特定 agent runtime。底層：SQLite + iii-engine，無需額外外部資料庫，Apache-2.0 授權。

## 深度分析補充（Slow Signals，2026-05-26）

### 成本比較

| 方式 | tokens/年 | 費用（台幣換算）|
|------|-----------|--------------|
| 全部歷史塞進 context | 19.5M+（超出限制）| 無法估算 |
| LLM 摘要式記憶 | ~650K | ~NT$16,000/年 |
| agentmemory | ~170K | ~NT$320/年 |
| agentmemory + 本地 embedding | ~170K | NT$0 |

**92% fewer tokens** 的前提：你本來靠摘要/重貼背景維持工作連續性。

### 混合搜尋架構：BM25 + Vector + Graph + RRF

三路同步搜尋：
- **BM25**：字面匹配（錯誤訊息/config key/檔案路徑）
- **Vector**：語意近似（架構決策/偏好/踩坑經驗）
- **Graph**：關聯圖譜（把「Redux 決策」連到當時的 session/PR/踩雷脈絡）

**RRF fusion（Reciprocal Rank Fusion）**：三路排名重新加權合成，最終給 agent 的是「三方都覺得相關」的精選清單。

> 純向量檢索給你「相關」的東西；hybrid + graph 給你「相關 + 可追溯 + 有脈絡」的東西。

### 關鍵洞見：儲存不是記憶，可召回才是記憶

你塞 300 條進去，不代表有用。有用的是：下次你問「上次為什麼不用 Redux？」時，它能精確撈回「2026 年 3 月 SSR hydration mismatch 的決議脈絡」——不是把垃圾堆全翻給你。

> **儲存不是記憶。可召回才是記憶。**

### 與其他工具定位比較

| 工具 | 定位 | 適合誰 |
|------|------|--------|
| mem0 | 企業記憶 API 平台（自架路線較弱）| 企業整合、雲端優先 |
| Letta（MemGPT）| 完整 agent runtime | 需要換整套架構的場景 |
| Khoj | 個人知識搜尋 second brain | 不專為 coding agent 跨 session 設計 |
| Hippo | 生物學式記憶（decay/consolidation/sleep）| 「forget by default，earn persistence」哲學 |
| **agentmemory** | 本地、跨多 agent、npm install 即用 | 每天用多個 coding agent 的開發者 |

### 7 天上手節奏

| 天 | 操作 |
|----|------|
| Day 1 | `agentmemory demo`：種範例 sessions，看召回長什麼樣 |
| Day 2 | 刻意種一條「以後必被問到」的決策（要越具體越好）|
| Day 3 | 開新 session，問同樣問題，驗證召回是否成功 |
| Day 4 | 打開 `localhost:3111`，看記憶的結構（confidence/lifecycle/source）|
| Day 5 | 接第二個 agent，驗證跨工具共享 |
| Day 6-7 | 日常使用，觀察「沒重講」的時刻 |

### 已知限制與風險

1. **個人維護**：作者 rohitg00，社群熱度在但非大公司產品，不建議放進公司 critical workflow
2. **v0.9.x 快速迭代**：`package.json` 鎖定版本，不要用 `^0.9.0` 自動跳版
3. **記憶治理需人工關心**：confidence/lifecycle 有機制，但改變主意後舊決策需手動更新
4. **Benchmark 是作者自跑**：LongMemEval-S R@5 95.2% 是 retrieval-only（500題/48 sessions/題），非第三方驗證

### 社群規模（截至 2026-05-26）

16.2K⭐ GitHub，npm 月下載 37K，Reddit r/WebAfterAI 爆文 461 分（2026-05-17 發文，5天達成）

## 相關連結

- [[claude-code-context-management]] — Claude Code 的靜態 context 管控（互補）
- [[claude-code-advanced-workflow]] — /loop 排程、長任務工作流
- [[hermes-agent]] — 另一種「越用越懂你」的學習型 agent（對比）
- [[harness-engineering]] — Harness 工程，agentmemory 是 Sensor 層的實作範例
- [[agent-memory-lifecycle]] — **理論框架**：Selection / Extraction / Recall / Consolidation 四層與 Memory Unit 欄位（Type・Scope・Confidence・Source・Last Verified・Expiry）；可用它檢視 agentmemory 存的是「Memory」還是「Conversation Archive」
