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

## 補充資料

**2026-11-19 補充（來源：aiposthub〈Notion 代理協作配置解析：官方分享七種用法，先把任務、記憶與權限分清楚〉，Philo，2026-10-13，https://www.aiposthub.com/notion-seven-agent-collaboration-setups-memory-skills-permissions/）**——二手整理，作者未實測：
- **三種代理入口，權限範圍不同**：① **Notion MCP**＝在外部 AI 工具裡讀寫 Notion，**以連接者本人的 Notion 權限行動**（提示寫「只讀這頁」不等於技術上只能讀這頁）；② **Custom Agents**＝Notion 內共享、背景執行，依排程或事件啟動，**只看得到明確分享給它的內容**；③ **Notion 內建的外部代理**（如 Claude 代理測試版）＝留在 Notion 互動，**用 Notion credits、不支援自帶 Anthropic 帳號計費**（此條件不能套到其他第三方代理）
- **官方七種配置是用戶案例、不是七個新功能**：看板拖卡交付、共享代理記憶、隨進度更新的待辦、工作成果紀錄、經外部 MCP 的財務流程、團隊共用技能庫、AudioPen 口述入庫；各自的產品、方案、連接與費用要分別確認
- 呼應本條 T 層：**共用技能庫的更新機制比一次下載更重要**——關鍵技能要指定維護者與適用範圍，否則舊下載副本會讓共享庫分裂成多個版本；驗證法是讓兩個代理跑同一典型任務，差異來自版本／遺漏指示就先修共同資料
- 作者建議（觀點）：看板狀態要分「準備執行（輸入足夠）／待審核」，測試卡只啟動一次、失敗保留原因；共享記憶每項要有來源、負責人、更新時間；待辦「完成」要有產出證據

**2026-11-19 補充（來源：aiposthub〈Notion Google Drive 代理連線開放測試，即時讀寫文件，權限與觸發怎麼設定？〉，Philo，2026-11-05，https://www.aiposthub.com/notion-agents-google-drive-beta-permissions/）**——轉述官方公告（2026-11-03）與 Help Center，**測試版**、作者未實測：
- 代理可**即時**搜尋、讀取、建立、更新 Drive 檔案（官方稱不儲存、不建索引）；這和工作區 AI 搜尋連接器（索引）、連結預覽是**不同連線**——搜尋得到不代表代理能改
- 條件：工作區成員＋已啟用 Notion AI（訪客不行）；個人在 Settings → Connections 連 Google 帳號；組織的 Google Workspace 應用程式規則可能卡住，Custom Agent 可能需管理者批准
- **有限 vs 完整存取**：有限＝可讀本人能讀的、只能改代理建立或使用者選取的檔案；完整＝沿用本人 Drive 讀寫權限。**Notion Agent 一律完整存取**，有限存取只在 Custom Agent 有意義；Custom Agent 還要在 Tools & access 加入 Drive，可再限定檔案／資料夾／共用雲端硬碟
- 🔑 權限即執行權的延伸：**共享 Custom Agent＝能用它的人都能搜尋它連的 Drive**；一個 Custom Agent 只能連一個 Google 帳號；**僅限同一個 Google Workspace**（跨 Workspace 分享給本人的檔案也讀不到）
- 原地編輯只限 Google Docs／Sheets／Slides；PDF、Word 只能讀，更新版另存新檔
- **資料夾觸發**：依頻率檢查指定資料夾或共用雲端硬碟，**子資料夾變更不觸發**、不能監看整個 My Drive → 用固定的待處理資料夾收件

**2026-11-19 補充（來源：aiposthub〈Notion 代理留言進度更新：團隊怎麼在同一頁追蹤、改派與接手工作？〉，Philo，2026-11-08，https://www.aiposthub.com/notion-agent-comment-progress-20261108/）**——轉述官方貼文（2026-11-06）與 Claude 任務板官方指南，作者未實測：
- **進度寫進原頁留言回覆**，代理也出現在頁面瀏覽者旁；有權存取任務的人可在工作中途留言**補充背景、提問或改方向**，卡住時直接回覆補資料；方向錯可從「Claude is working」即時工作階段（含推理與工具呼叫）**停止**，暫停後留言即可**續做**；已寫進頁面的錯誤改用頁面版本歷史復原（停止≠復原）。原交辦人離開時，接手者從任務文字＋留言就能**改派／接手**，不必重建私人對話
- **Claude 任務板案例的條件**（External Agents，建在 Custom Agents 設定上）：Notion **Business／Enterprise** 工作區、可用的 **Notion Credits**、由工作區擁有者啟用 External Agents；HIPAA 工作區與限制 Anthropic 模型的工作區預設停用 Claude。用量看 Credits 儀表板與代理 Insights。這是單一官方案例，不能外推成所有 Notion 代理的方案與計費
- 🔑 權限即執行權的延伸：代理**只看得到明確授權的頁面、資料庫與連線**（規格、設計文件沒分享就等於缺背景照做）；GitHub 可按儲存庫授權，開 PR 需 PAT 對 Contents 與 Pull Requests 讀寫；只問架構可先用 Codebase Q&A。留言裡補的一次性要求**不會變成長期規則**，要沿用須改代理指令；不同任務類型建議分成多個代理
- **進度可見≠完成**：成果仍要接回頁面、檔案（生成的 PPT／Excel 連結回原頁）或 **PR 審查**（看差異與預覽再決定合併），看板狀態建議分「可交代理／處理中／待審查／完成」

## 相關連結

- [[sop-to-skill-4-steps]] — SOP 轉 SKILL.md 的撰寫方法論；本條目談的是 Skill 放上協作平台後的權限與同步治理
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧（範本、description 寫法）
- [[notion-ai-workspace-analysis]] — Notion 的產品定位分析；本條目是其 3.7 版的 Skill 機制
- [[agent-skills-vs-mcp]] — Skills 與 MCP 的選擇判斷
- [[claude-skill-audit-four-step-minimal-method]] — Skill 盤點與稽核方法
- [[claude-skills-asset-building]] — Skills 從指令到資產（該篇提過用 Notion 建 AI 資源庫）
- [[agent-ux-status-transparency-maturity]] — 代理進度須來自真實狀態；對照本條 2026-11-19 補充「進度可見≠完成」
- [[self-hosted-agent-workspace-approval-licensing]] — 對照：OpenBot 明說「技能是指令不是權限」，與本條「檢視權限＝執行權」是不同的權限模型
