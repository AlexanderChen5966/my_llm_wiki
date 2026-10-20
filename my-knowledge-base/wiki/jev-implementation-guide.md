---
title: Jev 實作指南 — API、設計模式、限制與取用管道
date: 2026-09-23
tags: [ai/agent, ai/workflow, type/how-to]
source: https://codelove.tw/@tony/post/qJRPLq（原文 https://dev.to/valyuai/how-to-use-jev-a-practical-guide-to-typesafes-system-one-model-g5e）; https://www.aiposthub.com/jev-model-typesafe-ai-beginner-guide/; https://tools.wingzero.tw/articles/4074; https://codelove.tw/@tony/post/a9y7Pa（原文 https://qiita.com/kikuziro/items/2be9091b328d8b844640）; https://www.aiposthub.com/jevgrep-semantic-code-search-ai-agent-cost/; https://rar.design/posts/jev-typesafe-system-one-model-guide; https://vervecode.dev/ai/jev-laya-decision-models/; https://klab.tw/2026/09/laya-review-tutorial/; https://oomol.com/zh-tw/blog/jev-agent-skills/
type: article
status: processed
asset_value: 80
updated: 2026-10-08
---

把 Jev 接進系統時會反覆回查的參考資料：三個原語的參數上限、五個設計模式、官方自陳的弱項清單、
rate limit 與版本固定寫法，以及三條取用管道各自的限制。概念與定位見 [[jev-system-one-model-concept]]。

⚠️ **利益相關揭露（本條目主幹來源）**：資訊密度最高的《如何使用 Jev》由 **Valyu**（一家檢索 API 廠商）撰寫。
其「模式 5：先檢索再判斷」直接帶自家產品置入，本條目引用該模式時已另行標註。
文中引用的官方評測亦為 **TypeSafe 自評**，細節見本文〈誠實的成績單〉一節。

## T（核心論點）

🔑 **整個 API 只有三種問題型別（Choice / Score / Noul），而且這不是待繞開的限制，是它的設計本身。**
會影響架構的兩件事只有：**問題平行評估**（第十個問題增加 token，但幾乎不增加時間）與
**只計輸入、輸出免費**（所以多問幾題、Choice 多列幾個選項幾乎不花錢）。
這兩件事合起來推翻了對 LLM 養成的直覺——不要「先丟便宜請求、必要時再追問」，而是**一次把所有問題問完，再讓程式決定哪些答案有用**。

🔑 **Jev 對你交給它的 state 之外一無所知，而 state 塞太多又會讓準確率下降。**
組 state 的那一層決定了 Jev 被允許知道什麼，也就決定了整條流程的準確率上限。

## B（適用場景）

**適用：** 路由與分流、內容審核、昂貴 context 之前的相關性過濾、對 LLM 輸出評分或設 guardrail、
大量標註、任何 request handler 裡需要亞秒級回應的判斷。

**不適用 / 注意：**
- 生成內容、算術／計數、日期運算、需要寫給稽核看的理由、開放式答案空間
- ⚠️ **中文（CJK）準確度較低**——英文為主要訓練語言（aiposthub）。
  🔴 **不可用英文成果推估中文準確度**，這是該文列的新手五坑之一
- ⚠️ 只接受**文字輸入**，無圖片／音訊／影片
- ⚠️ 官方**不提供客製訓練**——要讓它懂你的領域，就把規則寫進題目（rar.design，2026-09-29 補）

### 新手五坑（aiposthub）

1. 問題沒拆小，把好幾個判斷藏在同一題裡
2. Choice 選項沒有 `other` / `none_of_the_above`，逼模型硬選一個最接近但錯的
   - （2026-10-08 補，實驗註）**加 `none` 只讓模型可以不選，不保證它知道何時該放棄**：vervecode 19 選 1 文章推薦實驗中，Laya 在 Astro、Unity 兩題誤選 none，Jev 同題正確（細節見 [[laya-open-source-decision-model]] 成績單）。⚠️ 該實驗前四題參與過輸入調整，作者明說不能當準確率。另記：把題目中的 VS Code 換成 Cursor 時，Jev 的機率降到 78%、none 18%
3. 用模糊形容詞當評分標準
4. 一開始就讓它觸發不可逆動作
5. 用英文成果推估中文準確度

## R（操作步驟）

### 1. 安裝與取得金鑰

