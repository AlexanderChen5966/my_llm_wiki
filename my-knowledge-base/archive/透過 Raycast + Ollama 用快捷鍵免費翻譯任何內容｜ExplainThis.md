---
title: "透過 Raycast + Ollama 用快捷鍵免費翻譯任何內容｜ExplainThis"
source: "https://www.explainthis.io/zh-hant/ai/free-translation-tool"
author:
  - "[[explainthis.io]]"
published: 2025-08-05
created: 2026-05-28
description: "因為 AI 工具發展越來越成熟完整，先前有訂閱用來翻譯的 DeepL 最近到期後，決定不繼續訂閱。在研究各種不同替代方案後，發現 Raycast 搭配 Ollama 的組合，是最能夠輕鬆做到之前用 DeepL 的快捷鍵翻譯使用體驗，除了免費外，翻譯品質比 DeepL 來得更好。如果你正在找一個能在電腦中，用快捷鍵一鍵完成翻譯的工具，非常推薦這個組合。以下讓我們詳細介紹一下如何安裝與設置。"
tags:
  - "clippings"
---
因為 AI 工具發展越來越成熟完整，先前有訂閱用來翻譯的 DeepL 最近到期後，決定不繼續訂閱。在研究各種不同替代方案後，發現 Raycast 搭配 Ollama 的組合，是最能夠輕鬆做到之前用 DeepL 的快捷鍵翻譯使用體驗，除了免費外，翻譯品質比 DeepL 來得更好。

如果你正在找一個能在電腦中，用快捷鍵一鍵完成翻譯的工具，非常推薦這個組合。以下讓我們詳細介紹一下如何安裝與設置。

## 使用情境說明

由於身為工程師，經常需要閱讀外文的文件。就目前市面上有的方案，最常見的是用 Google 翻譯，例如 Chrome 可以搭配 Google 翻譯把整個外文網頁翻譯成中文；但這主要有兩個問題，一個是 Google 翻譯的品質還是沒那麼好，第二個是這受限在網頁中 (假如用其他的軟體就沒辦法這樣一鍵翻譯)。

先前訂閱 DeepL 除了翻譯品質比 Google 翻譯來得好，DeepL 桌機版有提供快捷鍵，在電腦中不論用什麼應用程式，都可以用快捷鍵來翻譯，非常方便。

