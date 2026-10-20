---
title: Laya 開源決策模型 — 自架評估、數字前提與移植
date: 2026-10-08
tags: [ai/agent, ai/workflow, type/how-to]
source: https://klab.tw/2026/09/laya-review-tutorial/; https://foreverwebs.com/blog/laya-open-source-decision-model-guide; https://vervecode.dev/ai/jev-laya-decision-models/; https://www.aiposthub.com/llamacpp-systemone-local-decision-models/
type: article
status: processed
asset_value: 75
---

評估要不要自架 Laya、要不要把 Jev client 移植過來、要不要自己微調時回查的參考資料。
生態全貌與三產品比較見 [[decision-model-ecosystem]]；Jev 本身的 API 與設計模式見 [[jev-implementation-guide]]。

> ⚠️ **時效警示：Laya 發布一週就出了 28 個版本**（klab 記錄）。以下凡是「版本快照」區的資訊都標了版本與日期，
> **截至 2026-09-25（laya 0.3.20）**；「穩定層」是不隨版本失效的結構性結論。使用前先看 PyPI 版本紀錄。

⚠️ **利益相關揭露**：三個來源中，**恆遠數位科技（foreverwebs）是 AI 導入服務商，屬廠商來源**。
本條只取其技術內容與實測數字（標「作者實測（廠商）」），CTA、評估表、服務介紹與產品置入全數排除。
值得一提的是恆遠立場反而偏保守（強調 0.362 陷阱、「免費≠零成本」），沒有吹捧 Laya。
另兩個來源 klab（Kyle，M4 Pro 實測）與 vervecode（Ted Liou，RTX 3070 實測）無業配。三位是**不同作者、各自實機測試**。

## 身分（先釐清再往下讀）

- 開發者：**Convai Innovations**（印度，CEO Nandakishor M），與遊戲對話公司 convai.com **無關**，與 TypeSafe AI **無關**
- 發布：2026-09-18（PyPI 0.1.0 04:38、GitHub 04:46、HF 05:05，恆遠交叉核對時間戳）；程式碼與權重皆 **Apache 2.0**，訓練腳本與微調 notebook 也開源
- 自稱：「**System 1 決策引擎**」。🔴 「Jev 的開源替代」是**第三方定位**（klab 標題、恆遠被動句）；klab FAQ 明說 **Laya 不是 TypeSafe 推出的開源版 Jev**，只是採用相同問題格式並提供相容端點
- 原始碼 GitHub `NandhaKishorM/laya`；權重 Hugging Face `convaiinnovations/laya`

## T（核心論點）

🔑 **Laya 是可帶回家的判斷「底座」，不是開箱即用的 Jev 替代品。**
基礎版零樣本在官方 typed-decisions 資料集只有 **0.362，低於「永遠猜最大類」的 0.461**；官方宣傳的 0.766 是**用該 benchmark 訓練集微調過**的 typed-decisions 版。
要用就得自己微調、自己重設門檻。

🔑 **把 Jev client 改 base URL 指向 laya-serve 能讓程式跑起來，但門檻、選項數、Score 寫法都不能沿用**——
confidence 算法不同、選項共用一個小 token 預算、Score 每一級都要有描述。

## B（適用場景）

**適用：**
- 資料不能離開本機、或延遲預算在數十毫秒以內的判斷
- 選項數在 20 個以內、願意用自己的資料微調與重新校準門檻
- 需要看得到訓練方法、損失函數與權重的情境（閉源 API 給不了）

**不適用 / 注意：**
- 不想管 GPU、微調與版本固定的團隊——付費 API 比較省事（Artifilog 建議，恆遠轉述）
- 選項很多（>20）、長文件（>約 4,000 tokens）、繁中 Score 題
- 和 Jev 一樣**不能生成文字**；會付款、寄信或刪除資料的動作，要保留程式規則與人工確認（klab）→ [[agent-tool-authorization-gatekeeper]]

### 新手坑

