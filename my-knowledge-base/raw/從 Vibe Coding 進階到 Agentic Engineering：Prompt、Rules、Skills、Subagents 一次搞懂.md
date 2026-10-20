---
title: "從 Vibe Coding 進階到 Agentic Engineering：Prompt、Rules、Skills、Subagents 一次搞懂"
source: "https://www.aiposthub.com/vibe-coding-agentic-engineering-prompt-rules-skills-subagents/"
author:
  - "[[Vicky Tsai]]"
published: 2025-12-25
created: 2026-05-19
description: "當 AI 開始參與開發流程，工程師該如何管理而非只下指令？本文用工程視角拆解 AI 協作的四個層次。"
tags:
  - "clippings"
status: processed
---
前幾個月，我們帶大家實測了許多 [Vibe Coding 工具](https://www.aiposthub.com/vibe-coding/) ，相信你已經很熟悉「一句話叫 AI 幫我做」。但當你開始想把作品做成能長期維護、能交付、能擴張的專案時，光靠「憑感覺 + 祈禱」就不太夠了。

> 你需要的是一套更工程化的 AI 協作方法： **Agentic Engineering（代理工程）**

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2025/12/image-88.png)

這篇文章會用最直白的職場比喻，帶你搞懂四個關鍵層次： **Prompt（提示詞）、Rules（規則）、Skills（技能）、Subagents（子代理）** ，並補上「新手如何落地」的做法，讓你從 Vibe Coding 的快，走向真正可控、可複用、可迭代的穩。

---

## 為什麼 Vibe Coding 會「快，但不穩」？

你可能聽過「Vibe Coding」（憑感覺寫程式）——打開 ChatGPT 或 Cursor，輸入一句話，然後期待 AI 直接吐出能跑的程式碼。它確實很快、很爽，但也常見三種失控：

- **風格漂移** ：同一個專案越寫越不像同一個人寫的（Architectural Drift）。
- **需求走樣** ：你說 A，它做出 B，還很自信。
- **安全與品質風險** ：把敏感資訊寫進程式、產出不安全查詢、測試缺失。

要從「憑運氣」進階到專業的 **Agentic Engineering** ，你要做的不是更會下 Prompt，而是學會把 AI 當作一支團隊管理：你是技術總監，AI 是你的數位員工。

---

## 核心概念：Prompt、Rules、Skills、Subagents 的差異

接下來用「公司運作」的比喻拆解四層架構，你會發現： **Prompt 是你說什麼；Rules 是你規定怎麼做；Skills 是你給它工具；Subagents 是你把人分組。**

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2025/12/image-89.png)

---

## Prompt（提示詞）：給實習生的「口頭命令」

### 定義

Prompt 是你當下輸入給 AI 的指令，用來觸發動作或生成回答。專業一點說，它是「 **瞬時意圖的觸發器** 」。

### 職場比喻

你對實習生說：

- 「幫我查一下這段 API 的用法」
- 「幫我寫一個登入頁」

### 特點

- **暫時性** ：對話關了就忘。
- **高波動** ：你描述不清楚，AI 就容易自由發揮。
- **最容易踩雷** ：尤其是「直接開寫」而沒有規格與限制。
![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2025/12/image-90.png)

### 更穩的 Prompt 小技巧（你立刻用得上）

- 先下「 **目標 + 範圍 + 輸出格式** 」再下手
- 先要「 **方案與風險** 」再要「 **程式碼** 」
- 先要「 **最小可行版本（MVP）** 」再擴充

---

## Rules（規則）：寫在紙上的「員工手冊 / SOP」

如果你發現自己每次都在提醒 AI：「請用繁體中文」、「命名規則一致」、「不要把金鑰寫進程式碼」，那你缺的不是 Prompt，而是 Rules。

### 定義

Rules 是寫在專案規則檔（例如 `.cursorrules` 、 `CLAUDE.md` 、或團隊內部規範文件）裡的長期指導原則，讓 AI 在整個專案期間都遵守一致標準。

### 職場比喻

這就是公司員工手冊：

- 文件格式規範、命名規範、架構規範
- 安全規範、測試規範、PR 規範

### 特點

- **持久性** ：寫一次，整個專案都生效。
- **強制性** ：降低風格走樣與架構漂移。
- **可審核** ：規則檔本身就是可版本控制的資產。

### 範例（你可以直接搬到規則檔的方向）

- 「所有資料庫查詢必須使用參數化查詢，禁止字串拼接。」
- 「新增功能必須包含最基本的測試與錯誤處理。」
- 「回覆與註解使用繁體中文；程式碼命名採 lowerCamelCase。」
![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2025/12/image-91.png)

---

## Skills（技能）：發給員工的「工具箱與權限」

AI 不是只有腦，它還需要手去「操作外部世界」：讀檔、查資料庫、開瀏覽器、查文件、跑測試、開 PR。

### 定義

