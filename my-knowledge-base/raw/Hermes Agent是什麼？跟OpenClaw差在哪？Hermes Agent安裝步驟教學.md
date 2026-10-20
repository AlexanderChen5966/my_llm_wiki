---
title: "Hermes Agent是什麼？跟OpenClaw差在哪？Hermes Agent安裝步驟教學"
source: "https://www.bnext.com.tw/article/90591/hermes-agent-procedural-memory"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-04-10
created: 2026-05-18
description: "Hermes Agent 是開源自學習 AI 代理框架，一行指令安裝、支援 200 多種模型切換，讓你的 AI 助理越用越聰明。"
tags:
  - "clippings"
status: processed
---
> **重點一** ：Hermes Agent（愛馬仕Agent）是 Nous Research 開發的開源 AI 代理框架，最大特色是能自動從任務中萃取技能、跨對話記憶、並建立使用者個人檔案，越用越懂你。  
>   
> **重點二** ：安裝僅需一行指令，支援 Linux、macOS 與 WSL2，可自由切換 OpenAI、Anthropic、OpenRouter 等超過 200 種模型，不綁定任何供應商。  
>   
> **重點三** ：內建 Telegram、Discord、Slack 等六大通訊平台閘道，以及排程自動化功能，適合需要重複執行任務、希望 AI 持續進化的進階使用者。

大多數 AI 聊天工具每次對話都從零開始，你昨天教它的偏好、上週給過的指令，今天通通忘光。Hermes Agent 想解決的就是這個問題。

## Hermes Agent是什麼？

