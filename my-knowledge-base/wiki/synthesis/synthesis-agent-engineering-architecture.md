---
title: Agent 工程架構 知識精粹
date: 2026-07-03
source_count: 17
synthesis_snapshot_date: 2026-09-03
tags: [ai/agent, dev/architecture, type/concept]
type: synthesis
status: processed
sources: 17 篇
asset_value: 90
---

整合 17 篇 Agent 架構條目的跨篇精粹，回答一個問題：**要讓 Agent 在真實任務中可靠，該用什麼工程結構把 LLM 的自由度約束住？**

🔴 **2026-09-03 重跑（快照 8 篇 → 17 篇，新增一整個維度）**：2026-08 的八篇新條目把原精粹缺的一塊補上了——原版把 Guardrails 當成「20 模組裡的一項」，新來源顯示**控制層是一個獨立的平面，而且它的去留邏輯與其他結構完全相反**。同時修正原 A 層殘留的錯誤四層順序。

## E（精粹論點）

**Agent 的可靠性不來自模型更聰明，而來自用工程結構把 LLM 的自由度約束住。** 先定義任務邊界（能用 Workflow 就別上 Agent），再用「狀態機 + 工具契約 + 記憶分層 + 護欄 + 回饋循環」把不確定性關進邊界——**就算模型不夠聰明，也不容易跑偏**。

**而 2026-08 的新來源把這句話推進了三層：**

**① 模型只參與第一步。**

> 🔑 **Tool Call 只是模型提出的結構化意圖——它不是執行結果，不是安全邊界，也不是完整的 Tool Runtime。**

Registry（決定模型看得見哪些能力）／Schema 驗證／Permission Gate／執行／Timeout & Error／Output Normalization／Audit & Trace **七層全屬 Harness**。「Agent 會用工具」這句話裡，模型負責的部分小得驚人。

**② 可靠性的本質，是把裁決權從提示詞裡搬出來。**

> 🔑 **LLM 負責提案，政策負責裁決。再怎麼調提示詞都不能推翻拒絕——因為引擎在模型外部，而不是藏在提示詞裡。**

配套的三個推論：**`Capability`（有沒有這個工具）≠ `Permission`（能不能對這個對象用）**；二元 allow/deny 只是**可達性不是授權**（CI 沙箱裡的 `delete` 和正式環境的 `delete` 不是同一件事，需要 `environment × data_class × action` 的情境維度與 allow/audit/escalate/deny 四狀態）；而 🔴 **Prompt 不是安全邊界**。

**③ 🔑 這些工程結構分成兩種，去留邏輯正好相反**——這是本次重跑最重要的新增判準：

| | **Capability scaffolding** | **Control plane** |
|---|---|---|
| 包含什麼 | 任務拆解、重試、Evaluator、Memory、Subagent | 權限、沙箱、audit、預算、人工核准 |
| 該怎麼決定去留 | **用 A/B 測試案例決定**——模型變強後很多可以移除 | 🔴 **不可因模型變強而移除** |
| 為什麼 | 它們在補模型的能力缺口，缺口會隨世代縮小 | 🔑 **模型變強降低「犯錯機率」，不降低「一次嚴重錯誤的影響」** |

> 這條二分法解釋了一件本庫其他條目反覆撞到的事：Anthropic 工程師點名可以移除「拆 sprint」「限制 session 長度」（capability 類），卻沒有人主張移除權限與沙箱。

**④ ⚠️ 而「加機制」不等於「更可靠」——本主題最重要的反面證據**：實測顯示**加記憶反而讓表現變差**（ALFWorld 76.4% → 70.1%，Mem0／SimpleMem 更低到 52%），問題出在「靜態回放」整個範式。**這正是上表右欄要用 A/B 決定去留的理由——沒有實測支撐就加模組，是在增加失敗面而非減少。**

## C（共識核心）
> 出現於 2 篇以上來源

