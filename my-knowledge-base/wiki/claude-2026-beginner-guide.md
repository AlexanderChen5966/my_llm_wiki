---
title: Claude 教學 2026：從入門到 Projects、Skills、Cowork 新手必懂功能
date: 2026-07-09
tags: [ai/claude, type/how-to]
source: https://www.bnext.com.tw/article/91403/claude-tutorial-2026
type: article
status: processed
asset_value: 50
---

Anthropic 2026 年初推出 Skills、Cowork、Claude Code 三大工具後的新手完整導覽：方案選擇、介面（Chat/Cowork/Code 三模式）、提示詞三要素、模型選擇（Sonnet/Opus/Haiku）、五個新手必開功能，到三大進階工具比較。

## T（核心論點）

多數人還停在「貼問題、看回答、自己來」的階段，只用到 Claude 一小部分能力。搭配 Skills、Cowork 等工具，Claude 從對話機器人變成能實際執行任務的 AI 工作系統。入門建議路徑：先用 Sonnet 完成一個每天在做的任務，再逐步探索 Projects、Skills、Cowork。

## B（適用場景）

Claude 新手入門；決定要不要升級 Pro/Max；判斷該用哪個模式（Chat/Cowork/Code）處理任務。

## R（重點整理）

**方案**：免費版體驗、Pro（$20/月）標準用量、Max 5x（$100/月）5 倍容量、Max 20x（$200/月）20 倍容量+協作。

> ⚠️ **2026-09-16 已改版**：Cowork 獨立分頁已消失，併入一般對話 Chat，下方「三種模式」已縮減為 Claude 與
> Claude Code 兩入口，詳見 [[claude-cowork-merged-into-chat-2026-09]]。

**三種模式**：Chat（一般問答寫作）、Cowork（AI 同事，管理專案/分配任務/追進度）、Code（程式碼/除錯/建工具）。

**提示詞三要素**：背景設定（你是誰、目的）＋任務說明（愈精確愈好）＋規則限制（格式/語氣/字數）。範例：「我是一位行銷企劃，正在為主打 Z 世代的品牌活動準備社群文案。請幫我寫三個版本的活動標題……」對話變長導致回應品質下降時，直接說「請整理目前對話脈絡，換新視窗繼續」。

**模型選擇**：Sonnet（日常主力，八成以上任務足夠，速度快最省額度）／Opus（複雜推理、深度研究、高難度開發，支援延伸思考模式，代價是慢且貴）／Haiku（最快最便宜，免費方案可用，適合快速查詢與瀏覽器擴充）。

**五個新手必開功能**：① Connectors（串 Notion/Slack/Google Calendar）② Claude in Chrome（瀏覽器側邊欄直接呼叫）③ Use Style（固定回應語氣風格）④ Projects（集中管理同任務的文件與背景，長期工作必用）⑤ Research 模式（先拆解問題、搜尋多來源交叉比對、輸出附引用報告）。

> ⚠️ **2026-09-16 已改版**：「Cowork 桌面版限定」的說法已過時——Cowork 能力併入 Chat 後不再是獨立桌面分頁，
> 詳見 [[claude-cowork-merged-into-chat-2026-09]]。

**三大進階工具**：Cowork（桌面版限定，背景讀本機檔案、依排程執行、可裝 Plug-ins 官方範本涵蓋法律/行銷/財務）／Claude Code（自然語言寫程式除錯，非工程師也能做技術工作）／Skills（把常用工作流存成一鍵指令，本質是結構化 skill.md 檔，記錄角色設定/步驟/輸出規則）。

## C（關鍵行動）

- [ ] 新手從免費版開始，熟悉後升級 Pro 解鎖進階功能
- [ ] 八成任務用 Sonnet 即可，不需每次都上 Opus
- [ ] 長期持續性工作（固定客戶專案、研究計畫）優先建 Projects
- [ ] 對話變長回應變差是正常現象，換新視窗而非硬撐

## 補充資料｜新手前 4 個設定的操作順序與人工確認提醒（2026-09-11 併入）

來源：〈【AI工具地圖】Claude 新手教學：剛開始用 Claude，先完成這 4 個設定〉，aiposthub，2026-09-03，
<https://www.aiposthub.com/claude-beginner-guide/>。
（上方「五個新手必開功能」已列出 Connectors、Claude in Chrome 等項目，本節只補該文獨有、原條目未涵蓋的
**Memory 匯入設定**，以及 Connectors／Claude Code／Claude in Chrome 的具體操作提醒，不重複已有內容。）

原文建議的設定順序：**Memory → Connectors → Claude Code → Claude in Chrome**。

### Memory：把 ChatGPT 的背景整理後匯入（原條目未涵蓋）

