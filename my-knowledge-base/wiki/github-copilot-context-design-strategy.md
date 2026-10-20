---
title: 別吝嗇 Token 要好好設計：GitHub Copilot 上下文架構策略與 CDQR 框架
date: 2026-07-09
tags: [ai/prompt-engineering, ai/workflow, type/how-to]
source: https://codelove.tw/@tony/post/3V2n43（原文：https://qiita.com/ochtum/items/d442ed23d24245b789a0）
type: article
status: processed
asset_value: 76
---

深度上下文設計方法論：目標不是減少 token，而是把能幫助 AI 做出好判斷的資訊以適當時機與粒度交給它。涵蓋 AGENTS.md/Skills/DESIGN.md 依「資訊壽命」分工、MCP vs CLI 決策框架、上下文設計/Prompt Caching/上下文壓縮三者的區分、模型選擇三層次思考，最後統整成 CDQR（Cost/Delivery/Quality/Review）四軸判斷框架。

## T（核心論點）

**目標不是減少 token，是把能幫助 AI 做出好判斷的資訊，以適當時機與粒度交給它。** 單純刪減 token 可能連必要的設計資訊與審核依據一起弄丟；為了保險把所有資料都丟給 AI，又會增加成本與判斷雜訊。上下文消耗不只來自提問文字，還包含固定指示、工具定義、對話歷史、工具執行結果——很多時候把搜尋結果從 500 筆砍到 20 筆，比把提問文字縮短 20 個字效果大得多。

## B（適用場景）

GitHub Copilot Chat/Agent mode 重度使用者；已導入或評估導入 MCP 與 Agent Skills 的團隊；煩惱 AGENTS.md 與設計文件該放哪裡；需要同時管理 AI 開發速度、品質與成本的技術負責人。原則同樣適用於 Claude Code 等其他 agentic coding 工具。

## R（重點整理）

**先觀測再最佳化**：用 `/context`（上下文使用狀況）、`/usage`（session 用量）、`/compact`（壓縮對話歷史）三指令定位膨脹來源，而非直接猜測性地砍指示檔。比較時保持模型/設定/儲存庫/提問一致，一次只改一個條件（MCP 開關/搜尋結果筆數/檔案全文 vs 相關範圍/長對話 vs 新 session/壓縮前後/模型選擇）。

**依資訊壽命分工四層**：

| 資訊 | 角色 | 何時讀取 |
|------|------|---------|
| `AGENTS.md` | 常駐規則 + 參考入口（路由器，非全知識倉庫） | 任務入口，每次都讀 |
| `SKILL.md` | 特定作業流程 | 對應作業觸發時才讀 |
| `DESIGN.md` | 設計系統（YAML token + Markdown 理由兩層） | 需要 UI/外觀判斷時，需從 AGENTS.md 或提示詞明確引用 |
| `*.instructions.md` | 路徑專屬規則 | 處理特定檔案/目錄時 |

判斷放哪裡的五問法：①幾乎所有任務都需要嗎→AGENTS.md ②只有特定類型工作需要嗎→Skill ③是詳細背景/設計理由嗎→DESIGN.md 或 ADR ④只適用特定路徑嗎→路徑專屬 instructions ⑤只針對這次需求嗎→使用者提示詞。**不要看檔名決定，要依資訊壽命與適用範圍決定**。

**MCP vs CLI 決策原則**：「如果要讓 AI 選擇要用哪個工具，就用 MCP；若 Skill 已經決定『就用這個命令』，就用 CLI。」MCP 容易變重的三處：大量龐大 tool schema（工具數十百個時累加）、很長的 tool result（把列表與詳細拆開，如 `search_issues` 只回必要欄位、`get_issue` 才取詳情）、很長的 resource 本文（大文件用摘要/關鍵字搜尋/拆分章節而非整份載入）。MCP 伺服器端可用 `limit`/`cursor`/`fields`/`top_k`/`include_body`/`max_chars`/`summary` 等參數從介面本身限制輸出量。**CLI 不代表自動省 token**——`git log`、`dotnet test --verbosity diagnostic` 等指令一樣可能輸出爆量，需同樣做輸出縮減（`git diff --name-only`、`--json` 限欄位等）。

