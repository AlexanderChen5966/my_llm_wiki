---
title: OpenAI Private Intelligence：ZDR、Private Safety Processing、Private Inference 三者區分
date: 2026-10-08
tags: [ai/workflow, dev/architecture, type/concept]
source: https://www.aiposthub.com/openai-private-intelligence-zdr-guide/（引 https://openai.com/index/devday-2026-recap/）
type: note
status: processed
asset_value: 36
---

Private Intelligence 是 OpenAI 在 DevDay 2026 對「敏感資料能力」的統稱，底下其實是三個**層次不同、發布狀態也不同**的東西；評估企業 API 的合規與資料保留時，混用這三個名詞會在採購與架構上誤判。⚠️ **來源**：aiposthub（Philo，2026-09-30）單一來源、未實測、無價格，與同批 DevDay 文章同作者——**等 OpenAI 官方文件再核**。作者另附導入建議，見原文。

| 名稱 | 屬於哪一層 | 發布狀態（DevDay 2026） | 不保證什麼 |
|---|---|---|---|
| **ZDR**（Zero Data Retention） | 資料保留政策：在合格的 API 端點與組態下不保留應用資料 | 既有政策，有端點與組態資格限制 | 🔴 **≠ 不經處理、≠ 匿名**；**不自動涵蓋**所有端點、工具、檔案、背景模式或錯誤紀錄（作者依官方解讀） |
| **Private Safety Processing** | 安全監測：讓合格 ZDR 客戶的 API 流量也能做安全分類，且不保留客戶內容 | ✅ **發布時已可用**（官方轉述） | 只處理濫用與政策風險，不驗證業務輸出是否正確（作者觀點） |
| **Private Inference** | 執行隔離：在隔離環境推論，目標是可驗證的隱私保證 | 🔴 **僅預告秋季推出預覽，尚未推出** | 具體技術未公布；TEE、硬體證明等原文只以「這類系統常涉及」帶過，屬作者推測，**不代表 OpenAI 已採用** |

可轉用的判準（作者觀點）：**ZDR 只管供應商那一端**——應用端自己的日誌、監控、錯誤追蹤若留下內容，整條路徑就不算零保留。

## 相關連結

- [[chatgpt-claude-privacy-protection-9-checkpoints]] — 消費者操作層（關訓練、記憶、分享連結）；本條是企業 API 資料保留與推論隔離層，互補不重疊
- [[ideas/ai-products-privacy-policy-survey-36]] — 政策條款層
- [[claude-model-selection-guide]] — Anthropic 端：該條記載 Fable 5 強制 30 天保留、不提供 ZDR，可作跨廠商對照（本條不推論）
- [[claude-fable-5-usage-guide]] — Anthropic 端 Fable 5 規格（資料保留 30 天、不支援 ZDR）
- [[ai-agent-incident-governance-openai-anthropic]] — OpenAI／Anthropic 的 Agent 事件治理
- [[ideas/openai-devday-2026-chatgpt-platform-updates]] — 同批 DevDay 2026 其他平台更新
