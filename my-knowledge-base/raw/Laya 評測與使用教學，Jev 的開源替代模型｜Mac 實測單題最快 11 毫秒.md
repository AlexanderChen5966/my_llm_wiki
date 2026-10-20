---
title: "Laya 評測與使用教學，Jev 的開源替代模型｜Mac 實測單題最快 11 毫秒"
source: "https://klab.tw/2026/09/laya-review-tutorial/"
author:
  - "[[Kyle]]"
published: 2026-09-25
created: 2026-10-06
description: "Laya 是 Convai Innovations 以 Apache 2.0 開源的決策模型，問題格式與 TypeSafe Jev 相同，可以在自己的電腦執行，不必呼叫雲端 API。本文在 M4 Pro Mac 實測安裝步驟、GPU 與 CPU 的延遲和記憶體用量，並用繁體中文與英文測試客服分流、Agent 選工具與 RAG 文件過濾，整理各題型的準確度、信心門檻的效果、Jev 相容 API 的架設方式與使用限制。"
tags:
  - "clippings"
status: processed
---
Laya 是 Convai Innovations 在 2026 年 9 月 18 日以 Apache 2.0 授權開源的決策模型。它和 TypeSafe AI 的 Jev 一樣不生成文字，只回答程式事先定義好的選擇題、評分題與是非題；差別在權重可以下載，能在自己的電腦或伺服器上執行。

這篇在 M4 Pro Mac 上安裝 Laya 0.3.20，量測 GPU 與 CPU 的延遲、吞吐量和記憶體，並用繁體中文與英文各 12 題測試客服分流、Agent 選工具與 RAG 文件過濾，最後整理用 `laya-serve` 架設 Jev 相容 API 的方式與限制。

## Laya 是什麼

