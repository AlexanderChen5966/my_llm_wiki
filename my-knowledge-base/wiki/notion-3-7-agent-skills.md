---
title: Notion 3.7 Agent Skills：頁面即 Skill、權限即執行權，以及匯出 SKILL.md 的同步落差
date: 2026-09-23
tags: [ai/workflow, ai/claude, type/how-to]
source: https://www.aiposthub.com/notion-3-7-agent-skills-tutorial/
type: article
status: processed
asset_value: 60
---

Philo（AI Post Hub，2026-09-23）整理 Notion 3.7 Agent Skills。Notion 3.7 發布於 2026-09-15（依據 2026-09-22 可查到的官方發布說明與 Help Center）。本條目只收**產品機制與團隊治理**的新事實；Skill 範本怎麼寫、怎麼測，既有條目已涵蓋，只以連結帶過。原文的課程推廣段落已排除。

## T（核心論點）

**Skill 的價值在於成為有 owner、有版本、有權限的單一流程版本**，而不是把 Prompt 存進資料庫。在 Notion，**檢視權限就是執行權，編輯權限等於能改變所有人之後的輸出**；匯出到本機的 `SKILL.md` 是**快照**，不會跟著同步。Skill 能提高一致性，但**一致的輸出不等於正確的輸出**——它也能讓錯誤變得非常一致。

## B（適用場景）

**適用：**
- 團隊的流程知識已經在 Notion，想同時給 Notion Agent 與本機 agent（Claude Code、Codex、Cursor、Gemini、Grok）使用
- 判斷某段 Prompt 該不該做成 Skill：官方判準是「同一段 Prompt 會複製貼上第二次，就可能適合」
- 規劃 Skill 放在協作平台時的權限與版本治理

**不適用 / 注意：**
- 知識文件不一定是流程：產品介紹、會議背景資料維持一般頁面即可
- 涉及法務、財務、人事、醫療或對外承諾的輸出，仍要人工審核事實與決策
- 方案與推出範圍可能調整，以工作區實際看到的 Library 為準

## R（操作步驟）

### Notion 3.7 的產品機制

1. **Skill 本身就是 Notion 頁面**：可分享、編輯、放進資料庫、看頁面歷史。建立方式：側欄 `Library` → `Skills` 分頁 → `+ Add a skill`，或在既有 SOP 頁面右上選 `Use as a skill`。
2. **觸發方式**：在 Agent 對話輸入 `/` 手動選擇，或由 Notion Agent 判斷後自動使用；可在 Skill 頁面關閉自動使用。**要能自動觸發，Skill 必須位於 Skills database，且有清楚的 Description**（Description 同時給人看、也給 Agent 判斷時機）。Skills database 欄位含 Description、Files、Tags。
3. **權限就是治理邊界**：檢視權限＝可以執行；編輯權限＝可以改變所有使用者後續拿到的結果。流程更新不必通知每個人換 Prompt，但編輯者太多時，錯誤規則也會立刻擴散。Skill 不會讓原本沒權限的人取得資料，但頁面與附件的分享設定錯誤仍可能外洩。
4. **匯出到本機 agent**：Skill 頁面 `•••` → `Download to local agents` → 選目標工具，輸出 `SKILL.md`＋獲准分享的附件。**本機副本不會自動同步**：Notion 端更新後會提示重新下載，但本機執行的仍是下載當下的檔案。匯出前三檢查：移除機密與個資、確認附件真的需要且有權提供、在首段寫清楚適用範圍。
5. **Custom Agent 與子 Agent**：Custom Agent 只要有權存取 Skill 頁面就能使用；3.7 也支援 Custom Agent 呼叫其他 Custom Agent（子 Agent）。這是同版的另一項能力，不等於 Skills 自己會建立 Agent 團隊。

### 四種機制怎麼分（精簡版）

| 類型 | 適合用途 | 如何啟動 |
|---|---|---|
| Instructions | 幾乎每次互動都要遵守的個人偏好 | 持續套用 |
| Skill | 某一類任務的可重複方法、標準或格式 | 手動選擇或由 Agent 判斷 |
| Notion Agent | 一次性或多步驟的臨時任務 | 在聊天中提出要求 |
| Custom Agent | 需要固定工具、權限與觸發條件的持續流程 | 人工、排程或事件觸發 |

### 團隊導入最容易忽略的五件事

1. **沒有指定 owner**：每個 Skill 記錄 owner、最後驗證日期、下一次檢查時間，否則很快過期。
2. **把所有文件都做成 Skill**：只有描述重複任務、輸入、步驟與輸出的內容才適合。
3. **自動觸發開得太早**：描述太廣會選錯流程；先手動執行 3–5 次穩定後再開。
4. **把一致輸出誤認為正確輸出**：格式整齊不代表內容對。
5. **忽略 Notion 與本機版本差異**：定義誰負責重新下載、如何測試、舊版何時停用；重要流程在標題或內容加版本日期。

Skill 範本本身（何時使用／需要的輸入／執行規則含「資訊不足不可猜測」／輸出格式）、附一個好範例與一個反例、三類測試（資訊完整／不完整／內容混雜），和 [[sop-to-skill-4-steps]]、[[skill-md-writing-techniques]] 的撰寫方法重疊，不在此重寫。

> 附註：同版另有自訂 MCP 連線（執行變更前會要求確認），目前以 Business／Enterprise 方案 Beta 提供。

## C（關鍵行動）

- [ ] 若把本庫 Skill 放上協作平台，先決定 owner 與編輯權限範圍，再開放檢視
- [ ] 任何「從別處下載的 SKILL.md 副本」都記錄來源版本日期，並排定更新檢查點
- [ ] 新 Skill 一律先手動跑 3–5 次，再考慮自動觸發

## 原始摘要

Notion 3.7 把團隊 SOP 做成可分享、有版本歷史的 Notion 頁面，可由 Notion Agent 手動或自動觸發，也能下載成 `SKILL.md` 給多種本機 agent 使用。價值是讓團隊對「何時用、輸入、限制、輸出」有單一可維護版本；但它不會自動解決權限、錯誤輸出與 owner 缺席的問題。

**歸屬判斷（2026-09-23）**：本篇與 [[sop-to-skill-4-steps]] 部分重疊（範本與測試），但本條目 R 層的主體（產品機制、權限對應、匯出與同步落差、四機制對照、五個治理盲點）都是既有條目沒有的新內容，超過一半；目標條目的 T 是「SOP 轉 SKILL.md 會逼你補齊判斷標準」的撰寫方法論，裝不下「Skill 放在協作平台時的權限與同步治理」這條新論點，因此獨立成條。

## 相關連結

- [[sop-to-skill-4-steps]] — SOP 轉 SKILL.md 的撰寫方法論；本條目談的是 Skill 放上協作平台後的權限與同步治理
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧（範本、description 寫法）
- [[notion-ai-workspace-analysis]] — Notion 的產品定位分析；本條目是其 3.7 版的 Skill 機制
- [[agent-skills-vs-mcp]] — Skills 與 MCP 的選擇判斷
- [[claude-skill-audit-four-step-minimal-method]] — Skill 盤點與稽核方法
- [[claude-skills-asset-building]] — Skills 從指令到資產（該篇提過用 Notion 建 AI 資源庫）
