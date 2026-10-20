---
title: "代理能力躍進！Meta發表Muse Wonder系列模型：性能緊追Claude Opus 5.5，任務成本飆2.8倍"
source: "https://www.bnext.com.tw/article/92595/meta-muse-wonder-frontier-model-launch"
author:
  - "[[李先泰]]"
published: 2026-10-13
created: 2026-10-13
description: "Meta發表Muse Wonder系列前沿模型，主打主動記憶與代理任務執行力。第三方評測其智慧指數達56分、較前代顯著進步，但任務成本高出2.8倍。"
tags:
  - "clippings"
status: "inbox"
---
Meta於美國時間2026年10月12日發表Muse Wonder系列，包含Muse Wonder與Muse Wonder Full兩款模型，定位為「個人與專業AI代理」用的前沿模型，主打能跨對話記住使用者脈絡、遇到卡關會換方法把任務做完。

第三方評測機構Artificial Analysis（以下簡稱AA）測得兩款模型的智慧指數分別為52分與56分，與相隔約6周的前代Muse Spark 1.3（48分）相比明顯躍進；不過使用成本也跟著墊高，Muse Wonder每道評測任務的成本是前代的2.8倍。

## 兩款模型差在哪？Wonder開放API，Full僅限Muse Code訂閱者

Meta在官方部落格表示，公司14個月前開始從頭重建AI研發體系，兩款新模型在旗下GW（吉瓦）級的Prometheus資料中心訓練完成。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

官方強調兩項能力。一是 **Active Memory** ，模型會把使用者的重要脈絡帶進不同對話，並在任務需要時主動調出相關細節；二是 **Active Focus** ，面對困難目標時會嘗試替代做法、排除卡關、依新資訊調整，需要時先向使用者釐清需求，執行重要動作前也會先確認。簡單來說，Meta要的是記得你是誰、交辦後能自己做完的助理。