進入 **Settings → Memory**，會看到兩個開關：**Search and reference chats**（搜尋過去對話）與
**Generate memory from chats**（從對話生成 Memory），長期使用建議都先開啟。

若原本使用 ChatGPT，**不是把 Memory 資料直接搬過去**，而是：

1. 在 Claude 的 **Import memory → Start import** 按 Copy，複製 Claude 提供的一段 Prompt
2. 把這段 Prompt 貼到 ChatGPT，讓它整理「目前記得的使用者背景」成文字
3. 把 ChatGPT 整理出的內容複製回 Claude 的 **Paste your memory details here**，按 **Add to memory**

也就是「ChatGPT 先整理成文字 → 這份文字交給 Claude 建立新 Memory」，兩邊記憶資料不會直接同步。

**操作提醒**：匯入前應先快速檢查一次內容，把公司機密、客戶資料、個人敏感資訊與已過期的背景資料刪掉再匯入；
Memory 不是存得越多越好，只留下未來仍會影響 Claude 回答方式的資訊即可。

### Connectors／Claude Code／Claude in Chrome 的操作提醒（原條目未涵蓋）

- **Connectors（Gmail/Drive/日曆）現在不只能讀，也能執行操作**：寄送 Email、建立行事曆活動、管理部分
  Drive 檔案。涉及寄信、修改行程、移動檔案這類會真正改變資料的操作，建議執行前自己確認一次內容；
  企業帳號的 Connector 可用範圍可能另受管理員權限限制。
- **安裝 Claude Code 不會自動取得 Gmail/Drive 等外部帳號資料**——它只負責讀寫本機資料夾、執行終端機指令，
  外部服務仍需另外透過 Connector 或 MCP 連接，兩者解決的是不同問題，可以搭配但不互相取代。
- **Claude in Chrome 涉及付款、刪除資料、公司後台等重要操作時，送出前應自己先確認一次**——它除了讀取
  網頁內容外，也能點擊、輸入文字、切換頁面，操作範圍比單純「摘要頁面」更廣，需要對應的謹慎程度。

## 補充資料：Claude 資料匯出與記憶匯入的限制（2026-09-29 併入）

來源：數位時代〈換AI不用重教一次！Claude⇄ChatGPT雙向搬家，記憶、對話紀錄、Skills轉移教學一次看〉（2026-09-28，<https://www.bnext.com.tw/article/92350/claude-chatgpt-migration>；參考 Ruben Hassid《How to AI》流程並比對 Anthropic／OpenAI 官方文件）。上方「Memory：把 ChatGPT 的背景整理後匯入」已有 Start import → Add to memory 步驟，本節**不重複**，只補增量（截至 2026-09-28 介面）：

- **記憶匯入的限制**：屬**實驗功能**，不一定每次都完整吸收；Claude 記憶**聚焦工作相關主題**，與工作無關的個人細節可能不會保留（一定要留的可在記憶設定頁用「Tell Claude what to change or remove」補充）；舊版介面的記憶區塊在「設定 → 功能（Capabilities）」
- **ChatGPT 問不出記憶時**：先確認 ChatGPT「設定 → 個人化 → 記憶」已開啟，再直接問「你記得我哪些事？」並要求逐條列出、整理成程式碼區塊
- **Claude 資料匯出**（反方向搬家用）：設定 → 隱私 → 匯出資料；**僅網頁版／桌面版**（iOS／Android App 不行）；Team／Enterprise 只有 **Primary Owner** 能匯出；下載連結 **24 小時**失效
- 🔴 **信件下載的 `.json` 只是索引檔**，不含對話內容，內含 2 個一次性下載連結（`conversations-000.zip` → `conversations.json`；`light_metadata-000`）。**這兩個連結等同通行密鑰，不要轉傳、不要貼進任何 AI 對話**——隱私面見 [[chatgpt-claude-privacy-protection-9-checkpoints]]
- **介面異動**：Anthropic 自 2026-09-16 起把 Chat／Cowork／Artifacts 整合為統一介面（Pro／Max 先行，TechCrunch 報導），設定位置可能再調整，見 [[claude-cowork-merged-into-chat-2026-09]]
- 記憶檔該怎麼整理（五章節＋四規則）見 [[agent-memory-lifecycle]] 的 2026-09-29 補充段

## 相關連結
- [[claude-code-best-practices-index]] — **索引型條目**：85KB 教科書級指南的 8 章地圖與難度分級，適合帶新人時給一條有順序的路徑
- [[claude-ai-guide-cowork-skills-token]] — 38 篇教學地圖（本文的完整路線延伸版）
- [[claude-cowork-setup-guide]] — Cowork 本地自動化模式詳細設定教學
- [[claude-code-newbie-guide]] — Claude Code 新手入門（工程視角互補）
- [[claude-skills-beginner-guide]] — Claude Skills 新手入門（Skill 架構深入版）