1. **只看 0.766**——那是微調版；基礎版零樣本 0.362 比永遠猜最大類（0.461）還差
2. **輸入被靜默截短**——超過 token 預算不報錯（見 R 版本快照 0.3.3）
3. **沿用 Jev 的門檻**——confidence 算法不同
4. **以為加 `none` 選項模型就會正確放棄**——vervecode 實測 Laya 在兩題誤選 none
5. **把 laya-serve 預設設定直接開到網路上**——預設綁 0.0.0.0 且無驗證（0.3.20）
6. **用 `act_probability` 當訊號**——官方已說明沒有參考價值，改用 `answer_confidence`

## R（操作步驟）

### R-1　穩定層（結構性結論，不隨版本失效）

**1. 先讀懂數字前提**

| 官方數字 | 前提 | 來源 |
|---|---|---|
| typed-decisions 0.766（vs Jev 0.727） | 🔴 **用該 benchmark 自己的訓練集微調過的 checkpoint**；基礎版零樣本 **0.362**、多語版 0.342、隨機猜 0.318、**永遠選最大類 0.461** | 官方模型卡；恆遠解讀 |
| ECE 0.081（vs Jev 公布的 0.246） | 🔴 **溫度重新擬合後**的數字；**未調整前 0.466** | 官方；恆遠轉述 |
| README 與 Jev 比較：AG News 0.950 vs 0.910、DAIR Emotion 0.595 vs 0.480、Banking77 0.425 vs 0.870 | 🔴 **Jev 數字來自第三方**——Laya 團隊自陳**沒有 TypeSafe API 權限、沒有自測 Jev**；且 Banking77 Jev 測 72 類、Laya 測 77 類 | 官方 README；klab 轉述 |
| T4 單題 32.8 ms「比 Jev 快 7.8 倍」 | 🔴 比的是**本機推論 vs 跨海 API**（對照的是第三方 Jev 236–276 ms），**不是模型速度** | 官方；恆遠、klab 皆提醒 |
| SST-5 Score 0.372 | Score 是最弱題型 | 官方 |
| 51 種語言中 45 種可用；繁中 20 選 1 意圖分類 0.540（隨機 0.05） | 官方 51 語言測試（klab 9/25 引）；恆遠 9/23 寫「官方沒公布繁中單獨數字」——時間差，並列 | 官方 |

**2. 結構限制：編碼器＋共用 token 預算**
- Laya 是編碼器（ModernBERT／mmBERT）＋決策 head，不是生成式 LLM
- **所有選項共用一個固定 token 預算**（英文 192、多語 256），77 個選項時每個選項只分到 3–4 tokens；官方建議 **≤20 個選項**，20 個以上 Jev 領先，並提供 `predict_shortlist` 先縮小候選
  - 第三方 Artifilog 說「2–50 個選項可選 Laya」，與官方 20 並列，**以官方 20 為保守值**
  - issue #102 使用者回報：先粗篩到 20 個以內，54.3%→60.8%（**官方註明沒有重測**）
- **問題愈多愈慢**：每題各組一條序列、state 重複（英文信 18 tokens，問 5 題 usage 292 tokens，klab 讀碼＋實測）——與 Jev「問題平行評估、多問幾乎不增加時間」的直覺相反
- 長文件不穩：多語版 `max_len=8192` 可讀到 8,192 tokens，但 >約 4,000 tokens 時官方 20 題只答對 8–17 題

