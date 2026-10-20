---
title: "如何讓 Fable 比 Opus 更便宜：用委派改寫 Agent 的成本結構"
source: "https://www.blocktempo.com/making-fable-cheaper-than-opus/"
author:
  - "[[flip]]"
published: 2026-07-19
created: 2026-08-05
description: "Fable 像帶著資深工程師的經理，早早交派、寫清楚規格、少親自動手；Opus 則像帶實習生的微觀管理者。本文源自 Joon Lee X 文章，用 3,000 場評測拆解背後的成本結構。（前情提要：Anthropic 推出「Claude for Small Business」：瞄準中小企業 AI 自動化工作，幫你催發票、算薪水..）（背景補充：Anthropic 要求實名 KYC 驗證！Claude 部分功能將需上傳身分證件，合規壓力擴大） 我們把 Opus 4.8 換成 Fable 5，而 Devin 的帳單反而降低了。Fable 5 每 token 的成本是 Opus"
tags:
  - "clippings"
status: processed
---
Fable 像帶著資深工程師的經理，早早交派、寫清楚規格、少親自動手；Opus 則像帶實習生的微觀管理者。本文源自 Joon Lee X [文章](https://x.com/joon_h_lee/status/2076714221837173097) ，用 3,000 場評測拆解背後的成本結構。  
（前情提要： [Anthropic 推出「Claude for Small Business」：瞄準中小企業 AI 自動化工作，幫你催發票、算薪水..](https://www.blocktempo.com/anthropic-launches-claude-for-small-business-ai-automation/)）  
（背景補充： [Anthropic 要求實名 KYC 驗證！Claude 部分功能將需上傳身分證件，合規壓力擴大](https://www.blocktempo.com/anthropic-claude-identity-verification-persona-government-id-required/) ）

> 我們把 Opus 4.8 換成 Fable 5，而 Devin 的帳單反而降低了。

Fable 5 每 token 的成本是 Opus 4.8 的兩倍。但當我們用全新的 Fusion 架構，在 FrontierCode 1.1 上同時跑這兩個模型時，Fable 反而更便宜。不意外地，它的分數也更高。這篇文章會解釋為什麼，以及這對「為 agentic 工作定價」意味著什麼。

### 引言

每個跑程式 agent 的人都知道，更強的模型會給你更好的結果，但你得吞下成本。

當我們推出 Devin Fusion 時，我們展示了一條出路：讓一個前沿模型坐鎮指揮，讓它把工作委派給一個更便宜、更快的副手，於是你能以低 35% 的成本，獲得前沿等級的表現。

但一旦主導模型把大部分工作都委派出去，它每 token 的單價還會主宰整張帳單嗎?Fable 5 每 token 的成本比 Opus 4.8 貴兩倍，所以由 Fable 主導的 agent 照理說應該更貴。為了找出答案，我們在 FrontierCode 1.1 上跑了 3，000 場評測工作階段，橫跨四種配置：Fable 與 Opus 各自坐上主導席，並各自在「有」與「沒有」同一個便宜副手的情況下執行。

純粹的執行(pure runs)表現完全如直覺所料：Fable 的分數勝過 Opus(60.8 對 55.4)，成本也更高。更好的模型，更大的帳單。

有副手加持的執行，才是事情變得有趣的地方。

![](https://pbs.twimg.com/media/HNG9TzBaYAEAUXD?format=jpg&name=large) 在給定同一個副手的情況下，成本排序反轉了：Fable + 副手比 Opus + 副手更便宜($1.86 對 $2.04)，分數卻更高(60.7 對 54.6)。和純粹的 Fable 相比，Fable + 副手把成本砍掉 54%，分數卻幾乎沒變。

| 配置 | 分數 | 每次執行成本(平均) |
| --- | --- | --- |
| Fable 5(low)+ 副手 | 60.7 | **$1.86** |
| Opus 4.8(medium)+ 副手 | 54.6 | $2.04 |
| Fable 5(low) | 60.8 | $4.03 |
| Opus 4.8(medium) | 55.4 | $3.06 |

結果證明，「每 token 貴兩倍」是個看錯了的數字。一個 agent 的成本，主要取決於主導模型走了多少回合、拖著多少上下文一起走，以及最重要的——它決定「不」自己做哪些事。差別歸結到管理風格：Opus 表現得像個帶著實習生的微觀管理者；Fable 則像個帶著能幹工程師的經理。

### 實驗設定

快速複習一下 Fusion 的副手架構如何運作。主導 agent 擁有整個工作階段：它和使用者對話、規劃、審查工作，並提交(commit)。它還有一個常駐的副手子 agent 用來委派任務。主導模型用白話寫下一份交接簡報，而由一個便宜得多的模型驅動的子 agent，在它自己的上下文裡執行，並回報結果。主導模型審查結果，決定接下來怎麼做。

為了找出成本流向哪裡，我們做了兩件事。第一，我們解析了全部 3，000 場工作階段裡的每一次 LLM 呼叫：是哪個模型在說話、它呼叫了什麼工具、讀寫了多少 token，以及每次呼叫花了多少錢。第二，我們挑了 40 個任務做更近距離的觀察：Fable 明顯更便宜的那些、Opus 明顯更便宜的那些，以及另一批來自中間地帶的隨機樣本。對每一個，我們把 Fable 主導的執行和 Opus 主導的執行並排分析，檢視它們的軌跡，觀察錢花到哪裡去了。

### 一個 agent 的成本

以下是在我們的實驗中，成本如何在主導模型與副手之間分配：

|  | 主導 $ | 副手 $ | 每次總成本 $ | 主導每次回合數 | 主導輸入 token(累計) |
| --- | --- | --- | --- | --- | --- |
| Fable + 副手 | $1.28 | $0.58 | $1.86 | 11.5 | 545k tok |
| Opus + 副手 | $1.73 | $0.31 | $2.04 | 26.5 | 1，679k tok |

Fable 花在副手上的錢比 Opus 多——每次執行多花 $0.27。但它花在自己身上的錢少了 $0.45。Fable 的主導每次執行走 11.5 個回合，對比 Opus 的 26.5 個；寫出的 output token 只有三分之一(6.1k 對 19.0k)，消耗的 input token 也只有三分之一。Fable 每 token 明顯更貴，卻在上下文管理和回合數上勝出。

Fable 的 token 節省，來自於它乾脆地避開了工作。有趣的是，在 81% 由 Fable 主導的執行裡，主導模型從頭到尾沒有做過任何一次程式碼編輯。對 Opus 來說，只有 24% 的執行是如此。在 13% 由 Fable 主導的執行裡，主導模型甚至從未親自讀取過任何一個 repo 檔案。

### 帶實習生的微觀管理者 vs 帶資深工程師的經理

讓這個落差變得有趣的地方在於：兩個主導模型委派的次數一樣多，每次執行大約 3 次交接。逐次呼叫的日誌，推翻了「Fable 只是單純委派得比較多」這個簡單解釋。真正不同的，是它們「何時」委派、「委派什麼」。Fable 的第一次交接來得很早。

Opus 則常常很晚才委派，在一長段獨自探索與實作之後；到那時候，設計決策都已做完、重要檔案都已進了它的上下文，昂貴的工作也已經做完了。

![](https://pbs.twimg.com/tweet_video_thumb/HNHusihaIAAnk4B.jpg)

一場典型的、由 Fable 主導的執行，會先對 repo 做幾個偵察動作，然後寫一份規格等級的簡報，把整個「實作 + 測試 + lint」的迴圈一次委派出去。接著一個 git show 來審查 diff，然後提交。

一場典型的、由 Opus 主導的執行，則會歷經 20 到 45 個回合的獨自探索、設計與實作，再加上一次很晚才發生、只交派機械性收尾工作的交接。

有時候，Fable 在一個工作階段裡的第一個動作就是交接。在同一個任務上，兩個主導模型的開場是這樣的：

![](https://pbs.twimg.com/media/HNG8kuLaoAA2LH4?format=png&name=large) 顯而易見的修法，是強迫 Opus 多委派一些探索，但強加這種行為往往會降低表現。知道一項調查什麼時候可以安全地交派出去、什麼時候是你必須親自做的，這本身就是一種判斷。一個被脅迫著去委派的模型，並不會因此獲得那種判斷力；它只會把不該委派的東西委派出去。

每個模型的管理風格，也在交接簡報本身裡顯露無遺。當 Opus 委派實作時，它是在下指令；而 Fable 則是在寫一份設計文件：

![](https://pbs.twimg.com/media/HNG8qCybQAAen4G?format=png&name=large) 委派不只是把成本挪來挪去；它也改變了工作的品質。上面那個雜湊(hashing)任務就是個鮮明的例子。任務規格要求一個雜湊函式在指標(pointer)長度上必須是 O(1)。Opus 親手實作它，卻從沒把這條要求寫在任何地方。過程中的某一步，它忘了這條限制，交出了一個線性時間的實作，得分 25。相對地，Fable 用高層次的限制來委派。它的簡報寫著：「operator() 在指標長度上必須是 O(1)：不得做完整的 token 掃描。」副手成功地實作了它，拿下 94 分。

我們發現這個模式在各個任務間都能通用。Fable 的交接會列舉出各種限制、邊界情況，以及一份「何謂完成」的定義，既替自己省下力氣，又讓副手能便宜且正確地完成實作。

### 交接之後

另一半，是主導 agent 拿副手交回來的成果做了什麼。兩個主導模型常常都會跑同樣的便宜檢查：兩三次 git diff / git show 呼叫。但 Opus 不會停在那裡。它把副手的檔案拉回自己的上下文的頻率高出 2 倍，並以主導模型的價格做出多達 4 倍的修正性編輯。在最極端的情況下，它把副手的成果整個還原，親手重寫了一遍：

![](https://pbs.twimg.com/media/HNG8wyCb0AIoSSF?format=png&name=large) 而 Opus 的不信任，也並沒有讓正確性提高。在某些評測任務裡，Fable 單單一次的 diff 審查就抓到了副手真正的 bug，並選擇再做一次便宜的交接，而不是 Opus 那麼常訴諸的、主導模型等級的重寫。

### 當委派幫不上忙時

Fable 的委派策略並非放諸四海皆有用；當任務沒有可委派的組成部分時，它就會失效。以下這幾類任務，似乎很難被拆解：

- 只包含少數幾個主導模型回合的短任務，在「決定」與「交付」之間沒有東西可以委派。
- 序列式的除錯任務，其根因追查是一長串連續的判斷。在這裡，累積起來的上下文本身就是工作。

值得注意的是，在這些任務上，Fable 幾乎完全不委派。那份能寫出好簡報的判斷力，同樣也知道什麼時候不該寫。但當一個任務沒有任何值得交派的東西時，委派對成本就毫無施力點。

在正式生產環境裡，Fusion 會在另一個層次處理這件事：委派決定哪些工作留在昂貴的模型手上，而路由(routing)則決定昂貴的模型到底要不要被牽涉進來。

### 結語

我們開始這個實驗時，原本預期要衡量的是 Fable 那 2 倍的溢價會讓成本增加多少。結果我們很驚訝地發現，Fable 有效的委派實際上讓整體成本下降了。它指明限制與結果，而不是把實作一步步寫死；它給出回饋，而不是自己動手修；而且在大多數情況下，它根本從未碰過程式碼。這些，都是一個好經理的習慣。

隨著副手模型變得更便宜、更好，更多的工作可以交給它們。而未來仍值得付前沿價格的，將是判斷力：要做什麼、要限制什麼，以及該由誰來寫。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

### 📍相關報導📍

**[Anthropic 開源金融分析外掛：41 個 Skills 一鍵研究股票、財報分析、財富管理](https://www.blocktempo.com/anthropic-claude-financial-services-plugins-41-skills-11-data-providers/)**

**[神救援！他靠 Claude 翻 11 年前舊電腦，成功找回 5 枚 BTC](https://www.blocktempo.com/cprkrn-blockchain-com-wallet-5btc-claude-recovered-old-backup-mnemonic-decrypt-success/)**

**[OpenAI推出Claude Code「搬家工具」，不演了讓用戶無痛跳槽Codex](https://www.blocktempo.com/openai-codex-migration-tool-claude-code-settings-mcp-hooks-import/)**

### 最新文章

---

標普全球將甲骨文信用評等從 BBB 調降至 BBB-，只比垃圾等級高一階，理由直指甲骨文對 OpenAI 的高度曝險與龐大資本支出。分析師裡奇警告，甲骨文可能是超大規模雲端商中第一家轉弱的公司，也可能是股市進入更長熊市週期的早期訊號。  
（前情提要： [甲骨文罕見自曝資料中心「恐無法回本」，Oracle 六月股價重挫 40%](https://www.blocktempo.com/oracle-stargate-openai-data-center-spending-risk-payback-warning-bloomberg/) ）  
（背景補充： [甲骨文大漲9.6%分析師喊「錯殺」：雲服務營收亮眼，但扛 1300 億美元債務隱憂](https://www.blocktempo.com/oracle-stock-surge-cloud-debt-risk/) ）

標普全球在 10 日將甲骨文的 [信用評等從 BBB 調降至 BBB-](https://finance.yahoo.com/markets/stocks/articles/oracle-stock-shrugs-off-p-185346661.html) ，BBB- 是投資等級的最後一道防線，簡單來說就是，只要再降一階，甲骨文的債券就會被市場歸類為垃圾債。同時間，甲骨文股價自去年 9 月以來已崩跌 61%。

不過標普看的不是股價，是背後的資產負債表：甲骨文 2027 財年資本支出預估被上修至 900 億至 950 億美元，自由營運現金流缺口將擴大到負 420 億美元。分析師裡奇在近日發給客戶的報告中直言，甲骨文很可能是超大規模雲端服務供應商當中，第一家開始轉弱的公司。

### 標普為何出手？

標普調降的理由很具體，甲骨文手上握著 6,380 億美元 RPO（剩餘履約義務）。

簡單來說就是，這是已經簽約、但還沒認列營收的合約總額，其中約一半來自 OpenAI 一家公司。標普警告，如果 OpenAI 無法履行付款義務，甲骨文手上握著的，是一批既無法輕易終止、也難以轉租給其他客戶的長期資料中心租約。

這不是假設性風險，而是甲骨文自己 [在檔案中承認過的隱憂](https://www.blocktempo.com/oracle-stargate-openai-data-center-spending-risk-payback-warning-bloomberg/) ，動區 7 月初就報導過，甲骨文罕見自曝資料中心恐無法回本，當月股價重挫 40%。

加上資本支出上修、現金流缺口擴大、調整後槓桿（債務與 EBITDA 之比）逼近 4.5 倍，遠高於 BBB 級門檻該有的水準，標普幾乎沒有選擇，只能調降。甲骨文目前總債務約 1,600 億美元，光是這個數字，就足以讓任何信評機構謹慎以對。

### 債市早就在示警

裡奇的論點核心，不是甲骨文一家公司的財務問題，而是訊號的落差。他在報告中指出，去年甲骨文股價一路上漲的時候，CDS（信用違約交換）利差沒有同步收窄。簡單來說就是，債券投資人早就用價格表達了不安，但股票投資人選擇忽視，繼續追高。

[裡奇在報告中警告](https://www.aol.com/articles/why-oracles-recent-credit-downgrade-173224000.html) ，來自債市的關鍵訊號起初往往細微，但足以提供潛在下行的預先警告。這種落差不是新鮮事，只是這次的主角換成了甲骨文。債市領先股市反映風險並不罕見，等到股價也跟著鬆動，通常代表債市的警訊已經醞釀了一段時間。

### 甲骨文是縮影，不是特例

裡奇的判斷是，甲骨文愈來愈可能象徵股市進入一個更長期迴圈的熊市早期階段。邏輯很直接：一旦資本支出降溫，或超大規模雲端商砸進去的 AI 基建投資沒有帶來預期回報，違約風險就會提高，而甲骨文正是曝險最深的那一個。

這不只是甲骨文一家公司的問題。整個 AI 基建競賽的資金結構，建立在雲端商借錢蓋資料中心、AI 公司簽長約撐起營收的迴圈上。只要其中一環出現裂縫，比如 OpenAI 的付款能力，連鎖反應會直接寫進信評報告裡，而不是股價圖表上。

股市看的是敘事，債市看的是現金流。標普這次調降，某種程度上只是把債市早就知道的事，寫成了正式檔案。甲骨文的信評還沒掉到垃圾級，但市場的問號已經先掛上去了。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

### 📍相關報導📍

**[甲骨文暴跌 40%，AI 過度基建會拖垮巨頭嗎？](https://www.blocktempo.com/oracle-stock-plunge-ai-infrastructure-risk-tech-giants/)**

**[美債 AAA 神話破滅：不是末日，是警訊，更是新開場](https://www.blocktempo.com/the-myth-of-aaa-us-debt-is-shattered/)**

**[美債痛失3A信評》多檔臺灣美債ETF「重摔創新低」債蛙心淌血，後市影響如何？](https://www.blocktempo.com/many-taiwan-us-treasury-bond-etfs-hit-new-lows/)**