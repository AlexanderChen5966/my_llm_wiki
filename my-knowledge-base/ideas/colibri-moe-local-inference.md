---
title: Colibri：25GB RAM 筆電跑 7,440 億參數 GLM-5.2 的 MoE 常駐記憶體推論
date: 2026-07-17
tags: [ai/workflow]
source: https://www.koc.com.tw/archives/648922
type: idea
status: processed
---

> ⚠️ **原始素材正文缺失**：raw/ 檔案僅存標題與 frontmatter description，正文只有「Colibri」一詞。以下概念僅來自 description 欄位，內容確定性低，僅存供未來查閱，暫不建 wiki 條目。

## 核心概念（僅存 description 所述）
Colibri 讓網友在 **25GB RAM 筆電**上跑 7,440 億參數的 GLM-5.2。原理是抓住 MoE（Mixture of Experts）特性：每次 token 處理時，會改變的權重只有約 **11GB**（即被選中的 routed experts），其餘密集層（注意力機制、共享專家、嵌入層等約 170 億參數）可在記憶體中**常駐**。這讓 Colibri 得以在 25GB RAM 機器上運作。

## 為何暫存 ideas
- 正文缺失，無法核實工具細節、安裝方式、實測數據
- 概念方向（MoE routed experts 常駐記憶體推論優化）與本地 LLM 部署主題相關，若日後補到完整來源可升級

## 相關連結
- [[ds4-deepseek-local-inference]] — 同屬 MoE KV cache 優化 + 大模型本地推論（DeepSeek V4 Flash，96GB Mac 跑 250K context）
- [[local-llm-deployment-overview]] — 本地 LLM 部署概覽
- [[can-i-run-ai-tool]] — 下載前確認 GPU/RAM 是否足夠跑本地 LLM