**3. confidence 不可跨模型沿用**
- Choice 與 Score 的 `confidence` 算法和 Jev 不同，官方建議門檻改用 `answer_confidence`
- 出廠時信心值偏高，官方建議用自己的資料做 temperature 校準；英文版載入時會出 `RuntimeWarning`，提醒 11 個選項以上的 Choice 信心值未經校準
- 英文版處理高棉文：**0.000 準確率配 0.952 信心**——錯得很篤定，信心門檻擋不住，所以 Router 在推論前先偵測書寫系統（官方）
- 信心門檻實測（klab，**樣本小**）：Choice＋Noul 120 答 100 對；≥0.9 留 64 答 61 對；≥0.8 留 84 答 76 對；但繁中「需確認」答錯的 5 題**平均信心 0.89** → 同類現象見 [[llm-judge-confidence-cascade]]
- 🔴 `act_probability`：**官方已說明沒有參考價值**，門檻用 `answer_confidence`（恆遠 9/23「可當第二道訊號」的建議為已被推翻的舊說法，**不採用**）

**4. 安裝前的套件安全檢查（通用步驟，klab）**
三項：PyPI 套件與 GitHub 同版本標籤逐檔相同、權重用 safetensors（不執行權重檔裡的程式）、原始碼無遙測。第一項可自行比對：

```bash
python -m pip download "laya==0.3.20" --no-deps -d laya-wheel
unzip -q laya-wheel/laya-0.3.20-py3-none-any.whl -d laya-wheel/src
git clone --depth 1 --branch v0.3.20 https://github.com/NandhaKishorM/laya.git laya-src
diff -rq laya-src/laya laya-wheel/src/laya && echo "內容相同"
```

下載完權重後可設 `export HF_HUB_OFFLINE=1`，之後不再連線。

**5. 從 Jev 移植的差異（官方 GitHub 說明）**
- 選項約 >20 個附短描述就會被截短（Jev 上限 255）
- Score 每一級**必須有描述**，否則回 422
- confidence 算法不同，門檻不能沿用
- 正式環境要**同時固定套件版本與 Hugging Face 權重版本**，升級後重新驗證門檻

### R-2　版本快照（標版本與日期；下次健康檢查優先複查）

**三個 checkpoint**

| checkpoint | 骨幹 | 權重大小 | context |
|---|---|---|---|
| `laya`（英文） | ModernBERT-large 421M | 約 808MB（恆遠）／843MB（klab） | 512 |
| `laya-multilingual` | mmBERT-base 322M | 約 647MB（恆遠）／644MB（klab） | 1,024（可調到 8,192）；約快 2.2 倍 |
| `laya-typed-decisions` | 421M，微調過 | — | 1,024 |

權重大小差異是 MB／MiB 換算差，**不是矛盾**。

**安裝與第一次執行（laya 0.3.20，klab 2026-09-25）**

```bash
python3.12 -m venv laya-env && source laya-env/bin/activate   # Python 3.10 以上
python -m pip install "laya[serve]==0.3.20"                    # [serve] 一併裝 FastAPI／Uvicorn
```

```python
from laya import Router
router = Router()   # 第一次用到某版本時才下載權重；繁中自動交給多語版
result = router.predict(state, questions)   # questions 格式同 Jev：choice／score／noul
```

**Router 保留幾個 checkpoint（版本演進，並列）**
- 2026-09-23 恆遠：預設只保留一個，語言切換要重建模型（官方中位數 CPU 7.4 秒、T4 10.3 秒），正式環境要 `Router(preload=True)`
- 2026-09-25 klab（0.3.20）：預設最多保留 2 個，可設 `max_loaded=1`

**laya-serve（0.3.20）**
- `POST /v1/systemone`，與 Jev 相同請求格式；回應含 `answers`、`usage`、`routing`
- 🔴 **預設綁 `0.0.0.0`、未設 `LAYA_API_KEY` 時無驗證、預設預載三版**（多下載 843MB）；單一行程一次只跑一個推論，同時送來的請求排隊（klab 讀 0.3.20 原始碼）
- 啟動 4.1 秒、載入兩版佔 3.6GB；3 題 p50 英文 57.5 ms／中文 30.4 ms
- 建議：只綁本機、限定要載入的版本；對外開放時設 `LAYA_API_KEY` 並放在 HTTPS 反向代理後

```bash
LAYA_HOST=127.0.0.1 LAYA_PORT=8000 LAYA_MODELS=english,multilingual laya-serve
```