[Hermes Agent](https://github.com/nousresearch/hermes-agent) 是由 AI 研究機構 Nous Research 開發的開源 AI 代理框架，採 MIT 授權。

它的核心概念是「程序性記憶」（Procedural Memory）： **每次完成複雜任務後，系統會自動將成功的推理模式萃取為可重複使用的「技能」（Skill），下次遇到類似任務時直接調用、並在使用過程中持續優化。**

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

簡單來說，傳統 AI 助理像是每天換一個新實習生，Hermes Agent 則像一位跟著你越做越熟練的長期助理。

## Hermes Agent核心拆解

### 它怎麼「學會」做事？

Hermes Agent 的核心是一套學習迴圈，拆開來看有五個階段：

1. **執行任務** ：跟其他 AI 工具一樣，接到指令後拆解目標、呼叫工具、產出結果。
2. **評估結果** ：任務結束後，系統會判斷這次做得好不好。你直接採用了？那代表方法有效。你改了幾個地方？那代表某些環節要調整。
3. **萃取技能** ：如果這次的做法有效、而且不是隨便就能想到的，系統會把推理過程存成一張「技能卡」——記錄「遇到這類情境，用這套方法最有效」。
4. **優化技能** ：技能卡不是存了就不動。下次遇到類似任務，系統會拿新的結果跟舊技能比對，如果有更好的做法，就自動更新。
5. **檢索技能** ：新任務進來時，系統先搜尋技能庫，找到相關的技能卡直接套用，不用每次從頭推理。

用一句話理解：一般 AI 助理記住「你說過什麼」，Hermes 記住的是「怎麼做最有效」。

### 它怎麼「記住」你是誰？

除了學做事，Hermes 還會跨對話追蹤四件事，逐漸建立專屬於你的使用者檔案：

- **格式偏好** ：你習慣條列式摘要還是長段落？用過幾次後它就不再問了。
- **決策紀錄** ：你過去遇到類似狀況時怎麼選的，下次碰到同類問題會自動參考。
- **任務模式** ：你最常跑哪些類型的任務、什麼時間點跑、多久跑一次。
- **回饋訊號** ：你每次不改就接受，系統記錄為正向訊號；反覆修正的地方，系統知道要調整。

效果是什麼？用得越久，它越不需要你重複交代前提條件，你總是要求 Markdown 表格的，它不會再問你要什麼格式。

## Hermes Agent 跟 OpenClaw 差在哪？

OpenClaw 是另一個熱門的開源代理框架，兩者的取捨很明確：

- **OpenClaw** 追求「開箱即用、工具覆蓋廣」，設定門檻低，適合各式各樣的一次性任務。但它沒有內建學習機制，跑一百次同樣的任務，表現不會變好。
- **Hermes** 接受更高的初始複雜度，換取長期使用下的效率提升。同樣的任務跑得越多次，技能庫越精準，回應速度和品質都會提升。

簡單判斷：如果你的工作流程有大量重複性任務，Hermes 的投資報酬率會隨時間遞增；如果只是偶爾問問題，OpenClaw 或直接用 ChatGPT、Claude 更省事。

## Hermes Agent 安裝教學

### 第一步：一行指令完成安裝

Hermes Agent 支援 Linux、macOS（原生）與 Windows（需透過 WSL2），Android 用戶可透過 Termux 安裝。

在終端機執行：

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

安裝完成後，重新載入 shell 設定：

```bash
source ~/.bashrc  # macOS 用戶改用 source ~/.zshrc
```

接著啟動互動介面：

```bash
hermes
```

首次啟動會進入設定精靈，也可以隨時用 `hermes setup` 重新設定。

### 第二步：選擇你的 AI 模型

Hermes Agent 不綁定任何特定 AI 供應商，支援的平台包括：

- Nous Portal（Nous Research 自家平台）
- OpenRouter（可切換超過 200 種模型）
- OpenAI
- Anthropic
- z.ai / GLM
- Kimi / Moonshot
- MiniMax
- 任何相容的自訂端點

切換模型只需一行指令：

```bash
hermes model
```

系統會列出可用的供應商與模型，選擇後即生效，不需修改任何程式碼。如果你原本是 OpenClaw 用戶，可以用 `hermes claw migrate` 一鍵匯入設定、記憶、技能與 API 金鑰，加上 `--dry-run` 旗標可以先預覽再執行。

### 第三步：設定通訊閘道與排程自動化

Hermes 內建統一通訊閘道，支援 Telegram、Discord、Slack、WhatsApp、Signal 與 Home Assistant 六大平台：

```bash
hermes gateway
```

啟動後，你可以從任何一個平台與同一個 Hermes Agent 對話，技能與記憶跨平台共享。

排程自動化則透過內建的 cron 排程系統，用自然語言定義任務即可。例如「每天早上 9 點整理我的 Email 摘要」，Hermes 會自動在背景執行並透過指定平台回傳結果。

CLI 本身也不只是純文字介面——它支援多行編輯、指令自動補全、對話歷史、中斷重導等功能。

### 常用指令速查

| 指令 | 功能 |
| --- | --- |
| `hermes` | 啟動互動式 CLI |
| `hermes model` | 設定 AI 供應商與模型 |
| `hermes tools` | 啟用／停用工具 |
| `hermes gateway` | 啟動多平台通訊閘道 |
| `hermes setup` | 執行完整設定精靈 |
| `hermes update` | 升級至最新版本 |
| `/new` 或 `/reset` | 對話中開啟新對話 |
| `/skills` | 瀏覽已建立的技能 |

### 不是所有人都需要 Hermes

誠實來說，Hermes Agent 不適合所有人。它適合的場景是： **任務具有重複性與結構性、有固定使用者讓系統觀察行為模式、品質改善可以被量化。**

如果你的需求是「偶爾問一個問題就好」，或者不想管理自託管的基礎設施（技能儲存、學習迴圈、使用者模型持久化），那麼 OpenClaw 或是直接使用 ChatGPT、Claude 等現成服務會更省事。

Hermes 的價值在於長期投入後的複利效果，但前提是你願意花時間讓它學習。

> 延伸閱讀：  
> [Gemini新功能「notebooks」上線！串接NotebookLM，讓你把對話、文件整理成專屬AI知識庫](https://www.bnext.com.tw/article/90553/google-gemini-notebooks-personal-knowledge-hub)

資料來源： [Hermes Agent GitHub](https://github.com/nousresearch/hermes-agent) 、 [MindStudio Blog](https://www.mindstudio.ai/blog/what-is-hermes-agent-openclaw-alternative)

本文初稿為AI編撰，整理．編輯/ 李先泰