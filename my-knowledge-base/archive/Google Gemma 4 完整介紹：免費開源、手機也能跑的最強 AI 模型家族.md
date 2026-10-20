---
title: "Google Gemma 4 完整介紹：免費開源、手機也能跑的最強 AI 模型家族"
source: "https://rar.design/posts/google-gemma-4-open-source-ai-model"
author:
  - "[[設計師 Riven]]"
published: 2026-04-06
created: 2026-05-15
description: "Google DeepMind 發布 Gemma 4 開源模型家族，基於 Gemini 3 技術打造，首次採用 Apache 2.0 自由授權。提供 E2B、E4B、26B MoE、31B Dense 四種尺寸，支援多模態輸入、Function Calling 與 140+ 語言。本文介紹規格差異、..."
tags:
  - "clippings"
---
基於 Gemini 3 技術、Apache 2.0 授權、四種尺寸從手機到工作站全覆蓋，設計師也該認識的新世代開源 AI

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年4月6日 下午 9:17

AI 設計

![Google Gemma 4 完整介紹封面](https://s.teachifycdn.com/image/width=2048,quality=80/attachment/public_image/e23cc8db-8c2c-498e-9009-22f44ef048c2/e76b85c5-0b15-493d-88e3-3b2b8de1c236.png)

Google Gemma 4 完整介紹封面

**📑 文章目錄**

[1\. Gemma 4 是什麼？跟 Gemini 有什麼關係](#section1)

[2\. 四種模型規格一次看](#section2)

[3\. 核心功能亮點](#section3)

[4\. Benchmark 表現有多強](#section4)

[5\. 跟 Qwen 3.5、Llama 4 怎麼選](#section5)

[6\. 怎麼開始使用](#section6)

[7\. 設計師可以怎麼用 Gemma 4](#section7)

[8\. 注意事項與限制](#section8)

[9\. 重點整理](#section9)

Google DeepMind 在 2026 年 4 月 2 日正式發布了 **Gemma 4** ——目前為止最強的開源 AI 模型家族。如果你一直在觀望「什麼時候才有一個真正免費、性能又夠強的 AI 模型」，這次可以認真看一下了。

Gemma 4 不只是一個模型，它是一整個家族：從能跑在手機上的 2B 迷你版，到能處理 256K 超長文本的 31B 大模型，全都採用 Apache 2.0 開源授權，代表你可以拿來做任何商業用途，完全不受限。

## Gemma 4 是什麼？跟 Gemini 有什麼關係

![Google Gemma 4 官方視覺](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemma-4_blog_keyword_meta-dark.width-1300.png)

Google Gemma 4 官方視覺

▲ Google DeepMind 發布的 Gemma 4 官方視覺（圖片來源：Google 官方部落格）

先釐清一個常見的混淆： **Gemini 是 Google 的閉源商業模型** （像 ChatGPT 那樣由 Google 自己營運），而 **Gemma 是 Google 開源釋出的模型** ，讓開發者可以自己下載、自己跑、自己改。

Gemma 4 的技術底層來自 Gemini 3 的研究成果，可以理解成 Google 把旗艦級的 AI 技術「拆包」成適合在各種裝置上運行的開源版本。自第一代 Gemma 發布以來，全球開發者已經下載超過 4 億次，社群也衍生出超過 10 萬個變體版本，生態系非常活躍。

**💡 關鍵差異**

**Gemini** = Google 閉源 API 服務（像 ChatGPT） **Gemma** = Google 開源模型（你可以自己跑，免費商用）  
Gemma 4 這次從限制性的 Google 自家授權改為 **Apache 2.0** ，完全自由。

## 四種模型規格一次看

Gemma 4 提供四種尺寸，從手機端到工作站都能覆蓋。這裡的「E」代表 Effective（有效參數），是 Google 用了一種叫 Per-Layer Embeddings (PLE) 的技術，讓模型在推理時只啟動一部分參數，大幅降低記憶體和電量消耗。

| 規格 | E2B | E4B | 26B MoE | 31B Dense |
| --- | --- | --- | --- | --- |
| 架構 | Dense + PLE | Dense + PLE | MoE（128 專家） | Dense |
| 有效參數 | ~2B | ~4B | ~4B active | 31B |
| Context Window | 128K | 128K | 256K | 256K |
| 多模態 | 文字、圖片、音訊 | 文字、圖片、音訊 | 文字、圖片、影片 | 文字、圖片、影片 |
| 適用裝置 | 手機 / IoT | 手機 / 筆電 | 24GB GPU | 80GB GPU |
| Arena AI 排名 | — | — | **#6** | **#3** |

特別值得注意的是 26B MoE 版本，它使用 128 個小型專家模組、每次推理只啟動其中 8 個加上 1 個共享專家。這跟 Llama 4 的 16 大專家路線完全不同——Google 選擇了「多而小」的策略，在只有約 4B 有效參數的前提下達到接近 31B 的品質。

## 核心功能亮點

先看 Google 官方的發布影片，2 分鐘快速掌握 Gemma 4 的能力輪廓：

![](https://www.youtube.com/watch?v=jZVBoFOJK-Q)

▲ What's new in Gemma 4（影片來源：Google for Developers 官方頻道）

**🧠 思維鏈推理**

內建可配置的 Thinking Mode，模型會先逐步推理再回答。在 AIME 數學競賽題上從 Gemma 3 的 20.8% 飆升到 89.2%，這不是漸進式進步，是世代級的跳躍。

**👁️ 原生多模態**

所有模型都能理解圖片（支援不同長寬比和解析度），大模型可處理影片（60 秒、1fps），小模型還支援音訊輸入。OCR、圖表解讀、UI 元素偵測全都做得到。

**🔧 原生 Function Calling**

內建工具呼叫和結構化 JSON 輸出，可以自主完成多步驟任務。這讓 Gemma 4 不只是聊天模型，而是可以驅動 AI Agent 的引擎。

**🌍 140+ 語言支援**

包含繁體中文、日文在內的 140 種以上語言原生支援，配合離線運行能力，在沒有網路的環境下也能工作。

## Benchmark 表現有多強

Gemma 4 相比 Gemma 3 的進步幅度，在開源 AI 史上相當罕見。以下是幾個關鍵指標的對比：

**📊 Gemma 3 vs Gemma 4（31B）關鍵指標對比**

AIME 數學

20.8% → 89.2%

GPQA 科學推理

42% → 84%

BigBench XH

19.3% → 74.4%

LiveCodeBench

~35% → 80%

灰色 = Gemma 3｜彩色 = Gemma 4（皆為 thinking mode 成績）

![Gemma 4 Arena AI 評分排名圖](https://storage.googleapis.com/gweb-uniblog-publish-prod/documents/gemma-4__elo-score__eval__dark_Web.png)

Gemma 4 Arena AI 評分排名圖

▲ Gemma 4 在 Arena AI 排行榜的 ELO 分數表現（圖片來源：Google 官方部落格）

在 Arena AI 的文字排行榜上，31B Dense 模型拿下全球開源第三名（ELO 1452），26B MoE 則是第六名。值得注意的是，它們擊敗了許多參數量是自己 20 倍的模型。換句話說， **Gemma 4 的「每參數智能」是目前開源模型中最高的** 。

## 跟 Qwen 3.5、Llama 4 怎麼選

2026 年 4 月是開源 AI 史上最擁擠的月份。Gemma 4、Qwen 3.6 Plus、Llama 4 同時在場上。以下是各家的定位差異：

| 面向 | Gemma 4 | Qwen 3.5 | Llama 4 |
| --- | --- | --- | --- |
| 授權 | Apache 2.0 | Apache 2.0 | Meta 自訂 |
| 最長 Context | 256K | 1M (Qwen 3.6+) | 10M |
| 邊緣裝置部署 | ★★★★★ | ★★★☆☆ | ★☆☆☆☆ |
| 程式碼能力 | ★★★★☆ | ★★★★★ | ★★★★☆ |
| 多語言 | 140+ 語言 | 201 語言 | — |
| 最小可用模型 | E2B（手機） | 0.8B | Scout 109B |

**簡單的選擇指南：** 如果你需要在手機或邊緣裝置上跑 AI，選 Gemma 4；如果你主要做程式碼相關工作，Qwen 3.5 目前仍然更強；如果你需要處理超長文本（整個程式庫或大量文件），Llama 4 Scout 的 10M context 無人能比。

但也要注意，社群在 Gemma 4 發布後 24 小時內就發現了一些實際問題：MoE 模型的推理速度偏慢（同 GPU 上 Qwen 3.5 可以跑到 60+ tokens/s，Gemma 4 26B MoE 只有約 11 tokens/s），部分 fine-tuning 工具還不完全支援，Mac 上長時間運行有穩定性問題。這些都是剛發布的陣痛期，預計會逐步改善。

## 怎麼開始使用

Google 讓入門門檻變得非常低，以下是幾種方式：

**1**

**零設定體驗：Google AI Studio**

在 aistudio.google.com 直接選擇 Gemma 4 模型（支援 31B 和 26B MoE），打開瀏覽器就能用，不需要任何安裝。

**2**

**本機運行：Ollama / LM Studio**

安裝 Ollama 0.20+ 後，一行指令就能把模型拉到本機跑。E4B 在 8GB 筆電上就能運行，26B MoE 需要 24GB GPU（量化後）。

**3**

**開發整合：Hugging Face / Kaggle**

從 Hugging Face、Kaggle 下載模型權重，用 transformers 5.5.0+ 就能在 Python 裡直接呼叫。支援 vLLM、llama.cpp、MLX 等主流推理引擎。

**4**

**手機體驗：Google AI Edge Gallery**

iOS 和 Android 都有的 App，直接在手機上離線跑 E2B 和 E4B 模型，還能體驗 Agent Skills 等 agentic 功能。

## 設計師可以怎麼用 Gemma 4

雖然 Gemma 4 主要面向開發者，但它的多模態能力對設計師來說有幾個值得關注的應用方向：

**🖼️ 設計稿分析**

Gemma 4 能理解圖片中的 UI 元素、偵測按鈕和文字位置。你可以餵入設計稿截圖，讓它自動描述介面結構，甚至生成對應的 HTML/CSS 程式碼。

**📝 離線內容生成**

在飛機上、高鐵上沒有網路的時候，用筆電本機跑 Gemma 4 E4B，一樣可以寫文案、翻譯、整理素材。對常常在移動中工作的創作者來說很實用。

**🤖 自動化工作流**

原生的 Function Calling 和 Agent 能力，讓 Gemma 4 可以整合進你的自動化工具鏈。比如自動分析客戶回饋、分類設計素材、批次處理文件等。

**🔒 隱私安全的 AI 使用**

本機運行意味著你的設計稿、客戶資料完全不會上傳到任何伺服器。對於有 NDA 約束或處理敏感素材的設計師，這是 API 服務做不到的。

## 注意事項與限制

Gemma 4 雖然強大，但有幾件事需要知道：

**⚠️ 推理速度**

MoE 版本目前的推理速度不如 Qwen 同級模型，社群正在積極優化中。如果你需要高速推理，建議先用 Dense 31B 或等待後續優化。

**⚠️ Fine-tuning 生態**

部分主流微調工具還在更新支援中，Axolotl 已經跟上，但其他框架可能需要等幾天到幾週。如果你打算做領域微調，先確認工具鏈是否就緒。

**⚠️ 硬體需求**

31B Dense 完整運行需要 80GB GPU（如 NVIDIA H100），量化後可以在消費級顯卡上跑。E2B 最小只需 4GB RAM。

**⚠️ 只有文字輸出**

Gemma 4 可以「看」圖片、影片和聽音訊，但輸出只有文字。它不能生成圖片或影片。

### 📝 重點整理

→

Gemma 4 是 Google 基於 Gemini 3 技術打造的開源模型家族，首次採用 Apache 2.0 授權，完全自由商用

→

提供 E2B、E4B、26B MoE、31B Dense 四種尺寸，從手機到工作站都能覆蓋

→

31B 模型在 Arena AI 排名全球開源第三，AIME 數學成績從 20.8% 跳到 89.2%

→

原生多模態（圖片 + 影片 + 音訊）、Function Calling、140+ 語言支援

→

邊緣裝置部署是 Gemma 4 的最大優勢；程式碼能力 Qwen 仍較強；超長文本看 Llama 4

→

可在 Google AI Studio 免費體驗，或用 Ollama 一行指令在本機運行

## 延伸資源

🔗 [Google 官方部落格公告](https://blog.google/innovation-and-ai/technology/developers-tools/gemma-4/)

🔗 [Google DeepMind — Gemma 4 模型頁面](https://deepmind.google/models/gemma/gemma-4/)

🔗 [Hugging Face — Gemma 4 技術介紹與使用指南](https://huggingface.co/blog/gemma4)

🔗 [Google AI for Developers — Gemma 文件](https://ai.google.dev/gemma)

🔗 [Google AI Studio（免費線上體驗）](https://aistudio.google.com/)

![AI 覺醒設計應用攻略](https://s.teachifycdn.com/image/width=800,quality=80/membership_plan/cover_photo/04577eaf-3b7a-49e9-9e85-3279c3240e6d/cf891fca-7d2e-4e0f-994c-2d21e2631502.jpg)

AI 覺醒設計應用攻略

**訂閱制** ✨ AI × 設計趨勢

### AI 覺醒設計應用攻略｜RAR 設計攻略訂閱方案

AI 工具每週都在更新，我幫你過濾雜訊，只整理 **對設計師真正有用的 AI 應用** 。

✦ AI 工具實測與工作流分享

✦ 設計師角度的 Prompt 技巧

✦ 每週精選 AI × 設計新知

目前尚無留言