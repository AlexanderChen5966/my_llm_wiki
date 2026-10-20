---
title: 替工作桌減壓：Context Isolation 與 Compaction 的判準
date: 2026-08-13
tags: [ai/prompt-engineering, ai/agent]
source: https://ithelp.ithome.com.tw/articles/10402718（《觀察 AI，也觀察自己：30 天重新學會如何學習》Day 13）
type: article
status: processed
asset_value: 70
---

同系列前作：Day 05–09 併入 [[ai-terms-analogy-scaffolding]]、Day 10 見 [[jailbreak-four-cases-and-guardrail-layers]]、Day 11 見 [[prompt-injection-vs-jailbreak]]、**Day 12（Context Engineering I：Context 如何被堆疊）見 [[context-engineering-stacking]]**。前一篇把 Context 堆起來，這一篇開始整理桌面。

## T（核心論點）

> **Context Engineering 不只要決定什麼資訊該放進 Context，也要決定哪些探索應該在別的地方完成，最後只帶回真正有用的結果。**

前一篇問「AI 還需要知道什麼？」，這一篇多問一題：**哪些東西不必一直留在它眼前？** 所以 Context Engineering 到最後是：

> **選擇、隔離、壓縮與遺忘的工程。**

## B（適用場景）

**適用：**
- 判斷一段工作該交給 Subagent 還是留在主 Agent（本篇給了可直接用的四行判斷表）
- 寫 Compaction／交接摘要時該保留什麼、捨棄什麼
- 對「什麼時候該另開新對話」給出可解釋的理由

**不適用 / 注意：**
- 🔴 本篇明確反對「多 Agent 永遠比較好」：作者引 Anthropic 自家文章指出，**若工作高度互相依賴、每一步都要共用完整脈絡，硬拆成多個 Agent 反而增加溝通成本**
- ⚠️ 不是每個聊天工具看到 `subagent_1` 就會真的派出子 Agent，必須使用本身支援 Subagent／多 Agent 的工具
- Context Rot 的說法作者也標明分寸：**不是「多放一個字就壞掉」**，而是每段不相關內容都可能搶走一點注意力

## R（三個機制）

### 1. 先認出 Context Pollution：資料不是垃圾，只是放錯桌

最生活化的版本——正在做「夜市簡報」，中途插入晚餐吃什麼、股市抄底、東京住哪、履歷潤稿，再說「好，回到夜市簡報」。每一題都合理，但這張工作桌現在同時放了菜單、股票、地圖與履歷。

實用習慣：

```
一個主要 Context ≈ 一條主要工作線
```

小岔題不用每次另開對話；但要開始一個**會持續討論的新專案**，開新 Chat 通常更乾淨。這不是 AI「失憶」，而是你替不同專案開不同資料夾。🔑 **硬碟很大不代表所有檔案都該放 Desktop；Context Window 很長，也不代表所有對話都該塞進同一串。**

Anthropic 的兩個名詞：**Context Rot**（內容變長時，模型找出並正確使用重要資訊的能力可能逐漸變差）與 **Context Pollution**（即使 Context Window 更大，相關性與干擾仍是問題）。

### 2. Subagent 的價值是 Context Isolation，不是「多一個 AI 比較快」

「叫 AI 上網查」特別容易弄亂主 Context：最後的行程可能只有 500 字，但過程中讀進來的網頁原文、選單、Cookie 提示、搜尋失敗紀錄可能非常多，而且大部分只是「為了查到答案而暫時看過」。

研究用 Subagent 可以在自己的工作桌上放滿搜尋結果、HTML、404 頁面與走錯路的紀錄，**回到主 Agent 時只帶回摘要、來源和風險**。

主 Agent 不需要知道研究員撞過幾次牆：

```
搜尋 1 → 找到舊公告      主 Agent 真正該收到的是：
搜尋 2 → 網頁 404    →   「官方公告是目前採用的來源；
搜尋 3 → 店家已歇業        舊公告已過期，歇業店家不納入行程。」
搜尋 4 → 官方 FAQ
搜尋 5 → 最新公告
```

> **過程中有用的資訊，不等於後續每一步都還有用。**

⚠️ 關鍵在於**不是把細節偷偷刪掉，而是把細節變成「可追查的來源」**——主 Agent 仍要知道結論從哪來、哪一點還不確定。原文示範的回報格式即「已確認事項 / 仍要確認 / 來源」三段。

**什麼時候不該派 Subagent**（Context Isolation 有交代成本）：

| 工作特性 | 較適合的做法 |
|---|---|
| 要查很多資料，最後只需要結論與來源 | 交給 Subagent |
| 會產生大量 Log、網頁或檔案內容 | 交給 Subagent |
| 必須一直參考剛才的決策、反覆討論 | 留在主 Agent |
| 改一個小地方，交代成本比工作還大 | 留在主 Agent |

收斂成一個問題：**這段工作產生的中間資訊，之後還需要一直被主線使用嗎？**

### 3. Compaction：好摘要不是短摘要，而是能讓下一位接手

即使沒岔題、沒亂讀網頁，長時間工作仍會慢慢塞滿。Compaction 就是把快接近上限的對話整理成一份**可接力的交班便條**。

```
保留：目標、已完成工作、重要決策、失敗嘗試、
      未解問題、下一步、關鍵檔案與證據位置
移除：重複對話、已不需要的原始 Tool Output
```

十次嘗試前九次失敗，糟糕的摘要是「最後方案成功」。好的摘要長這樣：

