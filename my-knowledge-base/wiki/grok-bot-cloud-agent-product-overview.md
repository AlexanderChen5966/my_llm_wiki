---
title: Grok Bot 產品介紹與功能：雲端電腦、Outer/Inner Loop 與治理機制
date: 2026-09-17
tags: [ai/agent, ai/workflow, type/concept]
source: https://www.aiposthub.com/grok-bot-guides-deepdive-ai-agent-fleet-cloud-computer/, https://www.darrelltw.com/grok-bot-review/, https://www.aiposthub.com/grok-bot-personal-agent-workflow-security/, https://www.bnext.com.tw/article/91991/grok-bot-getting-started-60, https://rar.design/posts/grok-bot-agent-interface-design, https://www.aiposthub.com/grok-bot-customer-support-case-study/
type: article
status: processed
asset_value: 64
---

SpaceXAI（xAI／Cursor 合併後團隊）2026 年 9 月正式發布 Grok Bot 官方 9 大指南全集。核心賣點是「每個 Bot 配一台 24 小時不關機的雲端電腦」，終結「對話框奴役」——不必每次重新交代脈絡、不必守著螢幕盯進度。本篇整合官方指南深度拆解（09-17）、個人實測（08-23）與早期安全／工作流報導（08-12）三來源。

## T（核心論點）

> Grok Bot 的本質不是「更聰明的聊天機器人」，而是給 AI 一台**持久存在的雲端電腦**（桌面、終端機、瀏覽器、已登入 Session），讓它能跨對話、跨設備、跨時間接力工作；真正的工程突破在於用 **Outer Loop（治理／脈絡整理）與 Inner Loop（沙盒執行）雙層解耦**，解決「AI 在髒脈絡裡迷失」的問題，並用專職治理 Agent（Jenny）取代人類的持續微觀管理。

## B（適用場景）

**適用：**
- 每週存在大量固定、跨工具、規則明確的重複流程（工單分群、夜間代碼審查、比價、後台巡檢）
- 團隊已具備健全 CI/CD、自動化測試與版本控制，能承接「單人調度上百代理」的規模
- 需要離開電腦仍能推進的任務（筆電關機／休眠不中斷）、或需要無 API 網站的操作能力

**不適用 / 注意：**
- 任務高度隨機、驗收標準講不清楚時，雙層架構反而增加協調成本
- 專案沒有自動化測試、依賴手動 FTP／無版本控制，直接導入代理艦隊只會加速產生「代碼垃圾」
- 目前仍是 Early Beta，同帳號下所有具名 Bot **共用同一台雲端電腦、檔案系統與瀏覽器 Session**——不可把「不同 Bot 名稱」當作安全沙盒
- 瀏覽器型 Agent 的 prompt injection 風險目前無人能完全免疫（NIST、Anthropic 研究皆指出）

## R（核心架構與操作模式）

