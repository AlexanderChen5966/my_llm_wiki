---
title: Agentic RAG Day18：Context Builder 把搜尋結果組成帶來源身分的 ContextBundle
date: 2026-09-03
tags: [ai/agent, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10404634
type: article
status: processed
asset_value: 64
---

> **系列條目**：「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列 Day 18，
> 承接 [[agentic-rag-day17-cross-encoder-reranker]]（Day 17）「檢索線完全體」的排序結果，銜接
> [[agentic-rag-day19-generation-contract-prompt-design]]（Day 19）——本篇結尾直接預告 Day19
> 要替 Context 寫使用契約（來源是資料非指令、資料不足時如何回覆、輸出結構）。

## T（核心論點）

> **檢索排好序的 Top-3 不能直接串接餵給模型——三段文字若只用換行接起來，模型不知道各自來自哪份
> 文件，內容一多還會擠滿上下文視窗；檢索與生成之間需要一個「上下文建構器」（Context Builder），
> 負責保留來源身分、控制長度預算、固定順序，把結果組成結構化的 `ContextBundle`。**

一個可交給模型的來源至少要保留七項資料：來源代號、Chunk ID、文件 ID、標題、來源路徑、精排分數、
正文。來源代號用 `S1`/`S2` 這種只在單次請求中有效的短名稱，模型引用時不必抄寫長路徑，真正的標題
與路徑由程式保留、事後映射回去。

## B（適用場景）

**適用：** 檢索/精排完成後、送進生成模型前的中介層，需要固定的來源代號與可重跑的一致性（同組
搜尋結果重跑，Marker 與順序須一致）；需要控制送進 Prompt 的字元/Token 預算，避免任意處截斷程式碼
或設定步驟；需要留存 Context Trace（原問題、候選排名、被選來源、使用長度、索引版本）供除錯與
稽核；Top-K 結果中並非每一則都同等必要，需要程式化判斷「相關」不等於「回答必需」。

**不適用 / 注意：** 字元數只是開發階段的可讀近似，正式上線必須換算成該模型 Tokenizer 的真實
Token 預算，且要拆成 System Prompt、使用者問題與歷史、來源、預留輸出四筆帳，不能把來源塞滿整個
Context 上限；精排分數不應交給模型判斷「可信度」——分數是檢索訊號、不是事實正確率，模型只看
代號/標題/內容，分數留在內部 Trace 供拒答與診斷使用；Trace 不等於把完整正文永久寫進一般 Log，
含內部資訊的文件應只記 ID/分數/版本，正文放進有權限管控的稽核儲存。

## R（操作步驟）

1. **定義結構化的 ContextSource / ContextBundle**：`ContextSource` 保留 marker、chunk_id、
   document_id、title、source_path、score、text 七項；`ContextBundle` 除了給模型看的 `text`，
   還要保留 `sources`（結構化資料，供後續引用驗證）與 `used_characters`（實際用量）
2. **用短代號取代真實路徑**：來源代號（`S1`/`S2`）只在單次請求中有效，模型只需標記代號，
   不必複製標題或網址，真正的映射交給程式事後處理
3. **設定來源數量與長度預算**：先用可解釋的起點（本例 `max_sources=3`、`max_characters=2400`），
   依排名逐個加入完整 Chunk，若剩餘空間放不下下一個 Chunk 就整段停止；只有第一個 Chunk 本身就
   超過總預算時才截斷並加省略符號，確保至少有一份資料可供檢查
4. **保留精排排名順序，不再依標題或文件 ID 重排**：精排就是目前對相關性的最佳判斷；同一文件
   可以出現多個 Chunk，只要各自提供不同內容
5. **對 Context Builder 做四類單元測試**：結果不足時不可湊空來源；同組結果重跑 marker 與順序須
   一致；第一個 Chunk 超預算可安全截斷但後續放不下要整段停止；`document_id` 找不到對應 metadata
   應直接失敗而非把 ID 當標題繼續生成
6. **上線前把字元預算換算成 Token 預算**：拆成 System Prompt + 使用者問題與歷史 + 來源 +
   預留輸出四筆帳，並考慮第一名保留較多預算、後續來源只取相關句子等更細緻的分配策略
7. **留下上下文軌跡（Context Trace）**：每次問答至少記錄原問題、改寫後查詢、候選排名、被選入
   的來源代號、Chunk ID、使用長度與索引版本，讓錯誤可在知識庫更新或模型換版後仍可重建

## C（關鍵行動）

- [ ] 檢索結果送進生成模型前，一律經過 Context Builder 組成結構化 ContextBundle，不直接拼接字串
- [ ] 用快速確定性單元測試守住 Context Builder 的四類邊界情況（空來源、順序一致性、截斷、
      metadata 缺失）
- [ ] 上線前把字元預算換算為該模型 Tokenizer 的真實 Token 四筆帳，不能假設字元數等於 Token 數
- [ ] 建立 Context Trace 記錄機制，正文與 ID/分數分層儲存，兼顧可觀察性與資料最小化

## 原始摘要

Day 18 在檢索與生成之間插入「Context Builder」，把 Reranker 排序後的 Top-3 Chunk 組成帶有來源
代號（S1/S2…）、Chunk ID、文件ID、精排分數與正文的 ContextBundle，而非直接把文字拼接丟給模型。
首版採 max_sources=3、max_characters=2400 的字元預算近似值，完整 Chunk 優先、放不下則整段停止
（僅首個超預算才截斷），並強調要留存查詢/候選排名/選中來源等 Context Trace 供除錯與稽核。實測
查詢「HTTP 401和403差別」取回3來源、用1085字元，其中第三名（CSRF文件）只是主題相關但非回答
必需，證明「相關不等於必需」。文末預告下一篇要替 Context 寫使用契約。

## 相關連結

- [[agentic-rag-day17-cross-encoder-reranker]] — 系列前一篇（Day 17），本篇的 ContextBundle
  直接組裝自其精排排序結果
- [[agentic-rag-day19-generation-contract-prompt-design]] — 系列下一篇（Day 19），承接本篇結尾
  預告，把 Prompt 設計成可由程式驗證的生成契約，定義來源是資料非指令、資料不足時的出口與輸出結構