```bash
export TYPESAFE_API_KEY="sk-..."

pip install typesafe-sdk          # Python 3.10+
npm install @typesafe-ai/sdk      # Node 20+
```

兩個 SDK 都從環境變數讀 `TYPESAFE_API_KEY`，並預設使用 `jev-latest`。
直接呼叫時**唯一的端點**是 `POST https://api.typesafe.ai/v1/systemone`。

coding agent 的 skill 安裝（三個變體，是不同 agent／範圍，不是互相矛盾的說法）：

```bash
# Claude Code（plugin marketplace）
claude plugin marketplace add typesafe-ai/skills
claude plugin install typesafe@typesafe-ai

# 其他 agent（專案層）
npx skills add typesafe-ai/skills --skill typesafe-ai

# Codex 全域（-g＝全域，安裝後所有專案通用）
npx skills add typesafe-ai/skills --skill typesafe-ai --agent codex -g
```

🔴 **API Key 保管**（wingzero 的 Codex 備忘）：key 在 <https://console.typesafe.ai/home> 左側 API Keys 建立；
若要交給 agent，存成獨立檔案（例如 `jev-api-key.txt`）並**明確要求不得寫入程式碼、AGENTS.md 或提交 Git**。

### 2. 三個原語的參數上限

| 原語 | 回傳 | 上限／規則 |
|---|---|---|
| **Choice** | `.choice`、`.probabilities`（每選項一個）、`.confidence` | **最多 255 個選項**；每個選項只多吃一點 token，所以直接傳完整清單，別只傳候選；建議明確加一個 `other` |
| **Score** | `.score`（**可落在等級之間**，如 `1.035`）、`.probabilities`、`.confidence` | **2 至 10 個有序等級**，以文字描述；等級索引由**陣列順序**決定（level 0 ＝第一項）|
| **Noul** | `.noul`（0–1，答 yes 的機率）| **沒有 confidence 欄位**——這個數字本身就是信念程度 |

### 3. 五個設計模式

**模式 1：投機式展開** — 一次把所有問題問完，包含「只有在某個分支成立時才有意義」的題目也照問，
再用程式判斷哪些答案要用。TypeSafe cookbook 以一篇長 Wikipedia 文做 13 個問題，
**打包成一次呼叫比逐題問便宜 12.2 倍、快 10.0 倍，答案完全相同**（TypeSafe 自家 cookbook 數據）。
（2026-09-29 補：RAG 情境的實證見第 6 節——每個 chunk 一題 score＋一題 noul 共 40 題合併成一次請求。）

**模式 2：以信心值為門檻的路由** — 🔴 **不要替整個系統寫一個統一門檻**；每個動作各有門檻，依出錯代價調整。
範例：`confidence < 0.5` 一律轉人工；唯讀操作（查餘額）門檻低；會動到錢的操作（核准轉帳）要 `> 0.85`，否則回頭問使用者。
補充：若 confidence 不是你要的指標，可直接讀原始 `.probabilities`；
**分布很平均通常代表你的 criteria 有問題，而不是模型混亂**。
（2026-09-29 補）rar.design 引官方語音銀行例：<0.6 轉真人、查餘額 ≥0.6 直接顯示、轉帳 0.6–0.85 先確認、>0.85 自動執行；與上面 dev.to 的 <0.5 範例不同——門檻本來就依動作與來源而異，兩例並列。
judge 任務的外部評測與門檻風險見 [[llm-judge-confidence-cascade]]。

**模式 3：組合式評分** — 把模糊判斷拆成彼此獨立的維度各自評分，再用**你自己控制的權重**組合，
而不是直接問「這個候選人有多好」（那是把好幾個判斷包成一個答案）。好處是**重新加權只要改程式碼，不必重寫 prompt**，而且可以 A/B test。

**模式 4：級聯流程** — 🔑 **Jev 不是 Opus 5 或 GPT-5.6 的替代品，它是用來決定哪些請求值得交給它們的那個東西。**
一條分支完全不碰模型（純查詢）、兩條分支叫不同專家 LLM、一條升級轉人工。
TypeSafe 以一百萬張客服票估算：**約 $6,480 而非 $30,400**，其中約 80 萬筆可在半秒內完成而非十秒。
⚠️ 此為 TypeSafe 的單筆成本推算，非實際部署實測。
（2026-09-29 補）judge 任務的外部評測與門檻風險見 [[llm-judge-confidence-cascade]]。

