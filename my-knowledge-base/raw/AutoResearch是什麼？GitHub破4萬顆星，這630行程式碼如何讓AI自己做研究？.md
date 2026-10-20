---
title: "AutoResearch是什麼？GitHub破4萬顆星，這630行程式碼如何讓AI自己做研究？"
source: "https://www.bnext.com.tw/article/90349/auto-research"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-03-20
created: 2026-06-04
description: "OpenAI創始團隊成員、前特斯拉AI總監安德烈．卡帕西近日公佈了開源專案AutoResearch，讓AI可以自主做實驗，不斷優化成果。"
tags:
  - "clippings"
status: processed
---
OpenAI創始團隊成員、特斯拉前AI總監安德烈．卡帕西（Andrej Karpathy）在GitHub上發布了一個開源專案：AutoResearch。這個僅約630行Python程式碼的工具，卻展現出讓AI可以自行生成、執行與優化實驗的獨特能力。

透過這種新工具，不必再透過研究人員一個指令、一個步驟引導AI生成成果，審查成果品質後決定下一步做什麼，而是建立一套系統讓AI負責整個流程且不斷重複：制定假設、設計實驗、執行、評估成果，以及決定是否進行下一次迭代。

## 一上線就爆紅，能自己做實驗的AutoResearch厲害在哪？

依照卡帕西的描述，基本上就是為AI設計一個目標，然後讓它工作一整晚。當你醒來時，AI已經完成數十次實驗，評估出哪些實驗有效，並篩選出最佳結果，研究人員無需在每一次實驗後重新發號施令、調整研究方向。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

![andrej karpathy x auto research.jpg](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-03/5z29-1773882353.jpg?w=1200&output=webp)

卡帕西貼出的AutoResearch實驗結果，進行了83次實驗並找到15個改進點，成功讓模型變得更有效率。

圖／ X

過去其實就已經存在自動化機器學習（AutoML）工具，能幫助研究人員搜尋最佳模型架構或參數，但仍需要人工設計搜索空間並逐一啟動實驗。

AutoResearch的強大之處不僅僅是自動化，而是展現了AI自主迭代的潛力。卡帕西將整個繁瑣的過程交給AI，讓AI自行生成新的實驗，執行訓練並檢驗成果決定下一輪要怎麼改進。這就像給AI一個遊戲目標，它不斷嘗試不同策略，自己學會哪種方法效果最好。

AutoResearch專案在GitHub上線至今，已獲得超過4萬顆stars（類似按讚），以及5,600次forks（建立一份副本，代表有改造、實驗的價值），在開發社群內已獲得熱烈迴響。

卡帕西之所以能推出這樣的專案，是累積了多年在神經網路訓練、實驗設計與工具化方面的經驗。他此前開源過一系列被開發者廣泛學習和使用的極簡深度學習工具，例如nanoGPT、nanochat 等，而AutoResearch則是這種極簡及注重實用性理念的又一次嘗試。

## AutoResearch怎麼運作的？

那麼，AutoResearch究竟是如何運作？人類研究員首先提供方向，例如「我希望模型在語言預測上更準確」，並設定一個衡量標準，例如預測錯誤率。AI 接收到這個目標後，就開始自主迭代：

**1.生成假設：** 它修改程式碼，比如模型的層數或學習速率。  
**2.執行實驗：** 在電腦上跑訓練，測試哪種組合效果最好。  
**3.評估結果：** 根據預設的指標決定哪些改動有效，哪些需要捨棄。  
**4.下一輪迭代：** 保留有效改動後，再生成新的假設，繼續測試。

這個過程持續進行，形成一個 「生成→測試→評估→優化→再生成」的循環。AI不再只是被動執行命令，能夠在你設定好規則後，自行完成實驗改進。就像在科學實驗室裡有一位永不疲倦、能自己做試驗的小助手。

另外，AutoResearch設定的訓練時間正好是5分鐘，無論模型的規模，或是新的架構，都是只跑5分鐘，透過規範時間讓各個實驗的結果可以公平比較。

並且使用AutoResearch的硬體門檻較低，只要有單個輝達GPU即可運行，讓預算有限的個人研究者依然可以受惠這項新專案帶來的便利。

> 延伸閱讀： [簡報也能搞定！10大NotebookLM進階提示詞拆解：如何一步步提問，召喚超強AI研究助理？](https://www.bnext.com.tw/article/90338/how-to-ask-questions-effectively-in-ai-era)

## 2天內做700次實驗，找到20項改善點

而在卡帕西的實際使用中，他嘗試利用AutoResearch來改良他以前花費大量心力調整、優化的nanochat。短短兩天內，AI執行了約700次實驗，發現了20項可以改善的要點。而這些改善疊加起來，將用nanochat訓練到性能接近GPT-2所需的時間，從2.02小時縮短至1.8小時。

儘管看似不多，這卻是建立在一位擁有10多年經驗的AI大神、為這個專案優化無數次的基礎上，AI仍能在短短兩天內取得11%的提昇幅度。卡帕西表示，AutoResearch在過程中發現了當時他疏漏的改良點。

![toki lutke x.jpg](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-03/v6f9-1773882354.jpg?w=1200&output=webp)

Shopify執行長嘗試用AutoResearch優化自家的核心模板引擎Liquid，並測得最高達53%的效能提升與61%的記憶體優化。

圖／ X

[Shopify](https://www.bnext.com.tw/article/90362/shopify-ai-transformation) 執行長托比．路特克（Tobi Lütke）同樣高度關注AutoResearch。路特克便實際用AutoResearch進行了一項小實驗，自己跑去睡覺，結果醒來後發現，一個8億參數模型在基準測試中的得分，比他手動調整的16億參數模型要高19%。

在另一個獨立實驗中，路特克嘗試讓AutoResearch優化Shopify的核心模板引擎Liquid，並測得最高達53%的效能提升與61%的記憶體優化，但他同時指出結果可能存在過度針對測試情境調整的情況。這個結果顯示AutoResearch的潛力不侷限於AI研發領域，各種技術迭代都能依靠它完成。

「奇點已經開始了，種種跡象都這麼顯示。」路特克在轉推卡帕西關於AutoResearch的推文中直言。

而對AutoResearch的下一步，卡帕西表示，他希望建立像研究社群的平台，讓世界各地的AI代理能夠彼此交換研究成果。不過，各個使用者的設備不盡相同，在限定5分鐘訓練的條件，要如何統整不同硬體產出的研究成果，可能不是一個簡單的問題。

AutoResearch的問世，也代表著研究人員身分出現轉換，從過去親自埋首實驗，盯著參數與實驗結果，轉變為設定實驗方向，用自然語言與AI溝通執行的策劃者，這或許很類似軟體工程師正因為Claude Code等AI代理而面臨的工作變化。

> 延伸閱讀：  
> [一封備忘錄，把一間電商公司變成AI公司！Shopify如何讓8千名員工「強制愛上AI」？](https://www.bnext.com.tw/article/90362/shopify-ai-transformation)
> 
> [Claude Code問世，第一個被改變的是做出它的人！Anthropic內部揭秘：工程師重點工作「不是寫程式」了](https://www.bnext.com.tw/article/90340/ai-agent-software-engineer)

資料來源： [Data Science Dojo](https://datasciencedojo.com/blog/karpathy-autoresearch-explained/) 、 [X](https://x.com/karpathy/status/2030371219518931079/photo/1) 、 [Phil Schmid](https://www.philschmid.de/autoresearch)