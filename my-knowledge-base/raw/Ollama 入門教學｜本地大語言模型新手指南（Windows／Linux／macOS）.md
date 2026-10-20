---
title: "Ollama 入門教學｜本地大語言模型新手指南（Windows／Linux／macOS）"
source: "https://klab.tw/2025/09/ollama-tutorial/"
author:
  - "[[Kyle]]"
published: 2025-09-12
created: 2026-08-04
description: "近年 AI 大語言模型（LLM）百花齊放，大多數人接觸到的方式仍是透過雲端 API，例如 ChatGP、Gemini、 Claude、Grok、Deepseek等，雖然方便，但可能會受限於費用、額度、網路延遲，資料隱私等問題，希望可以在自己的電腦上執行。Ollama 讓我們能在自己的電腦上直接執行 LLM，不需要依賴雲端輕鬆體驗離線大模型，之後還能結合 AI Agent、MCP 或是 AnythingLLM 整合介面。這篇文章會教學在 Windows / Linux / macOS 安裝 Ollama、下載模型，選適合自己電腦的模型大小。"
tags:
  - "clippings"
status: processed
---
近年 AI 大語言模型（LLM）百花齊放，大多數人接觸到的方式仍是透過雲端 API，例如 ChatGP、Gemini、 Claude、Grok、Deepseek等，雖然方便，但可能會受限於費用、額度、網路延遲， [資料](#) 隱私等問題，希望可以在自己的 [電腦](#) 上執行。 **Ollama** 讓我們能在自己的電腦上直接執行 LLM，不需要依賴雲端輕鬆體驗離線大模型，之後還能結合 AI Agent、MCP 或是 AnythingLLM 整合介面。這篇文章會教學在 [Windows](#) 、Linux、macOS 安裝 Ollama、下載模型，選適合自己電腦的模型大小。

機器學習與人工智慧

## Ollma 介紹

**Ollama** 是一款可以在本地電腦上執行大語言模型的程式軟體，安裝後可以在終端機與REST API兩種方式操控使用，也可以在 Python、JavaScript、Jave、C# 等各種程式語言內使用它，或是連接 AI Agent、AnythingLLM、Open WebUI 使用它。

### Ollama 跟語言模型

**Ollama** 不是模型。它是「本地模型執行引擎（runtime）」負責下載、執行與管理模型。 **LLM 大語言模型** 才是內容（例如 *Llama 3* 、 *Phi-3* 、 *Qwen* 、 *Mistral* …），通常以 **GGUF** 權重檔形式發佈。可以把 Ollama想像成播放器，模型想成影片檔。同一個播放器可播很多片；你想用哪一部，就載入它來播放。

### Ollama 與 LLaMA

Ollama 是上面提過的執行模型的引擎，而 **LLaMA** 是由 Facebook 母公司 Meta 研發的一系列大語言模型。兩者沒有隸屬關係，只是名字很像、而且 Ollama 能跑 LLaMA。

開放原始碼

## 安裝與初次使用

### macOS 安裝

在 macOS 上推薦使用 Homebrew 來安裝，如果想了解 Homebrew 可以參考 [macOS Homebrew 新手教學｜安裝與使用指南](https://klab.tw/2025/09/macos-homebrew-tutorial/) 。想要直接安裝也可以在 [https://ollama.com/download/mac](https://ollama.com/download/mac) 下載 。

```bash
# 透過 brew 安裝
brew install ollama

# 在終端機啟動 ollama
ollama serve

# 將 ollama 作為服務啟動
brew services start ollama

# 停止 ollama 服務
brew services stop ollama
```

在終端機啟動後會佔用一個終端機畫面，要關閉時可以鍵盤輸入 `Control + C` 。

Ollama 還有提供一個有圖形界面的版本，可以透過 `brew install --cask ollama-app` 進行安裝。

### Linux 安裝

要在 [Linux](#) 安裝可以直接使用官方的安裝腳本 [https://ollama.com/download/linux](https://ollama.com/download/linux) 。

```bash
# 安裝 Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 查看狀態
systemctl status ollama

# 啟動服務
systemctl start ollama

# 停止服務
systemctl stop ollama

# 開機自動啟動（預設已啟用）
systemctl enable ollama
```

因為會安裝到Systemd，以上指令通常需要加上 sudo 使用最高權限。

### Windows 安裝

在 [Windows](#) 開啟 PowerShell 可以透過以下指令安裝，或是直接在 [https://ollama.com/download/windows](https://ollama.com/download/windows) 下載安裝檔。

```powershell
# 開啟 PowerShell 輸入以下指令
irm https://ollama.com/install.ps1 | iex

# 舊版透過 winget 下載的指令
winget install --id=Ollama.Ollama -e
```

安裝後，打開 PowerShell 輸入 `ollama` 測試。

開放原始碼

### 執行第一個模型

如果沒有透過服務啟動啟動 Ollama，可以在終端機輸入 `ollama serve` ，然後再開啟另一個終端機輸入以下指令。

```bash
# 從 Ollama 官方下載一個 Google Gemma3 4b模型
ollama pull gemma3:4b

# 下載後執行它
ollama run gemma3:4b
```

等它開起來就會顯示: **\>>> Send a message (/? for help)** ，可以直接輸入文字聊天，如果輸入 `/?` 會出現設定與資訊的指令教學，要離開互動介面可以輸入 `/bye` 。以下範例是我輸入的內容，以及獲得的回覆。

```bash
>>> 你知道台北101的高度嗎？
台北101的高度是 **508公尺（1667英尺）**。

這曾經是世界最高的摩天大樓，目前仍是台灣最高的建築。

想了解更多關於台北101的資訊，可以參考以下連結：

*   **台北101官方網站:** [https://www.taipei-101.com.tw/tw/](https://www.taipei-101.com.tw/tw/)

希望這些資訊對您有幫助！
```

回覆中會看到一些符號，像是 `**` 、 `[https://]` 等，這些是 Markdown 語法，通常語言模型輸出的都是這種格式，Ollama 是模型的執行引擎會直接顯示，透過其他圖形介面連結 Ollama 的時候，會把 Markdown 標記變成更漂亮的內容。

另外擁有思考鏈的模型輸出的內容前面通常會有一段 `<think>...</think>` 文字，是模型的思考內容。在 Ollama 會直接顯示，在圖形介面連結 Ollama 的時候會隱藏需要另外點開，方便使用者閱讀。

### 透過 REST API 使用

Ollama 內建 REST API（預設 `http://localhost:11434` ），可讓前後端程式呼叫。常見的 API 端點：

- `POST /api/generate` ：單輪輸出
- `POST /api/chat` ：多輪對話

```bash
curl http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{"model":"llama3.2","prompt":"Hello"}'
```

## 模型管理

### 常用的管理

```bash
# 下載/更新模型
ollama pull <模型名稱>      # ollama pull gpt-oss
ollama pull <模型名稱:標籤>  # ollama pull gpt-oss:20b

# 列出已安裝模型
ollama list

# 啟動互動模式
ollama run <模型名稱>      # ollama run gemma3
ollama run <模型名稱:標籤>  # ollama run gemma3:4b

# 查看運行中的模型
ollama ps

# 停止任務
ollama stop <id>

# 刪除模型
ollama rm <模型名稱>
ollama rm <模型名稱:標籤>
```

模型名稱後面會接上代表模型參數大小的標籤，如果沒有標籤就是預設的某個laste版本，所以 `gemma3` = `gemma3:latst` = `gemma3:4b` 。

我們可以上 Ollama 的網站 [https://ollama.com/search](https://ollama.com/search) 查詢目前有哪些模型，可以上模型的介紹頁面查詢有多少種大小，以及latst是對應什麼版本，例如 Gemma3 的頁面 [https://ollama.com/library/gemma3](https://ollama.com/library/gemma3) 。

開放原始碼

### 模型參數與量化

**參數（Parameters）** 是模型的「權重數量」。例如 `8B` 就是 80 億參數。一般而言參數越大，模型的理解與生成能力越好，但需要的 RAM 與 VRAM 越多，模型的檔案也越大。

**量化（Quantization）** ：把浮點權重壓成較低位元（如 `q4_K_M` 、 `q8_0` ），檔案更小、載入更快，但品質會略降。多數本地情境建議先用 q4 系列開始。

還有一種模型例如 **Mixtral** **8x7B** ，它是 **Mixture of Experts (MoE) 架構** 的模型。8x7B 表示這個模型內部一共有 8 個 7B 的專家子模型，總參數量理論上大約 **560 億** 。但是在推理時不會同時啟用全部 8 個專家，而是根據輸入內容，只挑選其中一兩個專家來處理。是增加知識量與推理能力，又降低硬體成本的方案。

### 模型參數與硬體規格

前面執行的範例中使用 4B 的是希望下載快一點，也讓大部分 [電腦](#) 都可以執行。以下提供一些簡易的參數大小與對應的硬體推薦。

| 模型大小 | 參數量級 | 建議最低記憶體/顯卡記憶體 | 典型裝置與體驗 |
| --- | --- | --- | --- |
| **3B** | 30億 | 4GB RAM / 2GB VRAM | 輕薄筆電：基本對話、工具指令 |
| **8B** | 80億 | 8GB RAM / 4–6GB VRAM | 多數筆電／桌機甜蜜點：日常助理、簡易程式輔助 |
| **13B** | 130億 | 16GB RAM / 8–10GB VRAM | 中高階顯示卡筆電、桌機：回覆更穩、上下文更長 |
| **30B** | 300億 | 32GB RAM / 16GB+ VRAM | 工作站／伺服器：RAG 與較複雜任務 |
| **70B** | 700億 | 64GB RAM / 48GB+ VRAM | 專業伺服器級 |

以我目使用的 MacBook pro 搭配 M2 pro 晶片與 16GB RAM，執行 gemma:4b 非常迅速，執行 gemma:12b 的回應速度也很快，但是會吃光大部分系統記憶體。再更高的參數大小就很吃力了，例如 deepseek-r1:14b 執行速度稍微能接受，gpt-oss:20b 就要幾秒才出現一個字不太理想。

Gemma3 還有一個超小模型 gemma3:270m，只有2.7億參數，執行起來飛快，但是理解與生成內容幾乎沒辦法使用。

---

Sponsored Links

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="908" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-3798660000975111&amp;output=html&amp;h=280&amp;slotname=5724391701&amp;adk=2215140890&amp;adf=69387288&amp;pi=t.ma~as.5724391701&amp;abgtt=7&amp;w=908&amp;fwrn=4&amp;fwrnh=0&amp;lmt=1785821743&amp;rafmt=1&amp;armr=3&amp;format=908x280&amp;url=https%3A%2F%2Fklab.tw%2F2025%2F09%2Follama-tutorial%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;rpe=1&amp;resp_fmts=3&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785821733091&amp;bpp=1&amp;bdt=431&amp;idt=47&amp;shv=r20260729&amp;mjsv=m202607290101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D72090ed750957578%3AT%3D1785821733%3ART%3D1785821733%3AS%3DALNI_MZOxI8iK5y8yQ2FXoAdByHYarM1_A&amp;gpic=UID%3D0000145fe841cb5d%3AT%3D1785821733%3ART%3D1785821733%3AS%3DALNI_MaUKXqxHQ3riwTJV118jGBX63Bs-g&amp;eo_id_str=ID%3D4ec592b26152e170%3AT%3D1785821733%3ART%3D1785821733%3AS%3DAA-AfjYlbzyR2qx9RT_8FqdW7f41&amp;prev_fmts=0x0%2C313x250%2C313x250&amp;nras=1&amp;correlator=2367733393207&amp;frm=20&amp;pv=1&amp;rplot=4&amp;u_tz=480&amp;u_his=3&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=185&amp;ady=6487&amp;biw=1619&amp;bih=908&amp;scr_x=0&amp;scr_y=2870&amp;eid=31099996%2C95396139%2C122880058&amp;oid=2&amp;psts=AOrYGslZ2_Sw1ojXx7t6iMkPulqosivLAIsxivPSFl9JtFxBLlFEsE3u6JIaqCNUTO9jvR1QzGuHReu82Le8o2uQcRvodzvaj897JfUT8AM&amp;pvsid=585276872459355&amp;tmod=1904139547&amp;uas=0&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1959%2C30%2C1959%2C30%2C1920%2C30%2C1859%2C989%2C1619%2C908&amp;vis=1&amp;rsz=%7C%7CEebr%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;plas=166x510_l%7C166x510_r&amp;bz=1.15&amp;pgls=CAEaBTYuOS41~CAEQBBoHMS4xNzcuMA..&amp;ifi=2&amp;uci=a!2&amp;btvi=2&amp;fsb=1&amp;dtd=10033" title="Advertisement" aria-label="Advertisement"></iframe>

---

## 常見的開源模型

在 Ollama 的 [https://ollama.com/search](https://ollama.com/search) 網站上有非常多模型可以下載，這邊簡單介紹幾種常見的提供參考。

開放原始碼

| 模型名稱 | 開發團隊 | 擅長用途／特色 |
| --- | --- | --- |
| **LLaMA 3.1 / LLaMA 3.2**   (如 llama3.1, llama3.2) | Meta (美國) | 通用型模型，支援多語言，廣泛應用於聊天、問答、程式輔助。常見大小有 8B / 70B，8B 適合本地，70B 則需高階硬體或雲端。 |
| **LLaVA**   (如 llava:7b) | 社群開源（最初由 UC Berkeley 與 Microsoft 合作） | 具備多模態理解能力的開源模型，可同時處理文字與圖片輸入。常用於圖片描述、視覺問答（VQA）、螢幕內容解析等應用。以 LLaMA 或 Mistral 為語言基底。 |
| **Gemma 2 / Gemma 3**   (如 gemma3:2b, gemma3:9b) | Google (美國) | 強調輕量與研究友善，小模型啟動快但能力有限，大一點的 2B/9B 可做研究與簡單應用。 |
| **GPT-OSS**   (如 gpt-oss:20b, gpt-oss:120b) | OpenAI (美國) | 這是 OpenAI 自 GPT-2 之後時隔多年首度釋出開放權重模型，擁有思維鏈，但是只有 20B 跟 120B 兩種參數大小。 |
| **Phi-3**   (如 phi3:3.8b, phi3:7b) | Microsoft (美國) | 設計為「小而精」，在程式、推理任務上表現佳。特別適合資源有限的 [電腦](#) ， **7B** 常用於本地開發助理。 |
| **Mistral**   (如 **/ Mixtral 8x7B**) | Mistral (法國) | Mistral 7B 效能佳、體積適中；Mixtral 8x7B 採用 MoE（Mixture of Experts），推理能力強，但需要更多記憶體。 |
| **Qwen2 / Qwen2.5**   (如 qwen2:7b, qwen2:14b) | 阿里巴巴 (中國) | 中文能力特別強，多語言也不錯。7B 適合一般筆電，14B 需要更高規格，適合做知識問答與文件處理。 |
| **DeepSeek**   (如 deepseek-r1:7b) | DeepSeek (中國) | 主打程式生成與數學推理，在小中型模型中表現突出。deepseek-r1:7b 適合做推理助手，deepseek-coder 在程式輔助上特別好用。 |

### 「開源」和「開放權重」

我們平常講的 **開源** Open Source，大部分是指軟體專案把原始碼完整公開，像是 [Linux](#) 、Python，可以自由下載、研究、修改，有些開源授權甚至能用修改後用在商業環境，真正做到「透明又能重現」。

Linux與 Unix

但在大語言模型情況就不太一樣，很多時候說的「開源模型」，其實是 **開放權重 Open-weight Model** 。開發團隊會把已經訓練好的 **模型權重檔** 公布出來，讓大家能下載在自己的電腦上執行。不過完整的訓練程式、 [資料](#) 集、訓練流程，大多不會公開。

所以我們可以在自己的電腦跑這個模型，但不是真的知道他如何從頭到尾訓練、用了什麼資料訓練。

## 簡易使用範例

這邊給兩個 Python 使用 Ollama 的例子，但是第一個是使用 REST API 呼叫，因此任何程式語言，甚至透過 CURL 都可以使用。

### Python with requests

透過 Python 的 requests 呼叫 API，使用其他程式語言也可以。記得執行前要啟動 ollama，但不需要執行 `ollama run gemma3` 等指令，ollama 會自動處理。

```python
import requests

def main():
    url = "http://localhost:11434/api/chat"
    payload = {
        "model": "gemma3:4b",
        "messages": [
            {'role': 'system', 'content': '你是一個SEO分析師，請用中文回答問題。'},
            {"role": "user", "content": "我是 https://klab.tw 部落格作者，請給我一些SEO優化建議"}
        ],
        "stream": False
    }
    resp = requests.post(url, json=payload)
    print(resp.json())

if __name__ == "__main__":
    main()
```

在 API 執行期間，以及執行後一段時間內，在終端機輸入 `ollama ps` 可以看見被開啟的模型在運作中。

程式設計

返回的JSON說明：

| 欄位名稱 | 型別 | 說明 |
| --- | --- | --- |
| **`model`** | `string` | 代表本次使用的模型名稱與版本。例： `gemma3:4b` → 表示使用 Gemma 3 模型的 4B 版本。 |
| **`created_at`** | `string (ISO 8601)` | 請求建立時間（UTC 時區）。例： `2025-10-24T20:11:41.068554Z` 。 |
| **`message`** | `object` | 模型的輸出訊息。通常包含：   • `role`: `"system"` 、 `"user"` 或 `"assistant"`   • `content`: 生成的文字內容。 |
| **`done`** | `boolean` | 是否已完成回覆。 `True` 代表整個輸出已結束。若是串流模式會在最後一筆才為 True。 |
| **`done_reason`** | `string` | 模型結束回覆的原因：   • `"stop"` ：正常結束（遇到結尾 token）   • `"length"` ：達到最大 token 限制   • `"error"` ：執行錯誤中止。 |
| **`total_duration`** | `integer (ns)` | 本次請求的總耗時，單位為奈秒（nanoseconds）。例： `8483334300 ≈ 8.48 秒` 。 |
| **`load_duration`** | `integer (ns)` | 模型載入耗時（第一次呼叫或未快取時較長）。 |
| **`prompt_eval_count`** | `integer` | 前置 prompt（包含 system + user）被評估的 token 數量。 |
| **`prompt_eval_duration`** | `integer (ns)` | 模型處理 prompt tokens 的耗時。 |
| **`eval_count`** | `integer` | 模型實際生成（output）token 的數量。 |
| **`eval_duration`** | `integer (ns)` | 模型生成 output tokens 的耗時。 |

### Python with ollama

這個方案需要先使用 pip 安裝 ollama 模組。

開放原始碼

```python
import ollama

def main():
    msgs=[
        {'role': 'system', 'content': '你是一個友善的助手，請用中文回答問題。'},
        {'role': 'user', 'content': '介紹一下東京迪士尼樂園'}
    ]
    response = ollama.chat(
        model='gemma3:4b',
        messages=msgs
    )
    print(response['message']['content'])

if __name__ == "__main__":
    main()
```