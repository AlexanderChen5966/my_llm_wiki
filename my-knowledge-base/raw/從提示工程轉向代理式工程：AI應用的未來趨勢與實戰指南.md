---
title: "從提示工程轉向代理式工程：AI應用的未來趨勢與實戰指南"
source: "https://vocus.cc/article/698d4276fd897800013083ae"
author:
  - "[[鄭智維 Wesley]]"
published: 2026-02-12
created: 2026-05-19
description: "本文深入探討了從提示工程到代理式工程的轉變，分享了吳恩達《Agentic AI》課程中的五大核心重點：為何選擇 Agentic AI、反思設計模式、工具使用與函式呼叫、評估與錯誤分析、以及規劃與多代理系統。文中詳述了 Agentic AI 的優勢、實踐方法，並結合企業場景提供了具體的應用範例"
tags:
  - "clippings"
status: processed
---
這週參加 Google 活動帶來了非常大的震撼，深刻感受到在 AI 開發的領域中，我們正處於從 ****提示工程 (Prompt Engineering)**** 轉向 ****代理式工程 (Agentic Engineering)**** 的關鍵節點。

為什麼會看到廣告

週末趁著熱度，花點時間上完了吳恩達 (Andrew Ng) 在 DeepLearning.AI 的課程《Agentic AI》。這門課讓我徹底理解 LLM 如何從單純的文字生成器，演變為能主動調用工具並進行複雜規劃的「代理人 (Agent)」，也讓我進一步掌握了 AI 應用的核心。

這堂課程非常實用，甚至解開了我最近幾個專案的卡點，非常推薦給大家！以下整理了課程中的 5 大核心重點與實戰場景拆解。

---

## 1\. 為什麼選擇 Agentic AI？

****Agentic AI**** 的核心在於它不僅僅是要求 LLM 生成回應（Zero-shot），而是一個 ****迭代的工作流程（Iterative Workflow）**** 。它允許模型像人類一樣，透過思考、研究、修正和改進來完成任務。

- 突破模型性能極限傳統的 LLM 使用方式就像要求人類「一次性寫完論文，不能使用退格鍵」。 ****代理式工作流程允許模型進行迭代（思考、研究、修訂），這能顯著提升產出品質。**** 資料顯示，將較舊的模型（如 GPT-3.5）包裝在代理工作流程中，其在程式碼編寫任務上的表現甚至能超越沒有使用代理的更強模型（如 GPT-4）。
- 處理複雜任務透過將任務拆解為多個步驟（如：先寫大綱 → 再搜尋 → 最後寫作）， ****代理能完成單次提示無法處理的深度任務。****
- 平行處理 (Parallel Processing)代理可以同時執行多個子任務。例如：同時進行三個不同的網頁搜尋並下載頁面，這比人類依序閱讀網頁的速度快得多。
- ****模組化與靈活性代理架構允許開發者針對特定步驟更換工具或模型（例如更換搜尋引擎或升級特定步驟的 LLM），使系統更具彈性。****

---

## 2\. 反思設計模式 (Reflection)

反思是提升系統性能最簡單且有效的方法之一。這種模式雖然不能保證 100% 正確，但通常能帶來顯著的性能提升。

### (1) 核心概念

要求 LLM 檢查自己生成的結果（如程式碼或郵件草稿），找出錯誤或改進空間，並撰寫更好的版本。

### (2) 外部回饋 (External Feedback) 的重要性

雖然自我反思有用，但結合「外部資訊」的反思更為強大：

- 程式碼執行：讓 LLM 寫程式，執行它，並將 錯誤訊息 (Error logs) 或輸出結果餵回給 LLM。這能讓模型根據具體的報錯來修正程式碼，而非憑空猜測。
- 工具驗證：例如使用字數統計工具檢查文案是否超長，或用搜尋引擎驗證事實，將這些客觀數據作為反思的輸入。

---

## 3\. 工具使用與函式呼叫 (Tool Use & Function Calling)

工具使用擴展了 LLM 的能力邊界，使其不僅僅是文字生成器。

- 解決幻覺與能力短板：LLM 本身不知道「現在幾點」或無法進行精確的數學運算。透過提供工具（如 getCurrentTime 或計算機），LLM 可以請求呼叫這些函式來獲取準確資訊。
- 運作流程：LLM 不會直接執行函式，而是輸出一個請求（JSON）。系統執行函式後，將結果（如 "8:00 AM"）回傳給 LLM，最後由 LLM 生成最終回答。
- 程式碼執行作為工具 (Code Execution)：這是一種特殊的強大工具。允許 LLM 編寫並執行 Python 程式碼來解決數學問題或數據分析，這比讓 LLM 直接預測答案準確得多。
- MCP (Model Context Protocol)：這是一個開放標準，旨在解決開發者需要為每個數據源（Slack, GitHub, Drive）重複編寫連接器的問題。MCP 讓開發者只需構建一次連接，就能讓 AI 應用程式存取多種數據與工具。