1. **持久雲端電腦**：圖形化桌面＋終端機＋常駐登入瀏覽器；持久會話（登入態跨任務保留）、跨端無縫接力（手機下指令、桌面接手）、安全交接機制（CAPTCHA/2FA/付款時把控制權交回人類）。
2. **Bot 設定三要素**：Name／Title／Description，權限邊界用**純自然語言**寫在 Description，由獨立 Reviewer Agent 依 Allow/Block 清單裁決自主執行或呼叫人工核准。
3. **Outer Loop vs Inner Loop 雙層解耦**：Outer Loop（Grok Bot 本身，扮演技術主管，跨 Slack/Notion/GitHub 梳理髒脈絡，自己不寫代碼，只產出含驗收標準的乾淨 Prompt）→ Inner Loop（Cursor Cloud Agent，在獨立沙盒中專注下載依賴、改檔、編譯、測試）。此架構讓內層執行者不被雜訊干擾，大幅提升一次通關率。
4. **治理主管 Agent「Jenny」**（唯一不寫代碼）：每天清晨 5 點與各工程 Bot 進行 1:1 晨會複習 Playbook、對焦卡點；出錯後主持 Postmortem，更新團隊規範並廣播全體 Bot；凌晨 3 點夜間巡檢（死碼清理、i18n 審計、「今晚隨便你想做什麼」自由探索）；P0 緊急流程每 5 分鐘輪詢 Agent 軌跡（極燒 Token，官方特別警示勿濫用）。
5. **三層記憶架構**：使用者記憶（姓名/時區/偏好，跨 Bot 共用）／Agent 記憶（各 Bot 自己的角色與互動紀錄）／專案記憶（團隊決策與慣例，不綁定單一 Bot 或成員）——把「你是誰」「這個 Bot 做什麼」「這件事怎麼做」分開維護，避免規則互相干擾。
6. **知識分享「食譜模式」（Templates）**：分享的是指令／技能／排程／外掛的**製作藍圖**，而非整台電腦複製；安裝時自動剔除私密記憶、機密腳本、帳密，接收者需人工確認權限清單才會實例化。
7. **費用方案**：免費 Trial（需綁卡，用量極小）／Pro+ $60 美金/月／隨 SuperGrok Heavy($300)或 Plus($100) 訂閱附贈。目前僅 macOS，Beta 限特定訂閱戶開放。（⚠️ 2026-09-23：已過時，見補充資料）
8. **與 Hermes Agent／OpenClaw（龍蝦）的定位對比**：可類比「拎包入住精裝公寓」vs「自建透天」——Grok Bot 架設門檻極低但模型鎖定 Grok、無法換模型；Hermes/OpenClaw 需自建 VPS/Mac mini，但模型自由度 100%、生態成熟。適合對象：不想碰 Linux、想把日常瑣事外包者選 Grok Bot；想自己掌控代碼與模型選擇的進階開發者選 Hermes/OpenClaw。

## C（關鍵行動 / 落地路線圖）

- [ ] 第一階段：挑一個「單一、唯讀、可挽回」的任務開始（如整理書籤、比價、僅產出建議草稿不自動發送）
- [ ] 第二階段：任務成功跑通三次以上，固化為 Skill 並設定 Routine；借鏡「Jenny 模式」每週檢查出錯記錄、修訂自然語言 Playbook
- [ ] 第三階段：工程或核心業務導入 Outer/Inner Loop 雙層解耦，脈絡蒐集者與執行者分離
- [ ] 涉及付款、刪除、大量發送、正式環境發布的動作，一律在自然語言規則中鎖死「人工審批」
- [ ] 多 Bot 分工時，切勿把「不同 Bot 名稱」當作安全沙盒——高風險登入態視同全帳號共享
- [ ] 共用電腦上的登入態與檔案要手動清，刪 Bot 不等於清除（2026-09-29 補）

## 原始摘要

三篇文章時間遞進（08-12 → 08-23 → 09-17），內容互補而非矛盾：
- **09-17（主來源，aiposthub 官方 9 大 Guides 拆解）**：最完整最深，涵蓋 Outer/Inner Loop 架構、Jenny 治理、5 位專職工程 Bot、30 分鐘 Notion PR 自動化閉環、客服/PM/GTM/設計四職能落地案例、Templates 食譜模式，以及「同帳號 Bot 共用一台電腦」關鍵安全警告與三階段落地路線圖。內部數據：單月合併 2,000+ PRs，單人從管理 15 個代理躍升至同時調度 200+ Cloud Agents。
- **08-23（darrelltw 實測）**：個人使用者視角，涵蓋安裝流程、三種取得方案費用、示範一次存成技能、Bot 互相交辦、排程功能，以及與 Hermes Agent/OpenClaw 的架構自由度對比（「拎包入住 vs 自建透天」）。
- **08-12（aiposthub 早期報導，基於 Matt Palmer〈Intro to Grok Bot〉）**：三層記憶架構詳細說明、人機交接機制（登入/2FA/CAPTCHA/付款時交還控制權）、prompt injection 風險學術引用（NIST、Anthropic 研究均指出沒有瀏覽器 Agent 能完全免疫）、五點安全實務建議、Matt Palmer 實際案例（Demo Bot、Content Bot、採購 Bot、DoorDash Bot）。

## 補充視角：五款競品定位對比表＋四週試用節奏（2026-09-17 併入）