Skills 指 AI 與外部工具互動的能力，常透過標準化介面連接（例如 **MCP：Model Context Protocol** ）。你可以把它理解成： **替 AI 插上外接設備與權限** 。

### 職場比喻

你給員工電腦還不夠，你還要：

- 裝好 IDE、開通 Git 權限
- 讓他能查內部文件、看數據儀表板
- 允許他跑測試、查 log、讀取專案檔案

### 特點

- **讓 AI 從「會說」變成「會做」**
- **可控權限** ：該能做什麼、不該能做什麼，要設計清楚
- **越強越要治理** ：工具越多，越需要 Rules 來防呆

### 實際場景

你問：「今天活躍用戶多少？」

- 沒 Skills：AI 可能只能猜一個數字
- 有 Skills：AI 可以直接查詢資料來源並回報結果（並附查詢依據）
![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2025/12/image-92.png)

---

## Subagents（子代理）：組建專門的「專案小組」

當任務變大（例如重構、做完整功能模組、跨檔案修改），單一 AI 容易「腦霧」：資訊爆量、上下文混亂、改 A 壞 B。

### 定義

Subagents 是多個專門處理特定任務的 AI 實例，每個都有獨立的上下文空間，彼此分工、不互相干擾。

### 職場比喻（最直覺）

你把任務拆給不同角色：

- **Plan Agent（PM）** ：拆需求、排優先級、定驗收標準
- **Coding Agent（工程師）** ：依規格寫程式、改架構
- **Test Agent（QA）** ：補測試、找邊界案例、壓回歸
- **Review Agent（Tech Lead）** ：看風險、抓一致性、守規範

### 特點

- **分工合作** ：降低單一模型的上下文負擔
- **品質更穩** ：專注的人做專注的事
- **適合平行化** ：同時做規格、實作、測試、文件
![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2025/12/image-93.png)

---

## 一張表快速看懂差異（收藏用）

| 名稱 | 角色類比 | 核心作用 | 持久度 | 最適合用在 |
| --- | --- | --- | --- | --- |
| **Prompt** | 口頭命令 | 告訴 AI「現在做什麼」 | 暫時 | 一次性任務、簡單問答 |
| **Rules** | 員工手冊 | 規定「必須遵守什麼」 | 長期 | 團隊風格一致、安全與架構治理 |
| **Skills** | 工具箱/權限 | 讓 AI「能操作外部」 | 功能型 | 查文件、跑測試、查 DB、看 log |
| **Subagents** | 專案小組 | 讓多個 AI 分工合作 | 任務導向 | 大任務：重構、模組開發、系統整合 |

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2025/12/image-94.png)

---

## 新手落地指南：從 Vibe Coding 走向 Agentic Engineering 的 3 步

### 步驟 1：先建立 Rules，讓 AI「變得可控」

你不需要一次寫到完美，先做最小版本：

- 語言與命名規範
- 安全底線（不寫金鑰、不拼接 SQL）
- PR / 測試 / 文件的最低要求

> 你會立刻感覺：AI 變得更像同事，而不是靈感型藝術家。

### 步驟 2：Spec-First（規格優先），先寫「驗收標準」再寫碼

不要一開始就叫 AI 寫程式。先讓它產出：

- 功能清單（MVP vs Nice-to-have）
- 資料流 / API 介面
- 驗收條件（什麼叫做完成）
- 風險與回滾策略

這一步會大幅降低「寫完才發現方向錯」的成本。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2025/12/image-95.png)

### 步驟 3：需要做事才開 Skills，需要做大事才開 Subagents

- 只是改一個元件？先別急著上多代理
- 要跨檔案重構？才上 Subagents
- 要讓 AI 讀/查/跑？才開 Skills（並保留人工審核）

---

## 安全提醒：越進階，越要「關掉 YOLO」

當你開始讓 AI 自動執行任務（例如自動修復、批次改檔、跑指令），務必：

- **預設人工確認** （Human-in-the-loop）
- **重要操作先 dry-run** （先列出變更清單）
- **保留版本控制與回滾點**
- **敏感資料永不上下文** （金鑰、憑證、個資）

這不是保守，而是專業工程的基本盤。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2025/12/image-96.png)

---

## 結語：你不是在學更多工具，而是在升級「管理 AI 的方法」

Vibe Coding 的價值是「快速啟動」，Agentic Engineering 的價值是「可控交付」。當你把 Prompt、Rules、Skills、Subagents 這四層架起來，你會開始擁有一種新能力： **把 AI 當成團隊、把輸出當成工程、把結果當成可迭代資產** 。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2025/12/image-97.png)

---

## 想讓 AI 成為你團隊的一員？

💡

如果你希望從 side project 邁向產品化實作，將「AI + 程式」變成真正的解決方案核心，而不是只停留在玩玩工具的階段，那你一定要深入了解 ****Vibe Coding**** 導入服務如何協助團隊實現目標。