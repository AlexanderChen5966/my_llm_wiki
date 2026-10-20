---
title: Codex with ChatGPT：Control／Data Plane 分離，讓 Reviewer 自己去看 diff
date: 2026-09-01
tags: [ai/agent, ai/mcp, ai/workflow]
source: https://tools.wingzero.tw/articles/4041（2026-08-29）／專案 https://github.com/XiaoDuoYa/codex-with-chatgpt
type: reference
status: processed
asset_value: 72
---

一個把 ChatGPT 網頁版與 Codex 橋接起來的開源專案。**它不是另一個 Coding Agent，而是一個讓兩個 AI 工作環境分工合作的橋接層。**

## T（核心論點）

一般雙 Agent Workflow 有個結構性缺陷：

> 🔴 **執行 Agent 修改完程式後只回報「已經完成，所有測試通過」，Review Agent 實際上只是在審查執行 Agent 自己寫的摘要。**

這個專案的解法是給 Reviewer 一條**獨立的唯讀資料通道**，讓它自己去看真正的 `git_diff` 與 test records，而不必相信執行方的自述。架構因此從

`AI A → 告訴 AI B 結果`

變成

> 🔑 `AI A 執行 → AI B 獨立檢查實際結果`

## B（適用場景）

**適用：** 已訂閱 ChatGPT Plus／Pro 但 Coding Agent 額度另計；想把 Planning／Review 與 Execution 分到不同環境；設計雙 Agent workflow 時想避免「reviewer 只看摘要」。

**不適用 / 注意：**
- 🔴 **這不等於「Codex 從此完全不消耗額度」**——專案介紹自己就說清楚了。Codex 仍是實際 Coding Harness，仍要執行修改、測試與修正。**它改變的是工作分配，不是把 Codex 替換掉。**
- ⚠️ Control Plane 靠 Computer Use 操作 ChatGPT 網頁版，屬於**依賴 UI 的整合**，網頁改版即可能失效。
- ⚠️ 走網頁版訂閱而非 API，是否符合服務條款需自行確認。

## R（操作步驟）

### 一、整體架構

```
需求 → ChatGPT 規劃 → Codex 執行 → ChatGPT Review → Codex 修正 → 完成
```

| 角色 | 負責 |
|---|---|
| **ChatGPT**（網頁版訂閱） | Reason、Plan、Review |
| **Codex Harness** | Edit、Git、Shell、Tests、Fix |
| **C2C Bridge** | 兩者之間的溝通 |

### 二、🔑 Control Plane / Data Plane 分離

這是這個專案最值得抄的設計：

| | Control Plane | Data Plane |
|---|---|---|
| 傳什麼 | **非常小的結構化狀態訊息** | 程式碼、目錄、diff、測試狀態 |
| 狀態機 | `INIT → PLAN → EXECUTED → REVIEW → DONE` | — |
| 機制 | Computer Use | 唯讀 MCP |
| 明確**不**傳 | 🔑 **Diff、Log 與完整 File Body 不透過 Control Plane 貼來貼去** | — |
| 取用方式 | — | 🔑 **ChatGPT 主動取得自己需要的資訊**，而不是要求 Codex 把大量程式碼複製到聊天視窗 |

> 🔑 這個分離同時解決兩件事：**狀態同步不必付上下文代價**，而**審查不必依賴被審查方提供材料**。

### 三、C2C Bridge 的 8 個唯讀工具

| 工具 | 用途 |
|---|---|
| `workspace_info` | 取得目前 Workspace 基本資訊 |
| `list_directory` | 查看目錄結構 |
| `read_file` | 讀取需要的程式碼 |
| `search_workspace` | 搜尋 Workspace |
| `git_status` | 檢查 Git 狀態 |
| `git_diff` | **查看實際程式修改** |
| `test_status` | **取得測試狀態** |
| `execution_summary` | 查看 Codex 執行摘要 |

### 四、🔑「唯讀」是整套架構的安全邊界

ChatGPT 可以讀程式碼、搜尋 Workspace、看 git diff 與 test status，**但不能透過這個 MCP Bridge 修改檔案**。真正具有 Edit／Git／Shell／Tests 執行能力的仍然只有 Codex Harness。

> 即使 ChatGPT 負責規劃，它也不會直接取得本機專案的寫入權限。

並且 MCP Bridge 受 **OAuth 保護**。

### 五、成本訴求與其誠實邊界

專案作者的問題意識：部分使用者已付費訂閱 ChatGPT Plus／Pro，但 Coding Agent 在 Planning、Reasoning 與 Review 階段仍持續消耗另外的 API 或 Codex 資源。因此把偏「思考」的工作交給已訂閱的 ChatGPT Web，Codex 專注在真正需要操作開發環境的工作。

⚠️ 但如上所述，這是**重新分配**而非淨消除。

## C（關鍵行動）

- [ ] 設計任何雙 Agent workflow 時先問：**reviewer 看到的是實際狀態，還是執行方寫的摘要？**
- [ ] 需要跨環境同步狀態時，把「狀態訊息」與「資料查詢」拆成兩條通道，別讓 diff 走狀態通道
- [ ] 給 reviewer 的資料通道設成**唯讀**，寫入權限留在單一執行方
- [ ] 評估這類工具的省錢效果時，扣掉「執行端仍要消耗」的部分再比較
- [ ] 依賴 Computer Use 操作網頁 UI 的整合，要有網頁改版即失效的預期

## 相關連結

- [[agent-bug-fix-eval-green-is-not-correct]] — 🔑 **本篇正是該條目問題的架構解**：「綠燈不等於改對，沒有驗證它就會自信地猜」——這裡讓 reviewer 自己查 test records
- [[loopx-stateful-control-plane]] — Control Plane 的既有條目，可與本篇的狀態機（INIT→PLAN→EXECUTED→REVIEW→DONE）對照
- [[multi-agent-cli-bridge]] — 跨 CLI 工具的橋接做法
- [[ai-pair-skill]] — 三方審查（GPT／Gemini／Claude）共讀單一 REVIEW_FILE 的人為編排版本
- [[model-fusion-dual-ai-decision]] — 雙 AI 對帳法：只用在不可逆決策、要用不同廠牌
- [[concepts/agent-workflow-taxonomy]] — 本專案屬於「③ 工作流編排（人為）」層
- [[agent-tool-runtime-seven-layers]] — 唯讀工具集作為 Permission Gate 的實例
- [[codex-advanced-tips-11]] / [[codex-plugin-workflow]] — Codex 側的既有整理
- [[deepclaude-cost-optimization]] / [[llm-cost-optimization-strategies]] — 成本優化的其他路線
