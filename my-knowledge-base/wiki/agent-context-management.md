---
title: Context Management 四層機制：Selection / Compaction / Eviction / Rehydration
date: 2026-08-12
tags: [ai/agent, ai/prompt-engineering]
source: https://ithelp.ithome.com.tw/articles/10401895（《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》Day 09）
type: article
status: processed
asset_value: 64
---

同系列 Day 08（Skills 與 Progressive Disclosure）已併入 [[agent-skills-vs-mcp]]，本篇把該篇「動態載入」的思想從 Skill 擴展到**整個 Working Context**。下一篇 Day 10 的跨 Session 版本見 [[agent-memory-lifecycle]]。

## T（核心論點）

Context Management 真正要解決的不是「Token 超過限制時怎麼辦」，而是：

> **哪些資訊值得繼續佔用模型的注意力？**

因此 **Context Window 大 ≠ 可以全部留下**——容量問題會被換成注意力問題。核心分工是：

> **Storage 負責不遺失資料，Context 負責讓模型看見現在最重要的資料。**
> Bigger Context Window ≠ Better Memory。

## B（適用場景）

**適用：**
- 設計自建 Agent 的 Context Manager（決定每輪要送什麼進 Prompt）
- 理解 Claude Code `/compact`、auto-compaction 在做什麼、什麼時候會出錯
- 長時間、多輪、多工具的任務（Agent 執行數十輪後 Context 一定會爆）

**不適用 / 注意：**
- 單輪問答、短對話用不到這套機制
- 🔴 **最危險的不是 Overflow，是壓錯東西**——錯誤 Summary 會讓後面的模型「自信地建立在錯誤狀態上」，比爆 Context 更糟
- 只做截斷（刪最舊訊息）會刪掉「第一輪就講好、但一直有效」的 Constraint

## R（操作步驟）

### 第一層：Selection——哪些資訊該進下一輪？

先把資訊分類，不同類別的保留優先序不同：

| 類別 | 內容 | 保留傾向 |
|---|---|---|
| **Goal** | 使用者真正要完成什麼 | 高優先保留 |
| **Constraints** | 不可改某資料夾／不可寄信／輸出格式／Budget | **通常比中間對話更重要** |
| **Current State** | Plan、已完成 Step、失敗 Step、Changed Files、Pending Approval | 保留 |
| **Recent Observation** | 最新工具結果（最近一次失敗通常很重要） | 保留 |
| **Evidence** | 仍支持目前判斷的證據 | 視情況 |
| **History** | 過去發生什麼 | 多半可摘要或移出 |

> ⚠️ **Selection ≠ Memory Retrieval**：Selection 處理「這個任務已產生的資訊哪些要留」；Retrieval 處理「過去 Session 的資訊哪些要載回」。兩者最後都放進 Context，責任不同（見 [[agent-memory-lifecycle]]）。

### 第二層：Compaction——仍重要但不需完整保留

`Full Artifact → Task-Relevant Summary → Working Context`

**好的 Summary 不是越短越好**，判準是一句話：

> 如果原始內容現在被移出 Context，未來的 Agent 還需要知道什麼才能繼續工作？

必須保留：結論、關鍵證據、尚未解決問題、重要 Constraint、下一步需要的資訊。

**Protected Fields（永遠不能在 Summary 中遺失）**：Goal／Constraints／Approval State／Security Boundaries／Unresolved Errors／Verification Result。
**Free-form History（可自由壓縮）**：對話細節、搜尋過程、成功 Tool Calls、被淘汰的假設。

🔴 反例：原文「可以修改設定，但**絕對不要修改 Production Secret**」被摘成「使用者允許修改設定」——關鍵 Constraint 消失。

**Compaction 本身也要 Verification**：檢查是否保留所有 Constraint／未完成 Task／重要 Error，是否錯誤標記完成、是否遺失 Resource Scope、是否產生原文不存在的結論。

### 第三層：Eviction——移出 Context（不是刪除資料）

可 Evict：已成功完成的 Tool Call 細節、被推翻的假設、不再使用的 Skill、重複搜尋結果、過時 Plan、已整合的 Child Transcript、大量成功 Log。
原始資料仍留在 Trace Store／Artifact Store／DB／File／External Log。

**Eviction Policy 的六個訊號**：Recency（最近是否使用）／Relevance（與目前 Plan 相關嗎）／Uniqueness（只是重複嗎）／Authority（是不是使用者 Constraint、System Policy、Verified Evidence）／Cost（多大）／Recoverability（移除後好不好重新取得）。

> 例：「不可自動部署 Production」即使很舊也要留；20K token 的 Test Log 可以隨時重跑，就適合 Evict。

### Context Tier：比 FIFO 穩定的分層

| Tier | 名稱 | 內容 |
|---|---|---|
| 1 | **Pinned** | 使用者目標、核心 Constraint、System Policy、重要 Permission、Current Plan |
| 2 | **Active** | 最近 Tool Result、當前 Step、相關 Skill、最近 Error、Active Artifact Summary |
| 3 | **Compressed** | 已閱讀檔案、已完成 Investigation、舊 Subagent Result、過去嘗試 |
| 4 | **External** | 完整 Log、大型文件、Binary Artifact、完整 Conversation、舊 Trace |

### 第四層：Rehydration——需要時再載回來

關鍵是 **Evict 之前要留下足夠索引**：Artifact ID／File Path／Trace ID／Search Key／Summary／Source Reference。

