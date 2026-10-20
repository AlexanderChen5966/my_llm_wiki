---
title: Claude Code 12 條規則：錯誤率從 41% 砍到 3%
date: 2026-06-01
tags: [ai/claude, ai/agent]
source: https://www.blocktempo.com/claude-code-12-rules-error-rate-3-percent-karpathy-agent/
type: note
status: processed
asset_value: 80
---

基於 Karpathy 對 Claude 寫程式問題的批評，Forrest Chang 整理出 4 條 CLAUDE.md 行為規則，後由另一位開發者在 6 週、30 個程式庫的測試後擴充為 12 條，涵蓋多步驟 Agent、hook 鏈式觸發等 2026 年 5 月才出現的新場景。錯誤率從 41% 降至 3%。

## T（核心論點）

CLAUDE.md 是 AI 程式設計技術棧裡最被低估的檔案。它不是願望清單，而是**封堵已知失敗模式的行為契約**。

**常見三類錯誤：**
- 把它當偏好垃圾桶（4000+ token，遵守率掉至 30%）
- 完全不用（5 倍 token 浪費，跨 session 不一致）
- 複製模板就不再更新（2 週後慢慢失效）

**Karpathy 原版 4 條規則（Forrest Chang 歸納）：**
1. 編碼前先思考——說明假設，暴露權衡，先提問
2. 簡單優先——最少程式碼，不加想象功能，不過度抽象
3. 外科手術式修改——只改必要部分，不順手最佳化、重構
4. 以目標為導向執行——定義成功標準，讓 Claude 自己迭代

**新增 8 條規則（對應 2026 年 5 月新場景）：**
5. AI vs 確定性程式碼分工——路由、重試、狀態碼由普通程式碼處理，不交給 Claude 判斷
6. Token 預算規則——單任務 4,000 tokens，單 session 30,000 tokens，超限就總結重開
7. 模式衝突處理——兩套矛盾模式選一個（選更新的），標記另一個待清理，不做平均折中
8. 先理解再新增——讀取檔案匯出內容與呼叫方再動工，「在我看來不相關」是最危險的一句話
9. 測試要驗真邏輯——測試必須在業務邏輯變化時失敗，不只驗「它做了什麼」
10. 多步驟檢查點——每步完成後總結已完成/已驗證/剩餘，不從不清楚狀態繼續
11. 風格一致優先於偏好——在現有程式庫內，一致性勝過個人偏好
12. 靜默失敗現形——不能確定就明確說出來，沒有全驗證就不能說「完成」

**關鍵實測發現：**
- 從 4 條擴充到 12 條，遵守率只從 78% 降至 76%（幾乎無負擔）
- 超過 14 條後遵守率從 76% 掉至 52% ⚠️ **此組數字僅社群實測、無法外部複現，且對應的是「規則條數」不是行數**；原文附註「200 行上限是真實的」**已不成立**（🔴 2026-09-03 更正：**理由是 context 成本，不是遵守率**——1,650 session 實測顯示 25→500 行對遵守率無可測量影響；「只讀前 200 行」的硬截斷屬 `MEMORY.md`。詳見 [[claude-md-guide]]）
- 整體不超過 200 行；不要放範例（佔 context 太多）；不要說「小心一點」（遵守率 30%）

## B（適用場景）

**適用：**
- 使用 Claude Code 做多步驟 Agent 任務、跨多個 commit 的工作流
- 有 monorepo 或多服務架構的開發者
- 想要跨 session 保持一致性的獨立開發者

**不適用 / 注意：**
- 不跑多步驟 pipeline → 規則 10 可省略
- 程式碼庫只有一種統一 lint 強制風格 → 規則 11 是冗餘的
- 保留對應你「實際犯過的錯誤」的規則，其餘刪掉（6 條精準 > 12 條通用）

## R（操作步驟）

1. 用 curl 拉取 Karpathy 4 條規則追加到你的 CLAUDE.md：
   `curl https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md >> CLAUDE.md`
2. 在下方手動貼上本文規則 5-12
3. 加入專案專屬規則（技術棧、測試指令、錯誤模式）
4. 整體保持在 200 行以內

## C（關鍵行動）

- [ ] 檢查目前 CLAUDE.md 的行數，超過 200 行就精簡
- [ ] 對照 12 條規則，找出自己「真正犯過的錯誤」對應的條目，保留它們
- [ ] 每隔 2-4 週重新審視 CLAUDE.md 是否仍適用於當前程式庫狀態

## 原始摘要

基於 Karpathy 對 Claude 寫程式問題的批評，開發者 Forrest Chang 整理出 4 條 CLAUDE.md 行為規則，後擴充為 12 條以涵蓋多步驟 Agent、hook 鏈式觸發等進階場景。實測 30 個程式庫後，錯誤率從 41% 降至 3%。

## 🔴 世代注記：規則 6 的「單 session 30,000 tokens 就重開」已不是鐵律（2026-09-03）

本篇規則 6 寫「單任務 4,000 tokens，**單 session 30,000 tokens，超限就總結重開**」。這條在 compaction 成熟後已被兩個獨立來源推翻（詳見 [[claude-code-context-management]] 的世代反轉注記）：

- **Anthropic 工程師投影片**已**主動移除**「限制單次 session 長度」這條建議
- OpenAI 開發者 Jason Liu 有運作五週、累積約 400 個子代理的對話串——做法是把 thread「當同事養」

⚠️ **但這是取捨不是單純推翻**，代價原文明列：壓縮品質不見得每次都穩定、細節可能遺失、快取失效時 token 成本也會墊高。

> 🔑 **修正後的判準：不是「超過某個數字就重開」，而是「依賴什麼而定」。**
> 純對話延續可以養長 thread；**依賴 `paths:` 路徑限定規則或子目錄 CLAUDE.md 的工作，compact 時這些會先遺失**，仍該及早存檔重開。

本篇的 4,000／30,000 兩個數字請視為**該篇來源當時的社群實測參考值**，不是現行建議。

## 相關連結

- [[claude-code-context-management]] — 🔴 **以該篇為準**：「長對話要重開」的世代反轉、compact 會遺失什麼（`paths:` rules 先遺失）
- [[claude-code-harness-internals]] — compaction 的實際保留／遺失清單

- [[claude-md-guide]] — CLAUDE.md 撰寫技巧（Boris Cherny 六大核心引擎 + Karpathy 12 條）
- [[claude-code-large-codebase]] — 大型專案配置決定成效
- [[agentic-engineering-sop]] — Agentic Engineering 六階段實戰 SOP
- [[agents-md-guide]] — AGENTS.md 完整指南