但因為訂閱費一年超過 3,000 台幣，也不是筆小費用，所以到期後決定探索免費的替代方案 (假如是前後端工程師，省下的這三千多元，可以訂閱一年的 [**E+ 成長計畫**](https://www.explainthis.io/zh-hant/e-plus) ，讓自己的技術與軟實力持續提升)。

如下方截圖所示，用 Raycast 搭配 Ollama，可以做到在電腦中選取任何文字 (下圖是選取某段在 PDF 檔案中的文字)，然後用快捷鍵呼叫 AI 完成翻譯。

![選取文字後，透過快捷鍵直接翻譯段落](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_205055412.png?alt=media&token=58d061b0-f91a-44a8-9749-5d196d459805)

選取文字後，透過快捷鍵直接翻譯段落

在許多網路文章中，都有談到用 Raycast 搭配 Google 翻譯，來免費透過快捷鍵翻譯，但這樣的問題是翻譯品質不太好，不過如果改成 Raycast + Ollama 搭配的開源 AI 模型翻譯品質甚至比 DeepL 來得好。

在翻譯的語言選擇上，如果日文不好，可以日文翻譯成中文，假如英文不好想要有英翻中，或者是反過來中翻日、中翻英，都能夠透過 Raycast + Ollama 這個組合做到。

## 步驟一：下載 Ollama 與開源模型到本機

首先，我們需要先下載 Ollama 來使用開源的 AI 模型。Ollama 是一個讓人可以輕鬆使用開源 AI 模型的工具，特別適合那些擔心資料安全相關疑慮的人。因為使用 Ollama，是把模型下載到自己的電腦，在本機使用，而不用經過某個第三方的雲端廠商。

目前業界各大開源模型，包含 Google DeepMind 的 Gemma，或是 Meta 推出的 Llama，以及微軟推出的 Phi，都可以透過 Ollama 下載到本機中使用。

要用這些模型，可以透過 Ollama 兩步驟做到。第一步是到 [https://ollama.com/download](https://ollama.com/download) 下載 Ollama (見附圖)，然後在本機打開 󠀠。

![下載 Ollama](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_210655321.png?alt=media&token=0b642038-7c4d-4b69-895a-ca4bea11f025)

下載 Ollama

第二步則是開啟電腦終端機 (terminal)，然後輸入 `ollama run [模型名稱]` 。例如要用 Google 的開源模型 Gemma 可以輸入 `ollama run gemma3` 即可 (像下面附圖這樣，輸入完就會開始下載)。完整的開源模型清單可以在 [https://ollama.com/search](https://ollama.com/search) 看到。

![在終端機中透過 Ollama 用開源模型](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_210759983.png?alt=media&token=53c7e8a2-02e7-45f4-80c5-bc7444c88b38)

在終端機中透過 Ollama 用開源模型

如果是要翻譯，推薦用參數小的模型即可。舉例來說，下面是 Google 的開源模型 gemma3 有供下載的選項，如果是翻譯用，推薦用 `gemma3:4b` 即可，所以可以在終端機輸入 `ollama run gemma3:4b` 。可以在下圖看到 `4b` 的模型只會佔用 3.3GB 的容量，但如果是 `27b` 的則會要 17GB 的大小。

![在 Ollama 中可以看到不同參數大小的模型](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_210902919.png?alt=media&token=cab43bde-624c-42aa-8832-5db05cbf1ce9)

在 Ollama 中可以看到不同參數大小的模型

我們實測後，用 `gemma3:4b` 的翻譯小果已經比 DeepL 來得好 (雖然 `1b` 模型更小，但感覺翻譯有時不到位，所以推薦 `4b`)。至於為什麼不用更大的模型，主要是殺雞焉用牛刀，假如只是翻譯不用下載容量那麼大的模型，為自己的電腦本機省點空間。

## 下載 Raycast 並設置 AI 快捷鍵

在透過 Ollama 下載完開源模型後，接著就可以下載 Raycast 。可以到 Raycast 的官網 [https://www.raycast.com](https://www.raycast.com/) 點擊右上角的 `Download` (現在 MacOS 可以直接下載，Windows 要加入等候清單才行)。Raycast 有免費版與付費版，以下談到的基本上免費版就夠，不用付費版。

![到 Raycast 首頁下載](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_213212014.png?alt=media&token=3a8ca113-818e-4d01-9a0b-43c2e66aa228)

到 Raycast 首頁下載

接著打開 Raycast 後用 `Cmd + ,` 這個快捷鍵打開設定頁面，先在上排選項中點擊 AI。一開始上面會有一個 `50` 的數字，這是 Raycast 免費提供的模型使用額度。換句話說，假如用 Raycast 免費版，只能用 50 次，但是以下我們會談搭配 Ollama 的開源模型，就不會受這個 50 次限制影響。

點到 AI 的面板後，下滑找到 `AI Commands` (AI 指令)，然後在 `AI Commands Model` 中點擊 `Add a Model via Ollama` ，點擊完後可以選你要用的模型。

![在 AI Commands 中新增 Ollama 模型](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_213300078.png?alt=media&token=f79065bf-bcb7-47c6-9eab-6d3a8c4dd045)

在 AI Commands 中新增 Ollama 模型

設定完模型後，接著點擊上排選項中的 `Extensions` ，然後按 `Cmd + n` 快捷鍵，會開啟一個選單，在選單中選 `Create AI Command` (新增 AI 指令)。

![新增新的 AI 指令](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_213355974.png?alt=media&token=a6f7593c-d704-41a1-8eed-10200faf2267)

新增新的 AI 指令

接著就可以新增快捷鍵，這邊我把 `Title` 設定成「翻譯成中文」，然後在 `Prompt` 加入提示詞，記得在提示詞中要加入 `{selection}` ，這樣你在電腦中選取任何的文字，就會被放到 `{selection}` 當中；最後 `Model` 要選在 Ollama 的模型。完成後記得要用 `Cmd + Enter` 儲存。

![編輯給 AI 的提示詞](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_213606350.png?alt=media&token=2db8a28f-2265-4f83-b17a-84a7319d4397)

編輯給 AI 的提示詞

新增完成後，要記得添加 `Hotkey` (快捷鍵) ，這邊把 `Hotkey` 新增為 `Cmd + 1` 。

![為 AI 指令新增快捷鍵](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_213657192.png?alt=media&token=33d91586-af97-4e81-9702-8f387e4f5b16)

為 AI 指令新增快捷鍵

接著，就可以在用電腦時，使用 `Cmd + 1` 來翻譯任何內容，不會受限在瀏覽器中。以下方截圖為例，Anthropic 關於 Claude Code 的最新文件，即使是 PDF 檔案，一樣可以選取文字後，用 `Cmd + 1` 來翻譯，非常方便。

![選取文字後，透過快捷鍵直接翻譯段落](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_205055412.png?alt=media&token=58d061b0-f91a-44a8-9749-5d196d459805)

選取文字後，透過快捷鍵直接翻譯段落

事實上，Raycast + Ollama 不僅可以用來翻譯，只要修改提示詞，就可以在電腦中用快捷鍵，請 AI 幫忙做各種大小事。舉例來說，把提示詞稍微調整 `用精簡通順的方式，以繁體中文總結以下的內容 {selection}` ，就可以選取任何內容，搭配設定的快捷鍵，讓 AI 幫忙總結某段看到的內容，這樣可以不用再額外開 ChatGPT 或其他 AI 工具，直接快捷鍵就能完成。

![更換提示詞就能用 AI 快捷鍵做其他事](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/%E5%9C%96%E7%89%87_2025-08-05_213932946.png?alt=media&token=24c2d550-a9a4-47ae-8bae-b04f74d40965)

更換提示詞就能用 AI 快捷鍵做其他事

以上介紹了如何透過 Raycast + Ollama 的搭配，輕鬆用快捷鍵就可以在本機使用 AI，可以用來快速翻譯任何內容，也可以做到 ChatGPT 能做到的事，而且不用額外開 ChatGPT 或其他 AI 工具。推薦假如你有類似的需求，也可以試試這個免費又好用的組合。

如果你覺得這類內容有幫助，想閱讀更深入的軟體工程、AI 工程內容，歡迎加入 [**E+ 成長計畫**](https://www.explainthis.io/zh-hant/e-plus) ，與 800+ 位工程師一起成長。