**Stub 技巧**（比整段刪掉好，模型知道它存在但不用每輪付全額）：

```markdown
[Artifact: test-log-042]
內容：完整 integration test log
大小：18,420 tokens
摘要：3 個失敗，主要集中於 payment timeout
需要時可重新讀取
```

### Soft Budget：不要在 Overflow 邊緣工作

模型上限 200K，不代表要等 195K 才動作——還要預留下一次輸出、新 Tool Result、新 Skill、Verification、Replan、Subagent Result。

```
Model Limit 200K / Target Working Context 120K / Reserve 80K
```

超過 120K 就依序：移除重複 → 移除已解決的成功紀錄 → 壓縮舊 Observation → 移出大型 Artifact → 保留 Pinned。

> 📌 這個「不要等到滿才處理」的原則，與 [[context-engineering-stacking]] 的 **40–60% 用量門檻**是同一件事的兩種表述：前者從系統設計端設 Soft Budget，後者從使用者端給觀察區間。

### 不同 Observation 的生命週期不同

| 類型 | 保留策略 |
|---|---|
| File Read | 摘要 + File Reference |
| Search Result | 最相關證據 + Source |
| Test Log | **失敗**保留關鍵 Error；**成功**只留 `Tests passed.` |
| Database Query | 聚合結論 + Query Reference |
| Build Output | 成功高度壓縮；失敗保留錯誤附近內容 |

**Context Priority**：`Unresolved Failure > Current Evidence > Current Plan > Recent Success > Resolved History`——**失敗通常比成功更值得保留**。

### Context Drift：Summary of Summary 的累積失真

`Raw → Summary A → Summary B → Summary C`，每次都掉一點，最後與原始事實越離越遠。
降低風險：關鍵事實獨立保存／Constraint 不進自由摘要／Summary 保留 Source Reference／定期從原始資料重新 Consolidate／不要無限摘要。

## C（關鍵行動）

- [ ] 🔴 不要讓模型自由決定所有 Eviction——System Policy、User Constraint、Permission Boundary、Pending Approval、Budget、Verified Failure、Required Output Schema 應由**程式**標為 Pinned
- [ ] 責任三分：**程式決定什麼不能丟／模型協助判斷什麼最相關／Context Manager 執行三個動作**
- [ ] 設 Soft Budget（例如上限的 60%）而不是等 Overflow
- [ ] Evict 前一律留 Stub 或索引，否則無法 Rehydrate
- [ ] Compaction 產出的 Summary 要跑一次「Constraint 有沒有掉」的檢查
- [ ] 不要對所有 Tool Result 用同一套保留策略——照資料類型分流

### 八個常見錯誤設計

1. Context Window 大所以全部保留　2. 只刪最舊訊息　3. Summary 只追求短　4. 只 Load 不 Evict（→ Context Hoarding，見 [[agent-skills-vs-mcp]]）　5. Evict 後沒有 Reference　6. 所有 Tool Result 同一策略　7. 無限 Summary of Summary　8. 所有 Eviction 交給模型

## 原始摘要

Context Lifecycle：`Created → Selected → Active → Compressed → Evicted → Rehydrated if needed`。第一版 Context Manager 只需四件事：Pinned Context、Recent Window、Compressed History、External Artifacts，加上 Soft Token Budget——已經比「超過上限就砍前半段」可靠很多。

## 相關連結

- [[system-prompt-assembly-pipeline]] — **同系列 Day 11**：本篇整理好的 Context 最後如何組裝成每輪的 System Prompt；該篇把「Context 是工作區不是倉庫」直接套到 System Prompt 上，並給出 Context Budget 的三級壓縮清單
- [[agent-memory-lifecycle]] — **同系列 Day 10**：本篇管的是同一任務內的 Working Context，該篇管跨 Session 的 Memory；兩者的 Selection 是同名不同源的兩個機制（本篇選「已產生的資訊」，該篇選「值得跨 Session 留下的資訊」）
- [[context-engineering-stacking]] — **同一組概念的入門視角**：Context 如何被堆疊、40–60% 壓縮門檻；本篇的 Soft Budget 是其工程化版本
- [[agent-skills-vs-mcp]] — **同系列 Day 08**：Skill 的 Discovery/Selection/Loading/Execution 四層與 Context Hoarding 命名；本篇把同一套思想擴展到整個 Context
- [[agent-control-layer-permission-hooks-planning]] — 同系列 Day 4–6 控制層；本篇「Pinned 由程式決定、不交給模型」是「Prompt 不是安全邊界」在 Context 層的對應
- [[agent-tool-runtime-seven-layers]] — 同系列 Day 3；本篇的 Tool Result 生命週期分流是 Tool Runtime 輸出責任的下游
- [[claude-code-context-management]] — Claude Code 實務版：六成存檔、`/clear` 重開
- [[claude-code-auto-memory]] — Auto Memory 的完整機制（儲存位置、200 行硬截斷、三分流判斷法）
- [[headroom-context-compression]] — 把 Compaction 做成獨立壓縮層的工具實作（tool outputs / logs / RAG chunks 進 LLM 前先壓）
- [[context-engineering-fundamentals]] — Context Engineering 的轉換函數 F 視角（P 與 M 的平衡）
- [[claude-5-context-engineering-rules]] — Claude 5 世代「從防呆到授權」；與本篇「不要什麼都塞」同向
- [[memharness-memory-reconstruction]] — ⚠️ 對照：記憶／脈絡不是回放而是**重構**，加了反而變差的實證
