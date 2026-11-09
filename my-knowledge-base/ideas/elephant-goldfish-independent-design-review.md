---
title: Google 開源 Elephant-Goldfish：大象保留脈絡寫設計、金魚不繼承對話獨立審稿，寫程式前先過三層審查
date: 2026-11-05
tags: [ai/agent, ai/workflow]
source:
  - https://www.aiposthub.com/google-elephant-goldfish-plugin-design-review-agents/（aiposthub／Philo，2026-10-26；二手整理）
  - 一手（本批未開啟核對）：opensource.googleblog.com 公告、github google/elephant-goldfish-model
type: idea
status: processed
asset_value: 18
---

> 🔴 **來源與適用**：aiposthub 二手整理，**本批未實測、一手公告與 README 未核對**。作者用的範例為假設情境。
> ⏰ **時效**：安裝指令為 **2026-10-26 查閱的快照**，README 與公告時點不同，以實際環境為準，未實測。
> 官方自稱：「數千名內部工程師每日使用」為**公告自稱，無對照數據**；「大象／金魚」成效無數據。授權 Apache 2.0；README 明示**非 Google 正式支援產品**。
> 為何放 ideas：asset_value 18（頻率 2×耗時 3×複雜度 3，保守估；樂觀 36 但頻率主觀且從未安裝實測）；留下的是「獨立讀者代理」的審稿設計。排除：站內延伸閱讀。

## 核心設計：兩種角色分開

- **大象（Elephant）**：保留整段對話脈絡，負責腦力激盪與寫設計草稿
- **金魚（Goldfish）**：**不繼承對話**，只讀設計文件，當獨立審稿者——文件裡沒寫的前提，它就看不到，因此能暴露「作者以為不用說」的缺口

## 流程（依二手整理，2026-10-22 開源）

1. `egm-brainstorm`：不寫程式，追問邊界與替代方案
2. `egm-draft`：寫入持續更新的 Markdown 設計文件（問題、方案、替代方案、要改的檔案）
3. 審查，**三層**：
   - 理解檢查：回述文件在說什麼
   - 對抗審稿：找缺口與反例
   - 實作就緒檢查：檔案路徑、資料結構、函式簽章是否寫得夠具體
4. 另有評論處理與**規格漂移報告**（實作與文件是否偏離）；建議設計核可後以 **PR 提交**再開始實作

## 與既有做法的差異

- [[spec-first-ai-coding-workflow]]：由人審 spec；這裡改為「不共享脈絡的代理」先審一輪
- [[pre-implementation-adr-alignment-skill]]：以三句話＋ADR 對齊；這裡多了獨立讀者與就緒檢查
- 概念上近似「只讀文件的外部讀者轉述」測試（讓沒參與討論的讀者先回述文件），可用在任何設計文件審查，不限此外掛

## 可帶走的通則（不必安裝外掛）

- 設計文件寫完，用**全新對話**（不給歷史）讓另一個代理回述，再比對原意
- 實作前再問一次：檔案路徑、資料結構、函式簽章是否已寫明
- 實作後比對文件，記錄漂移

## 待查

- 外掛市集安裝命令（Antigravity、Claude Code、Codex 各有）本批未實測
- 官方公告與 README 的細節差異

## 相關連結

- [[claude-agents-skill-file-comparison]] — AGENTS／CLAUDE／SKILL 檔案分工（同屬「讓代理讀文件」脈絡）
- [[design-md-google-format]] — Google 的 DESIGN.md 格式
- [[spec-first-ai-coding-workflow]] — 規格先行流程
- [[pre-implementation-adr-alignment-skill]] — 實作前 ADR 對齊
- [[spec-driven-ai-workflow]] — 規格驅動流程
