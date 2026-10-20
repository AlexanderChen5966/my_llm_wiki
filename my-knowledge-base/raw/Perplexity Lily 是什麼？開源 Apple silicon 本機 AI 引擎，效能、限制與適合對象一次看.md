---
title: "Perplexity Lily 是什麼？開源 Apple silicon 本機 AI 引擎，效能、限制與適合對象一次看"
source: "https://www.aiposthub.com/perplexity-lily-apple-silicon-local-ai/"
author:
  - "[[Philo]]"
published: 2026-09-03
created: 2026-09-04
description: "Perplexity 開源 Lily 本機 AI 推論引擎，專為 Apple silicon 與 Qwen3.6-35B-A3B 打造。本文整理運作方式、MLX 效能比較與安裝門檻。"
tags:
  - "clippings"
status: processed
---
Perplexity 在 2026 年 9 月 3 日宣布開源 Lily。Lily 並非新的 AI 模型，而是一套讓既有模型在 Mac 上跑得更快的「本機推論引擎」。目前 Lily 只針對 Apple silicon 與 Qwen3.6-35B-A3B 的特定量化版本設計。

這項消息真正重要的地方，不只是又多了一個開源專案。Perplexity 正在嘗試把雲端 AI 的推理能力與 Mac 的本機處理結合：一般研究、規劃交給雲端大型模型，敏感檔案與裝置操作則留在電腦上處理。

不過，Lily 現階段仍是高度專用的工程工具，無法直接取代 Ollama 或 LM Studio。一般使用者若只是想在 Mac 上使用 Perplexity 的混合運算，安裝官方 App 會更實際。Lily 原始碼比較適合開發者、推論框架工程師，以及想研究 Apple GPU 最佳化的人。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/09/perplexity-hybrid-compute-official-1.mp4" width="1280" height="720" controls=""></video>

Perplexity 官方影片示範 Computer 如何把工作分配給雲端模型與 Mac 本機模型，讓敏感檔案留在裝置端處理。 影片來源： Perplexity 官方 X 發布貼文 。

## Lily 是什麼？它跟 AI 模型有什麼不同？

大型語言模型可以把它想成「大腦裡學到的知識與能力」，推論引擎則像是負責把這顆大腦放進電腦、接收問題並產生答案的執行系統。同一個模型交給不同引擎執行，速度、記憶體使用方式與支援功能都可能不同。

Lily 是 Perplexity 為 Apple silicon 打造的小型推論伺服器。它使用 Rust 管理模型載入、對話狀態與文字生成，再透過 Metal 核心程式直接驅動 Apple GPU。Metal 是 Apple 提供的圖形與運算技術，可讓程式更直接使用 Mac 晶片內的 GPU。

目前 Lily 只支援 Qwen3.6-35B-A3B 的特定 4-bit 量化版本。量化是把模型數字壓縮成較小格式，目的是降低記憶體需求並提高執行效率。這個模型由 Qwen 團隊推出，總共有約 350 億個參數，但每次產生內容時只啟用約 30 億個參數，因此名稱中才會出現「35B-A3B」。

這種設計稱為混合專家模型（Mixture of Experts，MoE）。它不會讓所有參數同時工作，而是依照每個輸入挑選部分「專家」參與運算。對本機電腦來說，好處是能用較少計算量取得大型模型的能力，但不規則的專家分流也讓引擎更難最佳化。

## Perplexity 為什麼不直接使用 MLX？

MLX 是 Apple 為 Apple silicon 推出的開源機器學習框架，MLX-LM 則補上大型語言模型載入與文字生成所需的功能。它們的優勢是通用，可以支援許多模型與研究需求。

Lily 選擇另一條路：放棄廣泛相容性，換取針對單一模型與硬體的深度最佳化。它的執行路徑不使用 PyTorch，也不透過 MLX，而是把模型結構、記憶體移動、工作排程與 Metal 核心程式整合在同一套 Rust 執行環境裡。

這個取捨可以用下表理解：

| 比較項目 | Lily | MLX-LM |
| --- | --- | --- |
| 主要定位 | 單一模型、單一硬體路線的專用引擎 | 支援多種模型的通用框架 |
| 目前模型支援 | 指定的 Qwen3.6-35B-A3B 4-bit 版本 | 多種 Apple silicon 可執行模型 |
| 最佳化方式 | Rust 執行環境加上自訂 Metal 核心程式 | 由 MLX 的通用運算與核心程式處理 |
| 使用彈性 | 低 | 高 |
| 適合對象 | 推論工程師與特定部署需求 | 一般本機 AI 開發與模型實驗 |