```
目標：讓會員能重設密碼。
已完成：寄信流程可正常發送。
已決定：重設連結有效期為 30 分鐘。
已排除：舊 API 不支援目前的驗證方式，勿再使用。
目前問題：手機版按鈕太窄。
下一步：修正版面後測試小螢幕。
```

🔑 **「已排除」那一行是精華**——下一個 Agent（或明天的你）不會把已證明行不通的方法再做一次。真正困難的是「留下什麼」，不是單純把字數砍半。

## C（關鍵行動）

- [ ] 派 Subagent 前先問：這段工作的中間資訊之後還要一直被主線用嗎？答案是「不用，給我結論與證據就好」才適合隔離
- [ ] 交代 Subagent 時明確寫出「回傳什麼、不要把搜尋過程全文貼回來」
- [ ] 寫交接摘要時檢查有沒有「已排除／已證明行不通」這一段

## 補充資料

### 拆題比派 Agent 更前面

常見誤會是「有 Agent 之後只要說『幫我弄好』」。Agent 可以協助拆解，但你仍要知道**最後要做哪個決定，以及決定需要哪些資訊**。原文的類比很準：像去醫院前知道要描述哪裡不舒服、何時開始、什麼情況更嚴重——醫師仍是專家，但你的描述決定檢查方向。

所以好的 Prompt 不是咒語，而是你對任務的理解被說清楚：

```
我想完成什麼？ → 要靠哪些資訊做決定？
→ 哪些部分可以各自獨立查？ → 每位 Subagent 應交回什麼？
```

不知道怎麼拆時，可以先請主 Agent：「我要規畫台南一日遊。**請先不要搜尋**，先告訴我需要確認哪些問題，以及哪些可以各自交給 Subagent。」

### Harness 是資訊總編輯

補完 Day 09 的 `Agent ≈ Model + Harness`：Harness 不只把資料送進模型，也負責決定哪些資料留在目前工作區、哪些工作交出去、哪些結果值得帶回來。

```
主 Agent 定義子任務 → Subagent 在獨立 Context 探索
→ 回傳 Summary / Evidence / Uncertainty → 主 Agent 只用必要內容繼續推理
```

🔑 **把同一份 Context 複製給十個 Agent，不會自動得到十倍智慧；有時只會得到十個一起找不到便當訂單的人。**

### Handoff 的資料是可以設計的

OpenAI Agents SDK 把交接做成 Handoff：**預設會把對話歷史交給下一個 Agent**，但開發者可以用 `inputFilter` 指定規則改變它實際看到的歷史。原文對初學者的收斂很好：暫時不用背這個設定，先記得「交接時要帶什麼資料」是可以設計的就夠了。

### 引用來源

Anthropic《Effective context engineering for AI agents》（Context Rot / Compaction 保留架構決策、未解錯誤與實作細節，捨棄重複訊息或較早的原始工具輸出）、Claude Code Subagents 官方文件（明列適用情境：旁支工作會淹沒主對話的搜尋結果／Log／檔案內容，且之後不需逐字引用）、Anthropic《How we built our multi-agent research system》、OpenAI Agents SDK Handoffs。

### 系列後續

Day 14 離開 Context Engineering，改看個人如何使用 OpenAI／Anthropic／Google／Ollama 等產品。

## 相關連結

- [[context-engineering-stacking]] — 同系列 Day 12，Context 五來源與工作桌模型（本篇的直接前作）
- [[subagent-four-boundaries]] — 🔴 Subagent ≠ Parallelism，四層邊界與 Result Contract（本篇的工程化深版）
- [[agent-context-management]] — Selection／Compaction／Eviction／Rehydration 四層機制
- [[claude-code-session-handoff]] — `/compact`／`/rewind`／`/resume` 與交接骨架
- [[agent-memory-lifecycle]] — Memory 與 Compaction 的界線
- [[claude-5-context-engineering-rules]] — 官方 context engineering 規則
- [[multi-agent-papers-three-elements]] — 多 Agent 的迴圈／提示／集約三要素

## 補充資料：手動觸發 Compaction 的可複製 prompt（2026-08-18 併入）

來源：https://tools.wingzero.tw/articles/4028 （Codex 5.6，但 prompt 與工具無關）

本條目講的 Compaction 多半是系統自動觸發；這一則補上**主動要求整理**的做法，以及它列的「該清什麼」清單。

### 🔑 該被清掉的七類內容

> **你可能會以為，Agent 對話得越久、累積的資訊越多，能力就會越強。但實際情況往往相反。**

- 已經放棄的方案
- 失敗的嘗試與測試
- 大量除錯 Log
- 重複產生的內容
- 已經完成、不再需要參考的資訊
- 與目前開發方向無關的歷史紀錄
- **已經被新決策取代的舊內容**

🔴 **它們不只消耗 Token，更麻煩的是會干擾 Agent 的判斷——讓它繼續參考過時資訊，甚至沿著早已放棄的方向繼續執行。**

（⚠️ 注意這與本條目 R 段第 3 點的「已排除」有張力：**失敗嘗試不能全清**——
「已證明行不通的方法」要留，「失敗過程的逐字 log」該清。**分界是「結論」與「過程」**）

### 可直接使用的 prompt

> 請整理目前任務的上下文，清理無效資訊，刪除或標記已廢棄方案、已完成且無需繼續參考的內容、
> 與目前目標無關的歷史紀錄，以及低價值的重複輸出。接著重新產生一份精簡的任務摘要。

🔑 原則一句話：**不要讓 Context 最後變成一個什麼都往裡面丟的垃圾桶。**（同本條目「資料不是垃圾，只是放錯桌」）

