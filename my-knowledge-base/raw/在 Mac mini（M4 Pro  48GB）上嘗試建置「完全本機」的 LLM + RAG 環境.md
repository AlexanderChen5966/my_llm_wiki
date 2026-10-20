---
title: "在 Mac mini（M4 Pro / 48GB）上嘗試建置「完全本機」的 LLM + RAG 環境"
source: "https://codelove.tw/@tony/post/37Zzex"
author:
published:
created: 2026-07-29
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------你好。我是トライベック株式会社的岡山。因為想在本機驗證 LLM + RAG，最近買了 Mac mini（M4 P..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月29日](https://codelove.tw/@tony/post/37Zzex "2026-07-29 08:30:12")

## 在 Mac mini（M4 Pro / 48GB）上嘗試建置「完全本機」的 LLM + RAG 環境

## 前言

你好。  
我是トライベック株式会社的岡山。

因為想在本機驗證 LLM + RAG，最近買了 Mac mini（M4 Pro / 48GB 記憶體）。  
雖然只有外殼是「mini」，價格可一點都不 mini。

一開始會有這個念頭，主要是因為我對雲端 AI 服務有以下幾點疑慮：

- 將資料和個資傳送到外部服務的抗拒感
- 採用按量計費，越反覆試驗成本就越高
- 依賴網路環境

能解決這些問題的選項，就是「本機 LLM + 本機 RAG」。因為全部都在自己電腦上完成，資訊不會外流，而且怎麼嘗試都免費。

本文將整理在 Apple Silicon 的 Mac 上，使用 Ollama 與 LlamaIndex 建立完全本機的 RAG 環境的步驟。

本文是以「先跑起來」為目的的入門文章。精度調校與持久化會預計在下一篇之後再介紹。

## 環境

## 項目內容機器Mac mini（2024）晶片Apple M4 Pro記憶體48GB（統一記憶體）作業系統macOS Tahoe 26.5.1為什麼 Apple Silicon 適合本機 LLM

談到本機 LLM 時，GPU 的 VRAM 容量常常是焦點，但 Apple Silicon 有一個強項叫做 **統一記憶體** 。CPU 和 GPU 共用記憶體，因此可直接把大部分的實體記憶體拿來載入模型。

一般電腦常見的配置是「記憶體有 32GB，但 GPU VRAM 只有 8GB」，這種情況下，8GB 幾乎就會成為模型大小上限。相對地，在 M4 Pro / 48GB 的情況下，GPU 可使用的記憶體預設大約是 **65～75%（約 32～36GB）** 。

以實測來看，在這個配置下可順暢運作的大致範圍如下：

- **14B～32B 級** ：能以實用速度運作，適合日常使用
- **70B 級** ：需要低位元量化（如 IQ3\_M 等），只能算是「勉強能跑」的程度

也就是說， **可以在同一台機器上運行 14B～32B 的高品質模型，同時還能放入 RAG 所需的嵌入模型** ，這就是這台機器可達到的目標。

## 要建立的組成

這次要建置的環境全貌如下。

職責使用工具補充推論引擎 **Ollama** 負責模型管理與執行生成模型（LLM） **qwen3:14b** 產生回答文字嵌入模型 **bge-m3** 將文件向量化，對日文也很強RAG 框架 **LlamaIndex** 串接搜尋與生成 [![rag-architecture.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4105422%2Fc4362d67-b3b4-4ab7-8104-2c7c58c50303.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ad58a6985ebbe8743f15959c2f8ddd53)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4105422%2Fc4362d67-b3b4-4ab7-8104-2c7c58c50303.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ad58a6985ebbe8743f15959c2f8ddd53)

在 RAG 中，需要兩種模型：「用來搜尋文件的嵌入模型」以及「用來產生答案的生成模型」。這是最容易先混淆的地方。

## 導入方法（Mac 版）

### Step 1: 安裝 Ollama

使用 Homebrew 安裝。

```
brew install ollama
```

安裝完成後，啟動伺服器。

```
ollama serve
```

如果你已經安裝 GUI 版的 Ollama，只要它常駐在選單列，伺服器就已經在運作了。這種情況下再執行 `ollama serve` 會出現 `address already in use` 錯誤。

可以用以下指令確認是否正常啟動：

```
ollama list
```

### Step 2: 取得模型

取得生成模型與嵌入模型。

```
# 生成模型（整體平衡型）
ollama pull qwen3:14b

# 也順便下載較大的模型作比較
ollama pull gemma3:27b

# 嵌入模型（支援包含日文在內的多語言）
ollama pull bge-m3
```

`bge-m3` 是專用於嵌入的模型，不能拿來聊天。它會在 RAG 背後負責檢索工作。

### Step 3: 確認運作

在組 RAG 之前，先確認模型本身能不能正常運作。

```
ollama run qwen3:14b
```

看到提示字元後，試著用日文對話。

```
>>> 自己紹介して
```

如果有回覆就代表成功。結束時輸入 `/bye` 。

若想用數值確認生成速度，可以加上 `--verbose` 。

```
ollama run gemma3:27b --verbose
```

回答後會顯示 `eval rate` （tokens/s）。以體感來看， **15 tokens/s 以上通常可舒服地日常使用** ，低於 10 就會開始有明顯的等待感。

將 14B 與 27B 用同一個問題比較，會有助於決定日常要使用哪個模型。我自己的環境最後是分成兩種用途：日常用 14B，需要它慢慢思考時再用 27B。

### Step 4: 準備 Python 環境

接下來進入 RAG 的建置。先建立專案資料夾與虛擬環境。

```
mkdir ~/local-rag && cd ~/local-rag
python3 -m venv venv
source venv/bin/activate
```

安裝必要套件。

```
pip install llama-index \
            llama-index-llms-ollama \
            llama-index-embeddings-ollama \
            llama-index-vector-stores-chroma \
            chromadb
```

如果要讀取 PDF，另外還需要安裝以下套件：

```
pip install pypdf
```

### Step 5: 放置資料

把要讀取的文件放到 `data` 目錄。

```
mkdir data
# 將 PDF 或文字檔放到 ./data
```

建議一開始先從 1～2 個檔案開始。若檔案太多，初次做嵌入處理會花比較久時間，不利於確認是否能正常運作。

### Step 6: 建立 RAG 腳本

將以下內容儲存為 `rag.py` 。

```
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader, Settings
from llama_index.llms.ollama import Ollama
from llama_index.embeddings.ollama import OllamaEmbedding

# 指定要使用的模型
Settings.llm = Ollama(model="qwen3:14b", request_timeout=180)
Settings.embed_model = OllamaEmbedding(model_name="bge-m3")

# 讀取 ./data 的文件 → 嵌入 → 建立索引
docs = SimpleDirectoryReader("./data").load_data()
index = VectorStoreIndex.from_documents(docs)

# 提問
qe = index.as_query_engine(similarity_top_k=4)
resp = qe.query("請告訴我這份資料的三個重點")
print(resp)
```

執行：

```
python rag.py
```

如果能根據放入的資料內容回傳回答，就代表本機 RAG 已經完成了。

## 程式碼說明

雖然只有十幾行，卻包含了 RAG 的主要要素。

[![rag-pipeline-flow.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4105422%2F995506fd-b31a-4a1f-80f4-e51483c400ed.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2cf95099bcd7f945b269d60818dc3cfb)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4105422%2F995506fd-b31a-4a1f-80f4-e51483c400ed.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2cf95099bcd7f945b269d60818dc3cfb)

**`Settings.llm` 與 `Settings.embed_model`**  
將生成模型與嵌入模型分開指定。兩者都會參照本機的 Ollama，因此不會產生對外網路通訊。

**`SimpleDirectoryReader`**  
自動讀取目錄內的檔案。會依副檔名選擇適當的解析器，例如 PDF、文字檔、Markdown 等。

**`VectorStoreIndex.from_documents`**  
將文件切成適當長度的片段（chunk 化），再透過嵌入模型向量化並建立索引。第一次執行時這裡最花時間。

**`similarity_top_k=4`**  
表示針對問題，取出相關度最高的前 4 個 chunk 並交給 LLM。 **調整這個數字會改變回答傾向** ，因此它是精度調校的第一個重點。

## 疑難排解

**常見錯誤與處理方式：**

- **`Connection refused` 錯誤** ：Ollama 沒有啟動。請用 `ollama list` 確認是否有回傳模型列表。如果你同時有使用 LM Studio，請確認是 Ollama 那邊有啟動。
- **`address already in use` 錯誤** ：Ollama 已經在執行了。通常發生在 GUI 版已常駐時，這時不需要再執行 `ollama serve` 。
- **第一次執行異常慢** ：嵌入處理的時間與文件量成正比。這是因為 `bge-m3` 正在把全文做向量化，並非異常。請先用少量檔案測試。
- **PDF 無法讀取** ：需要安裝 `pip install pypdf` 。
- **載入模型時記憶體不足** ：可以提高分配給 GPU 的記憶體上限。以下設定在重新開機後會重設。 \`\`\`  
	sudo sysctl iogpu.wired\_limit\_mb=40960

## 充分運用 48GB 的重點

**優先選擇 MLX 格式**  
針對 Apple Silicon 最佳化的 **MLX 格式** 模型，即使參數量相同，也通常比 GGUF 更快、更省記憶體。在 Ollama 或 LM Studio 選模型時，如果有標示 `-mlx` ，優先選那個會比較有利。

**注意模型同時占用記憶體**  
在 RAG 中，生成模型與嵌入模型會同時消耗記憶體。如果想常駐使用 32B 級模型，建議嵌入端選較輕量的 `bge-m3` ，會更有餘裕。

**先從輕量模型開始**  
如果一開始就選很大的模型，執行速度會變慢，也很難分辨是「沒跑成功」還是「只是很慢」。先用 14B 跑通整個流程，再往上擴充，反而是更快的方法。

## GUI 工具也是一種選擇

這次是用 Python 建置，但也有不寫程式就能試 RAG 的方法。

- **AnythingLLM** ：專為 RAG 設計的 GUI 工具。只要拖放文件就能建立知識庫
- **LM Studio** ：可以用 GUI 完成模型搜尋、下載與執行。優點是比較容易找到 MLX 格式模型

這兩者都能在底層搭配 Ollama 使用，因此可以和這次建好的環境並用。如果只是想先體驗「RAG 到底是什麼」，從 GUI 入門也是很不錯的選擇。

## 總結

我在 Mac mini（M4 Pro / 48GB）上，建立了完全本機的 LLM + RAG 環境。

回頭整理後，重點可以濃縮成以下三點：

1. **Apple Silicon 的統一記憶體很適合本機 LLM**
2. **RAG 需要生成模型與嵌入模型兩種模型**
3. **使用 LlamaIndex 的話，RAG 的基本架構十幾行就能寫出來**

資訊不會外流，而且怎麼試都免費，這種環境大幅降低了反覆實驗的門檻。當我把 RAG 的概念整理成文章時，理解還停留在概念層面；但實際動手後，像「chunk 分割」、「檢索數量」這些參數的意義就能更有感地掌握。

不過，目前這個腳本還是有課題。因為每次執行都會重新做嵌入處理，當文件變多時，等待時間就會變得不能忽視。

## 參考

- [Ollama 官方網站](https://ollama.com/)
- [LlamaIndex 文件](https://docs.llamaindex.ai/)

---

原文出處： [https://qiita.com/y-okayama-tb/items/0127a7c5203f44507474](https://qiita.com/y-okayama-tb/items/0127a7c5203f44507474)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/37Zzex) [8瀏覽](https://codelove.tw/@tony/post/37Zzex)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/37Zzex)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 💬1 ❤️6

316

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

113

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785288106&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37Zzex&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTg3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjE4NyJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTg3Il1dLDBd&amp;dt=1785288106007&amp;bpp=2&amp;bdt=93&amp;idt=105&amp;shv=r20260721&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785287841%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785287841%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785287841%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1828062459725&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95397132%2C122880017%2C95396252&amp;oid=2&amp;pvsid=8025565744266887&amp;tmod=2121136989&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=143" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260721/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=2&amp;uci=a!2&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>