**模式 5：先檢索，再判斷** — ⚠️ **本模式由 Valyu（檢索 API 廠商）提出，程式碼範例直接使用其自家 `valyu` 套件，引用時請留意利益相關。**
去掉產品置入後，剩下的論點仍成立且有官方依據：jaggedness 頁面明講「先在程式裡做 retrieve 與 filter，只把問題需要的欄位送進去」，
因為**塞太多材料會因 context 汙染拉低準確率**；而只餵弱來源時，Jev 仍會對那份差材料做出「校準良好但沒有價值」的判斷。
所以完整模式是兩層：**精準擷取，再便宜判斷**——用哪家檢索則與模式本身無關。
（2026-09-29 補：實證見第 6 節。）

### 4. jaggedness：官方自陳的弱項（上線前必讀）

來源：`docs.typesafe.ai/model-jaggedness/jev-1.13`。作者評語是「對一次產品發布來說意外地誠實」。

- **會照字面理解** — 它回答的是你寫出的問題，不是你想問的那個；否定、範圍詞、隱含條件都照表面接收。
  🔑 判斷法：看到錯答案時你忍不住想解釋「我其實是要問…」，**那段解釋就是你指令中缺的一半**
- **不是計算機** — 計數不可靠且錯誤率隨規模上升；請在程式裡迭代，每個項目問一個 Noul，再由程式加總
- **日期只是文字，不是有序量** — 先後、相差多久、是否落在區間內都不可靠；擷取本身是判斷（用列舉式 Choice ＋ 明確的「未提及」選項），組合與排序留在程式裡
- **context 汙染是真的** — 先檢索與過濾再組 state
- **state 不會被視為惡意輸入** — 若 state 內文字本身意在操控分類結果，那是你的威脅模型要處理的事（見 [[rag-security-prompt-injection-four-layer-defense]]）
- **互相矛盾的 instructions 與 criteria 會讓它混亂** — 把 criteria 當成 instructions 的延伸
- **它不會生成** — 要從自由文字抽值，先用 regex 或生成模型找候選，再讓 Jev 選

（2026-09-29 補：官方頁面實際列 **9 項**（最後檢視日 2026-09-17，rar.design 補充），以下兩項為先前缺漏）

- **拐彎的問題** — 雙重否定、多層間接問法，準確度會下降
- **讀語意比讀數字好** — 用 hex／RGB 問顏色接不接近答不準，換成顏色名稱比較好

> 官方的元規則（普遍適用的好設計建議）：**避免問模型那些程式可以精確計算的事；避免把好幾個判斷藏在同一個問題裡。**

（2026-09-29 補）官方評測圖中，每個模型「拆成小題的工作流」都比自己的「單一大 prompt」更準、更便宜、更快——拆題對任何模型都有效，不是 Jev 特有（rar.design）。

（2026-10-08 補）輸入擺放的一個觀察：vervecode 把文章描述從 state 移到 criteria、並縮成短英文主題後結果改善，但**兩個變因同時改，無法歸因**是哪一個有效。

### 5. 營運參數

- **Context 上限（兩個不同層級，非矛盾）**
  - **官方 API（jev-1.13）**：state ＋單一最長問題 **32K token**；state ＋所有問題合計 **64K token**（dev.to、aiposthub 一致）
  - **Cloudflare Workers AI 管道**：context 長度 **32,000 token**（qiita/yushibats，該管道自身的限制）
  - 🔴 兩者是**不同層級的限制**（模型 API vs 第三方托管管道），不可互相覆蓋或寫成衝突
  - （2026-10-08 補）klab（2026-09-25）記 Jev 每次 request 64k，與上列官方 API 合計值一致；Cloudflare 管道 32,000 是不同管道，並列，不改舊值
- **Rate limit（jev-1.13）**：每秒 250,000 tokens、每分鐘 1,200 requests，超過任一上限回 `429`。
  兩個 SDK 都以 exponential backoff 重試並遵守 `retry-after`。⚠️ TypeSafe 提醒這些限制會在 GPU 容量到位前**不預告調整**
- 🔴 **調過門檻就要固定版本**：`jev-latest` 目前指向 `jev-1.13.0`，新版釋出時會變動，可能讓答案跟著改。
  回應的 `model` 欄位會回報實際作答的版本 ID，**請記錄它**