來源：〈Grok Bot 是什麼？SpaceXAI 那個會自己用電腦的 AI 同事〉（https://seo.whoops.com.tw/grok-bot/，published 2026-08-12）。本篇深度不及既有主來源（未提 Outer/Inner Loop、治理 Agent Jenny、三層記憶架構細節），僅取兩項獨有增量。

**① 五款「會用電腦的 AI」高階定位對比表**（既有條目僅比較 Hermes Agent／OpenClaw，此為新增比較維度）：

| 產品 | 高階定位 | 你該先問自己的問題 |
| --- | --- | --- |
| **Grok Bot** | AI 同事路線，主打一直都在的記憶、多個 Bot 並行、互相交班 | 你想用傳訊息的方式，把跨工具的流程交出去嗎？ |
| **Claude Computer Use** | API、開發者導向 | 你是想把「操作電腦」這個能力，嵌進你自己做的產品裡嗎？ |
| **OpenAI Operator** | 任務導向 | 你要的是做完一件明確的事，還是要一個長期分工？ |
| **Devin** | 軟體工程專用 | 你的工作是不是幾乎都在寫程式的流程裡？ |
| **Manus** | 通用型的 agent | 你是不是更需要一個什麼都能做的通用代理？ |

判斷時可帶三個問題重新對照官方說明：結果會放在哪裡（停在聊天視窗還是直接進到你在用的系統）、出錯時誰接手（能否中途插手/接續，還是只能重來）、你的東西留在誰那邊（登入/偏好/routine 存在誰的環境）。

**② 四週試用節奏建議＋六欄追蹤表**（較既有條目的三階段路線圖更具體可操作）：第一週只跑一件事，每天看結果；第二週修正流程、開始排時間自動跑；第三、四週加入第二個 Bot，觀察交班順不順、授權要不要調。試用期間建議記錄六欄：

| 欄位 | 記錄內容 |
| --- | --- |
| 任務 | 這次交代了什麼、預期結果是什麼 |
| 結果 | 做完了、做一半、卡住、還是重來 |
| 人工時間 | 教它、檢查它、收尾各花多久 |
| 出錯 | 填錯欄位、漏掉、重複、還是超出範圍 |
| 授權 | 這次實際碰到了哪些工具跟資料 |
| 批准 | 哪一步回來找你點頭、你的決定是什麼 |

⚠️ **定價資料衝突（發布時間相近但描述矛盾，中性並列，不擇一）**：本篇稱開放對象與定價為 SuperGrok Heavy／Cursor Ultra（每月 US$200）／Cursor Premium Teams（每席每月 US$120）；既有條目（主來源同為 2026-08-12 發布之一）稱免費 Trial／Pro+ $60 美金/月／隨 SuperGrok Heavy($300) 或 Plus($100) 訂閱附贈。兩者發布時間都是 2026-08-12，屬同期發布卻定價描述矛盾——**未查證何者為誤植或版本差異，讀者自行查證官網現況為準**。

## 補充資料：定價衝突解析、平台更新與幕僚長模式（2026-09-23 併入）

來源：〈Grok Bot是什麼？怎麼用？Grok Bot教學：4步驟建好「AI代理一人公司」〉（https://www.bnext.com.tw/article/91991/grok-bot-getting-started-60，《數位時代》，2026-09-18 更新版，原文首發約 2026-08）。本文屬 AI 初稿，內含錯字（「Claude.me／Agent.me」應為 CLAUDE.md／AGENTS.md），本段不採用。

**① 🔎 解析：上方「定價資料衝突」的時間線**（上方中性並列段原文保留不動，以下為新增解析）

- 本文指出：2026-08-21 之前，Grok Bot 主要綁在較高價方案；SpaceXAI 當天宣布下放到更多方案（文中附官方公告連結 x.ai/news/grok-bot-more-plans），納入 SuperGrok Plus、Cursor Pro+，以及 Cursor Teams Standard／Premium
- 三個來源的日期與這條時間線吻合：whoops（08-12，下放前）只列 SuperGrok Heavy／Cursor Ultra $200／Cursor Premium Teams $120；darrelltw（08-23，下放後）列 Pro+ $60 與 SuperGrok Plus／Heavy
- 🔴 **更正原並列紀錄的前提**：原註記寫兩組定價「發布時間都是 2026-08-12」，這個前提不成立。Pro+ $60／Plus $100／Heavy $300 那一組出自 darrelltw 08-23，不是 08-12 的來源
- 下放後完整價目表（本文）：

