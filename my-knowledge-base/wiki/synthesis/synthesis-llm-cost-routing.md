---
title: LLM 成本效率：任務路由策略 知識精粹
date: 2026-05-22
source_count: 8
synthesis_snapshot_date: 2026-09-03
tags: [ai/workflow, ai/agent, ai/claude]
source: synthesis
type: synthesis
status: processed
sources: 8 篇
asset_value: 85
---

整合六篇從不同切入點討論 LLM 成本優化的條目（四層策略框架、雙工具分工、Gemini 接入細節、Claude Pilot 執行手冊、Claude Code 官方 7 招精簡上下文、deepclaude 換便宜後端），萃取出共同底層邏輯與各篇獨有的反直覺洞見。

## E（精粹論點）

LLM 成本優化最大的誤解是「換便宜模型」。六篇來源指向兩個真正的槓桿，且兩者都不是降規：

**槓桿一 — 任務路由**：路由做對，最便宜的模型（Haiku）加最便宜的工具（Gemini 免費方案）可完成 60-70% 工作，高品質模型的額度留給真正需要推理的 5-10% 任務。AgentOpt 論文是最強的反直覺證明：**弱規劃者 + 強執行者（74.27%）遠優於純 Opus（31.71%）**——多模型協作的架構本身就是成本優化。

**槓桿二 — 上下文體積**：token 成本與上下文大小成正比，所以「你給模型看多少」和「你用哪個模型」是同等量級的變數。`/clear`、PreToolUse hook 過濾冗長輸出、CLAUDE.md 瘦身這類動作，在**不換任何模型**的前提下就能砍掉一半費用。

換言之：**先砍上下文，再談路由，最後才考慮換後端**——順序反了（一上來就換便宜模型或便宜後端）容易換來反覆修正，反而更貴。

---

> 🔴 **2026-09-03 重跑（6→8 篇）：槓桿一拿到新數據，並出現一個本精粹完全沒算進去的第三種成本。**
>
> **① 槓桿一（任務路由）新增實測**：[[fable-5-delegation-cost-benchmark]]（Cognition）——🔑 **Fable 5 當「主管」比 Opus 4.8 更便宜**，因為委派策略本身決定成本。這與 AgentOpt 的「弱規劃者＋強執行者」是同一命題的另一側：**貴的模型該放在哪一格，比用不用貴模型更重要。**
>
> **② 🔴 而 [[delivery-speed-vs-maintenance-cost]] 指出本精粹漏算了一整類成本**：
> 本精粹的兩個槓桿（任務路由、上下文體積）都在優化 **API 帳單**。但那篇用五個月實際帳單顯示——**AI 只讓「程式碼誕生的那一刻」變便宜，而軟體的一生是被閱讀、除錯、週五緊急修改、向新人解釋**，這部分跟 2019 年一樣貴、甚至更貴（因為程式碼變多了）。
>
> 🔑 **所以完整的成本模型是三層，而不是兩個槓桿：**
>
> | 層 | 優化手段 | 本精粹原本涵蓋 |
> |---|---|---|
> | ① 上下文體積 | `/clear`、hook 過濾、瘦身 | ✅ |
> | ② 任務路由 | 弱規劃＋強執行、模型分層 | ✅ |
> | 🆕 ③ **維護成本** | merge 前先讀、拒絕非最小解、review 排隊量 | 🔴 **完全沒算** |
>
> ⚠️ **而第三層可能是最大的一層**：「我省下的是那個上午。後面三次回頭去改，沒有省到。」

## C（共識核心）

> 出現於 6 篇來源（各點標註實際覆蓋）

- **優化的槓桿點是路由，不是模型降規**：正確的任務給正確的模型（讀取→免費、推理→付費）效益遠大於全面換便宜模型（[[llm-cost-optimization-strategies]]、[[dual-tool-strategy]]、[[gemini-cli-subagent]]、[[claude-pilot-suite]]、[[claude-code-cost-optimization-7-tips]]、[[deepclaude-cost-optimization]]）
- **重度使用者才有優化必要**：輕度使用者用預設設定就夠，過度優化反而增加複雜度（[[dual-tool-strategy]]、[[claude-pilot-suite]]、[[claude-code-cost-optimization-7-tips]]、[[deepclaude-cost-optimization]]）
- **任務類型是分流判斷標準**，不是任務難度：讀取/搜尋/摘要→免費工具；規劃/架構/安全→付費模型（[[dual-tool-strategy]]、[[gemini-cli-subagent]]、[[claude-code-cost-optimization-7-tips]]、[[deepclaude-cost-optimization]]）
- **失敗是升級信號**：≥3 次失敗或觸及架構/安全關鍵詞才升級模型，主動降級才是預設（[[claude-pilot-suite]]、[[deepclaude-cost-optimization]]）

