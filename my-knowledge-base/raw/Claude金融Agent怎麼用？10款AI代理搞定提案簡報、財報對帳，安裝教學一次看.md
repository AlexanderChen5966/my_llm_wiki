---
title: "Claude金融Agent怎麼用？10款AI代理搞定提案簡報、財報對帳，安裝教學一次看"
source: "https://www.bnext.com.tw/article/91588/anthropic-claude-financial-agents-guide"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-07-24
created: 2026-07-27
description: "Anthropic推出金融業開源AI代理範本，橫跨投行、股票研究、私募到財富管理；10個代理哪些團隊適用、導入前又該留意什麼？"
tags:
  - "clippings"
status: processed
---
在金融業中，團隊常常得在 Excel 與 PowerPoint 之間手動搬數據、更新報表與估值模型，過程不僅耗時、容易出錯，也嚴重壓縮了真正需要「動腦」的商業分析與決策時間。

針對這些痛點，Anthropic 官方推出了 10 種現成可用的 Claude 財務 AI 代理（AI Agents）範本，能自動整理提案簡報、建立 Excel 估值模型與對帳，可直接在 GitHub 下載，還能修改成專屬工作流。

本文帶你一次搞懂這 10 款官方 Agent 是什麼、各自解決哪些痛點、如何安裝，以及動手導入流程前須釐清的使用邊界。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## Claude財務AI代理是什麼？跟直接問AI差在哪？

Claude 財務 AI 代理是 Anthropic 官方首度針對高度監管、流程嚴密的金融產業，釋出一套開源的「AI 代理工作流參考範本」，採 Apache 2.0 免費授權、涵蓋投資銀行、股票研究、私募股權與財富管理四大場景的 AI 代理，以及工作流技能與資料連接器參考架構。

與一般對話型 ChatGPT 或 Claude 的主要差異，在於一般對話 AI 會被動回答問題，若無明確詳細的提示詞，容易產生幻覺或排版錯亂。

Claude 財務 AI 代理只需下達高階任務，例如：請為某公司製作一份 LBO 收購提案， **便會自動調閱內部合規政策、讀取專案資料夾、呼叫 Excel 等外部運算工具進行現金流折現建模** ，並輸出符合公司品牌視覺的簡報初稿。

## Claude財務AI代理由哪些元件組成？

依官方文件，Claude 財務 AI 代理由五個元件搭起來，其中對使用者最有感的是以下三個：

- **代理（Agents）：** 自成一體的外掛，一個代理就負責一整條工作流，安裝它就等於一次備齊所需能力。
- **技能（Skills）：** 定義特定分析方法與作業步驟，例如：如何做現金流折現建模、三表平衡的公式規則、簡報視覺排版規範。AI 會在適當時機自動調用。
- **連接器（Connectors）：** 透過 MCP（Model Context Protocol）開放協定建立的資料安全通道，讓 Claude 在權限控管下，讀取市場行情或機構內部的資料倉儲。

另外兩個元件較偏技術：一是斜線指令（Commands），供你主動呼叫特定功能（如 /comps、/earnings）；二是受管代理封裝（Managed-agent wrappers），供無人化部署使用。在受管部署的情境下，主代理可再委派子代理處理特定子任務，但這項委派能力目前仍標註為研究預覽。

## Claude財務AI代理總整理！這10款分別適合哪些團隊？

這 10 款 AI 代理涵蓋了從交易提案到月底關帳的完整環節，可分為兩類：一類負責對外的研究、承銷與客戶工作，一類負責對內的帳務、估值與法遵工作：