- **Agent = LLM + Tools + Memory + 自主決策迴圈，缺一不可**（4 篇）——只回答不是 Agent；有工具無迴圈只是 Function Calling
- **ReAct（Thought→Action→Observation→Loop）是最基礎的循環**（多篇）——Agent Loop 執行的是「目標」非「指令」，反覆逼近直到符合停止條件
- **四種開發範式**：Workflow / ReAct / Plan-and-Execute / Multi-Agent（3 篇）——依任務路徑確定性選擇
- **先定義任務邊界，再選框架；能用 Workflow 就別硬上 Agent**（3 篇）——「邊界 > 智慧」
- **Memory 要分層，長期記憶靠向量 + 混合檢索（向量 + BM25）**（3 篇）
  - ⚠️ **但分層本身不保證有效**——見 E 層 ④ 的反面證據
- **Guardrails + Prompt Injection 防護：外部/檢索內容只能當資料，不能當指令**（4 篇，含 94.4% 攻擊成功率實測）
- **高風險動作必須 Human-in-the-loop 確認**（刪除/發送/支付）（4 篇以上）
- **可觀測性（trace）、錯誤恢復、成本控制、Sandbox 隔離** 是企業落地四大必備（4 篇）
- 🆕 **權限與執行必須在模型外部強制**（4 篇：[[agent-tool-runtime-seven-layers]]、[[agent-tool-authorization-gatekeeper]]、[[agent-control-layer-permission-hooks-planning]]、[[agent-permission-and-os-sandbox]]）——寫進 prompt 只是「期待」，寫進 Gate／Hook／OS sandbox 才是「保證」
- 🆕 **Subagent 的理由是邊界，不是並行**（[[subagent-four-boundaries]]、[[ai-agent-architecture-10-modules]] 的 SubAgent context 隔離）——多開 Agent 帶來的是更多 Token／延遲／協調／重複工作／不一致結果／權限風險
- 🆕 **「列出步驟」不等於「擁有計畫」**（[[agent-control-layer-permission-hooks-planning]]、[[production-agent-architecture]] 的 FSM）——計畫必須有可查詢的執行狀態（Pending/Running/Blocked/Failed/Completed）

## A（獨特視角）