| 方案 | 月費（美元） |
|---|---|
| Cursor Teams Standard | 40／座位 |
| Cursor Pro+ | 60（個人最低） |
| SuperGrok Plus | 100 |
| Cursor Teams Premium | 約 120／座位 |
| Cursor Ultra | 200 |
| SuperGrok Heavy | 約 300 |

Free Trial 需綁信用卡，token 額度極少，只夠看懂介面。

⚠️ 2026-09-29 附註：rar.design（09-24）另列 Cursor Pro $20／Pro+ $60／Ultra $200／Teams 依座位，最低門檻與上表不同，見下方「方案下放、平台與風險更新」一節的並列說明。

- **結論**：兩組價格是時間先後（下放前／後）的差異，不是同期矛盾。⚠️ 但這個解析的依據是媒體轉述官方公告，本庫**未直接讀過 x.ai 公告原文**，價格仍以官網為準

**② 平台支援更新**（R7「目前僅 macOS」已過時）

本文 FAQ（2026-09-18 版）寫：支援 macOS、Windows 桌面與 iPhone（需 iOS 18 以上），尚未支援 Linux 桌面、Android、iPad。但本文**自相矛盾**：重點段說 08-11 初發時支援「macOS 桌面版與 iOS」，第 1 步與 FAQ 才加上 Windows，所以原文沒寫 Windows 何時上線。另外，08-12 的 whoops 已寫「電腦版跟 iOS 手機版能用」，可見「僅 macOS」原本就只是指桌面版。

📎 2026-09-29 附註：rar.design（09-24）寫桌面 macOS／Windows／**Linux**、行動 iPhone／**iPad**／**Android**，台灣 App Store 已上架。判為 09-18 之後的更新（時間差），不視為矛盾。

**③ 幕僚長模式（多 Bot 編制）**

你只跟幕僚長講話，由它讀每個專責 Bot 的職責描述，判斷該給誰、轉派出去，再把結果帶回來。交辦時給「目標」而非任務清單（清單等於你已經先拆好工作了），也可以在對話框打「@」直接點名專責 Bot。Bot 要用真人職位命名，因為名稱就是幕僚長轉派的依據。六個起手角色：幕僚長（只有衝突、對外訊息與花錢才找你）／研究員／內容（只備稿不發布）／工程（交付附測試結果）／客戶（信件一律停在草稿）／財務（寫入財務系統前要你核准）。幕僚長在架構上等於 coordinator，見 [[agent-router-vs-coordinator-state-tracking]]。

**④ 時間線**：SpaceX 與 Cursor 2026-06-16 簽合併協議、08-14 完成交割，Cursor 成為 SpaceX 全資子公司。Grok Bot 與 Cursor 使用同一組帳號登入，要用**持有方案與用量的那組 Cursor 帳號**，否則看不到付費額度。

職務章程三要素（職責範圍／執行標準／不能做的事）與「先做對一次 → 固化 Skill → 才排程」的完整操作，見 [[grok-bot-routine-automation-ladder]]。

## 補充資料：方案下放、平台與風險更新＋客服案例與數字讀法（2026-09-29 併入）

來源：〈AI Agent 的介面為什麼像通訊軟體？拆解 Grok Bot 的 Bot 名單、頭像狀態與雲端電腦〉（設計師 Riven，https://rar.design/posts/grok-bot-agent-interface-design，2026-09-24；同篇的設計文拆解另立 [[grok-bot-persistent-agent-interface-design]]）；〈Grok Bot 客服案例解析：SpaceXAI 稱工單增 175%、每張成本低至 0.2 美元〉（aiposthub，https://www.aiposthub.com/grok-bot-customer-support-case-study/，2026-09-24）。

