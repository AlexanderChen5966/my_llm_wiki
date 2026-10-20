---
title: "告別隨意寫程式！Matt Pocock 推出 grill-me 技能，讓 AI 成為最嚴苛的架構審查官"
source: "https://cyberq.tw/2026/07/25/matt-pocock-grill-me-skill/"
author:
  - "[[BabyQ]]"
published: 2026-07-25 12:05:38Asia/Taipei
created: 2026-07-28
description: "在生成式 AI 工具大幅普及的當下，「Vibe Coding」（隨性寫程式）成為不少開發者的日常。許多人在概念..."
tags:
  - "clippings"
status: processed
---
在生成式 AI 工具大幅普及的當下，「Vibe Coding」（隨性寫程式）成為不少開發者的日常。許多人在概念尚未釐清時，便直接要求 AI 代理生成程式碼，隨後陷入不斷除錯與重構的泥淖。CyberQ 觀察，最近知名 TypeScript 專家與 AI 工程化推手 Matt Pocock 大神，在其開源專案 `mattpocock/skills` 中推出的 [`grill-me`](https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me) 技能，正是為了解決這種軟體開發中的根本難題，非常值得學習。

![](https://cyberq.tw/wp-content/uploads/2026/07/grill_me.jpg)

## 何謂 grill-me？專為 AI 代理設計的深度追問機制

`grill-me` 的核心理念非常簡單，在正式撰寫任何一行程式碼或產品需求文件（PRD）之前，讓 AI 轉型為一名極具批判性思考的系統架構師或產品經理。當開發者提出一個新功能構想時， `grill-me` 不會盲目奉承或直接輸出程式碼，而是會展開一場高強度的深度訪談。

這項技能會在對話中系統化地走過決策樹的每一個分支。它堅持每次只拋出一個關鍵問題，同時附上 AI 根據現有情境推導出的建議解答，並耐心地等待使用者給予明確反饋與決策後，才會推進至下一個關卡。這種逐步收斂邊界條件的設計，能確保雙方在進入實作階段前達成真正的共識。

## 自動探索程式碼庫，拒絕無效廢問

市面上許多提示詞技巧往往流於形式，甚至會向使用者詢問專案中早已存在的既有架構資訊。 `grill-me` 在設計上導入了明確的工具使用權限。若某個事實或脈絡可以透過檢視現有檔案系統、讀取配置檔或搜尋程式碼庫來取得，AI 代理必須自主完成調查，嚴禁向使用者提問已知資訊。

AI 只會針對需要商業邏輯選擇、架構權衡（Trade-offs）或技術選型的「決策點」進行提問。這種高度專業的互動方式，不僅節省了大量重複溝通的時間，更讓開發者能將精力集中在最核心的架構設計與商業考量上。

## 從源頭排除架構風險，建立嚴謹的工程文化

對於專業的資安工程師、系統架構師與開發團隊而言， `grill-me` 補足了當前 AI 協同開發中最為欠缺的一環，邊界條件壓力測試。在開發早期透過無情的問答釐清模糊地帶，能夠在程式碼尚未寫下前，就先一步發現潛在的安全漏洞、效能瓶頸與邏輯衝突。

Matt Pocock 提出的工程理念相當明確：真實的工程設計不該依賴直覺或運氣。透過 `grill-me` 技能，AI 不再只是幫你打字的工具，而是成為協助你嚴謹推演邏輯、確保系統穩健運作的最佳技術夥伴。

## 實戰教學：如何在開發工作流中導入並活用 grill-me

CyberQ 簡單帶領你一步步在常用的 AI 編輯器與代理環境（如 Cursor 或 Claude Code）中安裝並執行 `grill-me` 技能，打造流暢且嚴謹的架構設計流程。

## 第一階段：環境設定與技能安裝

在開始之前，請確保開發環境已經安裝 Node.js 16 以上版本，並且具備支援 Agent Skills 的 AI 編輯器環境。

請開啟終端機並切換至你的專案根目錄，執行下列 CLI 指令來下載並安裝技能：

Bash

```js
npx skills add https://github.com/mattpocock/skills --skill grill-me
```

指令執行完成後，系統會在專案目錄下的 `.cursor/skills/grill-me` 或對應的 Agent 設定檔中自動建置好相關規則與政策設定。重啟編輯器的 AI Agent 介面後即可完成啟用。

## 第二階段：觸發與實戰對話

當你準備規劃一項新功能、設計 API 介面或準備撰寫規格書時，可以透過指令面板或對話框直接輸入語法：

Plaintext

```js
/grill-me 我打算在專案中新增一個支援多租戶的 RBAC 權限管理系統，請開始追問我。
```

當 AI 接收到指令後，會自動啟動深度訪談流程，其典型對話模式如下：

> **AI Agent** ：在檢查現有的 `schema.prisma` 與認證機制後，我注意到目前專案採用 JWT 進行驗證。關於多租戶權限，我的第一個問題是：租戶之間的資料隔離層級為何？
> 
> **建議選項** ：採用 Row-Level Security (RLS) 結合 `tenant_o_id` 欄位進行邏輯隔離，此方案對於現有資料庫結構變更最小。
> 
> 請確認是否採用此方案，或給予其他指導？

此時你可以直接回答「同意採用建議選項」，或者輸入具體的微調指令（例如「改用獨立 Schema 隔離」）。AI 在收到你的確認前，絕對不會一口氣丟出多個問題，也不會冒然建立檔案。

## 第三階段：整合工作流與架構升級

在與 `grill-me` 順利釐清所有決策點並達成共識後，你可以將這份收斂後的資訊作為後續工作流的基礎。

Matt Pocock 推薦的完整 AI 工程流程為：先透過 `domain-model` 或 `grill-me` 對概念進行壓力測試；接著搭配 `to-prd` 技能將共識自動轉化為標準的產品需求文件；隨後利用 `to-issues` 拆解為可執行的任務清單；最後導入測試驅動開發（TDD）進行程式碼撰寫。

透過這種層層遞進的自動化鏈結，你能確保寫出的每一行程式碼都有嚴密的架構支撐，大幅提升專案的維護性與整體系統品質。

## 規格書審查的防線：grill-me-doc 技能

除了著重於概念與架構審問的 `grill-me` 之外，這個技能庫還延伸出了專門針對既有規格文件的 `grill-me-doc` 技能。當團隊已經撰寫好初步的產品需求文件（PRD）或架構設計草案時，只要啟動 `grill-me-doc` 並指向該份文件，AI 代理便會化身為極其嚴苛的審查委員。它會自動掃描檔案內容，針對文字中的模糊條款、未定義的邊界條件與潛在邏輯矛盾進行抽絲剝繭的提問，確保文件在交付開發前就經過充分的壓力測試。

## 無縫銜接實作技能：打造完整的 AI 開發閉環

在透過兩大審問技能徹底收斂架構與規格後，專案能接著搭配實作導向的技能進行自動化接軌。藉由結合任務拆解與程式碼生成的實作技能，AI 能將審核無誤的文件直接轉換為具體的執行計畫、微服務架構與單元測試。

這種從概念審問、規格驗證到最終程式碼產出的自動化閉環，讓開發者既能保有資安與架構層面的嚴謹度，又能享受 AI 輔助帶來的倍數執行效率。

## CyberQ 觀點：為何 grill-me 組合拳優於重量級的 Superpowers 技能？

隨著 GPT-5.6 (Sol) 與 Claude Fable 5 等新一代高推理大語言模型的問世，AI 在理解程式碼庫脈絡與邏輯推演的能力上已取得突破性進展。相較於 Superpowers 這類採用「全自動強制約束」機制、會在每一次對話中硬性綁定複雜流程與繁複產出物的重量級技能框架，Matt Pocock 的 `grill-me` 採用了極簡且可自由組合的「手術刀」策略。

由於 Sol 5.6 與 Fable 5 本身就具備強大的自主探索與推理底子，過度嚴苛與重型的引導框架反而會造成昂貴的 Token 消耗、拉長回應延遲，甚至衍生出無謂的對話迴圈；而 `grill-me` 透過一次僅拋出一個關鍵決策與自動給出建議選項的設計，能恰到好處地引導高智慧模型聚焦於架構權衡，實現極高效率的人機協同。

CyberQ 認為，Matt Pocock 所主張的「單點精準擊破、彈性鏈結下游」開發閉環，讓開發者能完全掌握決策節奏。在搭配頂級推理模型時，開發者只需在關鍵的架構歧異點啟動 `grill-me` 或 `grill-with-docs` 進行深度交鋒，隨後便能將收斂好的共識無縫交給下游技能，並由模型發揮極致的垂直切片與自動化執行能力。相較於 Superpowers 容易將簡化任務過度複雜化， `grill-me` 的輕量化設計賦予高靈活性，更真正釋放了新一代高推理模型在軟體工程領域的龐大潛能。

<iframe sandbox="allow-scripts" title="Claude Opus 5 登場：半價逼近尖端模型，基準測試與 API 新機制解析 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/25/claude-opus-5-debuts-13483/embed/#?secret=5tt0V8EC6f#?secret=Xa2lAW15jf" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="OpenAI 模型突破資安測試環境自主入侵 Hugging Face — CyberQ 賽博客" src="https://cyberq.tw/2026/07/23/openai-model-breaks-13445/embed/#?secret=gA4f2SWmOJ#?secret=zjNtzIKx1f" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="Linux 之父擁抱 AI 新趨勢，Linus Torvalds 透過 Vibe Coding 開發專案 — CyberQ 賽博客" src="https://cyberq.tw/2026/01/15/linus-torvalds-vibe-coding-python/embed/#?secret=W8GVk0POoq#?secret=LfjDDahYfu" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="餐廳服務生也能幫忙 Debug？ Vibecoding 與工業化浪潮席捲矽谷 — CyberQ 賽博客" src="https://cyberq.tw/2026/01/06/ai-coding-productivity-revolution/embed/#?secret=tH0xfzUuIA#?secret=xoz4dyozD7" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="效益與速度的平衡：Google 正式推出 Gemini 3.6 Flash 與實測剖析 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/22/the-ultimate-balance-13436/embed/#?secret=ymfRdGQtjL#?secret=ZsJJXMgdcP" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="效益與速度的平衡：Google 正式推出 Gemini 3.6 Flash 與實測剖析 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/22/the-ultimate-balance-13436/embed/#?secret=ymfRdGQtjL#?secret=ZsJJXMgdcP" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="Claude Fable 5 訂閱新制正式上路：Max 內含 50% 額度，Pro 改點數制並獲 100 美元補貼 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/18/claude-fable-539s-new-subscription/embed/#?secret=QtFE7RgP8b#?secret=e3gzP8Kc3X" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="Thinking Machines 開源 Inkling：975B 多模態模型測試成績與企業部署解析 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/17/thinking-machines-open-source-inkling/embed/#?secret=hDLgtLAzys#?secret=YY0jIHHtMl" width="500" height="335" frameborder="0"></iframe>