---
title: Flutter Agent 工程實戰：GenKit-Dart 企業級 Agent 完整模組拆解
date: 2026-07-09
tags: [dev/flutter, ai/agent, type/concept]
source: https://codelove.tw/@tony/post/3QKlEq（原文：https://juejin.cn/post/7658865284564926518）
type: article
status: processed
asset_value: 74
---

以一個實際 Flutter GenKit-Dart 專案為案例，逐模組拆解企業級本地 Agent 的完整工程實作：TurnPlan 三集合工具邊界、11 態 FSM、Agent Loop 不自寫迴圈交給 GenKit、5 種 RAG 檢索模式+RRF 融合、21-span Trace、三層脫敏、Auditor 只校驗不生成答案。核心命題：Agent 的作用不是讓模型變聰明，而是把 LLM 能力約束在業務邊界內、可回溯可驗證。

## T（核心論點）

GenKit-Dart 是 Google 官方唯一的 Flutter Agent SDK，能力介於「輕量」與 LangGraph 全家桶之間——它把模型呼叫/結構化輸出/工具呼叫/RAG/觀測整合成一套以 **Flow**（可型別校驗、除錯、追蹤、部署的函式）為核心的工程規範，但狀態機、Planner、Auditor 等業務邏輯仍需自行搭建。**貫穿全文的設計哲學是「模型即路由器」**：本地程式碼只提供能力快照、schema、權限邊界、隱私邊界，所有語意決策（工具選不選、query 怎麼改寫、claim 是否成立）都交給模型，本地不做關鍵字硬編碼分支。

## B（適用場景）

用 Flutter/Dart 建構本地化或全平台企業 Agent；評估「要不要自己寫 Agent」還是用通用產品（OpenCode 等）——本文答案是：企業內 Agent 的意義正是「你的業務 Flow、你的工具集合、你的隱私策略不是通用的」；理解為何企業 Agent 需要 RAG 而 Claude Code 這類程式碼 Agent 不需要（程式碼庫本身結構化可搜尋可執行，企業知識分散在私有系統裡，需要檢索增強層）。

## R（模組拆解）

- **TurnPlan 三集合模式**：`requiredToolNames` / `forbiddenToolNames` / `candidateToolNames` 表達工具邊界，模型看不到被禁止的工具（如 device_control），不需要在自然語言層面猜權限；意圖識別分三層（規則過濾高頻明確請求 → 小模型處理常規意圖 → 大模型整合兜底），每層都要有置信度判斷與降級策略
- **Soft plan + hard invariant**：ExecutionPlan 只是寫進 prompt 的 hint，模型可偏離；真正的「必須呼叫」靠 `forceToolUse=true` + 後置校驗 `_ensureRequiredToolsWereUsed()` 保證，折衷尊重模型推理自由度與工程約束
- **雙預算裁剪**：CJK 與英文的字元/token 比例不同，先跑字元預算做廉價粗過濾，再跑 token 預算終審；裁剪 fallback 順序固定（歷史 turn → 記憶 → web → 知識 → 資料，**使用者當前 user_turn 永不裁**）
- **11 態粗 FSM + 每業務 Flow 的細 FSM**：每個狀態標記 Actor（`fsm`/`llm`/`genkit`），trace 裡能立刻分辨「這步失敗是模型問題還是工程問題」；四種終態有嚴格不變量校驗，違反直接 `throw StateError`，寧可明顯 crash 也不留「看起來 committed 但其實有 error」的沉默污染
- **Agent Loop 不自寫 while 迴圈**：工具循環交給 GenKit `generateStream(maxTurns:)` 內部完成，避免重複實作 tool_call parsing/回填/cancellation/streaming；三條 recovery 路徑（required tool missing／context overflow 階梯壓縮／truncated answer 續寫）全部靠「丟異常→上層重試」完成，語意決策永遠回歸模型；**Prefill 機制**預跑無副作用必要工具（如 current_time）減少內部循環輪數，但有副作用工具絕不預跑
- **工具信任×副作用矩陣**：`sideEffectLevel`（readOnly/idempotent/mutating/externalAction）× `trustLevel`（localTrusted/hostTrusted/externalUntrusted）決定自動執行/需審批/拒絕；只有 readOnly+parallelSafe 才能並行，高危工具（cli/device_control）強制非並行+預設需審批；不同 provider 的 tool-call 格式需歸一化層，無法歸一化直接丟異常，不做字串關鍵字兜底
- **RAG：4 條 grounding 通路解耦 + 5 種檢索模式**（hashLexical/bm25/hybrid/embedding/hybridEmbedding）+ **5 類 Query Rewrite**（原查詢/關鍵字化/技術術語/程式碼API/同義擴展，由模型動態生成而非寫死同義詞表）+ RRF 融合（混合原始分數+rank 倒數，非純 RRF）+ Source Diversity Reranker（每來源最多 K 個 chunk，保證多樣性而非只保分數排序）
- **Trace 5 層 + Replay Harness + EvalDataset**：每個 lifecycle step 都是真實 GenKit span（21 個），TraceReport 帶 schema 版本號方便未來 breaking change 並存；Replay Harness 把一次 turn 的所有依賴打包可離線重放；EvalDataset 把標註過的 trace 轉成回歸資料集，定期跑 pass rate 趨勢
- **三層脫敏**：Key-based（結構化敏感欄位名直接 mask）+ Text-based（6 條串行 regex 抓隱式密鑰/路徑/stack trace）+ 長度限制（單條字串最長 1200 字元）；另外 Cancellation Token 全程貫穿、Context Overflow Guard 預算收緊重試而非直接 fail
- **Auditor 職責邊界「只能校驗和拒絕，不能生成領域答案」**：讓模型 A 生成答案，模型 B 抽取每條 factual claim 並在 evidence 裡定位，任一 verdict≠grounded 就產出結構化 IssueCode（非文字），觸發帶「具體 issue code + evidence」的結構化 regenerate（最多 3 次後 hard fail）——這個邊界犧牲效率但保持系統可追溯性，答案 100% 反映模型能力

## C（關鍵行動）

- [ ] Flutter 做 Agent 前先確認場景是否需要 GenKit 這層工程規範，或簡單場景直接呼叫模型 API 即可
- [ ] 工具邊界設計優先用「三集合模式」（required/forbidden/candidate）而非讓模型自己猜權限
- [ ] Auditor 類角色堅守「只校驗不生成」邊界，避免規則校驗與語意生成職責混淆
- [ ] Trace/Replay/EvalDataset 三件套是企業 Agent 長期可維護的基礎設施，不是事後可補的細節

## 相關連結

- [[synthesis/synthesis-android-flutter-ai-toolchain]] — Android/Flutter AI 工具鏈知識精粹（本主題 12 篇整合，本篇為 Layer 5 企業級深度）
- [[production-agent-architecture]] — 生產級 Agent 20 模組工程清單，同源的「把自由度關進工程邊界」思路
- [[ai-agent-dev-concepts-practitioner]] — Agent 開發九步流程與 SDK 選型對照，GenKit 在其中的定位
- [[flutter-google-io-2026]] — Flutter/Dart 官方最新動態，GenKit Dart 出處背景
- [[rag-retrieval-augmented-generation]] — RAG 基本流程，本文 5 種檢索模式+RRF 是進階實作對照
