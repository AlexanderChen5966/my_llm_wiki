---
title: "AI Agents 背後的骯髒祕密（Demo 🚀）"
source: "https://codelove.tw/@tony/post/qzorQx"
author:
published:
created: 2026-07-24
description: "到現在為止，我一直覺得 AI agent 周圍籠罩著一種神祕的光環。沒有人真的知道它們在做什麼，它們大概很快就要接管世界了；而如果你想自己做一個……嗯，顯然你需要一個框架。如果我告訴你這不是真的呢？你可以在大約 80 行程式碼內做出自己的 AI agent。_我得承認，這週不太好過。工作上發..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月24日](https://codelove.tw/@tony/post/qzorQx "2026-07-24 08:45:09")

到現在為止，我一直覺得 AI agent 周圍籠罩著一種神祕的光環。沒有人真的知道它們在做什麼，它們大概很快就要接管世界了；而如果你想自己做一個……嗯，顯然你需要一個框架。

如果我告訴你這不是真的呢？你可以在大約 80 行程式碼內做出自己的 AI agent。

*我得承認，這週不太好過。工作上發生了很多事。除此之外，我還收到其中一個 CFP 的自動拒絕信。正常來說，這大概只會佔據我五分鐘的心思，因為會議被拒本來就是家常便飯。*

*只不過……那場會議其實是 **邀請我** 去的。「你已經被接受了，只需要在 CFP 裡填交演講細節。」因為那個邀請，我還推掉了另外兩個會議機會。唉，至少我現在九月有空了。😉*

*不管怎樣，生活還是要繼續。今天是我的生日，所以當作送給自己、也送給大家的一份小禮物，我寫了這篇文章。😄 希望你會喜歡！*

## 我們真的需要框架嗎？

直接切入正題。

像 LangChain、CrewAI 或 Mastra 這類框架，並不是在施魔法。它們只是把像是對話記憶、工具執行、重試、備援等事情簡化了。

一旦你理解底層機制，就會更容易判斷什麼時候框架真的值得用。即使最後還是會用框架，你也會知道幕後到底發生了什麼。

所以我決定做一個小型 demo，看看 AI agent 到底只需要多少程式碼。

我用 Node.js 寫了一個 AI agent，大約只用了 **80 行程式碼** ……好吧好吧， **核心迴圈** 大概是 80 行。還是有工具、供應商抽象層，以及一些周邊邏輯。不過老實說， *「80 行寫出一個 AI agent」* 聽起來就是比較厲害。😄

這是專案倉庫：