Lily 證明的是「針對模型與晶片一起設計，還能再擠出效能」，這不表示通用框架已經失去價值。如果模型更新、硬體改變，或使用者要加入圖像、工具呼叫與抽樣參數，Lily 現有的專用優勢也可能變成維護成本。

![Perplexity Lily 與 MLX-LM 本機 AI 推論架構比較](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/perplexity-lily-architecture-official.jpg)

官方架構圖顯示，MLX-LM 強調多模型通用性，Lily 則把執行路徑鎖定在單一模型與 Apple 晶片。 圖片來源： Perplexity 官方 Lily 貼文串 。

## Lily 如何加速 Apple silicon 上的本機 AI？

語言模型回答問題時，大致可以拆成兩個階段。

第一個階段是 prefill，也就是先讀完使用者輸入的提示與文件。第二個階段是 decode，模型開始一個接一個產生新 token。Token 是模型處理文字的基本單位，可以是一個中文字、詞的一部分或標點符號。

這兩個階段需要的硬體能力不同。Prefill 會同時處理大量輸入，適合矩陣運算。Decode 每次通常只生成一個 token，更容易受到記憶體頻寬限制。Lily 因此沒有用同一種方式處理整段流程。

在 prefill 階段，Lily 會利用 M5 GPU 的矩陣運算能力，並在需要計算時才把 4-bit 模型權重還原成較高精度格式。模型權重就是模型訓練完成後保存的核心參數，也是產生答案時需要讀取的資料。這種做法可避免先產生一整份較大的中間資料。Perplexity 的消融測試顯示，在 512-token 的提示下，把解量化直接合併進矩陣運算，可讓端到端 prefill 吞吐量提高 77.4%。這個數字只比較 Lily 自己「有無採用該最佳化」的差異，不能視為 Lily 對 MLX-LM 的整體領先幅度。

![Perplexity Lily prefill 執行與資料存放官方架構圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/perplexity-lily-prefill-official.jpg)

Lily 在 prefill 階段依照 Qwen 架構安排專家路由、量化權重與統一記憶體中的資料。 圖片來源： Perplexity 官方 Lily 貼文串 。

在 decode 階段，Lily 會盡量讓模型權重、注意力快取與下一個 token 留在 GPU 上，減少 CPU 與 GPU 來回同步。它也會重用多個查詢共享的快取資料。Perplexity 表示，這項注意力資料重用技術在 32K 上下文的測試中，讓端到端 decode 吞吐量提高 23.8%。

![Perplexity Lily decode 迴圈與 GPU 資料重用官方架構圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/perplexity-lily-decode-official.jpg)

Lily 的 decode 路徑讓下一個 token 與部分狀態留在 GPU，減少 CPU 與 GPU 之間的等待。 圖片來源： Perplexity 官方 Lily 貼文串 。

這些做法的共同目的很簡單：少搬資料、少產生中間結果，也不要讓 GPU 等待 CPU。Apple silicon 使用統一記憶體，CPU 與 GPU 可以存取同一個記憶體池，但讀取模型權重仍然會消耗頻寬。統一記憶體降低了複製成本，資料移動依然要付出時間代價。

## Lily 真的比 MLX-LM 快嗎？最新測試要分兩段看

答案是：生成文字明顯較快，但讀取超長提示不一定比較快。

Perplexity 最初的技術文章使用 MLX 0.31.2 路線作比較，結論是 Lily 在測試的所有長度都領先。到了 2026 年 9 月 2 日，專案又加入 MLX 0.32.2 的新版比較。這份較新的資料顯示，MLX 更新後已縮小 prefill 差距，甚至在超長提示反超 Lily。

以下是 Perplexity 在一台配備 40 核心 GPU、128 GB 統一記憶體的 M5 Max 上測得的部分結果：

