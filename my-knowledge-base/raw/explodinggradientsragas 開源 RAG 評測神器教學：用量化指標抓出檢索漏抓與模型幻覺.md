---
title: "explodinggradients/ragas 開源 RAG 評測神器教學：用量化指標抓出檢索漏抓與模型幻覺"
source: "https://www.aiposthub.com/explodinggradients-ragas-rag-evaluation-metrics-guide/"
author:
  - "[[Philo]]"
published: 2026-11-18
created: 2026-11-19
description: "explodinggradients/ragas 是開源界評估 RAG 系統準確度的標準工具，能自動量化忠實度、答案相關性與檢索召回率，一篇教你建立自動化 RAG 評測管線。"
tags:
  - "clippings"
status: "inbox"
---
每個團隊剛把檢索增強生成系統上線時，都會遇到一個極度尷尬的困境。使用者回報回答怪怪的，工程師自己測了五題覺得沒問題，但老闆問「改完切塊大小後到底有沒有變準」，現場卻沒有人拿得出客觀數字。

靠人工肉眼抽查幾十題不僅耗時，更無法在每次修改提示詞、更換向量資料庫或升級模型時做回歸測試。開源專案 explodinggradients/ragas 目前官方倉庫已更名為 vibrantlabsai/ragas，正是為了解決這套「憑感覺調參」的惡性循環而成為業界標準。

Ragas 把黑箱般的問答品質拆成可量化的數學指標，讓你一眼看穿問題究竟出在「資料庫沒撈到正確段落」，還是「模型拿到資料卻自己瞎編幻覺」。

這篇 Ragas RAG 評測教學會帶你認識核心設計架構、安裝與快速範本指令，並實際撰寫評估忠實度與檢索召回率的 Python 管線，同時揭露自動化評測在隱私與成本上的實務限制。

## 為什麼不能只靠人工抽查：Ragas 的核心評測邏輯

檢索增強生成系統由兩個截然不同的模組組成：負責從知識庫撈取相關文件片段的檢索器，以及負責閱讀片段並組織語言回答的生成模型。當系統答錯時，如果只看最後生成的答案給一個總分，工程師根本不知道該修向量檢索邏輯，還是該改大型語言模型的提示詞。

Ragas 的核心突破在於「元件級拆解與無參考答案評估」。它把評估流程拆成檢索品質與生成品質兩大維度，並利用強大的裁判模型作為自動化評分員，將主觀的文字表現轉化為介於零到一之間的客觀分數。

- **檢索漏抓診斷** ：透過比對撈出來的文本片段與標準答案，量化系統是否成功找齊回答問題所需的全部關鍵事實，抓出切塊過碎或嵌入向量失準的問題。
- **模型幻覺抓漏** ：逐句拆解模型生成的答案，檢查每一個陳述是否都能從檢索到的原文片段推論出來，一旦模型偷渡訓練記憶中的錯誤資訊或無中生有，分數就會立刻下降。
- **自訂維度評估** ：除了傳統的檢索指標，Ragas 也提供 `DiscreteMetric` 等彈性介面，讓開發者用自然語言提示詞定義「摘要是否準確」、「語氣是否符合客服規範」等客製化評分規則。
- **測試集自動生成** ：對於剛起步、還沒有累積大量人工標註問答對的團隊，Ragas 也能直接讀取原始文件自動合成多樣化的測試題庫，加速建立第一版基準線。

理解這套拆解邏輯後，團隊每次調整參數就能像跑單元測試一樣，在幾分鐘內產出完整的效能對照表，不再需要開會爭論哪個版本的回答比較順眼。

![Ragas官方 GitHub 專案封面圖：Ragas RAG 評測教學](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/explodinggradients-ragas-rag-evaluation-metrics-guide-official.png)

Ragas官方開源專案視覺圖。 圖片來源： 官方原文 。

## 安裝步驟與 quickstart 範本快速起步

Ragas 是純 Python 生態系的評估框架，安裝方式非常直接，可以直接從 PyPI 取得穩定版本，或是從官方 GitHub 倉庫安裝最新原始碼。

```bash
# 從 PyPI 安裝穩定版本
pip install ragas

# 或從 GitHub 原始碼安裝最新開發版本
pip install git+https://github.com/vibrantlabsai/ragas
```

為了讓新手不必從零摸索專案目錄結構，Ragas 內建了 `ragas quickstart` 命令列工具。執行後可以查看目前支援的評估專案範本，並一鍵複製完整的 RAG 評估骨架到指定資料夾。

