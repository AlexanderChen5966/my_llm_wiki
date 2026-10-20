---
title: LLM 成本優化四層策略：Caching / 跨平台分散 / 複雜度路由 / 批次處理
date: 2026-05-20
tags: [ai/workflow, ai/agent]
source: https://zylos.ai/research/2026-02-19-ai-agent-cost-optimization-token-economics
type: reference
status: processed
asset_value: 80
---

四層疊加可達 60-80% 總成本降低：Prompt Caching 省固定 context 費用、跨平台分散避免單一額度耗盡、複雜度路由自動分流輕重任務、Semantic Cache 讓熱點查詢跳過模型呼叫。

## T（核心論點）

Agent 系統比單次聊天多消耗 3-10x token（規劃 + 工具選擇 + 執行 + 驗證 + 回應），優化不在「換小模型」，而在**正確的層次做正確的事**：重複 context 用快取、輕量任務用輕量模型、多提供商分散額度風險、熱點查詢完全跳過模型。

## B（適用場景）

**適用：**
- 重度使用 Claude Code / 多 Agent 工作流，每月費用明顯
- 工作流中有大量重複性子任務（分類、摘要、格式轉換）
- 使用多個 AI 提供商（Claude + Copilot + Gemini）
- 知識庫查詢類任務（同類問題反覆出現）

**不適用 / 注意：**
- 輕度使用者（直接用預設設定即可）
- Semantic Cache 需要額外基礎設施（向量 DB），小規模不值得
- Compaction API 仍在 beta，穩定性需觀察

## R（四層策略詳解）

### 第一層：Prompt Caching（最大槓桿）

**原理**：固定的 system prompt / CLAUDE.md / Skill 指令做 cache，後續呼叫只計算新增部分。

**效果**：Anthropic cache read = **0.1x** 原始 input 價格（省 90%）

**適用位置**：
- CLAUDE.md 內容（每次工作階段都讀）
- Skill 的完整指令（raw-pipeline / ai-pair 的 SKILL.md）
- 知識庫的 INDEX.md（頻繁查詢）

**Compaction API（2026-02 beta）**：Opus 4.6 自動壓縮對話歷史，長工作階段（如 raw-pipeline 批次處理）最有效。

---

### 第二層：跨平台分散額度

**原理**：把不同角色的 token 消耗分配給不同提供商，避免單一額度耗盡。

**效果**：Claude 額度緊張時工作流不停擺；免費模型吸收審查/分類成本。

**可用免費 / 低成本模型**：

| 提供商 | 模型 | 成本 | 適合角色 |
|--------|------|------|---------|
| Copilot | gpt-5-mini | FREE | 簡單審查、分類 |
| Copilot | gpt-4.1 | FREE | Fallback 審查 |
| Copilot | gpt-5.4-mini | 0.33x | 標準審查 |
| Gemini | gemini-3.1-flash-lite-preview | 極低 | 編輯型審查 |
| Gemini | gemini-2.5-flash | 低 | 標準審查 |

**最佳組合**：Claude 負責規劃/創作（核心邏輯），Copilot + Gemini 負責審查/驗證（分散消耗）。

---

### 第三層：複雜度路由（Complexity-Based Routing）

**原理**：任務進入前先判斷複雜度，自動路由到對應層級的模型。

**效果**：實測減少 **37-46%** 整體 LLM 使用量，簡單查詢快 32-38%。

**判斷維度**：

| 訊號 | 輕量模型 | 升級模型 |
|------|---------|---------|
| Token 數 | < 500 | > 2000 |
| 關鍵詞 | 摘要/分類/格式 | 分析/設計/架構/安全 |
| 失敗次數 | 0 | ≥ 3 次 |
| 輸出長度 | < 400 字 | > 800 字 |

**工具**：RouteLLM（開源，自動路由）、LiteLLM（統一 API 介面）

**手動版**：Pilot Suite 的 Haiku Pilot 規則（失敗 ≥3 → 升 Sonnet；架構/安全關鍵詞 → 升 Opus）

---

### 第四層：Semantic Cache（熱點跳過）

**原理**：向量相似度 > 0.95 的查詢直接回傳快取結果，完全不呼叫模型。

**效果**：熱點查詢 0 token 消耗；整體省 45-80%（依重複率）。

**適用**：知識庫查詢（同一概念被反覆問）、FAQ 類任務。

**工具**：GPTCache、Zep（向量 DB + cache 層）

**注意**：需額外部署，小規模使用 overhead 大於收益，wiki 超過 200 篇、查詢頻繁後再考慮。

---

## C（關鍵行動）

