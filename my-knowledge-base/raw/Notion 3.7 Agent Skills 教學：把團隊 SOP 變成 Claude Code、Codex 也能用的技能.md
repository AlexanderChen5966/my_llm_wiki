---
title: "Notion 3.7 Agent Skills 教學：把團隊 SOP 變成 Claude Code、Codex 也能用的技能"
source: "https://www.aiposthub.com/notion-3-7-agent-skills-tutorial/"
author:
  - "[[Philo]]"
published: 2026-09-23
created: 2026-09-23
description: "Notion 3.7 推出 Agent Skills，可把團隊 SOP 做成可分享、可更新的 AI 指令，並下載到 Claude Code、Codex、Cursor 等工具。"
tags:
  - "clippings"
status: processed
---
很多團隊已經累積不少「很好用的 Prompt」，但它們通常散落在聊天紀錄、私人文件或某位同事的腦中。真正需要重複執行時，大家又各自複製一份，過幾個月便出現五個版本，沒有人知道哪一個才是最新標準。

Notion 在 2026 年 9 月 15 日發布 3.7 版，把這個問題包成 Agent Skills。你可以把校稿規則、會議摘要格式、提案審查流程或職缺撰寫標準做成 Notion 頁面，讓 Notion Agent 在需要時執行，也能下載成 `SKILL.md` ，交給 Claude Code、Codex、Cursor、Gemini 或 Grok 等本機 AI Agent 使用。

先說結論： **Agent Skills 的價值不是把 Prompt 存進資料庫，而是讓團隊對「何時使用、需要哪些輸入、必須遵守什麼限制、輸出長什麼樣」有同一個可維護版本。** 它能提高一致性，但不會自動解決權限、錯誤輸出與流程擁有者缺席的問題。

本文以 2026 年 9 月 22 日可查到的 Notion 官方 3.7 發布說明與 Help Center 為準，帶你建立第一個 Skill，也會說明它和一般指令、Notion Agent、Custom Agent 的差別。

## Notion Agent Skills 是什麼？

Skill 是一份可重複使用的指令，用來教 Notion AI 如何完成某一類工作。它本身就是 Notion 頁面，因此可以像一般頁面一樣分享、編輯、整理到資料庫，並透過頁面歷史查看修改。

官方給的簡單判斷是：如果你會把同一段 Prompt 複製貼上第二次，它就可能適合做成 Skill。常見例子包含校稿、把會議紀錄轉成待辦事項、把草稿套進固定模板，以及統一文件格式。

Skill 可以由使用者在 Agent 對話中輸入 `/` 主動選擇，也可以讓 Notion Agent 在判斷任務相符時自動使用。若你不希望它自行觸發，可以在 Skill 頁面關閉自動使用。

## Notion 3.7 帶來哪些重要變化？

### 1\. Skills Library 成為團隊共用入口

個人建立的 Skills 可以從側邊欄 Library 的 Skills 分頁管理。團隊也能建立 Skills database，把不同流程的 Skill 集中在一個資料庫，並加入 Description、Files 和 Tags 等欄位。

Description 不只是給人看的摘要，也幫助 Notion Agent 判斷何時應該使用。若希望 Skill 能自動觸發，它必須位於 Skills database，並有清楚的 Description。

![Notion 官方 Skills Library 畫面，顯示團隊可共用與管理的技能清單](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/notion-skills-library-official.jpg)

Skills Library 集中管理個人與團隊的可重複指令。 圖片來源： 官方發布素材 。

### 2\. Skills 可以像頁面一樣分享與更新

你可以只分享一個 Skill，也能分享整個 Skills database。擁有檢視權限的人可以執行 Skill。擁有編輯權限的人則能修改所有使用者後續會用到的內容。

這一點同時是優勢與風險。流程更新後不用通知每個人替換 Prompt，但如果太多人都有編輯權限，錯誤規則也會立刻擴散。團隊最好替每個 Skill 指定一位 owner，也就是負責維護內容的人。

### 3\. 可以匯出到本機 AI Agent

Notion 3.7 支援把 Skill 下載到 Claude Code、Codex、Cursor、Gemini 或 Grok。Notion 會輸出一個 `SKILL.md` ，並附上獲准分享的參考檔案。

`SKILL.md` 是用 Markdown 撰寫的技能說明檔。本機 Agent 讀取後，可以依照同一套目標、限制和輸出格式工作。當 Notion 裡的 Skill 更新時，已下載版本會出現提示，提醒使用者重新取得最新檔案。

這讓 Notion 可以成為流程的維護中心，而程式碼工具仍在原本環境執行。不過，它是「下載副本」而非永遠即時同步。更新後是否重新下載，仍需要管理。

![Notion 官方技能匯出畫面，可下載到 Claude Code、Codex、Cursor、Gemini 或 Grok](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/notion-skills-export-official.jpg)

Notion Skill 可下載為 SKILL.md，交給多種本機 AI Agent 使用。 圖片來源： 官方發布素材 。