```bash
# 列出目前可用的評估專案範本
ragas quickstart

# 建立一個標準的 RAG 評估專案到指定目錄
ragas quickstart rag_eval -o ./my-rag-eval
```

執行程式碼前，請務必在系統環境變數中設定 `OPENAI_API_KEY`，因為多數預設評測指標需要呼叫大型語言模型擔任裁判。如果你在意企業內部測試環境的遙測紀錄，Ragas 官方在文件中明確說明系統預設會收集匿名的基本使用統計，只要在執行環境設定環境變數 `RAGAS_DO_NOT_TRACK=true`，就能徹底關閉匿名遙測功能。

## 核心指標實戰：看懂 Faithfulness 與 Context Recall

在所有 Ragas 指標中，工程團隊上線前最該盯緊的兩個核心數字，就是用來抓幻覺的忠實度 Faithfulness，以及用來抓漏網之魚的上下文召回率 Context Recall。

### 忠實度 Faithfulness：抓出模型偷渡的幻覺

忠實度專門檢驗「模型有沒有照著給它的資料講話」。運作原理分為兩步：裁判模型會先把系統生成的完整回答拆解成一條條獨立的事實陳述，接著逐一比對這些陳述能否從檢索到的上下文中推論得出。計算公式是「有上下文依據的陳述數量」除以「回答中的總陳述數量」。這個指標不需要標準答案就能計算，非常適合用來監控線上真實流量的幻覺比例。

### 上下文召回率 Context Recall：抓出檢索漏掉的拼圖

上下文召回率則用來檢驗檢索器到底有沒有盡責。它需要團隊提供一份人工核對過的標準參考答案，裁判模型會把這份標準答案拆解成核心事實清單，然後檢查知識庫撈出來的段落裡，究竟涵蓋了多少比例的關鍵事實。如果某個問題的 Context Recall 偏低，代表不管後端換上多聰明的生成模型都救不回來，因為正確答案根本沒被撈進提示詞裡。

下面這段完整的 Python 實戰程式，示範如何用 `EvaluationDataset` 整理測試樣本，並同時計算這兩項關鍵指標。

```python
from langchain_openai import ChatOpenAI
from ragas import EvaluationDataset, evaluate
from ragas.llms import LangchainLLMWrapper
from ragas.metrics import Faithfulness, LLMContextRecall

# 1. 設定擔任評分裁判的語言模型
evaluator_llm = LangchainLLMWrapper(ChatOpenAI(model="gpt-4o"))

# 2. 實例化忠實度與上下文召回率指標
faithfulness_metric = Faithfulness(llm=evaluator_llm)
context_recall_metric = LLMContextRecall(llm=evaluator_llm)

# 3. 整理 RAG 系統產出的測試資料集
eval_dataset = EvaluationDataset.from_list([
    {
        "question": "Ragas 如何關閉匿名使用統計追蹤？",
        "answer": "只要將環境變數 RAGAS_DO_NOT_TRACK 設為 true 即可關閉，另外建議每週重啟伺服器。",
        "contexts": ["若要退出匿名統計收集，請將 RAGAS_DO_NOT_TRACK 環境變數設為 true。"],
        "reference": "設定環境變數 RAGAS_DO_NOT_TRACK 為 true 即可退出匿名統計。"
    },
    {
        "question": "如何透過指令建立 Ragas 範例專案？",
        "answer": "可以執行 ragas quickstart rag_eval 建立專案。",
        "contexts": ["使用 ragas quickstart rag_eval -o ./my-project 可快速建立 RAG 評估專案。"],
        "reference": "執行 ragas quickstart rag_eval 指令即可建立範本專案。"
    }
])

# 4. 執行自動化評測並轉為表格檢視
result = evaluate(
    dataset=eval_dataset,
    metrics=[faithfulness_metric, context_recall_metric]
)

df = result.to_pandas()
print(df)
```

仔細觀察第一筆測試資料，模型擅自加上了「建議每週重啟伺服器」這句原文完全沒提到的建議，忠實度指標就會精準扣分，提醒工程師模型發生了幻覺外溢。

