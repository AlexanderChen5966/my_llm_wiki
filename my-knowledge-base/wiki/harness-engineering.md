---
title: Harness Engineering：AI 工程師的第三個維度
date: 2026-05-14
tags: [ai/agent, ai/claude, dev/architecture]
source: https://hackmd.io/@BASHCAT/SkQEW0F2bg
source_2: https://wenwender.wordpress.com/2026/04/02/harness-engineering-ai-%E5%B7%A5%E7%A8%8B%E5%B8%AB%E7%9A%84%E7%AC%AC%E4%B8%89%E5%80%8B%E7%B6%AD%E5%BA%A6/
source_3: https://codelove.tw/@tony/post/3bbOA3（〈Claude Code Harness 工程：數倉側落地方案｜得物技術〉，raw/ 素材 2026-05-27，對應下方「企業數倉落地案例」補充章節）
type: article
status: processed
asset_value: 90
---

Harness Engineering 是 AI 工程從 Prompt / Context Engineering 演進而來的一代範式——不再問「怎麼說」（Prompt Engineering）或「模型能看什麼」（Context Engineering），而是問「我該建什麼系統」。護城河不在模型，在環境：Guides（前饋）× Sensors（回饋）組成的 Steering Loop 決定 AI Agent 的輸出品質。

> **定位更新（2026-07）**：本文原標題稱 Harness 為「第三個維度／第三代」，但 2026-06 起 **Loop Engineering** 已成形為後續一代範式（見 [[agent-loop-explained]] / [[loop-engineering-6-components]]）。因此「三代」的說法已不完整，實為四代，權威來源（Addy Osmani、codecentric、O'Reilly）一致的堆疊順序為 **Prompt → Context → Harness → Loop**（Loop 在最外層，Harness 在其下）。本文核心的 Guides × Sensors 框架不受此定位變動影響。

## T（核心論點）
> "Agents aren't hard; the Harness is hard." — OpenAI Ryan Lopopolo

Manus 用同一個模型重寫了 5 次 Harness，每次提升都比換模型大。Harness 是 AI 開發的真正護城河，因為它封裝了無法從程式碼直接推斷的工程紀律與組織知識。

## B（適用場景）
**適用：** 多步驟 AI 開發流程；多 Agent 協作；需要可靠性和品質保證的生產環境
**不適用：** 一次性 prompt 問答；個人小工具（Harness 過度設計）

## R（Guides × Sensors 技術框架）

### 兩大控制機制
| 機制 | 時機 | 類型 | 範例 |
|------|------|------|------|
| **Guides（前饋）** | Agent 行動前 | Computational | TypeScript 型別系統、LSP |
| **Guides（前饋）** | Agent 行動前 | Inferential | AGENTS.md、Skills、AI 生成規劃 |
| **Sensors（回饋）** | Agent 行動後 | Computational | ESLint、semgrep、Coverage 檢查 |
| **Sensors（回饋）** | Agent 行動後 | Inferential | AI Code Review、Architecture Review |

成熟 Harness 需要覆蓋四個象限。多數團隊只有左下角（linter + 測試），缺少右上（系統性前饋）和右下（語義審查）。

### 三大 Harness 模板
1. **Maintainability Harness**：命名/格式一致性 → 自定義 linter + AI style review
2. **Architecture Fitness Harness**：防架構漂移 → dep-cruiser + structural tests + AI review
3. **Behaviour Harness**：行為符合規格 → 測試套件 + 規格文件 + 人工審查

### 漸進式建構路徑
- **Level 1**：建立 AGENTS.md / CLAUDE.md（最基本的前饋 Guide）
- **Level 2**：加自定義 linter，錯誤訊息包含修復指示（正向 prompt injection）
- **Level 3**：整合 CI Pipeline，Agent PR 自動驗證
- **Level 4**：加 AI Code Review（Inferential Sensor）
- **Level 5**：可觀測性閉環（追蹤 Agent PR 首次通過率、平均修正迭代次數）

