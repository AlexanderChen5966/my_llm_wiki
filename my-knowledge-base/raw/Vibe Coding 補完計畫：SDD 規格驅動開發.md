---
title: "Vibe Coding 補完計畫：SDD 規格驅動開發"
source: "https://hackmd.io/@flora8411/SyjOSg2qWe"
author:
published:
created: 2026-05-21
description: "講者：高見龍"
tags:
  - "clippings"
status: processed
---
> 講者：高見龍 20260321

## 核心觀念

**Context window 不是越大越好。**

真正重要的是「先定好什麼叫完成」——在開始寫程式之前，把規格確立清楚。

---

## SDD 是什麼？

SDD（Specification Driven Development，規格驅動開發）強調在動手實作之前，先把三件事做好：

### 1\. 需求 — 寫 User Story

用具體場景描述功能，而不是模糊的需求。

> 作為使用者，我希望能用信箱和密碼登入，這樣我就能存取個人資料。

### 2\. 設計 — 明確定義邊界

**要做什麼，不做什麼** ，兩者都要明確寫下來。

### 3\. 任務 — 拆分小任務

把工作拆成每個都有 **明確目標** 和 **驗收標準** 的小任務。

---

## SDD 工作流程

規格 → 規格錨定（加入版控）→ 規格即原始碼（理想）

每個規格的生命週期：

**討論 → 提案 → 實作 → 更新 → 歸檔**

---

## 推薦工具

| 工具 | 說明 |
| --- | --- |
| **openspec** | 推薦使用 |
| **Kiro** | 規格管理工具 |
| **speckit** | 規格工具 |
| **Spectra** | 將文件轉成向量 RAG，可透過 Claude 查詢用法 |

Spectra 本身也是用 SDD 開發的，可參考： [https://kaochenlong.com/spectra-with-openspec](https://kaochenlong.com/spectra-with-openspec)

---

## 實務建議

- **先 Plan，不要一次做全部**
- 套件和 Git 的寫入權限不要給 AI
- Context window 滿了？有完整規格文件就能切換 AI 繼續做

### SDD vs Plan Mode 的差異

Plan Mode 是對話內的暫時狀態；SDD 強調 **把文件留下來** ，好處是：

- 隨時可以快速切換任務
- Context window 滿了也能接著做
- 可以換不同的 AI 工具繼續

---

## 最後的思考

> AI 是放大能力差距的工具。

未來不是 AI 取代人類，而是 **懂得用 AI 的人取代不懂的人** 。

建立清晰的信任邊界，讓 AI 做它擅長的，人負責定義「什麼是對的」。