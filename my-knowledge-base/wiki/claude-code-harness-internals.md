---
title: Claude Code 運作機制：Harness 三層架構、外層迴圈與上下文注入
date: 2026-08-03
updated: 2026-08-03
tags: [ai/claude, dev/architecture, ai/agent]
source: https://codelove.tw/@tony/post/qOZGva（譯自日文 Qiita）
type: article
status: processed
asset_value: 92
---

## T（核心論點）

Claude Code 的運作可用**三層結構**理解：**Claude API 只負責推論且完全無狀態**、**Harness（本機的 Claude Code 主體）承擔全部狀態管理**、**JSONL 工作階段記錄是持久化層**。官方用語就叫 **agentic harness**：
> *Claude Code serves as the **agentic harness** around Claude: it provides the tools, context management, and execution environment that turn a language model into a capable coding agent.*

理解這層分工能解釋幾乎所有「為什麼會這樣」的疑問——為什麼對話越長越貴、為什麼改一個字快取就失效、為什麼 `/clear` 不是刪除、為什麼隔天開 session 仍會反映最新的 CLAUDE.md。

## B（適用場景）

**適用：** 想理解 token 成本從哪來、要優化快取命中率、要判斷 compaction 會遺失什麼、除錯「Claude 為什麼沒讀到某個設定」

**不適用 / 注意：**
- 部分內容標註為**作者實測**而非官方文件（如 TTL=1h），會隨版本變動
- thinking 參數形態有**世代分界**（見下方），引用時要標明模型世代

## R（五個機制）

### 一、三層架構與「無狀態」的真正含義

| 層 | 實體 | 角色 |
|---|---|---|
| **Claude API** | Anthropic 伺服器 | 只負責推論（思考、生成、判斷是否用工具）。**不保有任何狀態** |
| **Harness** | 本機的 Claude Code 主體 | 工具執行、上下文組裝、權限管理、工作階段持久化——**承擔所有狀態管理** |
| **工作階段記錄** | JSONL 檔案 | 對話持久化層，組裝請求時從這裡讀出歷史 |

**Messages API 無狀態的三個推論**：
- 伺服器完全不保留對話歷史
- 每次呼叫，harness 把系統提示 + 過去所有訊息 + 這次輸入**全部一起送出**
- **所謂「上下文」就是每次送出的整個 payload**——payload 依 `tools` → `system` → `messages` 順序串接

> 🔑 **系統提示與 CLAUDE.md 不會存進 JSONL**——每次呼叫時由 harness 從磁碟重新讀取生成。**這就是為什麼隔天重開工作階段，最新的 CLAUDE.md 仍會反映進來。**

### 二、提示快取：無狀態的代價消除機制

伺服器保存上次處理 payload 的結果；若這次 payload **開頭部分與上次相同**，就沿用已保存結果，只處理新增的尾端。

🔴 **但「相同」的判定是從開頭開始的完全一致——payload 中只要有 1 個位元組不同，從那裡之後的快取全部失效。**

- 快取由 **harness 在 payload 中指示**（不是伺服器自行決定）：在要快取的範圍末端加 `cache_control` 標記
- **每個請求最多 4 個標記**，每次都重新加在延伸後的對話尾端（過去位置的快取仍有效）
- TTL 有 5 分鐘與 1 小時兩種，**Claude Code 指定 1 小時（實測）**
- **從快取讀出的成本約為一般輸入的 0.1 倍**

→ 這直接解釋了為什麼「**頻繁變動的資訊不該寫進 CLAUDE.md**」（見 [[claude-md-optimization-techniques]]）——它在 payload 開頭，一改就讓後面全部重算。

### 三、Agentic Loop 的雙重結構

自律性由**兩個性質不同的層次**構成，**迴圈只有外層這一個**：

**外層（harness 端）**——使用者每發言 1 次，背後會跑多次 API 呼叫（複雜任務甚至數十次）：
1. **執行迴圈的是 harness**——模型只透過 `tool_use` 區塊「要求」下一步，真正執行工具的是 harness
2. **工具結果以 `user` 角色訊息回傳**——harness 建立含 `tool_result` 的 user 訊息追加到尾端
3. **是否繼續由模型決定**——`stop_reason` 為 `tool_use` 就繼續，`end_turn` 則回合結束