| 來源 | 獨特貢獻 |
|------|---------|
| [[production-agent-architecture]] | 生產級 20 模組最完整清單 + 每模組「防跑偏 prompt」；Hybrid Search + semantic chunking + Query Rewrite、**idempotency_key 冪等**、Output Contract（JSON Schema）、Reranker |
| [[ai-agent-architecture-10-modules]] | 具體實作模組：**Context 4 層壓縮**（Tool 輸入落磁碟/壓冗餘/AI 總結/裁剪保底）、Permission 4 階段、Hook、Query Engine 封裝、Skills=Markdown 技能手冊、SubAgent context 隔離 |
| [[ai-agent-architecture-patterns]] | 四模式的**場景/成本對照表**、Dual-Agent 評審者模式、94.4% Prompt Injection 實測、企業落地三挑戰 |
| [[ai-agent-dev-concepts-practitioner]] | **九步開發流程**、SDK 選型表（LangGraph/OpenAI/ADK/Genkit/CrewAI）、Sandbox 類型（Docker/microVM/E2B）、工具原則「先讀→再寫→最後執行高風險」 |
| [[langchain-agent-interview-guide]] | 框架與協議層：LangChain 三層/六模組、**A2A（Agent↔Agent）vs MCP（Agent↔工具）** 分野、Transformer/訓練三階段底層原理 |
| [[agent-loop-explained]] | 最高層心法：**AI 工程化四層階梯（Prompt → Context → Harness → Loop）**、Loop=設計循環而非寫提示詞、六塊積木、PingCAP 產品化案例。🔴 **本精粹前一版把此順序誤記為 Prompt→Context→Loop→Harness，2026-09-03 已更正**（權威順序經 Osmani／codecentric／O'Reilly 三方交叉比對，Loop 在最外層）|
| [[api-cli-skills-agent-architecture]] | **唯一從「被 Agent 使用的產品端」反向設計**：API（動作層）→ CLI（橋接層）→ Skills（智慧層）；**「Skill 不取代 CLI 而是包裝 CLI，沒有好 CLI 的 Skill 只是空洞 prompt」**；**MCP vs CLI 務實對照表**；Agent-friendly CLI 五原則 |
| [[hermes-agent-architecture-deep-dive]] | **自建路線的完整拆解**：六大設計原則（提示詞穩定性命中 Cache 降 10 倍、漸進式揭露 3 層撐 47 工具、自我註冊 Registry）；**分層防 Prompt Injection**（XML 強制解析→Approval→Sandbox）；迭代預算退還、壓縮而非截斷；14 天 9 階段自建清單 |
| 🆕 [[agent-tool-runtime-seven-layers]] | 🔑 **重構了「Agent 會用工具」這句話**：Tool Call 只是工作單，Registry／Schema 驗證／Permission Gate／執行／Timeout&Error／**Output Normalization（別把幾萬行 Log 塞進 messages）**／Audit&Trace 七層全屬 Harness；**沒有 Audit 就只剩最終輸出** |
| 🆕 [[agent-tool-authorization-gatekeeper]] | 🔑 **唯一把授權當獨立系統設計**：`allow/audit/escalate/deny` 四狀態 ＋ `environment × data_class × action` 情境維度；**LLM 提案、政策裁決**；全程 **fail-closed**（引擎當掉也拒絕，寫在第一行程式碼之前）；🔑 **`deny → replan → allow` 安全迴圈 8/8 成立——代理不是被阻擋而是被導向**；方法論亮點：**涵蓋式設計把 2,490 次壓成 206 次**測試；🔴 區分 `not-available`（LLM 沒呼叫）vs `unexpected-decision`，混在一起會讓 CI flaky |
| 🆕 [[agent-control-layer-permission-hooks-planning]] | **控制層三件事的最小完整集**：`Capability` ≠ `Permission` ＋ Approval ＋ Sandbox 三道防線；🔴 **Prompt 不是安全邊界**；Hooks 承接 Logging/Validation/Business Rules 而不塞 prompt；🔑 **列出步驟 ≠ 擁有計畫**（要有可查詢的 Execution State）|
| 🆕 [[subagent-four-boundaries]] | 🔴 **Subagent ≠ Parallelism**：四層邊界（Context／Responsibility／Capability&Permission／**Result Contract 只回結論不回過程**）＋ Tool vs Subagent 判準 ＋ 平行前五問 ＋ **Context Duplication 五種緩解** |
| 🆕 [[awesome-agent-architecture-from-scratch]] | **八層架構的 22 章教材**（核心 loop→複雜工作→知識韌性→長時間非同步→多 Agent→擴充整合→組合），以 Claude Code／Hermes／mini-swe-agent 三真實系統對照，每章附可執行程式碼。🔑 **並提供本精粹 E 層③ 的 Capability scaffolding vs Control plane 二分法** |
| 🆕 [[agent-error-recovery-taxonomy]] | 🔑 **Retry 是 Recovery 的一種，不是全部**——Retry 解決暫時失敗，Recovery 解決「下一步應該怎麼改」；**全用 Retry 的系統看起來有韌性，其實只是把同一個錯誤多執行幾次**。所以 Recovery 第一步是**分類**（八種錯誤型別）而非執行 |
| 🆕 [[agent-loop-stop-conditions]] | 停止條件的三條規矩：🔑 **① 停在「日記完整」的位置**（停錯位置等於弄壞整段對話）；② 停下來要留話；③ 🔑 **錯誤訊息要先問「這句話是寫給誰看的」**——模型與人要分開對待 |
| 🆕 [[memharness-memory-reconstruction]] | 🔴 **本主題唯一的強反面證據**：加記憶反而變差（76.4%→70.1%，Mem0/SimpleMem 低到 52%），病因是**靜態回放**這個範式；解法是用之前先依當下狀態**重構**記憶（可整段拒絕 `<EMPTY>`）；最妙的消融：**訓練用重構、測試不給記憶仍 83.0%**——重構訓練順便長出推理能力 |