---

## A（獨特視角）

| 來源                                   | 獨特貢獻                                                                                                                                                       |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[llm-cost-optimization-strategies]] | **四層疊加框架**（Cache → 跨平台分散 → 複雜度路由 → Semantic Cache），疊加效益 60-80%；**第五層「知識萃取」**：Claude 設計邏輯 → Python 腳本執行，重複任務 token 消耗降至 10%（實測：2500 份 PDF 分類從燒光整週額度降至 ~10%） |
| [[dual-tool-strategy]]               | **工具角色化隱喻**（手術刀 vs 廚房菜刀）讓分工直覺化；**AGENTS.md 單一維護點**：多工具共用規則，修改一份自動同步（B2B Manager 實測 context 從 315 行降至 250 行）                                                |
| [[gemini-cli-subagent]]              | **「傳路徑不傳內容」原則**：直接讀比先讀再轉省 50% token；**`excludeTools` 安全邊界必設**（實測：未設定時 Gemini 可能寫入/刪除檔案）；Gemini 背景模式無工具時不報錯、直接幻覺的**具體風險警告**                                 |
| [[claude-pilot-suite]]               | **AgentOpt 論文數據**：弱規劃者 74.27% vs 純 Opus 31.71%（多模型委派架構本身就是優化）；**Opus 只能向下委派**的硬規則防止成本膨脹；**Sonnet 6 步驟前置檢查**防止高信心偏誤跳過驗證                                     |
| [[claude-code-cost-optimization-7-tips]] | **唯一把「上下文體積」當主軸**而非模型選擇：`/clear`÷`/compact`÷`/context` 三指令分工；**PreToolUse hook 過濾測試輸出**（數萬 token → 數百，最有感的一招）；`--max-budget-usd` + `--max-turns` + `--bare` 的 CI 防護；**Agent Teams token 約為一般 session 7 倍**的放大警告；vague prompt 是最大隱形開銷 |
| [[deepclaude-cost-optimization]]     | **唯一「換後端不換工作流」路線**：只改 `ANTHROPIC_BASE_URL`/`AUTH_TOKEN` 環境變數，agent loop 與 tool dispatch 全不動；價差實測（Output 17.2x，但經 OpenRouter 實際約 3-5x 的**誠實修正**）；後端延遲對照表；**AI 成本節省五件套**分層（deepclaude→ds4→headroom→agentmemory→opensquilla）|
| 🆕 [[fable-5-delegation-cost-benchmark]] | **槓桿一的新實測**：Cognition 測出 🔑 **Fable 5 當主管比 Opus 4.8 更便宜**——委派策略決定代理成本，貴模型放哪一格比用不用更重要 |
| 🆕 [[delivery-speed-vs-maintenance-cost]] | 🔴 **本精粹漏算的第三層成本**：AI 只讓程式碼「誕生」變便宜；🔑 **讀 200 行 10 分鐘、除錯沒讀過的 200 行一整天**；🔑 **AI 幾乎從不給最小解**（多的每一行都變成某人要讀的東西）；🔴 review 速度沒跟上導致 LGTM 橡皮章。**這些成本不出現在任何 API 帳單上** |

---

## V（知識缺口）
- 🆕 🔴 **維護成本無法用 token 計價，因此完全落在本精粹的視野外（2026-09-03）**：三層成本模型的第三層（維護）只有質性描述與單一案例，**沒有任何來源提出可比較的量化方式**。這意味著「省了 50% token」與「多了三次回頭修改」目前無法放在同一張表上權衡
- 🆕 ⚠️ **省 token 的手段可能推高維護成本**：例如「不給模型看太多上下文」會讓它產出更不貼合既有架構的程式碼。**兩個槓桿與第三層之間的交互作用無人討論**

- ❓ **路由判斷本身的 overhead**：決定「這個任務給哪個模型」也需要消耗 token，六篇都沒有量化這個分類成本 vs 節省效益的比例
- ⚠️ **訂閱制與 API 計費被混為一談**：[[claude-code-cost-optimization-7-tips]] 點出 Pro/Max 訂閱者的 `/cost` 不代表額外收費（應看 `/stats`），但其餘來源談的「省錢」多半預設 API 按量計費——**同一招對兩種計費模式的實際效益天差地別**，沒有一篇把兩者分開討論
- ⚠️ **換後端的相容性代價未量化**：[[deepclaude-cost-optimization]] 誠實列出「extended thinking 不一定對應、少數 tool calling 邊緣案例有問題」，但這些失敗會造成多少額外重試成本，沒有數據——省下的錢可能被重試吃掉
- ⚠️ **多模型輸出一致性**：Claude / Gemini / Haiku 在同一工作流中的格式、風格不統一，整合時的摩擦成本被低估
- ❓ **Prompt Cache 命中率的影響因子**：CLAUDE.md 超過 1024 token 才啟用，但實際命中率受哪些因素影響（對話中斷、context 壓縮）沒有說清楚