**靜默截斷（SDK 0.3.3，vervecode 讀 SDK）**
- 整體輸入 1,024 tokens／題目＋選項區預算 256 tokens／單一選項先限 48 tokens——超過**自動截短、不報錯**
- state／criteria 擺放：vervecode 初版把文章描述放 state 時四題全選同一篇，改成「state 只放問題、描述移到 criteria 並縮成短英文主題」後改善——但**兩個變因同時改，無法分辨哪個有效**

**延遲與載入（硬體與量法不同，不是矛盾；引用時硬體、checkpoint、題數一起寫）**

| 環境 | 延遲 | 載入 | 來源 |
|---|---|---|---|
| M4 Pro 64GB，laya 0.3.20，1 題 p50 | 多語 GPU 11.2 ms／英文 GPU 20.2／多語 CPU 29.7／英文 CPU 87.0 ms；吞吐 113／60／70／32 題每秒 | 1.9–4.7 秒（已下載） | klab 實測 |
| M1 Mac mini 16GB，多語 CPU | 中位 86 ms、p90 91 ms | 首次 35 秒 | 恆遠實測（廠商） |
| RTX 3070，laya-multilingual，SDK 0.3.3 | 載入後 24–165 ms（含 SDK） | 約 10 秒 | vervecode 實測 |
| 官方 T4 | 英文 39.5 ms／多語 32.8 ms；5 題 84.5／40.1 ms | — | 官方 |
| 官方 CPU | 193–464 ms | — | 官方 |

🔴 **「Mac 單題最快 11 毫秒」的完整條件**：M4 Pro 64GB、多語版、GPU（MPS）、不量化、laya 0.3.20、單封繁中客服信只問 1 題、暖機 5 次後取 50 次的 p50（p95 12.6 ms），
**不含載入 2.9 秒與第一次推論 534 ms**。

🔴 **「比 Jev 快 7 倍」**（恆遠：台北呼叫 Jev 611 ms 對本機 86 ms）比的是本機推論對跨海 API，**不是模型速度**。

**記憶體（klab，0.3.20）**
- 峰值 2.0–4.1GB（權重的 2.9–5.5 倍；fp16 權重載入成 fp32）；兩版同時 4.6GB
- `ps`／`top` 的 RSS 會**低估** GPU 模式，要看 phys_footprint（活動監視器「記憶體」欄）
- 恆遠 M1：環境 922MB＋權重 647MB，常駐 1.5–2GB

**微調成本**
- 官方 notebook：雙 T4 4–6 分鐘
- 恆遠 M1 CPU：150 筆 × 4 epoch 609 秒（作者實測，廠商）

**其他執行環境（版本快照）**
- llama.cpp 10/2 起的支援名單有 Laya（aiposthub 轉述 ggml-org 公告；GGUF 實際形態待核對）
- **沒有 Ollama 版本**（klab 9/25；10/1 Ollama 0.35 名單亦無）→ [[decision-model-ecosystem]] 本機執行矩陣

## C（關鍵行動）

- [ ] 引用 Laya 準確度一律連同 0.362／0.461 與「0.766 是微調版」一起寫
- [ ] 自架前先跑套件安全檢查（PyPI vs GitHub、safetensors、無遙測），並同時固定套件與權重版本
- [ ] laya-serve 一律加 `LAYA_HOST=127.0.0.1` 或 `LAYA_API_KEY`，不要用預設值上線
- [ ] 送進去的 state／選項先自己算 token，避免靜默截短
- [ ] 用自己的資料重設 `answer_confidence` 門檻；繁中以 Choice／Noul 為主，Score 先不用
- [ ] 🔴 **下次 wiki-health-check 優先複查本條「版本快照」區**（Router 預設、laya-serve 預設、截斷限制、執行環境）

## 誠實的成績單（作者小樣本實測，不能當準確率）