## C（關鍵行動）
- [ ] 約束架構決策（API 走哪層），不要約束實作細節（變數命名）——讓 Agent 有解題自由
- [ ] linter 錯誤訊息直接包含修復方法（OpenAI Codex 做法）
- [ ] 設定最大迭代次數上限，超過就升級給人類（Stripe 做法）
- [ ] Agent 犯錯時問「Harness 缺了什麼？」而非只是修 bug

## 五維度工程師 Checklist

在任何 AI Agent 系統上線前，確認這五個維度：

| 維度 | 核心問題 |
|------|---------|
| **資源管理** | Context 滿了之後怎麼辦？有沒有熔斷機制防止無限循環消耗？|
| **狀態持久化** | 跨 session 的知識存在哪？怎麼讀回來？有沒有機制判斷哪些值得存？|
| **信息流控制** | 每一輪送給模型的 context 是精心選擇的，還是把所有東西都塞進去？|
| **安全邊界** | 模型能呼叫的工具有沒有約束？危險操作有沒有攔截和確認機制？|
| **任務編排** | 任務複雜到需要多個 Agent 協作嗎？如果需要，通信和權限怎麼設計？|

## Managed Agents 之後 Harness 的角色

Managed Agents（Anthropic 2026/4）解決的是 Harness 的「管線問題」（plumbing）：Session 持久化、Sandbox 隔離、水平擴展。

它**不解決**：你的 tools 怎麼設計、system prompt 怎麼寫、任務怎麼拆解、domain-specific 的錯誤怎麼處理。

> Managed Agents 不是讓 Harness Engineering 消失，而是讓它升維——從「怎麼讓 Agent 不掛掉」升級到「怎麼讓 Agent 在你的業務場景裡做對的事」。

## 越做越薄

Manus 在半年內重寫 Harness 五次，每次方向都是**簡化，不是加複雜度**。反直覺洞察：

- 隨著模型能力提升，Harness 應越做越薄
- 如果發現 Harness 越做越複雜 → 大概率是過度工程化，在用基礎設施補償本來可以直接信任模型的地方
- 更強的模型不代表更少 Harness，而是**更精準的 Harness**（Nicholas Carlini C 編譯器實驗：Opus 4.6 能編譯 Linux 內核，但每個能力級別都需要重新設計 Harness）

📎 一種「變薄」的具體機制見文末對照（Harness-Zero，2026-09-29）。

## 企業實戰案例
| 公司 | 做法 | 關鍵成果 |
|------|------|----------|
| OpenAI Codex | 零人工程式碼；Garbage Collection Agent；linter 錯誤訊息含修復指示 | 100萬行程式碼全靠 Agent |
| Anthropic | Planner + Generator + Evaluator 三代理；Initializer Agent 設定環境 | 多小時長時間自主開發 |
| Stripe Minions | 每週數千 PR；隔離沙箱；最多 3-5 次迭代後升級人工 | 大規模落地 |
| Datadog | Production telemetry 納入 Harness 閉環；形式化方法表達不變量 | 可觀測性閉環 |
| Manus | 相同模型重寫 5 次 Harness | 每次提升 > 換模型 |

## 補充資料：Harness Engineering 關鍵量化數據

來源：Truong Phung（dev.to，2026-04-20），整理自 WalkingLabs 課程。

**最能說明問題的對比**：
> 同一個模型，$200/6小時有 Harness → 做出可運行的遊戲；$9/20分鐘無 Harness → 一堆垃圾。模型沒變，Harness 變了。

**行業基準**：
- SWE-bench Verified 頂尖 Agent：通過率約 50-60%（精心挑選任務）。真實 repo 更低。
- OpenAI 百萬行實驗：3 工程師 + Codex → 5 個月約 1,500 PR（每人每天 3.5 PR）——**前提是大量投資 Harness 基礎設施**。

---

