---
title: "ChatGPT Skills怎麼用？@一下呼叫技能、3種建立方式教學、6組好用範例一次看"
source: "https://www.bnext.com.tw/article/91495/chatgpt-skills-tutorial"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-07-15
created: 2026-07-16
description: "本文整理Chatgpt Skills的運作方式、3種建立方法與官方推薦的職場應用範例，帶你一次掌握怎麼用。"
tags:
  - "clippings"
status: processed
---
每次請 ChatGPT 寫週報或整理會議紀錄，都得重新輸入格式、語氣與欄位要求，即使指令寫得很完整，產出結果還可能不一致。

讓重複工作的產出更穩定，OpenAI 推出了 ChatGPT Skills（技能功能），這項功能就像替 AI 準備工作說明書，事先寫好執行流程、輸出格式與注意事項，日後遇到相同任務，只要呼叫對應的 Skill，ChatGPT 就能依照既定規則完成工作，省下反覆說明需求與調整格式的時間。

本文將介紹 ChatGPT Skills 的運作方式、3 種建立方法，以及 OpenAI 官方整理的 6 種職場應用範例，一次把常用的工作流程變成可以重複使用的 AI 工具。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

#### 文章目錄

[**一、ChatGPT Skills 是什麼？與 GPTs、Projects 差在哪？**](#介紹)  
[**二、ChatGPT Skills 哪些帳號可以使用？免費版、Plus 支援嗎？**](#方案)  
[**三、3 種方式建立專屬 Skill：對話、編輯器與檔案上傳**](#建立步驟)  
[**四、ChatGPT Skills 怎麼呼叫？@ 指定與自動觸發 2 種情境**](#呼叫方式)  
[**五、職場實戰必備！OpenAI 官方點名的 6 大 Skills 範例**](#範例推薦)  
[**六、使用前必看：ChatGPT Skills 的 3 大限制與安全規範**](#注意事項)  
[**七、Skills 進階應用：報帳、資料分類等日常實測教學**](#延伸閱讀)

## 一、ChatGPT Skills 是什麼？與 GPTs、Projects 差在哪？

根據 OpenAI 官方定義，Skill 是一組可重複使用的指令與相關資源，用來教 ChatGPT 執行特定任務或遵循固定工作流程。

一個 Skill 通常以資料夾或壓縮套件的形式存在，其中必須包含 SKILL.md 作為主要入口。除了文字指令，也能加入範本、參考資料、圖片或可執行腳本。

許多人常把 Skills 與 GPTs、Projects 搞混，這三者的功能定位差異如下：

### Skills、自訂 GPTs、Projects三大功能比一比

| 功能 | 定位與使用邏輯 | 適合的實務情境 |
| --- | --- | --- |
| **Skills** | **寫一份標準作業流程（SOP）** ：教導 ChatGPT 在處理特定任務時，依循預先設定的步驟、格式與品質標準。 | 撰寫週報、整理會議紀錄、產出固定排版報告等高度重複性的日常任務。 |
| **自訂 GPTs** | **請一位專屬領域專家** ：客製化一個具備特定人設、知識庫與語氣的獨立 AI 助理。 | 需要一個固定角色的長期顧問，例如：多國語言翻譯官。 |
| **Projects（專案功能）** | **開一間專案戰情室** ：將特定任務的歷史對話、參考檔案與背景脈絡集中管理於同一工作區。 | 團隊圍繞特定目標進行長期協作，例如：籌備季度的新產品發表會行銷專案。 |

使用時，三者功能可互補：Skills 用來保存可重複使用的工作流程；自訂 GPTs 是針對特定目標打造的客製化 ChatGPT；Projects 則集中管理共同的對話脈絡、檔案與工作內容。

## 二、ChatGPT Skills 哪些帳號可以使用？免費版、Plus 支援嗎？

目前的 Skills 功能仍處於測試版（Beta）階段，目前已開放給以下以工作與教育情境為主的方案與平台使用：

| 支援狀態 | 方案與平台 |
| --- | --- |
| 已開放 Beta | ChatGPT Business、Enterprise（企業版）、Edu（教育版）、Teachers 與 Healthcare 方案；Codex 與 API 也支援 Skills |
| 預計支援 | 個人付費版 Plus 與 Pro，官方已預告將於後續逐步開放 |
| 尚未支援 | 免費版（Free）與 Go 方案，目前未列入官方支援名單 |

在企業版與教育版工作區中，Skills 目前預設為關閉，需由管理員進入「Permissions & roles」，設定哪些角色可以建立、使用、上傳、分享或安裝 Skills。

此外，目前 Skills 尚未提供跨產品自動同步機制，使用者在 ChatGPT 建立的技能，不會自動出現在 Codex 中，需透過手動下載檔案後再自行上傳。

值得留意的是， **符合資格的帳號預設包含一個名為 skill-creator 的 Skill** 。當你在對話中請 ChatGPT 建立或修改 Skill 時，系統會透過這項工具協助完成。

## 三、3 種方式建立專屬 Skill：對話、編輯器與檔案上傳

想要打造符合個人或公司需求的 Skill，可依據習慣選擇以下三種途徑：

### 方法一〉在對話中直接下指令

這是最平易近人的入門方法。開啟全新的對話視窗，直接輸入提示詞：「幫我建立一個技能…」，接著詳細描述你希望的工作流程。

ChatGPT 可能進一步詢問輸入資料、預期輸出格式及需要使用的工具，完成後再依介面提示儲存或安裝。

![#0 chatgpt skills](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/vqxj-1783972933.png?w=1200&output=webp)

圖／ 數位時代

### 方法二〉使用 Skills 編輯器從零自訂

進入 [https://chatgpt.com/skills](https://chatgpt.com/skills) ，點選頁面右側的「＋」→「透過編輯器建立」。

此介面允許使用者直接編寫 SKILL.md 的 Markdown 指示指令，並上傳附加參考資源，完成後點擊建立；適合希望針對執行步驟進行精細調校的專業使用者。

圖／ 數位時代

### 方法三〉上傳現成的 SKILL.md 檔案

在同樣的 [https://chatgpt.com/skills](https://chatgpt.com/skills) 頁面中點選右側的「＋」→「從電腦上傳」，即可直接匯入電腦中的現成設定檔。

OpenAI Skills 採用開放的 Agent Skills 標準格式，即在 Claude 或 Codex 系統中寫好的技能檔案，下載後可以直接上傳至 ChatGPT 套用，就能跨工具轉換。

![#2 chatgpt skills](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/bi5f-1783972933.png?w=1200&output=webp)

圖／ 數位時代

為了確保系統安全，上傳外部的 SKILL.md 時，ChatGPT 會先進行安全掃描。多數 Skill 可在掃描完成後立即使用；若包含特殊代碼，系統可能會標示為 「Needs Review（需人工確認）」；若偵測到潛在風險與惡意指令，檔案將直接被 「Blocked（封鎖）」而無法使用。

## 四、ChatGPT Skills 怎麼呼叫？@ 指定與自動觸發 2 種情境

安裝好需要的 Skill 後，在日常對話中有兩套主要的運作與呼叫方式：

### 1\. 手動輸入「@」明確指定 Skill

在對話框中輸入 @ 符號，系統便會列出已安裝的技能清單。當使用者明確知道這項任務要採用哪一套 SOP 時，例如輸入：@weekly-report 幫我把這段文字整理成本週工作進度，為最直接且精準的做法。

### 2\. 讓 ChatGPT 自動判斷與執行

即使不手動輸入 @，ChatGPT 也會在對話過程中，根據每個 Skill 預設的名稱與文字描述，自動選用相關 Skill，也可能搭配其他適用的 Skills 完成任務。因此，在建立 Skill 時，文字描述欄位撰寫得越具體清晰，AI 自動命中的準確率就越高。

## 五、職場實戰必備！OpenAI 官方點名的 6 大推薦 Skills 範例

OpenAI Academy 官方部落格整理了多種職場 Skills 應用情境，涵蓋行銷、業務、財務、營運與管理，以下精選 6 個官方範例，帶你快速看懂 Skills 能如何減少重複工作、提升產出效率：

| 應用領域 | 技能名稱（可直接複製建立） | 解決問題 |
| --- | --- | --- |
| **行銷** | **campaign-brief-builder** | 將初步的行銷點子或提案，快速轉化為包含活動目標、目標受眾（TA）、核心溝通訊息、發布管道與執行時程的標準行銷企劃案。 |
| **業務** | **discovery-to-next-steps** | 將業務拜訪紀錄與會議筆記，自動歸納為客戶痛點摘要、資格與預算評估及下一步追蹤行動建議。 |
| **財務** | **monthly-close-narrative** | 自動起草財務月結報告，並針對數據差異原因、關鍵財務風險與待解決帳目提供結構化的分析說明。 |
| **營運** | **ops-playbook-writer** | 將日常營運流程整理成標準作業程序（SOP），清楚列出操作步驟、交接方式、服務標準（SLA）與例外狀況處理方式。 |
| **管理者** | **weekly-status-multi-format** | 把團隊成員零散的工作進度彙整，並自動輸出為三種格式：團隊工作更新、高階主管摘要，以及風險與支援需求說明。 |
| **高階主管** | **exec-decision-brief** | 將龐雜的市場研究或會議資訊，濃縮成一頁式決策簡報，清楚條列方案選項、各自優劣勢對比與最終執行建議。 |

## 六、使用前必看：ChatGPT Skills 的 3 大限制與安全規範

使用 Skills 前，有三項限制與管理規範需要留意：

**跨產品不會自動同步：** Personal Skills 不會在 ChatGPT、Codex 或不同介面之間自動同步。使用者可以下載 Skill，再安裝到另一個支援的產品，但仍需分別完成安裝與權限設定。

**外部 Skill 可能帶來安全風險：** Skill 可包含指令、參考資料與可執行腳本。上傳後，ChatGPT 會進行安全掃描，結果可能顯示為可直接使用、Needs Review（需確認）或 Blocked（封鎖）。從外部來源取得 Skill 時，仍應先檢查內容，避免敏感資料外洩。

**企業環境受權限管控：** 在商用與教育方案中，使用者能否建立、上傳、分享或安裝 Skill，取決於工作區管理員開放的權限。若介面中沒有相關功能，可先向內部 IT 或系統管理員確認。

當企業或團隊導入商用方案後，最有效率的起手式就是打開新對話，輸入「Build me a skill...」，把那些每週都要重複打字解釋三次的繁瑣行政流程，正式交由 SOP 化的 AI 助理全面接手。

## 七、Skills 進階應用：報帳、資料分類等實測教學

準備開始動手打造專屬 Skill 前，推薦搭配以下2篇實測分析：

**概念與格式釐清：** 這篇 [Agent Skills 概念與比較懶人包](https://www.bnext.com.tw/article/90186/what-is-agent-skill) 整理了 SKILL.md 的文字規範，並梳理此功能與 GPTs、Gem 在運作邏輯上的差異。

**工作流程實測：** 這篇 [Skill 訓練專屬助理實測教學](https://www.bnext.com.tw/article/90328/claude-skill-efficiency-guide) ，記錄了將技能運用於報帳、資料分類等日常行政事務的操作過程與產出結果。

> 延伸閱讀：  
> [ChatGPT專案功能怎麼用？免費、Plus版差異、自訂指令範例、5步驟設定教學一次看](https://www.bnext.com.tw/article/91370/chatgpt-projects-tutorial)  
> [新版ChatGPT攻略｜Chat、Work、Codex 差在哪？如何選擇適合自己的模式？差異一次看](https://www.bnext.com.tw/article/91491/chatgpt-work-chat-codex-guide)

資料來源：OpenAI Help Center、OpenAI Academy

本文初稿為 AI 編撰，整理・編輯／蘇柔瑋