- [ ] **立即可做**：確認 CLAUDE.md / Skill 指令是否觸發 Prompt Cache（固定 context 超過 1024 token 才啟用）
- [ ] **短期**：ai-pair 工作流設定 Economy / Balanced / Quality 三模式，指定 Copilot 和 Gemini 的輕量模型
- [ ] **中期**：評估 RouteLLM 或 LiteLLM 是否值得引入，依實際費用決定
- [ ] **長期**：知識庫查詢量大後評估 Semantic Cache（GPTCache / Zep）

## 原始摘要

搜尋來源（2026-05-20）：zylos.ai AI Agent Token Economics、swfte.com Intelligent LLM Routing、maviklabs.com LLM Cost Optimization 2026、getmaxim.ai Top 5 LLM Routing Techniques、obviousworks.ch Token Optimization 2026。實測數據：Prompt Cache 省 90% input、複雜度路由減少 37-46% LLM 使用、Semantic Cache 省 45-80%、整體疊加可達 60-80% 總成本降低。

## 補充資料：NotebookLM + Claude 分工節省 17 倍費用

來源：DeTools 工具翼零（tools.wingzero.tw，2026-04-22），整理自 @MinLiBuilds 的 X 貼文。

**核心概念**：讓 NotebookLM 負責「資料檢索」，Claude 專注「推理與生成」，兩者分工可節省 17 倍 Token 費用並降低錯誤率。

**原理**：Claude 做 RAG 時需要把大量文件塞進 context（昂貴），改用 NotebookLM 先檢索出最相關段落，再把精簡後的資料交給 Claude 推理——Claude 只需處理小量高品質輸入。

**適用場景**：
- 論文研究、大型文件分析
- 企業知識庫問答（文件量大）
- 需要引用來源的長文生成

**對應本條目第二層「跨平台分散額度」的延伸**：不只是分散 token 額度，更是把「檢索」這個工作完全外包給免費/低成本工具。

## 相關連結

- [[dual-tool-strategy]] — Gemini CLI + Claude Code 成本分工（第二層的實作範例）
- [[gemini-cli-subagent]] — Gemini CLI 接入 Claude Code 的具體設定
- [[claude-pilot-suite]] — Pilot Suite 三模式（第三層的 Claude 內部實作）
- [[ai-pair-skill]] — 跨平台多模型審查架構（第二層的審查端應用）
- [[claude-code-context-management]] — Context 管控與 Compaction 機制
- [[llm-judge-confidence-cascade]] — 評審成本的分層路由：信心夠就用便宜評審、不確定才升級
- [[claude-opus-5-5-release-notes]] — Opus 5.5 單價降 20%、cache read 降 60%，「成本降 40%」要用合格任務成本驗證
- [[claude-code-prompt-audit-scope-trap]] — `/doctor prompt-audit` 在 `-p` 下看不到 `~/.claude`，實際有效的是設定化石偵測（與下方「Claude API 官方三招」段的 `/claude-api prompt-audit` 依本機 changelog 為不同指令，待官方文件核對）
- [[gpt-6-astra-prompting-guide]] — OpenAI GPT-6 單次輸入 >272K tokens 整筆加價（輸入與快取 2 倍、輸出 1.5 倍），長 context 任務要算進每任務成本（2026-09-29 補）

## 補充資料：5 個省 Token 好習慣（PDF 處理差 22 倍）

來源：數位時代（bnext.com.tw，2026-05-06）《工程師5招「省token」大法》

**最關鍵發現：PDF 直接丟 vs 先轉 .md 再丟，token 消耗差了 22 倍。**

五個習慣：
1. **先把 PDF 轉 .md** 再提供給 AI——去除版面、圖形、編碼等非文字資訊，大幅降低輸入 token
2. **任務分段**：別一次丟全部，拆成獨立小任務，每次 context 都精準
3. **善用 /clear**：長對話後手動清除，避免舊訊息累積成「隱性成本」
4. **指定模型**：簡單問題用 Haiku，不要讓 Opus 做低複雜度任務
5. **格式指定輸出**：「用條列式回答，100 字以內」比「告訴我怎麼做」節省輸出 token

> PDF 22 倍差異的實際意義：處理 10 份 PDF 文件，若先轉 .md，相當於只消耗 1 份 PDF 的 token 量。這是最高槓桿的單一操作。

---

## 補充資料：Caveman — Prompt Compression 極端方案

來源：Ashley Hsieh（cyberq.tw，2026-05-11）《Caveman 幫 AI 開發者暴力省下 75% Token 成本》

