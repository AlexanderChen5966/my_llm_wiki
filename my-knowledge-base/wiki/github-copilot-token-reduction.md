---
title: GitHub Copilot 新計費下的 Token 削減四法
date: 2026-06-17
tags: [ai/claude, ai/prompt-engineering]
source: https://codelove.tw/@tony/post/qJW7ex
type: article
status: processed
asset_value: 62
---

GitHub Copilot 2026-06-01 起改為 AI Credits 按量計費（Sonnet 9x、Opus 27x 更貴），作者整理出四種實用 Token 削減技法：Caveman Prompt（壓縮輸出）/ Prompt Caching（壓縮輸入）/ Model Routing（任務分級）/ Sub-agent Isolation（委派隔離），並指出各技法的適用情境與注意事項。

## T（核心論點）

**Token 成本上漲讓「用量意識」成為必要技能**——不是換便宜模型就能解決，而是需要了解成本從哪裡來（輸入 token > 輸出 token），針對不同場景選對工具。四種方法各自解決不同的膨脹來源，**Prompt Caching 對長 session 效益最高**。

## B（適用場景）

- 日常重度使用 GitHub Copilot Pro/Pro+/Max 的工程師
- 需要控制 AI agent session 費用的開發者
- 理解 prompt caching 機制以設計更省錢的工作流

## R（完整內容）

### 新計費架構（2026-06-01 起）

| 方案 | 月費 | AI Credits | 備注 |
|------|------|-----------|------|
| Copilot Pro | $10 | 1,500 credits | |
| Copilot Pro+ | $39 | 7,000 credits | |
| Copilot Max | $100 | 20,000 credits | |

**模型費率倍數（相對基準）：**
- Claude Sonnet：9x 基礎費率
- Claude Opus：27x 基礎費率
- Code completion：**仍免費**（暫時）

### 四種 Token 削減法

#### 1. Caveman Prompt（壓縮輸出）

目標：壓縮 **輸出** token

方法：關鍵字 + 箭頭 + 符號取代完整句子，移除所有填充詞

範例：
```
# 一般 Prompt
請你分析這段程式碼的問題，並提供解決方案

# Caveman Prompt  
analyze→find bugs→fix→show diff only
```

**實際數字：**
- 官方宣稱：可壓縮 75%（理論值）
- 實際節省：14-21%（arXiv:2604.00025 研究數據）

**科學根據：** 研究顯示對大型模型而言，簡潔指令反而提升準確率（自發性的規模依賴冗長性）。

**注意事項：**
- ❌ 不適合單次查詢（幾百 token 的 overhead 不划算）
- ✅ 適合長 agent session + 搭配 prompt caching

#### 2. Prompt Caching（壓縮輸入）

目標：壓縮 **輸入** token（最高 90% 折扣）

原理：靜態內容（system prompt、專案規則、工具定義）快取後重複使用，只有動態內容（使用者訊息）計費。

**正確排列順序：**
```
[頂部] system prompt（靜態，最大化 cache 命中）
[頂部] 工具定義（靜態）
[頂部] 專案規則 CLAUDE.md（靜態）
---
[底部] 使用者訊息（動態，每次重新計費）
```

**VSCode 1.118+：** Copilot caching 已優化，無需手動設定。

**關鍵禁忌：**
- ❌ 中途切換模型（破壞 cache，全部重算）
- ❌ 動態內容放在靜態內容前面

#### 3. Model Routing（任務分級）

目標：按任務複雜度自動選用最便宜的合適模型

Copilot Auto Mode 機制：自動評估任務複雜度並選模型。
- **注意：** Auto Mode 預設排除 Opus（費率 27x 太貴）

**手動分級建議：**
| 任務類型 | 建議模型 |
|---------|---------|
| 簡單 autocomplete | 免費 completion |
| 一般 Q&A | 較便宜模型 |
| 複雜架構分析 | Sonnet |
| 極複雜推理（慎用）| Opus |

#### 4. Sub-agent Isolation（委派隔離）

目標：避免 **探索成本** 污染主 context

問題：讓主 agent 讀取大型 DB 結果、grep 輸出、file tree，會造成大量輸入 token 膨脹。

解法：把探索性工作委派給 sub-agent，只回傳摘要：

```
主 agent → sub-agent：「搜尋 src/ 中所有 API endpoint 定義」
sub-agent 讀取數千行 → 回傳：「找到 12 個 endpoint，清單如下...」
主 agent 只收到簡短摘要（少數 token）
```

### 適用情境速查