| Agent | 核心任務 | 適用團隊 |
| --- | --- | --- |
| **Pitch Agent** | 從同業比較、前例交易到收購模型，產出帶品牌樣式的提案簡報 | 投資銀行、業務拓展 |
| **Meeting Prep Agent** | 於客戶會議前，自動彙整一份行前簡報包 | 客戶關係、業務團隊 |
| **Market Researcher** | 就特定產業或主題，產出產業概觀、競爭格局與投資構想清單 | 研究、產業分析 |
| **Earnings Reviewer** | 研讀財報與法說會逐字稿，自動比對市場預期差距、更新 Excel 預估模型，並草擬 2 頁式研究更新筆記 | 股票研究、資產管理 |
| **Model Builder** | 直接於 Excel 中建立現金流折現、收購與三大財務報表（損益/資產負債/現金流）預測模型 | 財務建模、投資銀行分析 |
| **Valuation Reviewer** | 讀取普通合夥人 (GP) 提供的投資資料包，檢核其估值假設是否合理，並草擬提交予有限合夥人 (LP) 的季度審查報告 | 基金行政、估值團隊 |
| **General Ledger Reconciler** | 執行總帳對帳、追查差異根因，並準備送交簽核的文件。 | 會計、基金會計 |
| **Month-End Closer** | 處理應計、結轉與差異說明等月結作業 | 會計、企業財務 |
| **Statement Auditor** | 於對外發送前，稽核財務報表的一致性與完整性 | 財務報表與稽核團隊 |
| **KYC Screener** | 解析開戶文件、比對規則、標示缺漏與異常 | 法遵、客戶審查 |

須留意的是，這些代理產出的皆為「初稿」，設計上須交由專業人員審核；AI 不會自行做出投資決策，也不會執行下單、入帳或核准開。

## Claude財務AI代理如何安裝？3 種部署方式與適合對象

Claude 財務 AI 代理採同一套範本，提供 3 種安裝與部署路徑，差別主要在於使用者的技術背景，以及系統運作的環境。

**無論採取哪一種部署路徑，都必須優先載入 financial-analysis 核心模組。** 這 10 款 AI 代理僅封裝了特定任務的工作流邏輯，真正執行財務建模、抓取外部數據時，都需依附核心模組內建的共用技能與資料連接器；若未先安裝此模組，其他代理都將因缺乏底層工具而無法運作。

先參閱下方對照表，再為團隊評估最合適的導入方式（安裝名稱以 GitHub Marketplace 為準）：

| 安裝方式 | 適合對象 | 操作方式 |
| --- | --- | --- |
| **1\. Claude Cowork** | **一般商務與分析團隊**   （如：金融分析師、業務人員） | **視窗介面操作** ：直接在軟體畫面上點選按鈕、或上傳壓縮檔即可完成安裝，不需要任何程式背景。 |
| **2\. Claude Code** | **具備技術背景的研發團隊**   （如：工程師、數據分析師） | **指令碼操作** ：透過在系統的文字介面中，輸入幾行安裝指令來完成設定與調用。 |
| **3\. Claude Managed Agents** | **負責企業系統架構的 IT 部門**   （需對接公司內部資料庫） | **後台自動化串接** ：將 AI 代理直接串聯到企業自有的作業後台，讓系統能在無人監管下自動調度運行。 |

### 方式一〉Claude Cowork（適合非技術人員）

開啟 Claude 桌面版 App，在最上方的模式選擇器點選 Cowork 分頁，再於左側欄點「Customize」，接著點「+」按鈕選擇「Add marketplace from GitHub」，貼上以下網址，市集就會列出所有可安裝的金融外掛：