延伸閱讀： [Anthropic AI 安全評估 10 項建議：模型更安全的分數怎麼來？](https://www.aiposthub.com/anthropic-safety-evaluation-design-recommendations/)

## 進階客製化指標：使用 DiscreteMetric 檢查特定規範

除了標準的檢索與忠實度分析，實務上產品經理常常會提出非常具體的驗收條件，例如「摘要不能漏掉產品型號」或「回答結尾不能出現承諾退款的字眼」。這時可以直接使用 Ragas 提供的 `DiscreteMetric` 搭配 `llm_factory` 非同步介面，快速打造專屬業務邏輯的自動化守門員。

```python
import asyncio
from openai import AsyncOpenAI
from ragas.llms import llm_factory
from ragas.metrics import DiscreteMetric

async def main() -> None:
    client = AsyncOpenAI()
    llm = llm_factory("gpt-4o", client=client)

    metric = DiscreteMetric(
        name="summary_accuracy",
        allowed_values=["accurate", "inaccurate"],
        prompt="""請評估以下摘要是否準確且完整保留關鍵資訊。

回應內容：{response}

請只回答 'accurate' 或 'inaccurate'。"""
    )

    score = await metric.ascore(
        llm=llm,
        response="本系統支援透過 pip install ragas 進行安裝，並提供多種自動化評估指標。"
    )
    print(f"評分結果：{score.value}")
    print(f"判斷理由：{score.reason}")

if __name__ == "__main__":
    asyncio.run(main())
```

透過 `score.reason`，開發者不只會拿到冷冰冰的及格或不及格標籤，還能直接看到裁判模型給出的文字推論依據，方便後續微調系統提示詞。

## Ragas 與同類評測工具的實務對比

在開源評估生態中，Ragas 經常被拿來與通用型提示詞測試工具以及可觀測性平台比較。以下整理三種常見工具的定位差異，幫助團隊避免買錯工具或重複造輪子。

| 評估維度 | Ragas | 通用提示詞矩陣測試工具 | 企業級 LLM 可觀測平台 |
| --- | --- | --- | --- |
| 核心專長領域 | 專精 RAG 檢索品質與生成幻覺拆解 | 適合跨模型比較提示詞與紅隊安全測試 | 專注線上請求追蹤、延遲監控與成本統計 |
| 指標細緻度 | 提供 Faithfulness、Context Recall 等元件級指標 | 偏重字串比對、正規表達式與自訂斷言 | 多數內建基礎評分，複雜 RAG 指標仍常整合 Ragas |
| 測試資料生成 | 內建從原始文件自動合成測試集功能 | 需自行準備測試案例清單 | 偏重從真實線上流量收集並標註樣本 |
| 導入門檻 | 輕量級 Python 套件，本機即可執行 | 需撰寫 YAML 或設定檔管理測試矩陣 | 通常需架設後端資料庫或註冊雲端服務 |

最理想的工程實踐並不是三選一，而是把 Ragas 當成持續整合流程中的自動化品質閘門。每當知識庫文件更新或調整切塊策略時，先在本地或測試環境跑一次 Ragas 基準測試，分數達標才允許部署上線，上線後再交給可觀測平台持續監控延遲與例外錯誤。

延伸閱讀： [Google TPU Sync 開源：AI 等第一個字，為何先要搬好 KV 快取？](https://www.aiposthub.com/google-tpu-sync-kv-cache-transfer/)

## 評測管線的成本、偏差與資安邊界

把 Ragas 接進自動化測試流程前，團隊必須先認清「用 AI 評測 AI」背後的三大現實代價。

首先是 API 呼叫成本與執行時間。因為忠實度與召回率都需要把長文本拆成原子陳述句再逐句驗證，跑完一百題測試集實際送給裁判模型的 Token 數量，往往是原本問答系統消耗量的五倍到十倍以上。建議只在重大版本發布前跑全量測試，日常開發先用二十到三十題的高代表性黃金樣本集快速驗證。

其次是裁判模型的系統性偏差。語言模型天生偏愛字數較多、排版工整或與自身訓練分布接近的答案。千萬不要把 Ragas 分數當成絕對真理，每次建立新指標時，至少要人工抽檢三十筆樣本，確認自動評分與人類專家的判斷高度一致。

最後是機密資料外洩與遙測風險。如果你的知識庫包含尚未公開的財報、法務合約或病患隱私，把評估任務直接送往公有雲裁判模型就會構成資料外流。高敏產業務必改用本地端部署的私有開源模型擔任裁判，並記得設定 `RAGAS_DO_NOT_TRACK=true` 關閉遙測。

## Ragas 分數異常診斷與調校速查表

跑完評測拿出一張滿是小數點的成績單後，工程師最常問的問題是「接下來到底該改哪一行程式」。這裡整理出四種最常見的指標異常組合與對應的實務排查方向，讓團隊能直接對症下藥。

- **Faithfulness 偏低但 Context Recall 很高** ：這代表檢索器其實有成功把正確答案撈出來，問題出在後端的生成模型不乖，擅自加入訓練記憶中的資訊或過度腦補推論。解決方式是調低生成模型的溫度參數，並在系統提示詞中嚴格要求模型只能依據提供的參考段落作答，遇到資料不足時必須明確回答不知道。
- **Context Recall 偏低但 Faithfulness 很高** ：代表生成模型非常老實沒有亂編，但因為檢索器根本沒撈到關鍵段落，導致回答不完整或直接答不出問題。此時再怎麼換更貴的生成模型都沒用，應該回頭檢查文件切塊大小是否把完整語意切斷、嵌入模型是否不支援該領域專有名詞，或是嘗試改用結合關鍵字與向量的混合檢索策略。
- **兩項指標同時偏低** ：通常發生在系統剛上線或知識庫文件格式混亂的時候。常見元兇是 PDF 表格或雙欄排版在轉純文字過程中順序錯亂，造成撈出來的文本支離破碎，生成模型讀不懂也只能跟著胡說八道。建議先抽檢原始切塊後的純文字內容，從文件前處理管線開始徹底清理。
- **測試集分數極高但真實用戶依然抱怨答錯** ：這意味著團隊準備的黃金測試集已經脫離真實流量分布，例如測試題全都是簡短明確的單一事實問答，但真實使用者卻常問跨多份文件的比較型問題。這時應該定期從線上失敗對話紀錄中挑選代表性案例，持續擴充並版本化管理你的 `EvaluationDataset`。

## 常見問題

### 沒有人工寫好的標準答案，還能用 Ragas 評測嗎？

可以。像忠實度 Faithfulness 這類指標屬於無參考答案評估，只需要提供系統撈出來的上下文片段與模型生成的回答就能運作，非常適合用來抓幻覺；但若要計算上下文召回率 Context Recall，就必須提供人工確認過的參考答案作為比對基準。

### Ragas 只能搭配 OpenAI 的模型使用嗎？

不是。雖然官方入門範例多以 OpenAI 示範，但透過 `LangchainLLMWrapper` 或 `llm_factory` 介面，你可以自由替換成 Anthropic Claude、Google Gemini，或是透過 Ollama 與 vLLM 架設在公司內網的本地開源模型擔任評分裁判。

### 為什麼 GitHub 網址搜 explodinggradients/ragas 會跳轉到 vibrantlabsai/ragas？

這是因為核心開發團隊 VibrantLabs 統一了組織名稱，GitHub 會自動將舊倉庫網址重新導向至新倉庫。目前無論使用 `pip install ragas` 還是從新倉庫安裝，取得的都是同一套持續更新的官方專案。

### Ragas 評分低於 0.7 就代表系統不能上線嗎？

不一定，評分門檻取決於業務容錯度。醫療、金融法遵等零容錯場景通常要求忠實度極高，而一般內部文件搜尋助理則可以接受較寬鬆的分數。重點不在追求絕對滿分，而是確保每次改版後關鍵指標不能比前一版退步。

## 結語

打造可靠的檢索增強生成系統，最忌諱「改完參數後自己問兩題覺得不錯就上線」。Ragas 把原本模糊不清的問答體感，變成了可以寫進自動化測試報告裡的精準儀表板，讓檢索團隊與模型團隊不再互相推卸責任。

- **適合誰** ：正在開發或維護企業知識庫問答系統的 AI 工程師、需要量化比較不同向量資料庫與切塊策略的技術團隊，以及想在持續整合流程中加入自動化品質閘門的開發者。
- **不適合誰** ：完全不處理外部文件檢索、只做純創意寫作或單純聊天陪伴的應用團隊，以及連數十筆測試集都不願整理、只想靠人工即時試玩的個人專案。
- **為什麼** ：因為評估系統本身需要消耗額外算力與建立測試資料的紀律，只有當你的產品規模大到需要反覆迭代檢索管線時，Ragas 帶來的除錯效率與穩定性紅利才會遠超它的導入成本。

如果你想學習更多 RAG 架構優化技巧、開源評測工具實戰與企業 AI 上線檢核清單，歡迎免費訂閱 AI 郵報，每週為你精選最值得工程師投資時間的硬核技術指南。