## 範式演進對比（2026-07 更新為四代）
| 世代（由內而外堆疊） | 核心問題 | 局限 |
|------|----------|------|
| 1. Prompt Engineering (2022-24) | 你送出的「字」怎麼寫 | 無法處理多步驟 |
| 2. Context Engineering (2025) | 模型每一輪能「看」到什麼（docs/工具/記憶/歷史）| 只管單一 Agent 視角 |
| 3. **Harness Engineering (2026 上半)** | 單一 Agent 在什麼「環境」裡跑（工具/skills/MCP/hooks/sandbox/可觀測性）| 仍在發展中 |
| 4. **Loop Engineering (2026-06 起)** | 怎麼讓被 harness 包住的 Agent「自己持續跑」直到達標 | Token 成本硬傷、驗證機制是命門、成熟度仍新 |

**權威來源一致的堆疊順序：Prompt → Context → Harness → Loop**，每一層包住前一層、也繼承前一層的弱點（context first, then harness, then loop — codecentric）。關鍵關係（Addy Osmani / codecentric / O'Reilly）：

> Harness 是「單一 Agent 的工作台」（有哪些工具、知道哪些規則、輸出怎麼被檢查）；**當這個 Agent 不再由人類逐一觸發、而是自己持續運轉時，harness 就變成了 loop**。Loop 就坐落在 harness 上面一層（Loop engineering sits one floor above the harness）。

**⚠️ 知識庫內部曾有一處排序錯誤**：[[agent-loop-explained]] 依據的 codelove 中文轉述文，把四層階梯寫成 Prompt → Context → **Loop → Harness**（Harness 在最外層），與上述權威來源相反。經 2026-07 上網交叉比對（Osmani 本人 blog / codecentric / O'Reilly 三源一致），正確順序應為 **Harness 在下、Loop 在最外層**，該條目已同步加註修正。[[loop-engineering-playbook-cli-tools]] 的 Tier A/B/C runtime 選型即「Loop 跑在哪種 Harness 上」的具體展開，可佐證此上下層關係。

## 相關連結
- [[agents-md-guide]] — AGENTS.md 設計原則（Harness 的核心 Guide）
- [[claude-code-context-management]] — Context Engineering（Harness 的子模組）
- [[claude-skills-guide]] — Skills 即 Inferential Guide
- [[agent-loop-explained]] — Loop Engineering 概念總覽（四層階梯：Prompt→Context→Harness→Loop）
- [[loop-engineering-6-components]] — Loop 六大組件（Harness 之上的自動驅動層）
- [[loop-engineering-playbook-cli-tools]] — Loop 實戰 Playbook（Tier A/B/C = Loop 跑在哪種 Harness runtime 上）
- [[harness-engineering-vue3-admin-practice]] — Harness 概念在單一開源 Vue3 專案的落地案例（CLAUDE.md/Rules/Skill 三層分工）
- [[ideas/harness-layer-research-agentbug-smith-mosib]] — 📎 2026-10-19 新增：harness 層研究兩則（AgentBug-Smith 重現框架錯誤、MOSiB 多分支最佳化不改權重；🔴 增益高度依模型，arXiv 經轉述未核原文）
- [[ideas/claude-managed-agents-dynamic-workflows-every-2026-10]] — 📎 2026-10-19 新增：Managed Agents 動態工作流程 public beta＋Every 公司代理（對照本篇「Managed Agents 之後 Harness 的角色」段）

## 補充資料：企業數倉落地案例（得物技術，2026-05-25）

來源：得物離線數倉團隊實戰，主力工具 Claude Code（見 frontmatter `source_3`）。

### 三大結構性痛點

**痛點一：Context Compact 失憶**
會話開始說的約束（如「金額單位是千元」），在 context 達 95% 觸發自動壓縮後全部丟失，導致生成 SQL 把千元當元，差了 1000 倍。

**痛點二：規範穿透率不足**
人工規範遵守率 60-70%，AI 靠 prompt 記憶的規範遵守率也只有 70-80%。需要把規範從「LLM 記憶中的指導性內容」變成「每次執行時強制檢查的護欄」。

