---
title: 四個 Agent 框架對「穩定性」的不同理解：hermes / OpenClaw / CrewAI / Claude Code
date: 2026-05-22
tags: [ai/agent, dev/architecture]
source: https://wenwender.wordpress.com/2026/04/30/%e5%9b%9b%e5%80%8b-agent-%e6%a1%86%e6%9e%b6%ef%bc%8c%e5%9b%9b%e7%a8%ae%e5%b0%8d%e3%80%8c%e7%a9%a9%e5%ae%9a%e6%80%a7%e3%80%8d%e7%9a%84%e7%90%86%e8%a7%a3/
type: article
status: processed
asset_value: 80
---

四個框架（hermes-agent、OpenClaw、CrewAI、Claude Code）在架構最底層收斂到相同的「可插拔機甲」模式，但在上下文管理、控制流設計、錯誤恢復、反饋迴路四個維度的選擇，反映了截然不同的問題意識。框架選擇的核心問題不是「哪個更好」，而是「在你的場景裡，Agent 壞掉最壞的情況是什麼」。

## T（核心論點）

所有框架都長成了同樣的機甲骨架（工具/記憶/模型/Skills/子 Agent/Plugin 可插拔），但可插拔是共識，**插什麼、怎麼插、什麼時候插**才是真正的分歧。框架之間最關鍵的差異體現在 context 滿了怎麼辦、誰決定下一步、出錯後怎麼補救、如何避免永遠停在現有能力上——這四個問題的答案直接決定了框架能在哪種場景下工作。

## B（適用場景）

**適用：** 選擇 Agent 框架時的決策依據；設計多 Agent 系統的架構討論；理解各框架的設計哲學差異

**不適用 / 注意：** 框架版本快速迭代，部分技術細節可能過時；文章為個人研究角度，非官方比較

## R（四維度比較分析）

### 共同基礎：可插拔機甲模式

| 能力層 | hermes | OpenClaw | CrewAI | Claude Code |
|--------|--------|----------|--------|-------------|
| 工具 | 40+ tools，tool registry | 40+ tools，Plugin SDK | BaseTool ABC | MCP 動態擴展 |
| 記憶 | 8 種 MemoryProvider ABC | vector / QMD 後端可選 | LanceDB / ChromaDB 可換 | MCP 提供 memory |
| Model | OpenAI-compat 任意端點 | 40+ provider 輪換 | LangChain BaseLLM | model flag |
| Skills | SKILL.md + Skills Hub | bundled skills | Skill 模組系統 | 20 bundled + MCP 動態 |
| 子 Agent | SubAgent tool | SubagentRegistry spawn | Hierarchical manager + A2A | Agent tool + Team tools |
| Plugin/Hook | 13 個 hook 注入點 | 100+ plugins，Plugin SDK | callbacks + Flow decorators | Hooks / Skills / MCP 三層 |

### 維度一：上下文管理 — Context 滿了留什麼

| 框架 | 壓縮方法 | 留什麼 | LLM 參與 | 成本可預測性 |
|------|---------|--------|---------|-------------|
| hermes | LLM 摘要 | 語意重要的內容 | 是 | 中 |
| OpenClaw | LLM 摘要 + 識別符保護 | 語意摘要，但 UUID/IP/filename 完整 | 是（+ 規則保護） | 中 |
| CrewAI | LLM 評分 + 向量召回 | 重要性加權的記憶（Group A-D 分類） | 是（深度參與） | 低（Group D 難預測） |
| Claude Code | 本地確定性算法，XML 結構化 | 工作狀態事實（pending work/files/timeline） | 否 | 高 |

**核心分歧**：是否信任 LLM 的語意判斷做壓縮——Claude Code 完全不信任 LLM，只留確定性事實；CrewAI 把最多決策權給 LLM，代價是成本難預測。

OpenClaw 特有的 **Identifier Preservation Policy**：LLM 重構文字時傾向縮短 UUID、重構 IP，壓縮後 Agent 拿到被改過的識別符就找不到目標，strict 模式強制一字不改。