> [https://github.com/anthropics/financial-services-plugins](https://github.com/anthropics/financial-services-plugins)

清單中先安裝核心的 financial analysis，再依需求勾選所需代理。安裝後外掛會自動啟用，也可在對話中打「/」或點「+」呼叫功能，例如 /comps（同業比較）、/dcf（現金流折現）、/earnings（財報更新）。

圖／ 數位時代

### 方式二：Claude Code（適合工程／進階使用者）

於終端機環境下依序執行以下指令，先匯入官方市集（Marketplace），接著安裝核心運算技能，最後選擇需要的 Agent 及領域技能包：

```
# 加入 Marketplace
claude plugin marketplace add anthropics/financial-services

# 安裝核心技能與連接器
claude plugin install financial-analysis@claude-for-financial-services

# 挑選並安裝所需的 Agent
claude plugin install pitch-agent@claude-for-financial-services
claude plugin install gl-reconciler@claude-for-financial-services

# 安裝垂直領域技能包
claude plugin install investment-banking@claude-for-financial-services
```

若需讓代理無人化、排程自動執行，Anthropic 也提供透過 Managed Agents API 部署至自家後端的方式，目前為公開測試階段。此外，也能搭配 Claude 的 Microsoft 365 增益集，在 Excel、PowerPoint 與 Word 之間接力作業。

![#0 claude金融agent](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/eq1n-1784823183.png?w=1200&output=webp)

圖／ 數位時代

### 方式三：Claude Managed Agents（企業後台 API 部署）

針對需將 Agent 深度整合至企業自有系統與工作流的情境，技術團隊需先配置 API 金鑰，再透過官方提供的腳本進行自動化部署：

```
export ANTHROPIC_API_KEY=sk-ant-...
scripts/deploy-managed-agent.sh gl-reconciler
```

系統會自動解析檔案、建立子代理，並透過 POST 請求部署。此方法適合將 Agent 深度整合至企業自有系統。

## Claude 財務 AI 代理能串接哪些外部資料供應商？

Claude 財務 AI 代理的外部資料連線功能，統一集中於 financial-analysis 核心外掛中。這些連接器採用 MCP 開放協定。

以下為官方支援的資料供應商清單：

| 資料類別 | 支援的供應商 |
| --- | --- |
| **財報、市場與基本面** | Daloopa、FactSet、S&P Global、Morningstar |
| **信用評等與公司資料** | Moody's |
| **私募與投資組合** | PitchBook、Chronograph |
| **新聞與法說會資料** | MT Newswires、Aiera |
| **債券、利率、外匯與總經** | LSEG |
| **企業內部文件庫** | Egnyte、Box |

需要留意的是，連接器的作用是「打通管道」，企業仍需自行訂閱並取得該供應商的 API 金鑰才能實際存取資料。

## 如何將 AI 代理改成企業專屬的工作流？

Anthropic 將這 10 款 Agent 定位為參考範本，由於整個 Repo 皆為 Markdown 與 YAML 純文字檔，不需複雜的編譯即可修改。官方建議透過以下 5 個方向進行企業客製化：

> - **抽換資料連接器：** 將.mcp.json 指向企業已訂閱的資料商或內部系統。
> - **灌入企業脈絡：** 將團隊專用的術語、分析流程與格式規範寫入 Skill 檔案中。
> - **套用品牌範本：** 利用 /ppt-template 指令，教導 Claude 使用企業專屬的 PowerPoint 視覺版型。
> - **調整 Agent 權限範圍：** 修改 agents/.md，使 AI 的行為準則對齊團隊實際的作業標準。
> - **擴充新工作流：** 若有未涵蓋的任務，可直接複製現有架構進行擴充。

## 使用前須知：人工審核與 4 大邊界限制

導入這套系統前，企業必須建立一個認知：這 10 款 Agent 產出的是待審初稿，而非可以直接送出或執行的最終定案。

以下是實際應用時必須留意的限制：

- **人工簽核（Human-in-the-loop）：** 官方聲明內容不構成投資或法律建議。Agent 只負責草擬文件，不執行交易、不過帳、不做終端決策，最終仍需專業人員核可。
- **子代理功能仍為預覽階段：** 主 Agent 呼叫子代理（callable\_agents）的功能目前標註為 Research Preview，建議先詳閱各 Agent README 的安全性細節。
- **外部資料庫需額外付費：** 連接器僅為存取接口，存取 FactSet、Moody's 等資料庫需具備企業自身的有效訂閱權限與金鑰。

資料來源： [anthropics/financial-services（GitHub）](https://github.com/anthropics/financial-services) 、 [Anthropic：Agents for financial services](https://www.anthropic.com/news/finance-agents) 、 [Claude 說明中心](https://claude.com/resources/tutorials/install-financial-services-plugins-for-cowork)

> 延伸閱讀：  
> [Claude Cowork 大升級！串起 Excel＋PPT 全自動工作流，如何重寫高薪白領工作流？](https://www.bnext.com.tw/article/90146/anthropic-enterprise-ai-agent-finance-revolution)  
> [Claude 可以做什麼？90 個官方使用情境、Prompt 範例一次看](https://www.bnext.com.tw/article/90411/what-you-can-do-with-claude)  
> [AI Agent 落地金融業！Robinhood 採 MCP 協定，開放 Claude 與 Cursor 買股及刷卡](https://www.bnext.com.tw/article/91078/robinhood-agentic-trading-agentic-credit-card-ai-trading-mcp-claude-cursor)

本文初稿為 AI 編撰，整理．編輯／蘇柔瑋