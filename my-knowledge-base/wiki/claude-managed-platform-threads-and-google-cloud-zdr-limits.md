---
title: Anthropic 管理平台新入口多不適用 ZDR／HIPAA：Message Threads 與 Google Cloud 代管版
date: 2026-11-05
tags: [ai/claude, type/concept]
source: https://www.aiposthub.com/claude-message-threads-api-long-context-latency/ ; https://www.aiposthub.com/claude-google-cloud-anthropic-managed-ga-iam-billing/
type: article
status: processed
asset_value: 24
---

> ⚠️ **邊界條目（asset_value 24，剛好壓線）**：兩篇各自單獨不到門檻（Message Threads 18、Google Cloud 代管 12），合併後才成立；受眾是 API 開發者與雲端採購／資料治理人員，使用頻率低。若覺得用不到，可視為「只備查」。
>
> **來源性質**：皆為 aiposthub（Philo）**二手整理**，轉述 Anthropic 官方公告與文件、Google 官方比較文件；**本庫未對一手來源核對**。數字與功能為快照：Message Threads 於 2026-11-02 公開測試（beta；發文 11-05）；Google Cloud 代管版於 2026-10-22 宣布（發文 10-24）。

## T（核心論點）

**Anthropic 自己管理的新平台入口，常常有「資料處理不是你原本那套」的條件：Message Threads 不適用零資料保留（ZDR）與 HIPAA；Google Cloud 的 Anthropic 代管版會建立獨立 Anthropic 組織，原本的金鑰、工作區、醫療資料控制與零資料保留都不承接。** 效能或採購上的便利（首字更快、沿用雲端帳單）不能代替資料條件的重新確認。

兩篇天然互相引用：Message Threads 的支援平台清單包含「Anthropic 管理的 Google Cloud Claude Models」。

## B（怎麼選、注意什麼）

**適用：**
- 活躍、上下文很長（官方例：程式代理、電腦操作代理、大量文件／圖片）的 API 代理，想縮短每回合的首字等待 → Message Threads
- 已用 Google Cloud 採購與 IAM、又需要 Claude Platform 原生功能的團隊 → Anthropic 代管版

**不適用 / 注意：**
- 🔴 **處理敏感或受法規約束資料前，先確認資料條件**：Message Threads 不適用 ZDR 與 HIPAA；Google Cloud 代管版需重新檢查新組織的資料保留與適用條款。安全／合規類事實**引用前回官方文件核對**
- ⚠️ 短對話、或時間多花在工具執行的代理，Message Threads 收益可能很小；每回合間隔很久時會常需重建
- ⚠️ Google Cloud 代管版「正式開放」不代表所有附屬功能轉正式；功能仍可能標 beta

## R（內容）

### 一、Message Threads（API 測試版，2026-11-02 公開測試）

- **機制**：第一次送完整對話（含系統提示、工具定義）並建立對話串；之後只送**新增訊息＋上一個 response id**，服務端短期保存歷史並重建完整上下文。SDK 另有輔助物件組合請求（完整歷史仍在記憶體，範圍取決於程序生命週期）。
- **效能（廠商自稱，未驗證）**：官方稱 50 萬–100 萬 token 的長請求，**首字延遲（TTFT）中位數約降 35%**，限這個區間；官方稱多數 Claude Code 使用者已預設啟用。改善的是回覆**開始前**的等待，不是整段回答或整個任務時間。
- **限制**：
  - **閒置 5 分鐘過期**（每次保存重新計時）；保存為「盡力提供」，過期時回「找不到對話串」錯誤，須用**完整歷史重建**新串——應用程式才是對話的主要紀錄來源。
  - 每個 response id **只能接續一次**；平行或分支要建新串。續接請求只放新增訊息，重送舊訊息會讓歷史重複或出錯。
  - **無列出／讀取／刪除 API**；對話串不能當長期記憶。
  - 某些上下文管理、壓縮等功能組合可能使對話串無法使用。
- **計費**：token 用量、既有費率與速率限制不變（模型每回合仍讀完整上下文）；Prompt Caching 需另外設定，不會自動啟用。
- **🔴 資料保留**：加密並存於記憶體、不寫持久儲存，**最後使用後 1 小時內刪除**；5 分鐘與 1 小時回答不同問題（可否續用 vs 刪除上限）。**官方文件列為不適用零資料保留與 HIPAA**（引用前回官方核對）。
- **支援平台**：Claude Platform、Claude Platform on AWS、Microsoft Foundry、Anthropic 管理的 Google Cloud Claude Models。
- 評估建議：分開量首字時間、輸入 token、快取用量；測試要含一次刻意超過保存時間的停頓，驗證重建路徑。

