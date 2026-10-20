---
title: "誰適合使用 ComfyUI？從零開始生成你的第一張圖"
source: "https://vocus.cc/article/6902d602fd8978000165a206"
author:
  - "[[M i o]]"
published: 2025-10-30
created: 2026-07-22
description: "這篇文章詳細介紹 ComfyUI 的基礎操作流程，適合剛接觸 ComfyUI 圖像生成的新手。內容逐步說明如何載入模型、設定提示詞與生成圖片，並透過多張實際介面截圖清楚示範每個步驟，讓讀者無痛掌握使用 ComfyUI 生成圖像的要領。教學清晰易懂，是初學者入門 ComfyUI 的理想指南。"
tags:
  - "clippings"
status: processed
---
今天先聊聊幾個 ComfyUI 使用前的想法觀念：

1. ComfyUI 適合誰？為什麼要使用 ComfyUI？

ComfyUI 跟生成平台第一個差異在於生成的「數量限制」，在2025年10月 的當下，無論是 ChatGPT 還是 Grok 都有一定的生成數量限制，免費使用者的限制又更多，ComfyUI則完全沒有這個問題，除非是硬碟滿了。

ComfyUI 適合想「大量」產出自己的創意想法，喜愛嘗試各種風格創意，又討厭被框架限制的創作者；但也同時必須帶有一點「研究」精神。因為ComfyUI 有自己的開源社群平台，除了可以導入ComfyUI 的資源極多以外，有時也必須面對一些版本衝突問題，有能力自己找到答案也是非常重要的。

如果你只是想偶爾娛樂一下自己，那麼恭喜你，一般的生成平台就已經非常足夠你使用。未來各家生成平台免費開放可生成的圖像與影像品質只會越來越高，相對來說，本地端的生成成本也會更高。

2. ComfyUI 的使用成本？

一台桌上型電腦 (包含一張RTX獨立顯示卡，建議12G以上)  
硬碟空間越大越好 - 因為下載每個 "Checkpoint" 都要10G左右  
網路速度越快越好 - 因為太多開源 "Checkpoint" 可以下載  
我的時間越多越好:P  
  
ComfyUI官方建議：  
[https://docs.comfy.org/installation/system\_requirements#manual-installation](https://docs.comfy.org/installation/system_requirements#manual-installation)

3. ComfyUI 要怎麼使用？

這裡介紹 ComfyUI 最基本的使用方式，上一篇我們已經順利產出水瓶！

![zoomable](https://images.vocus.cc/a42d0ad9-185e-4270-b5fd-00ec0fac8b4b.jpg)

順利產出水瓶

1. 今天選擇畫面左邊的「模板」
![點選模板](https://resize-image.vocus.cc/resize?norotation=true&quality=80&url=https%3A%2F%2Fimages.vocus.cc%2F12a0caf8-71a2-4fb6-ba50-ed2992abd9d3.jpg&width=740&sign=p17-0EaYb1tUToNO_BVQq2SK7YVFsr5glrNMS30Rb-8) ![zoomable](https://images.vocus.cc/12a0caf8-71a2-4fb6-ba50-ed2992abd9d3.jpg)

點選模板

你會看見這一個頁面  
2\. 如果你想生成人像，我建議直接選擇 "Flux" 這個模型，可以相對保證生成出來的人物不會有太多問題。

![按照順序 1.圖片 2選擇"Flux"](https://resize-image.vocus.cc/resize?norotation=true&quality=80&url=https%3A%2F%2Fimages.vocus.cc%2F11cc0aeb-31c8-450c-a958-1d3bb9afb4de.jpg&width=740&sign=AkKwcztDWcY9qFQMJFkj38VQYGdjPgiDyKHLgYNjzeo) ![zoomable](https://images.vocus.cc/11cc0aeb-31c8-450c-a958-1d3bb9afb4de.jpg)

按照順序 1.圖片 2選擇"Flux"

3. 接著點選 "Flux Dev fp8 "
![選擇 Flux Dev fp8 ](https://resize-image.vocus.cc/resize?norotation=true&quality=80&url=https%3A%2F%2Fimages.vocus.cc%2F6d52e52d-5cdf-456e-ae70-3a90711f9cb9.jpg&width=740&sign=5BLQCdYXrtiG5ai-uFUM3zWhRu7CRSNOxKZKGZL0TOw) ![zoomable](https://images.vocus.cc/6d52e52d-5cdf-456e-ae70-3a90711f9cb9.jpg)

選擇 Flux Dev fp8

4. 然後系統會跳出這個 "工作流(workflow)" 裡缺少的 "模型(model)"。  
	不要緊張，下載好之後把它貼到 "checkpoint" 這個資料夾就好  
	我的資料夾路徑是 C:\\Users\\User\\ComfyUI\_manual\\models\\checkpoints

![下載缺少的模型](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F48e77dd2-fc1f-434f-816d-e579224f6c5c.png&width=740&sign=rsq9L47yninQufA3GsX_CIJe9N6mJ6SDRayw3Ey6Vl8) ![zoomable](https://images.vocus.cc/48e77dd2-fc1f-434f-816d-e579224f6c5c.png)

下載缺少的模型

5. 然後將瀏覽器關掉重開一次，點選「執行」

![按照模板預設的文字生成圖像](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fae193e92-2003-42d6-9556-965c0303f148.png&width=740&sign=wmU4I5bo-mvFVh269Af-QVvLj0vqMA_87FYCGJEaOd0) ![zoomable](https://images.vocus.cc/ae193e92-2003-42d6-9556-965c0303f148.png)

按照模板預設的文字生成圖像

按照預設文字生成的第一張圖應該會是一隻帶著耳朵的獸娘

\--  
以下示範兩個提示詞

在綠色框框裡寫下"1 woman, realistic style, Live photography"，點擊「執行」！

![生成真人圖像1](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F99cd03f6-1f86-4768-9fdf-fa0b92696a1f.png&width=740&sign=q2qTQDNMjIhUnNpNn2QS21QCPb-5-fMUMgSH7imiN8M) ![zoomable](https://images.vocus.cc/99cd03f6-1f86-4768-9fdf-fa0b92696a1f.png)

生成真人圖像1

綠色框框的正式名稱是「正面提示詞 (Positive Prompt) 」，在 Flux 這一系列模型中不需要寫「負面提示詞 (Negative Prompt) 」，就是綠框下方摺疊起來的紅色節點。

正面提示詞寫下"1 woman, Real-life street fashion photography"，點擊「執行」！

![生成真人圖像2](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F61257bdd-29d9-42b1-b267-af50fbfb9137.png&width=740&sign=vMUWEjdNF559cKpNwSf5NS4VrD_hFeQGq6tUuObnlMg) ![zoomable](https://images.vocus.cc/61257bdd-29d9-42b1-b267-af50fbfb9137.png)

生成真人圖像2

雖然提示詞 (Prompt) 是影響生成圖像的關鍵要素，但影響更大的其實是模型 (model)，也就是那16GB的 "checkpoint"，多數基礎模型都是以歐美外貌來訓練的，所以如果只寫下 "1 mowan" 就會是歐美女性的外貌。

順帶一提，這個模型如果寫下 "1 girl" 會優先以動漫方式呈現，有興趣的朋友可以再多嘗試看看。

按照這個 ComfyUI 內建的模板功能，基本上就已經玩不完了，所以硬碟空間跟網路速度就顯得格外重要。

下一篇將繼續介紹 ComfyUI 的 工作流(workflow) 以及 Lora！  
如果對於內容有建議或提醒，歡迎留言告訴我！