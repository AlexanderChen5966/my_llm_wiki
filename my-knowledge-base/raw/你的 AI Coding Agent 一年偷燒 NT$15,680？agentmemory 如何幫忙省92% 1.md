---
title: "你的 AI Coding Agent 一年偷燒 NT$15,680？agentmemory 如何幫忙省92%"
source: "https://vocus.cc/article/6a11041ffd89780001b1f8a7"
author:
  - "[[Slow Signals]]"
published: 2026-05-26
created: 2026-06-23
description: "本文深入評測 GitHub 爆紅的開源工具 Agent Memory，探討其如何解決 AI Agent 的「失憶症」問題。透過將記憶從 Agent 本體拆離，建構成獨立、可共享、可檢索的本地基礎設施，Agent Memory 有效降低 token 消耗高達 92%。"
tags:
  - "clippings"
status: processed
---
> 16.2K⭐ GitHub、npm 月下載 37K、Reddit 爆紅的開源工具實測：92% fewer tokens、跨 8 種 agent 共用同一份記憶，為什麼它和 mem0 走的不是同一條路。

上一篇我寫了 CodeGraph，講的是怎麼讓 AI agent 進 repo 之後不要像第一次來，少走一些

```
grep
```
、
```
glob
```
、
```
read
```
的冤枉路。CodeGraph 解決的是「它看不懂地圖」；今天這篇要補上的，是另一個更常見也更貴的問題： ****它根本記不得昨天發生過什麼。****

如果你最近常用 Claude Code、Codex、Cursor 或 Hermes，大概都感受過這種失憶症。今天早上你才花了二十分鐘解釋專案結構，說明前端是 React、後端是 FastAPI，為什麼你拒絕 Redux、為什麼某條 migration 不能重跑。下午換一個新 session，它又像第一次見面一樣，重新問你一次。

這件事表面上只是麻煩，本質上其實是成本。你不是只在重複貼上下文，你是在反覆支付本來不該再付的 token——還有更難量化的那種心力成本：每次都要重新把自己教過的事再教一遍。

agentmemory 的切入點很簡單，也很狠： ****既然 agent 的記憶會蒸發，那就把記憶從 agent 本體裡拆出來，變成一層獨立、可共享、可檢索的本地基礎設施。****

