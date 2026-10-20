---
title: LightRAG 中文導讀：知識圖譜 + 向量搜尋的 RAG 框架概念介紹
date: 2026-07-26
tags: [ai/workflow, dev/architecture]
source: https://tools.wingzero.tw/articles/3996（介紹 GitHub 專案 hkuds/lightrag）
type: note
status: processed
asset_value: 18
---

中文導讀文章，面向初次接觸 LightRAG 的讀者，概念層級說明香港大學 HKUDS 推出的這套開源 RAG 框架如何結合知識圖譜（實體與關聯）與向量檢索，透過在文件匯入階段自動抽取實體關聯建立知識圖譜、同時保留向量索引，讓複雜跨文件問題（例如「某公司曾與哪些企業合作，並有哪些共同投資的專案？」）能沿著圖譜關係鏈找到更完整脈絡的答案，而非只靠關鍵字命中。

文中列出的核心賣點：自動建立知識圖譜、向量+圖譜混合檢索、支援增量更新（不需重建整個索引）、簡潔 API、相容多種 LLM 與向量資料庫、模組化可替換元件、開源自架。並將 LightRAG 定位為「與 LangChain/LlamaIndex 互補而非取代」——後者是應用開發框架，LightRAG 聚焦 RAG 本身的檢索品質與知識管理。

## 為何列為研究筆記而非 wiki 條目

本文屬於第三方轉介性質的概念導讀（無實測數據、無架構圖細節、無安裝配置資訊），asset_value 18 明顯低於 wiki 門檻。完整的技術細節（雙層架構原理、五種查詢模式、LLM 四角色配置、效能評測數據、生產部署要點）已由官方 GitHub README 完整涵蓋，見 [[lightrag-architecture-and-query-modes]] 的完整 wiki 條目——本篇可作為該條目的「白話前導版」保留備查，不重複展開技術細節。

## 相關連結

- [[lightrag-architecture-and-query-modes]] — LightRAG 官方 README 完整 wiki 條目：雙層架構、五種查詢模式（local/global/hybrid/naive/mix）、LLM 角色分工、效能評測數據、生產部署細節，本篇的技術深化版
- [[rag-retrieval-augmented-generation]] — RAG 基本概念與三種檢索方式，本篇是其「向量 RAG 遇到跨文件關聯查詢受限」痛點的知識圖譜化解法
