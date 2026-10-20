---
title: "Fable 5單價貴一倍，帳單卻大降！Cognition實測3000次發現：AI代理成本關鍵在委派"
source: "https://www.bnext.com.tw/article/91566/ai-management-fable-5-opus"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-07-27
created: 2026-07-27
description: "AI新創Cognition發現，導入單價貴一倍的Fable 5模型後，整體成本反而下降了，原因在於尖端模型「當主管」的能力差異。"
tags:
  - "clippings"
status: processed
---
能力越強、成果越好的AI模型，往往代表著更高的成本與費用。但AI新創公司Cognition卻打破了這樣的規則，他們將原先使用的Claude Opus 4.8換成每token牌價為其兩倍的高階模型Claude Fable 5後，平均每次任務的總成本反而下降，評測分數也更高。

## 新創實測！改用Fable 5反而更便宜，差別在會不會「管」

[Cognition](https://www.bnext.com.tw/article/84438/cognition-ai) 成立於2023年，由程式競賽金牌出身的Scott Wu領軍，是近兩年竄升最快的AI應用新創之一。旗下產品Devin號稱全球第一個「全自動AI軟體工程師」，不同於主要協助人類寫程式的副駕駛工具，它能在雲端環境中自行規劃、撰寫程式碼、測試與除錯，完成被交派的軟體開發任務。

![COGNITION X.jpg](https://image-cdn.learnin.tw/bnextmedia/image/album/2025-07/img-1752652867-80659.jpg?w=1200&output=webp)

Cognition共同創辦人Jeff Wang與Scott Wu。

圖／ COGNITION X

Cognition工程師Joon Hee Lee於7月13日在公司官方部落格發表分析指出，在Devin的Fusion架構下，團隊把負責統籌工作的「領導模型」從Claude Opus 4.8換成Claude Fable 5後，整體任務成本反而下降。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

團隊利用自建的FrontierCode 1.1軟體工程評測，針對四種模型配置合計執行3,000個評估工作階段。這套評測衡量的不只是程式能否通過測試，也包括產出的程式碼是否具備可被合併的品質。

單獨執行時，結果一如預期：Fable得分60.8，高於Opus的55.4，平均每次成本也較高，分別為4.03美元（約新台幣130元）與3.06美元（約新台幣99元）。但當團隊替兩者配上同一個較便宜、速度較快的Sidekick副代理後，成本排序就反轉了：Fable搭配Sidekick平均每次成本為1.86美元（約新台幣60元）、得分60.7；Opus搭配Sidekick則為2.04美元（約新台幣66元）、得分54.6。

值得注意的是，兩個領導模型採用的推理強度不同，Fable 5為low、Opus 4.8為medium；相關結果也來自Cognition自建的評測與Devin Fusion架構，尚未經第三方獨立驗證。

若比較兩種Sidekick配置，Fable的成本比Opus低約8.8%，分數則高出6.1分。若改以純Fable作為比較基準，Fable搭配Sidekick後，平均每次成本由4.03美元降至1.86美元，減少54%，得分則僅由60.8微降至60.7。

**也就是說，當Fable 5有了助手後，每次成本大幅減少，分數卻幾乎沒掉。** 這個結果也讓Joon Lee大感訝異。

Cognition進一步分析3,000個評估工作階段中的模型呼叫紀錄，發現兩個領導模型使用的是同一個Sidekick，委派次數也相近，平均每次任務都約交辦3次。這排除了「Fable只是委派得更頻繁」這項解釋。

由於兩者使用同一個Sidekick，委派次數也接近，Cognition因而將主要差異歸因於領導模型何時委派、如何交辦，以及如何驗收成果。團隊用職場情境比喻：Opus像是緊盯實習生的微管理主管，Fable則像是信任能幹工程師、懂得委派工作的經理。

Cognition認為，這項差異反映在領導模型處理任務所需的回合數與token用量上。Fable領導模型平均使用11.5個回合，Opus則需要26.5個回合；兩者輸出token分別約為6,100與19,000，Fable約為Opus的三分之一。

在81%的Fable主導測試中，領導模型未親自修改任何一行程式碼；Opus主導組只有24%符合這種情況。另有13%的Fable主導測試中，領導模型自己從頭到尾未讀取任何程式庫檔案，而是將相關工作交由Sidekick處理。

簡單來說，在Cognition觀察到的典型案例中，Fable會在任務早期就把大部分工作交給Sidekick；Opus則常先自行探索、設計與實作20至45個回合，最後才把部分收尾工作交給副代理。

Cognition也觀察到，Fable的交辦說明通常會列出限制條件、邊界情境及完成標準。例如在一項雜湊（hashing）任務中，Fable撰寫了一份規格完整的設計文件，明確要求演算法不得隨指標長度增加而延長執行時間；Sidekick依照文件完成實作後，在該項任務取得94分。相較之下，Opus選擇親自實作，過程中漏掉這項限制，最後取得25分。不過這是Cognition挑選出的單一案例，並非兩個模型的整體平均成績。

> 延伸閱讀： [Fable 5怎麼用？官方教你這樣下提示詞，才用得出最強能力](https://www.bnext.com.tw/article/91415/claude-fable-5-guide)

## 未來值得付高價的，是「當主管」的判斷力

管理能力的差異還體現在成果交回來之後。兩位主管都會做同樣的便宜檢查，但Opus明顯更不放心：它把副手的檔案拉回自己脈絡重看的次數多了2倍，親自動手修改的次數更多了4倍，極端時甚至整個推翻副手成果、自己重寫一遍。

不過，至少在Cognition檢視的部分評測任務中，Opus投入更多成本重新檢查與修改，並未展現更高的正確率。Fable有時只檢查一次程式碼差異，就能發現Sidekick的錯誤，再透過一次成本較低的委派完成修正。

當然，委派不是萬用解法。Cognition指出，遇到難以拆解的工作，例如只需少數回合即可完成的短任務，或必須依賴一連串連續判斷、累積脈絡本身就是解題關鍵的序列式除錯，Fable幾乎也不會委派。懂得分工的另一面，是判斷哪些工作不適合交給其他代理。

![AI工程師.jpg](https://image-cdn.learnin.tw/bnextmedia/image/album/2025-07/img-1752468370-18298.jpg?w=1200&output=webp)

圖／ ChatGPT

團隊據此認為，隨著Sidekick模型越來越便宜、能力越來越強，可被交辦的工作也會增加。未來值得為高階模型支付更高價格的，可能不只是它們親自執行工作的能力，更包括判斷該做什麼、設定哪些限制，以及將工作交給誰的能力。換句話說，模型能否扮演好「主管」，可能逐漸成為影響AI代理成本與品質的重要因素。

> 延伸閱讀：  
> [Claude金融Agent怎麼用？10款AI代理搞定提案簡報、財報對帳，安裝教學一次看](https://www.bnext.com.tw/article/91588/anthropic-claude-financial-agents-guide)  
> [Claude模型怎麼選？Fable、Opus、Sonnet、Haiku一表看懂，選錯可能會多花10倍成本](https://www.bnext.com.tw/article/90667/claude-model-comparison-opus-sonnet-haiku)

資料來源： [Cognition官方部落格](https://cognition.com/blog/making-fable-cheaper-than-opus)

本文初稿為AI編撰，整理．編輯/陳建鈞