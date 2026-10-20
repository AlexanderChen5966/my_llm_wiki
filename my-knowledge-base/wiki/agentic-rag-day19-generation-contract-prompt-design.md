---
title: Agentic RAG Day19：把 Prompt 寫成可由程式驗證的生成契約
date: 2026-09-03
tags: [ai/agent, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10404701
type: article
status: processed
asset_value: 64
---

> **系列條目**：「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列 Day 19，
> 承接 [[agentic-rag-day18-context-builder-context-bundle]]（Day 18）的 `ContextBundle`（檢索結果
> 整理成 S1/S2 代號），與
> [[agentic-rag-day20-first-pipeline-llmclient-protocol]]（Day 20）互連——Day20 首次跑通完整
> Pipeline 時，正好驗證了本篇建立的契約會被違反。也與既有 [[rag-citation-fail-closed-validation]]
> （Day 21）的引用驗證機制鋪墊基礎。

## T（核心論點）

> **Prompt 不是一段祈求模型聽話的咒語，而是一份生成契約：哪些資料可以使用、哪些行為禁止、
> 資料不足時走哪個出口、輸出要長成什麼樣子——而且這份契約要能被程式檢查，不能只停留在文字上。**
> System Message 放固定規則、User Message 放本次問題與來源，因為來源是不可信的外部資料，
> 必須與系統指令分層，防止 Prompt Injection 混淆。

模型只需標記 `[S1]`/`[S2]` 代號，不必複製標題或網址，真正的來源映射交給程式處理，讓引用成為
**封閉集合**——這次只提供 S1/S2/S3，模型就沒有合法理由輸出 S4，這條規則會在 Day 21 被做成驗證器。

## B（適用場景）

**適用：** 設計 RAG／Agent 系統的生成階段 Prompt，需要讓輸出可由程式解析與驗證；建立
System/User 訊息分層，隔離不可信外部來源；設計「回答/拒答」兩種合法狀態的輸出契約；規劃
Prompt 測試的分層策略（結構測試/契約測試/生成品質評測）。

**不適用 / 注意：** Prompt 是行為引導，不是型別系統，更不是安全邊界——即使規則寫得清楚，模型
仍可能漏欄位、引用不存在的來源，或正文與宣告欄位不一致（Day 20 的真實案例正好證明這點）；
JSON 格式本身也不是 Prompt Injection 的防護罩，惡意文字放進 `content` 後模型依然看得見。

## R（操作步驟）

1. **System/User 分層**：System Message 保存不隨問題改變的規則（Grounding、拒答出口、輸出格式），
   User Message 只放本次問題與來源，因為來源是不可信外部資料
2. **只給短代號，不要模型重寫來源**：Prompt 只要求模型寫 `[S1]`，不要求複製標題/路徑/網址，
   降低虛構風險，讓引用成為封閉集合
3. **來源包成結構化 JSON**：只保留回答需要的代號、標題、Chunk ID、正文，不含內部精排分數或
   絕對檔案路徑
4. **只留兩條合法輸出路徑**：`{"status":"answered","answer":"...[S1]","citations":["S1"]}` 或
   `{"status":"insufficient","answer":"...","citations":[]}`——拒答不引用，回答必須引用，
   正文與 `citations` 欄位可交叉檢查一致性
5. **規則分兩類**：任務規則（要做什麼）與禁止事項（不能跨越的邊界）分開列，讓 Prompt Ablation
   有清楚單位，且能由程式檢查的規則交給 Validator，不靠加強語氣重複
6. **確認任務沒有混合**：同一份來源可能承接不同任務（比較 vs 抽取），規則若同時要求「簡短」與
   「完整列出」會互相拉扯，本系列第一版只處理單一技術問答任務
7. **分三層測試 Prompt**：純結構測試（快，每次執行）→ `StaticLlmClient` 契約測試（固定替身，
   每次執行）→ 真實模型生成品質評測（慢，變更時執行），前兩層不需啟動本機模型即可攔住多數
   契約錯誤
8. **延後加入對話歷史**：Day 19 payload 暫不含聊天歷史，避免模型引用自己上一輪輸出形成「自我
   背書」，Day 24 才把歷史獨立成 `conversation_history` 並註明它只協助解析省略、不能當事實來源

## C（關鍵行動）

- [ ] Prompt 設計時將 System（固定規則）與 User（問題＋不可信來源）分層，不混在同一層
- [ ] 建立「回答/拒答」兩種合法輸出狀態，正文引用與宣告欄位設計成可交叉檢查
- [ ] 把規則分成任務規則與禁止事項兩類，能由程式驗證的規則不留在 Prompt 裡加強語氣
- [ ] Prompt 版本化：每次請求 Trace 記錄 `prompt_version`、模型 ID、生成參數與知識庫版本
- [ ] 採三層 Prompt 測試策略，優先用固定替身攔截契約錯誤，再花成本跑真實模型評測

## 原始摘要

「Agentic RAG」系列 Day 19（article_id 10404701），把 Prompt 設計成可由程式驗證的「生成契約」而非
單純指令：System Message 固定 Grounding／拒答／輸出格式規則，User Message 以 JSON 傳入問題與不可信
來源（sources 只含代號、標題、chunk_id、正文，不含路徑或分數）。模型只能輸出兩種合法狀態
（answered 附引用 / insufficient 不引用），且要求任務規則與禁止事項分開列，避免規則堆疊互相稀釋。
文末明確預告下一篇（Day 20）會把此 Prompt 與檢索、Context Builder、本機 LLM 串成完整流程，並提前
警告「回答讀起來正確不代表已通過機器驗證」。

## 相關連結

- [[agentic-rag-day18-context-builder-context-bundle]] — 系列前一篇（Day 18），本篇的
  System/User 分層與來源代號規則直接承接其 `ContextBundle` 設計
- [[agentic-rag-day20-first-pipeline-llmclient-protocol]] — 系列下一篇（Day 20），用本篇建立的契約
  跑出第一個真實回答，並發現正文引用與 citations 欄位不一致的真實案例，直接驗證了本篇「Prompt
  不是驗證器」的預告
- [[rag-citation-fail-closed-validation]] — 既有條目（Day 21），把本篇的引用封閉集合規則做成程式
  可執行的驗證器與 Fail Closed 機制
