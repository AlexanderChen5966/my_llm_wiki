---
title: "Day 04 - 用 LLM 執行 Karpathy 的六條準則 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10409915"
author:
  - "[[iThome]]"
published:
created: 2026-09-17
description: "前言 之前我們已經把 Karpathy 的六條準則分成兩個部分：怕 LLM 弄壞的寫成程式碼，其餘寫成 Markdown 交給使用者定義。今天詳細說明怎麼讓 LLM 去處理 schema/ 裡定義的那..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 4

0

[佛心分享-SideProject30](https://ithelp.ithome.com.tw/2026ironman/side-project-30)

### 為你自己蓋一座會複利的知識庫——WikiBrain系列 第 4 篇

## Day 04 - 用 LLM 執行 Karpathy 的六條準則

- 分享至

## 前言

之前我們已經把 Karpathy 的六條準則分成兩個部分：怕 LLM 弄壞的寫成程式碼，其餘寫成 Markdown 交給使用者定義。今天詳細說明怎麼讓 LLM 去處理 `schema/` 裡定義的那些規則——讀規則、呼叫之前我們已經看過的那六個工具、判斷什麼時候算做完。

我用一個工具呼叫迴圈來處理，七十行出頭，沒有用任何 agent 框架。編纂、對話、健檢三個操作全部跑在它上面，把知識庫與模型串接起來。

我設計了兩條驅動路徑。一條是 WikiBrain 自己的網頁，使用者填一把模型供應商的 API key，按下「編纂」就開始跑。另一條走 MCP，使用者把知識庫接上自己平常在用的 AI 工具——Cursor、Claude Code、Claude.ai、ChatGPT 都行——由那些工具直接讀寫。

今天我們先講第一條路徑。

  
*三個操作從上面進來，分派成兩套供應商實作，最後收斂到同一組六個工具。這一篇由上往下走一遍。*

## 迴圈本身很簡單

一個工具呼叫迴圈的骨架就是這樣：

```ts
const messages = [{ role: 'user', content: userPrompt }];

for (let step = 0; step < maxSteps; step++) {
  const res = await callModel({ system, messages, tools });

  if (res.toolCalls.length === 0) {
    return { finalText: res.text, steps: step };   // 模型不再呼叫工具，結束
  }

  for (const call of res.toolCalls) {
    const result = await exec(call.name, call.input);   // 真的去讀寫資料庫
    messages.push(toolResultMessage(call.id, result));
  }
}
```

這是骨架，不是原始碼——真正的實作要處理串流事件、取消訊號、步數上限與 token 計數，  
[`runAnthropic`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/ai/providers.ts#L51-L77) 二十七行、  
[`runOpenAICompatible`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/ai/providers.ts#L80-L120) 四十一行，上面再加一個四行的分派函式。

剩下的是兩家供應商的格式差異：Anthropic 用 `tool_use` 與 `tool_result` 區塊，OpenAI 相容的介面用 `tool_calls` 與 `role: 'tool'` 訊息。所以有兩套 `callModel` 的實作，共用同一個迴圈骨架。

OpenRouter 走 OpenAI 相容的格式，所以三家供應商只要兩套實作。

## 為什麼不用框架

框架能幫你的是：多 agent 協作、複雜的狀態機、RAG 管線、可觀測性整合、prompt 模板管理。

我需要的是：呼叫模型、執行工具、把結果送回去、記下花了多少 token。

更實際的理由是 **除錯** 。當 agent 行為不對的時候，我要看的是「送給模型的訊息陣列到底長什麼樣」。自己寫的迴圈，加一行 console.log 就看得到。透過框架的話，我得先搞懂它怎麼組訊息、有沒有偷偷加系統提示詞、工具描述被它改成什麼樣子。

但是這個判斷有前提： **如果我需要的是多步驟規劃、子 agent、複雜的記憶管理，答案會不一樣。選擇何種工具，取決於你要做的事有多複雜。**

## 工具直接呼叫資料層

伺服器端 agent 的工具跟 MCP 那六個 **同名同介面** ，但它不走 HTTP，直接呼叫資料存取層的函式。

```ts
// 發動的地方決定作者是誰
void runJob(job, ws, cfg, { system: ingestSystem(lang), user,
  actor: { kind: 'agent', name: \`${cfg.provider}/${cfg.model}\` } });

// runJob 把它交給工具層
const r = await runAgent({ …, tools: TOOLS, exec: makeExec(ws, spec.actor), … });
```

*出處： [`src/ingest.ts:128`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/ingest.ts#L128) 與 [`src/ingest.ts:169`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/ingest.ts#L169)*

那個 actor 參數決定版本紀錄裡的作者欄位。所以你在版本歷史裡看得到「這一版是 openrouter/gemini 寫的」，跟「這一版是 Cursor 寫的」、「這一版是我自己在網頁上改的」分得清清楚楚。

同名同介面這件事很重要：MCP 那頭的 agent 跟伺服器端的 agent 面對的是同一組工具、同一組錯誤訊息、同一份規則。行為一致，我也只需要維護一套。

## 重試

模型 API 會失敗，而且失敗的種類不一樣：

- 429 限流、5xx、逾時、連線中斷 → 重試有意義，退避兩秒、五秒、十二秒，最多三次
- 401 金鑰錯誤、400 請求格式錯 → 重試一百次也不會變好，直接放棄

判斷依據只看錯誤物件上的狀態碼，不要用字串比對錯誤訊息——訊息會隨供應商改版而變。另外要記得把 SDK 自己的重試關掉，否則你的退避策略跟它的疊在一起，一次失敗會等很久。

重試發生時會在工作紀錄裡留一筆註記，這樣事後看得出來「這次跑得比較久是因為被限流過」。

## 記帳

每一步的 token 用量都會累加，工作結束時算成金額，存進工作紀錄。價格表用的是 OpenRouter 的公開價目，三家供應商都用它當參考價。

![設定頁的 AI 供應商區塊：目前模型、API key 欄位，底下是本月與累計用量、依模型分項的單價與費用，以及最近幾次自動編纂的步數、token 與金額](https://ithelp.ithome.com.tw/upload/images/20260913/2018412905yTuBAhAe.png)  
*每一次編纂都留下步數、輸入與輸出 token、以及估算金額。最後那一列是一次失敗的工作——三十六步、$0.03，錢照樣花掉了。*

會做這個是因為使用者用的是自己的 key，錢是他們付的。如果他們看不到花了多少，就不會信任這個功能——「按下去會不會噴掉我五美元」是一個真實的疑慮。設定頁上有本月與累計的統計、依模型分項，進度面板也會即時顯示這一次跑到目前為止花了多少。實測一次編纂大約零點零幾美元。

## 小結

知識庫的規則由 `schema/` 決定，資料怎麼寫由資料存取層決定。今天介紹的中間層負責把模型跟那六個工具接起來，並且把每一步都寫進工作紀錄，讓我們看得懂過程中發生了什麼事。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10409772)

[

Day 03 - 把 Karpathy 的六條準則寫成程式

](https://ithelp.ithome.com.tw/articles/10409772)[下一篇](https://ithelp.ithome.com.tw/articles/10410555)

[

Day 05 - Karpathy 的準則，在 WikiBrain 實現會自我強化的知識庫

](https://ithelp.ithome.com.tw/articles/10410555)

系列文

[為你自己蓋一座會複利的知識庫——WikiBrain](https://ithelp.ithome.com.tw/users/20184129/ironman/9590) 共 7 篇

目錄

1. 3
	[Day 03 - 把 Karpathy 的六條準則寫成程式](https://ithelp.ithome.com.tw/articles/10409772)
2. 4
	[Day 04 - 用 LLM 執行 Karpathy 的六條準則](https://ithelp.ithome.com.tw/articles/10409915)
3. 5
	[Day 05 - Karpathy 的準則，在 WikiBrain 實現會自我強化的知識庫](https://ithelp.ithome.com.tw/articles/10410555)
4. 6
	[Day 06 - 為什麼選 MCP，以及怎麼用無狀態的 Streamable HTTP 實作](https://ithelp.ithome.com.tw/articles/10411041)
5. 7
	[Day 07 - 把提示詞寫進工具描述與錯誤訊息](https://ithelp.ithome.com.tw/articles/10411905)
[完整目錄](https://ithelp.ithome.com.tw/users/20184129/ironman/9590)

[![.](https://itadstatic.ithome.com.tw/B9/1789119962_6aa3cdda3af6d.png)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/6/8/44265d37-4976-42f0-820c-15d996fea475_small.jpg)

[雲端資安如何合規: 5 種常見曝險情境](https://itplus.ithome.com.tw/vod-page/45?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cymetrics |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/5166a320-ace9-4344-af3a-778557b83873_small.png)

[透過 AI 強化 Kubernetes 管理：Anthos 與 Cloud Inference API 的結合](https://itplus.ithome.com.tw/vod-page/868?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/ead58431-65f0-494c-b0bd-a95f3d606b84_small.png)

[導入 OPA 在金融業監控 k8s 部署檔的安全性](https://itplus.ithome.com.tw/vod-page/1054?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/bd4e7a30-73ea-48d2-8eee-3530c39a699b_small.png)

[如何使用 kubeflow 在不同硬體上建立 AI 開發及服務平台](https://itplus.ithome.com.tw/vod-page/1198?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分