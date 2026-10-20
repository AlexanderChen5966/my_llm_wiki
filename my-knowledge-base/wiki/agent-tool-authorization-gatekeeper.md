---
title: 工具授權四狀態：allow/deny 是可達性，不是授權
date: 2026-08-17
tags: [ai/agent, dev/architecture]
source: https://codelove.tw/@tony/post/31oZ8q（譯自 https://dev.to/debashish_ghosal/i-stopped-trusting-ai-agents-with-tools-so-i-built-a-gatekeeper-26fb ；專案 github.com/deghosal-2026/agent-tooltrust）
type: article
status: processed
asset_value: 82
---

## T（核心論點）

> **大家都在競相替 AI 代理提供更多工具，幾乎沒有人在打造決定這些工具何時該觸發的權限系統。**
> **目前代理權限只有二元：允許或拒絕——那是可達性（reachability），不是授權（authorization）。**

因為情境決定風險：**同一個工具在測試環境無害，在正式環境卻很危險；同一個讀取動作在公開文件上沒問題，在客戶資料上卻很敏感。CI 沙箱裡的 `delete` 和正式環境裡的 `delete` 不是同一件事。**

而分工原則說得比多數文章都乾淨：

> 🔑 **LLM 負責提案，政策負責裁決。再怎麼調提示詞都不能推翻拒絕——因為引擎在模型外部，而不是藏在提示詞裡。**

## B（適用場景）

**適用：**
- 自建 agent 要設計工具授權層（四狀態設計與 fail-closed 可直接照抄）
- 已有 OPA/Rego 政策的團隊想接上 agent
- **對 agent 做實地測試（field test）的方法論**——本篇這部分的價值不亞於引擎本身

**不適用 / 注意：**
- ⚠️ 這是單一作者的 v0.1.0 開源專案（`pip install agent-tooltrust`），**平台級功能（升級往返、政策包、HTTP `/authorize`、重放偵測、子代理委派）仍是 v0.2.0 的 32 個開放議題**
- ⚠️ 作者自己最不確定的是 **`audit` 這個狀態是否過度設計**（他在文末直接把這個問題丟給讀者）
- ⚠️ 文中的產業數據（18%／80%）未附出處連結，引用時應標明來自該文轉述

## R（四狀態決策與五階段流程）

工具呼叫執行前跑五階段——**正規化 → 評分 → 決策 → 解釋 → 稽核**——回傳四種決策之一：

| 決策 | 意義 |
|---|---|
| `allow` | 放行 |
| `audit` | **允許，但全部記錄**（例如對敏感資料的讀取）|
| `escalate` | **先停下來，找人核准** |
| `deny` | 拒絕 |

🔑 **為什麼不是兩種：二元的允許／拒絕會逼你在「過度授權的代理」和「審核疲勞」之間二選一。四種狀態提供了中間地帶。**

情境由 `CallContext` 承載，關鍵是三個維度的組合：

```python
@dataclass(frozen=True)
class CallContext:
    tool_name: str
    action: str          # read / write / delete / deploy …
    environment: str     # staging / production …
    data_class: str      # public / internal / restricted …
    agent_id: str
    session_id: str | None = None
    arguments: dict | None = None
```

整合點是**裝飾器**：代理呼叫工具 → 引擎攔截評估 → 放行／稽核／升級／拒絕。🔑 **代理永遠看不到政策，LLM 也不知道規則存在。**

```python
@adapter.guard(tool_name="deploy_service", action="deploy",
               environment="production", data_class="restricted")
def deploy_service(service: str) -> str: ...
# → ToolTrustDecisionError: escalate —「在正式環境中對受限資料執行寫入動作需要核准」
```

### 🔴 全程 fail-closed，而且是第一行程式碼之前就寫下的設計決策

未知工具 → 拒絕；輸入格式錯誤 → 拒絕；**引擎當掉 → 拒絕**。

> **替代方案是 fail-open，也就是攻擊者只要讓引擎崩潰，就能無限制存取工具。**

⚠️ 作者特別強調這是 **DD-14「在第一行程式碼之前就先寫下來，而不是在差點出事之後才補的」**——與 [[agent-permission-and-os-sandbox]] 的「沒有人可以問的時候拒絕」是同一個原則。

### 其他設計面