**① 🔴 事實衝突：方案下放日期與最低門檻（並列、不擇一）**

| 項目 | 既有紀錄（《數位時代》09-18 版，見上一節） | rar.design（09-24） |
|---|---|---|
| 下放日期 | **2026-08-21** | **2026-08-26** |
| 下放範圍 | SuperGrok Plus、Cursor Pro+、Cursor Teams Standard／Premium | SuperGrok、Cursor Pro、所有 Teams 方案 |
| 個人最低門檻 | **Cursor Pro+ $60** | **Cursor Pro $20** |

- 可能是分兩波下放，也可能其中一方誤植。兩者都是媒體轉述官方公告，本庫**未讀過官方公告原文，不擇一**，以官網為準（rar.design 所附方案說明頁：https://cursor.com/help/grok-bot/plans）
- 平台差異（09-18 FAQ「尚未支援 Linux、Android、iPad」vs 09-24 三者皆支援）判為時間差，已在上一節 ② 後附註

**② 產品面新增量（rar.design，多為媒體轉述或官方文件）**

- 取得方式：付費 Cursor 個人方案與 Teams 內含；也可連結個人 SuperGrok（Plus／Heavy）或 **X Premium+**（用量低於 SuperGrok Plus）。SuperGrok Lite 不含；SuperGrok Team／Enterprise 帳號不能連結
- 🔴 **SuperGrok 連結一建立就不能解除，也不能轉到別的 Cursor 帳號**，連結前先確認登入的是對的帳號
- Cursor 方案與 SuperGrok 連結**不疊加**（兩邊都付錢，用量也不會變多）
- 用量每週重置；開了隨用隨付（on-demand）後改由 Cursor 帳單計費，上限是自設的每月額度，**不會在任務中途硬切**，正在跑的 Bot 可以超過上限把事做完
- 免費試用＝一筆用量額度＋7 天期限，依代理實際跑的步驟與 token 扣，與傳幾則訊息無關，一個大任務就可能用完
- 上限：每帳號約 50 個 Bot、每個群組聊天 6 個 Bot、每個 Bot 最多 50 個 routine
- 不支援 Cursor 的舊版隱私模式（Legacy Privacy Mode），需開啟資料儲存
- Teach a task：最長 10 分鐘、不錄麥克風，產出技能草稿（逐步開放中）
- 使用量：彭博社報導上線約一個月每週使用者超過 41 萬、單週成長 24%（第三方媒體轉述）
- 第三方評測整理的論壇回報：重度使用者幾天就用完每週額度、瀏覽器層會當掉；08-20～21 有用戶回報所有 Bot 都無法運作

**③ 風險新增量（官方安全文件，經 rar.design 轉述）**

- 🔴 **刪除 Bot 不會清掉它在共用電腦上的檔案與登入狀態**；專案結束要自己登出網站、刪工作檔、到原服務撤銷連接器授權
- 核准只管下一個動作，已完成的事不會因此復原，所以邊界要在交辦時寫清楚（例：「整理數據、提出預算調整建議，不要改廣告設定，也不要聯絡代理商」）
- Auto Review 本身是模型判斷，只能輔助最小權限原則；規則越窄越好，「一律先問我」與「自動允許」衝突時以前者為準，不要設「瀏覽器裡一律允許」這種寬鬆規則
- Bot 在使用者本機執行指令預設每次先問，官方建議直接設成「永不允許」，除非真的需要碰本機檔案

**④ 客服案例與數字讀法（SpaceXAI 2026-09-22 公開）**

流程五層（串接 Plain 客服平台、Linear、Datadog）：

1. 先查說明中心與過往 FAQ（避免簡單問題消耗推理用量）
2. 比對 Linear 已知問題與 Datadog 錯誤紀錄；新問題建工程追蹤項目，並嘗試錄下重現步驟
3. 回覆與依退款規則處理申請
4. 依急迫度重排工單、調整負責人，在可能違反 SLA 前提醒團隊
5. 每日整理重複問題與產品意見給產品團隊

官方建議的導入階段：連接系統 → 唯讀觀察 → 人工覆核 → 擴大自動執行，與本條目 C 層三階段路線圖一致。

