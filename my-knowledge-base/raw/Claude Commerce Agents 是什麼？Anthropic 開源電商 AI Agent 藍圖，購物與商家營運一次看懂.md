---
title: "Claude Commerce Agents 是什麼？Anthropic 開源電商 AI Agent 藍圖，購物與商家營運一次看懂"
source: "https://www.aiposthub.com/claude-commerce-agents-shopping-merchant-ai/"
author:
  - "[[Philo]]"
published: 2026-09-03
created: 2026-09-03
description: "Anthropic 開源 Claude Commerce Agents，提供購物 Agent、商家 Agent、四種產業範例與安全機制。本文整理功能、架構、導入方式與限制。"
tags:
  - "clippings"
status: processed
---
Anthropic 在 2026 年 9 月 2 日開源 [Claude Commerce Agents](https://github.com/anthropics/commerce-agents?ref=aiposthub.com) ，希望讓企業更快做出能協助購物與商家營運的 AI Agent。

它不是一套註冊後就能直接使用的電商系統，也不會替商家處理付款、物流或庫存。比較準確的理解是：Anthropic 把自己與電商團隊合作時累積的架構、安全限制與測試方式，整理成一套可以自行修改的參考程式碼。

這次開源真正有價值的地方，也不是「Claude 會推薦商品」這件事。現在很多模型都能做商品問答，困難的是如何讓 Agent 只引用真實價格、不能擅自扣款、修改促銷前要取得人工批准，而且換模型或改 Prompt 後仍然不會破壞原本流程。Claude Commerce Agents 把這些容易被忽略的產品問題，直接做進程式架構裡。

ClaudeDevs 以官方影片展示 Shopping Agent、Merchant Agent 與開源專案。影片來源： [ClaudeDevs 官方 X 貼文](https://x.com/ClaudeDevs/status/2095233745167282602?ref=aiposthub.com) 。

## Claude Commerce Agents 是什麼？

Claude Commerce Agents 是 Anthropic 公開的電商 AI Agent 參考藍圖。所謂 AI Agent，是能理解目標、呼叫外部工具並依結果繼續處理任務的 AI 系統，不只是回答一段文字。

整套藍圖分成兩個角色：

| Agent 類型 | 服務對象 | 主要工作 |
| --- | --- | --- |
| Shopping Agent | 消費者 | 找商品、比較選項、規劃購物清單、加入購物車、查訂單與退貨政策 |
| Merchant Agent | 商家內部人員 | 分析銷售、檢查庫存、整理商品、建議價格與促銷、草擬行銷活動 |

Anthropic 也提供零售、旅遊、電信與娛樂票務四種可執行的示範。開發團隊可以先看完整流程，再把假資料與示範後端換成自己的商品目錄、訂單、會員、庫存及行銷系統。

專案採用 [Apache License 2.0](https://github.com/anthropics/commerce-agents/blob/main/LICENSE?ref=aiposthub.com) ，可依授權條款修改與散布。不過，官方也明確說明這是參考實作，不是 Anthropic 維護的正式產品，沒有服務水準保證，也不接受外部貢獻。換句話說，企業可以少走一段從零搭架構的路，但後續維護責任仍在自己身上。

## Shopping Agent 如何改變消費者的購物流程？

傳統電商搜尋通常要求消費者先知道關鍵字，再自行打開多個商品頁比較。Shopping Agent 想處理的是更接近真實需求的說法，例如：「我要帶兩個小孩露營兩天，請幫我準備帳篷、睡袋和爐具。」

Agent 會把這個目標拆成多項商品需求，讀取商家的目錄與庫存，整理比較結果，再把合適商品放進購物車。它也能記住使用者主動提供的尺寸、品牌或取貨偏好，並在同一段對話中回答訂單、換貨與退款問題。

這跟一般聊天機器人最大的差別，在於回答後面接著實際的電商流程。Claude 會透過工具讀取商家原本的搜尋、排序、購物車與訂單系統，不能只憑印象寫出幾個商品名稱。商品價格、庫存和政策仍由企業既有系統提供，模型負責理解需求與決定如何呈現。

介面也不必只剩一大段對話文字。藍圖把商品卡、比較表、購物車、行程與座位圖視為可以呼叫的 UI 工具。Agent 輸出結構化資料後，再由前端畫面呈現。這種做法比要求模型產生自訂 HTML 標籤更容易驗證，也能保留畫面順序，讓使用者接著說「我要左邊第一個」時，系統知道他指的是哪個項目。

![Claude Commerce Agents 零售 Shopping Agent 官方介面，依預算推薦玩具並加入購物車](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/claude-commerce-shopping-agent-official.webp)

Shopping Agent 可依自然語言需求搜尋與比較商品，再把使用者選擇交給購物車與結帳流程。示範中的 ACME 與商品皆為虛構資料。 圖片來源： Anthropic 官方產品公告 。

## Merchant Agent 如何協助商家營運？

Merchant Agent 面向的是店主、行銷人員與營運團隊。使用者可以直接詢問「哪些商品賣得不好？」、「這波促銷開始前，哪些品項可能缺貨？」或「如果降價 15%，庫存是否足夠？」

Agent 會從銷售、商品、庫存、定價與行銷系統取得資料，整理成圖表或建議。它也能草擬補貨、調價、商品內容與行銷活動，但預設只會建立「待執行變更」，不會直接讓內容上線。

這項限制很重要。商家需要的是能提早找出問題、提出完整方案，最後把決定權留給人的營運助手，不是什麼都敢做的 AI。尤其調價、退款或廣告預算都會直接影響收入，只靠一句 Prompt 要求模型「小心操作」並不夠。

![Claude Commerce Agents Merchant Agent 官方介面，顯示銷售、庫存與待批准變更](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/claude-commerce-merchant-agent-official.png)

Merchant Agent 可整理銷售與庫存問題，但調價、補貨等修改預設先建立草稿，由操作人員批准。 圖片來源： Anthropic 官方產品公告 。

## Claude Commerce Agents 的安全設計有什麼不同？

Claude Commerce Agents 最值得產品與工程團隊研究的部分，是它把安全規則放進程式。Prompt 可以提醒模型該怎麼做，程式則負責攔住不該發生的操作。

首先，Shopping Agent 沒有直接扣款的方法。結帳工具只負責顯示購物車，再把使用者交給商家自己的結帳頁面。付款網址由後端加入，模型本身不會讀到網址，也不會接觸付款憑證。

其次，Merchant Agent 的修改會先進入暫存狀態。調價、補貨或啟動行銷活動前，必須由操作人員在真正的批准介面確認。即使有人在對話中輸入「我已經批准」，也不等於系統收到有效授權。

第三，系統只接受伺服器先前提供過的商品或變更 ID。ID 是系統辨認商品與操作對象的編號。如果模型憑空編出一個 ID，或第三方評論故意塞入惡意指令，後端會在執行前拒絕。商品數量、折扣幅度、調價幅度與活動預算也能設定硬性上限，並在真正套用變更時再次檢查。

這套設計傳達了一個實用原則：模型可以提出建議，但涉及金錢或不可逆操作時，最終權限應該掌握在程式規則、使用者身分與批准流程手上。

## 為什麼 Anthropic 建議用單一 Agent 搭配 Skills？

面對零售、客服、庫存與行銷等不同任務，很多團隊的直覺是各做一個 Subagent，再由主 Agent 分派工作。Subagent 是專門處理某一類任務的子代理人。

Anthropic 的工程文章提出不同看法。一次購物對話常同時需要商品、購物車、訂單與偏好資料，頻繁把任務交給不同 Subagent，容易遺失上下文，也會增加 Token 成本與等待時間。因此，這套藍圖主要使用單一 Agent 保留完整對話，再透過 Skills 載入不同工作流程。

Skills 可以理解成可重複使用的任務說明書。常用且重要的規則放在系統 Prompt，較少出現的客服、採購研究或促銷流程再按需要載入 Skill。只有研究範圍明確、可以獨立完成的任務，才交給 Subagent 處理。

這不是所有產品都必須照抄的唯一答案，但它提供了很實際的 MVP 起點：先用一個 Agent 跑通完整流程，等測試證明特定任務真的需要獨立上下文，再拆成更多 Agent。這比一開始就建立複雜的多代理人架構，更容易維護與找出錯誤。

![Anthropic Commerce Agent 工程架構官方主視覺，以手托住打開的說明書](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/claude-commerce-agents-anatomy-official.jpg)

Anthropic 建議以單一 Agent 保留完整對話，再透過 Skills 與 Tools 連接企業既有系統。 圖片來源： Anthropic 官方工程文章 。

## 開發團隊可以怎麼開始？

這套專案支援三種執行方式。API 是讓不同軟體交換資料與指令的介面，Messages API 適合想自行控制 Agent 迴圈的團隊。Claude Agent SDK 提供較完整的 Agent 開發框架，Claude Managed Agents 則把部分執行環境交給 Anthropic 管理。團隊也能使用專案內附的 Claude Code plugin，依自己的技術架構建立初始專案、增加流程、產生評測案例或檢查既有 Agent。

若要先執行零售示範，官方 README 提供以下流程：

```bash
git clone https://github.com/anthropics/commerce-agents.git
cd commerce-agents
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
cd examples && npm ci && cd ..
python scripts/run_demo.py retail
```

環境需要 Python 3.11 以上、Node.js 22，以及 Anthropic API key。API key 應填入本機 `.env` ，不能提交到公開的 Git 儲存庫。

但如果目標是正式導入，我不建議一開始就串接付款、退款與自動調價。比較實際的 MVP 做法，是先讓 Shopping Agent 只做商品搜尋、商品比較與客服問答；Merchant Agent 則先提供唯讀的銷售與庫存分析。確認答案有來源、權限隔離正確，而且評測穩定後，再加入購物車與需要人工批准的變更流程。

## Claude Commerce Agents 有哪些限制？

這份藍圖縮短的是 Agent 架構與介面設計的時間，不會替企業完成最耗工的系統整合。

示範環境沒有正式的身分驗證，內附的 MCP server 也只預設綁定本機。MCP 是讓 AI 連接外部資料與工具的標準介面。企業仍要自己處理會員與員工權限、API 憑證、流量限制、稽核紀錄、個資保存政策，以及各地法規。

付款也完全不在專案範圍內。商品能不能買、優惠是否適用、庫存能不能調整，仍然要由商家自己的後端規則判斷。官方示範中的公司、商品與人物都是虛構資料，不能把畫面中的成功流程當成已經接上真實商店。

官方提到，使用 Claude 購物 Agent 的零售商曾觀察到購物車金額最高增加 35%，完成購買的機率提高 60%。但 Anthropic 沒有在公告中公開完整樣本、產業分布與實驗設計，這些數字也不是使用開源藍圖後保證能得到的結果。企業評估成效時，仍應用自己的轉換率、任務完成率、客單價、錯誤率與人工接手率做 A/B test。

更直接地說，這不是小型店家下載後隔天就能上線的外掛。它比較適合已經有商品、庫存、訂單與權限系統，而且能投入工程資源的企業團隊。

## Claude Commerce Agents 值得導入嗎？

我的結論是：值得拿來做原型與架構參考，但不應把它當成可以直接上線的完整產品。

對已有電商後端的產品與工程團隊來說，它最大的價值是把許多隱性的 PRD 變成可以檢查的程式規則。價格必須來自商品資料、付款不能由模型執行、商家修改必須批准、第三方文字不得變成操作指令。Demo 畫面是否順暢只是表面，這些規則才決定產品能不能安全營運。

它的最大風險則是「參考程式碼看起來很完整」，讓團隊低估後端整合、權限、測試與法規成本。Anthropic 已表明不維護這個專案，也沒有服務水準保證。只要企業缺少清楚的資料來源、權限模型與測試案例，換成再強的 Claude 模型也無法補上這些缺口。

因此，我會先把它當作一套電商 Agent 的可執行規格書。先選一個高頻、低風險流程做 MVP，累積真實對話與錯誤案例，再決定是否擴大到購物車、促銷與跨系統操作。當唯讀流程的正確率、速度與使用率都站得住腳，這份藍圖才有機會從展示專案變成商業產品。

## Claude Commerce Agents 常見問題

### Claude Commerce Agents 可以直接接到 Shopify 或其他電商平台嗎？

它沒有內建可直接使用的正式連接器。開發團隊要透過 `StorefrontBackend` 或 `MerchantBackend` ，把自己的商品、購物車、訂單、庫存與行銷系統映射到 Agent 能讀取的格式。官方頁面提到合作夥伴正在建立相關實作，但不代表原始專案已經能免開發接上所有平台。

### Claude Commerce Agents 會自動幫消費者付款嗎？

不會。參考架構沒有下單或扣款方法，只會整理購物車並把使用者交給商家自己的結帳頁面。這是刻意設計的安全邊界。

### 商家 Agent 可以自己修改價格嗎？

預設不行。Agent 可以提出調價並建立待批准變更，但必須由有權限的人在真正的操作介面確認，系統才會套用。企業若自行關閉這項限制，就必須承擔額外風險並重新設計測試與權限政策。

### 不使用 Claude Managed Agents，也能採用這套藍圖嗎？

可以。官方參考實作也支援 Messages API 與 Claude Agent SDK，並說明可部署在 Amazon Bedrock、Google Cloud Vertex AI 與 Microsoft Foundry 等環境。選擇方式應以現有雲端架構、權限需求、成本與團隊維護能力為主。

### Claude Commerce Agents 適合沒有工程團隊的小商家嗎？

目前不太適合。它是開發藍圖，不是低程式碼商店外掛。沒有工程資源的商家，更適合先使用既有電商平台提供的 AI 搜尋、客服或行銷功能，再觀察是否真的需要自建 Agent。

## 結語：會推薦商品只是起點，守住操作權限才是關鍵

Claude Commerce Agents 把購物、客服、分析與商家營運放進同一套 Agent 架構。這次開源最值得注意的重點，在於它如何畫出權限邊界，而非列出多少功能。

模型負責理解需求與提出方案，企業原有系統繼續掌握價格、庫存、身分與付款。高風險變更先暫存，再由人或明確政策批准。對準備做電商 AI Agent 的團隊來說，這是一個務實的起點，也提醒大家：做出像真人的回答相對容易，讓每次回答與操作都能追溯、測試、拒絕和復原，才是產品真正的難題。