---

## 4\. 評估與錯誤分析 (Evals, Error Analysis)

這是區分高效能團隊與普通團隊的關鍵技能。Andrew Ng 強調要建立「 ****有紀律的開發流程」。****

### (1) 先構建，再分析 (Build fast, then analyze)

不要花數週空想完美架構。 ****先快速建立原型（Quick and dirty），然後觀察失敗案例**** 。

### (2) 錯誤分析 (Error Analysis)

- 檢查軌跡 (Traces)：當系統表現不佳時，不能只看結果。必須檢查中間步驟，找出是哪個組件出錯（是搜尋關鍵字下錯？還是搜尋引擎回傳垃圾網站？）。
- 量化數據：建立試算表，統計每個組件導致錯誤的頻率。這能幫助你決定優先優化哪個部分（例如：若 75% 的錯誤來自資料庫查詢，就專注修復該部分）。

### (3) 評估矩陣 (Evals 2x2 Matrix)

根據任務性質選擇評估方式：

![zoomable](https://images.vocus.cc/5e295913-b079-43d4-9747-b4f46887d459.png)

### (4) 組件級評估 (Component-Level Evals)

除了評估整個系統，也應隔離出單一組件（如搜尋模組）進行獨立評估，以加速優化迭代。

---

## 5\. 規劃與多代理系統 (Planning, Multi-agent Systems)

這是更高階的自主性設計，讓 AI 自行決定如何解決問題。

### (1) 規劃 (Planning)

- 不再由開發者寫死步驟（Hard-code），而是提供一組工具，要求 LLM 生成 「逐步計畫 (Step-by-step plan)」 並執行。
- 程式碼規劃 (Planning with Code)：相比於輸出 JSON 計畫，讓 LLM 直接寫程式碼來規劃步驟（例如使用 Pandas 分析數據）通常表現更好且更靈活。

### (2) 多代理系統 (Multi-agent Systems)

透過指派不同角色（如研究員、圖形設計師、撰稿人）來協作，能比單一通用模型完成更複雜的任務。常見的溝通模式包括：

- 線性接力 (Linear)：A 做完傳給 B，B 傳給 C。
- 層級/主管模式 (Hierarchical/Manager)：一個經理代理指揮其他子代理，並整合結果。
- 深層層級模式 (Deep Hierarchy)：多層級的管理結構。
- 全對全 (All-to-all)：允許任何代理隨時與其他代理對話。

---

## 企業場景拆解範例

最後，將上述概念應用到實際的企業場景中：

### 案例 A：發票處理 SOP (結構化流程)

**這類有明確規則的 SOP 最容易轉化，通常是 線性 (Linear) 且 低自主性 (Less Autonomous) 的流程。**

1. \[輸入\] 接收發票 PDF。
2. \[工具\] 呼叫 API 將 PDF 轉換為文字。
3. \[LLM\] 判斷這是否為發票？如果是，提取關鍵欄位（付款人、金額、日期）。
4. \[工具\] 呼叫資料庫 API，將提取的資料寫入系統。

### 案例 B：客戶訂單查詢 SOP (需判斷流程)

**這類流程需要模型介入判斷，屬於 Human-in-the-loop 的應用。**

1. \[LLM\] 讀取客戶 Email，提取關鍵資訊（客戶姓名、訂單號碼）。
2. \[工具\] 根據提取的資訊，查詢訂單資料庫 (Orders Database)。
3. \[LLM\] 根據查詢結果，撰寫回覆 Email 草稿。
4. \[工具\] 將草稿放入佇列，等待人類員工審核後發送。

---

總結來說，Agentic AI 透過「工具與規劃」獲得執行力，透過「反思與多代理協作」提升品質，並依賴嚴謹的「評估與錯誤分析」來持續優化系統。這正是未來 AI 開發的新範式！

![vocus｜新世代的創作平台](https://images.unsplash.com/photo-1694903110330-cc64b7e1d21d?crop=entropy&cs=srgb&fm=jpg&ixid=M3wyNTY4N3wwfDF8c2VhcmNofDEwfHxBSXxlbnwwfHx8fDE3NzA4NjYxNDl8MA&ixlib=rb-4.1.0&q=85) ![zoomable](https://resize-image.vocus.cc/resize?norotation=true&quality=80&url=https%3A%2F%2Fimages.unsplash.com%2Fphoto-1694903110330-cc64b7e1d21d%3Fcrop%3Dentropy%26cs%3Dsrgb%26fm%3Djpg%26ixid%3DM3wyNTY4N3wwfDF8c2VhcmNofDEwfHxBSXxlbnwwfHx8fDE3NzA4NjYxNDl8MA%26ixlib%3Drb-4.1.0%26q%3D85&width=740&sign=vi9Bgwu5px0ql8sLRGxMhSRO8kIpPpsSRMfbgpZCqW8)