**MCP 通訊 ≠ LLM token 消耗**：`tools/list` 等 RPC 通訊本身不直接等於模型輸入，取決於 client/agent runtime 是否把結果傳給模型；「一連接 MCP 所有定義都會送進模型」是不精確的說法，但 AI 要選工具仍需某種程度知道有哪些可用工具，工具數量與 schema 大小依然是設計重點。

**上下文設計 vs Prompt Caching vs 上下文壓縮是三件不同的事**：①上下文設計＝一開始就不放入不需要的資訊 ②Prompt Caching＝重用多次請求間相同的前綴計算（順序改變/工具 schema 改變會破壞共通 prefix；MCP 伺服器端快取與 Prompt Caching 是不同機制）③上下文壓縮＝把變長對話歷史摘要化（保留決策，可能遺失細節，重要原文應另外存檔）。**「計費上能便宜重用」與「值得讓 AI 讀」是兩回事**——被快取的無關資訊仍會造成 AI 判斷雜訊，不能因為能快取就放棄設計。實作優先順序：先不放不必要資訊→快取需要但反覆出現的固定資訊→壓縮變長的對話歷史。

**模型選擇三層次思考**（不寫死具體模型名，因為名稱會變）：高推理模型（設計/規劃/困難除錯/審查/安全性判斷）／標準模型（一般實作/補測試/跨檔案修改，猶豫時的中心選項）／輕量模型（小型重構/格式轉換/摘要，不把複雜設計判斷丟給它）。**先檢查上下文設計，再考慮換模型**——失敗原因有時不是模型能力而是輸入雜亂，切到高效能模型可能只是「模型夠強、勉強撐住雜亂輸入」，不利於可重現。建議順序：`/context`/`/usage` 定位膨脹→減少無關檔案/log/工具結果→補必要設計文件與限制→拆成調查/規劃/實作/審查階段→真正困難判斷才交給高推理模型。Chat 模型與 Inline suggestion 模型是分開設定，可以「Chat 用高推理、補全用輕快模型」。

**CDQR 判斷框架**（把 Token 最佳化放進更大的決策脈絡）：

| 軸 | 核心問題 |
|----|---------|
| **Cost** | 是否有不必要的輸入/輸出/重試（含是否用高推理模型處理輕量工作）|
| **Delivery** | 交給 AI 後整體作業是否真的更快（有時固定成 Skill/CLI 比每次讓 AI 想流程更快）|
| **Quality** | 是否在必要範圍內讓 AI 讀到設計意圖、限制與驗證條件 |
| **Review** | 是否保留人類應批准的邊界（公開 API 破壞性變更/資料刪除/上線部署/認證變更/機密處理/架構變更）|

Token 最佳化不只改善 Cost，透過良好的上下文設計也能同時改善 Delivery、Quality、Review 三者。

## C（關鍵行動）

- [ ] 減 token 前先用 `/context`/`/usage` 觀測，定位是哪裡膨脹而非直接猜測性刪減
- [ ] AGENTS.md 只放常駐規則+參考入口，不要把 DESIGN.md/架構文件全文複製進去
- [ ] MCP 工具設計優先做「列表回精簡欄位、詳情另開工具」的拆分
- [ ] 模型選擇規則寫進 AGENTS.md/Skill，用「任務難度層級」而非寫死模型名稱
- [ ] 導入新任務自動化前，用 CDQR 四軸逐一檢查，而非只看省了多少 token

## 相關連結

- [[github-copilot-token-reduction]] — GitHub Copilot 計費與四種 Token 削減技法（Caveman Prompt/Prompt Caching/Model Routing/Sub-agent Isolation），本文是更深層的架構設計補充
- [[claude-code-context-management]] — Claude Code 版本的 Context 管控全攻略
- [[claude-code-auto-memory]] — Auto Memory 三分流判斷法（要不要傳給別人？）
- [[design-md-google-format]] — DESIGN.md 官方規格詳解（YAML token + Markdown 理由兩層）
- [[api-cli-skills-agent-architecture]] — CLI vs MCP 的另一個工程比較視角
- [[agent-skills-vs-mcp]] — Skills vs MCP 選擇判斷（互補：本文更聚焦上下文成本面）
