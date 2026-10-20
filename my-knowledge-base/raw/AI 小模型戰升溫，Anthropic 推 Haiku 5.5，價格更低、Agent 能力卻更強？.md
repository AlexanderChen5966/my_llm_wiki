---
title: "AI 小模型戰升溫，Anthropic 推 Haiku 5.5，價格更低、Agent 能力卻更強？"
source: "https://techorange.com/2026/10/08/anthropic-claude-haiku-5-5-ai-agent/"
author:
  - "[[廖紹伶]]"
published: 2026-10-08
created: 2026-10-13
description: "AI 小型模型的競爭，正從價格延伸到能承接多少實際工作。Anthropic 在美國時間 10/7 推出新 AI 模型 Claude Haiku 5.5，API 價格最高調降 90%，電腦操作測試表現更大幅超越前代，甚至領先 OpenAI GPT-6 Luna。隨著小型模型逐漸具備執行多步驟任務的能力，Haiku"
tags:
  - "clippings"
status: "inbox"
---
AI 小型模型的競爭，正從價格延伸到能承接多少實際工作。Anthropic 在美國時間 10/7 推出新 AI 模型 Claude Haiku 5.5，API 價格最高調降 90%，電腦操作測試表現更大幅超越前代，甚至領先 OpenAI GPT-6 Luna。隨著小型模型逐漸具備執行多步驟任務的能力，Haiku 5.5 究竟適合接手哪些工作？

## 電腦操作成功率躍升，小模型能做的事越來越多

Anthropic 將 Haiku 5.5 定位為適合大量、高頻且對成本敏感任務的小型模型。除了摘要、資料分類、資料庫查詢等工作，新模型的能力也進一步延伸至瀏覽器與電腦操作，能夠執行涉及多個步驟的工作。

OSWorld 主要測試 AI Agent 能否實際操作電腦，完成需要多個步驟的任務。根據 Anthropic 公布的測試，Haiku 5.5 在 OSWorld 2.1 電腦操作評測的離線子集取得 72.4% 成功率，較前代 Haiku 4.5 的 15.7% 提升 56.7 個百分點，也超越 GPT-6 Luna 的 48.9%。