---

## 整合行動清單

- [ ] **先做上下文瘦身再談路由**：建 PreToolUse hook 過濾測試輸出、任務切換用 `/clear`、CLAUDE.md 壓到 200 行內（不換模型就先砍一半）
- [ ] 依任務類型建立分流判斷表（讀取/搜尋 → Gemini CLI，複雜推理 → Claude，批次分類 → Python 腳本）
- [ ] CI/CD 一律加 `--max-budget-usd` 與 `--max-turns`；用 Agent Teams 前先意識到 **token 約 7 倍**放大
- [ ] 確認自己是「訂閱制」還是「API 按量」——訂閱制該優化的是同配額內做更多事，而非帳單金額
- [ ] 設定 `excludeTools` 安全邊界後，把探索性任務移到 Gemini CLI（優先處理「唯讀探索」類）
- [ ] 預設使用 haiku-pilot，只在失敗 ≥3 次或觸及架構關鍵詞時才升級
- [ ] 對重複執行的批次任務：用 Claude 設計邏輯 + 寫腳本，後續讓 Python 執行（第五層知識萃取）

---

## 單平台 Pilot Suite 模式：Copilot Pro 與 Codex CLI 的模型分層

與 claude-pilot-suite 相同的「預設便宜層、條件升級」邏輯可套用到任何有多模型層級的平台。

### GitHub Copilot CLI Pro（$10/月）

> ⚠️ 2026-06-01 起改為 AIC 點數計費，模型選擇直接影響帳單
> 以下模型清單與倍率來自 CLI 實測（2026-05-22）

| 層級 | 模型 | AIC 倍率 | 適合任務 |
|------|------|---------|---------|
| **免費層** | GPT-5 mini | **0x** | 簡單問答、格式整理、快速查詢 |
| **免費層** | GPT-4.1 | **0x** | 中等複雜度任務，0 成本 |
| 便宜層 | GPT-5.4 mini | 0.33x | 輕量編程、小範圍修改 |
| 便宜層 | Claude Haiku 4.5 | 0.33x | 快速問答、簡單審查 |
| 標準層 | GPT-5.4（預設）| 1x | 日常編程任務 |
| 標準層 | GPT-5.3-Codex | 1x | 編程優化，與 GPT-5.4 同價 |
| 標準層 | GPT-5.2-Codex | 1x | 編程優化（舊版）|
| 標準層 | GPT-5.2 | 1x | 長時間 Agent / 專業任務 |
| 標準層 | Claude Sonnet 4.6 | 1x | 高品質推理、跨檔案修改 |
| 標準層 | Claude Sonnet 4.5 | 1x | 同上（舊版）|

**Pilot Suite 套用原則**：
1. **先用 0x 免費層**（GPT-5 mini / GPT-4.1）：簡單任務、探索、格式整理 → 完全不燒額度
2. **0.33x 層**（GPT-5.4 mini / Claude Haiku 4.5）：輕量編程，額度只用 1/3
3. **1x 層**（GPT-5.3-Codex / Claude Sonnet 4.6）：真正複雜的任務才升，且同為 1x 倍率，優先選品質最高的

> 關鍵發現：Copilot CLI Pro $10 方案無高倍率貴層（GPT-5.5 的 7.5x 不在此方案），1x 即為天花板，代表不存在「誤選貴模型爆帳」的風險。

> ⚠️ **時間點注意**：AIC 計費 2026-06-01 才生效。目前顯示的 0x / 0.33x / 1x 倍率是預告值，現階段所有模型實際上都免費。六月後 Pilot Suite 策略是否需要調整，取決於 0x 模型是否維持免費，需在 2026-06 後重新確認實際帳單。

> 📌 **待研究**：AIC 生效後 GPT-5 mini / GPT-4.1 的 0x 是否維持（免費）或改為計費；GPT-5.3-Codex vs Claude Sonnet 4.6 在同為 1x 情況下的任務分工（編程特化 vs 推理通用）；0x / 0.33x 模型的隱藏請求數量上限。
> 🗓️ **2026-06 後需更新此條目**：以實際帳單數據取代預測值。

---

### Codex CLI（隨 ChatGPT Plus $20/月）

> Codex 無獨立訂閱，包含在 ChatGPT Plus/Pro/Business 方案中
> 以下模型清單來自 CLI 實測（2026-05-22）