- **每個決策都附解釋**（原因碼＋人類可讀句子＋顯示哪個維度驅動判定的因素拆解）；⚠️ **可選的 LLM 文案預設關閉，且 LLM 無法改變決策**
- **每個決策都被稽核**（JSONL／SQLite／Postgres，含政策版本、時間戳記、session ID）
- **三種預設安全姿態**（strict／balanced／permissive），**沒有人要從空白檔案開始**
- **雙後端**：YAML（給人看）＋ OPA/Rego（給已有政策的團隊）
- 🔑 **shadow mode**（`dry_run=True`）：先部署、觀察原本會被拒絕的內容、調整、再正式強制——**而且不用改代理程式碼**

## R（🔑 deny → replan → allow：代理不是被阻擋，而是被導向）

作者自稱最滿意的結果：代理嘗試 `drop_database` → 引擎拒絕 → **好的代理不會就此停下，它會重新規劃** → 改呼叫 `query_audit_log` → 引擎允許 → **兩次呼叫都被稽核**。

實測 **8/8 個 LLM 框架都成立**（8/8 live、8/8 scripted）：每個框架都拒絕了破壞性呼叫、重新規劃成無害的讀取並獲得允許。

> **代理不是被擋下來，而是被導向。每一步都在稽核軌跡上。**

（這與 [[agent-error-recovery-taxonomy]] 的「Permission Denied → Alternative Path or Stop」正好對上：那邊是策略分類，這邊是實測證據）

## C（關鍵行動）

- [ ] 檢查自己的工具權限是不是只有二元白名單——有沒有把 environment × data_class × action 納入判斷？
- [ ] 授權層一律 **fail-closed**，並在寫程式之前就把這條寫成設計決策
- [ ] 導入時先跑 **shadow mode** 觀察會被拒絕什麼，再開強制
- （2026-10-08 補，作者觀點）決策模型版本（aiposthub，2026-10-02）：confidence 再高也不能替代操作權限——權限、工具範圍、確認要求由程式執行；決策模型只負責「選」。→ [[decision-model-ecosystem]]（同作者在 Decisions API 文中也寫過「選擇≠授權」，不算兩個來源）

## 補充資料

### 🔑 本篇的另一半價值：實地測試方法論

作者前三個專案學到同一件事——**「模擬代理會說謊。單元測試會通過。Demo 看起來很乾淨。然後真正的代理一跑起來，一切都壞掉了。」**

這次的做法是**零模擬代理**：從 GitHub 找來 **83 個真實代理**（有真實相依性與封裝方式）、10 個框架 adapter（LangGraph、PydanticAI、CrewAI、OpenAI Agents SDK、Google ADK、AutoGen/AG2、LlamaIndex、smolagents ＋ 2 個自我測試）、30 個情境（20 決策 ＋ **10 個對抗情境**：prompt injection、Unicode 混淆、重放攻擊、空白工具名稱、格式錯誤輸入、繞過授權）。

**模擬代理不會做的事**（每一項都是會被帶上線的 bug）：帶著 ABI 不相容的 C extension／在模組層級硬寫 API key／**在 import 的時候往 `/root` 寫東西**／花 80 秒等一次 LLM 回應。

### 🔑 涵蓋式設計：把 2,490 次壓成 206 次（12 倍）

理論上 83 代理 × 30 情境 = 2,490 次執行（每次呼叫本機 `Qwen3.5-4B-4bit`，30–80 秒，約 10 worker 跑 2.7 小時）。⚠️ **實務上要除錯，實際 LLM 呼叫是理論值的 4–5 倍，超過 10,000 次。**

作者的關鍵洞察是**這是涵蓋問題，不是笛卡兒積問題**——引擎正確性已由 2,490 個 deterministic 斷言（零 LLM 呼叫）驗證過，而 `Engine.evaluate()` 不在乎呼叫者是誰。實地測試真正的任務是**驗證 adapter**：

- **Plan A** — 每個代理一個情境（83 次）→ **83/83，100%**
- **Plan B** — 每個框架跑完四種決策類型＋對抗情境（123 次）→ **116/123，94%**
- 合計 **206 次而非 2,490 次，覆蓋範圍相同**（30/30 情境、83/83 代理、10/10 框架、5/5 類別）

> 🔑 **昂貴的 LLM 呼叫應該用在「只有真實代理才能證明」的部分，而不是拿來重新證明 deterministic tests 已經覆蓋的內容。**

完整交叉乘積要約 12 天，涵蓋式設計只花一個下午。作者把自己四個專案的演進寫成一條線：**跳過它 → 很晚才加上 → 從一開始就規劃 → 做最佳化。**