| 上下文長度 | Lily decode | MLX decode | Lily 相對表現 | Lily prefill | MLX prefill | Lily 相對表現 |
| --- | --- | --- | --- | --- | --- | --- |
| 256 tokens | 194.7 tokens/s | 148.5 tokens/s | 快 31.1% | 2,866.6 tokens/s | 2,536.4 tokens/s | 快 13.0% |
| 4,096 tokens | 182.8 tokens/s | 145.6 tokens/s | 快 25.5% | 5,724.4 tokens/s | 5,165.7 tokens/s | 快 10.8% |
| 32,768 tokens | 151.1 tokens/s | 118.2 tokens/s | 快 27.9% | 3,927.5 tokens/s | 3,963.7 tokens/s | 慢 0.9% |
| 131,072 tokens | 92.7 tokens/s | 75.0 tokens/s | 快 23.6% | 1,792.7 tokens/s | 2,237.5 tokens/s | 慢 19.9% |

資料來源：Perplexity Lily 專案的 MLX 0.32.2 效能報告。數值為兩輪紀錄的中位數。

這份測試量的是程式內部生成吞吐量，不包含模型載入、斷詞、HTTP 傳輸與把 token 轉回文字的時間。兩個引擎執行的工作也不是完全一致：MLX-LM 會計算完整詞彙的機率資訊，Lily 的最小化介面只做固定的最高機率選擇。因此，這些數字能證明 Lily 在特定生產路徑上更快，卻不能直接等同於使用者從按下送出到看到完整答案的總等待時間。

我的解讀是，Lily 最有說服力的成績在 decode。即使換成較新的 MLX 0.32.2，它在所有測試長度仍快約 23.6%～31.9%。Prefill 則沒有同樣穩定的優勢，尤其是 32K 以上的長內容。若後續 MLX 持續更新，Lily 也必須同步最佳化，專用引擎才能維持領先。

## Lily 與 Perplexity Hybrid Compute 有什麼關係？

Lily 的開發背景來自 Perplexity Computer 的 Hybrid Compute，也就是「混合運算」。這套架構不要求所有工作都在雲端或本機完成，而是依照任務性質分工。

雲端模型負責複雜推理、網路搜尋與規劃。Mac 上的本機模型則處理私人檔案、敏感資訊與裝置操作。Perplexity 表示，Mac 端還有一個隱私閘門，會辨識姓名、地址、帳號與憑證等敏感內容，再決定要留在本機、遮罩後送到雲端、拒絕動作，或先詢問使用者同意。

對使用者來說，這比「全部改成本機 AI」更實際。最強的雲端模型仍可處理需要大量運算與即時網路資料的工作，本機模型則成為資料邊界的守門員。

但 Hybrid Compute 不等於完全離線。只要任務需要雲端推理或網路搜尋，仍會有資料離開裝置。真正該問的是哪些資料會留在本機、哪些會被遮罩、什麼情況會送往雲端，以及管理者能否稽核這些流程。

![Perplexity Computer 以雲端模型與 Mac 本機模型並行處理工作的官方畫面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/perplexity-hybrid-compute-official.jpg)

Perplexity Computer 可讓雲端模型研究公開資料，同時由 Mac 本機模型處理私人檔案。 圖片來源： Perplexity 官方 Hybrid Compute 貼文串 。

## 一般 Mac 使用者可以用 Lily 嗎？先分清楚兩條路

Perplexity 官方 App 的 Hybrid Compute 與自行編譯 Lily，硬體門檻並不相同。

### 路線一：在 Perplexity Mac App 使用 Hybrid Compute

Perplexity 表示，Hybrid Compute 已提供給 Pro、Max 與 Enterprise 訂閱者。官方產品需求是 Apple silicon Mac、macOS 15 以上，以及至少 24 GB 統一記憶體。使用者可以在 App 內一鍵下載本機模型，再從模型選擇器設定本機與雲端模型。

這條路適合想直接使用功能的人。官方目前提供 Gemma 4 E4B、Qwen3.6-35B-A3B 與一款 Perplexity 模型，但不代表這三款模型都由開源版 Lily 執行。

### 路線二：下載 Lily 原始碼自行執行

截至 2026 年 9 月 3 日，Lily 的公開版本要求 Apple GPU family 10 以上，也就是 M5 或更新晶片，並需要 macOS 26 以上與 Rust 1.92。模型也必須是指定的 Qwen3.6-35B-A3B MLX affine 4-bit、group size 64 版本。

它目前不接受 GGUF、AWQ、GPTQ、BF16、int8、fp8，也不支援較小或 dense 版本的 Qwen。Dense 模型是每次推論都會使用全部主要參數的模型，和 Lily 目前鎖定的 MoE 架構不同。

