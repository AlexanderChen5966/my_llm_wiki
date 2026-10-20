---
title: Claude Managed Agents 2026-10：動態工作流程 public beta（子代理 25 子執行緒／動態流程上限 1,000）與 Every 公司代理案例
date: 2026-10-19
tags: [ai/claude, ai/agent, ai/workflow]
source:
  - https://www.aiposthub.com/every-company-agent-claude-managed-agents-slack/（引 https://x.com/claudeai/status/2107574195978641911）
  - https://www.aiposthub.com/claude-managed-agents-dynamic-workflows-public-beta/
type: idea
status: processed
asset_value: 38
---

> 🔴 **來源集中：aiposthub／Philo**。兩篇同作者（10-07、10-11 發布），且互為延伸閱讀；同站互引不構成獨立印證，作者皆**未實測**。
> 來源等級：Anthropic 公告／文件與 Claude 官方 X 貼文是一手，但本庫**只有 aiposthub 的轉述**——以下標「**官方，媒體轉述**」者皆如此；🔴 找錯實驗是**廠商自評**；其餘標「**作者觀點**」。
> 為何放 ideas：依 [[ideas/openai-agents-api-public-beta]] 的先例，單一媒體轉述的產品公告放 ideas；等剪藏到 Anthropic 官方公告或文件，再升格或補進 wiki，同時解決下方 25／16 的核對。

## T（核心論點）

Claude Managed Agents（Anthropic 的受管理代理服務，API 端）在 2026-10 有兩件事：**10-06** Claude 官方帳號分享 Every 用它做「Slack 入口＋共用 Skills」的公司代理；**10-09** 宣布**動態工作流程（dynamic workflows）進入 public beta**——主代理寫一段程式，在背景編排多個代理、多個階段並合併結果。

🔑 兩篇共同的重點都不是「能開多少代理」，而是：**代理數量 ≠ 成果**；合併與覆核是正式工作；要設**工作階段總預算**與停止條件；共用 Skill 要有版本與負責人。

## B（背景）

**適用：** 評估在 API 端（而非 Claude Code CLI）跑多代理長任務；團隊想集中共用 Skills。

**不適用 / 注意：**
- 🔴 **與 Claude Code 端 Dynamic Workflows 的數字不可混用**（見 R 層對照表）
- 原文**沒有定價**資訊
- 時序：Every 案例（10-06）早於動態流程 public beta（10-09），兩者沒有矛盾，依日期排序

## R（關鍵細節）

### 一、案例：Every 公司代理（2026-10-06）

- 一手：`x.com/claudeai/status/2107574195978641911`（2026-10-06）
- Every 用 Managed Agents 建公司代理，**以 Slack 為入口**；內部使用受歡迎後**開放給訂閱者**（官方，媒體轉述）
- 動機之一：新模型推出時團隊共享 Skills，接上新能力，而不是各自重建提示詞（官方，媒體轉述）
- Managed Agents 快速入門把 **agent 配置、執行環境（environment）、工作會話（session）** 分開，並透過 **events** 了解進度（官方文件截圖，媒體轉述）
- 🔴 官方案例**沒有揭露** Every 實際的 Skills 內容或成效數字；文中「整理資料、準備會議背景、會議整理欄位」等用途**是作者建議，不是 Every 的功能**
- 共用 Skill 需要版本、修改紀錄（改了什麼、為何改、用什麼案例檢查）、能回到前一版、有負責人；每項 Skill 要求對應一個檢查點，資料不足時列出缺口而不是補造（作者觀點）
- 換模型時用幾份固定代表性輸入＋合格條件做回歸比較（作者觀點）

### 二、動態工作流程 public beta（2026-10-09）

- 2026-10-09 Anthropic 宣布 Managed Agents 動態工作流程進入 public beta（官方，媒體轉述）
- 子代理 vs 動態流程（官方文件，媒體轉述）：
  - **子代理**：主代理直接交辦、讀回報再決定下一步；適合需要**持續追問的專家分工**
  - **動態流程**：Claude 撰寫程式在背景組織多個代理與階段；適合**大量可拆開的工作與長任務**
