---
title: 三個工具的多模型編排擴充規劃：fox-ai-roundtable / notebooklm-kit Codex 端 / claude-pilot-suite
date: 2026-07-08
tags: [ai/agent, ai/workflow]
source: 對話分析（延伸自 fable-advisor-subagent-skill 討論）
type: reference
status: processed
---

# 三個工具的多模型編排擴充規劃

> 背景：延續 [[fable-workflow-institutionalization-plan-2026-07-08]] 與 [[fable-advisor-subagent-skill]] 的討論。核心前提修正：**Fable 5 目前是限時可用，未來成本可能很高，不能當成常態依賴**。fable-advisor 只有軟性節流（克制心法），沒有硬性護欄——本規劃在三個目標系統都加入硬性成本護欄，不重複 fable-advisor 的缺口。

## 通用設計原則（三個規劃共用）

借用 [[loop-engineering-6-components]] 的「三道必要護欄」，任何要接「更貴/更強模型當顧問或統籌」的設計都必須具備：

1. **次數上限**：每個任務／每場對話最多諮詢 N 次，硬性寫死，不是文字克制建議
2. **Fallback 分支**：顧問模型不可用、太貴、或超出上限時，自動退回原本流程（不中斷主流程）
3. **總開關**：可以整包關掉顧問機制而不影響主流程，因應未來 Fable 5（或任何顧問模型）計費/存取狀態改變

---

## 1. fox-ai-roundtable：CLI 擴充選項 + 選配統籌者 + CONTESTED 仲裁整合

### 現況

零相依 Node server，固定三支 CLI（Claude / Codex / Antigravity）平行呼叫，同一 prompt、同時作答、並排顯示，無角色分工、無匯總。詳見 [[fox-ai-roundtable-multi-cli-tool]]。

### 規劃 A：CLI 擴充選項（架構已支援，屬工程擴充而非模式移植）

> ✅ **已實作（2026-07-09）**：`工具類型專案/fox-ai-roundtable-main` 新增 **Ollama**（免費本機模型，`ollama list` 自動偵測模型清單；CLI 無 session 機制，改由伺服器保存歷史重組上下文）、**Copilot CLI**、**OpenCode CLI**（兩者續聊用 `--continue` 接續工作目錄最近 session）。三家預設不勾選、未安裝時錯誤只顯示在該卡片。

現有模式是「spawn 一支 CLI → 解析 JSON output → 記 session/thread id 供 resume」，`agy`（Antigravity）已證明此模式可複製。擴充方式：

- 新增 CLI 只需比照三個既有區塊（`claude -p --output-format json` / `codex exec --json` / `agy -p`）的固定介面：呼叫、解析、記錄 resume 用的 id
- 候選：本機 Ollama CLI（免費、可測試開源模型）、Grok CLI（若日後有本機 CLI）
- 每支新增 CLI 應保持零相依原則，不引入新的執行環境或 API key 依賴

### 規劃 B：選配統籌者（吸收先前對話的成本收斂結論）

> ✅ **已實作（2026-07-09）**：`POST /api/consolidate` + 每輪 ≥2 份答案後出現的「🧭 統合摘要」按鈕。四道護欄全落地：按鈕觸發（每輪一次，用後即收）、複用 Claude/Codex/Antigravity 其中一家做**一次性呼叫**（不帶 resume、不寫 session，不污染對話記憶）、prompt 硬性限定「只列共識/分歧、禁止裁決」、獨立摘要卡（details 可收合）與原始並排視圖並存。

**不做成每次自動觸發的付費呼叫**。設計要點：

- 使用者主動按下「統合」按鈕才觸發，不是每次提問自動附加
- 統籌步驟**複用已經在跑的三家其中一家**（例如 Claude），不額外開一個新的付費呼叫或第四支模型
- 統籌產出限定為「共識點／分歧點」摘要，不做篩選或代替使用者判斷——保留工具「使用者自己看三份原始答案」的核心賣點
- 介面上明確標示這是「摘要視圖」，與三份原始答案分開顯示，使用者可隨時切回原始並排視圖

### 規劃 C：CONTESTED 仲裁整合者（借 ai-pair 的 CONTESTED 機制，但反向應用）