**內層（伺服器端）**——單次回應內部的 thinking 推論。
⚠️ **世代分界**：4.5 以前用 `budget_tokens`；**4.6 以後改 `{"type":"adaptive"}` + `output_config.effort`**。引用時務必標世代（見 [[claude-opus-5-prompting-guide]] 的 effort 說明）。

### 四、上下文注入的四個管道

harness 不只是把 JSONL 轉成 messages，還會**到處注入控制模型思考的資訊**：

| 管道 | 作用 |
|---|---|
| **`system` 參數** | 最上層系統提示（角色設定、工具原則、回應格式）|
| **`<system-reminder>` 標籤** | 插入 user 訊息的**萬用注入標籤**——通知模式狀態、技能清單、hook 產生的上下文 |
| **合成歷史** | 「**假裝模型曾呼叫工具**」的虛構歷史 |
| **工具結果包裝** | 對 `tool_result` 格式化與暫存 |

**實測案例（很有意思的三個）**：
- **防重複讀檔**：模型想再讀已載入的檔案時，harness **不會真的讀**，而是注入 `<system-reminder>This file is already in your context ... Use that content instead of re-reading.` 來修正行為
- **`@README.md` 提及**：payload 被**合成出一組看起來像模型呼叫了 Read 的 `tool_use`/`tool_result` 配對**
- **輸出過大**：WebFetch 輸出過大時，`tool_result` 改成指向暫存檔的參照 + 前 2KB 預覽（`<persisted-output>`）

### 五、🔑 pull 型設計（不是全部 push）

harness **不把「模型可能需要的資訊」全部注入**：

```
❌ push 型：<system-reminder>目前任務：1. 修正建置（進行中）2. 新增測試 3....（全文）
✅ pull 型（實際）：<system-reminder>任務清單已更新。可以使用 TaskList 工具查看。
```
TodoList 狀態、Plan 檔案本體都**不整份放進 messages**，只注入小通知，**由模型判定需要時才透過 tool_use 取全文**。

## ⚠️ compaction 會保留什麼、遺失什麼（官方整理）

| 要素 | compaction 後 |
|---|---|
| 系統提示 | ✅ 不受影響（本來就不在 messages）|
| 專案根目錄 `CLAUDE.md` / Auto memory | ✅ **從磁碟重新注入** |
| 🔴 **帶 `paths:` 的 rules / 子目錄 `CLAUDE.md`** | ❌ **會先遺失**（直到再次讀取該檔案）|
| 已啟動技能的正文 | ✅ 重新注入（每技能上限 5,000 token、總計 25,000）|

> 🔴 **這是判斷「長對話能養多久」的關鍵依據**——若你依賴 path-scoped rules 或子目錄 CLAUDE.md，長對話反而更危險。詳見 [[claude-code-context-management]] 的世代反轉注記。
>
> ⚠️ **反向提醒（2026-08-03 補）**：官方與媒體都把 `.claude/rules/` 的 `paths:` 當成「省 context 的推薦技巧」介紹，**卻不提這一行**。因此 [[claude-md-optimization-techniques]] 收錄該技巧時已加上守則：`paths:` 只放「碰到這類檔案才需要」的局部慣例，**必須全程生效的硬性約束不能放進去**（compact 後會安靜消失且無通知），該用 Hook／settings 強制。

`compact` 的實際機制：`parentUuid: null` **實體切斷鏈結**（重建 payload 不再追溯之前），但 `logicalParentUuid` 保留邏輯連接供顯示與 `/rewind` 使用。

## C（關鍵行動）

- [ ] 理解「上下文 = 每次送出的整個 payload」——這是所有 token 成本判斷的基礎
- [ ] **把穩定內容放 payload 前段**（system/CLAUDE.md），頻繁變動的別放——一個位元組不同就讓後面快取全失效
- [ ] 用 `/context` 查自己工作階段各類別的 token 消耗（**只有 `messages` 陣列會單調增加**，是膨脹主因）
- [ ] 依賴 `paths:` rules 或子目錄 CLAUDE.md 時，**別養太長的對話**（compact 會先丟這些）
- [ ] 引用 thinking 參數時標明世代（4.5 `budget_tokens` vs 4.6+ `adaptive` + `effort`）

## 補充資料：2.1.260 的快取修正只修了一半（2026-09-04 實測）