**核心概念**：強制 AI 用原始人方式說話——省略冠詞、刪除修飾語、避免客套話、關鍵詞取代完整句子。本質是 Prompt Compression 技術。

**實測效果**：減少 65-75% output token；21,000 token 工作流壓縮至 4,800 token（省 77%）。

**支援範圍**：Claude Code、Gemini CLI、Cursor、Copilot、Cline、Windsurf 等 40+ 工具，單行指令安裝。

**生態系**：
- **Cavemem**：長效記憶管理
- **Cavekit**：規格導向開發自動化工作流
- **文言文模式**：比英文壓縮更進一步

**副作用警告**：過度壓縮可能降低複雜推理能力（output token 本身就是推理過程的一部分）。作者本人坦承高風險開發工作未必敢用。

**中文環境警告**（2026-07-13 補充）：Caveman 的壓縮句法建立在英文 tokenizer 特性上，官方 benchmark 數據全為英文任務；中文 1 字約 1-2 token、電報式中文省得有限且易生歧義。**中文工作環境、輸出以程式碼為主時，改用 Ponytail（[[ponytail-lazy-senior-dev-skill]]，語言中立的程式碼減量方案）更可靠**——兩者作用軸線不同（Caveman 管對話文字、Ponytail 管生成程式碼），完整比較見 [[github-copilot-token-reduction]]。

> 對應本條目**第七層（輸出格式控制）**的極端版本；適合資訊密度需求高、可容忍簡短輸出的**英文**工作流。

---

## 補充資料：Token 通膨與知識萃取策略

來源：Stan Wu（Vocus，2026-05-03），《當 Claude 額度燒太快，我決定自己寫 Python》。

### Token 通膨現象

**Token 通膨**：訂閱費不變，但實質算力購買力持續縮水。

- Anthropic 2026-03 公開承認：「用戶消耗 Claude Code 額度的速度遠超預期」
- $200/月 Max 方案訂閱者，最快不到 20 分鐘見底
- 結構性原因：Nvidia Blackwell 晶片租用價格 2026 年初兩個月內上漲 48%（達每小時 $4.08），交期拉長至 36-52 週

> AI 算力現在類似早年 54kbps 數據機時代——每個 byte 都得精打細算，這是暫時的供需問題，但在算力跟上之前，優化策略的重要性提升。

### 第五層：知識萃取（Claude 設計，腳本執行）

**原理**：把 Claude 的判斷邏輯一次性萃取為可重複執行的腳本，後續執行完全繞開 AI。

**舊流程（高耗損）**：我 → prompt → Claude → 臨時產生工具 → Claude 自己使用 → 噴 token

**新流程（知識萃取）**：我 → prompt → Claude → 萃取判斷邏輯 + 寫 unit test → 腳本執行（token 消耗 = 0）

> 提示詞：「請將你剛剛用的工具存到 ~/Desktop 上，並附上詳細使用說明書 md 檔案」

**實測對比（2500 個 PDF 分類任務）**：

| 維度 | 舊流程（Claude 逐一執行）| 新流程（Claude 寫腳本，Python 執行）|
|------|---------|---------|
| 執行時間 | < 10 分鐘（額度清零）| < 3 分鐘（Python 跑完）|
| Token 消耗 | 100%（燒光整週額度）| ~10%（僅寫腳本那一次）|
| 可重用性 | 每次重頭來過 | 腳本固定隨時可跑 |
| 行為一致性 | 每次略有差異 | 完全一致 |

**本質**：「AI 負責思考，程式碼負責記憶，執行交給機器」——把 AI 的邊際成本降到零。重複邏輯做一次性萃取，等同於企業知識管理的「把隱性知識固化為流程」。

**適用條件**：邏輯固定 + 大量重複執行（批次分類、格式轉換、批次重命名）。**不適用**：需要 AI 靈活判斷的非結構化任務。

**意外收穫**：邊緣情況（如 owner password vs user password）讓 Claude 一次思考清楚，固化進腳本，後續不再重複消耗 token。

---

## 補充資料：Claude API 官方三招（Effort 設定與 Prompt Audit 實測數據）

來源：李先泰（數位時代 bnext.com.tw，2026-09-11），整理自 Anthropic 工程師 Lance Martin 2026-09-08 公開的實測案例（客服應用從 Opus 4.8＋預設 effort 調整到 Sonnet 5＋低 effort＋任務分流規則，準確率從 78.6% 升到 90.5%，成本降至約五分之一）。與本條目第一層 Prompt Caching 互補，補強該篇既有內容未涵蓋的兩項戰術：

**戰術一：Effort 參數設定（output_config.effort）**