**痛點三：大型需求 Context 膨脹**
血緣查詢結果 + SQL 執行結果 + SKILL 規範文件 + 資料比對樣本 → context 迅速膨脹 → compact 觸發 → AI 開始犯低級錯誤。

### Harness 落地解法

| 痛點 | 解法 | 原理 |
|------|------|------|
| Context 失憶 | 關鍵約束寫進 SKILL.md + CLAUDE.md | 每次 session 必載入，不依賴對話歷史 |
| 規範穿透率 | Hooks 自動執行規範檢查腳本 | 執行後強制校驗，不靠 LLM 記憶 |
| Context 膨脹 | 任務 Checkpoint 機制 + 分段壓縮 | 大型任務切分為多個獨立 session，交接時帶摘要 |

> 核心洞察：確定性不來自模型記憶，而來自包覆在機率模型外的**機制層**——SKILL.md 是知識護欄，CLAUDE.md 是行為規範，Hooks 是強制執行器。

## 補充資料

### 補充視角一：Harness 六層架構（WenHao Yu，2026-03-16）

來源：yu-wenhao.com，以馬具比喻和 LangChain 實驗數據解析 Harness 概念。

**Harness 六層**（越做越好不需換模型）：

| 層次 | 作用 |
|------|------|
| 1. 迴圈（Loop） | 觀察→決定→行動→驗證→更新，持續循環 |
| 2. 工具（Tools） | 讓 AI 能讀檔、跑程式、call API、搜尋 |
| 3. 上下文（Context） | 控制 AI 看到哪些資訊、避免過載 |
| 4. 持久化（Persistence） | 跨對話記住之前做過什麼 |
| 5. 驗證（Verification） | 做完跑測試、檢查語法、自我審查 |
| 6. 限制（Constraints） | 哪些事不准做、哪些檔案不能碰、花費上限 |

**LangChain Terminal Bench 2.0 實驗**：同一模型，只改 Harness → 52.8% 跳到 66.5%，排名從 Top 30 外跳到 Top 5。改了什麼：
1. 加了自我驗證迴圈（完成前強制跑 checklist）
2. 注入環境上下文（開工前先掃目錄結構）
3. 防迷路機制（偵測 AI 是否重複編輯同一個檔案）
4. 調整推理預算（規劃/驗證多分配，實作少分配）
5. 失敗分析（自動分析失敗模式歸納改進方向）

**Vercel 案例**：把可用工具從 15 個砍到 2 個 → 準確率 80% 升到 100%，Token 少 37%，速度快 3.5x。**少即是多，限制本身就是 Harness 設計。**

> 「同一個模型，Claude Code 感覺比網頁版聰明那麼多？同一個模型，差別完全在 harness。」

**人生哲學延伸**：Harness 概念對個人效率同樣適用——大腦是模型，環境/習慣/工具/回饋機制是 Harness。最快的改變不是升級大腦，而是升級環境。

---

### 補充視角二：AI 工程師的第三個維度（溫煜鈞，2026-04-02）

來源：wenwender.wordpress.com，以三代 AI 工程範式對比深化 Harness 概念。

**三個詞，三種不同問題**：
- **Prompt Engineering**：怎麼說，才能得到最好的輸出？
- **Context Engineering**：什麼信息，應該在什麼時候進入 context window？
- **Harness Engineering**：怎麼建立一套系統，讓模型能安全、可控地在真實世界行動？

三者不是競爭關係，而是不同抽象層次的工程問題。

**為什麼模型從「文字生成工具」變成「執行代理」後，Harness 變得必要**：
1. **操作有副作用，不可逆**（刪了的檔案、送出的 API 請求）
2. **任務有狀態，跨越多個輪次**（模型每輪都是「無記憶」的，Harness 帶狀態）
3. **系統有成本，資源不是無限的**（Context 上限、API 費用）
4. **複雜任務需要多個代理協作**

**無人值守並行的前提**：Stripe 能讓 Agent 自主完成到 PR 再通知人，是因為 Harness 夠成熟。多數團隊目前還在「有人值守」階段。**一個團隊在這個光譜上的位置，取決於 Harness 的成熟度，不是模型能力。**