Lily 雖然提供 OpenAI 相容的 Chat Completions API，但只涵蓋最小功能。它支援純文字訊息與非串流回應，並固定使用 greedy decoding，也就是每一步都選機率最高的 token。串流輸出、工具呼叫、回應格式控制、多模態內容、抽樣參數與 speculative decoding 都會被拒絕。

因此，「有 OpenAI 相容 API」不代表現有應用能直接無痛替換。開發者仍要先確認請求格式與功能是否落在 Lily 支援的範圍內。

## Lily 開源後，對本機 AI 發展有什麼意義？

Lily 採用 Apache License 2.0，使用的 Qwen3.6-35B-A3B 模型也採 Apache License 2.0。只要遵守授權中的聲明與再散布條件，開發者可以研究、修改與整合程式碼。

我認為 Lily 現階段最有價值之處，是把 Perplexity 如何針對 Apple GPU、MoE 路由、量化權重與注意力快取做最佳化公開出來。它目前還稱不上全民本機 AI 工具，但對其他團隊而言，已是一份可以重現與延伸的工程參考。

它的最大限制也來自同一個選擇：為單一模型壓榨效能，必然犧牲相容性。專案未來如果能擴大到更多模型、更多 Apple 晶片，並補上串流、工具呼叫與多模態功能，才有機會從技術展示走向更通用的本機服務。

因此，目前的結論很明確：

- 一般 Perplexity 使用者想保留敏感資料，優先使用官方 Hybrid Compute，不需要自行編譯 Lily。
- 使用 M5 Mac、剛好部署指定 Qwen 模型的開發者，Lily 值得測試，尤其是重視生成速度的服務。
- 需要多模型、舊款 Apple silicon、GGUF、圖像輸入或完整 OpenAI API 功能的人，現階段仍應選擇 MLX-LM、llama.cpp 或其他通用工具。

延伸閱讀： [幣安 AI Agent Skills 是什麼？功能總整理與加密貨幣自動交易教學](https://www.rayskyinvest.com/138683/binance-ai-agent-skills?ref=aiposthub.com)

## 常見問題

### Lily 是新的 AI 模型嗎？

不是。Lily 是本機推論引擎，負責在 Apple silicon 上執行模型。目前它只支援指定格式的 Qwen3.6-35B-A3B 4-bit 權重。

### Lily 可以在 M1、M2、M3 或 M4 Mac 上執行嗎？

公開版 Lily 的 README 要求 M5 或更新晶片，因此不支援舊款 Apple silicon。不過 Perplexity 官方 App 的 Hybrid Compute 需求較寬，只要是 Apple silicon、macOS 15 以上並具備至少 24 GB 統一記憶體即可。兩者不能混為一談。

### Lily 比 MLX-LM 快多少？

依 Perplexity 在 M5 Max 上對 MLX 0.32.2 的測試，Lily 的 decode 吞吐量約快 23.6%～31.9%。Prefill 在短提示可快約 3.0%～16.6%，但從 32K 長度開始落後，128K 時約慢 19.9%。這是單一硬體、單一模型與特定測試方法的結果，不代表其他 Mac 或模型也有相同比例。

### 使用 Hybrid Compute，資料就完全不會上傳雲端嗎？

不一定。Hybrid Compute 的設計就是讓本機與雲端分工。敏感內容可由 Mac 端保留、遮罩、拒絕或先要求同意，但網路搜尋與複雜推理仍可能交給雲端模型。使用者與企業管理者仍要確認實際資料規則。

### Lily 可以取代 Ollama 或 LM Studio 嗎？

現階段不適合。Ollama、LM Studio 與 MLX-LM 強調多模型與使用便利性，Lily 則鎖定特定 Qwen 模型、M5 以上晶片與有限 API。它比較像專用高效能元件，而不是一般使用者的模型管理工具。

## 結語：Lily 值得注意，但真正的重點是混合運算

Lily 最容易吸引目光的是「比 MLX-LM 更快」，但新版比較也提醒我們，效能領先會隨基準框架更新而改變。它目前較穩定的優勢是 decode，而不是所有階段、所有長度都更快。

更長期的意義，是 AI 產品開始把雲端與本機視為同一條工作流程。大型模型留在雲端提供推理能力，裝置端模型負責敏感資料與即時操作，推論引擎則決定本機這一段會不會成為瓶頸。

Lily 仍然很專用，但它把這條路線需要的工程細節公開了。一般使用者現在不必急著下載原始碼。本機 AI 開發者則可以把它當成研究起點，但仍須在自己的硬體與工作負載上重新測試。