> ⏸️ **暫緩（2026-07-09 決策）**：規劃 B 的分歧摘要已涵蓋主要價值（分歧內容 B 就會列出，C 只是加分級標籤）；且 C 有誤判調校成本（措辭差異 vs 實質衝突）。**重啟條件**：使用 B 一段時間後若出現「只看統合摘要、不再細讀原始答案」的行為，再回來補 CONTESTED 警示當護欄。

ai-pair 的 v4.5 Mesh 設計中，CONTESTED（reviewer 間 Round 2 後仍分歧）目前是直接丟給使用者仲裁（見 [[ai-pair-mesh-topology-design]]）。fox-ai-roundtable 的情境不同——三家本來就是平權比較，不是審查投票，沒有「BLOCK/PASS/WARN」這種可仲裁的結構化判定。要整合 CONTESTED 概念，需要先加一層**結構化判定**才有東西可仲裁：

- 統籌者（規劃 B）除了摘要共識/分歧，可額外標記三家答案是否構成「實質衝突」（而非只是措辭不同）
- 若判定為實質衝突，才呈現「⚖️ 三家意見分歧，需要你判斷」的標記，比照 ai-pair CONTESTED 的呈現方式，但**仲裁者永遠是使用者本人**，不像 ai-pair 規劃中考慮過的「Fable 先給建議裁決」——因為 roundtable 的定位是輕量比較工具，不該引入額外付費模型當二階仲裁
- 此規劃依賴規劃 B 先落地，屬於規劃 B 的進階選配，非獨立功能

**判定分類（三級，比照 ai-pair 的 BLOCK/PASS/WARN 精神但改成比較語境）**：

| 分類 | 判定標準 | 呈現方式 |
|------|---------|---------|
| **CONSISTENT** | 三家結論/建議方向一致，僅措辭或詳略不同 | 不特別標記，維持原本並排視圖 |
| **OVERLAPPING** | 部分一致、部分互補（例如各自補充了不同細節，但無互斥主張） | 統籌摘要標出「互補點」，不觸發警示 |
| **CONTESTED** | 至少兩家對核心問題給出**互斥**的結論或建議（例如一家說「可行」、另一家說「不可行」；或給出的具體數字/步驟直接矛盾） | 呈現「⚖️ 三家意見分歧，需要你判斷」標記 + 並列三方核心主張 |

**判定提示詞設計要點**：統籌者做這個分類時，prompt 要明確要求「只看結論/建議是否互斥，不要因為表達方式不同就判定衝突」——這是避免誤判的關鍵，語氣差異、詳略程度不該觸發 CONTESTED。可比照 citation-integrity-check 的做法，只做三選一分類，不展開複雜的信心分數。

**成本護欄**：CONTESTED 判定是統籌者（規劃 B）產出的附加標記，不是獨立的額外呼叫，因此不需要另外的次數上限——它繼承規劃 B「按鈕觸發、複用既有模型」的護欄。

---

## 2. notebooklm-kit（v2.0.1 / v2.1.1）Codex 端：借 fable-advisor 模式，用現有高階模型當統籌

### 前提修正

不等 GPT-5.6（尚未發布，能力與價格未知）。改用**現在就能用的高階模型／設定**達成同樣的分層效果。

### 架構對應

fable-advisor 是「Opus 統籌＋Sonnet worker＋Fable 顧問」，三個角色是三個不同模型。Codex desktop 目前（依 [[fox-ai-roundtable-multi-cli-tool]] 條目記錄）ChatGPT 帳號通常只有 GPT-5.5 一個模型可選，但 **Codex CLI 支援調整 reasoning effort**——因此可以用「同一模型、不同 effort 檔位」重現分層效果，而非等一個更強的新模型：

| 角色 | fable-advisor 對應 | Codex 端現行可行方案（已查證，見下方） |
|------|--------------------|---------------------|
| 統籌 / worker | Opus 統籌、Sonnet worker | GPT-5.5，`medium` reasoning effort（互動編碼預設），處理日常整理流程 |
| 顧問 | Fable 5，決策點諮詢 | GPT-5.5，`xhigh` reasoning effort，僅在真正決策點單次呼叫（成本約 medium 的 8–15 倍，須配合下方次數上限） |

未來 GPT-5.6 發布後，可直接把「顧問」角色換成 GPT-5.6（若確認比 GPT-5.5 `xhigh` 更強），架構不需重新設計，只是替換顧問層的模型/設定值。

### 決策點（掛在哪裡）