**Managed Agents 後的角色升維**：Anthropic Managed Agents 解決「管線問題」（plumbing），不解決「這個 Agent 要做什麼」。讓 Harness 從「怎麼讓 Agent 不掛掉」升維到「怎麼讓 Agent 在業務場景裡做對的事」。

---

### 補充視角三：多 Agent 對話的 Harness 技巧（OpenClaw/Hermes 場景）

來源：Hana 的多 Agent 對話實戰筆記（OpenClaw + Hermes 場景），2026-05。

**四大陷阱**

| 陷阱 | 說明 |
|------|------|
| 隱式觸發繞過 | `requireMention` 對 Bot-to-Bot 無效，設定 `allowBots: "all"` 後任何 Bot 都能觸發彼此 |
| 無限對話迴圈 | Bot A 觸發 Bot B，Bot B 觸發 Bot A——MAST 2026 研究：41.8% 的多代理失敗源於缺少終止條件 |
| 對話永不結束 | 沒有 stopping condition，Agent 不知道任務完成了沒有 |
| Agent 無法保持沉默 | Agent 只能「回應」或「不回應」，無法選擇性沉默（對特定訊息靜音）|

**兩種解法對比**

| 解法 | 設定 | 優點 | 缺點 |
|------|------|------|------|
| **Mention Gating** | `allowBots: "mentions"` | 結構明確，觸發可控 | 要求 Agent 用 @mention 語法，語言不自然 |
| **Response Gating** | `allowBots: true` + AGENTS.md NO_REPLY 規則 | 語言自然，符合人類溝通方式 | 依賴模型理解 AGENTS.md，可靠性稍低 |

作者實戰選擇：Response Gating + Channel Scope（特定頻道完全開放，其他頻道要求 @）+ 5 輪 Agent-to-Agent 上限。

**平台能力差異**

- **OpenClaw**：支援 `NO_REPLY` 規則（inbound silence ✅）——可在 AGENTS.md 明確告知 Agent 何時不要回應
- **Hermes**：`[SILENT]` 僅適用於 cron 排程 outbound（inbound silence ❌）——無法靠規則讓 Agent 對特定輸入保持沉默

**設計原則**：任何多 Agent 系統必須在設計初期就定義：① 誰能觸發誰；② 什麼條件下任務結束；③ Agent 看到不該回應的訊息時怎麼辦。

---

### 補充視角四：Harness Coding 的「第一讀者」洞察（Brickverse，2026-05）

來源：Brickverse，《從 Prompt 到 Harness Coding：你寫的東西，第一讀者都不再是人》。

**核心洞察**：你的 Prompt、文件、API 設計、介面——這些東西的第一個讀者，正在從「人」變成「AI」。這個轉變要求重新思考整個軟體工程的設計原則。

**三代演進的「讀者視角」詮釋**

| 世代 | 時期 | 關鍵問題 | 設計焦點 |
|------|------|---------|---------|
| Prompt Engineering | 2022-2023 | 「怎麼問，AI 才聽懂？」| 學會問（對話技巧）|
| Context Engineering | 2024-2025 | 「怎麼餵，AI 才有足夠資訊？」| 學會餵（資訊管理）|
| Harness Coding | 2026~ | 「怎麼設計環境，AI 才能正確行動？」| 學會設計環境 |

「Harness Coding」這個詞比「Harness Engineering」更強調「程式碼本身就是設計給 AI 讀的」——變數命名、函式簽名、錯誤訊息、README 結構，都要以 AI 作為主要讀者來設計。

這與 Guides × Sensors 框架的 Inferential Guide 設計原則一致：AGENTS.md、Skills、錯誤訊息的措辭，都是在優化 AI 作為讀者的理解效率。

---

## 補充資料：一條義務配一個 guide 加一個 sensor（2026-08-07 併入）

