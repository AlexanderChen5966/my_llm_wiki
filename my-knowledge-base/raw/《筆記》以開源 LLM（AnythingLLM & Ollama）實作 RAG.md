---
title: "《筆記》以開源 LLM（AnythingLLM & Ollama）實作 RAG"
source: "https://vip.studycamp.tw/t/%E3%80%8A%E7%AD%86%E8%A8%98%E3%80%8B%E4%BB%A5%E9%96%8B%E6%BA%90-llm%EF%BC%88anythingllm-ollama%EF%BC%89%E5%AF%A6%E4%BD%9C-rag/8501"
author:
  - "[[sky]]"
published: 2025-11-11
created: 2026-06-02
description: "這堂課用 AnythingLLM 和 Ollama，在自己的電腦上，練習類似 RAG 的簡單玩具。 筆記由 Gemini 撰寫，然後再人工微調。由於這堂課比較簡單，聽課的時間少了許多，就找了比較多的課外補充提供給大家參考。 ▌1. 課程核心：建立本地 RAG 應用程式 (Lo"
tags:
  - "clippings"
status: processed
---
> 這堂課用 AnythingLLM 和 Ollama，在自己的電腦上，練習類似 RAG 的簡單玩具。
> 
> 筆記由 Gemini 撰寫，然後再人工微調。由於這堂課比較簡單，聽課的時間少了許多，就找了比較多的課外補充提供給大家參考。

---

## ▌1. 課程核心：建立本地 RAG 應用程式 (Local RAG)

建立一個 100% 在本地電腦運作、完全免費且保護隱私的 RAG 應用程式。

- **Ollama:** 作為本地 LLM 伺服器，用以執行開源模型 (如 Llama, Gemma)。
- **AnythingLLM:** 作為 RAG 應用的圖形化介面 (GUI)，負責處理文件嵌入、聊天和 Agent 技能等。

## ▌2. Ollama - 本地 LLM 伺服器

Ollama 讓我們能輕鬆在本地下載、管理並執行大型語言模型。

> 補充：新版的 Ollama 界面已和老師講解的不同。要查看該模型的所有版本，原本是左上側的下拉式選單，目前的版本則是在右上方有個 **View all** 連結，點擊觀看。

### 2.1. 硬體、模型與量化

- **VRAM 關鍵:** 選擇模型的主要限制是 VRAM (顯示卡記憶體)。模型大小應小於 VRAM 容量（建議保留 1.1x 至 1.2x 的緩衝空間）。
- **模型選擇:**
	- **Gemma 3** (Google) 和 **Llama 3.2** (Meta) 是 2025 年的新模型。
		- **Qwen** (阿里巴巴) 和 **Phi** (Microsoft) 也是熱門選項。
		- **Dolphin** 系列模型是「未經審查」(uncensored) 的版本。
- **模型量化 (Quantization):**
	- **FP16 (Float16):** 原始、最精確但最大的模型。
		- **Q8, Q6, Q5, Q4, Q2:** 量化模型，檔案更小、 VRAM 佔用更低，但會犧牲些許精確度。Q4 是最常見的平衡點。
		- **GGUF:** 這是目前在本地運行量化模型的標準檔案格式。

### 2.2. 關鍵 CLI 指令

- `ollama serve`: 啟動 Ollama 伺服器（這是 AnythingLLM 連線的重點）。
- `ollama pull [model_name]`: 下載模型 (例如 `ollama pull llama3.2`)。
- `ollama list`: 列出所有已下載的模型。
- `ollama run [model_name]`: 在終端機中與模型聊天。
- `ollama rm [model_name]`: 刪除模型。
- `ollama show [model_name]`: 顯示模型的詳細資訊。

### Ollama CLI 常用指令參考