不是隨機加，鎖定 kit 裡已經標記為「AI 判斷、advisory-only」的高風險節點：

- **`citation-integrity-check` 檢查 B**（claim-faithfulness：ALIGNED / OVERSTATED / NOT_SUPPORTED 判定）——kit 自己引用「2026 年單年 14.6 萬筆幻覺引用」作為此 skill 的動機，屬於後果最重的判斷點
- **`knowledge-synthesizer` 的 V 層來源衝突判定**——多篇論文結論衝突時的取捨，同樣需要更強的判斷力

### 技術前提查證結果（2026-07-08 已查證）

1. **Codex CLI 確實支援同一 orchestrator session 內、針對特定子任務指定不同 reasoning effort / 模型**——已透過網路查證確認，非假設：
   - Effort 檔位共五級：`minimal` / `low` / `medium`（互動編碼預設）/ `high` / `xhigh`，代幣成本相對 `medium` 約為 0.1x / 0.3x / 1x / 3–5x / 8–15x
   - 三種指定方式：全域 `config.toml`（`model_reasoning_effort` / `plan_mode_reasoning_effort`）、單次 session 旗標（`codex -e high "..."`）、多 subagent 情境下的 `codex-tasks.toml`（可對不同 subagent 分別指定 effort，官方建議「orchestrator 用 high，執行者用 low」）
   - 已有 orchestrator/worker 分層的實測案例：GPT-5.4/5.5 做規劃協調、GPT-5.4-mini 做檔案搜尋/測試/程式碼審查等有界子任務，GPT-5.4-mini 僅耗 GPT-5.4 約 30% 的 credits
   - **已知落地缺口**（[openai/codex#26948](https://github.com/openai/codex/issues/26948)）：subagent spawn 工具**技術上支援**每次啟動指定 reasoning_effort，但工具 schema／文件描述會讓 agent 誤以為無法更改——實作時需明確在 prompt/設定中指定，不能依賴預設行為
   - 另有社群未解決訴求（[openai/codex#11795](https://github.com/openai/codex/issues/11795)）要求在 config 層級更完整地支援 subagent 模型與 effort 配置，代表這塊仍在演進中，非完全成熟穩定的介面

2. **判斷力提升幅度是否值得額外成本**——維持「待實測」狀態，這點無法靠文件查證，需要在 citation-integrity-check 實際跑過的案例上比較 `medium` 與 `xhigh` 的判定差異後才能定論，不納入本次規劃的阻斷條件（先用最小次數上限實測，見下方成本護欄）

**結論**：技術前提成立，可以動手設計；原方案的「用 GPT-5.5 max effort 當顧問層」修正為「用 GPT-5.5 **xhigh** effort」（Codex CLI 實際檔位命名），並改用 `codex-tasks.toml` 或單次 `-e xhigh` 旗標實作，而非等待 GPT-5.6。

來源：[Reasoning Effort Tuning: Minimal to xhigh for Cost and Speed](https://codex.danielvaughan.com/2026/03/27/reasoning-effort-tuning/)、[Codex sub-agent spawn supports per-launch reasoning_effort... #26948](https://github.com/openai/codex/issues/26948)、[Allow configuring subagent model and reasoning_effort in config #11795](https://github.com/openai/codex/issues/11795)

### 成本護欄（比照通用設計原則）

- 次數上限：每篇論文的 citation-integrity-check 最多觸發 1 次顧問諮詢；knowledge-synthesizer 每次合成最多 1 次——`xhigh` 相對 `medium` 約 8–15 倍代幣成本，次數上限比一般護欄更需要嚴格執行
- Fallback：Codex 端若判定無法呼叫顧問層（額度/技術限制，或遇到 #26948 提到的 schema 誤判問題導致 effort 未生效），直接標記該筆判定為「待人工確認」，不中斷 pipeline
- 總開關：AGENTS.md 加一條可關閉此機制的旗標，避免綁死在某個 effort 層級的計費模式上

### 與 kit 分工原則的衝突提醒

kit 的設計鐵則是「教授端一律對話即可，不碰設定」（見 [[claude-fable-5-usage-guide]] 之外的 kit AGENTS.md 記錄）。顧問機制若要自動觸發，必須在 AGENTS.md 寫死規則讓它對教授透明；若只能手動觸發（`/顧問諮詢` 之類），則實務上只有工程師會用，教授不會主動觸發——這點需要在實作前想清楚要做成自動或手動。

---

## 3. claude-pilot-suite 更新：納入 Fable 5，但附註成本警語

### 需要更新的前提

`wiki/claude-pilot-suite.md` 目前的 Opus Pilot 硬規則寫著「Opus 是天花板，子任務只能向下委派給 Sonnet/Haiku」——這條規則寫於 Fable 5 發布之前，現在已經過期：Fable 5（2026-06-09 發布）能力高於 Opus 4.8。

### 更新內容

1. **新增第四層／顧問層**：在 Opus Pilot 模式的 5 個機制之上，加一層「Fable 5 決策點顧問」，比照 fable-advisor 的角色定位——不是新增一個常駐的第四種 Pilot 模式，而是 Opus Pilot 內部的一個可選子機制
2. **修正「Opus 是天花板」措辭**：改為「Opus 是**預設**天花板，特定高風險決策可短暫諮詢 Fable 5」，避免誤導後續讀者以為 Opus 永遠是能力上限

### 必須附註的成本警語（本次規劃的重點修正）

> ⚠️ **Fable 5 目前為限時可用（Anthropic 曾於 2026-06-12 因出口管制全面暫停過一次，見 [[claude-fable-5-usage-guide]]），且定價明顯高於 Opus/Sonnet（輸入 $10/M、輸出 $50/M）。不建議把 Fable 5 諮詢設計成 claude-pilot-suite 的常態路徑，僅適合當作**臨時的、可隨時關閉的**選配子機制。**

具體要求：

- Opus Pilot 文件需明確寫出「若 Fable 5 存取權限或計費條件改變，此子機制應可直接停用，不影響 Opus Pilot 原有三種模式（Haiku/Sonnet/Opus）的運作」
- 不修改 Haiku Pilot、Sonnet Pilot 的分級路由邏輯——Fable 顧問層只掛在 Opus Pilot，因為只有觸及 Opus 天花板的任務才有諮詢更強模型的必要，避免其他兩個模式也牽連進這個不穩定依賴
- 沿用 fable-advisor 的克制心法（僅在高價值決策點、非例行任務）之外，**必須加上本規劃「通用設計原則」的三道硬護欄**，不能只靠文字克制

---

## 執行順序建議（2026-07-08 更新：技術前提已查證，規劃全數完成）

1. ✅ **claude-pilot-suite 更新**（規劃 3）——已完成，`wiki/claude-pilot-suite.md` 已加入 Fable 5 決策點顧問子機制與成本警語
2. ✅ **fox-ai-roundtable 規劃 B——已實作（2026-07-09）**；規劃 C 決策為 ⏸️ 暫緩（見上方重啟條件）
3. ✅ **notebooklm-kit Codex 端**（規劃 2）——技術前提已查證（Codex CLI 確實支援 subagent 分層 effort 配置），角色對應表已改為 Codex 實際檔位命名（`medium` worker / `xhigh` 顧問）
4. ✅ **規劃 A（CLI 擴充選項）——已實作（2026-07-09）**：Ollama / Copilot / OpenCode 三家

> **狀態**：本文件三個目標系統的規劃內容皆已完成設計與（能查證的部分）技術驗證。實際落地仍需要：① 在 `wiki/fox-ai-roundtable-multi-cli-tool.md` 或原始碼 fork 中實作統籌者與 CONTESTED 判定的程式邏輯；② 在 notebooklm-kit 的 `AGENTS.md` / `codex-tasks.toml` 中寫入顧問層設定並實測 `xhigh` 判斷力提升幅度；③ claude-pilot-suite 的文件更新已是規劃終點（該 skill 本身是執行手冊文件，不涉及額外程式碼）。

## 相關連結

- [[fable-advisor-subagent-skill]] — 本規劃借鑑與修正的原始模式
- [[fable-workflow-institutionalization-plan-2026-07-08]] — 上一份規劃，本文件是其延伸與修正
- [[claude-fable-5-usage-guide]] — Fable 5 費用與限時可用性的原始依據
- [[claude-pilot-suite]] — 規劃 3 的更新對象
- [[fox-ai-roundtable-multi-cli-tool]] — 規劃 1 的更新對象
- [[ai-pair-mesh-topology-design]] — CONTESTED 仲裁機制的原始設計依據
- [[loop-engineering-6-components]] — 三道護欄設計原則的出處