- **繁中 221 筆台灣客服訊息零樣本 44.8%**，其中 140 筆被判成同一類（恆遠實測，**廠商**）；同 221 筆用 Jev 成本 0.0054 美元
- klab 12 題 × 三用途（繁中／英文各 12 題）：Choice 48 答對 45；Score 繁中客服緊急度 5/12、RAG 相關度繁中 4/12 vs 英文 12/12；Noul「需使用者確認」繁中 7/12——繁中 Score 幾乎都落在中間值，與官方 SST-5 0.372 一致
- vervecode 19 選 1 文章推薦（18 篇＋`none`），Jev 與 laya-multilingual 同題對照：

| 題目 | Laya | Jev |
|---|---|---|
| TouchDesigner | 96.13% 對 | 99% 對 |
| Astro | 選 none 28.28%，錯 | 99% 對 |
| Unity | 選 none 21.31%，錯 | 99% 對 |
| 義大利麵（應選 none） | none 55.50% | none 100% |

  ⚠️ 前四題參與過輸入調整，作者明說**不能當準確率**。🔑 結論只有一條：**加 `none` 只讓模型可以不選，不保證它知道何時該放棄**。

## 衝突時間軸（並列不擇一，各標日期）

| 項目 | 2026-09-23 恆遠 | 2026-09-25 klab（0.3.20） | 判定 |
|---|---|---|---|
| 有沒有 HTTP API | 沒有，只有 pip 與權重，要自己包 | 有 `laya-serve`，Jev 相容 `POST /v1/systemone`（10/2 aiposthub 亦同） | 版本演進，最新狀態＝有；首次出現版本待核對 |
| Router 預設保留幾個 checkpoint | 只保留一個，要 `preload=True` | 預設最多 2 個，可 `max_loaded=1` | 版本演進 |
| `act_probability` | 可當第二道訊號 | 官方說明沒有參考價值 | 🔴 以官方為準，恆遠建議不採用 |
| 繁中官方數字 | 官方沒公布 | 官方 51 語言測試繁中 0.540 | 時間差，並列 |

## 待核對

1. Laya 以 GGUF 在 llama.cpp 執行的實際形態（BERT 系編碼器能否如公告所述執行）
2. `laya-serve` 第一次出現的版本（可化解 9/23「沒有 API」的時間點）

其餘生態層待核對項目見 [[decision-model-ecosystem]]「待核對」。

## 原始摘要

主幹＝《Laya 評測與使用教學，Jev 的開源替代模型｜Mac 實測單題最快 11 毫秒》（klab，2026-09-25，M4 Pro 實測、安全檢查、laya-serve、移植差異，無業配）；
＋《Laya 模型是什麼？開源決策模型完整解析與自架評估》（恆遠數位科技，2026-09-23，**廠商來源**，取其官方數字前提拆解、M1 實測、繁中 221 筆與微調成本；推廣段落全數排除）；
＋《Jev 與 Laya 決策模型怎麼用？用文章推薦實測輸入、輸出與差異》（vervecode，2026-09-19，靜默截斷、state／criteria 擺放、與 Jev 同題對照）；
llama.cpp 一行引自 aiposthub（2026-10-04）。

## 相關連結

- [[decision-model-ecosystem]] — 總綱：三產品比較、`/v1/systemone` 時間線、本機執行矩陣
- [[jev-implementation-guide]] — 移植對照：Jev 的選項上限、門檻設計、版本固定
- [[jev-system-one-model-concept]] — RLCD 同名方法、校準定義、Jev 數字口徑
- [[llm-judge-confidence-cascade]] — 高信心仍會錯；門檻不能跨模型沿用
- [[rag-refusal-evidence-gate-threshold-calibration]] — 門檻是實驗值，要用自己的校準集選
- [[ollama-2026-complete-guide]] — Ollama 0.35 決策介面（名單沒有 Laya）
- [[agent-tool-authorization-gatekeeper]] — 有副作用的動作由程式決定是否確認