來源：iThome〈繁瑣即安定〉系列 Tedium Is Stability-04。本頁上方的 Guides × Sensors 四象限已涵蓋該篇的框架，此處只收它獨有的**配對原則**與失敗模式——這與「四象限都要覆蓋」是不同層級的操作指引。

### 骨架的等式與比喻

**Agent = Model + Harness**。同一個模型給它不同骨架，產出品質可以天差地遠。

> 沒有骨架的模型像**一台蒙眼開的車**：引擎再強，它不知道要往哪開（沒有地圖），也不知道自己開歪了（沒有回饋）——**結果引擎越強、撞得越用力**。
> guides 讓它知道往哪開，sensors 讓它知道開歪了；**模型，只是引擎。**

### 🔑 配對原則：每條義務都要成對

> **你希望 AI 顧到的每一條義務，最好同時給它一個 guide 和一個 sensor。** guide 告訴它去做，sensor 檢查它做了沒——兩個都有，這條義務才算真的被包住。

**失敗模式是對稱的**：
| 缺哪半 | 後果 |
|---|---|
| 只給 guide 不給 sensor | **AI 可能忘了做，而你也不知道** |
| 只給 sensor 不給 guide | **它每次都得撞牆才學會** |

### 三個具體配對範例

| 義務 | guide（前饋，寫進規約檔）| sensor（回饋，機器判定）|
|---|---|---|
| **稽核** | 「會改狀態的動作都要記一筆稽核」 | 一句 `grep` 搜那個統一寫入口的呼叫點，看新路徑在不在裡面 |
| **權限** | 「端點都要掛授權閘、綁對案件類型」 | **負向測試**：拿沒權限的身分去打，期望被擋 |
| **資料庫** | 「改欄位要同步遷移腳本、四層一起改」 | **啟動時的結構檢查**，對不上就紅燈 |

### 與 obligations 系列的串接

該系列前幾篇其實一直在準備這兩半：**第二篇把義務寫下來 = guides；第三篇替每條義務配一個可判定的檢查 = sensors**（見 [[obligations-as-code-tedium-is-stability]]）。

一句話總結：**把隱性知識寫成 guides，把「算不算對」寫成 sensors。**

### ⚠️ 骨架有效的前提

> guides 要有效，還卡著一個前提：**你餵給 AI 的那些 guides，它得先看得懂、接得上。** 如果它連這個產品的長相、慣例、地雷都還沒摸清，你給它再漂亮的規約，它也接不住。

→ 這正是 [[zerospec]] 的 Layer 0 命題（讓專案先「可被 AI 讀懂」）存在的理由，也是該系列下一篇要談的「接手陌生系統先畫地圖」。

## 補充資料：組織側的 harness——AI 暴露的是程式碼周邊的拖累因素（2026-09-10 併入）

來源：〈AI 工程很簡單，改變我們的工作方式才困難〉，codelove.tw 譯本，2026-09-06，
<https://codelove.tw/@tony/post/qzonBx>（原文：Ujjawal，dev.to，
<https://dev.to/ujja/ai-engineering-is-easy-changing-how-we-work-is-hard-39j4>）

本條目的 Guides × Sensors 是**技術側**的 harness（型別系統、linter、AI review）。
本節補的是同一概念的**流程與組織維度**——這是既有內容最缺的一塊。

> 🔑 **AI harness 的概念比 prompt 與 agent 設定大得多。**
> harness 是我們為 AI 建立的**周邊環境**：如何定義工作、產品／UX／工程如何協作、知識如何結構化、
> 程式庫如何組織、如何驗證工作，以及 **agent 擁有哪些存取權限**。
>
> 「AI 工程很簡單，改變工作方式才難。」

### 一、問題已經不是「AI 能做什麼」

AI 已能參與開發五階段——**需求、細化、規劃、建置、驗證**——的每一個。
所以真正該問的是：**我們現在打造軟體的方式，是否準備好迎接它。**

這讓需求品質變得更重要：人類同事知道「改善體驗」指什麼，因為已經討論過好幾次；
**agent 沒有這種共同背景**，它需要明確的問題、範圍、限制、邊界情況與預期結果。
⚠️ 這不等於要寫超長規格書，而是**在開始建置前先把需求打磨精確**。