| 場景 | 建議方法 |
|------|---------|
| 讀取大型 DB/log/JSON | Headroom 壓縮（[[headroom-context-compression]]）|
| 長 agent session 中的大量說明 | Caveman Prompt（**中文環境見下方補充**）|
| 同一 system prompt 多次重複使用 | Prompt Caching |
| 大量 file 探索、grep、測試 | Sub-agent Isolation |
| AI 生成程式碼過度工程/過量 | Ponytail（見下方比較）|

## 補充：Caveman vs Ponytail——不是同一類工具，中文環境選 Ponytail

（2026-07-13 新增）常見混淆：兩者都掛著「省 token」名號，但作用軸線完全不同，**不是二選一的替代關係**。

| | Caveman Prompt | Ponytail |
|---|---|---|
| 省的是什麼 | **對話文字**的 token（電報式壓縮回覆：刪冠詞/修飾語，箭頭+關鍵字取代完整句子）| **生成程式碼**的量（六梯級決策樹：需要存在嗎→stdlib→原生能力→已裝依賴→一行→最少實作）|
| 語言相依性 | **高**——壓縮句法建立在英文 tokenizer 特性上：英文單字≈1 token，刪填充詞收益直接；中文 1 字約 1-2 token，電報式中文省得有限，且中文縮寫易產生語意歧義讓模型誤解、增加返工 | **低**——省的是 code，code 不分中英文；用中文對話下指令，Ponytail 的節省效果不變 |
| 實測數據 | 平均 65% 輸出 token 減少（**全部以英文任務測得**，範圍 22-87%）| 平均 54%（修正後數字，Claude Code + Haiku 4.5 測得）|
| 已知限制 | 只影響 output token，thinking token 不受影響；複雜推理任務過度壓縮會導致邏輯跳步 | 效果因模型而異，推理型模型可能不減反增；安全邊界類程式碼不適用 |

**關於 Caveman 的中文支援**：社群「Caveman 對中文支援不理想」的說法方向正確。Caveman 官方雖有 `wenyan`（文言文）模式試圖利用古典漢語濃縮特性，但：① 官方 benchmark 數據（65%）全部來自英文任務，中文節省率無實測佐證 ② 文言文/電報式中文在現代開發語境有理解風險與歧義問題，返工成本可能抵銷節省 ③ CJK 的 token 計價結構（1 字約 1-2 token）讓「刪字」的邊際收益天生低於英文。

**選用結論**：中文工作環境、輸出以程式碼為主時，**Ponytail 是更可靠的選擇**——不是因為它「更好」，而是它的節省機制與對話語言無關。兩者理論上可疊加（Ponytail 管 code、Caveman 管對話文字），但中文使用者疊加 Caveman 的增量效益有限。

**中文環境省 token 優先順序**：Ponytail（改寫版規則進 CLAUDE.md）＞ Prompt Caching（語言無關，長 session 效益最高）＞ codebase-memory-mcp（中大型專案的輸入端節省）＞ Sub-agent Isolation ＞ Caveman（可跳過）＞ pxpipe（CJK 保守支援，暫不採用）。開發流程整合細節見 research/dev-workflow-optimization-tools。

## C（關鍵行動）

- [ ] 在長 agent session 前啟動 Caveman Prompt 模式（**英文工作流**；中文工作流改用 Ponytail）
- [ ] 確認 CLAUDE.md / system prompt 放在 context 頂部以命中 cache
- [ ] 設定 Copilot Auto Mode 並確認 Opus 已排除
- [ ] 將 file 探索類任務改為 sub-agent 委派
- [ ] 中文環境的省 token 決策先分清楚要省的是「對話文字」還是「生成程式碼」，再選對應工具

## 相關連結

- [[headroom-context-compression]] — Headroom：context 壓縮層（Sub-agent Isolation 的工具搭配）
- [[llm-cost-optimization-strategies]] — LLM 成本優化四層策略（更完整的成本框架）
- [[deepclaude-cost-optimization]] — deepclaude：換便宜後端（五件套第一件）
- [[opensquilla-intelligent-model-router]] — OpenSquilla：自動 Model Routing（五件套第五件）
- [[claude-code-billing-usage]] — Claude Code 計費說明（Copilot 外的另一選項）
- [[ponytail-lazy-senior-dev-skill]] — Ponytail 六梯級決策樹完整說明（與 Caveman 的軸線比較見上方補充）
- [[codebase-memory-mcp]] — 輸入端省 token：知識圖譜結構查詢取代 grep+整檔讀取
- [[pxpipe-image-token-compression]] — pxpipe 完整失效模式（CJK 保守支援警告出處）
- [[github-copilot-context-design-strategy]] — 上下文架構策略深度篇（CDQR 框架）