![zoomable](https://images.vocus.cc/f8793169-fc2b-427d-869f-8349d95875c8.png)

---

## 先講結論

agentmemory 值得認真看的原因，不是它又是一個熱門 open source repo。

而是它把一個你早就感覺到、但很少有人說清楚的問題，直接搬到桌面上：

****AI agent 不是不會記。它是每次都重新開始記。****

過去這幾年，業界處理「AI 記憶」的主流答案，大概可以分成三派：

- 把 context window 一直加大——但成本變天文數字，且 token 越多召回越糟
- 用摘要式記憶——但摘要會丟細節，最關鍵的那條規則常常剛好被摘掉
- 接一個 vector DB——但純向量檢索對字面化的資訊（config key、錯誤碼、檔案路徑）表現很差

agentmemory 給的答案不是「再加一個更大的 context」，而是換一個方向想： ****既然 agent 的記憶會蒸發，那就把記憶從 agent 本體裡拆出來，做成一層獨立、可共享、可檢索的本地基礎設施。****

這個切法的厲害之處，不在於它發明了新技術。它用的是現成元件：BM25、向量搜尋、graph、SQLite、本地 embedding。

它的厲害之處在於—— ****它是第一個把「跨 session、跨 agent、本地優先」這三件事同時做齊的工具。****

而這三件事，剛好對應每天用 AI 寫程式的人最痛的三個地方：

1. 你重講過的偏好，下次開新對話又要再講一次
2. 你在 Claude Code 教過的事，切到 Codex 又要從頭來
3. 你不想把專案結構、決策、踩雷紀錄上傳到任何雲端

如果這三點任何一條打到你，往下看會有收穫。

---

## 你每年替 AI 的失憶症付了多少錢？

agentmemory 官方 benchmark 對這件事有一個很適合拿來當標題的算式。

README 的 token efficiency 表把幾種使用方式放在一起比較：

- 把完整歷史一直貼進 context：19.5M+ tokens/年，最後甚至超出 context window
- 用 LLM 摘要式記憶：大約 650K tokens/年，約 ****NT$16,000/年****
- 用 agentmemory：大約 170K tokens/年，約 ****NT$320/年****
- 用 agentmemory 搭配本地 embedding：大約 170K tokens/年， ****NT$0****

（以 1 USD ≈ NT$32 換算 agentmemory README 原始美元數據，方便台灣讀者抓尺度——實際數字會隨匯率與你的 token 用量浮動。）

其中最亮眼的數字是 ****92% fewer tokens**** 。如果你本來靠摘要、手動補充背景、或把舊對話重貼一遍來維持工作連續性，那 agentmemory 的論點就是：你不是非得把所有過去都塞回 prompt——你只要在需要時，取回跟當下任務真正相關的那一小段記憶就好。

這裡有一個很重要的觀念差別。

很多人把「AI 記憶」理解成把內容存下來。但真正有價值的不是儲存，而是 ****可召回**** 。你專案裡 300 次零碎互動，不會因為全都存在 SQLite 就突然變成可用知識。真正關鍵的是，下次你問「上次為什麼不用 Redux？」或「那個 rate limit 是 100/min 還是 60/min？」時，系統能不能把正確、足夠近、可信度夠高的片段撈出來——而不是把整個垃圾堆翻給你。

也因此，agentmemory 賣的其實不是「儲存容量」，而是 ****把上下文重新壓縮成可檢索的基礎設施。****

### 一個可以帶走的判斷句

如果這段你只能帶走一句話，我會推薦這句：

> ****儲存不是記憶。可召回才是記憶。****

下次再看到任何標榜「AI memory」的工具，你只要問它一個問題：「我塞 300 條進去，你撈得回我此刻最相關的那 5 條嗎？」

撈得回——它是記憶。

只是把資料寫進 SQLite 或 vector DB——那只是儲存。

這兩件事差很多。整個 AI memory 產品線過去兩年的混戰，本質上就是在卡這條線。agentmemory 之所以值得單獨拉出來講，就是因為它的整套設計——hybrid search、結構化條目、confidence、lifecycle——都是繞著「可召回」這一個字打磨的，不是繞著「能存多少」。

## agentmemory 是什麼？

一句話說完： ****agentmemory 是給現有 AI coding agent 裝上的持久記憶層。****

它不是像 OpenHuman 那樣要你整套工具跟著換掉，也不是叫你遷移到一個全新的 agent runtime。你的 Claude Code 還是 Claude Code，Codex 還是 Codex，Hermes 也還是 Hermes。差別只是，這些 agent 開始共用同一套可持久化的記憶服務。

這個定位很重要，因為它大幅降低採用門檻。多數開發者不想再學一套新代理框架，只想讓眼前已經在用的工具變得沒那麼健忘。agentmemory 正好切在這個需求上：不是替代 agent，而是升級 agent。

官方 README 目前列出的支援清單相當廣，包含 Claude Code、Codex CLI、Hermes、Cursor、Gemini CLI、OpenClaw、OpenCode 與其他 MCP client。這代表它真正有吸引力的地方，不只是「單一工具有記憶」，而是 ****跨工具共享同一份記憶。****

想像一個很實際的工作流：

- 早上你用 Hermes 討論架構決策
- 下午切到 Claude Code 實作
- 晚上再用 Codex 做收尾和測試

如果每個 agent 都是孤島，你一天會重講三次。若它們背後連的是同一個記憶層，決策、偏好、踩雷紀錄和專案規則就不必在每個 session 重生一次。

這就是 agentmemory 真正站得住腳的地方： ****它把「多 agent 工作流」從口號變成可操作的工程配置。****

## 數字說話——但先把話說清楚

agentmemory 現在最容易被引用的一組指標，是 LongMemEval-S 的 retrieval 成績：R@5 95.2%、R@10 98.6%、MRR 88.2%。

不過這裡我要先替它把話說嚴謹，因為這一段很容易被寫壞。

```
benchmark/LONGMEMEVAL.md
```
明確寫了幾件事：

- 使用的是 ****LongMemEval-S**** ，500 個問題、每題約 48 個 sessions
- 這是 ****retrieval-only evaluation****
- ```
	recall_any@K
	```
- ****不是**** LongMemEval 官方端到端 QA leaderboard 分數

也就是說，95.2% 這個數字可以拿來說「它在 LongMemEval-S 的檢索召回很強」，但不該偷換成「它在 ICLR 2025 官方 benchmark 的完整問答準確率就是 95.2%」。這兩件事不是同一件事。

這麼寫不只更準，也更有說服力。因為它代表 agentmemory 的作者至少把方法論寫出來了，沒有把 retrieval 分數偽裝成 end-to-end 神話。

同一份官方文件也給了另一個有意思的對照：BM25-only fallback 的 R@5 是 86.2%，混合檢索之後到 95.2%。這代表它不是單純靠 keyword search，也不是全押 vector，而是兩者混合後才把召回撐上去。

社群面上，agentmemory 也已經不是沒人看的小專案。根據 Reddit

```
r/WebAfterAI
```
的爆文紀錄，那篇〈7 GitHub Repos That Replace $1,380/Month in AI Subscriptions〉是在 ****2026 年 5 月 17 日**** 發出，截至 ****2026 年 5 月 22 日**** 已達 ****461**** 分。這代表它不是只在自己的 README 裡自嗨，而是真的進入了開發者社群的話題流。

如果你要一句話總結這段數據，那會是：

****它不是學術排行榜冠軍敘事，而是一個方法論相對透明、社群熱度也開始外溢的工程型工具。****

## 它怎麼做到的？

agentmemory 的底層不是傳統那種「有向量庫就叫 memory」的做法。官方比較表與 README 都把它描述成 ****BM25 + Vector + Graph**** 的混合搜尋，並提到使用 ****RRF fusion**** 做結果融合。這個架構的意義在於，它不是把所有希望都押在語意向量上，而是讓關鍵字、語意近似與關聯圖譜一起參與召回。

對工程使用場景來說，這比單純的 RAG 更合理。因為開發工作裡有些資訊是語意近似，有些則是高度字面化：

- 錯誤訊息、config key、ENV 名稱——通常 keyword 更強
- 架構決策、偏好、踩坑經驗——通常 semantic 更強
- 某段規則和某個檔案、某個 session、某個事件的關係——則需要 graph 幫忙接起來

### 用一個真實查詢跑一遍，會比較有畫面

假設你下午切到 Claude Code，問了一句：

> 「我們專案上次為什麼決定不用 Redux？」

如果接的是傳統純向量檢索的 memory，會發生什麼事？它會找出語意上「跟 Redux 有關」的句子一大堆，但你三個月前那句「禁用 Redux，原因是 SSR hydration mismatch」很可能被埋在第 20 條之後。

接 agentmemory 之後，這個查詢會同時跑三條路：

- ****BM25**** ：抓到字面包含「Redux」「禁用」「state management」的記憶條目
- ****Vector**** ：抓到語意相近的條目，例如你過去提過「全域狀態管理」「reducer 不要用」
- ****Graph**** ：把「Redux 決策」這個節點連到當時關聯的 session、檔案、PR 編號、踩雷紀錄

三條路各自給出一份分數排名，RRF fusion（Reciprocal Rank Fusion）再把這些排名重新加權合成，最後丟回給 agent 的，是一份「三方都覺得相關」的精選清單。

所以你問「上次為什麼不用 Redux」，它撈回的不只是「禁用 Redux」這條結論，還能順手帶上「2026 年 3 月那次 SSR hydration mismatch 的踩雷脈絡」和「當時你說『下次有人提議用 Redux 我會直接 close PR』那條偏好」。

這個就是 hybrid + RRF 的差別。

純向量檢索給你「相關」的東西。

hybrid + graph 給你「相關 + 可追溯 + 有脈絡」的東西。

對 coding agent 來說，後者才是真的有用。

此外，它也不是把記憶當成一坨原文亂存。官方描述裡提到結構化記憶條目、confidence、lifecycle、source provenance、audit trail 等概念，代表它試圖把「我記得這件事」變成「我知道這件事來自哪裡、可信度多少、該活多久」。

在部署層，agentmemory 最大的友善點是 ****本地優先。**** README 與 benchmark 文件都反覆強調：

- 可使用本地 embedding 模型
	```
	all-MiniLM-L6-v2
	```
- 不需要 API key
- 不需要外部 DB
- 預設狀態存放走 SQLite

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4b6ebcfa-0053-42df-a886-8cbc3774b3f7.png&width=740&sign=ChbmqHodXK5k7H-ilYItf6QacWoCiWqDWJkc5f7rj1Y) ![zoomable](https://images.vocus.cc/4b6ebcfa-0053-42df-a886-8cbc3774b3f7.png)

至於它文件裡一直出現的

```
iii-engine
```
，這裡也值得拆開講。

```
AGENTS.md
```
，agentmemory 是建立在 iii-engine 的三個原語上： ****Worker / Function / Trigger。**** 也就是說，iii-engine 更像是它的運行與觸發底座，負責函式註冊、trigger 與狀態管理；真正讓記憶好用的檢索邏輯，則是上層的 hybrid search、結構化觀察與記憶生命周期策略。

這件事有兩層意義：

第一，它不是一個單純的 SQLite 小腳本，而是有明確 runtime 分層的系統。

第二，也別把 iii-engine 誤認成神奇檢索演算法本身。你真正該記住的是： ****agentmemory 的體驗來自本地狀態、混合搜尋和記憶治理三者的組合，不是只因為它用了某個 engine。****

## 它和 mem0、Letta、Khoj 有什麼不同？

很多人第一眼會把 agentmemory 跟 mem0 放在一起比，這很合理，因為兩者都在打 memory layer。

但截至 2026 年 5 月，這兩條路線其實不太一樣。

mem0 現在確實不是「完全不能本地」。它的 GitHub README 已經有 ****Self-Hosted Server**** 章節，也提供

```
docker compose
```
方式啟動；但同一份 README 也寫得很清楚，它的預設 LLM 是 OpenAI，預設 embedding 是
```
text-embedding-3-small
```
。換句話說，mem0 雖然有自架路線，但整體產品敘事仍較偏向 API 與平台化整合。

相較之下，agentmemory 的賣點更集中在：

- 預設 local-first
- SQLite 即可落地
- 不綁單一 agent runtime
- 強調多 agent 共用
- 不需要額外的向量資料庫

這不代表 mem0 比較差，而是目標族群不同。mem0 比較像「企業記憶 API / 平台」，agentmemory 比較像「給每天都在 terminal 跟 coding agent 混熟的開發者用的本地記憶層」。

Letta（前 MemGPT）則又是另一類。它更像完整 agent runtime，需要你接受它的記憶模型與運作方式；如果你要的是「讓我現有的工具立刻變聰明」，採用成本通常比 agentmemory 高。

Khoj 也值得放進來，但它更偏向個人知識搜尋與 second brain，而不是專門為 coding agent 的跨 session、跨 agent 記憶協作設計。

至於 Hippo，agentmemory 官方 comparison 把它定位成一個 ****biologically-inspired memory system**** ，主打 decay、consolidation、sleep 和 multi-agent shared memory。這個描述是有脈絡的，因為 Hippo 自己的 repo 也確實強調「forget by default, earn persistence through use」這種半衰期式記憶哲學。

所以如果你快速對照一下定位差異：

- 要平台化 API 與雲端整合——偏向 mem0
- 要完整 agent runtime——偏向 Letta
- 要個人知識搜尋——偏向 Khoj
- 要生物學式記憶哲學——偏向 Hippo
- 要本地、跨 agent、工程採用門檻低——agentmemory 目前最對味

## Hermes 使用者為什麼特別該注意它？

因為它不只是「理論支援 Hermes」，而是 README 直接列出 Hermes Agent 的接法。

官方說明裡，Hermes 可以透過 MCP server 接上 agentmemory，還能進一步使用 deeper 6-hook memory provider integration。翻白話就是：它不只是讓 Hermes 查得到記憶，而是打算讓 Hermes 在 session lifecycle 裡，把記憶抓取、注入、寫入做得更自然。

這對 Hermes 使用者的意義非常直接：

- 你的主人偏好可以跨 session 留下來
- 專案規則不必每次重講
- 上次為什麼做某個技術決策，可以在新對話裡被找回
- 同一份記憶還能被別的 agent 共用，而不是 Hermes 專屬黑盒

如果你手上本來就同時用 Hermes、Claude Code、Codex，那這種跨工具共享的效益會比單一工具使用者更大。

## 三步驟裝起來

如果你只想先試，再決定要不要把它納入日常工作流，最短路徑其實很短：

```javascript
npm install -g @agentmemory/agentmemory
agentmemory
agentmemory connect claude-code
```

官方文件另外提供

```
demo
```
模式，可以快速種一批範例 session，直接驗證召回效果。對第一次接觸的人來說，這比讀一堆抽象架構說明更有效——你會立刻理解它不是在賣「記憶感」，而是在賣「可搜尋、可回放、可跨 session 的工作紀錄」。

目前 GitHub main 的

```
package.json
```
版本是 ****0.9.21**** ，也代表它仍處於快速迭代期。這種工具的正確打開方式不是「相信它永遠穩」，而是： ****先在自己的工作流裡跑起來，確認它真的替你省下重複解釋的成本，再決定要不要長期收編。****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F2c71f552-75a2-4c50-ab03-945cab31db20.png&width=740&sign=WMC-cjD4vi8kGLSfhFhijAF-woWFdcD5B1pMVdE7OvQ) ![zoomable](https://images.vocus.cc/2c71f552-75a2-4c50-ab03-945cab31db20.png)

---

## 第一週這樣用最有感

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fff4a5982-3822-4d85-808f-3d07cb0de18d.png&width=740&sign=uVSKW1BDosr_ZVNZCM3N92J3O8OsA-fDJ6FkoVOqLNg) ![zoomable](https://images.vocus.cc/ff4a5982-3822-4d85-808f-3d07cb0de18d.png)

裝起來之後，最容易發生的事是——你裝了，但忘了它的存在。

這類工具的特點是「平常感覺不到，需要時才會驚喜」。所以如果你想在第一週就讓自己感受到它的價值，這裡有一條我覺得最有效的 7 天節奏。不是必照做，但可以當參考：

****Day 1：跑 demo，先看它召回長什麼樣****

```javascript
agentmemory demo
```

這個指令會種一批範例 session 進去，然後做一輪檢索。你不用先接自己的 agent，光看示範就能理解「它記下來的記憶是什麼形狀、能撈回什麼」。這比讀任何架構文件都直接。

****Day 2：刻意種一條「你以後絕對會被問」的決策****

接上你最常用的 agent 之後，做一件刻意的事：丟一句明確的決策進去，例如——

> 「請記住：本專案禁用 Redux，原因是 SSR hydration mismatch，2026 年 3 月已決議。」

刻意越具體越好。你是在替未來的自己鋪伏筆。

****Day 3：開新 session 驗證召回****

隔天開一個全新對話，問它：「我們專案有什麼禁用規則？」

如果它能把昨天那條撈回來，恭喜你——你已經親身驗證了 92% fewer tokens 不是行銷話術。

如果它沒撈到，去 Web UI 看一下記憶條目的狀態（confidence、tag）有沒有問題，這也是學習它怎麼運作的最好時機。

****Day 4：打開 Web UI:3111，看記憶的結構****

```javascript
http://localhost:3111
```

這個介面比想像中重要。你會看到記憶條目被結構化成什麼樣子——confidence 多少、lifecycle 在哪、source 從哪來。

看一次之後，你對「結構化記憶」這四個字會有完全不同的感受。

****Day 5：接第二個 agent，驗證跨工具共享****

如果你同時用 Claude Code 跟 Codex CLI，這天把第二個也接上：

```javascript
agentmemory connect codex
```

然後在 Codex 問同樣的問題：「我們專案有什麼禁用規則？」

如果它撈到了 Day 2 你在 Claude Code 種的那條，這就是 agentmemory 整個論述最核心的賣點—— ****跨工具共享同一份記憶**** ——的第一次落地。

****Day 6–7：開始觀察「沒重講」的時刻****

這兩天不用做什麼新事。日常用 agent 寫 code、改 bug、做 PR review。

只是要刻意觀察：哪些以前你會重講一次的背景，這禮拜它自己記得了？

那些瞬間，就是 agentmemory 真正的價值兌現點。

****「沒發生的事」往往比「發生的事」更值錢——你少講的每一句重複介紹，都是 token、是時間、是心力。****

## 誰最該裝？

如果你符合下面任何一條，這類工具都值得認真看：

- 你每天都在用 Claude Code、Codex、Cursor 或 Hermes
- 你會在多個 agent 之間切換工作
- 你很在意資料不要上雲
- 你已經明顯感覺到「重講背景」比真正寫程式更煩

反過來說，如果你目前只用單一 agent、session 都很短、專案規則也不複雜，那它的效益可能還沒大到今天就非裝不可。留著，等你的使用情境走到那一步再回來看也不遲。

---

## 也要老實講風險

我盡量不把工具寫成只有亮點。agentmemory 有幾個你最好在裝之前就知道的限制——不是用來勸退，是用來幫你估算它在你日常工作流裡的真實位置。

****1\. 個人/小團隊維護中。****

作者是 rohitg00，這是一個個人 repo，活躍度沒問題（README、benchmark、AGENTS.md 都還在更新），社群熱度也已經外溢到 Reddit。但它畢竟不是大公司產品。要把它放進公司 critical workflow 之前，比較穩的做法是先在個人專案或非關鍵情境跑幾週。

****2\. v0.9.x 仍在快速迭代。****

目前 GitHub main 的

```
package.json
```
是 0.9.21。這個版號代表：API 還可能微調、行為仍在收斂。建議在
```
package.json
```
鎖定一個版本，不要寫
```
^0.9.0
```
讓它自動跳版本，免得哪天行為突然變了你不知道為什麼。

****3\. 記憶治理仍需人工關心。****

agentmemory 在設計上已經有 confidence、lifecycle、source provenance 等機制，但「這條記憶該不該保留」「這條 confidence 是不是該降低」最終還是需要你去看一眼。它不會自動把錯誤的偏好淘汰乾淨——尤其是當你某天改變主意，舊的決策可能還在記憶層裡擋路。

****4\. Benchmark 是作者自跑。****

LongMemEval-S 的 R@5 95.2% 來自官方

```
benchmark/LONGMEMEVAL.md
```
，方法論寫得清楚（500 問、48 sessions/題、retrieval-only），數字也都可重現。但這還不是第三方獨立驗證。實際效果會隨你的 agent 配置、記憶累積方式、查詢風格而變動。

這四點放在判斷邊上，你拿捏會比較準。不是用來勸退，是讓你裝起來之後不會被任何一個小驚喜嚇到。

---

## 我對這題真正的判斷

agentmemory 最值得注意的，不是今天 16.2K stars，也不是 92% fewer tokens 這個數字。

而是它把一件事說得很清楚：

****AI coding agent 的下一層基礎設施，是讓它記得跨 session、跨工具的你。****

過去這兩三年，AI 工具的競爭很熱鬧，但大部分熱度集中在「模型」這一層——誰的模型更強、context window 更大、推理更準。

但如果你每天都在用這些工具寫 code，你會慢慢發現：模型再強，它依然在每一場對話的起點失憶。你不是輸給模型不夠強，你是輸給「它不記得你」。

而真正能解這件事的層級，從來不在模型裡。它在外面。它要有一層獨立的、可持久化、可跨工具共用的記憶基礎設施。

這個方向不是 agentmemory 發現的。mem0、Letta、Khoj、claude-mem 都在這條路上跑。但他們各自走的位置不太一樣：

- ****mem0**** 走 API 平台路線——適合企業整合，但不太適合不想資料上雲的個人開發者
- ****Letta**** 走完整 agent runtime——需要你把整套架構換掉，採用成本不低
- ****Khoj**** 偏向個人知識搜尋——不專門為 coding agent 跨 session 設計
- ****claude-mem**** 只服務 Claude Code 一個 agent——綁定太深

agentmemory 的位置剛好是這幾個都沒占到的： ****本地、跨多個 agent、不要求你換整套工具、採用門檻就是一行 npm install。****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb0e8e74c-08b4-4025-ae19-a06733d66024.png&width=740&sign=QB8SBrplxo5xLCTOtAE89CQISxo7PuwkxW1G0eKC92U) ![zoomable](https://images.vocus.cc/b0e8e74c-08b4-4025-ae19-a06733d66024.png)

它不一定最後是這個方向的唯一答案。

但它很可能代表了一個方向。

那個方向就是：

****AI coding agent 正在從「會生成的助手」，走向「有結構感知 + 有跨會話記憶的工作系統」。****

CodeGraph 解的是前半（結構感知），agentmemory 解的是後半（跨會話記憶）。

如果這個方向成立，這類工具就不會只是短期爆紅。它們會慢慢變成標配——就像今天你會預期 IDE 有 LSP、預期 git 有 hook 一樣自然。

---

## 結語：CodeGraph 是地圖，agentmemory 是日記

如果把上一篇 CodeGraph 的比喻接過來，我會這樣說：

****CodeGraph 是地圖，agentmemory 是日記。****

前者解決的是 agent 怎麼在 repo 裡找到路，後者解決的是 agent 能不能記住它走過哪些路、踩過哪些坑、做過哪些決策。少掉任何一層，AI coding agent 都還只是「會生成」，很難變成真正可持續協作的工程基礎。

所以如果你現在已經開始同時和多個 agent 共事，agentmemory 值得你至少花一次

```
npm install
```
的成本去驗證。

因為真正昂貴的，從來不是安裝一個工具。真正昂貴的是：你每天都在為同一場失憶，重複付款。

---

## 寫到這裡，剛好想問你一件事

你平常在用 Claude Code、Codex、Cursor 或 Hermes 的時候，應該也有過那種——明明上禮拜才跟它說過的偏好、規則、踩過的雷，今天開新 session 又要從頭講一次——的瞬間吧。

如果你方便的話，留言聊聊你最常被迫重講的那一條規則或背景是什麼。我自己很想看，也對之後在這裡寫東西的方向會很有幫助。

這篇如果剛好對你有用，輕輕點個愛心我就很開心了；想收藏起來慢慢看、或哪天順手轉給同事，那更是我意料之外的禮物。

接下來這裡還會繼續聊類似的工程基礎設施題——AI coding agent 從「會生成」走向「真的能合作」這條路上，還有不少值得拆開來看的工具與想法。

如果你也對這個方向有興趣，那篇出來的時候歡迎回來看看。沒空也沒關係，這裡會一直在。