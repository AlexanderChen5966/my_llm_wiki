---
title: OpenAI Habitat：支撐 ChatGPT 10 億人流量的儲存平台架構與 Python→Rust 遷移
date: 2026-09-12
tags: [dev/architecture, ai/workflow, type/concept]
source: https://www.aiposthub.com/openai-habitat-chatgpt-storage-billion-users/
type: article
status: processed
asset_value: 60
---

## T（核心論點）

> **規模化的關鍵不是選對程式語言，而是知道何時該換下一個方案。** OpenAI 自建的線上儲存平台 Habitat（每秒逾 7,000 萬次請求、支援每週 10 億人使用的產品、管理逾 500 PB 資料），先用 Python 程式庫快速建立共用能力並接受「策略性技術債」，等 API 與營運方式穩定、成本跨過明確門檻後，才由 2 名工程師配合 Codex／GPT-5.5 於 2026 Q2 完成 Rust 重寫（CPU 效率提升 6 倍、記憶體效率提升 15 倍，已處理 95% 正式流量）。

Habitat 不是 ChatGPT 的記憶功能，而是 ChatGPT、Codex、API 與內部服務共用的資料交通中心，負責 routing、authorization、encryption、serialization、connection pooling。它從 2023 年 DevDay 隨 GPTs 上線的 Python 程式庫，因「服務版本難同步」的營運風險，於 2025 年中拆成獨立服務。

## B（適用場景）

**適用：**
- 任何維護連線池（connection pool）的後端服務，尤其想理解「為何一個修正配置就能解決反覆惡化的延遲問題」
- 評估「先用慢語言穩定產品、再用 AI Coding Agent 加速遷移到高效語言」這種技術債策略是否適用於自己團隊
- 想找具體案例說明「平台限制（不支援任意 SQL）反而換來可預測成本」的設計取捨

**不適用 / 注意：**
- 原文只是 Habitat 系列上篇，多租戶隔離、讀取效能與 Azure Cosmos DB 優化細節尚未公開，不能只憑本篇評估整套架構的可靠性
- 「2 名工程師靠 Codex 完成 Rust 重寫」的成效建立在該系統已有穩定 API、正式流量與監控資料之上，不能直接套用「AI 讓 2 人取代一個團隊」的人力估算

## R（操作步驟／關鍵工程模式）

1. **LIFO→FIFO 連線池修正 metastable failure**：`aiohttp` 連線池原採 LIFO（優先重用最後歸還的連線），突發流量後較慢的 server process 較晚歸還連線，卻被 LIFO 優先分配新請求，形成「慢節點收到更多請求、因而更慢」的回饋循環（metastable failure），修正前部分 process 負載曾達平均 5–10 倍。改成 FIFO（優先用較早歸還的連線）後打破循環。
   - 🔑 **這個模式對任何有連線池的後端服務都可轉用**：只要連線池預設 LIFO 且下游節點速度不均，就可能出現同樣的回饋循環，值得對照自己維護的 Spring Boot／其他後端服務的連線池預設策略是否也是 LIFO。
2. **尾端延遲（tail latency）優先於平均延遲**：一次操作串接數百筆查詢時，p99 延遲決定使用者體感速度。`asyncio` 事件迴圈的 CPU 密集任務會阻塞同迴圈其他 I/O，排程抖動可達數百毫秒甚至數秒；解法是限制每個 process 的同時請求數，改用大量水平 process 換取穩定尾端延遲。
3. **背景排程加 jitter**：feature flag 工具每分鐘全量更新設定，造成所有 worker 同時解析大型 JSON 產生週期性尖峰。修法是只下發所需設定、延長更新間隔、加入隨機時間差（jitter），避免「固定週期」被誤以為成本平均分散。
4. **Envoy + HTTP/2 multiplexing 避免驚群效應**：大量增加 Python process 解決事件迴圈壅塞後，若每個 process 各自建連線，部署或重啟時會對資料庫形成 thundering herd；改用 Envoy 集中連線並升級 HTTP/2 multiplexing，讓多筆請求共用同一連線的不同 stream。
5. **用限制換可預測成本**：Habitat 刻意只提供受限 NoSQL API（非任意 SQL），複雜查詢透過 CDC 分流到獨立的 Rockset 環境分析，犧牲彈性換取核心路徑成本可預測、易於水平擴充。

## C（關鍵行動）

- [ ] 檢查自己維護的後端服務（含連線池）預設是 LIFO 或 FIFO，評估是否有類似 metastable failure 風險
- [ ] 若團隊正處於「先用熟悉工具快速交付」階段，明訂技術債的可觀察還款條件（如 CPU/記憶體/延遲門檻），避免暫時方案變成無限期方案
- [ ] 導入 Coding Agent 做大型重寫前，先確保有穩定 API、真實流量與可驗證規格作為對照基準

## 原始摘要

OpenAI 首度公開自建線上儲存平台 Habitat 架構：每秒處理超過 7,000 萬次請求、支援每週逾 10 億人使用的產品、橫跨近 40 區域、管理超過 500 PB 資料，連續 3 年每年成長逾 10 倍。文章詳述其從 Python 共用程式庫演變為獨立服務的過程、asyncio 尾端延遲與 LIFO 連線池造成的 metastable failure 等具體工程問題，以及 2026 年 Q2 由 2 名工程師配合 Codex 與 GPT-5.5 完成 Rust 重寫，CPU 效率提升 6 倍、記憶體效率提升 15 倍。文章給出 4 項對 AI 產品團隊的啟示：先穩定服務邊界再追求效能、技術債要有可觀察還款條件、平台的好處常來自限制而非功能數量、導入 Coding Agent 前先建立可驗證規格。

## 相關連結

- [[rust-hot-path-optimization-ai-review]] — 同樣是「AI agent 協助 Rust 重寫/優化」案例，可對照兩篇對「AI 產碼速度 vs 人工把關責任」的態度是否一致
- [[production-agent-architecture]] — 生產級 Agent 工程架構的模組化清單，與本篇「Habitat 用限制換可預測性」的設計哲學可互相印證
- [[fastapi-python-api-framework]] — Python 後端框架條目，可對照本篇 Python 在超大規模場景下的實際瓶頸案例
