---
title: "Hermes Desktop 讓 Windows 直接就能安裝使用 Hermes Agent！初學者也能自己搞定"
source: "https://www.koc.com.tw/archives/642822"
author:
  - "[[電腦王阿達]]"
published: 2026-05-16
created: 2026-05-22
description: "Hermes Desktop 的功能涵蓋範圍相當全面，幾乎覆蓋了 Hermes Agent 的所有能力："
tags:
  - "clippings"
status: processed
---
近期因為穩定且具備自我進化能力的開源 **[AI Agent](https://www.koc.com.tw/archives/tag/ai-agent)** 專案： [**Hermes Agent**](https://www.koc.com.tw/archives/tag/hermes-agent) ，深受廣大使用者喜歡，但因為主要還是以 CLI 介面為主，對於一般用戶來說來是有一定的安裝使用門檻，不過近日由開發者 Fathah 打造的 **Hermes Desktop** 在將 Hermes 改為可直接在 Windows 上安裝使用的軟體， 安裝後使用者不再需要面對終端機畫面敲打指令，而是透過直覺的 GUI 完成 Hermes Agent 的安裝、設定、對話、工具管理與排程任務等所有操作。對於想使用 Hermes Agent 的初學者來說相當實用。  

## Hermes Desktop 是什麼？

根據 README 文件，Hermes Desktop 是一套為 Hermes Agent 設計的桌面伴侶應用程式。 它取代了傳統 CLI 的手動管理流程，將安裝嚮導、供應商設定、日常對話、工作階段管理、個人檔案切換、記憶系統、技能管理、排程任務、訊息閘道設定等功能全部整合在一個圖形化介面中。  

專案使用 TypeScript 開發，採用 MIT 授權，支援 macOS（.dmg）、Windows（.exe）與 Linux（.AppImage/.deb/.rpm）三大平台。最新版本 v0.4.3 的各平台安裝檔合計已累積超過 5,000 次下載。

### Hermes Desktop 官方 Github

## Hermes Desktop 簡易安裝教學

首先去 Hermes Desktop 的 releases 頁面下載最新版本（目前是 v0.4.3），支援 Windows、Mac、Linux平台，Windows使用者請下載後綴為 setup.exe 的執行檔：  

接著執行安裝檔，由於安裝的人數還不夠多，windows 會跳警告，請點「其他資訊」後直接安裝（擔心的人就別用吧）：  
  

接下來開始安裝，選擇「get started」，根據專案說明，首次啟動的流程經過精心設計。應用程式會先詢問使用者要採用本地模式還是遠端模式（下方的選項）。若選擇本地模式，它會自動檢查 Hermes 是否已安裝；若尚未安裝，則執行官方安裝腳本，自動處理 Git、uv、Python 3.11+ 等依賴項目。如果你現在已經有在用 CLI 版本的 Hermes 使用者，Hermes Desktop 也提供了「遠端模式」，只需輸入雲端 Hermes API 伺服器的網址與 API Key，就能將桌面應用程式當作遠端代理的前端介面使用，適合在伺服器上執行 Hermes 然後從桌面端操作的情境（就是用本地端UI去操控雲端 Hermes）：  

  

跑完繼續執行「Continue to Setup」：  

安裝完成後，使用者會進入供應商設定畫面，選擇要使用的 LLM 供應商（如 OpenRouter、Anthropic 或 OpenAI），輸入 API Key，然後直接進入主工作區開始對話。整個過程不需要開啟終端機，也不需要手動編輯任何設定檔。開始設定模型時，大家輸入自己的 API KEY 就好，沒有的話就去 OpenRouter 申請一個帳號後把 API KEY 填進去（應該是最無腦的方法），綁卡額度還會比較多，我實測本地端的 Qwen 3.6 27B 也能順利跑起來。：  
  

不會用 OpenRouter 的可以看這篇：

<iframe title="Claude Cowork 現在也能使用第三方 API、本地 LLM 模型，教你怎麼設定 — 電腦王阿達" src="https://www.koc.com.tw/archives/640513/embed#?secret=ljYL6pV4RE#?secret=YoUk8RSC3y" width="500" height="287" frameborder="0"></iframe>

接著就會出現一個運行介面，跟跑 WebUI 介面的 Hermes 差不多，一樣有對話與各種功能介面，不過 Hermes Desktop 內容更加豐富：  

可以去 SETUP 中切換語系，不過目前只有簡體中文，等網路大大加入繁中了：  

### 核心功能一覽

Hermes Desktop 的功能涵蓋範圍相當全面，幾乎覆蓋了 Hermes Agent 的所有能力：

- **引導式首次安裝** ：初次啟動時自動檢查系統依賴（Git、uv、Python 3.11+），若 Hermes 尚未安裝則執行官方安裝腳本，並顯示進度追蹤。
- **本地或遠端後端** ：支援在本地（127.0.0.1:8642）執行 Hermes，或連接遠端 Hermes API 伺服器（輸入 URL 與 API Key 即可）。
- **串流對話 UI** ：完整的 SSE 串流聊天介面，包含工具執行進度指示器、Markdown 渲染、程式碼語法高亮，以及即時的 Token 用量與成本顯示。
- **22 個斜線指令** ： `/new` 、 `/clear` 、 `/fast` 、 `/web` 、 `/image` 、 `/browse` 、 `/code` 、 `/model` 、 `/memory` 、 `/compress` 等，幾乎將 CLI 的所有能力搬進 GUI。
- **14 組工具集** ：網頁搜尋、瀏覽器控制、終端機、檔案操作、程式碼執行、視覺分析、圖片生成、語音合成（TTS）、記憶、技能、工作階段搜尋、委派任務等。

- **16 種訊息平台閘道（網關）** ：Telegram、Discord、Slack、WhatsApp、Signal、Matrix、Email（IMAP/SMTP）、SMS（Twilio/Vonage）、iMessage、釘釘、飛書、企業微信、微信、Webhooks、Home Assistant。

- **排程任務（Cron Job）** ：圖形化的排程任務建立器，支援分鐘、小時、每日、每週與自訂 Cron 表達式，搭配 15 種交付目標。
- **個人檔案切換** ：建立、刪除與切換不同的 Hermes 環境，每個環境擁有獨立的設定配置。
- **記憶系統** ：檢視與編輯記憶條目、使用者個人資料記憶，並支援 Honcho、Hindsight、Mem0 等多種記憶供應商。
- **角色設定編輯器** ：直接編輯當前個人檔案的 SOUL.md 個性設定檔。
- **多供應商支援** ：OpenRouter、Anthropic、OpenAI、Google Gemini、xAI Grok、Nous Portal、Qwen、MiniMax、Hugging Face、Groq，以及任何 OpenAI 相容的本地端點（LM Studio、Ollama、vLLM、llama.cpp），管理模型比 CLI 介面直覺很多。

- **備份與匯入** ：完整的資料備份/還原與系統診斷功能，直接從設定頁面操作。

剩下功能大家可以自己玩玩，原則上都跟我們之前的教學一樣，只是多了圖形介面，可在 Windows 底下不需 WSL2 就能跑，不過要注意的是，如果你執行了工作區或特定功能時跳出的 CMD 視窗不可以關閉，因為那些都是背景在運行的工作流。如果有其他不懂的如 TELEGRAM 等傳訊軟體的設定可以去看我們之前的教學影片：

![](https://www.youtube.com/watch?v=Gjd9JoiTZds)

### 當前版本與未來展望

截至本文撰寫時，Hermes Desktop 最新版本為 v0.4.3（2026 年 5 月 15 日發布），仍處於積極開發階段。專案作者在 README 中明確標示「This project is in active development」，並鼓勵使用者遇到問題時開 Issue。

從功能覆蓋率來看，v0.4.3 已經包含了絕大多數 Hermes Agent 的核心功能，但仍有改進空間。目前的已知限制包括：Windows 安裝檔缺少程式碼簽章（SmartScreen 會跳出警告）、Fedora RPM 不支援自動更新、macOS 版本未公證（需執行 xattr 繞過 Gatekeeper）等。這些都是 Electron 應用的常見痛點，隨著版本迭代應能逐步改善。

與此同時，NVIDIA 也在近期 [宣布](https://blogs.nvidia.com/blog/rtx-ai-garage-hermes-agent-dgx-spark/) Hermes Agent 將登陸 RTX AI PC 平台，進一步擴大了 Hermes 的硬體生態版圖。 Hermes Desktop 作為前端介面，未來若能與 NVIDIA RTX 的本地 AI 加速深度整合，將為開源 AI 代理的日常應用開創更多可能性。

### 小結

Hermes Desktop 是開源 AI 代理生態系中的關鍵補完拼圖。它將一個功能強大的 CLI-only 代理包裝成直覺易用的桌面應用程式，讓不熟悉命令列的使用者也能充分發揮 Hermes Agent 的自動化能力。

對於已經在使用 Hermes Agent 的人來說，Hermes Desktop 提供了一個更現代、更直覺的操作介面；對於還在觀望的人來說，它消除了「必須會用終端機」這個最大的入門障礙。隨著專案持續迭代與社群貢獻的增加，Hermes Desktop 有潛力成為開源 AI 代理前端的新標竿。但你如果已經有 Mac 或 Linux 主機在跑 Hermes 的話，其實也不用換，因為 Hermes Desktop 跟 Windows 一起運行，電腦如果關機了他也沒作用了，它目前就是適合不會虛擬機、沒有 Mac 的用戶嚐鮮使用。