### 🔴 那 7 個失敗教會的事：`not-available` ≠ 政策失敗

Plan B 的 7 個失敗全是同一型態 `not-available`——**guard 根本沒觸發，因為 LLM 沒有呼叫工具**（本機 Qwen 在同時給 5 個工具時，有時直接用文字回答）。**引擎甚至沒機會做出決策。**

> **永遠不要把 `not-available` 解讀成政策失敗。它和 `unexpected-decision`（guard 有執行但引擎判錯）不一樣——只有後者才是真正的回歸問題。**

🔑 **這對 CI 至關重要**：把 `not-available` 當失敗，門檻會因模型非決定性而變得不穩定（flaky）；只在 `unexpected-decision` 時失敗，門檻才會**既嚴格又穩定**。報告建議提交一份 golden `not-available` 允許清單，**讓 CI 只在真正回歸時失敗，而不是因為 LLM 當天狀況不好**。

（⚠️ 這正是 [[ai-era-quality-assurance-test-oracle]] 說的「評估函式能不能表達正確性」在 agent 測試上的具體形態）

### 各框架的真實接線坑（12 個發現的節錄）

- **LangGraph** v1.x 的 node 不能直接呼叫 → 改成**工具進 graph 之前先包裝**
- **Google ADK** 的 LLM registry 只認 Gemini（本機模型要傳 `LiteLlm(model=f"openai/{MODEL}", api_base=...)`）；且 `InMemorySessionService.create_session()` **是 coroutine 必須 await——文件沒提，執行時環境會教你**
- **LlamaIndex** 舊版 `ReActAgent` 沒有 `.query()`／`.chat()`，要用 workflow agent，且**必須 `async for event in handler.stream_events()` 才會往前跑**——單獨 await 什麼都不會發生，**agent 會默默什麼都不做**（作者花一小時才搞懂）
- **AutoGen** 要清掉 agent ID 的連字號（`ag-01` → `ag_01`）；本機 Qwen 會用文字回答，除非明確要求「你必須精確呼叫名為 `scn_<id>` 的工具，不要跳過工具呼叫」
- **smolagents** 每個 `@tool` 都要完整 docstring 含每個參數說明，否則 `DocstringParsingException`
- **CrewAI** 需要 `litellm` 當 fallback；**OpenAI Agents SDK** 需要 `function_tool(..., strict_mode=False)` 修 pydantic 衝突

🔑 **「這些問題在模擬代理上完全看不到。只有在你跑來自真實 repo 的真實程式碼時，它們才會浮現。」**

### 引用的產業數據（⚠️ 原文未附出處）

約 **18%** 的 MCP 伺服器部署實作了某種存取範圍控管；**80%** 的組織承認代理曾做出超出預期範圍的操作；**OWASP 把代理工具濫用列為一級風險**。

### 交付狀態

2,490 項 deterministic tests、Ruff 0、Mypy strict 0、Docker 通過、SWE-bench verified、OWASP 5/10、OpenSSF Silver、分支保護、Repo 公開（v0.1.1）。

## 相關連結

- [[synthesis/synthesis-agent-engineering-architecture]] — **本篇為其來源之一**（2026-09-03 重跑，8→16 篇）：Agent 工程架構精粹，含 Capability scaffolding vs Control plane 二分法

- [[agent-permission-and-os-sandbox]] — 🔑 **同一命題的手刻版**：deny/ask/allow 三級＋fail closed＋OS 沙箱；本篇多了 `audit` 狀態與 environment×data_class 維度
- [[agent-control-layer-permission-hooks-planning]] — Capability ≠ Permission、Prompt 不是安全邊界
- [[agent-error-recovery-taxonomy]] — Permission Denied 該走 Alternative Path or Stop（本篇的 replan 迴圈是其實證）
- [[agent-tool-runtime-seven-layers]] — Tool Runtime 的七層責任分工
- [[ai-era-quality-assurance-test-oracle]] — 評估標準與實作不該進同一個最佳化迴圈
- [[mcp-stateless-architecture]] — MCP 的企業級架構（本篇引用 MCP 部署數據）
- [[agent-plugins-open-standard]] — 開放標準把信任與安全刻意留白
- [[github-skill-security-audit]] — 第三方擴充的實際安全事故
- [[production-agent-architecture]] — 生產級 Agent 的 Guardrails