### 維度二：控制流設計 — 誰決定下一步

- **hermes + Claude Code**：**扁平迴圈**。`while LLM 還在呼叫工具: 執行工具`，直到停止。可測試、可推理，但天然不支援複雜多 Agent 協作。Claude Code 加了 Bootstrap 7-stage 啟動序列（trust gate 後才啟動 plugin/skill/MCP）。

- **OpenClaw**：**有狀態的多 Agent 樹**。SubagentRegistry 持久化每個 subagent 的生命週期（PENDING → RUNNING → COMPLETE/ERROR/KILLED），AnnounceFlow 讓 subagent 非同步交付結果（parent 不 blocking），SessionKey 編碼 agent 身份+session+深度。設計動機：Gateway crash 時，正在跑的 subagent 要能 Orphan Recovery。

- **CrewAI**：**宣告式 DAG**。兩層分離：Crew 回答「誰做什麼」（Sequential 靜態分工 / Hierarchical Manager 動態決策），Flow 回答「什麼時候做」（`@start/@listen/@router` 事件驅動路由）。路由圖在 class 定義時就用 AST 解析，可靜態渲染。AND/OR 觸發條件支援多路並行與 barrier 收斂。

### 維度三：錯誤恢復 — 出錯之後怎麼辦

- **Claude Code（執行前預防）**：Hook + Permission system 在工具執行前攔截。exit 2 = 「永遠不讓這件事發生」，不是「發生了再補救」。Permission 系統 enum 有序（ReadOnly < WorkspaceWrite < ...），防止越權操作。

- **hermes（執行中偵測）**：Middleware Stack 在工具執行中攔截。Recovery 機制在失敗後嘗試 retry 或降級策略。

- **OpenClaw（執行後修補）**：SubagentRegistry 持久化讓 Orphan Recovery 成為可能——Gateway crash 後重啟，仍能追蹤哪些 subagent 在跑、哪些已完成。Session 狀態不丟失。

- **CrewAI（層次式升級）**：Hierarchical 模式下 Manager 可以動態決策「這個任務失敗了，改派給誰」，但代價是多一次 Manager 的 LLM 呼叫成本。

### 維度四：反饋迴路 — 如何避免永遠停在當前能力

- **hermes**：程序性記憶（Procedural Memory）——每次完成任務後自動萃取可重用技能，寫回 Skills Hub。學習是持續自動的。

- **OpenClaw**：Plugin SDK 讓外部貢獻者擴充能力，100+ plugins 生態。能力增長靠社群，而非自動學習。

- **CrewAI**：EncodingFlow 4-group 分類讓系統自己決定記憶的 scope/categories/importance，RecallFlow adaptive depth 信心不夠時繼續深挖。

- **Claude Code**：Hooks 系統讓使用者自訂觸發行為，Skills/MCP 動態擴充工具集。能力邊界由人設計，框架不自動學習。

## C（關鍵行動）

- [ ] **選框架前先回答**：在你的場景裡，Agent 壞掉「最壞的情況」是什麼？是輸出錯誤、服務中斷、執行造成傷害、還是永遠停在現有能力水平？
  - 最怕造成傷害 → Claude Code（執行前預防）
  - 最怕多 Agent 協作中途崩潰 → OpenClaw（Orphan Recovery）
  - 最怕複雜工作流無法表達 → CrewAI（宣告式 DAG）
  - 最怕 Agent 學不到新技能 → hermes（程序性記憶）

- [ ] 設計多 Agent 系統時，確認四個維度都有對應的設計決策，而非預設使用框架的隱含選擇

## 相關連結

- [[harness-engineering]] — Harness Engineering：護城河在環境而非模型（四個框架的 Harness 差異）
- [[hermes-agent]] — Hermes Agent 深度介紹
- [[agentic-engineering-sop]] — 六階段實戰 SOP（Claude Code 視角）
- [[claude-code-five-layer-architecture]] — Claude Code ADK 五層架構