- 可選層級：`low` / `medium` / `high` / `xhigh` / `max`，不設定時預設為 `high`（幾乎每個任務都投入較多推理）
- 純分類、摘要、格式轉換等簡單任務用 `high` 可能花掉不必要成本；調低不等於回答變短，影響的是模型思考與查資料的多寡，不是字數
- 實測：新模型 Fable 5.1 用 `low`，成績與前一代 Fable 5 用 `high` 相近，費用僅三分之一——換新模型不必急著開高 effort，先從低的試起
- 無公式可循，只能用固定測試題逐級測試，找到準確率明顯下滑的層級，停在它的前一級
- ⚠️ 兩個限制：調低後需確認需要多步推理的任務品質是否下滑；**同一段對話中途改 effort 會讓已存快取失效**，應一開始就選定，不要邊聊邊改

**戰術二：Prompt Audit（提示詞審查）——降成本 14.6%、準確率 +5.3%**

- 換新模型時提示詞若原封不動，常留有舊模型時代的保險叮嚀（「回答前再檢查一次」「一定要非常仔細」「請照這幾個步驟想」）
- 新模型自己就會推理，這些叮嚀變成多餘工作：「再檢查一次」會讓模型把同一筆訂單查兩遍；兩條互相矛盾的舊規則會讓模型不敢執行本該做的動作（如該退的款不敢退）
- **實測數據**：刪除這類多餘指令後，同一個新模型平均**成本降 14.6%、準確率升 5.3%**
- 做法：升級前把提示詞從頭讀一遍，業務相關規則（如退款前確認訂單狀態）保留，純粹「再想一次」「再確認一次」類指令刪除；Claude Code 使用者可直接跑 `/claude-api prompt-audit` 找出這類叮嚀
  - ⚠️ 待核對（2026-09-29 附註）：依本機 changelog，`/claude-api prompt-audit`（2.1.221，claude-api skill 子指令，審提示詞與 tool description）與 `/doctor prompt-audit`（2.1.283，內建指令，審 CLAUDE.md／skills／agents／commands）兩者並存，官方文件頁待核對；後者的範圍陷阱見 [[claude-code-prompt-audit-scope-trap]]

**驗收方法**：固定測試題分兩份（一份反覆調設定用，一份留到最後驗收），每改一項設定就重新比較成本與準確率；`/claude-api hillclimb` 自動反覆試設定，`/claude-api cost-optimize` 先盤點錢花在哪再逐項試省錢做法。建議順序：先看錢花在哪 → 清 Prompt（Audit）→ 試 Effort → 最後用 Message Batches（非即時任務再打五折，可與快取折扣疊加）。

**與 [[claude-code-cost-optimization-7-tips]] 的關係**：該篇聚焦 Claude Code IDE 情境的省 Token 技巧，本篇補充的是 Claude API/SDK 呼叫層級的官方優化方法，兩者情境互補、不重疊。

---

## 補充資料：Copilot 用量計費震撼彈與跨平台省錢實作

### GitHub Copilot 改為 Usage-Based 計費（2026-06 生效）

來源：Rewrite（Vocus，2026-05-16）《Copilot 吃到飽終結》

**震撼彈**：GitHub Copilot 2026 年 6 月起全面從「月費吃到飽」改為「用量計費（AICs 點數制）」。

**實測衝擊（Pro+ $39 方案，4 月實際用量）**：
- 4 月使用量：47,473 AICs
- 新制額外費用：$404.73 美金
- 換算月帳單：$39 → $443（約台幣 15,000 元）
- GitHub 建議：升級至 $100/月 Max 方案（即便如此仍需付 ~$370/月）

**對比 Claude Code Max（$200/月）**：滾動式容量限制（每 5 小時 Token 上限，時間到自動回補），重度使用者的成本上限更可預測。

**自保策略（對應第三層複雜度路由的實踐）：**
1. **優化 Prompt Big O**：精準選取代碼片段，精算每次對話的 context 大小
2. **大小任務分流**：
   - 程式碼自動補全（Code Completions）：仍免費，繼續使用
   - 小聊天/抓錯字：切換到輕量模型（Claude Haiku、Gemini Flash），幾乎不花費
   - 跨檔案複雜 Bug / 大架構重構：才用高階模型或 Claude Code

---

## 補充資料：Claude Code 7 個 Token 省 80% 實用技巧

來源：Tony（codelove.tw，2026-05-21）《7 個實用技巧，讓 Claude Code 的 Token 消耗暴降 80%》