🔴 以下數字**逐項為 SpaceXAI 廠商自述，未經外部驗證**：

| 數字 | 官方說法 | 讀數限制 |
|---|---|---|
| 工單量增加 175% | SpaceXAI 廠商自述 | 未說明比較期間與基準；同期 Cursor 於 08-14 併入、兩個客服團隊整合、產品組合擴大 |
| 沒有新增人力，估計原本可能要多聘 200 人 | SpaceXAI 廠商自述 | 是**估算**，不是裁員；未公布人力成本或排班口徑 |
| 最佳化後單筆解決成本 $0.20–0.30，對比「傳統 AI 客服每次 $1–4」 | SpaceXAI 廠商自述 | 未列比較產品與口徑；未說明是否含訂閱、建置、維護、覆核、補救成本 |
| 99% 退款申請不需人工介入 | SpaceXAI 廠商自述 | 無統計期間、金額分布、錯誤率或申訴比例 |
| 每日整理超過 20,000 筆產品意見 | SpaceXAI 廠商自述 | 去重方式未公開，不等於 2 萬名不同客戶 |
| 參考超過 100 萬筆客服互動學團隊語氣 | SpaceXAI 廠商自述 | 未說明資料範圍 |

🔴 **張力（並列，不下結論）**：Grok Bot 使用條款要求客戶在允許對外溝通、財務交易、刪除資料或重大第三方影響的 Agentic Actions 前採取適當人工覆核；官方安全文件也建議退款等動作保留核准關卡。但同一家公司的客服案例宣稱 99% 退款申請無人介入，且沒有說明退款工作流的權限設定、核准方式或例外處理。

作者建議試行時同時追蹤：解決時間、一次解決率、CSAT、重新開啟率、轉真人比例、退款錯誤率、含覆核與維護的單件總成本。作者判斷「中性偏正面」：可當流程設計參考，**不能當預算試算或裁員依據**。

⑤ 相關延伸：「功能強大 ≠ 可靠性已驗證」的讀法見 [[ai-agent-tool-selection-maturity-and-reliability-framework]]；權限分層見 [[enterprise-ai-permission-three-layers-governance]]；「單件成本要含覆核」與 [[fable-5-delegation-cost-benchmark]] 的含驗收總成本同一方向。

## 相關連結

- [[ai-agent-tool-selection-maturity-and-reliability-framework]] ——「功能強大 ≠ 生產可靠性已驗證」：本篇聚焦 Grok Bot 具體功能與架構介紹，該篇則從產業懷疑論視角（三階段成熟度框架、跨機構 88% 試行失敗率、xAI/SpaceX 商業動機批判）提醒讀者功能清單不等於可靠性驗證，兩篇建議對照閱讀
- [[grok-bot-figma-screenshot-voice-workflow]] —— 窄角度案例：SpaceXAI 設計師用截圖＋語音驅動 Figma Bro 的下指令方法論，是本篇「設計團隊落地案例」的細節延伸
- [[grok-bot-cursor-five-product-decisions-colleague-pilled]] —— 窄角度案例：Cursor 產品負責人還原 Grok Bot 七周開發的 5 個「同事腦」反直覺決策，可與本篇的雲端電腦架構設計互相印證
- [[grok-bot-persistent-agent-interface-design]] —— 官方設計文拆解：Bot 名單取代對話紀錄、頭像即狀態、電腦視窗監看三層；本條目「三層記憶架構」講架構，該條目講介面上東西歸在哪裡（2026-09-29 新增）
- [[enterprise-ai-permission-three-layers-governance]] —— 客服案例「99% 退款無人介入」與使用條款人工覆核要求之間的權限治理問題（2026-09-29 新增）
- [[fable-5-delegation-cost-benchmark]] —— 單件成本要含覆核與驗收（2026-09-29 新增）
- [[grok-bot-routine-automation-ladder]] —— 操作展開：本篇 C 層「跑通三次→固化 Skill→設 Routine」的完整版，含職務章程三要素、排程五要素、「失敗 vs 空結果」與六項排查清單