### 4\. Custom Agents 可以使用 Skills 與子 Agent

只要 Custom Agent 有權存取 Skill 頁面，就能依照其中內容執行。Notion 3.7 也讓 Custom Agent 呼叫其他 Custom Agents，把大型任務分給具不同指令、資料權限與模型的子 Agent。

例如，主要 Agent 接到一份客戶回饋後，可以叫分類 Agent 判斷問題類型，再叫報表 Agent查詢指標，最後整合成 Slack 更新。這是同一版更新裡的另一項能力，不等於 Skills 自己會自動建立 Agent 團隊。

![](https://www.youtube.com/watch?v=ndTwYKuWjlE)

Notion 3.7 官方發布頁連結的子 Agent 實例。這段影片補充同版的 Agent 協作功能，並非 Skills 操作示範。

## Skill、一般指令、Notion Agent 和 Custom Agent 的差別

| 類型 | 適合用途 | 如何啟動 |
| --- | --- | --- |
| Instructions | 幾乎每次互動都要遵守的個人偏好 | 持續套用 |
| Skill | 某一類任務的可重複方法、標準或格式 | 手動選擇或由 Agent 判斷 |
| Notion Agent | 一次性或多步驟的臨時任務 | 在聊天中提出要求 |
| Custom Agent | 需要固定工具、權限與觸發條件的持續流程 | 人工、排程或事件觸發 |

如果只是希望所有回答都用繁體中文，放在 Instructions 比較合理。若要把每週會議整理成固定格式，適合 Skill。若要每逢新客訴自動分類並通知負責人，則更接近 Custom Agent。

## 實作教學：建立第一個「會議紀錄轉行動清單」Skill

第一次可以選擇一個低風險、每週都會發生、結果容易檢查的流程。會議紀錄轉行動清單很適合，因為輸入和輸出都清楚，也不需要立刻修改外部系統。

### 步驟 1：從 Library 建立 Skill

在 Notion 側邊欄開啟 `Library` ，切換到 `Skills` 分頁，選擇 `+ Add a skill` 。你也可以打開既有 SOP 頁面，從右上角選單選擇 `Use as a skill` 。

如果是團隊共用，建議建立一個 Skills database。先只用一個資料庫，等到不同部門有不同權限、擁有者或測試需求時，再拆成多個資料庫。

### 步驟 2：寫清楚觸發時機與邊界

一份好 Skill 至少要有目標、輸入、限制與輸出格式。可以使用以下範本：

```markdown
# 會議紀錄轉行動清單

## 何時使用
當頁面包含會議逐字稿或條列筆記，且使用者要求整理後續工作時使用。

## 需要的輸入
- 會議日期與參與者
- 原始紀錄
- 專案名稱

## 執行規則
1. 只根據原始紀錄整理，不自行補任務。
2. 每項任務需包含負責人、截止日與來源段落。
3. 沒有負責人或日期時標記「資訊不足」，不可猜測。
4. 決議、待辦與未決問題分開。

## 輸出格式
先寫 100 字內摘要，再輸出決議、待辦表格、未決問題。
```

「何時使用」能幫 Agent 選對 Skill。「不可猜測」則避免它把討論內容誤寫成已承諾的任務。

### 步驟 3：加入一個好範例和一個反例

只寫抽象規則容易產生不同解讀。附上一段短輸入與合格輸出，能讓 Agent 看懂欄位、語氣和資訊不足的處理方式。再加一個反例，說明「不要把建議當成決議」或「不要自行指定負責人」。

Notion 官方建議 Skill 頁面保持精簡，較長的規範改用連結或支援檔案。這能讓主要流程容易維護，也避免每次執行都載入大量無關資料。

### 步驟 4：用三種真實案例測試

至少準備以下測試：

1. 資訊完整：每項任務都有負責人和日期。
2. 資訊不完整：故意缺少截止日，確認 Agent 不會猜測。
3. 內容混雜：同時有決議、想法和被否決方案，確認分類正確。

不要只測官方範例。真正會暴露問題的，通常是你團隊平常寫得不完整、格式不一致的資料。

### 步驟 5：設定分享與編輯權限

檢視權限代表可以執行。編輯權限代表可以改變所有使用者後續取得的結果。將編輯者限制在流程 owner 和必要協作者，其他人先以檢視為主。

若 Skill 會讀取客戶資料、合約或內部財務內容，也要檢查頁面和附件的分享範圍。Skill 不會讓原本沒有權限的人突然取得資料，但錯誤的頁面分享設定仍可能造成外洩。

### 步驟 6：在 Notion Agent 中執行

在 Agent 對話輸入 `/` ，選擇 Skill 後指定要處理的頁面。也可以在編輯器反白文字，從選單執行。第一次建議手動啟動，確認三到五次結果穩定後，再考慮開啟自動使用。

自動使用適合意圖明確的低風險任務，例如統一格式。涉及刪除、對外發送、金額或權限變更時，應保留人工確認。

## 如何把 Skill 交給 Claude Code 或 Codex？

打開 Skill 頁面，從右上角 `•••` 選擇 `Download to local agents` ，再選擇目標工具。Notion 會輸出 `SKILL.md` 與獲准分享的附件，讓本機 Agent 依同一套流程執行。

匯出前先做三項檢查：

- 移除不應離開 Notion 的機密內容與個資。
- 確認參考檔案真的需要，且有權提供給目標工具。
- 在 Skill 首段寫清楚適用範圍，避免本機 Agent 在無關任務誤用。

匯出後也要記錄版本。Notion 會提示來源已有更新，但本機執行的仍是下載當下的檔案。重要流程可以在 Skill 標題或內容加入版本日期，並安排固定更新節點。

## 想把 Claude Code 與 Notion 串成實際工作流，可以學什麼？

如果你還不熟悉本機 AI Agent， [《Claude Code 不只會寫程式！打造個人資料庫工作流》](https://academy.aiposthub.com/p/skill-shot-05-claude-code?ref=aiposthub.com) 適合先理解 Claude Code 如何讀取資料、遵守固定流程並產出可重用結果。這會讓你更容易判斷哪些 Notion SOP 值得做成 Skill。

想進一步讓 Notion 成為內容管理中心，可以接著看 [《Claude Code × Notion：不用會寫程式也能打造個人網站》](https://academy.aiposthub.com/p/skill-shot-06-claude-code-notion?ref=aiposthub.com) 。它聚焦 Notion 與 Claude Code 的實際串接，可作為 Skills 匯出後的下一步應用。

## 團隊導入時，最容易忽略的 5 件事

### 1\. 沒有指定 owner

沒有負責人，流程很快就會過期。每個 Skill 都應記錄 owner、最後驗證日期和下一次檢查時間。

### 2\. 把所有文件都做成 Skill

知識文件不一定是流程。只有當內容描述重複任務、輸入、步驟與輸出時，才適合轉成 Skill。產品介紹或會議背景資料，通常維持一般頁面即可。

### 3\. 自動觸發開得太早

官方預設允許 Notion Agent 在認為合適時自動使用 Skill。若描述太廣，可能選錯流程。新 Skill 先手動執行，確認觸發條件清楚，再決定是否開啟自動使用。

### 4\. 把一致輸出誤認為正確輸出

Skill 可以讓錯誤變得非常一致。涉及法務、財務、人事、醫療或對外承諾時，仍要有人審核事實和決策，不能只看格式整齊。

### 5\. 忽略 Notion 與本機版本差異

Notion 頁面更新後，本機的 `SKILL.md` 不會自動改寫。團隊要定義誰負責重新下載、如何測試，以及舊版本何時停止使用。

## Notion 3.7 的其他 Agent 更新值得用嗎？

同一版還加入 Custom Agent 呼叫子 Agent、自訂 MCP 連線、會後自動工作與模型選擇等能力。MCP 是讓 Agent 連接外部工具和資料的介面。官方說明自訂 MCP 連線在執行變更前會要求確認，並先以 Business、Enterprise 方案 Beta 提供。

這些功能適合已經有清楚流程和權限邊界的團隊。新手不必一次全部導入。先讓一個 Skill 穩定完成低風險任務，再把它交給 Custom Agent 或外部工具，會更容易找到錯誤發生在哪一層。

## 常見問題

### Notion Skills 免費方案可以用嗎？

方案與推出範圍可能調整，應以工作區當下的 Library、Notion AI 設定與官方方案頁為準。若看不到 Skills 分頁，也要確認功能是否已推送到你的帳號。

### Skill 會自動讀到整個工作區嗎？

不會因為它是 Skill 就取得所有內容。實際可讀範圍仍受頁面分享、資料庫權限和 Agent 存取設定控制，但管理員仍需檢查 Skill 引用的附件與外部連線。

### 可以直接把公司 SOP 全部匯出給 Claude Code 嗎？

技術上可下載 Skill 和核准附件，但不建議一次匯出全部。應依任務採最小必要範圍，先移除機密與個資，再確認本機工具、模型服務與儲存位置符合公司政策。

### Notion Skills 和 Claude Skills 一樣嗎？

概念上都用結構化指令教 Agent 執行重複任務，但管理介面、支援檔案、觸發方式和權限模型不同。Notion 3.7 的特點，是能在 Notion 頁面維護，再下載為 `SKILL.md` 給多種本機 Agent 使用。

## 結語：把 Prompt 變成有人負責的流程資產

Notion 3.7 Agent Skills 讓團隊從「收藏好用 Prompt」走向「維護可執行的方法」。同一份規則可以在 Notion Agent 執行，也能下載給 Claude Code、Codex 等工具，減少每個人各自保存版本的混亂。

但真正決定成效的，仍是 Skill 是否有明確觸發條件、真實範例、權限邊界、測試案例與負責人。先從一個每週會做、出錯成本低的流程開始，穩定後再開自動觸發和跨工具使用，會比一次把所有 SOP 交給 Agent 更可靠。