> 核心洞察：真正「燒 Token」的不是 Prompt，而是背後帶著的整段 Context（聊天記錄、已讀程式碼、工具輸出、CLAUDE.md、系統指令）。

**7 個技巧（對應本條目的四層策略）：**

1. **依任務複雜度切換模型**（對應第三層）：日常任務→ 適中模型；複雜任務→ 高階模型；輕量操作→ 低參數模型。搭配 `/effort` 降低思考預算。
2. **把 CLAUDE.md 當規則索引而非百科全書**：只放核心約束，避免每次重複輸入專案規範。
3. **善用 /clear 重開 Context**：完成一個功能後立即 /clear，防止舊對話記錄累積佔用 Token。
4. **用 Prompt Caching 固定 Context**（對應第一層）：CLAUDE.md、Skill 指令定期觸發 cache 熱點（需超過 1024 token）。
5. **避免不必要的檔案讀取**：明確指定路徑，不讓 Claude 廣泛搜索；大檔案只讀相關片段。
6. **子任務拆分 + 子 Agent 並行**：大任務拆成獨立子任務，各自開新 context 執行（對應 Claude Code 的 Agent 模式）。
7. **輸出格式控制**：指定簡短輸出格式，避免 Claude 自動產生冗長說明。

---

### 非官方實驗：Claude Code → Copilot Bridge

來源：James Tsai（2026-05-09）《讓 Claude Code 轉串接到 GitHub Copilot 服務》

> ⚠️ **聲明**：非官方推薦，僅限實驗。可能因 API 調整隨時失效，不建議生產環境使用。

**使用情境**：已有 Copilot 訂閱，不想再額外訂閱 Claude 方案（Copilot Business 包含 Opus/Sonnet 等 41 種模型）。

**原理**：透過 `copilot-bridge`（betaHi/copilot-bridge）建立本地代理，把 Claude Code 的 API 請求轉發到 GitHub Copilot 服務。

**設定方式：**

```bash
npm exec betahi-copilot-bridge@latest -- start --port 4142 --account-type individual
```

在 `~/.claude/settings.json` 加入：

```json
"env": {
  "ANTHROPIC_BASE_URL": "http://127.0.0.1:4142",
  "ANTHROPIC_AUTH_TOKEN": "dummy",
  "ANTHROPIC_MODEL": "gpt-5.5"
}
```

**注意**：Copilot 改為 AIC 計費後，高階模型（gpt-5.5 = 7.5x 倍率）仍快速消耗點數。省的是「不需另購 Claude 訂閱」，但 Copilot 用量成本依然計算。Claude Desktop 不適用此方案。

---

## 補充資料：LangSmith 觀測 → 閘道 → 路由（aiposthub 轉述 LangChain，2026-10-05）

來源：https://www.aiposthub.com/langsmith-coding-agent-cost-gateway-router/ （轉述 Harrison Chase 的 X 貼文與兩篇 LangChain 官方部落格）

> ⚠️ **二手轉述**，原文**沒有任何降幅數字**；官方說路由實驗結果限於其測試條件，作者分享也沒有可套用到所有團隊的固定降幅。
> 作者 Philo 在同站其他文章重複使用「每個成功任務成本」等措辭，**不算多來源共識**。

本節補的是第三層「複雜度路由」的**前置層**：改路由之前，先要看得到錢花在哪裡。

| 步驟 | 工具／位置 | 負責什麼 |
|---|---|---|
| 1. 觀測 | LangSmith 執行軌跡 | 整理不同程式代理的模型輸入輸出、token、成本、時間與子代理，找出重複花費 |
| 2. 閘道 | LLM Gateway（docs.langchain.com/langsmith/llm-gateway） | 集中管理模型存取與用量 |
| 3. 路由 | 在 Harness（代理執行框架）內 | 把模型選擇邏輯放進執行框架 |

- **順序：先觀測，再改路由**——沒有軌跡就不知道該把哪類任務降到便宜模型
- **指標（作者定義）**：每個成功任務成本＝（成功＋失敗嘗試的總費用）÷ 達標任務數——只看單次呼叫費用會漏掉重試與失敗的成本
- **子代理交接失誤案例**（官方案例轉述）：主代理把子代理指向用舊分頁方式的程式，使用者一直重述需求卻沒修背景來源；看了交接紀錄才找到真正要修的指引——成本浪費的源頭常在交接，不在模型
- ⚠️ 部分工具若直接用自己的 API 金鑰，**閘道統計會少一塊**（作者提醒）

相關：[[loop-engineering-6-components]]（LangSmith Sandboxes 與 durable execution，本庫既有的 LangSmith 提及）