反過來，AI 本身就可以當一個 **inferential guide**——一個「有點煩但很有用的審查者」，
會問：**這件事失敗時會發生什麼？這個需求可測試嗎？文件中兩個部分是否互相矛盾？還有哪些沒考慮到？**
也可以拿它比較不同版本的 PRD，或讓一個模型審查另一個模型的輸出。
🔑 **重點是 AI 幫我們找出模糊之處，而不是替我們做決定**——這正是本條目 Sensors 的精神提早搬到需求階段。

### 二、🔑 瓶頸位移：釐清需求的兩週，代價變大了

複雜工作通常要在產品／UX／需求／工程之間來回好幾輪才能開工。
**既然 agent 能很快做出可運作的實作，為了釐清需求等上兩週的問題就比以前大得多。**

對策是把交接改成迭代：工程師更早參與（不等需求被認定「完成」），UX 更早加入；
粗略原型或線框圖往往比再開一輪會議更快看出需求缺口，而 AI 讓輕量原型的成本低很多。
→ 需求／UX／技術設計應成為**反覆迭代的循環，而不是一連串的交接**。

### 三、🔴 文件不是越多越好——多份文件描述不一致 ＝ 給 agent 更多混淆自己的方式

直覺是餵更多 PRD、架構圖、wiki。但**若好幾份文件對同一功能的描述不一致，那不是更好的上下文，是更多噪音**。

agent 真正需要的是**理解系統的路徑**：清楚的專案結構、聚焦的文件、實用的 agent 指令、
**能說明某個架構決策為何存在的設計記錄**，以及整理良好的程式碼庫。
> 目標不是把我們知道的一切都塞給 AI，而是**讓 AI 在需要時能輕易找到它需要的東西**。

（與 [[agents-md-guide]] 的「不可推斷的隱性知識密度」同一命題，本節補的是**文件漂移**這個失敗模式。）

### 四、ticket 切法與「流程要與複雜度相符」

一張要做數週甚至數月的 ticket，**對 agent 難以推理，對人類也不是好的工作單位**——
「建置驗證機制」和拆成密碼重設／權杖驗證／密碼更新加上測試，是兩回事。
判準不是「拆得夠碎」，而是**有明確目的、可管理範圍、有完成定義**。
若某件事要做幾個月，那它其實是一個專案，只是披著 Jira 的外衣。

⚠️ **但流程應與複雜度相符**：大型功能受益於結構化的需求／細化／規劃／驗證；
**小型 BAU 變更可能只需要一個 prompt 和一位開發者**。把同一套流程套到所有事情上，
只是**用另一種官僚流程取代原本的官僚流程**——這是本條目 B 層「個人小工具屬 Harness 過度設計」的組織版。

### 五、agent 必須看到真實系統，而最大的阻礙常與模型無關

只給 PRD 而不給程式碼存取權，agent 仍然只能憑假設做事；有程式碼才能找到既有模式、理解限制、
重用功能、辨識提議的解法是否不適用。
但讓 AI 存取原始碼牽涉**資安、授權、隱私與組織**層面——
🔑 **導入 AI 不只是選對模型，有些最大的阻礙根本和模型無關。**

📌 **多人多 agent 並行的新失效模式**：每個 agent 只憑自己看得到的上下文決策，
可能改掉另一個 agent 不知道的東西，或**兩個 agent 各自做出合理但搭不起來的決定**。
這反而讓傳統工程實務更重要：小幅變更、清楚邊界、良好測試、頻繁審查、**一致的專案規則**。

### 收斂

> AI 改變的不只是寫程式的速度，它**把程式碼周邊那些拖慢我們的因素全都暴露出來**：
> 不清楚的需求、過大的 ticket、UX 介入太晚、文件漂移、交接、存取限制，以及要花好幾天才能做出的決策。