來源：〈Claude Code 的快取，只會套用在使用工具的回合中〉，小編精選技術文章翻譯（tony），codelove.tw，
2026-09-05，<https://codelove.tw/@tony/post/qEWjX3>（譯自 Qiita，作者 suwa_nobu）

R 層第二點講的是快取**機制**（開頭一致才沿用、一個位元組不同就全失效）。本節補的是同一機制在
**實作版本層**的一個具體缺口——原來「payload 開頭一致」並不保證命中，還要看 context 掛在哪個位置。

### 修正的條件寫在發行說明裡，而且很窄

Claude Code **2.1.260**（2026-09-03）進了 3 個 Fable 5.1 快取修正，發行說明原文：

> Fixed prompt caching on Claude Fable 5.1 **not covering the context attached after tool results**,
> so it was re-sent as uncached input on every tool-call turn

🔑 條件是 **「工具結果之後（after tool results）附加的 context」**——只有這一段被修好。

### 實測結果（Windows 10 / Claude Code 2.1.260 / `--resume` 延續同一 session）

| 第 2 回合的情況 | 命中率 |
|---|---|
| **有呼叫工具**（先 Read 一個 `.md` 再繼續） | **6 次全部 99.7%，無波動** |
| **不呼叫工具**（`hi` → `hi again`） | **9 次中 8 次仍停在 38.6%** |

→ 不使用工具那一側**原樣保留，沒有修好**。

### 成本換算

未命中的回合要重寫約 **50,959 token**。以 Fable 5.1 的 1 小時快取寫入 $20/M 計：

```
沒命中  50,959 tok × $20/M = $1.02
命中        496 tok × $20/M = $0.010
```

**單一回合就差將近 1 美元**（訂閱制不另計費，此為 API 定價換算）。

### 受影響最大的用法

`claude -p` 只丟問題的 batch、不動檔案的純文字討論、摘要與翻譯——這類回合第 2 輪就要重寫約 5 萬 token。
⚠️ 作者認為「實際寫程式通常會讀檔，所以影響應不大」，但**自陳這部分是推測、沒有實測**。

### 🔑 方法論教訓：發行說明的條件必須驅動重測

作者 9/2 測的是 `claude -p "hi"`，**一次都沒呼叫工具**，因此根本不在這次修正的範圍內。
若漏看「after tool results」這個限制，會落入兩種誤判之一：明明沒修好卻寫成修好了，
或明明修好了卻寫成沒修好（作者自陳差點做成後者）。

> **「官方發了修正」和「我看到的現象已經修好了」是兩回事。**
> 發行說明若寫了條件，**必須依那個條件重新設計測試**——只有在改變條件後重測，才算完成驗證。

這條與本條目 B 層的注意事項同源：本條目多處內容標註為**作者實測**而非官方文件，且會隨版本變動；
引用任何快取數據時都該連版本號一起記。成本面的訂閱制 vs API 差異見 [[claude-code-billing-usage]]。

## 相關連結

- [[claude-code-api-request-reverse-engineering]] — **同主題的逆向工程視角**：用 proxy 攔下真實 payload 驗證本篇的規格描述（快取、thinking、stateless 三處互相佐證）
- [[claude-code-context-management]] — **本篇是其「世代反轉注記」與 `/clear` 更正的機制依據**
- [[claude-md-optimization-techniques]] — 「CLAUDE.md 每次請求都載入」「頻繁變動資訊不該寫進去」的機制解釋來自本篇快取原理
- [[harness-engineering]] — Harness Engineering 理論框架（Guides × Sensors）；**本篇是 Claude Code 的具體實作**
- [[awesome-agent-architecture-from-scratch]] — 從零拆解 Agent Harness 的 22 章教材
- [[claude-opus-5-prompting-guide]] — effort／thinking 的官方調校建議（對應本篇的世代分界）
- [[agent-loop-explained]] — Agent Loop 概念；本篇補上「迴圈只有外層一個」的精確界定
- [[mcp-stateless-architecture]] — MCP 也走 stateless，與本篇 Messages API 無狀態是同一設計哲學
- [[synthesis-claude-code-internals]] — **本篇為其核心來源**（六篇整合精粹）；該精粹釐清了本篇「三層架構」與五層架構／四機制分工**不是競爭而是三個切面**（runtime / build-time / design-time）
