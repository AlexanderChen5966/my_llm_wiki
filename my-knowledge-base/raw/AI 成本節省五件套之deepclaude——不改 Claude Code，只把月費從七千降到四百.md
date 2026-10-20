---
title: "AI 成本節省五件套之deepclaude——不改 Claude Code，只把月費從七千降到四百"
source: "https://vocus.cc/article/6a1576d4fd897800014a887e"
author:
  - "[[Slow Signals]]"
published: 2026-05-28
created: 2026-06-05
description: "一個 shell script，替 Claude Code 換上比較便宜的模型後端。它不神秘，也不是魔法；比較像把每天通勤的車，從喝高級汽油改成比較省油的版本。"
tags:
  - "clippings"
status: processed
---
> 一個 shell script，替 Claude Code 換上比較便宜的模型後端。它不神秘，也不是魔法；比較像把每天通勤的車，從喝高級汽油改成比較省油的版本。

![zoomable](https://images.vocus.cc/39d013d0-a306-4ac3-adbd-e11ec8d9b542.png)

如果你也用 Claude Code 寫 code，我想先問一個有點不禮貌、但很實用的問題：

你上個月的 API 帳單是多少？

如果答案是「我不知道」，先別緊張，我也是這種人。不是不在乎錢，是開發時很容易進入一種狀態：需求先改完、bug 先抓完、帳單晚點再說。晚點通常就變成月底，月底通常就變成「咦，這數字是不是多一位」。

這篇要介紹的 deepclaude，就是一個很務實的小工具。它不取代 Claude Code，也不要求你重學一套工作流。它做的事很單純：在你啟動 Claude Code 前，先把背後的 API 後端換成比較便宜的選項，例如 DeepSeek V4 Pro。

效果很直接：如果你的日常 coding 任務不一定每次都需要 Claude Opus 等級的能力，帳單有機會從每月 NT$7,200 左右，降到約 NT$416。

先說在前面：這不是叫你「別用 Claude」。我自己比較認同的用法是，把 Claude 留給真正需要它的時候；日常大量消耗 token 的任務，交給比較便宜但夠用的後端。省錢不是信仰，省下來的錢能買咖啡，這件事比較實在。

---

## 為什麼突然在意 AI 帳單？

最近有個很有代表性的事件。

2026 年 5 月 22 日，The Verge 報導，微軟 Experience + Devices 部門準備在 6 月 30 日前取消內部多數 Claude Code 授權，並將工程師導向 GitHub Copilot CLI。

表面上看，這像是大型公司常見的工具整合：公司有自己的產品線，也希望內部多用自家工具。這很合理。

但討論區裡不少開發者很快注意到另一個問題：Claude Code 的 token-based pricing，對團隊來說很難預測。個人每月付 Claude Pro 可能還好；但如果是一整個工程組織，每天大量跑 agent、改 code、重構、debug，帳單就會變成一個很難估的變數。

這裡我不想把事情講成「誰打敗誰」。現實通常沒那麼戲劇化。比較貼近真相的說法可能是：Claude Code 真的很好用，所以大家用得多；用得多，成本就開始冒出來。

這也是 AI coding 工具現在最有趣、也最麻煩的地方。

好用的工具會讓你放心多用，但 token 計價會在旁邊安靜計數。它不像訂閱制那樣，每月固定扣一筆就結束。它比較像計程車跳表，而且你一邊趕路一邊看不到表。到目的地時，司機很客氣地說：「到了，這邊請結帳。」你也只能客氣地冒汗。

deepclaude 的價值，就在這個問題上。

---

## deepclaude 做了什麼？

簡單說：deepclaude 不是新的 AI coding agent。

它沒有重寫 Claude Code 的 agent loop，也沒有自己做 bash executor、tool dispatch、檔案操作、工作區管理。這些是 Claude Code 最有價值、也最難做好的部分。

deepclaude 的做法很巧妙：它在 Claude Code 啟動前，先調整幾個環境變數，讓 Claude Code 連到不同的 API 後端。

主要概念是這樣：

- ```
	ANTHROPIC_BASE_URL
	```
	：改成 DeepSeek 或其他供應商的 API endpoint
- ```
	ANTHROPIC_AUTH_TOKEN
	```
	：換成對應供應商的 API key
- ```
	DEFAULT_MODEL
	```
	相關變數：指定要用哪個模型
- ```
	SUBAGENT_MODEL
	```
	：讓 subagent 也走同一個後端

Claude Code 啟動後，仍然照原本的方式工作。你還是在同一個介面裡下指令、看 diff、讓它改檔案。差別只在於，背後回答問題的模型不一定是 Anthropic。

如果用生活化一點的比喻：方向盤、油門、煞車都沒換，你只是把導航路線從「收費高速公路」改成「比較便宜但仍然能到的路」。偶爾真的趕時間，用

```
--switch anthropic
```
就能在 session 中直接切回 Anthropic，不用重啟。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Facb70238-3068-4e48-8d4c-6cd4d367d465.png&width=740&sign=xg_FsdAAbUHl6dAtfHwzfAOuYCX1741hdLSjSD73Noc) ![zoomable](https://images.vocus.cc/acb70238-3068-4e48-8d4c-6cd4d367d465.png)

這就是我覺得它漂亮的地方：它沒有試圖證明自己比 Claude Code 厲害，而是承認 Claude Code 的工作流很好，然後專心解決「後端成本」這一件事。

---

## 帳單差多少？

來看最實際的部分。

deepclaude 預設可用 DeepSeek V4 Pro 當後端。以 2026 年 5 月公開定價粗估，差距大概是：

| 項目 | Anthropic（Opus 級） | DeepSeek V4 Pro | 差距 |
| --- | --- | --- | --- |
| Input（每百萬 tokens） | NT$96（$3） | NT$14（$0.44） | 6.8x |
| Output（每百萬 tokens） | NT$480（$15） | NT$28（$0.87） | 17.2x |
| 每日 0.5M output | 約 NT$240/天 | 約 NT$14/天 | — |
| 每月估算 | 約 NT$7,200 | 約 NT$416 | 約省 94% |

這裡要誠實補一句：這是用 list price 做比較。實務上，如果你透過 OpenRouter 或其他平台使用 Anthropic 模型，價格可能沒有官方定價那麼高，所以實際省幅可能不是 17x，而是更接近 3-5x。

但 3-5x 仍然很有感。

尤其是你已經習慣把 Claude Code 當日常開發工具時，成本不是一次性的，而是每天慢慢累積。一天看起來還好，一個月就有感；一個人看起來還好，一個團隊就非常有感。

我會把 deepclaude 看成「成本分層」工具，而不是「便宜替代品」。

日常任務，例如改小 bug、整理型別、補測試、重構重複邏輯，可以先用便宜後端。遇到需要深度推理、架構判斷、複雜 bug 根因分析時，再切回 Anthropic。

這樣比較像正常的工程決策：不是每次出門都叫商務艙，也不是每件事都要省到最底。該花的錢花，不該燒的 token 就別燒。

---

## 它不只支援 DeepSeek

deepclaude 比較有意思的地方，是它不只是一個「把 Claude 換成 DeepSeek」的腳本。

它支援多個後端，讓你依照任務選擇：

| 後端 | Input (/M) | Output (/M) | 延遲 | 適合場景 |
| --- | --- | --- | --- | --- |
| DeepSeek V4 Pro | NT$14（$0.44） | NT$28（$0.87） | 150-300ms | 日常 coding、重構、一般修改 |
| OpenRouter | NT$14（$0.44） | NT$34（$1.05） | 中等 | 便宜且彈性的日常使用 |
| Fireworks AI | 中 | 中 | 較快 | 互動式 debugging |
| Anthropic | NT$96（$3） | NT$480（$15） | 50-100ms | 關鍵決策、深度推理 |

```
--benchmark
```
```
--cost
```
這類指令，讓你看不同後端在自己環境裡的延遲與價格。這點很重要，因為「便宜」不一定等於「適合」。

如果你在台灣、美西、歐洲，實際延遲可能不同；如果你的任務是快速互動式 debug，等 300ms 可能還好，也可能讓你心裡出現一些不適合公開發表的詞。

所以最好的方式不是盲目切到最便宜，而是替不同任務建立習慣：

- 一般修改：便宜後端先跑
- 卡住的 bug：換較強模型試一次
- 架構決策：用 Anthropic 或你最信任的模型
- 成本快爆：先停下來看帳單，不要假裝沒看到

最後一點很重要。我們都會假裝沒看到。

---

## 誰適合用？

我自己會這樣想。

****適合試試看的人：****

- 你每天用 Claude Code 寫 code，而且 API 月費已經超過 NT$1,600（約 $50）
- 你是 indie hacker、學生、自由工作者，工具費需要精算
- 你的團隊想比較多個模型後端，但不想改掉既有 Claude Code 工作流
- 你大多數任務是日常 coding，不是每次都需要最高階推理

****不一定需要的人：****

- 你 Claude Code 用量很低，每月帳單不到 NT$640（約 $20）
- 你工作的內容高度敏感，公司規定只能用特定供應商
- 你做的是金融交易、安全漏洞分析、複雜分散式系統等高風險任務
- 你對延遲非常敏感，多等 200ms 就會懷疑人生

說真的，工具選擇要看場景，不是看省多少錢。

另外也要注意幾個限制：

- DeepSeek 伺服器位置可能造成較高延遲
- Claude Code 的 extended thinking 不一定能完整對應
- 少數 tool calling 邊緣案例可能有相容性問題
- 公司合規要求永遠優先於個人省錢偏好

如果你只是想省小錢，結果把工作流搞得不穩，那就不划算。省錢的前提是不要把自己省進坑裡。

---

## 它在「AI 成本節省五件套」裡的位置

我把 deepclaude 放在「AI 成本節省五件套」的第一篇，是因為它門檻最低、回報最直接。

整個五件套大概是這樣：

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F3a702c92-8a89-4f26-99a2-9eab1e54a821.png&width=740&sign=25ChqDpP2ezJsElAzMdX9xKZoVuP-PtlWcUcA9X7DwY) ![zoomable](https://images.vocus.cc/3a702c92-8a89-4f26-99a2-9eab1e54a821.png)

五個工具解的是不同層次的問題：

1. ****deepclaude：換後端**** 。不改工作流，先把最貴的 API 呼叫換成較便宜的選項。
2. ****ds4：本地跑**** 。如果你有硬體資源，就把部分任務拉回本機。
3. ****headroom：壓 token**** 。不要只換便宜模型，也要減少不必要的上下文浪費。
4. ****agentmemory：記住上下文**** 。避免每個 session 都重新燒一次 token 解釋背景。
5. ****opensquilla：自動路由**** 。讓系統幫你選擇「夠用且便宜」的模型。

如果只想先做一件事，我會從 deepclaude 開始。原因很簡單：它不要求你改變習慣。對多數人來說，最容易持續的省錢方法，就是不要太麻煩。

人類的意志力很寶貴，不該浪費在每天手動精算 token 上。

---

## 系列其他文章

如果你對另外幾件也有興趣，目前已經有兩篇上線了：

- ****（二）agentmemory——〈你的 AI Coding Agent 一年偷燒 NT$15,680？〉**** ：這篇講持久記憶怎麼幫 agent 記住你的專案背景，省掉每個 session 重新解釋一次上下文的 token。
- ****（三）headroom——〈你的 AI 編碼工具每天都在燒錢，但問題可能不在模型費〉**** ：這篇講 context 壓縮——在 agent 把內容送進模型前先瘦身，benchmark 顯示 tool-heavy 任務可省 47%–92% tokens。

這三篇的發文順序不是按五件套的編號排的，而是按「馬上能用」的程度。不過內容各自獨立，從哪篇開始讀都可以。如果你想抓一條主軸：先 ****換便宜後端**** （這篇）→ 再用 ****記憶省重複的 token**** （agentmemory）→ 最後靠 ****壓縮減少每次對話的 token 量**** （headroom），會最順。

---

## 下一步

如果你現在就在用 Claude Code，我會建議先做三件小事：

1. ****查帳單**** 。去 Anthropic Console 看過去三個月的用量。不是為了責備自己，只是先知道錢花在哪。
2. ****如果月費已經有感，再試 deepclaude**** 。GitHub 上搜尋
	```
	aattaran/deepclaude
	```
	。它是 MIT 開源，主要使用方式是把原本的
	```
	claude
	```
	工作流改成透過
	```
	deepclaude
	```
	啟動。
3. ****保留切回 Anthropic 的選項**** 。日常用便宜後端，關鍵任務切回你最信任的模型。省錢和品質不必二選一。

這篇不是要說 deepclaude 適合所有人。它比較像一個提醒：當 AI coding 工具越來越好用，我們也需要更成熟地管理它的成本。

以前我們優化程式效能，現在也該開始優化 AI 使用成本。畢竟 token 不會自己省下來，帳單也不會因為我們不看它就變小。很遺憾，這方面它還滿有自我意識的。

五件套的最後兩篇——ds4 和 opensquilla——我也會盡快整理出來。目標不是把每個工具都吹成神器，而是幫自己和同樣重度使用 AI coding 工具的人，建立一套比較不容易失控的成本管理方法。

****留言跟我說**** ：你現在每月 AI API 帳單大概多少？有沒有用過換後端、壓 token、本地跑之類的方法？我很想知道大家實際省下來的是錢，還是只省到一點焦慮。後者也算啦，只是不能拿去買咖啡。