> [GitHub - ollama/ollama: Get up and running with Kimi-K2.5, GLM-4.7, DeepSeek, gpt-oss, Qwen, Gemma and other models.](https://github.com/ollama/ollama?tab=readme-ov-file#cli-reference)

| 指令 (Command) | 指令說明 |
| --- | --- |
| `ollama run [model_name]` | 執行一個模型。如果模型尚未下載，Ollama 會自動從模型庫下載它。 |
| `ollama pull [model_name]` | 從 Ollama 模型庫下載或更新一個模型。這會拉取最新的版本。 |
| `ollama list` | 列出所有已下載到本機電腦上的模型 (包含名稱、大小和下載時間)。 |
| `ollama rm [model_name]` | 移除 (Remove) 一個已經下載到本機的模型，以釋放硬碟空間。 |
| `ollama create -f [Modelfile_name]` | 透過一個 `Modelfile` 檔案來建立一個自訂模型。 |
| `ollama cp [source_model] [new_name]` | 複製 (Copy) 一個已存在的模型，並為其指定一個新名稱。 |
| `ollama serve` (或 `ollama start`) | 啟動 Ollama 的背景伺服器。 (在 Windows 和 macOS 上通常是自動的) |
| `ollama ps` | 查看目前正在運作中的模型程序 (Process Status)。 |
| `ollama --help` | 顯示所有可用的指令和選項的輔助說明選單。 |
| `ollama --version` | 顯示目前安裝的 Ollama 版本。 |

**補充：**

- `[model_name]` 是要操作的模型名稱，例如 `llama3.2` 或 `phi3:mini`。
- 如果執行 `ollama run` 時只輸入模型名稱（例如 `ollama run llama3.2`），它會自動使用 `latest` (最新) 標籤。
- 如果需要指定特定版本（例如 `q4_K_M`），必須使用完整的名稱，例如：`ollama run llama3.2:3b-q4_K_M`。

### 2.3. 關於 Dolphin 模型

> 一般的模型(如 Llama 3, GPT-4) 經過大量的「對齊」(Alignment) 和「安全訓練」(Safety Training)。如果問它們如何撰寫釣魚郵件或發表歧視性言論，它們會拒絕回答，這是一般模型的「安全護欄」。
> 
> Dolphin 模型訓練的目標是移除護欄，「無條件服從」用戶的指令。

Dolphin 是「更原始」、「服從性」極高的 LLM，對於需要嚴格按照 RAG 流程辦事的場景（例如嚴格基於法律文件、醫療報告或內部資料庫回答問題）來說，是一個非常好的測試對象。

但不推薦用來做公開的聊天機器人，因為很危險。

**基本資訊：**

- **基礎模型：** 基於 Meta 官方的 `Llama-3.1-8B-Instruct` 模型進行微調的。
- **微調目的：** 創建者 (Eric Hartford) 的目標是讓模型**更「服從」(obedient)** 使用者所下的指令。
- **最大特點 (Uncensored)：**
	- 強調這是一個「**未經審查**」的模型。
		- 它被訓練來**避免「道德說教」 (moralizing)** 和 **「拒絕回答」 (refusals)**。
		- 它會盡可能地遵循用戶的**所有**指示，即使這些指示在標準模型看來可能是有害、不道德或不恰當的。
- **免責聲明：** 創作者**警告**：由於沒有安全護欄，這個模型「**可以且將會**」產生有偏見、不道德、虛假或有害的內容。

在 RAG 中，這個特性非常重要：

- 在 RAG 流程中，最關鍵的指令是：「『只』根據我提供的上下文來回答問題，不要使用你自己的知識。」
- 標準模型的「壞習慣」： 有些「過度幫助」(overly helpful) 的模型，即使給了上述指令，它有時還是會「不聽話」，使用它自己內部的訓練知識來回答。
- 像 Dolphin 這樣未經審查、高度服從的模型，更有可能嚴格遵守 RAG 指令。

## ▌3. AnythingLLM - RAG 介面與設定

> 從這節開始，介紹使用 AnythingLLM 來建立一個簡單的 RAG 系統。（講師提及 LM Studio 也是類似工具，但他個人偏好 AnythingLLM ）
> 
> AnythingLLM 比較像玩具，只是拿來介紹 RAG 的一些基本觀念，不適合大型專案。

AnythingLLM 是一個開源的 RAG 平台，以其「本地優先」和易用性著稱。

### 3.1. 建立 “Local-First” 堆疊

講師建議使用完全免費且本地的設定，以避免 API 金鑰和額外費用：

1. **LLM 供應商:** 選擇 **Ollama**。
2. **Ollama Base URL:** 填入 `ollama serve` 顯示的 URL (通常是 `http://127.0.0.1:11434`)。
3. **向量資料庫 (Vector DB):** 選擇 **LanceDB**。
	- *註：* LanceDB 是一個嵌入式 (in-process) 向量資料庫，它會自動包含在 AnythingLLM 中，無需額外設定，非常適合本地使用。
4. **嵌入模型 (Embedder):** 選擇 **AnythingLLM 內建 Embedder**。
	- *註：* 這樣可以確保從文本分塊到向量化的整個過程都在本地機器上完成。

### 3.2. 嵌入文件的建議

- 講師強烈建議，與其使用 AnythingLLM 內建的資料連接器 (如 PDF, YouTube, GitHub)，**不如手動將 PDF 轉檔為 Markdown (.md) 文件**，清理後再上傳。
- **原因:** 內建的自動爬蟲和轉檔器有時會出錯 (wobbly)，手動轉檔為 Markdown 能確保嵌入的資料品質最高、最乾淨。

## ▌4. RAG 核心概念 (1): 分塊 (Chunking)

分塊 (Chunking) 是 RAG 的靈魂。如果沒有正確分塊，RAG 的效果會大打折扣。

### 4.1. 為什麼要分塊？

- **解決「中間遺失」(Lost in the Middle):** LLM 在處理單一、超長的文件時，通常只會記得開頭和結尾的資訊，中間的資訊會被「遺忘」。分塊能確保每個「中間」部分都有機會被 LLM 仔細閱讀。
- **效能與價格:**
	- *講師警告：* 即使是擁有一百萬 (1M) token 上下文視窗的模型，也不代表應該把整本書丟進去。
		- **價格 (Price):** 每次查詢 (Query) 都傳送 50,000 token 的 chunk，會比傳送 5,000 token 的 chunk 昂貴得多。
		- **延遲 (Latency):** 處理 50,000 token 也會比處理 5,000 token 慢得多。

### 4.2. 分塊策略 (Chunking Strategy)

- **Chunk Size (分塊大小):**
	- **長篇故事 (Stories):** 1000 - 5000 tokens。
		- **短篇文本 (Short Texts):** 500 - 1000 tokens。
		- **列表/連結/數字 (Lists/Numbers):** 100 - 500 tokens (越小越好，確保精確度)。
		- *註：* AnythingLLM 在此課程版本中上限為 1000。
- **Chunk Overlap (區塊重疊):**
	- **目的:** 確保在區塊邊界處的語義不會被切斷。
		- **經驗法則:** 設為 Chunk Size 的 **1% 到 5%**。 (例如 1000 的 size，overlap 可設為 10-50)。

## ▌5. RAG 核心概念 (2): 參數調校與實踐

### 5.1. RAG 實踐 (CoD 範例)

講師展示了 RAG 的核心價值：

1. **無 RAG:** 詢問 Llama 3.2 “什麼是 CoD?”。 Llama 回答 “Cod 是一種魚 (鱈魚)”。
2. **有 RAG:** 上傳關於 “Chain of Draft (CoD)” 的 PDF。再次詢問 “什麼是 CoD?”。
3. **結果:** 模型利用 Agent Skill (函數呼叫) 搜尋了向量資料庫，並正確回答 “CoD 是 Chain of Draft 提示技巧”。

### 5.2. 關鍵參數調校

- **Top-K (最大上下文片段):**
	- **意義:** 相似度搜尋後，要取回「幾個」最相關的 chunks 餵給 LLM。
		- **設定:** 預設為 4。講師認為 4-8 都是合理範圍。
- **相似度分數 (Similarity Score Threshold):**
	- **意義:** 只有相似度高於這個門檻 (例如 25% / 0.25) 的 chunk 才會被採用。
		- **設定:** 預設 0.25 適用於通用文本。若是高精確任務 (如會計)，可調高至 0.75，以過濾掉不相關的雜訊。
- **溫度 (Temperature):**
	- **意義:** 控制模型的「創意度」。
		- **設定:** 0 (最固定) 到 1 (最有創意)。會計建議 0-0.3；創意寫作 1.0；通用平衡 0.7-0.8。
- **聊天紀錄 (Chat History):**
	- **機制:** AnythingLLM 使用「滑動視窗緩衝區」(Sliding Window Buffer)。
		- **設定:** 預設 20 則訊息。

### 5.3. 斜線指令 (Slash Commands)

> AnythingLLM 允許使用者自定義「指令簡寫」來快速輸入常用提示文字。像我最常用的是「以下請以繁中回答。」

老師的範例：

- `/llm` (Respond short): 注入 “請簡短回答”。
- `/long` (Long explanation): 注入 “請使用描述性語言並詳細解釋”。

### 5.3.1. 系統內建 (Default) 指令

| 指令 | 說明 |
| --- | --- |
| `/reset` | 重設（清除）當前的對話紀錄。 這會開始一個全新的對話，清除 LLM 的短期記憶，但不會刪除您上傳的文件。 |
| `/exit` | 結束一個「代理」(Agent) 會話。 如果您使用了 `@agent` 來啟動一個 AI 代理，此指令會結束該代理的運作，讓您返回標準的 RAG 聊天模式。 |

### 5.3.2. 自訂 (Custom) 指令

使用者可以「自訂」常用提示(Prompt)內容，並將其儲存為「捷徑」。

- 用途： 它是「文字片段」的快捷鍵。您可以將常用指令（例如：「請你扮演一位專業的法律顧問，僅使用我提供的文件，並用繁體中文總結以下內容：」）儲存為一個簡寫指令（例如：`/legal`）。
- 建立內容：
	1. 指令：你想使用的簡寫指令，例如：`/legal`。
		2. Prompt：前述簡寫指令代表的內容，例如：「請你扮演一位專業的法律顧問，僅使用我提供的文件，並用繁體中文總結以下內容：」
		3. 描述：給自己看的提示。
		4. 範例：
		- 指令: `summary`
				- Prompt: `請幫我用條列式總結我提供的上下文，使用繁體中文。`
				- 儲存後，未來只要在 chat 輸入 `/summary`，它就會自動替換為前述那段提示文字。
- 建立方式：

方法一：進行 AnythingLLM App 中，首面下方中間的「斜線命令」區塊，點擊 創建斜線命令 。

方法二：點擊對話欄下方的 / 後，選擇 新增預設

- 使用其他人建立的自訂指令
	> 截至本文撰寫日期（2025/11/13），共有 140 個自訂指令。

[**Community Hub - Slash Commands**](https://hub.anythingllm.com/list/slash-command)

### 5.3.3. 補充：`/` vs `@`

請不要將這兩者混淆，它們的功能不同：

- `/` (Slash Commands): 主要是您自訂的文字捷徑（例如 `/summary`），以及系統的 `/reset` 和 `/exit`。
- `@` (Agent Invocation): 這是用來啟動 AI 代理的指令（例如 `@agent`），讓模型可以執行更複雜的任務，如網路搜尋或檔案操作。

## ▌6. 講師評論與進階工具展望

講師對 AnythingLLM 及其在 RAG 生態系中的定位，給出了非常坦率的評價。

### 6.1. AnythingLLM 評價

- **優點 (Good):**
	- **核心 RAG 功能:** 極其簡單、易用，是建立「本地 RAG 應用」的完美工具。
		- **文件總結 (Summarization):** 運作良好。
- **缺點 (Bad / Wobbly):**
	- **Agent Skills:** 大部分附加功能（如網頁搜尋、圖表生成）都「非常不穩定」(really wobbly)。
		- **Agent Flows:** 內建的代理流程建構器功能不佳。

### 6.2. 進階概念與工具 (課程預告)

講師總結：AnythingLLM 適合「入門」，但若要建構「可靠的進階 Agent」，應轉向更專業的工具（進入「開發者模式」）。

- **MCP (Model Context Protocol):**
	- **定義:** 這是 2024-2025 年的新興開放標準，用於 AI Agent 與外部工具 (API、資料庫) 之間的標準化互動。
		- **評價:** 講師認為這是「熱潮」(hype)，並不滿意 AnythingLLM 的內建實作。他預告將在「雲端桌面」(Cloud Desktop) 環境中，搭配其他工具 (如 Flowise) 來深入探討。
- **LangChain / LangGraph:**
	- **定義:** LangChain 是 RAG 開發的核心框架；LangGraph 則是 LangChain 團隊推出的、用於建構多步驟、有狀態 (stateful) Agent 的圖形框架。
		- **定位:** 這將是下一章「開發者模式」的核心，提供最強大的控制力。
- **Flowise / KNIME:**
	- **定義:** 這兩者都是視覺化、低程式碼的流程建構平台。
		- **定位:** 講師認為它們是比 AnythingLLM 更可靠 (reliable) 的 Agent 建構工具。