![](https://techorange.com/app/uploads/2026/10/ae077754a40dd004.png)

圖片來源： Anthropic 。

在衡量 AI 透過終端機執行複雜工作的 Terminal-Bench 4.0 測試中，Haiku 5.5 也取得 39.2%，高於 GPT-6 Luna 的 16.4%。不過，《VentureBeat》提醒，39.2% 是最高推理強度下的成績，預設設定約為 20%。

企業初步測試也反映這項變化。專案管理平台 Asana 表示，在錯誤分類、建立專案及搜尋風險工作等 AI Agent 任務中，Haiku 5.5 相較目前使用的模型，任務完成延遲降低超過 30%，顯示小模型除了在基準測試取得進步，也開始展現處理企業實際工作流程的潛力。

## 大模型負責複雜工作，小模型接手高頻子任務

Haiku 5.5 的另一項定位，是作為大型模型的子代理（Subagent）。Anthropic 指出，它可以搭配 Opus 5.5、Sonnet 5.5 處理程式設計工作，由能力較強的模型負責複雜任務，小模型則執行範圍明確、需要大量重複的工作。

金融 AI 公司 Rogo 提供了一個具體案例。該公司表示，當大型模型負責製作財務簡報時，可以交由 Haiku 5.5 子代理讀取企業提交的 10-K 年報，找出其中一張投影片需要的部門營收數據。

Rogo 應用 AI 團隊成員 Alex Wang 表示，Haiku 5.5 的準確度已足以讓團隊信任它執行這類工作，速度與價格也適合大量使用。AI 程式開發公司 Cognition 同樣將 Haiku 5.5 用作輔助模型，搭配 Opus 5.5 擔任主要模型，以降低成本與延遲。

Haiku 5.5 的價格則進一步提高這種分工的吸引力。對於提示詞不超過 10 萬 tokens 的請求，Haiku 5.5 每百萬輸入、輸出 tokens 分別收費 0.1 美元、0.5 美元，較前代降低 90%；超過門檻則降價 50%。Anthropic 估計，考量實際 Token 消耗後，平均任務成本約可降低 75%。

![](https://techorange.com/app/uploads/2026/10/dbef21b031bfdcf0.png)

圖片來源： Anthropic 。

這也代表，企業可以依工作複雜度，選擇不同等級的模型。不過，小模型仍有能力界線，例如 Sonnet 5.5 在電腦操作與複雜程式設計測試中仍明顯領先。因此，企業仍須依任務成功率、執行時間及重試成本，判斷哪些工作適合交由小模型。

## OpenAI 低價模型攻勢下，Anthropic 也加入成本競爭

Anthropic 這次不只調降 Haiku 5.5 的價格，也同步將 Sonnet 5.5 的快取讀取費用減半，從每百萬 tokens 0.20 美元降至 0.10 美元。快取讀取可讓模型重複使用先前處理過的上下文，避免每次都重新運算，對於需要反覆讀取文件或執行多步驟工作的 AI Agent 而言，有助於進一步控制成本。

這波降價背後，也反映 AI 模型市場日益激烈的價格競爭。《華爾街日報》報導，Anthropic 今年初憑藉 Claude Code 等產品在企業市場取得優勢，但 OpenAI 隨後推出不同能力與價位的 GPT-5.6 系列，吸引企業轉向成本較低的模型。

根據該報導援引的 OpenRouter 數據，今年初，在約 12 萬家使用兩家公司模型的企業中，Anthropic 一度占相關支出約 75%；到了 9 月，兩家公司已大致持平。

Anthropic 已明確將 Haiku 5.5 定位為 Opus、Sonnet 的子代理，讓小型模型承接範圍明確、需要大量執行的任務。從這次電腦操作能力的提升與價格調降來看，小型模型可承擔的工作正在擴大，也讓企業在規劃 AI Agent 時，有更多依任務需求選擇模型的空間。

> 【推薦閱讀】
> 
> ◆ [美中以外最強 AI 模型出現了，Mistral Large 4 靠 4,000 顆 Blackwell 怎麼追上來？](https://techorange.com/2026/10/07/mistral-large-4-le-chonk/)
> 
> ◆ [中國的開放模型對手來了？Reflection AI 推 Beam，憑什麼挑戰 GLM、Qwen](https://techorange.com/2026/10/06/reflection-ai-beam/)
> 
> ◆ [誰真的在為生成式 AI 買單？a16z：前 1% 付費者支出超過後 50% 總和](https://techorange.com/2026/10/07/the-top-100-gen-ai-consumer-apps-7th-edition/)

> 科技的變化總是快得驚人，而我們每天的工作，就是從龐雜的趨勢中理出觀點、提煉出決策者真正需要的洞察。如果你也對 AI 發展充滿熱情， **《TechOrange 科技報橘》正在徵內容編輯！**  
> 👉 [了解職缺內容](https://supr.link/x1Y5A)

＊本文開放合作夥伴轉載，資料來源： [Anthropic](https://www.anthropic.com/claude-haiku-5-5) 、 [《SiliconANGLE》](https://siliconangle.com/2026/10/07/anthropic-releases-claude-haiku-5-5-small-model-and-halves-sonnet-5-5-cache-read-prices/) 、 [《VentureBeat》](https://venturebeat.com/technology/anthropic-launches-claude-haiku-5-5-with-90-api-price-reduction-matching-gpt-6-luna) 、 [《The Wall Street Journal》](https://www.wsj.com/tech/ai/the-ai-price-war-is-heating-upand-openai-is-gaining-ground-on-anthropic-48b8525b) ，首圖來源： [Anthropic](https://www.anthropic.com/claude-haiku-5-5)

![](https://techorange.com/app/uploads/2026/09/8536d0b8afe5e190.png)

達梭系統於日前舉辦 2026 達梭系統臺灣年度高峰論壇暨 SIMULIA 用戶大會，以「虛擬雙生 × AI 虛擬助手」為主題，聚焦三條主軸：如何以虛擬雙生與 AI 虛擬助手在實際部署前完成規劃驗證，降低時間、成本與風險；如何讓 AI 真正理解並運用產業知識，提升分析、推理與決策能力；以及 AI 如何深入產品研發、智慧製造與機器人領域，加速從概念到生產的流程。 今年論壇匯聚達梭系統全球與亞太區高層、NVIDIA 及臺灣產業領袖，共同探討企業 AI 如何從單點導入邁向規模化與工業化，並由達梭系統臺灣戰略客戶銷售總監張銘輝開幕致詞，法國在台協會主任龐維德與達梭系統全球執行副總裁暨亞太區總裁 Samson KHAOU 致歡迎詞。適逢深耕臺灣 20 週年，本次大會同步規劃 3DEXPERIENCE 互動體驗區，集結高科技、航太、汽車、電池、材料、工業設備及生技醫療等產業解決方案，展示 AI 工廠、人形機器人、虛擬雙生、AI 驅動設計與多物理場模擬等跨產業創新案例，見證達梭系統與臺灣產業共同推動轉型的歷程。

## 從「做出來再修」到「先理解再建造」

達梭系統全球執行副總裁暨亞太區總裁 Samson KHAOU 以新舊產業模式的對照破題。舊的產業模式是「做出來、測試、修正、重來」，新模式則是「理解、模擬、最佳化，然後才建造」，Samson KHAOU 表示關鍵在於預測能力：「能預測就能最佳化，能最佳化就能創新，能創新就能領先。」因此虛擬雙生（Virtual Twin）已不等同市場上談的數位雙生（digital twin），後者只是模型，前者正在成為智慧系統，把智慧往上游推，讓產品在造出實體之前就完成預測與調整。

「在既有軟體上外掛 AI，只會得到一個助理；把 AI 嵌進平台內部，才會為客戶帶來轉型。」Samson KHAOU 說明，這正是達梭系統三年前決定從 AI-enabled 軟體轉向 AI-Native 平台的核心概念，而這個決定主要以 3D UNIVERSE 和 3DEXPERIENCE 為核心。3D UNIVERSE 是達梭系統提出的願景，「它不是一個新平台，而是新的產業作業系統，」預期在這個環境中，讓產品的虛擬雙生能夠延伸到製造流程與售後服務，貫穿完整的生命週期，並將人、工作流程與組織內部的 Know-how 連結在一起。要支撐這樣的環境，底層平台必須先改變，因此將 3DEXPERIENCE 升級為代理式平台（agentic platform），成為驅動 3D UNIVERSE 的引擎，平台的核心則是工業世界模型（Industrial World Model），並由四個部分組成：企業的知識與 Know-how 資料庫、建模與模擬、具備產業推理能力的 LLM，以及負責協作調度的 AI Agent。Samson KHAOU 強調，重點不是打造一個只有 LLM 的 AI，而是讓 AI 具備正確的推理能力，並與產業知識和 Know-how 相連。

![](https://techorange.com/app/uploads/2026/09/e030a8b20f015d75-2048x1366.jpg)

達梭系統全球執行副總裁暨亞太區總裁 Samson KHAOU 表示達梭系統決定從 AI-enabled 軟體轉向 AI-native 平台。

針對工業 AI 為何無法沿用通用型 AI 問題，達梭系統 SIMULIA 執行長 Michelle ASH 說明，通用型 AI 容易出現產生 AI 幻覺或出現過時資訊的情況，而工業 AI 必須具備確定性、安全性、環保規範與極高效能，同時還需衡量 Token 的成本與價值。因此達梭系統的應對方式是打造三位虛擬 AI 助手「Aura、Leo、Marie」各自聚焦特定任務，將任務範圍縮小，幻覺機率隨之降低，並運用 LLM、機器學習與電腦視覺三種 AI 技術，以求解器產生的合成資料與實際測試、實際使用的歷史資料進行訓練，讓 AI 的判斷建立在真實物理之上。

而達梭系統與 NVIDIA 的合作就建立在此基礎上。Samson KHAOU 說明 NVIDIA 蓋的是 AI 工廠，用算力規模化智慧，達梭系統蓋的是虛擬雙生工廠，用產業引擎與代理式平台規模化理解。Michelle ASH 補充，SIMULIA 與 NVIDIA 目前正並行推進 AI Agent，將 MODSIM 與 Physics NeMo、Domino 等訓練架構結合，目標是以互連虛擬雙生串起產品、工廠製造、供應鏈、客戶到終端回收的完整鏈路，讓企業在做出第一個原型之前，就在虛擬環境中完成驗證。

![](https://techorange.com/app/uploads/2026/09/e16c020054279bb8-2048x1366.jpg)

達梭系統 SIMULIA 執行長 Michelle ASH 說明，達梭系統打造三位虛擬 AI 助手「Aura、Leo、Marie」各自聚焦特定任務，將任務範圍縮小，AI 幻覺或過時資訊的情況也隨之降低。

科技報橘創辦人暨社長戴季全則將視角拉回臺灣，並點出問題：「未來三到五年，公司內部的資料、客戶資料、工作流程、研發成果，全部都要丟給雲端的 AI 嗎？還是我們要建立自己的 AI？」戴季全表示，企業的生產要素正在改寫，過去是資本、土地與勞力的靜態組合，如今加入了 human intelligence 與 machine intelligence 的動態協作，可自動化、可由 AI 判斷的工作交給機器，人則專注於優化流程與創造新的可能，競爭因此不再是盤點企業擁有什麼，而是比誰的動態循環轉得更快，戴季全也提醒，若只把 AI 當成每月 20 美元的輔助工具，那是「成本」，唯有把 Know-how 累積成模型與資產，才構成真正的「資本形成」。

![](https://techorange.com/app/uploads/2026/09/543c4e735fd53246-2048x1365.jpg)

科技報橘創辦人暨社長戴季全提醒，若只把 AI 當成每月 20 美元的輔助工具，那是「成本」，唯有把 Know-how 累積成模型與資產，才構成真正的「資本形成」。

## 如何靠「虛擬雙生技術」重塑 AI 工程流程？

論壇上半場談的是「理解、模擬、最佳化，然後才建造」這套順序該如何成立，下半場則談論如何在 AI 工廠、AI 伺服器，到一位工程師的工作流程，共用達梭系統同一套虛擬雙生與模擬技術。

NVIDIA 資深解決方案架構經理劉冠良指出，AI Factory 與傳統資料中心不同，它投入的原料是電力與資料，產出的則是智慧與 Token，因此最核心的衡量指標是 Token per Watt，為避免散熱效率不佳、電源最佳化不足與重啟穩定性問題，需要把這些可能的浪費在 AI 基礎建設興建之前就找出來，靠的正是「模擬」，並舉例 NVIDIA 在美國建造的資料中心，正是導入達梭系統的 SIMULIA 與 MBSE 架構，管理從設計、模擬、部署到營運的全生命週期。劉冠良也強調，AI Factory 市場規模預期在接下來十年，從不到 1 兆美元將躍升至 9 至 11 兆美元，如此龐大的工程不可能由單一公司完成，需要臺灣生態系夥伴在散熱、冷卻、電力與硬體製造上的深厚實力。

![](https://techorange.com/app/uploads/2026/09/857bfde8ca01fe0b-2048x1365.jpg)

NVIDIA 資深解決方案架構經理劉冠良強調，AI Factory 市場規模預期在接下來十年，從不到 1 兆美元將躍升至 9 至 11 兆美元，如此龐大的工程不可能由單一公司完成，需要臺灣生態系夥伴在散熱、冷卻、電力與硬體製造上的深厚實力。

緯創資通副總經理王光宇也分享臺廠的實作路徑，緯創將二十年來分散採購的工程模擬軟體集中於 HPC 平台統一管理，包含達梭系統 SIMULIA 旗下的求解器，面對無法進行實體破壞性實驗的高階晶片，以 HPC 算力結合達梭系統的模擬求解器進行高精度仿真模擬，並以累積的 CAE 數據訓練 AI 代理模型，讓工程師調整參數後就可獲得結果預測。

![](https://techorange.com/app/uploads/2026/09/599c216fad1de21b-2048x1366.jpg)

緯創資通副總經理王光宇分享緯創以 HPC 算力結合達梭系統的模擬求解器進行高精度仿真模擬，並以累積的 CAE 數據訓練 AI 代理模型，讓工程師調整參數後就可獲得結果預測。

達梭系統 CATIA 設計總監 Claude BOURBIGOT 則舉例與日本自動化設備商歐姆龍（Omron）的合作，產線模擬出現 18% 的機器人失誤率時，便透過虛擬助手 Leo 完成分析、提出建議並執行修改。

![](https://techorange.com/app/uploads/2026/09/1e91235a5e6de7cb-2048x1365.jpg)

達梭系統 CATIA 設計總監 Claude BOURBIGOT 透過虛擬助手 Leo 完成分析、提出建議並執行修改。

## 第七代平台如何重新定義未來工程語言？

最後，達梭系統大中華區資深技術經理陳哲基指出，產業經驗的傳承方式從老師傅的口耳相傳、手繪圖紙與紙本作業指導書，再到 PLM 系統把規範數位化，而第七代平台要做的，就是讓累積多年的數據知識庫能夠透過 AI 主動被調用，因此必須讓 AI 看得懂 3D 模型，以及訓練資料必須來自真正的工程資料，才能真正讓 AI 協助工作流程。

![](https://techorange.com/app/uploads/2026/09/e8037511bd4ff09d-2048x1365.jpg)

達梭系統系統大中華區資深技術經理陳哲基指出，第七代平台要做的就是讓累積多年的數據知識庫能夠透過 AI 主動被調用。

從 AI 工廠的能源效率、AI 伺服器模擬，到鞋面的紋路程式碼與臨床試驗的醫院選點，這些看似相距甚遠的應用場景，通用的都是：理解、模擬、最佳化，然後才建造的邏輯，也呼應 Samson KHAOU 的結語：「不論你蓋的是 AI 工廠、飛機還是晶片，原則都相同：未來必須先被理解，才能被建造。」

對深耕臺灣二十年、全球服務超過 37萬個來自各產業與不同規模的客戶的達梭系統而言，下一個階段的課題，是與臺灣企業一同從單點的工具導入，變成可規模化、可複製的組織能力。

想進一步了解虛擬雙生、工業 AI 與 **3D** EXPERIENCE 平台，歡迎瀏覽達梭系統官方網站： [https://www.3ds.com/](https://www.3ds.com/)

![](https://sslcode.adgeek.com.tw/public/images/popad_close_button.png)