- 規模：**一般子代理同時最多 25 個子執行緒**（官方文件，媒體轉述）；**動態流程最多協調 1,000 個代理**（官方公告，媒體轉述）——🔴 1,000 是**規模上限，不是建議值**，兩種上限不能混用
- 官方文件建議設定**工作階段（session）預算**（涵蓋其中執行的流程），並指示代理何時適合用動態工作（官方，媒體轉述）
- 🔴 **找錯實驗（廠商自評、特定程式庫、未獨立重現）**：約 116,000 行程式植入 70 個錯誤；單一代理三輪分別找到 **14／15／27** 個，動態流程三輪**各 66** 個。原樣列出，不算平均；不代表其他程式庫有同樣比例，也不代表剩下的錯誤不存在
- 分工要依資料範圍或問題類型切，指定每個代理回報的證據與不確定性；區分真正問題、同一問題的重複回報、無證據猜測（作者觀點）

### 三、🔴 與 Claude Code 端 Dynamic Workflows 對照（並列不擇一）

| 項目 | Managed Agents 端（API，本條） | Claude Code 端（CLI，[[claude-code-dynamic-workflows]]） |
|---|---|---|
| 發布 | 2026-10-09 public beta（官方，媒體轉述） | 2026-05-28 發布（oao.tw 轉述） |
| 同時並行 | **一般子代理 25 個子執行緒**（官方文件，媒體轉述） | **同時 16 個並行**（該條 T 層） |
| 規模上限 | 動態流程 **1,000 個代理**（官方公告，媒體轉述） | 最多 **1,000 個**平行 subagent（該條 T 層） |

- 兩端都稱 1,000，但出處不同，分別標示
- 25 與 16 可能是**不同產品表面**（API vs CLI），也可能是**不同指標**（「一般子代理的子執行緒」vs「workflow 內同時並行數」）；**是否同一功能延伸，待核對官方文件**

## C（連結與應用）

- [ ] 剪藏 Anthropic 官方 Managed Agents dynamic workflows 公告／文件，核對 25 vs 16 的定義
- [ ] 本庫若要讓多代理處理長任務，先設總預算與停止條件，再談代理數
- [ ] 本庫的共用 skill 已有 SKILL_INDEX 與版本追蹤（見 CLAUDE.md 外部追蹤點），可對照 Every 案例「版本＋負責人＋檢查點」再檢一次

## 相關連結

- [[claude-code-dynamic-workflows]] — Claude Code 端同名能力；🔴 16 並行 vs 本條 25 子執行緒並列待核
- [[harness-engineering]] — 「Managed Agents 之後 Harness 的角色」段：Managed Agents 解決管線問題，不解決「這個 Agent 要做什麼」——Every 案例正是團隊自己負責工作規則的例子
- [[subagent-four-boundaries]] — 🔑 Subagent ≠ Parallelism：多開代理帶來更多 token、協調與重複工作，對照本條「代理數量 ≠ 成果」
- [[synthesis/synthesis-multi-agent]] — Multi-Agent 精粹（本條僅屬待佐證觀點，未列為其來源）
- [[claude-code-agent-teams-when-to-multiply]] — 什麼時候值得多開 AI
- [[agent-loop-stop-conditions]] — 預算上限與停止條件
- [[claude-tag-slack]] — ⚠️ 同為 Slack 入口但產品不同：Claude Tag 是 Anthropic 的 Slack 功能；Every 是用 Managed Agents **自建**的公司代理
- [[enterprise-ai-adoption-skill-library-buttons]] — 企業把 Skill 做成按鈕給員工用；對照 Every「共用 Skills 集中工作方法」
- [[ideas/multi-agent-handoff-acceptance-workbench-agency-agents]] — 同作者同期的多代理交接／驗收論述（同源，不是獨立佐證）
- 📎 2026-10-19：[[claude-platform-opentelemetry-beta]] — Managed Agents 的 OTel 追蹤（Beta）；同為 aiposthub 轉述，不是獨立佐證