作法上不必一步到位——挑一些真實工作試跑這些流程，看它們在哪裡失效，再逐步改善。
相關流程側條目：[[ai-native-sdlc-playbook-anthropic]]、[[spec-driven-ai-workflow]]、
[[spec-first-ai-coding-workflow]]（需求→驗證的循環）、[[claude-code-team-rollout]]（組織導入阻礙）。

## 補充資料：AI 代理是聰明的猴子——窄範圍收斂、寬範圍發散，邊界要讓錯誤可發現（2026-09-23 併入）

來源：〈【AI 代理只是聰明的猴子】軟體工程師的新工作，是打造讓代理更強大的系統邊界〉，TechOrange（Min 編譯，2026-09-22），<https://techorange.com/2026/09/22/software-engineer-new-job-ai-agent/>。⚠️ 信任度：編譯自 VentureBeat 與 InfoWorld、論點人物為資料工程專家 Ananth Packkildurai，屬第 4–5 級轉述文章，無實作。

本段只收增量。文中的「系統邊界＝明確輸入＋顯性規則＋有效回饋，讓代理的錯誤變得**具體、可發現、可修復**」即本頁 Guides × Sensors；「人類介入的價值是提供新訊號（失敗的測試紀錄、確定性工具、精確的錯誤原因）」與上方 Level 2（錯誤訊息包含修復指示）一致。

1. **猴子迴圈的收斂條件**：代理不是隨機的猴子，它有編譯器、工具、repo、測試與回饋，工作方式是「提出 → 執行 → 觀察 → 修正」。範圍窄、有回饋時迴圈會收斂；長時間迴圈則會累積過時假設、分支化的上下文與未解的依賴，**下一步的確定性甚至不如第一步**。
2. **企業環境的變動源**：第三方 API、區域政策、半在程式碼半在人腦的業務規則、速率限制與版本更新、schema 演進、動態安全政策、埋在例外處理裡沒寫下來的舊規則。
3. **格式合規 ≠ 語意正確**：資料可能結構完全合規、通過管線測試，業務意義卻是錯的——同一命題在 agent 修 bug 情境的版本見 [[agent-bug-fix-eval-green-is-not-correct]]。
4. **五種「隔離場」手段**：嚴格語義層、不可變事件日誌、資料合約、冪等 API、確定性狀態機——用確定性減少代理同時做出的假設數量。

同一原則的具體實作：[[tool-description-and-error-message-as-prompt]]（描述擋不住行為，要寫成程式碼）、[[human-agent-shared-data-layer-optimistic-locking]]（條件式 UPDATE 就是確定性的狀態轉移）。

## 補充資料：對照——harness 能不能蒸餾進模型（2026-09-29 併入）

來源：aiposthub〈Harness-Zero 是什麼？把 AI Agent 的 Harness 教進模型〉（2026-09-29）。🔴 **論文出處待查**（剪藏只寫「2026 年 9 月的 arXiv 預印本」，沒有編號、作者或連結）；數字為**論文自報、single-run、未經同儕審查與外部複現**。

Harness-Zero 讓強 reviewer 在訓練階段扮演專用 harness，把它誘發的行為（先檢查再修改、儲存後重讀等）改寫成最小 harness（一個 system prompt＋一支 Bash 工具）能執行的軌跡，再用 LoRA SFT 蒸餾進 Qwen3.5-9B，部署時只留最小 harness。三領域 macro-average 從 23.3 升到 44.3（高於掛專用 harness 的 41.7），但 **USPTO 30.0 仍低於專用 harness 的 38.0**。這是上方「越做越薄」的一種具體機制，但邊界很清楚：權限、金流、刪除、法遵、不可逆動作仍要留在 deterministic enforcement，與本頁 Guides × Sensors 裡 Computational（由程式確定性執行）那一類控制的分工不衝突；部署時也仍保留最小 harness，**不是「harness 已不需要」**，而是重劃「可學的習慣」與「必須強制的規則」的邊界。完整拆解（含 Perplexity 從中途錯誤學習的第二案例）見 [[agent-trajectory-distillation-harness-zero-perplexity]]。