Laya 由 Convai Innovations 發布，原始碼放在 GitHub 的 [NandhaKishorM/laya](https://github.com/NandhaKishorM/laya) ，權重放在 Hugging Face 的 [convaiinnovations/laya](https://huggingface.co/convaiinnovations/laya) 。第一版在 2026 年 9 月 18 日上架 PyPI，程式碼與權重都採用 Apache 2.0 授權。

官方把 Laya 定位為 System 1 決策引擎，System 1 指快速、直覺式的判斷：程式把一段文字或 JSON 當作 `state` 交給模型，再用 `questions` 定義問題與可選答案，模型在一次運算中算出每個答案的機率。Laya 不逐字生成文字，因此不會回傳格式錯誤或不在選項裡的答案，適合客服分流、Agent 選工具、RAG 文件過濾與輸出檢查這類答案範圍已知的判斷。

問題型別沿用 Jev 的三種：Choice 從選項挑一個、Score 依等級評分、Noul 回傳是非題「是」的機率。Jev 是 TypeSafe AI 在 2026 年 9 月 15 日推出的雲端決策模型，只能透過 API 使用、不公開權重；Laya 三天後發布，把同樣的用法做成可以下載、自己架設的版本，HTTP 端點也沿用 Jev 的格式。Laya 不是 TypeSafe 的產品，兩者由不同公司開發。Jev 的功能、價格與限制在 [Jev 是什麼？TypeSafe System One 決策模型](https://klab.tw/2026/09/jev-system-one-ai-model-guide/) 有完整整理。

| 版本 | 骨幹模型 | 參數量 | 權重檔 | Context | 用途 |
| --- | --- | --- | --- | --- | --- |
| 英文版（ `laya` ） | ModernBERT-large | 4.21 億 | 843 MB | 512 tokens | 英文 |
| 多語版（ `multilingual` ） | mmBERT-base | 3.22 億 | 644 MB | 1,024 tokens，可調到 8,192 | 100 多種語言，包含繁體中文 |
| 微調版（ `typed-decisions` ） | ModernBERT-large | 4.21 億 | 843 MB | 1,024 tokens | 官方微調的 4 種工作流程 |

三個版本放在同一個 Hugging Face 專案裡。官方建議透過 `Router` 使用：它會先判斷輸入的文字，英文送英文版，中文、日文等非拉丁文字送多語版，所以繁體中文的請求會自動走多語版。

## Laya 和 Jev 的差別

兩者的問題格式相同，差別在誰負責執行模型。以下 Jev 的資訊依 TypeSafe 與各平台的官方文件，查詢日期為 2026 年 9 月 25 日。

| 項目 | Laya | Jev |
| --- | --- | --- |
| 開發者 | Convai Innovations | TypeSafe AI |
| 發布 | 2026 年 9 月 18 日，開源 | 2026 年 9 月 15 日，early access |
| 權重 | 可從 Hugging Face 下載，Apache 2.0 | 不公開 |
| 執行位置 | 自己的電腦或伺服器，支援 CPU、NVIDIA GPU 與 Apple GPU | TypeSafe 雲端，或 Vercel AI Gateway、Cloudflare Workers AI 等平台 |
| 費用 | 沒有使用費，硬體與電費自行負擔 | 輸入每百萬 tokens 0.042 美元（約新台幣 1.3 元），輸出不計費 |
| Context | 英文版 512 tokens，多語版 1,024 tokens（可調到 8,192） | 每次 request 64k tokens |
| 微調 | 可以用自己的資料微調 | 不提供微調與自架 |
| HTTP API | `laya-serve` 提供相同格式的 `POST /v1/systemone` | `POST https://api.typesafe.ai/v1/systemone` |

Laya 的 GitHub 說明列出與 Jev 的準確度比較：AG News 4 類新聞分類是 0.950 比 0.910，DAIR Emotion 6 類情緒是 0.595 比 0.480；Banking77 這類 70 多個選項的題目則是 Jev 領先，0.870 比 0.425（Jev 測 72 個選項、Laya 測 77 個）。這些 Jev 數字來自第三方公布的結果，Laya 團隊表示沒有 TypeSafe API 權限、沒有自己測過 Jev，題目與樣本數也不同，只能當作方向參考。

Laya 的好處是資料不必離開自己的機器、沒有 token 費用，也可以用自己的資料微調；代價是主機、記憶體與模型版本都要自己維護。需要一次比較 50 個以上的選項、處理幾萬 tokens 的長文件，或不想維護推論主機時，Jev 仍是比較省事的選擇。

## Laya 的 Mac 安裝步驟

Laya 是 Python 套件，需要 Python 3.10 以上。macOS 內建的 `python3` 是 3.9，要另外安裝新版，例如用 Homebrew 安裝 `python@3.12` ，Homebrew 的基本操作可參考 [macOS Homebrew 新手教學](https://klab.tw/2025/09/macos-homebrew-tutorial/) 。Apple Silicon 的 Mac 會自動使用 GPU（PyTorch 的 MPS 後端），沒有可用 GPU 的電腦則用 CPU 執行。

### 安裝步驟

```bash
# 建立獨立的 Python 環境（需要 Python 3.10 以上）
python3.12 -m venv laya-env
source laya-env/bin/activate

# 安裝 Laya；[serve] 會一併安裝 HTTP 伺服器需要的 FastAPI 與 Uvicorn
python -m pip install "laya[serve]==0.3.20"

# 確認版本，以及 Apple GPU（MPS）能不能使用
python -c "import laya, torch; print(laya.__version__, torch.backends.mps.is_available())"
# 輸出：0.3.20 True
```

安裝會一併裝上 PyTorch 與 Transformers，在這台 Mac 上約 35 秒完成，虛擬環境佔 0.7–0.9 GB。Laya 在發布後一週內出了 28 個版本，指令裡固定版本號，升級前再看 PyPI 的版本紀錄，避免答案的機率分布在不知情的情況下改變。

### 第一次執行與模型下載

以下範例把一封客服信交給 Laya，同時問負責部門（Choice）與是否要取消服務（Noul）。 `Router` 會在第一次用到某個版本時才下載權重，這個中文例子只會下載多語版。

```python
from laya import Router

router = Router()  # 第一次使用時才下載需要的模型

state = "三月份被重複扣款兩次，請今天退款，否則會取消訂閱。"
questions = {
    "department": {
        "type": "choice",
        "instructions": "這封客服信應該交給哪個部門處理？",
        "criteria": {
            "billing": "帳單、扣款、退款、發票",
            "technical": "系統故障、錯誤訊息、無法使用",
            "other": "其他詢問",
        },
    },
    "churn": {
        "type": "noul",
        "instructions": "寄件者是否表示要取消服務？",
    },
}

result = router.predict(state, questions)
print(result["routing"]["model"])                      # 實際使用的模型
print(result["answers"]["department"]["choice"])       # 部門
print(result["answers"]["department"]["probabilities"])
print(result["answers"]["churn"]["noul"])              # 是的機率（0～1）
```

```
multilingual
billing
{'billing': 1.0, 'technical': 0.0, 'other': 0.0}
0.8753
```

權重會存在 `~/.cache/huggingface/hub` ，英文版加多語版共約 1.5 GB，實測分別花 18.7 秒與 14.5 秒下載完成。下載完成後可以設定離線模式，讓之後的載入不再連線檢查更新：

```bash
# 只使用已下載的權重，不再連線到 Hugging Face
export HF_HUB_OFFLINE=1
```

### 安裝前的套件檢查

Laya 發布才一週，版本更新又頻繁，安裝前做了三項檢查：PyPI 上 0.3.20 的套件內容與 GitHub 的 `v0.3.20` 標籤逐檔相同；權重用 safetensors 格式載入，不會執行權重檔裡的程式；0.3.20 的原始碼沒有遙測，推論時不連網，網路只用在下載權重。第一項可以用下面的指令自己比對：

```bash
# 下載 PyPI 上的套件並解開（不安裝）
python -m pip download "laya==0.3.20" --no-deps -d laya-wheel
unzip -q laya-wheel/laya-0.3.20-py3-none-any.whl -d laya-wheel/src

# 取得 GitHub 上同一版本的原始碼，比對兩邊的 laya 資料夾
git clone --depth 1 --branch v0.3.20 https://github.com/NandhaKishorM/laya.git laya-src
diff -rq laya-src/laya laya-wheel/src/laya && echo "內容相同"
```

## 實測環境與方法

| 項目 | 內容 |
| --- | --- |
| 電腦 | M4 Pro、64 GB 統一記憶體，macOS 27.0 |
| 軟體 | Python 3.12.14、PyTorch 2.14.0、Transformers 5.17.0、laya 0.3.20 |
| 模型 | 英文版與多語版，Hugging Face 2026 年 9 月 24 日的版本，不量化 |
| 裝置 | GPU（MPS）與 CPU（PyTorch 預設 10 個執行緒） |
| 測試內容 | 1 封信問 1 題、1 封信問 5 題、16 封信各問 3 題（批次） |
| 量法 | 每組先暖機 5 次，再量 50 次取中位數（p50）與 p95；每組在獨立的行程執行 |
| 記憶體 | 行程的 phys\_footprint（活動監視器「記憶體」欄採用的數值），以 1 GB＝1,024 MB 換算 |

英文版用英文客服信，多語版用內容相同的繁體中文客服信，也就是兩個版本各自處理 Router 會交給它的語言。GPU 上一次推論有 5 列以上時，Laya 會自動改用 fp16 計算，CPU 則維持 fp32。

## Laya 在 M4 Pro 上的執行速度

![Laya 在 M4 Pro 上的單題延遲與批次吞吐量：多語版 GPU 11.2 毫秒、每秒 113 題，英文版 GPU 20.2 毫秒、每秒 60 題，多語版 CPU 29.7 毫秒、每秒 70 題，英文版 CPU 87.0 毫秒、每秒 32 題](https://klab.tw/wp-content/uploads/2026/09/laya-speed-mobile.png)

多語版在 GPU 上最快，英文版改用 CPU 時最慢。

多語版在 GPU 上單題 11.2 ms，是四種組合裡最快的；英文版在 CPU 上單題 87.0 ms，是前者的 7.8 倍。

| 組合 | 載入 | 第一次推論（5 題） | 1 題 p50 | 1 題 p95 | 5 題 p50 | 16 封信 × 3 題 | 吞吐量 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 多語版・GPU | 2.9 秒 | 534 ms | 11.2 ms | 12.6 ms | 46.6 ms | 424 ms | 每秒 113 題 |
| 英文版・GPU | 4.7 秒 | 1,389 ms | 20.2 ms | 22.6 ms | 95.0 ms | 799 ms | 每秒 60 題 |
| 多語版・CPU | 3.4 秒 | 149 ms | 29.7 ms | 35.3 ms | 90.3 ms | 682 ms | 每秒 70 題 |
| 英文版・CPU | 1.9 秒 | 265 ms | 87.0 ms | 255.8 ms | 190.7 ms | 1,487 ms | 每秒 32 題 |

### 多語版比英文版快

多語版的骨幹是 mmBERT-base，參數比英文版的 ModernBERT-large 少約四分之一，單題速度在 GPU 上是英文版的 1.8 倍，在 CPU 上是 2.9 倍。官方也把多語版標為速度較快的版本，所以繁體中文走多語版，速度反而比英文好。

### 問題愈多，推論時間愈長

在 GPU 上，同一封信從問 1 題變成問 5 題，時間變成 4.1–4.7 倍；CPU 上是 2.2–3.0 倍。原因是 Laya 把每個問題各自組成一條序列，客服信的內容在每一條裡都會重複一次：英文信本身只有 18 個 tokens，問 5 題時 `usage` 回報的輸入是 292 個 tokens。問題愈多、文件愈長，運算量就愈大，只問需要的題目，就能直接減少運算時間。

### 批次在 CPU 上的效果比 GPU 明顯

把 16 封信合成一批，用 `predict_batch()` 一次送出，GPU 上的吞吐量只比逐題處理多 21–27%；CPU 則變成 2.1–2.8 倍。使用 GPU 時，批次的主要好處是少呼叫幾次，速度提升有限。另外，英文版在 CPU 上的 p95 約是 p50 的 3 倍，延遲波動比其他組合大。

### 和官方 T4 數據與 Jev 的比較

官方在 NVIDIA T4 上測得英文版單題 39.5 ms、多語版 32.8 ms，M4 Pro 的 GPU 分別是 20.2 ms 與 11.2 ms；問 5 題時官方是 84.5 ms 與 40.1 ms，M4 Pro 是 95.0 ms 與 46.6 ms。兩邊的測試文字與量法不同，只能看出落在同一個量級。

Laya 的 GitHub 說明引用第三方對 Jev 的量測，單題 p50 為 236–276 ms。Jev 只能透過網路呼叫，這個數字應該包含網路往返的時間，和本機推論的條件不同，不能直接比較；把 Laya 放在應用程式旁邊執行，就沒有這段網路往返。

載入後的第一次推論會慢很多：同樣問 5 題，GPU 上英文版第一次要 1.4 秒、多語版 0.5 秒，暖機後只要 95 ms 與 47 ms。做成服務時，建議啟動後先送一次暖機請求，避免第一個使用者等待。

## Laya 的記憶體用量

![Laya 記憶體峰值與權重檔大小：英文版 GPU 4.1 GB、多語版 GPU 3.3 GB、英文版 CPU 2.3 GB、多語版 CPU 2.0 GB、兩版同時載入 4.6 GB](https://klab.tw/wp-content/uploads/2026/09/laya-memory-mobile.png)

執行時的記憶體約為權重檔的 2.9–5.5 倍。

單一模型執行時的記憶體峰值在 2.0–4.1 GB 之間，約為權重檔的 2.9–5.5 倍。GPU 比 CPU 多用 1–2 GB，英文版與多語版同時載入時是 4.6 GB。

| 組合 | 權重檔 | 載入後 | 峰值 |
| --- | --- | --- | --- |
| 英文版・GPU | 843 MB | 3.1 GB | 4.1 GB |
| 多語版・GPU | 644 MB | 2.0 GB | 3.3 GB |
| 英文版・CPU | 843 MB | 1.9 GB | 2.3 GB |
| 多語版・CPU | 644 MB | 1.7 GB | 2.0 GB |
| 兩版同時・GPU（Router） | 1.5 GB | 3.6 GB | 4.6 GB |

### 權重檔是 fp16，載入後變成 fp32

Hugging Face 上的權重以 fp16 儲存，Laya 載入時會先建立 fp32 的模型再把權重填進去，所以光是權重在記憶體裡就是檔案的兩倍：英文版約 1.6 GB、多語版約 1.2 GB。GPU 模式還有 PyTorch 保留的 MPS 暫存區，跑過批次推論後會變大，多語版就從載入後的 2.0 GB 增加到 3.3 GB。

### 用 ps 或 top 看會低估 GPU 模式

`ps` 與 `top` 顯示的 RSS 不包含 GPU 的配置。英文版在 GPU 上的 RSS 只有 1.3 GB，實際佔用是 4.1 GB。實測在 MPS 上配置 512 MB 的張量，phys\_footprint 增加 512 MB，RSS 只增加 0.3 MB。查看 Laya 用了多少記憶體，要看活動監視器的「記憶體」欄，或用 `footprint` 、 `vmmap` 這類工具。

記憶體較小的 Mac 可以改用 CPU：多語版峰值 2.0 GB，單題 29.7 ms，延遲仍在數十毫秒內。 `Router` 只載入用到的版本，預設最多同時保留 2 個；想限制只保留 1 個，可以設定 `Router(max_loaded=1)` ，或直接用 `laya.load()` 載入單一版本。

## 客服分流、Agent 選工具與 RAG 過濾實測

三種用途各準備 12 題，每題寫成繁體中文與英文兩個版本，內容相同，交給 `Router` 自動選擇版本，在 GPU 上執行。中文全部走多語版，英文全部走英文版。題目與標準答案都是自行設計，每個用途只有 12 題，結果只能看出傾向，不能當作正式評測。

| 用途 | 問題（型別） | 繁體中文 | 英文 |
| --- | --- | --- | --- |
| 客服分流 | 負責部門（Choice，5 選 1） | 11／12 | 12／12 |
| 客服分流 | 緊急程度（Score，3 級） | 5／12 | 5／12 |
| 客服分流 | 是否要取消服務（Noul） | 10／12 | 7／12 |
| Agent 選工具 | 要呼叫的工具（Choice，5 選 1） | 11／12 | 11／12 |
| Agent 選工具 | 是否需要使用者確認（Noul） | 7／12 | 10／12 |
| RAG 過濾 | 文件能否回答問題（Noul） | 10／12 | 11／12 |
| RAG 過濾 | 相關程度（Score，3 級） | 4／12 | 12／12 |

每次請求問 2–3 題，多語版處理中文花 18–35 ms，英文版處理英文花 35–62 ms。

### 選擇題最穩定

負責部門與工具選擇兩個 Choice 問題，48 個答案答對 45 個。答錯的 3 題：中文要求「查一下今天台北的天氣」的指令被分到計算機；中文回報「匯出報表時一直出現 500 錯誤」、並表示考慮改用其他廠商的客服信被分到業務；英文「Send these meeting notes to everyone in the department」被分到行事曆。選項寫得具體、彼此不重疊時，Choice 的準確度比 Score 與 Noul 高。

### 評分題集中在中間值

Score 回傳的是各級機率的期望值，實測時很少接近兩端。「整個系統從早上九點開始打不開，全公司都無法作業」只得到 1.52 分（滿分 2），註明「不急」的閃退回報中文版反而有 1.36 分。RAG 相關程度在英文是 12 題全對，中文卻把 12 份文件都評在 0.64–1.31 分之間，分不出「無關」和「包含答案」。官方的說明也寫到 Score 是三種型別中最弱的。

需要分級時，可以把等級改成 Choice 的選項，或拆成「是否緊急」「是否影響全公司」這類 Noul 問題，再由程式組合結果；中文的相關程度判斷可以改用「文件能否回答問題」的 Noul，這題中文答對 10／12。

### 是非題容易照字面判斷

「是否需要使用者確認」這題需要先理解動作會不會對外造成影響，中文版錯得最多（12 題錯 5 題）。中文要求在「下週三下午兩點」與設計部開一小時會議的指令，判定需要確認的機率只有 0.009，「每天存 150 元，存一整年 365 天總共是多少？」卻有 0.705。英文的閃退回報寫了「No rush」，仍被判定有 0.80 的機率要取消服務。官方也提到 Noul 有時會依選項標籤作答，而不是根據內容判斷。會造成副作用的動作，應該由程式依工具類型決定是否要確認，不要交給模型判斷。

### 信心門檻能擋掉大部分錯誤

Laya 的每個答案都附有 `answer_confidence` 。Choice 與 Noul 共 120 個答案，整體答對 100 個；只採用信心值 0.9 以上的答案時，剩下 64 個、答對 61 個；門檻降到 0.8，剩下 84 個、答對 76 個。低於門檻的交給人工或生成式 LLM 處理，就能把自動處理的錯誤率壓低。

不過門檻擋不住所有錯誤：中文「是否需要使用者確認」答錯的 5 題，平均信心值反而有 0.89。官方也建議先用自己的資料做 temperature 校準，再決定門檻。另外，回應裡的 `act_probability` 官方已說明沒有參考價值；設定門檻時建議用 `answer_confidence` ，也就是回報答案的機率，三種題型都適用。

## 用 laya-serve 自架 Jev 相容 API 的方式

`laya-serve` 是 Laya 內建的 HTTP 伺服器，提供和 Jev 相同格式的 `POST /v1/systemone` 。依官方說明，現有的 Jev client 只要把 base URL 改成自己的伺服器就能使用。啟動前有兩個預設值要先改：

- **預設綁定 0.0.0.0** ：同一個網路上的其他裝置都連得到，而且沒有設定 `LAYA_API_KEY` 時不需要任何驗證
- **預設預載三個版本** ：包含微調版，第一次啟動會多下載微調版約 843 MB 的權重，也會多佔記憶體

```bash
# 只接受本機連線，只載入英文版與多語版
LAYA_HOST=127.0.0.1 LAYA_PORT=8000 \
LAYA_MODELS=english,multilingual \
laya-serve
```

在另一個終端機視窗送出請求：

```bash
# 確認伺服器已載入模型
curl -s http://127.0.0.1:8000/health

# 用 Jev 的格式送出問題
curl -s http://127.0.0.1:8000/v1/systemone \
  -H 'content-type: application/json' \
  -d '{"state": "App 在 iOS 27 上一打開就閃退，可以幫忙看一下嗎？",
       "questions": {"department": {"type": "choice",
         "instructions": "這封客服信應該交給哪個部門？",
         "criteria": {"billing": "帳單、扣款、退款",
                      "technical": "故障、錯誤、無法使用"}}}}'
```

回應包含 `answers` 、 `usage` 與 `routing` ， `routing` 記錄這次用了哪個版本與原因（以下省略部分欄位）：

```json
{
  "model": "laya-rl-agent",
  "answers": {
    "department": {
      "type": "choice",
      "choice": "technical",
      "probabilities": {"billing": 0.0288, "technical": 0.9712},
      "confidence": 0.8114,
      "answer_confidence": 0.9712
    }
  },
  "usage": {"input_tokens": 56, "output_tokens": 0},
  "routing": {
    "model": "multilingual",
    "reason": "non-Latin script (han, 73% of letters); the English checkpoint cannot read it"
  }
}
```

實測從啟動到可以回應花 4.1 秒，載入兩個版本後佔 3.6 GB。一封信問 3 題的延遲 p50，英文 57.5 ms、中文 30.4 ms，和直接呼叫 Python 的結果相近，本機 HTTP 只增加很少的時間。

從 Jev 移植既有的 client 時，Laya 在 GitHub 最新的說明列出三個差異：

- **選項數量** ：每題的選項共用固定的 token 額度，大約超過 20 個附短描述的選項就會被截短；Jev 的上限是 255 個選項
- **Score 的等級** ：每一級都必須有描述，沒有描述的等級會回傳 422 錯誤
- **信心值** ：Choice 與 Score 的 `confidence` 算法和 Jev 不同，在 Jev 上設定的門檻不能直接沿用，官方建議改用 `answer_confidence`

從 0.3.20 的原始碼看，同一個 `laya-serve` 行程一次只執行一個推論，同時送來的請求會排隊，吞吐量大約是單次延遲的倒數。要開放給其他機器使用時，除了設定 `LAYA_API_KEY` ，也建議放在有 HTTPS 的反向代理後面，不要直接把 8000 port 暴露在網路上。停止伺服器按 Control + C。

## Laya 的限制

以下前五項是官方在 Hugging Face 模型卡與 GitHub 說明列出的已知限制，後兩項是實測時遇到的情況：

- **選項太多時準確度下降** ：所有選項共用固定的 token 額度，77 個選項時每個選項只分到 3–4 個 tokens。官方的比較在 20 個選項以上就由 Jev 領先，並提供 `predict_shortlist` 先縮小候選範圍
- **Score 是最弱的題型** ：官方在 SST-5 五級情感評分只有 0.372，和這次實測的結果一致
- **需要微調才能用在特定領域** ：英文版未經微調時，在官方 typed-decisions 資料集只有 0.362，低於全部猜最常見答案的 0.461
- **出廠時信心值偏高** ：官方建議用自己的資料做 temperature 校準；英文版載入時會出現 `RuntimeWarning` ，提醒 11 個選項以上的 Choice 信心值未經校準
- **長文件不穩定** ：多語版設定 `max_len=8192` 可以讀到 8,192 tokens，但文件超過約 4,000 tokens 後，官方測試的 20 題只答對 8–17 題；Apple GPU 處理 4,000 tokens 約需 1.7 秒
- **繁體中文的評分題不可靠** ：實測中文的 Score 問題幾乎都落在中間值，中文判斷建議以 Choice 與 Noul 為主
- **版本變動快** ：發布一週內出了 28 個版本，正式環境要固定套件版本與 Hugging Face 的權重版本，升級後重新驗證門檻

Laya 和 Jev 一樣不能生成文字，需要寫回覆、摘要或程式時仍要交給生成式 LLM；會付款、寄信或刪除資料的動作，應該保留程式規則與人工確認，不能只看模型的判斷。

## 常見問答

### Laya 是 TypeSafe 官方推出的開源版 Jev 嗎

不是。Laya 由 Convai Innovations 發布，Jev 由 TypeSafe AI 開發，兩者是不同公司的產品。Laya 採用相同的問題格式，並提供相容 Jev 的 HTTP 端點，所以可以當作 Jev 的自架替代方案。

### Laya 一定要有 GPU 嗎

不需要。M4 Pro 的 CPU 上，多語版單題 29.7 ms、記憶體峰值 2.0 GB；英文版單題 87.0 ms、峰值 2.3 GB。GPU 的單題速度是 CPU 的 2.6–4.3 倍，但會多用 1–2 GB 記憶體。

### Laya 支援繁體中文嗎

支援。 `Router` 會把繁體中文交給多語版，官方的 51 種語言測試中，繁體中文在 20 選 1 的意圖分類是 0.540（隨機猜測為 0.05）。實測中文的 Choice 與 Noul 表現接近英文，Score 則明顯較差。

### 可以用 Ollama 執行 Laya 嗎

官方沒有提供 Ollama 版本。Laya 是以 ModernBERT 與 mmBERT 為骨幹的判斷模型，不是 Ollama 執行的生成式 LLM，要透過 Python 套件或 `laya-serve` 使用。Ollama 的用法可參考 [Ollama 入門教學](https://klab.tw/2025/09/ollama-tutorial/) 。

### Laya 會把資料傳到網路上嗎

推論在本機執行。0.3.20 的原始碼只在下載權重時連到 Hugging Face，下載完成後設定 `HF_HUB_OFFLINE=1` 就不會再連線。LangChain 整合設定成呼叫遠端的 `laya-serve` 時，資料會送到指定的伺服器。

## 參考來源

- [Hugging Face: convaiinnovations/laya](https://huggingface.co/convaiinnovations/laya) (Laya 模型卡、規格與已知限制)
- [GitHub: NandhaKishorM/laya](https://github.com/NandhaKishorM/laya) (原始碼、安裝、HTTP 伺服器與 Jev 比較)
- [GitHub: laya/BENCHMARKS.md at main](https://github.com/NandhaKishorM/laya/blob/main/BENCHMARKS.md) (多語言與校準評測)
- [PyPI: laya](https://pypi.org/project/laya/) (Python 套件與版本紀錄)
- [TypeSafe AI Blog: Introducing System One Models & Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) (System One 模型與 Jev 介紹)
- [TypeSafe AI: Models](https://docs.typesafe.ai/models) (Jev 版本、價格與部署方式)