```python
client = TypeSafeClient(model="jev-1.13.0")   # 固定版本
```

- **計費只算輸入**，輸出免費——這是投機式展開與長選項清單便宜的根本原因
- 官方表示一般客戶請求不用於訓練（aiposthub 轉述）

### 6. 實證：RAG 的三個插入點（kikuziro，Qiita，codelove 2026-09-23 轉載）

> ⚠️ 單一作者自測、自製虛構公司內規語料（5 領域 × 8 文件 × 10 chunk＝400 chunk），日文原文經中文翻譯轉載；Qiita 原文日期不明，驗證時間為 2026/09。

**實驗設計**：問題分可回答 60／接近命中 40（話題對上但答案不在 chunk 裡）／完全無關 20 題；閾值先在 40 題 dev set 決定，再打開測試集。環境 `jev-1.13.0`（typesafe-sdk 0.7.0）、Claude Haiku 4.5、Cohere Rerank 3.5、Amazon Rerank 1.0（Bedrock，東京區）。

| 插入點（每千查詢） | Jev | 對照 | 備註 |
|------|------|------|------|
| 範圍攔截（搜尋前，choice：in_scope／out_of_scope／unclear） | 非目標題 241ms／$0.044 | 不攔：搜尋→重排→LLM 拒答 1,794ms／$3.28 | 約 1/75 成本 |
| 拒答判定（呼叫 LLM 前） | 99.2%，接近命中 0 漏判、誤攔 1.7%；+263ms／+$0.05 | Haiku 4.5 98.3%／+584ms／+$1.04；Cohere 分數閾值 65.0%；Amazon 閾值 65.8% | 無法回答題 1,459ms→263ms、$1.37→$0.05（約 1/27） |
| 重新排序 | $0.25／290ms | Cohere $2.00／198ms；Haiku $2.73／981ms | 🔴 **精度比較無效，只看價格**（見下） |
| 合併請求（每 chunk 一題 score＋一題 noul，共 40 題；noul 最高值 <0.755 判無法回答） | 292ms／$0.34，判定 99.2% | reranker＋Jev 兩段式 468ms／$2.05 | 實證模式 1 |

- 🔴 **重新排序的精度比較不能當結論**：不排序時 Recall@5 已經是 1.000，reranker 等於沒派上用場，作者自己說要用更大語料重測。
- 🔴 **翻譯錯誤更正**：轉載譯文「速度是 Jev 的 2 倍、價格只有 1/20」應理解為 **Jev 比 Haiku 快約 2 倍、價格約 1/20**（263 vs 584ms、$0.05 vs $1.04）；同一句中的「0.983」指哪個指標不明，本庫不引用這句。
- **關鍵結論**：越早攔下越省；每 1,000 題擋下 37 題（3.7%）Jev 的判定費用就回本；合法題目會多一點延遲與成本，要先用真實數據估算無法回答題的比例。
- **範圍 criteria 要逐條寫**（呼應 jaggedness「照字面理解」）：只寫 5 個領域名稱時 34 題誤擋 5 題，把處理項目逐條寫出後誤擋 0 題；漏擋的「寫 Python 備份腳本」是被範圍描述中的「資料備份」誤導；5 題續接式發話（「請詳細說明」）全被判為 unclear → 需要先做多輪改寫或帶入對話歷史，見 [[rag-multi-turn-followup-resolver-state]]。
- 作者列的延伸（**未驗證**）：以 Jev 信心值分流 Sonnet／Haiku、與 Amazon Bedrock Guardrails 比較黑名單式護欄。
- 分數閾值 vs 判斷模型的對照另見 [[rag-refusal-evidence-gate-threshold-calibration]]。

### 7. Jev 相容端點與自架替代（2026-10-08 補）

- Laya 的 `laya-serve`（0.3.20）提供同格式的 `POST /v1/systemone`，現有 Jev client 改 base URL 即可呼叫——但 🔴 **門檻、選項上限（約 20 vs 255）、Score 每級必須有描述，三者都不能沿用** → [[laya-open-source-decision-model]]
- 🔴 **Ollama、llama.cpp 都跑不了 Jev 本體**（閉源）；它們的 `/v1/systemone` 跑的是其他開放權重模型
- 生態全貌（三產品比較、`/v1/systemone` 時間線、本機執行矩陣）→ [[decision-model-ecosystem]]

## C（關鍵行動）

