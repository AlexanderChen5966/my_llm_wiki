---
title: "Jev 與 Laya 決策模型怎麼用？用文章推薦實測輸入、輸出與差異"
source: "https://vervecode.dev/ai/jev-laya-decision-models/"
author:
  - "[[Ted Liou]]"
published: 2026-09-19
created: 2026-10-06
description: "用文章推薦小專案理解 Jev 與 Laya 的輸入、選項和機率輸出，記錄 Laya 調整前後的誤判及 Jev 的對照結果。"
tags:
  - "clippings"
status: processed
---
摘要

用文章推薦小專案理解 Jev 與 Laya 的輸入、選項和機率輸出，記錄 Laya 調整前後的誤判及 Jev 的對照結果。

文章目錄

![](https://vervecode.dev/_astro/thumbnail.BP7c8fRU_ZPrFOz.avif "thumbnail")

Jev 和 Laya 是用來做判斷的 AI 模型。給它一段資料、要判斷的問題與允許的答案，就能取得程式可用的選擇與機率，例如從幾個部門中選出工單應該交給誰。

我想了解這類模型的技術用法，因此做了一個獨立的 [Decision Model Playground](https://github.com/tedliou/decision-model-playground) ，拿自己部落格的文章當實驗資料。題目是「輸入一個問題，從現有文章中選出適合的一篇」，這樣答案容易核對，也能故意問沒有相關文章的問題，看看模型會不會硬選。

## Jev 與 Laya 的使用方式

依照 TypeSafe 的 [Jev 技術介紹](https://typesafe.ai/blog/introducing-system-one-models-and-jev) ，Jev 專注於回傳預先定義的型別與機率，官方將這類模型稱為 System One Models。Laya 也提供類似的決策介面，但可以下載到本機執行。

這次使用的版本與執行方式如下。因為實驗問題是繁體中文，Laya 選用多語版：

| 模型 | 開放方式 | 本次執行方式 |
| --- | --- | --- |
| Jev `jev-1.13.0` | 閉源，透過 API 使用 | 後端將問題與選項送到 TypeSafe |
| `laya-multilingual` | 權重與 SDK 開放，模型採 Apache 2.0 授權 | Windows、RTX 3070 8GB 本機執行 |

Laya 多語版的 [模型卡](https://huggingface.co/convaiinnovations/laya-multilingual/blob/052592a15d198d9ad47da779604259b10b47b7aa/README.md) 說明它以 mmBERT-base 為編碼器，Jev 則採 TypeSafe 自行開發的模型架構。

它們都提供三種題型。我們先決定答案應該長什麼樣子，再準備資料與判斷要求：

| 題型 | 要準備什麼 | 回傳結果 | 用途示例 |
| --- | --- | --- | --- |
| Choice | 資料、問題、候選選項 | 選中的選項與各選項機率 | 工單分派部門 |
| Score | 資料、問題、有順序的評分描述 | 分數與各等級機率，分數可落在兩級之間 | 問題嚴重程度 |
| Noul | 資料、要判斷是否成立的敘述 | 敘述為真的機率，介於 0 到 1 | 是否附上重現步驟 |

本次只實測 Choice。其他題型的定義可參考官方的 [Score](https://docs.typesafe.ai/primitives/score) 與 [Noul 文件](https://docs.typesafe.ai/primitives/noul) 。

## 把文章選擇整理成模型輸入

專案從網站 RSS 整理出 [18 篇文章的資料快照](https://github.com/tedliou/decision-model-playground/blob/1b9f5f0/data/articles.json) ，另外加入「沒有適合文章」的 `none` ，讓模型做 19 選 1。實驗介面可以切換模型，並顯示全部選項的機率與耗時。

先看調整後的資料安排。模型讀取的是事先整理的 [文章精簡主題](https://github.com/tedliou/decision-model-playground/blob/1b9f5f0/data/descriptors.json) ，完整中文標題、摘要與網址只供介面顯示：

| 欄位 | 放什麼 | 本次例子 |
| --- | --- | --- |
| `state` | 要判斷的資料 | 我想讓 AI 幫我操作 TouchDesigner |
| `instructions` | 判斷要求 | 選出最能回答問題的文章，沒有合適文章就選 none |
| `criteria` | 選項 ID 與描述 | `a03` 對應 `TouchDesigner AI control` |

程式透過共同的 [題目組裝邏輯](https://github.com/tedliou/decision-model-playground/blob/1b9f5f0/apps/api/src/playground/decision.py) 產生以下結構，範例省略部分候選。實際請求會帶齊 18 篇文章與 `none` ：

```json
{
  "state": "我想讓 AI 幫我操作 TouchDesigner",
  "questions": {
    "recommendation": {
      "type": "choice",
      "instructions": "Which article best answers the reader's question? Choose none if no article helps.",
      "criteria": {
        "a01": "Astro Cloudflare build cache",
        "a03": "TouchDesigner AI control",
        "a12": "Unity VS Code integration",
        "none": "No relevant article"
      }
    }
  }
}
```

`a03` 是程式用來查找文章的 ID，描述則提供模型判斷所需的主題。 `none` 也是真正參與選擇的選項，讓模型可以回答「這批文章都不適合」。這與官方 [Choice 文件](https://docs.typesafe.ai/primitives/choice) 建議在候選不完整時加入其他選項的做法一致。

同一份題目分別交給兩個模型處理：

- [Jev 呼叫實作](https://github.com/tedliou/decision-model-playground/blob/1b9f5f0/apps/api/src/playground/providers/jev.py) ：加上 `model` 欄位後，送到 `https://api.typesafe.ai/v1/systemone` 。
- [Laya 推論實作](https://github.com/tedliou/decision-model-playground/blob/1b9f5f0/apps/api/src/playground/providers/laya.py) ：把 `state` 與 `questions` 交給本機的 `agent.predict()` 。

Jev 在這題選中 `a03` ，機率為 0.99， `none` 為 0.01，其餘 17 篇都是 0。摘錄輸出的選擇與非零機率如下：

```json
{
  "choice": "a03",
  "probabilities": {
    "a03": 0.99,
    "none": 0.01
  }
}
```

程式再用 `a03` 查出 [td-cli 文章](https://vervecode.dev/ai/td-cli/) 的標題與網址。模型不需要生成網址，也不會另外寫回答，但仍可能選錯文章。

**99% 是這次候選中的選擇機率，不代表文章有 99% 機率解決問題。** Jev 另外提供 `confidence` ，依官方 [Confidence 文件](https://docs.typesafe.ai/confidence) 所述，它是由分布形狀計算的摘要值。本次介面只顯示 `probabilities` 。

## Laya 的輸入調整與實測差異

上面的輸入結構是調整後的版本。初版把文章描述都放在 `state` ，用 TouchDesigner、Astro、Unity 與義大利麵四題測試，竟然全部選了 Minecraft 指令文章。

後續由 Agent 調整輸入，改了兩個地方：

1. `state` 只保留輸入的問題。
2. 文章描述移到 `criteria` ，並縮成短英文主題。

調整後，TouchDesigner 題選到 td-cli，義大利麵題也改成不推薦。這次一起改了位置與描述，還無法分辨是哪一項發揮作用。

檢查 SDK 時也發現，Laya 會自行截短輸入。本次 SDK 0.3.3 與模型設定有三層長度限制：

| 範圍 | 限制 |
| --- | --- |
| 整體輸入 | 最多 1,024 tokens |
| 題目與選項區 | 預算 256 tokens，選項標記也占空間 |
| 個別選項 | 先限制為 48 tokens，選項區不足時還會縮短 |

專案因此在推論前加上 [長度檢查](https://github.com/tedliou/decision-model-playground/blob/1b9f5f0/apps/api/src/playground/providers/laya.py#L63-L91) ，會被截斷就要求縮短輸入，避免把成功回應誤認成完整讀過資料。這也是使用短主題的原因，但目前沒有證據能把第一輪誤判直接歸因於截斷。

接著用 Jev 測同樣四題，與先前 [Laya 的驗證紀錄](https://github.com/tedliou/decision-model-playground/blob/1b9f5f0/docs/validation.md) 比較。兩邊使用相同文章快照與調整後的題目，表中百分比為選中答案的機率：

| 問題 | 人工預期 | Laya 調整後 | Jev |
| --- | --- | --- | --- |
| 我想讓 AI 幫我操作 TouchDesigner | td-cli | td-cli，96.13% | td-cli，99% |
| Astro 網站每次部署都要很久，怎麼加速？ | Astro 建置快取 | none，28.28% | Astro 建置快取，99% |
| 如何讓 Unity 使用 VS Code 寫 C#？ | Unity 編輯器連動 | none，21.31% | Unity 編輯器連動，99% |
| 怎麼煮出好吃的義大利麵？ | none | none，55.50% | none，100% |

Jev 四題都符合預期，Laya 則漏掉 Astro 與 Unity。這也說明加入 `none` 只是讓模型可以不選文章，不保證它能正確判斷何時該放棄。

接著再用 Jev 測幾種問法，觀察換個描述後是否仍選到同一篇：

- Astro 題補上圖片處理與 Cloudflare Pages 快取：仍選快取文章，機率為 96%。
- Unity 題把 VS Code 換成 Cursor：仍選編輯器連動文章，機率降到 78%，另有 18% 分給 `none` 。
- 詢問未收錄的 React Hooks 與番茄炒蛋食譜：兩題都選 `none` 。

Cursor 題比較值得留意。文章包含 Cursor 的設定，但送給模型的短描述只有 `Unity VS Code integration` 。短描述確實漏掉了 Cursor，但還不能確認這就是機率下降的原因。

Jev 整輪共 9 次請求、8 個不同問題，選擇都符合預期。這是少量案例對照，前四題也參與過輸入調整，不能當成整體準確率。

## 耗時與試用費用

兩者的耗時分開記錄。Laya 第一次需要載入模型，之後重用同一個實例。Jev 則每次透過網路呼叫遠端服務：

| 測量項目 | 本次結果 |
| --- | --- |
| Laya 首次載入 | 約 10 秒 |
| Laya 載入後的四題推論 | 約 24–165 ms，含 SDK 處理與本機 GPU 推論 |
| Jev 九次 API 往返 | 約 570–852 ms，中位數約 629 ms，含網路與遠端處理 |

測量範圍不同，這些數字適合了解實際等待時間，不能直接拿來比較純模型速度。

想試用 Jev，可以到 [TypeSafe 官網](https://typesafe.ai/) 申請 Waitlist。我當時用 Email 申請，大約一天後收到通過通知，註冊後獲得 5 美元試用額度，因此能直接接進小專案測試。

截至 2026 年 9 月 19 日，官方公布每百萬輸入 tokens 收費 0.042 美元，輸出免費。這九次實測共使用 5,726 個輸入 tokens，估計約 0.00024 美元。以這次的輸入長度來說，反覆試問法的 API 費用很低。

## 總結

整個 Side Project 的實作與實驗都交給 GPT-6 Astra，以 Light effort 完成。我只在過程中適時調整它的設計方向，讓結果符合自己的需求。

用文章選擇當題目，讓我比較容易看懂決策模型需要什麼資料，以及回傳的數字代表什麼。這次 Jev 選到了 Laya 漏掉的文章，Laya 的實驗則讓我發現輸入位置、描述長度與截斷都需要檢查。接通之後，還是要拿實際問題核對結果。

想替換成自己的資料，可以參考專案 README 的 [本機啟動流程](https://github.com/tedliou/decision-model-playground#%E5%95%9F%E5%8B%95) 。目前提供 Windows PowerShell 安裝方式，預設使用 NVIDIA CUDA。測試時保留幾題明確有答案、幾題沒有合適選項的問題，會比只看一次成功結果更容易發現差異。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="786" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-9736607245120454&amp;output=html&amp;h=280&amp;slotname=6549963926&amp;adk=1336004163&amp;adf=3407817974&amp;pi=t.ma~as.6549963926&amp;abgtt=6&amp;w=786&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791258794&amp;rafmt=1&amp;format=786x280&amp;url=https%3A%2F%2Fvervecode.dev%2Fai%2Fjev-laya-decision-models%2F&amp;fwr=0&amp;fwrattr=true&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuOTUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3Ljk1Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy45NSJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1791257675712&amp;bpp=1&amp;bdt=498&amp;idt=72&amp;shv=r20261005&amp;mjsv=m202610010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie_enabled=1&amp;eoidce=1&amp;prev_fmts=0x0%2C300x250&amp;nras=1&amp;correlator=6948922385828&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=447&amp;ady=5427&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=1954&amp;eid=95403358%2C95405219&amp;oid=2&amp;pvsid=6451371634627914&amp;tmod=1142377466&amp;uas=1&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7CEebr%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>