## V（知識缺口）

- 🔴 **「該加哪些機制」缺 A/B 方法論（2026-09-03 升級為首要缺口）**：[[awesome-agent-architecture-from-scratch]] 說 capability scaffolding 該用 A/B 測試案例決定去留、[[memharness-memory-reconstruction]] 證明加錯機制會倒退——**但沒有任何一篇說怎麼建這組 A/B 案例**。20 模組檢查表因此有個危險用法：當成「都要加」而非「逐項驗證」
- 🔴 **控制層的成本沒人量測**：四狀態授權、fail-closed 引擎、audit trail、sandbox 都要錢與延遲。[[agent-tool-authorization-gatekeeper]] 的涵蓋式設計把測試從 2,490 次壓到 206 次是唯一的效率數據，**但執行期的額外延遲與 token 成本全無資料**
- ❓ **Multi-Agent 跨 Agent 狀態同步**：多篇都說「先跑通單 Agent 再升 Multi-Agent」，但跨 Agent 狀態同步、訊息總線、A2A 實戰細節都沒講清楚
- ❓ **Agent 品質如何 eval**：production 篇要求 eval 測試集、agent-loop 提「下一步決策 51%→64%」，但**開放式決策的量化評測仍無標準方法**
  - 🆕 [[agent-bug-fix-eval-green-is-not-correct]] 補上了一個具體陷阱（**綠燈不等於改對**），但仍未給出正面方法
- ⚠️ **長期記憶的衝突/版本/生命週期管理**：production 篇點到「Memory 需時間/版本/關係/衝突/生命週期」，[[memharness-memory-reconstruction]] 與 [[external-memory-layer-exploration-tax]] 各補了一角（重構、信任分級與過時標記），但**衝突裁決與版本管理仍留白**
- ⚠️ **框架真實生產取捨缺長期數據**：LangGraph/ADK/Genkit/CrewAI 的選型多憑特性描述，缺長期生產對照
- ⚠️ **MCP vs CLI 的方向性分歧未被正面處理**：[[api-cli-skills-agent-architecture]] 主張多數場景 CLI 優於 MCP，其餘來源多把 MCP 當標準工具層預設——**沒有一篇處理「既有 MCP 生態要不要改回 CLI」的遷移取捨**
- ❓ **「自建 vs 用框架」缺成本對照**：[[hermes-agent-architecture-deep-dive]] 證明 14 天可自建核心，但長期維護（跟上模型/協定演進、安全修補）成本無人評估
- 🆕 ⚠️ **Result Contract 的粒度沒有判準**：[[subagent-four-boundaries]] 主張 subagent「只回結論不回過程」，但**結論要多詳細才夠讓上層判斷、又不至於把過程搬回來**，沒有給線

## 整合行動清單