| 層級 | 模型 | 描述 | Pilot Suite 對應 |
|------|------|------|----------------|
| 貴層 | gpt-5.5 | Frontier，複雜編程 / 研究 / 真實世界任務 | Opus 等級，謹慎使用 |
| 中高層 | gpt-5.4 | 日常編程的強力模型 | Sonnet 等級 |
| **便宜層**（建議預設）| **gpt-5.4-mini** | 小、快、省，適合簡單編程任務 | Haiku 等級 ✅ |
| 特化層 | gpt-5.3-codex | 編程優化模型 | 複雜程式碼任務替代選項 |
| Agent 層 | gpt-5.2 | 長時間運行 Agent / 專業工作優化 | 多步驟 Agent 任務專用 |

**Pilot Suite 套用原則**：
- 預設 `gpt-5.4-mini`（快速省額度，簡單任務足夠）
- 跨多檔案、邏輯複雜 → 升 `gpt-5.4`
- 純程式碼重構 / 複雜 debug → 考慮 `gpt-5.3-codex`（編程特化）
- 長時間 Agent 任務 → `gpt-5.2`（為此設計）
- `gpt-5.5` 只留給真正需要 frontier 推理的場合

**額度限制**：以 5 小時視窗 + 週配額計算，Plus 使用者回報額度消耗速度快、容易撞限。

> 📌 **待研究**：各模型的確切額度消耗比例未公開（gpt-5.5 vs gpt-5.4-mini 的額度倍率差異）；`gpt-5.3-codex` 與 `gpt-5.4` 在實際編程任務的品質差異需實測；Plus 方案週配額確切數字官方仍未公開。

---

## 實戰驗證：Claude Pro + Copilot CLI Pro 分工模式

這是「跨平台分散額度」原則的具體實作，也是 ai-pair 模式的變體：

| 角色 | 工具 | 消耗的額度 |
|------|------|-----------|
| 規劃 + 審查（高推理）| Claude Pro | Claude 訂閱額度 |
| 開發執行（高程式碼生成量）| GitHub Copilot CLI Pro | Copilot 訂閱額度 |

**為什麼這符合 E 層論點**：Claude 的滾動額度只用在最需要推理的工作（規劃架構、審查邏輯），大量的程式碼生成交給 Copilot 吃自己的額度——路由做對，不是降規，而是讓每個工具做它最擅長的事。

**與 ai-pair 的關係**：ai-pair 的角色設定是 Claude 創作 + Codex 分析審查 + Gemini 編輯審查（三模型互補視角）。此處的 Claude Pro + Copilot 是同一思路的雙模型版：Claude 負責規劃/審查（Team Lead），Copilot 負責執行（Developer）。若品質要求更高，可在審查階段加入 Gemini CLI（免費）組成三層，接近完整 ai-pair 架構。

**實際預算**：Claude Pro $20/月 + GitHub Copilot Pro $10-20/月，合計 $30-40/月。相較於單一 Max 方案（$100-200/月），總支出降低 70-80%，且兩平台額度互相補位，不會因單一平台燒完而停擺。

**適用前提**：兩個訂閱都已存在，額度互不佔用，分散才有實質意義。Max 方案（$100-200）的邊際效益在重度使用者才顯現；一般開發強度下，$20+$10-20 的雙平台組合是更務實的起點。

## 來源條目

- [[llm-cost-optimization-strategies]] — 四層疊加框架：Cache / 跨平台 / 複雜度路由 / Semantic Cache（+ 知識萃取第五層）
- [[dual-tool-strategy]] — 雙工具分工：Gemini CLI 廚房菜刀 + Claude Code 手術刀，AGENTS.md 單一維護點
- [[gemini-cli-subagent]] — Gemini 接入細節：傳路徑原則、excludeTools 安全設定、背景幻覺風險
- [[claude-pilot-suite]] — Claude Pilot 執行手冊：AgentOpt 論文、三模式升降規則、Opus 向下委派硬規則
- [[claude-code-cost-optimization-7-tips]] — Claude Code 官方管道內 7 招：上下文體積為主軸、PreToolUse hook、CI 費用防護（AV 62）
- [[deepclaude-cost-optimization]] — 換便宜後端不換工作流：環境變數切換、價差誠實修正、成本節省五件套（AV 58）
- [[fable-5-delegation-cost-benchmark]] — 🆕 Cognition 實測：Fable 5 當主管比 Opus 4.8 便宜，委派策略決定成本（AV 80）
- [[delivery-speed-vs-maintenance-cost]] — 🆕 🔴 第三層成本：交付變快、維護沒變的五個月帳單（AV 82）
