---
title: "PewDiePie 開源自家 AI 工作空間 Odysseus，主打「你的隱私不該交給大公司」"
source: "https://www.koc.com.tw/archives/644829"
author:
  - "[[達小編]]"
published: 2026-06-04
created: 2026-06-10
description: "PewDiePie 在發布影片中直言：「我打造 Odysseus 是因為我相信隱私和自托管。這些科技公司濫用你的私人資料，你的地址、電話號碼、親屬資訊，一個令人不安的程度的資訊被儲存並在公司之間交易。」他更撂下一句：「對大科技公司的戰爭才剛開始。」"
tags:
  - "clippings"
status: processed
---
全球最具影響力的 YouTuber 之一 PewDiePie（本名 Felix Arvid Ulf Kjellberg），在 5 月 31 日透過 YouTube 影片「MY trillion $Dollar Project is finally OUT!」正式發布了他耗時約一年打造的開源專案： **Odysseus** 。這是一個免費、開源、完全自托管的 AI 工作空間，定位為 ChatGPT 與 Claude 的本機替代方案。專案上線 GitHub 不到 3 天，就從零衝到超過 42,000 顆 Star、4,900 個 Fork，成長速度驚人。  

PewDiePie 在發布影片中直言：「我打造 Odysseus 是因為我相信隱私和自托管。這些科技公司濫用你的私人資料，你的地址、電話號碼、親屬資訊，一個令人不安的程度的資訊被儲存並在公司之間交易。」他更撂下一句：「對大科技公司的戰爭才剛開始。」

![](https://www.youtube.com/watch?v=rAzT5lcezPs)

### Odysseus 是什麼？功能全覽

Odysseus 的 GitHub 頁面將其描述為「自托管的語言模型介面」，但實際功能遠超這個一句話定義。它是一個模組化的 AI 生產力套件，核心功能包括：

- **Chat（聊天）** ：支援連接任何本機模型或外部 API，包括 vLLM、llama.cpp、Ollama、OpenRouter、OpenAI 等多種後端
- **Agent（代理）** ：不是只能對話的聊天機器人，而是能執行真實任務的自主代理，具備網頁瀏覽、檔案編輯、Shell 指令、MCP 協定支援、持久記憶等能力。底層建構於 `opencode` 專案之上
- **Cookbook（模型廚房）** ：掃描使用者硬體配置，推薦適合的模型，一鍵下載並啟動服務。內建超過 270 個開源模型的目錄，支援 GGUF、FP8、AWQ 等格式
- **Deep Research（深度研究）** ：多步驟的研究流程，自動蒐集、閱讀、綜合多個來源，生成視覺化報告。改編自阿里巴巴的 Tongyi DeepResearch
- **Compare（模型比較）** ：多模型盲測對比工具，消除偏見
- **Documents（文件編輯器）** ：支援 Markdown、HTML、CSV 的多分頁編輯器，AI 輔助編輯但以人為主導
- **Memory / Skills（記憶與技能）** ：基於 ChromaDB 的持久記憶系統，搭配 fastembed 向量嵌入，代理會隨使用時間進化
- **Email（郵件管理）** ：IMAP/SMTP 收件匣，內建 AI 分類、自動摘要、自動草稿回覆、垃圾郵件過濾
- **Notes & Tasks（筆記與任務）** ：快速筆記、待辦清單、排程任務，支援 ntfy、瀏覽器、Email 通知
- **Calendar（行事曆）** ：本機行事曆，支援 CalDAV 同步（Radicale、Nextcloud、Apple、Fastmail）
- **Mobile Support（手機支援）** ：響應式設計，可安裝為 PWA，支援觸控手勢

PewDiePie 在影片中特別強調：「基本上，它是 Claude 和 ChatGPT 的網頁 UI，但是自托管的。你獲得所有功能，但更好。」  

### 隱私至上：本地優先的技術架構

Odysseus 與 ChatGPT、Claude、Gemini 等商業服務最大的差異在於資料處理方式。所有主流 AI 助理都運行在雲端基礎設施上，使用者的提示詞、對話紀錄、上傳的文件都會經過這些公司的伺服器。Odysseus 的設計原則是： **除非你明確連接外部服務，否則沒有任何資料離開你的機器** 。

官方描述寫得很直白：「Local-first, privacy-first, and no telemetry. Just you and your models.」沒有遙測追蹤、沒有背景數據收集、沒有帳號綁定。使用者可以選擇連接本地模型（透過 Ollama、vLLM、llama.cpp）或接入外部 API（OpenRouter、OpenAI），但預設一切都是離線運行。

安裝方式有兩種：Docker（推薦）或原生安裝。Docker 方式只需 clone、 `docker compose up -d --build` ，就能在 `localhost:7000` 啟動。原生安裝則需要 Python 3.11+。針對 Apple Silicon 使用者，官方提供了專門的 `start-macos.sh` 腳本，支援 Metal GPU 加速。

有趣的細節是，根據專案貢獻者在 Hacker News 上的 [留言](https://news.ycombinator.com/item?id=48346693) ，Odysseus 的一部分功能「是從手機上開發的」，使用 Termux 行動 Shell 環境和 PWA 安裝，因此手機體驗並非事後才加入的功能。  

### 技術底層：站在巨人肩膀上的整合

Odysseus 並非從零開始打造所有功能。PewDiePie 坦率地承認他「借鑑」了許多現有開源專案：

- Agent 系統底層建構於 `opencode` 專案
- Deep Research 功能改編自阿里巴巴的 Tongyi DeepResearch
- Cookbook 模型推薦系統基於 `llmfit` 專案
- 記憶系統使用 ChromaDB + fastembed（ONNX）
- 專案語言為 JavaScript，授權為 MIT

Hacker News 社群對此有不同看法。有使用者指出：「這本質上是一個 Python UI 套在其他開源專案上面」，認為真正的價值在底層引擎而非 UI 層。 但也有使用者認為， **整合本身就是價值** ，把散落在各處的開源工具統一到一個連貫的介面中，降低使用門檻，這件事本身就有意義。

### pewdiepie-archdaemon/odysseus – GitHub

### 「我不想讓 AI 替我工作」

PewDiePie 在影片中反覆傳達一個核心理念：AI 應該是工具，不是替代品。Odysseus 的文件編輯器設計就體現了這一點，使用者自己撰寫文字，AI 在旁邊輔助修改、查證、改善，而不是直接代寫。

「我不想要 AI 替我工作或取代我的工作。我想自己做，如果我需要，它可以幫忙。」PewDiePie 這樣說道。他也半開玩笑地表示正在「建造自己的 Photoshop」，雖然承認目前還遠不如 Adobe 的產品，但強調重點在於： **Odysseus 是你的，它永遠不會收費** 。

專案目前支援 Linux、Windows、macOS 甚至手機端，PewDiePie 說這歸功於社群的集體貢獻。他邀請使用者一起參與開發：「我發布它是因為我希望你們幫我繼續打造它。」

無論如何，3 天內 42,000 顆 Star 的數據說明了一件事：市場對於「不把資料交給大公司」這件事有真實需求。在 ChatGPT Plus 每月 20 美元（約新台幣 640 元）、Claude Pro 每月 20 美元的訂閱制時代，一個完全免費、開源、MIT 授權的替代方案，即使不完美，也足以讓人想試試看。