### 二、Google Cloud 的 Anthropic 代管版（2026-10-22 宣布，GA）

- **兩種代管並存**：Anthropic 代管（Anthropic 經營服務、功能對齊第一方 Claude API，文件列檔案、程式執行、網頁搜尋、遠端工具；測試版標頭可傳但各功能仍標 beta）vs Google 代管（跟著 Agent Platform／Model Garden 節奏）。
- **認證與採購**：用 **Google IAM**／服務帳號／應用程式預設憑證，**不是 API 金鑰**；經 **Google Cloud Marketplace** 採購（牌價、協商折扣或私人報價）；官方 X 公告提到可消化既有雲端用量承諾——**是否折抵、折多少看合約，無公開數字**。帳單整合不等於自動打折。
- 🔴 **會建立獨立 Anthropic 組織**：原**API 金鑰、工作區、醫療資料（HIPAA）相關控制、零資料保留**與其他設定**不承接**，須在新組織重新檢查。
- 🔴 **資料位置**：資料儲存與推論在 **Anthropic 管理的基礎設施**，**可能路由到 Google Cloud 之外**；Google 與 Anthropic 就各自服務部分為獨立資料處理者，治理要兩邊條款一起核對。可按請求指定推論地理位置。**零資料保留須申請，官方用語是「可能提供」，非預設**。
- **速率限制與配額由 Anthropic 管理**，既有 Google 代管服務的額度不會自動變成新入口的容量。
- 作者建議（非官方）：用同一個有基準的代表性任務跑兩條路徑，保留功能、權限、帳單紀錄；先建新組織與測試工作區、接沒有敏感資料的任務，再談正式遷移。

### 三、兩者並讀

| 面向 | Message Threads | Google Cloud 代管版 |
|---|---|---|
| 狀態（本庫快照） | API beta，2026-11-02 | GA，2026-10-22；功能仍有 beta |
| 資料條件 | 不適用 ZDR／HIPAA；1 小時內刪除 | 獨立組織，ZDR／HIPAA 控制不承接；資料可能出 Google Cloud |
| 廠商自稱未驗證 | 首字延遲中位數降約 35%（50–100 萬 token） | 無數字；折抵看合約 |

## C（關鍵行動）

- [ ] 要把敏感／受法規資料送進 Message Threads 或 Google Cloud 代管版前，先向官方文件與窗口確認 ZDR／HIPAA 條件，不憑「同一張帳單」「同一個模型名」推定
- [ ] 接 Message Threads：自己保存完整歷史，並實測「超過 5 分鐘停頓後重建」
- [ ] 評估 Google Cloud 代管版：列出應用實際呼叫的功能清單再比對兩條路徑；先確認配額由誰受理
- [ ] 引用「降約 35%」時一併寫明：廠商自稱、僅限 50–100 萬 token、未驗證

## 補充資料：處理紀錄

- 排除：Google Cloud 篇文末「訂閱 AI 郵報」導流與站內「延伸閱讀」（OpenAI Google Cloud 合作、Managed Agents）；Message Threads 篇的站內延伸閱讀。
- 未查：Google 官方比較文件原文；Message Threads 官方文件原文；Vertex／Bedrock 專屬條目庫內沒有。

## 相關連結

- [[claude-connector-auth-and-long-task-tasks-elicitations]] — 同批姊妹條目：連接器授權路徑與長任務的補問
- [[claude-code-cost-optimization-7-tips]] — 成本與 token 面向；Message Threads 不改 token 計費
- [[claude-code-harness-internals]] — 每回合如何組上下文，對照「只送新增訊息」的差異
- [[claude-code-api-request-reverse-engineering]] — 請求內容怎麼組成，可對照續接請求只放新增訊息的做法
- [[claude-platform-opentelemetry-beta]] — 同為 Claude Platform 的 2026-10 beta 功能，可追蹤 API 呼叫過程
- [[enterprise-ai-permission-three-layers-governance]] — 企業治理分層；資料保留條件屬 Platform 層，不能被採購入口取代
- [[claude-security-continuous-scanning-fix-tracking]] — 另一個「不支援 ZDR」的 Claude 新功能（僅 Enterprise 公開測試），同類邊界
- [[claude-model-selection-guide]] — Fable 5 強制 30 天資料保留、不提供 ZDR 的選型注意，與本條目同屬「選型前先查資料保留條件」