**[https://github.com/sylwia-lask/code-review-agent](https://github.com/sylwia-lask/code-review-agent)**

而且今天是我的生日……如果你喜歡這個專案，歡迎幫我點個 ⭐。當然，前提是你真的喜歡。😄

## 認識 Steve

這個應用程式是一個簡單的程式碼審查 agent。嗯……其實也不完全是。

認識一下 **Steve** ：一位有 15 年經驗、專門審查別人程式碼的軟體工程師。Steve 不會照單全收。沒錯，他有時候可能有點酸……但他的結論通常很難反駁。

以下是他的一個程式碼審查範例：

[![Steve 的 agent 程式碼審查](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/ze0i895uuvoj4cmvx1ag.png)](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/ze0i895uuvoj4cmvx1ag.png)

[![Steve 的 agent 程式碼審查 - 第 2 部分](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/qesd379zen7j5zswkdof.png)](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/qesd379zen7j5zswkdof.png)

或者當你有空的 diff 時：

[![空 diff 的審查](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/4u5427g12z0ematgrgv1.png)](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/4u5427g12z0ematgrgv1.png)

目前 Steve 會審查本機的 Git diff。這基本上就表示……他在審查 **他自己** 。所以我想，我大概是做出了一個傳說中的自我修復 agent 原型；根據 @nitsancohen770 的說法，這東西有一天會把我的工作搶走。

如你所見，我基本上是在幫自己自動化失業。也許我真的該開始考慮退休了。😄

## 「Agent 就只是一個 while 迴圈」

在我上一篇文章裡，我曾開玩笑說，有些人認為 AI agent 不過就是一個 `while` 迴圈。嗯……我的甚至連 `while` 都不是。它是個 `for` 迴圈，因為我想避免不小心做出無限迴圈，然後在 token 上燒掉太多錢。😄

好吧，公平地說，這個迴圈本身其實沒有真的 *做* 什麼。它只是負責協調流程。不過有趣的是，大多數 agent 框架在底層做的事情其實也非常類似。

這個迴圈本身很容易寫。真正的挑戰，其實就是你猜得到的那一塊。

做 AI agent 的第一步就是……選擇 LLM。😄 這次我選了 Gemini API，因為這類專案的 token 真的便宜到誇張。我確實很想下一版改用本機模型來做，但……先等等。😄

我立刻遇到一個問題：有些 Gemini 模型負載過高，所以我一直收到 **503** 回應。這代表我必須實作一些框架通常開箱即有的功能——簡單的重試機制。

我的版本刻意做得非常基本。正式環境的框架通常還會提供更多功能，例如指數退避、抖動（jitter），或自動切換到另一個模型。

接下來的挑戰當然就是，寫出正確的提示詞。之後，一切就變得出乎意料地順利。

## 這個 agent 到底是怎麼運作的？

比你想的簡單多了。

### 步驟 1：送出提示詞和可用工具

我們送兩樣東西給模型：

- 使用者訊息，
- 模型可以使用的工具清單。

請求長這樣：

```json
{
  "model": "gemini-2.5-flash",
  "contents": [
    {
      "role": "user",
      "parts": [
        {
          "text": "Please review the current git diff."
        }
      ]
    }
  ],
  "config": {
    "systemInstruction": "You are Steve, a senior software engineer with 15 years of experience...",
    "tools": [
      {
        "functionDeclarations": [
          {
            "name": "getDiff",
            "description": "Get the git diff of the current repository...",
            "parameters": {
              "type": "OBJECT",
              "properties": {},
              "required": []
            }
          },
          {
            "name": "getFile",
            "description": "Read a file from the repository...",
            "parameters": {
              "type": "OBJECT",
              "properties": {
                "path": {
                  "type": "STRING",
                  "description": "Path to the file relative to the repository root"
                }
              },
              "required": ["path"]
            }
          },
          {
            "name": "listFiles",
            "description": "List files and directories at a given path...",
            "parameters": {
              "type": "OBJECT",
              "properties": {
                "path": {
                  "type": "STRING",
                  "description": "Directory path relative to the repository root"
                }
              },
              "required": ["path"]
            }
          }
        ]
      }
    ]
  }
}
```

### 步驟 2：等待模型回應

模型可以用三種方式回應：

- 純文字，
- 要求呼叫某個工具，
- 或兩者都有。

如果它回傳純文字，我們就完成了：那就是最後的程式碼審查結果。如果它要求我們呼叫工具，就進入步驟 3。

例如，我們可能會收到：

```json
{
  "candidates": [
    {
      "content": {
        "role": "model",
        "parts": [
          {
            "text": "Let's see what damage we're dealing with today..."
          },
          {
            "functionCall": {
              "id": "call_001",
              "name": "getDiff",
              "args": {}
            }
          }
        ]
      }
    }
  ]
}
```

### 步驟 3：在本機執行工具

接下來輪到我們的應用程式。

我們在本機執行要求的工具。例如執行 `git diff` 或讀取倉庫中的某個檔案，然後把結果 **作為對話中的另一則訊息** 送回給模型。

這個 demo 可用的工具有：

- `getDiff`
- `getFile`
- `listFiles`

換句話說，這就是一個好的程式碼審查者需要的一切。😄

### 步驟 4：重複直到模型完成

然後我們只要回到步驟 2。為了避免卡進無限迴圈，我把最大迭代次數限制在 **10** 次。

不過，還有一個重要細節。

看看下一個請求。注意我們把 **整段對話歷史** 一起送回模型：

```json
{
  "contents": [
    {
      "role": "user",
      "parts": [
        {
          "text": "Please review the current git diff."
        }
      ]
    },
    {
      "role": "model",
      "parts": [
        {
          "text": "Let's see what damage we're dealing with today..."
        },
        {
          "functionCall": {
            "id": "call_001",
            "name": "getDiff",
            "args": {}
          }
        }
      ]
    },
    {
      "role": "user",
      "parts": [
        {
          "functionResponse": {
            "id": "call_001",
            "name": "getDiff",
            "response": {
              "result": "diff --git a/src/auth.ts b/src/auth.ts\n--- a/src/auth.ts\n+++ b/src/auth.ts\n@@ -12,7 +12,7 @@\n-  if (password === storedHash) {\n+  if (password == storedHash) {\n"
            }
          }
        }
      ]
    }
  ]
}
```

就這樣而已。LLM 會決定 **要用哪個工具** ，以及 **什麼時候結束** 。其他所有事情——執行工具、處理重試、限制迭代次數、以及協調整個迴圈——都是我們應用程式的責任。

很簡單，對吧？如果你喜歡視覺化，ChatGPT 已經幫我們準備了一張圖 😉

[![agent 如何運作的示意圖](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/odkpukjfb770s4bfipqa.png)](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/odkpukjfb770s4bfipqa.png)

## 接下來呢？

我未來想至少再寫兩篇延伸文章。一篇是把 Steve 接到 **MCP** ，另一篇是用 **本機 LLM** 取代託管模型。

不過……先等等，一步一步來。😄

## 加碼：模型怎麼知道自己該呼叫函式？

如果你只是想學怎麼做 AI agent，那你大概可以看到這裡就停了。😄

這一段是給好奇的人看的。因為遲早會有人問： *「Sylwia，你到底在說什麼？你說你在做自己的 AI agent，但你其實只是用了 Gemini SDK。你送進去 JSON，回來也是 JSON。」*

而且，正如 @darkwiiplayer 常常指出的，LLM 基本上就是一個很厲害的下一個 token 預測器，不是什麼神奇的 JSON 產生器。

那麼……當我們送 JSON 給它時，模型到底怎麼知道該做什麼？如果不是 Gemini，而是一些老舊的本機 Llama 模型，會發生什麼事？

是 Gemini SDK 偷偷在前面加了一段特殊提示詞嗎？還是這些模型本身就有針對這種情境訓練？

老實說，我們並不知道所有細節。不過我們 **知道** 的是，像 **Gemini** 和 **GPT** 這類模型原生支援工具呼叫。SDK 負責正確格式化請求並與 API 溝通，而模型本身理解工具宣告，並且在判斷需要時可以產生函式呼叫。

換句話說，如果我們拿一個較舊的 Llama 模型，也還是可以寫一段提示詞，說明如何解讀 JSON，並要求它以特定的 JSON 格式回應。

然後我們只要呼叫 `JSON.parse()` ……

……前提是模型真的回傳有效的 JSON，而不是 Markdown、說明文字，或它決定額外加上的幾句「貼心」註解。😄

話雖如此，值得一提的是，較新的開源模型也越來越多原生支援工具呼叫了。

## 尾聲：由另一個 agent 寫成的 agent

老實說，現在是 2026 年。這個 agent 很大一部分是由…… **另一個 AI agent** 寫出來的。😄

因為我已經當了一段時間的 AWS Community Builder，所以我取消了 Claude Code 訂閱，改用 **Kiro** ：一個內建支援多種 LLM 的 AI IDE，包括 Claude、GPT 和 Gemini。價格差不多，但因為這個計畫，我可以免費使用。

到目前為止，我真的很喜歡它。它有點像是建構在 VS Code 上的 agent 驅動層，所以我很快就上手了。我也覺得我現在可能只用了它實際功能的 **10%** ，所以我很確定未來還會再寫更多關於它的內容。

現在我只希望 AWS 終於會寄一些周邊給我。😄 我特別想要他們的一件 T 恤。好多年前，波蘭其實真的有一個名叫 **AWS** 的政黨，所以我超想看看一些年長者看到它時的表情。😄

AWS……如果你們正在看這篇……我在等你們！！😄

## 最後想法

如你所見，AI agent 不一定非得從框架開始。有時你只需要一個 LLM、一些工具、對話歷史，以及一個簡單的迴圈。其他的一切，都是便利性、正式環境強化，以及提升使用體驗的改良。

所以……

**你覺得 Steve 怎麼樣？你又喜不喜歡這種打造 AI agent 的方式？** 😄

如果你喜歡這篇文章，也可以在 [LinkedIn](https://www.linkedin.com/in/sylwia-laskowska-5a8467131/) 上追蹤我。

---

原文出處： [https://dev.to/sylwia-lask/the-dirty-secret-behind-ai-agents-demo--273d](https://dev.to/sylwia-lask/the-dirty-secret-behind-ai-agents-demo--273d)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qzorQx) [2瀏覽](https://codelove.tw/@tony/post/qzorQx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qzorQx)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [首次亮相 NVIDIA Jetson Orin Nano Super - 最經濟實惠的生成式 AI 超級計算機](https://codelove.tw/@tony/post/qvpzV3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 💬2 ❤️5

277

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

112

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784854184&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqzorQx&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTI5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjEyOSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTI5Il1dLDBd&amp;dt=1784854184181&amp;bpp=2&amp;bdt=1131&amp;idt=68&amp;shv=r20260721&amp;mjsv=m202607220101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784792125%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784792125%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784792125%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6962113899937&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95395795%2C95396139%2C95396922%2C31099900%2C95396252&amp;oid=2&amp;pvsid=5633510004905152&amp;tmod=1534546218&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=95" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="320" height="50" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260721/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>