Muse Wonder自10月12日起可透過Meta Model API與Muse Code使用；Muse Wonder Full則只開放給 [Muse Code](https://dev.meta.ai/products/muse-code) 訂閱者（Meta首款AI程式開發代理工具）。至於一般用戶使用的Muse應用程式，官方只表示Muse Wonder「很快」會導入、優先開放給訂閱者，截至10月13日未見具體時程。

根據AA整理，兩款模型的上下文長度（一次能讀進的內容量）皆為100萬個token（AI處理文字的計量單位），與前代相同；可輸入文字、圖片、影片與音訊，輸出則僅限文字。

## 第三方評測怎麼看？代理任務大躍進，總分仍未登頂

AA在X發文指出，Muse Wonder Full在最高推理強度下，於AA智慧指數v4.3（彙整10項評測的綜合分數）拿下56分，10項評測全數高於Muse Spark 1.3。進步最大的是代理型任務：Terminal-Bench 4.0（在命令列環境完成程式工作）從33%升至57%；衡量知識工作代理表現的GDPval-AA v2.1，Elo積分（類似棋賽排名的相對分數）從1681升到1857。Muse Wonder的智慧指數為52分，Terminal-Bench 4.0成績則從33%升至52%。

依AA統計，Muse Wonder Full在10項評測中有6項擠進前三，其中科學推理類的CritPt以33%排名第一，GDPval-AA v2.1排第二，同屬科學推理類的SciCode以63%排第三。寫程式方面，在AA程式代理指數中，Muse Wonder Full搭配Muse Code拿下68分，與Claude Code搭配Claude Sonnet 5.5同分，高於搭配Claude Opus 5.5的66分；Muse Wonder為63分，前代Muse Spark 1.3只有54分。

不過，Meta官方部落格附上的比較總表，只列了Claude Fable 5.1、GPT-6 Astra與Claude Opus 5.5三款競品，AA同一輪評測涵蓋的GPT-6.1 Sol與Gemini 4 Argon都不在表上。

即使只看這張表，AA智慧指數最高的仍是Claude Opus 5.5的58分，Muse Wonder Full以56分居次。 **Meta宣稱Muse Wonder系列在個人與專業代理任務達到最先進水準（state-of-the-art），但論綜合智慧分數，Muse Wonder Full還沒拿下第一。**

## 定價走高端：每道任務成本是前代2.8倍

Meta官方部落格未列出價格。根據AA整理，Muse Wonder的API定價為每百萬輸入token 4美元（約新台幣128元）、快取輸入0.20美元（約新台幣6元）、輸出20美元（約新台幣642元）；Muse Wonder Full不採每token計價，僅供Muse Code訂閱者使用。

AA指出，Muse Wonder在最高推理強度下，每道評測任務平均輸出約5.4萬個token，比前代少約一成；但單價偏高，加上每道任務平均要讀進769萬個輸入token，每道任務成本達4.52美元（約新台幣145元），是Muse Spark 1.3的1.60美元（約新台幣51元）的2.8倍。

放進AA的比較，這個成本低於Claude Fable 5.1的7.63美元（約新台幣245元）、Claude Opus 5.5的5.98美元（約新台幣192元）與Claude Sonnet 5.5的5.46美元（約新台幣175元），但高於GPT-6 Astra的3.26美元（約新台幣105元）、Gemini 4 Argon的1.99美元（約新台幣64元）與GPT-6.1 Sol的0.72美元（約新台幣23元）。

亦即， **Meta新模型的使用成本，已從前代的低價位拉到僅次於Claude系列的高價位帶。**

安全方面，Meta表示Muse Wonder對化學、生物、放射性與核子（CBRN）武器及網路攻擊濫用的防護是「迄今最強」，同時依「Meta超級智慧擴展框架（Meta Superintelligence Scaling Framework）」保留資安防禦用途的前沿能力。官方稱含更多安全評測的完整報告將「很快」公布，截至10月13日未見釋出。

> 延伸閱讀： [Muse是什麼？實測Meta爆紅AI助理：免費額度大方，3個玩法比ChatGPT更像助理！](https://www.bnext.com.tw/article/92505/muse-tutorial-hands-on)

資料來源： [Meta官方部落格](https://research.meta.ai/blog/muse-wonder) 、 [Artificial Analysis X貼文](https://x.com/artificialanlys/status/2109697083519447118) 、 [Artificial Analysis官方評測文章](https://artificialanalysis.ai/articles/muse-wonder) 、 [《DevOps.com》](https://devops.com/meta-launches-ai-coding-agent-to-challenge-openai-and-anthropic/) 、 [台灣銀行牌告匯率](https://rate.bot.com.tw/xrt/quote/2026-10-13/USD)

本文初稿為AI編撰，整理．編輯/ 李先泰

關鍵字： [＃馬克·祖克柏](https://www.bnext.com.tw/tags/%E9%A6%AC%E5%85%8B%C2%B7%E7%A5%96%E5%85%8B%E6%9F%8F) [＃meta](https://www.bnext.com.tw/tags/meta)

往下滑看下一篇文章

tw\_bnext \[Dynamic Article\]-20261009-08:03

arrow\_forward\_ios

閱讀文章

00:00

00:17

00:44

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/bnext.com.tw/bnext.com.tw-1791533127.653351.mp4?verify=1791868250-t57aDpuSuo%2F1HK3TiLYvrUbRbn2KShX%2FdEhdaR1trs8%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/bnext.com.tw/bnext.com.tw-1791533127.653351.mp4?token=cFh9pMvk1rlaUNxSV0BTjtS_ntl5jwU1M3cx9f3YZuI&amp;expires=1791954650"></video>

即時熱門文章

[1 玻璃基板技術大突破！韓廠攻克填銅瓶頸，半天製程縮至幾分鐘，欣興、群創進度到哪了？](https://www.bnext.com.tw/article/92573/jntc-tgv-glass-substrate-copper-filling) [2 Claude Opus 5.5做影片指令大全！像素風、食譜、3D遊戲到產品宣傳片，10種風格提示詞一次看](https://www.bnext.com.tw/article/92576/claude-opus-5.5) [3 德宏砸12億踩雷、連虧14季，股價竟從9元飆上400元！玻纖布老廠如何翻身？](https://www.bnext.com.tw/article/92584/glotech-5475-glass-fiber-quartz-cloth) [4 Gemini也開始推Skills！常用指令打「/」就能直接叫出來，免費帳號也能用](https://www.bnext.com.tw/article/92466/gemini-skills-how-to-write-free) [5 Muse是什麼？實測Meta爆紅AI助理：免費額度大方，3個玩法比ChatGPT更像助理！](https://www.bnext.com.tw/article/92505/muse-tutorial-hands-on) [6 台股ETF受益人破1800萬，但你真的懂ETF嗎？清大教授拆解4大迷思：這類型別長抱！](https://www.bnext.com.tw/article/92504/active-passive-etf-leverage-risk)