- [ ] 第一次接時直接固定 `jev-1.13.0`，不要用 `jev-latest` 上正式環境
- [ ] 把「每個動作各自的 confidence 門檻」寫成設定表，而非散落在程式碼各處
- [ ] 上線前把 jaggedness 九項逐條對照自己的使用情境勾一次
- [ ] 中文場景另跑一份自己的評測，不沿用任何英文數據
- [ ] 記錄回應中的 `model` 欄位進 log，未來門檻失準時才有辦法回溯

## 誠實的成績單（TypeSafe 自評，⚠️ 尚未第三方驗證）

在 TypeSafe 的四個工作流程評估中：Jev **67.8%**、GPT-5.6 Terra 67.9%、Sol 74.1%、Opus 5 73.1%；
最刺眼的一句是 **Claude Sonnet 5 也剛好 67.8%，但每案成本高 293 倍、延遲高 195 倍**。
（2026-09-29 補原始值：每案 Jev $0.0004／0.4 秒 vs Sonnet 5 $0.1174／78.1 秒，官方評測站，rar.design 引。）

🔴 依 `rules/research-protocol.md` 對廠商自評的處理，四個限定條件必須連同數字一起記：

1. **那一欄不是 accuracy** — **沒有 ground truth**。TypeSafe 取 GPT-6 Astra 與 Claude Fable 5.1 高思考模式的結果建立 **consensus labels** 再比對，
   量的是「與兩個前沿模型的共識程度」（所以這兩個模型本身不在榜上）。TypeSafe 自承這會**偏向 OpenAI 與 Anthropic**
2. **自家跑的結果** — 工作流程、測試框架、執行全由 TypeSafe 自己來，**無獨立重現**；請用自己的流量評估
3. **「不會幻覺」比聽起來更狹義** — 0% 是**宣稱不是實測**；45.5% 的對照只是離群值（Haiku 4.5），多數模型介於 0.58%–13.2%
4. **價格可能會變** — TypeSafe 無法證明目前沒有補貼，僅表示預期價格會下降而非上升

## 取用管道與其時間軸（🔴 本批唯一的實質衝突，以日期並列而非二選一）

四篇來源對「怎麼拿到存取權」的說法不同。逐一比對日期後，判定為**開放程度在 9/18→9/21 之間改變**，
而不是任何一篇寫錯——因此一律以時間標註保存：

| 日期 | 來源 | 當時的說法 |
|---|---|---|
| 截至 2026-09-18 | qiita/yushibats | 官方 API 需**註冊候補名單等邀請**；Vercel AI Gateway 自 9/16 起以 `typesafe-ai/jev` 提供（9/17 時不含免費額度，**儲值後可免等邀請**）；Cloudflare Workers AI 以 `typesafe/jev` 提供，context 32,000 token |
| 2026-09-18 | OOMOL | Cloudflare 模型目錄已列 Jev（2026-10-08 補）|
| 2026-09-19 | aiposthub | 折衷描述：仍處早期開放階段，**額度以控制台顯示為準** |
| ≤ 2026-09-19（申請日不明） | vervecode | 以 **Email 申請 waitlist，約一天通過**，註冊得 **5 美元**試用額度（2026-10-08 補）|
| 2026-09-20 | dev.to/valyu | 金鑰於 console 取得，**早鳥存取需要排隊等候名單（waitlist）**，或改從 Vercel AI Gateway 取得 |
| 2026-09-21 | 數位時代 | typesafe.ai **註冊免費帳號、不需綁信用卡、直接送 5 美元測試額度**（約新台幣 159 元）|
| 2026-09-21 01:19 | rar.design | 仍需**排候補名單**才能使用（2026-09-29 補）|
| 2026-09-25 | klab | 執行位置：TypeSafe 雲端、Vercel AI Gateway、Cloudflare Workers AI（2026-10-08 補）|
| 約 2026-09-26 | jevgrep 專案文件（aiposthub 9/29 轉述） | `jg auth` 可選的 provider 有 Vercel AI Gateway、TypeSafe、**OpenRouter、OpenCode Zen**（2026-09-29 補，新增後兩個管道）|

同一天（9/21）兩種說法：可能是當天政策改變，也可能是 rar.design 沿用較早的資訊，無法判定，兩者並列。
（2026-10-08 補，**僅為推論**）vervecode 的經驗是「waitlist 約一天通過、註冊得 $5」——waitlist 與 $5 可能是同一流程的兩步（排隊通過後送額度），若成立可化解 9/21 的兩種說法；未經證實，上方並列照舊保留。