- [ ] 設計任何 Agent 前先過三關：① 能用 Workflow 嗎？② 任務邊界與成功標準是否明確？③ 選哪種範式
- [ ] 🔑 **先把要加的結構分兩堆**：capability scaffolding（拆解/重試/Evaluator/Memory/Subagent）**要有 A/B 案例才加**；control plane（權限/沙箱/audit/預算/核准）**無條件保留，且不因換更強的模型而移除**
- [ ] 用 [[production-agent-architecture]] 的 20 模組當**逐項驗證清單**，不是「都要加」的採購單
- [ ] 🔑 **把裁決權搬到模型外部**：權限走 Gate／Hook／OS sandbox，全程 fail-closed；**別靠 prompt 當安全邊界**
- [ ] 授權設計別停在 allow/deny——至少納入 `environment × data_class × action` 的情境維度，並讓 `deny` 觸發 replan 而非直接失敗
- [ ] 工具鏈檢查七層是否到位，尤其 **Output Normalization**（別把幾萬行 Log 塞進 messages）與 **Audit & Trace**（沒它就只剩最終輸出）
- [ ] 要開 Subagent 前先問「哪一段 Context／責任／權限值得切出去」，並先定義 **Result Contract**；⚠️ 別把它當並行手段
- [ ] 錯誤處理先**分類**再決定動作——確認自己不是「全部用 Retry 假裝有韌性」
- [ ] 停止條件挑在「日記完整」的位置，並讓錯誤訊息分別寫給模型與人看
- [ ] RAG 一律 Hybrid（向量+BM25）+ semantic chunking + rerank，勿只用向量 TopK
- [ ] 高風險動作走 HITL；外部內容設 source boundary（資料≠指令）
- [ ] ⚠️ **加記憶之前先確認它有沒有讓表現變好**——靜態回放會倒退，必要時整段拒絕比硬塞更好
- [ ] 先跑通單 Agent + trace，再考慮 Multi-Agent

## 相關概念框架

- [[concepts/agent-engineering-vocabulary-layers]] — 本主題的**詞彙層級地圖**：把 Agent 工程術語歸位為範式／機制／實體／應用四層，並標出本庫覆蓋深度與三個真缺口（World Models／Long-Horizon Agent／RACG）
- [[concepts/agent-workflow-taxonomy]] — 工具層的六大類型與選型決策樹（本精粹談架構、該篇談工具）

## 來源條目

- [[production-agent-architecture]] — 生產級 20 模組工程清單（含防跑偏 prompt）
- [[ai-agent-architecture-10-modules]] — 10 核心模組（智語案例）
- [[ai-agent-architecture-patterns]] — 四大架構模式 + 安全 + 企業落地
- [[ai-agent-dev-concepts-practitioner]] — 實踐者 9 步流程 + SDK 選型 + Sandbox
- [[langchain-agent-interview-guide]] — LangChain/A2A/MCP/Transformer 全景
- [[agent-loop-explained]] — Agent Loop 概念與工程化四層階梯（Prompt→Context→Harness→Loop）
- [[api-cli-skills-agent-architecture]] — API+CLI+Skills 三層與 MCP vs CLI 對照（AV 65）
- [[hermes-agent-architecture-deep-dive]] — Hermes 自建路線：六大設計原則、漸進式揭露、分層防注入、14 天清單（AV 76）
- [[agent-tool-runtime-seven-layers]] — 🆕 Tool Call 只是工作單，其餘七層全屬 Harness
- [[agent-tool-authorization-gatekeeper]] — 🆕 四狀態授權、LLM 提案政策裁決、fail-closed、deny→replan→allow
- [[agent-control-layer-permission-hooks-planning]] — 🆕 控制層三件事：Capability≠Permission、Hooks、可查詢的 Execution State
- [[agent-permission-and-os-sandbox]] — 🆕 deny/ask/allow 與 OS 層沙箱
- [[subagent-four-boundaries]] — 🆕 Subagent ≠ Parallelism、四層邊界、Result Contract
- [[awesome-agent-architecture-from-scratch]] — 🆕 八層架構 22 章教材；**Capability scaffolding vs Control plane 二分法**
- [[agent-error-recovery-taxonomy]] — 🆕 Retry ≠ Recovery，先分類再處理
- [[agent-loop-stop-conditions]] — 🆕 停在「日記完整」的位置、錯誤訊息分對象
- [[memharness-memory-reconstruction]] — 🆕 🔴 加記憶反而變差，靜態回放範式的反面證據（AV 85）