⚠️ 因此「要不要排隊」取決於你何時去申請；**本條目日期為 2026-09-23，最新可信狀態是 9/21 的免綁卡註冊送 $5**（2026-09-29 補：9/26 前後已可經 OpenRouter、OpenCode Zen 取用），
但這類早期存取政策變動快，實際以 <https://console.typesafe.ai> 顯示為準。
另外，發表當日（9/15）開發者湧入曾一度使官方 API 斷線（數位時代）。

## 社群實作案例（上線前 48 小時，dev.to 整理）

這些案例共同的模式是：**把迴圈、安全性與算術留在一般程式碼裡，讓 Jev 只處理中間那段難以用程式表達的狹窄判斷。**

- **1kpapers.com**（Hassan El Mghari）：1,018 篇論文，DeepSeek V4 Flash 摘要 $3.99 ＋ Jev 分類 **$0.08**，中位延遲 256ms
- **browser-use/jev-ultrafast**（641★）：Google Flights 蘇黎世→倫敦，**7.1 秒 / $0.0039**
- **awlevin/typesafe-computer-use**：每決策 **$0.0002 vs Opus 5 的 $0.032**；12 步任務 $0.003 vs $0.40–$0.90；模型延遲 0.13–0.38s vs 5.2s
- **jarrodwatts/jev-trader**：Monad 每區塊（約 300ms）一次決策，模型延遲約 **81ms**，hot loop 只做兩次 RPC 往返；附 dry-run 模式
- **RomanSlack/jev-drone**：2.5Hz 戰術判斷，**僅供建議**
- **TheoLeeCJ/openjev**（166★）：在 4B 開源模型的 logits 上一次前向傳播讀出帶型別的選項機率，
  ⚠️ 作者明確聲明重現的是**介面模式**，不是 Jev 的模型或訓練方法
  （2026-10-08 補）llama.cpp 10/2 支援名單中的 **OpenJev**（CC BY-NC 4.0，限制商業使用）是否即此專案，**待核對**（見 [[decision-model-ecosystem]]）
- 其他：fhshaik/typesafe-mario（73★）、devagrawal09/jev-review（48★）、phyous/tsai-sc（星海爭霸 421 次決策）、AbdelStark/awesome-typesafe
- **jevgrep**（指令 `jg`，2026-09-26 發布，aiposthub 2026-09-29 整理）：David Zhang 的**社群開源專案**（npm `@dzhng/jevgrep`，個人 scope），**不是 TypeSafe 官方工具**；文中「官方 README／官方 skill」指的是該專案自己的。用 Jev 從資料夾、檔案、宣告一層層縮小範圍，讓 Coding Agent 用自然語言找程式碼
  - 🔴 「省 40%」要連條件一起看：專案自測 10 題經調整的 Python SWE-bench 任務（Sol 模型），第一輪成本 $7.62→$4.52（約 −40%），但解題率從 8/10 **降到 7/10**；9/28 那一輪雙方解題率都是 8/10，只省 **29.97%**（token −41.51%、wall time −12.70%）；**兩輪都沒有算入 Jev 本身的 API 費用**（原生回應沒有價格 metadata，是「未知」，不是零）
  - 用法：Node.js 22+、macOS／Linux；`npm install -g @dzhng/jevgrep` → `jg auth` → `jg skill`（CLI 與 skill 要分開更新）
  - 風險：符合條件的原始碼會送到選定的 provider，ignore 過濾不保證擋掉所有敏感資訊
  - 作者建議：已知符號用 `rg`、只知道行為用 `jg`、要完整找引用用 LSP（見 [[claude-code-large-codebase]]、[[codebase-memory-mcp]]）

### 另一批社群專案（OOMOL 整理，2026-09-18；2026-10-08 補）

⚠️ OOMOL 整理、**作者未重現**，文中示例結果**並非實際呼叫回傳值**。與上方 dev.to 清單沒有重複。

- **wustep/jev-playground**：Jev 選音樂參數 → 程式生成音符／MIDI；有不需 key 的啟發式離線模式
- **AbdelStark/heist-one**：遊戲守衛的威脅判斷，預設為腳本模式
- **monteduro/killmyidea**：10 題 → 加權得出 KILL／FIX／SHIP（點子評審）；預設在服務端歸檔
- **GodsBoy/jev-agent-skill-router**：回傳三類——選某 Skill／無需 Skill／需要覆核（Skill 路由三出口，見 [[agent-skill-design-patterns]]）
- **reachjalil/jevlogs**：日誌的診斷價值／優先順序／路由，附 npm 與 SKILL.md
- 官方 Skill **typesafe-ai/skills**（安裝方式見第 1 節）、社群 **rashedInt32/jev-mcp**

## 相關連結

- [[jev-system-one-model-concept]] — 同主題的概念面：System One 定位、與 LLM／Structured Output／分類模型／Agent 框架的分界、數字口徑
- [[rag-refusal-evidence-gate-threshold-calibration]] — 門檻值是實驗值不是真理，設 confidence 門檻前的必要警示
- [[rag-security-prompt-injection-four-layer-defense]] — 對應 jaggedness 的「state 不會被視為惡意輸入」
- [[llm-cost-optimization-strategies]] — 級聯流程在成本策略中的位置
- [[opensquilla-intelligent-model-router]] — 另一條分層路由實作路線
- [[agent-reliability-timeout-retry-fallback-verification]] — rate limit / 429 / backoff 的通用可靠性機制
- [[llm-judge-confidence-cascade]] — 模式 2／模式 4 在 judge 任務上的外部評測（CMU，原論文待查）：門檻不能跨任務沿用
- [[rag-multi-turn-followup-resolver-state]] — 第 6 節範圍攔截把續接式發話判為 unclear，需先做多輪改寫
- [[claude-code-large-codebase]] — jevgrep 的使用情境：Agent 在大型陌生程式庫找程式碼
- [[codebase-memory-mcp]] — 另一種讓 Agent 找程式碼的做法，可與 jevgrep 對照
- [[decision-model-ecosystem]] — 決策模型生態總綱：Jev／Laya／Decisions API 比較、`/v1/systemone` 時間線（2026-10-08 補）
- [[laya-open-source-decision-model]] — 自架替代：laya-serve 相容端點與移植差異（2026-10-08 補）
- [[agent-skill-design-patterns]] — Skill 路由型；GodsBoy/jev-agent-skill-router 三出口（2026-10-08 補）

## 原始摘要

主幹＝《如何使用 Jev：TypeSafe 的 System One 模型實用指南》（dev.to/valyuai，2026-09-20，資訊密度最高，
唯一提供 255 選項上限、Score 2–10 級、rate limit、版本對應、五大設計模式與社群專案清單）；
＋《Jev 模型是什麼？新手完整教學》（aiposthub，2026-09-19，補首次呼叫範例、Python 3.10+、CJK 警告、新手五坑、
「零型別幻覺 ≠ 零語意錯誤」的精準措辭）；
併入小節＝《在 Codex 全域安裝並使用 Jev 的方法》（wingzero，2026-09-21，2KB 操作備忘，取其 `--agent codex -g` 變體與 API Key 保管提醒）。

2026-09-29 補充來源分工：《驗證看看是否能透過 Jev 實現 RAG 搜尋的極速化與成本削減》（kikuziro／Qiita，codelove 2026-09-23 轉載）→ 第 6 節 RAG 三個插入點實證；《jevgrep 是什麼？》（aiposthub，2026-09-29）→ 社群實作案例與取用時間軸的 OpenRouter／OpenCode Zen；《只會答選擇題的 AI 有什麼用？》（rar.design，2026-09-21）→ jaggedness 補到 9 項、拆題通用結論、不提供客製訓練、評測原始值、語音銀行門檻例與 9/21 候補名單說法。

2026-10-08 補充來源分工：《Jev 與 Laya 決策模型怎麼用？》（vervecode，2026-09-19）→ none 實驗註、輸入擺放、waitlist 資料點；《Laya 評測與使用教學》（klab，2026-09-25）→ 9/25 執行位置、64k、相容端點；《讓 AI 學會做選擇：Jev 帶來的 5 個 Agent Skill 靈感》（OOMOL，2026-09-18，廠商部落格，Provider／Leina 推廣未收）→ 另一批社群專案、Cloudflare 目錄資料點；OOMOL 提到的只接受文字、問題平行評估、英文最準、jaggedness 弱項本條既有，未重複寫入。
