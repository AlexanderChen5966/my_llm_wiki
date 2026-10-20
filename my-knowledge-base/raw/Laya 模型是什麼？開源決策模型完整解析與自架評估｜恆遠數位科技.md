---
title: "Laya 模型是什麼？開源決策模型完整解析與自架評估｜恆遠數位科技"
source: "https://foreverwebs.com/blog/laya-open-source-decision-model-guide?srsltid=AU7gw4Xm983p9HNxhHnTAdW66q34wnofqG74onOpy-Bt0gMXlRBWKjNX"
author:
  - "[[恆遠數位編輯團隊]]"
published: 2026-09-23
created: 2026-10-06
description: "Laya 是 Convai Innovations 開源的 System One 決策模型，Apache 2.0、可完全自架、沒有 API。拆解三個 checkpoint、RLCD 訓練法，以及誰該自架誰該用 API。"
tags:
  - "clippings"
status: processed
---
**Laya 模型** 是 Convai Innovations 在 2026 年 9 月 18 日開源的決策模型：你給它一段文字或一包 JSON 當作狀態，附上幾個答案範圍固定的問題，它回你選項、分數或是非，每個答案都帶機率。它不生成任何文字，官方定位跟 TypeSafe 的 Jev 同一類，叫 System One 模型。最大的差別在授權與部署：權重採 Apache 2.0 全部公開，可以整包跑在自己的機器上，沒有 API、沒有帳單。

我們是在寫完 [**Jev 模型技術解析**](https://foreverwebs.com/blog/jev-model-typesafe-system-one-technical-guide) 的隔幾天遇到它的。當時留了一個問號：官方明講英文最準、中日韓文字要自己測。Laya 出現後這個問號變成兩個，因為它宣稱支援 100 多種語言、而且比 Jev 快 7 倍、還免費。我們花了一個晚上把它裝進一台 M1 Mac mini 實際跑過，這篇先把它是什麼講清楚：架構怎麼運作、三個 checkpoint 差在哪、官方數字哪些可信、以及開箱即用會踩到的那個大坑。

先講為什麼值得花時間看。 [**官方 GitHub 專案**](https://github.com/NandhaKishorM/laya) 開源四天就累積 14,736 顆星、1,206 個 fork，生態系裡已經長出 Apple Silicon 的 MLX runtime、Rust 版推論引擎、瀏覽器版本。這種速度在模型圈很少見，代表「把判斷從生成裡拆出來」這件事不只是一家公司的行銷語言，而是一整批工程師都在等的東西。

## Laya 模型是什麼：一個可以整包搬回家的判斷引擎

一句話定位：Laya 是給程式用的判斷引擎，跑在你自己的硬體上。下面這張表先把整篇結論擺出來，後面每節再展開。

| **你想知道的** | **一句話答案** |
| --- | --- |
| 它是什麼 | Convai Innovations 的開源 System One 決策模型，輸入狀態與問題、輸出選項／分數／是非加機率 |
| 跟 Jev 的關係 | 介面幾乎一樣、定位相同，差別在 Jev 是付費 API、Laya 是可自架的開放權重 |
| 授權 | Apache 2.0，商用免授權金 |
| 大小 | 英文版 421M 參數約 808MB，多語言版 322M 約 647MB |
| 速度 | 官方 T4 GPU 單題 32.8 毫秒；我們在 M1 Mac mini 的 CPU 上實測 86 毫秒 |
| 中文 | 要用 multilingual 那個 checkpoint，英文版遇到非拉丁文字會自信地答錯 |
| 有沒有 API | 沒有。只有 pip 套件與權重，要 HTTP 端點得自己包一層 |
| 最大的坑 | 開箱即用的準確率很低，官方自己的數字是 0.362，比「永遠猜最大類」還差 |
| 怎麼開始 | pip install laya，第一次呼叫會自動下載權重 |

要理解它，先看它跟一般語言模型在做分類時的差別。你請 GPT 或 Claude 把一封客服信歸類，模型做的事是一個一個預測下一個 token：先吐左大括號、再吐欄位名、再吐冒號、再吐答案，一路到 JSON 收尾。你要的其實只是「technical」這一個詞，卻付了整份 JSON 的生成成本。

Laya 把這件事反過來。它讀一次狀態，對每個問題直接輸出「答案空間裡每個選項的機率」，全程沒有生成任何文字。用棒球主審來比喻：一般語言模型像個會把整場比賽寫成賽後報導的記者，Laya 像那個只喊好球壞球的主審，從頭到尾沒寫過一個字，但每一球都要當場判。

這個設計帶來一個實際好處：沒有東西需要解析，也沒有格式可以壞掉。用語言模型做分類最惱人的其實是格式：它偶爾漏一個逗號就害 JSON 解析失敗，你得多寫一套重試邏輯。Laya 的輸出本來就是結構化的數值，程式拿到就能用。

講到「AI 讀完內容做判斷、程式接著動」這種形狀的任務，這件事我們自己已經有現成的在跑。 [**SalesKing**](https://foreverwebs.com/portfolio/salesking) 是我們每天在用的 CRM，AI 讀完 LINE 對話後判斷客戶處在哪個銷售階段、生成追蹤計畫，那一層「判斷銷售階段」就是標準的選擇題：選項固定、答案要能讓後面的自動化直接用。這類判斷點該用付費 API 還是自架開源模型，正是這篇要幫你想清楚的事。如果你手上的系統也有這種節點，可以直接跟我們聊，我們會先幫你看哪幾題適合怎麼拆。

### 它是誰做的

作者是 Nandakishor M，Convai Innovations 的執行長，過去在印度理工學院帕拉卡德分校的技術創新中心擔任主持人。發布節奏可以從三個地方交叉核對： [**PyPI 套件**](https://pypi.org/project/laya/) 0.1.0 版的上傳時間是 9 月 18 日 04:38，GitHub 專案建立於同日 04:46， [**Hugging Face 模型頁**](https://huggingface.co/convaiinnovations/laya) 建立於 05:05，27 分鐘內一條龍發布。 [**官方網站**](https://laya.convaiinnovations.com/) 也把這三個連結掛在一起。

有兩個容易混淆的地方要先講清楚。第一，Convai Innovations 這家印度公司跟做遊戲角色對話的 convai.com 完全無關，兩者只是名字接近。第二，Laya 這個字在中文圈會撞到 LayaAir 這套 HTML5 遊戲引擎，搜尋時要加上「模型」或「決策模型」才找得到對的東西。

## 三個 checkpoint 與那個會自動選型的 Router

先給答案：中文一律用 multilingual，英文版遇到非拉丁文字會壞掉，而且是自信地壞掉。

| **checkpoint** | **骨幹編碼器** | **參數** | **context** | **適用** |
| --- | --- | --- | --- | --- |
| laya（英文） | ModernBERT-large | 421M | 512 | 英文內容、防護欄、郵件分流 |
| laya-multilingual | mmBERT-base | 322M | 1024（編碼器可到 8k） | 100 多種語言，速度約快 2.2 倍 |
| laya-typed-decisions | ModernBERT-large | 421M | 1024 | 官方那四個工作流，微調過 |

三個 checkpoint 前面掛了一個 Router，它會在半毫秒內用純 Python 偵測文字的書寫系統，再決定送給哪個模型。官方給的理由很嚇人：英文版處理高棉文時是 **0.000 的準確率配上 0.952 的信心** 。模型錯得很篤定，信心門檻完全擋不住，所以偵測必須發生在推論之前。

Router 有個效能陷阱要先知道。預設只保留一個 checkpoint 在記憶體裡，所以流量在語言之間跳動時，每次切換都要重建模型，官方實測中位數是 CPU 7.4 秒、T4 10.3 秒。正式環境一定要用 \`Router(preload=True)\` 把要用的都常駐，這也決定了它天生是「常駐服務」的形狀，不能寫成每次請求才載入的無伺服器函式。

```
import laya
from laya import Router

# 正式環境務必 preload，否則語言切換要重建模型（CPU 中位數 7.4 秒）
router = Router(preload=True)

state = {
    "from": "user@acme.com",
    "subject": "報價單第 3 項金額怪怪的",
    "body": "剛收到報價單，第三項單價好像多算了一個零，可以幫忙確認嗎？",
}

questions = {
    "department": {
        "type": "choice",
        "instructions": "這則訊息應該由哪個團隊處理",
        "criteria": {
            "billing": "帳款、發票、退費",
            "technical": "系統錯誤、串接問題",
            "sales": "報價、合約、方案內容",
        },
    },
    "urgency": {
        "type": "score",
        "instructions": "這件事需要多快處理",
        "criteria": ["這週內回覆即可", "今天內要有人回", "現在就要處理"],
    },
    "churn_risk": {
        "type": "noul",
        "instructions": "客戶有沒有表達想終止合作",
    },
}

res = router.predict(state, questions)
print(res["answers"]["department"]["choice"])      # sales
print(res["answers"]["urgency"]["score"])          # 0.87
print(res["answers"]["churn_risk"]["noul"])        # 0.04
print(res["routing"]["model"])                     # multilingual
```

回應除了答案本身，還會附一段 routing 說明它為什麼選這個 checkpoint。另外每個答案都有一個 Jev 沒有的欄位叫 \`action\`，裡面是 \`act\_probability\`，可以當作「這題要不要直接讓程式動作」的第二道訊號。

### 裝起來要多少空間

我們在一台 16GB 記憶體的 M1 Mac mini 上實際裝過，數字給你參考：虛擬環境連同 PyTorch 與 Transformers 是 922MB，多語言版權重 647MB，載入模型後常駐記憶體約 1.5 到 2GB。整包裝完約 1.6GB，跟一般人想像的「跑模型要很多空間」差很遠，因為它的參數量只有能實用的最小語言模型的十七分之一。

速度我們也量了：M1 的 CPU 上單題中位數 86 毫秒、九成在 91 毫秒以內。這個數字比官方自己給的 CPU 區間（193 到 464 毫秒）還快一倍多，也比我們量到的 Jev 從台北呼叫的 611 毫秒快 7 倍。模型第一次載入要 35 秒，之後常駐就穩定在 86 毫秒。

## 三種問題型態與 RLCD 訓練法

問題型態跟 Jev 完全對得上，這是刻意的相容設計，也是它被稱為開源替代品的原因。

| **型態** | **問什麼** | **回傳** | **典型用途** |
| --- | --- | --- | --- |
| choice | 從固定選項挑一個 | 選中的選項、每個選項的機率、信心 | 工單分類、意圖路由 |
| score | 依有順序的等級評分 | 分數、每個等級的機率、信心 | 情緒強度、風險分級 |
| noul | 一句敘述成不成立 | 0 到 1 的機率 | 有沒有要求退款、有沒有想解約 |

訓練方法官方叫 RLCD，全名是針對校準決策的強化學習。機制講白話是這樣：模型輸出一個機率分佈，訓練時在它的輸出上加零平均的雜訊做探索，然後用一種數學上「只有誠實報告機率才能拿到最高分」的評分規則來給獎勵。所以模型學到的不只是「答對」，還包括「該有幾成把握就報幾成」。

這個「校準」概念值得多解釋一句，因為它是這類模型最重要的賣點。校準的意思是：一群被模型標成 0.8 信心的答案，長期看大約八成是對的。它是群體性質，不保證單一答案正確。有了校準，你才能在程式裡寫「信心低於某個值就交給人」這種規則，整套自動化才敢上線。

這套方法跟 Jev 用的名字一樣也叫 RLCD，兩家的實作細節沒有公開到能逐項比對的程度。Laya 這邊有把訓練腳本與微調 notebook 一起開源，所以至少看得到損失函數怎麼寫、超參數設多少，這是閉源 API 給不了的東西。

官方公布的校準指標 ECE 是 0.081，比 Jev 公布的 0.246 好三倍。但要注意這個數字有個前提，下一節會講。

## 官方數字怎麼讀：哪些可信、哪些有前提

先給答案：速度的數字是真的，準確率的數字有前提，而且那個前提在中文圈的報導裡幾乎都被漏掉了。

| **官方宣稱** | **怎麼讀** |
| --- | --- |
| 單題 32.8 毫秒，比 Jev 快 7.8 倍 | 可信。基準是 T4 GPU，Jev 那邊引用的是第三方量到的 236 到 276 毫秒 |
| typed-decisions 準確率 0.766，勝過 Jev 的 0.727 | 有前提。這是在該 benchmark 自己的訓練集上微調過的 checkpoint |
| ECE 0.081，校準比 Jev 好三倍 | 有前提。官方註明是經過溫度重新擬合之後；未調整前基礎模型是 0.466 |
| 支援 100 多種語言，51 種語言中 45 種可用 | 可信，但中文要用 multilingual、而且沒有繁體中文的單獨數字 |
| Apache 2.0、自架零成本 | 可信。但零成本指的是沒有 API 帳單，硬體與維運仍要算 |
| Banking77 上 0.425，落後 Jev 的 0.870 | 官方自己揭露的弱項，選項超過 20 個就會明顯掉 |

### 最重要的那個前提：開箱即用接近亂猜

官方模型卡在「誠實的限制」那節寫得很白：基礎 checkpoint 在 typed-decisions 上的零樣本準確率是 0.362，多語言版 0.342，而隨機猜是 0.318、「永遠選最大類」是 0.461。換句話說， **開箱即用的 Laya 比亂猜好一點點，比最笨的基準線還差** 。那個亮眼的 0.766 屬於另一個 checkpoint，它在該 benchmark 自己的訓練集上微調過。

官方自己的結論是一句很誠實的話：Laya 是一個可以快速特化的底座，不是零樣本決策引擎。這句話跟中文圈流傳的「開源模型直接秒殺閉源 API」是兩件完全不同的事，而差別就在你有沒有準備好標註資料與微調的力氣。

我們拿 221 筆台灣客服訊息實測過這件事，數字比官方講的更難看，完整的混淆矩陣與錯誤分析寫在 [**Laya 繁體中文實測那篇**](https://foreverwebs.com/blog/laya-traditional-chinese-benchmark-finetune) 。這裡先講結論：繁中零樣本的分類正確率是 44.8%，而且 221 筆裡有 140 筆被判成同一個類別。

第三方的獨立比較也指向同一個結論。 [**Artifilog 的 Laya 與 Jev 對照**](https://www.artifilog.com/posts/laya-vs-jev) 整理出的建議是：延遲要壓在 50 毫秒內、選項在 2 到 50 個之間、需要跨語言、而且願意投入時間在自己的資料上微調，這四個條件同時成立時才選 Laya；反過來，選項超過 50 個、或者不想管 GPU 與微調的團隊，付費 API 仍然是比較省事的選擇。

### 那個 0.362 對台灣團隊的意思

把這個數字翻成白話：你 pip 安裝完、照文件寫好問題、把客戶訊息丟進去，得到的答案品質大概跟丟骰子差不多。這跟「模型不好」是兩回事，它只是還沒學過你的任務長什麼樣子。官方把微調 notebook 一起開源，就是因為他們知道大家一定得走這一步。

好消息是這一步比想像中便宜。官方 notebook 在雙 T4 上訓練那格標的是四到六分鐘，我們改成單機版在 M1 Mac mini 的 CPU 上跑 150 筆資料，四個 epoch 花了 609 秒。錢是零，要的是標註資料。

## 它沒有 API，這件事的實際後果

先給答案：Laya 只有 pip 套件跟權重，要 HTTP 端點得自己包，而且必須是常駐服務。

| **你想要的** | **Laya 給你的** | **你要自己做的** |
| --- | --- | --- |
| 一個可以 curl 的網址 | \`router.predict()\` 這個 Python 呼叫 | 包一層 FastAPI 或 Flask |
| 按用量付費 | 權重下載後隨你跑 | 準備一台常駐機器，算它的月租與電費 |
| 自動擴容 | 單一行程 | 自己做負載平衡與健康檢查 |
| 穩定的版本 | Hugging Face 上的 checkpoint | 自己釘版本，換版前重跑一次評測 |
| 模型卡與弱點清單 | 有，而且寫得誠實 | 把它讀完再設計你的問題 |

官方提供的入口只有三個：\`pip install laya\`、Hugging Face 上的線上體驗空間、以及一本微調用的 notebook。體驗空間可以拿來試感覺，它終究只是展示頁，沒辦法當production 端點用。完整清單是 [**Hugging Face 模型庫**](https://huggingface.co/convaiinnovations/laya) 、 [**PyPI 套件頁**](https://pypi.org/project/laya/) 、以及 [**線上體驗空間**](https://huggingface.co/spaces/convaiinnovations/laya-demo) 。

所以評估時真正要比的從來就不是「0.042 美元對上 0 元」。自架要算的是常駐機器的月費、包 API 的工時、監控與換版的維運，以及最重要的那一項：準備標註資料的人力。我們在 [**AI 導入成本拆解**](https://foreverwebs.com/blog/custom-ai-system-development-cost-breakdown) 那篇列過三個預算級距各能做什麼，自架模型這條路的成本結構比較接近「養一個小系統」，跟「買一個服務」差很多。

### 三個中文圈常見的誤讀

- 把 0.766 當成開箱即用的成績：那是微調後的專用 checkpoint，基礎版零樣本只有 0.362
- 把「快 7 倍」當成「準 7 倍」：速度差來自本機推論與跨海 API 的架構差異，跟判斷品質是兩件事
- 把「免費」當成「零成本」：省的是 API 帳單，付的是常駐機器、包服務的工時、還有標註資料的人力

## 誰適合用 Laya，誰應該直接用 API

先給答案：資料不能出境、或者延遲必須壓在 100 毫秒內，這兩種情況值得自架；其他情況先用 API 把價值驗證出來。

<svg id="mermaid_r_0_" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 999.171875px;" viewBox="0 0 999.171875 1379.0625" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid_r_0__flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid_r_0__flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid_r_0__flowchart-v2-pointEnd-margin" viewBox="0 0 11.5 14" refX="11.5" refY="7" markerUnits="userSpaceOnUse" markerWidth="10.5" markerHeight="14" orient="auto"><path d="M 0 0 L 11.5 7 L 0 14 z" style="stroke-width: 0; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid_r_0__flowchart-v2-pointStart-margin" viewBox="0 0 11.5 14" refX="1" refY="7" markerUnits="userSpaceOnUse" markerWidth="11.5" markerHeight="14" orient="auto"><polygon points="0,7 11.5,14 11.5,0" style="stroke-width: 0; stroke-dasharray: 1, 0;"></polygon></marker><marker id="mermaid_r_0__flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid_r_0__flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid_r_0__flowchart-v2-circleEnd-margin" viewBox="0 0 10 10" refY="5" refX="12.25" markerUnits="userSpaceOnUse" markerWidth="14" markerHeight="14" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 0; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid_r_0__flowchart-v2-circleStart-margin" viewBox="0 0 10 10" refX="-2" refY="5" markerUnits="userSpaceOnUse" markerWidth="14" markerHeight="14" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 0; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid_r_0__flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid_r_0__flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid_r_0__flowchart-v2-crossEnd-margin" viewBox="0 0 15 15" refX="17.7" refY="7.5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M 1,1 L 14,14 M 1,14 L 14,1" style="stroke-width: 2.5;"></path></marker><marker id="mermaid_r_0__flowchart-v2-crossStart-margin" viewBox="0 0 15 15" refX="-3.5" refY="7.5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M 1,1 L 14,14 M 1,14 L 14,1" style="stroke-width: 2.5; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g><path d="M563.086,72L563.086,76.167C563.086,80.333,563.086,88.667,563.086,96.333C563.086,104,563.086,111,563.086,114.5L563.086,118" id="mermaid_r_0_-L_A_B_0" style=";" data-edge="true" data-et="edge" data-id="L_A_B_0" data-points="W3sieCI6NTYzLjA4NTkzNzUsInkiOjcyfSx7IngiOjU2My4wODU5Mzc1LCJ5Ijo5N30seyJ4Ijo1NjMuMDg1OTM3NSwieSI6MTIyfV0=" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M483.421,282.335L445.018,301.779C406.614,321.223,329.807,360.112,291.404,403.733C253,447.354,253,495.708,253,544.063C253,592.417,253,640.771,255.189,681.121C257.378,721.47,261.755,753.816,263.944,769.988L266.133,786.161" id="mermaid_r_0_-L_B_C_0" style=";" data-edge="true" data-et="edge" data-id="L_B_C_0" data-points="W3sieCI6NDgzLjQyMTEyNzQyNDIzMTAzLCJ5IjoyODIuMzM1MTg5OTI0MjMxMDN9LHsieCI6MjUzLCJ5IjozOTl9LHsieCI6MjUzLCJ5Ijo1NDQuMDYyNX0seyJ4IjoyNTMsInkiOjY4OS4xMjV9LHsieCI6MjY2LjY2OTE3MjkzMjMzMDgsInkiOjc5MC4xMjV9XQ==" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M612.301,312.784L622.292,327.154C632.283,341.523,652.265,370.261,662.255,390.131C672.246,410,672.246,421,672.246,426.5L672.246,432" id="mermaid_r_0_-L_B_D_0" style=";" data-edge="true" data-et="edge" data-id="L_B_D_0" data-points="W3sieCI6NjEyLjMwMTQ4ODUyMjIwNTMsInkiOjMxMi43ODQ0NDg5Nzc3OTQ3NH0seyJ4Ijo2NzIuMjQ2MDkzNzUsInkiOjM5OX0seyJ4Ijo2NzIuMjQ2MDkzNzUsInkiOjQzNn1d" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M600.043,579.922L563.396,598.122C526.749,616.323,453.454,652.724,403.414,687.242C353.374,721.761,326.588,754.397,313.195,770.715L299.802,787.033" id="mermaid_r_0_-L_D_C_0" style=";" data-edge="true" data-et="edge" data-id="L_D_C_0" data-points="W3sieCI6NjAwLjA0Mjg0NjcyMTEzNzUsInkiOjU3OS45MjE3NTI5NzExMzc1fSx7IngiOjM4MC4xNjAxNTYyNSwieSI6Njg5LjEyNX0seyJ4IjoyOTcuMjY0MDk3NzQ0MzYwOSwieSI6NzkwLjEyNX1d" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M690.764,633.607L692.678,642.86C694.592,652.113,698.419,670.619,700.332,685.372C702.246,700.125,702.246,711.125,702.246,716.625L702.246,722.125" id="mermaid_r_0_-L_D_E_0" style=";" data-edge="true" data-et="edge" data-id="L_D_E_0" data-points="W3sieCI6NjkwLjc2NDQ4MDA0MDYxMDUsInkiOjYzMy42MDY2MTM3MDkzODk1fSx7IngiOjcwMi4yNDYwOTM3NSwieSI6Njg5LjEyNX0seyJ4Ijo3MDIuMjQ2MDkzNzUsInkiOjcyNi4xMjV9XQ==" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M650.901,866.78L633.971,881.504C617.041,896.228,583.181,925.677,566.25,956.646C549.32,987.615,549.32,1020.104,549.32,1036.349L549.32,1052.594" id="mermaid_r_0_-L_E_F_0" style=";" data-edge="true" data-et="edge" data-id="L_E_F_0" data-points="W3sieCI6NjUwLjkwMTAzODYyNDc5LCJ5Ijo4NjYuNzc5OTQ0ODc0Nzl9LHsieCI6NTQ5LjMyMDMxMjUsInkiOjk1NS4xMjV9LHsieCI6NTQ5LjMyMDMxMjUsInkiOjEwNTYuNTkzNzV9XQ==" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M753.591,866.78L770.521,881.504C787.451,896.228,821.312,925.677,838.242,958.646C855.172,991.615,855.172,1028.104,855.172,1046.349L855.172,1064.594" id="mermaid_r_0_-L_E_G_0" style=";" data-edge="true" data-et="edge" data-id="L_E_G_0" data-points="W3sieCI6NzUzLjU5MTE0ODg3NTIxLCJ5Ijo4NjYuNzc5OTQ0ODc0Nzl9LHsieCI6ODU1LjE3MTg3NSwieSI6OTU1LjEyNX0seyJ4Ijo4NTUuMTcxODc1LCJ5IjoxMDY4LjU5Mzc1fV0=" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M271,854.125L271,870.958C271,887.792,271,921.458,271,943.792C271,966.125,271,977.125,271,982.625L271,988.125" id="mermaid_r_0_-L_C_H_0" style=";" data-edge="true" data-et="edge" data-id="L_C_H_0" data-points="W3sieCI6MjcxLCJ5Ijo4NTQuMTI1fSx7IngiOjI3MSwieSI6OTU1LjEyNX0seyJ4IjoyNzEsInkiOjk5Mi4xMjV9XQ==" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M215.754,1153.816L199.795,1169.191C183.836,1184.565,151.918,1215.314,135.959,1238.188C120,1261.063,120,1276.063,120,1283.563L120,1291.063" id="mermaid_r_0_-L_H_I_0" style=";" data-edge="true" data-et="edge" data-id="L_H_I_0" data-points="W3sieCI6MjE1Ljc1Mzc2ODMxNDUzNTY4LCJ5IjoxMTUzLjgxNjI2ODMxNDUzNTZ9LHsieCI6MTIwLCJ5IjoxMjQ2LjA2MjV9LHsieCI6MTIwLCJ5IjoxMjk1LjA2MjV9XQ==" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M326.246,1153.816L342.205,1169.191C358.164,1184.565,390.082,1215.314,406.041,1236.188C422,1257.063,422,1268.063,422,1273.563L422,1279.063" id="mermaid_r_0_-L_H_J_0" style=";" data-edge="true" data-et="edge" data-id="L_H_J_0" data-points="W3sieCI6MzI2LjI0NjIzMTY4NTQ2NDMsInkiOjExNTMuODE2MjY4MzE0NTM1Nn0seyJ4Ijo0MjIsInkiOjEyNDYuMDYyNX0seyJ4Ijo0MjIsInkiOjEyODMuMDYyNX1d" data-look="classic" marker-end="url(#mermaid_r_0__flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g><g data-id="L_A_B_0" transform="translate(0, 0)"></g></g><g transform="translate(253, 544.0625)"><g data-id="L_B_C_0" transform="translate(-16, -12)"><foreignObject width="32" height="24"><p>不行</p></foreignObject></g></g><g transform="translate(672.24609375, 399)"><g data-id="L_B_D_0" transform="translate(-16, -12)"><foreignObject width="32" height="24"><p>可以</p></foreignObject></g></g><g transform="translate(431.58899, 663.58322)"><g data-id="L_D_C_0" transform="translate(-16, -12)"><foreignObject width="32" height="24"><p>需要</p></foreignObject></g></g><g transform="translate(702.24609375, 689.125)"><g data-id="L_D_E_0" transform="translate(-24, -12)"><foreignObject width="48" height="24"><p>不需要</p></foreignObject></g></g><g transform="translate(549.3203125, 955.125)"><g data-id="L_E_F_0" transform="translate(-16, -12)"><foreignObject width="32" height="24"><p>沒有</p></foreignObject></g></g><g transform="translate(855.171875, 955.125)"><g data-id="L_E_G_0" transform="translate(-48.90625, -12)"><foreignObject width="97.8125" height="24"><p>有 150 筆以上</p></foreignObject></g></g><g><g data-id="L_C_H_0" transform="translate(0, 0)"></g></g><g transform="translate(120, 1246.0625)"><g data-id="L_H_I_0" transform="translate(-8, -12)"><foreignObject width="16" height="24"><p>是</p></foreignObject></g></g><g transform="translate(422, 1246.0625)"><g data-id="L_H_J_0" transform="translate(-8, -12)"><foreignObject width="16" height="24"><p>否</p></foreignObject></g></g></g><g><g id="mermaid_r_0_-flowchart-A-0" data-look="classic" transform="translate(563.0859375, 40)"><rect style="" x="-128" y="-32" width="256" height="64" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-88, -12)"><rect></rect><foreignObject width="176" height="24"><p>你有一個判斷點要自動化</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-B-1" data-look="classic" transform="translate(563.0859375, 242)"><polygon points="120,0 240,-120 120,-240 0,-120" transform="translate(-119.5, 120)" fill="none" stroke="currentColor"></polygon><g style="" transform="translate(-88, -12)"><rect></rect><foreignObject width="176" height="24"><p>客戶資料可以送到境外嗎</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-C-3" data-look="classic" transform="translate(271, 822.125)"><rect style="" x="-97.328125" y="-32" width="194.65625" height="64" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-57.328125, -12)"><rect></rect><foreignObject width="114.65625" height="24"><p>自架路線：Laya</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-D-5" data-look="classic" transform="translate(672.24609375, 544.0625)"><polygon points="108.0625,0 216.125,-108.0625 108.0625,-216.125 0,-108.0625" transform="translate(-107.5625, 108.0625)" fill="none" stroke="currentColor"></polygon><g style="" transform="translate(-76.0625, -12)"><rect></rect><foreignObject width="152.125" height="24"><p>需要壓在 100ms 內嗎</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-E-9" data-look="classic" transform="translate(702.24609375, 822.125)"><polygon points="96,0 192,-96 96,-192 0,-96" transform="translate(-95.5, 96)" fill="none" stroke="currentColor"></polygon><g style="" transform="translate(-64, -12)"><rect></rect><foreignObject width="128" height="24"><p>手上有標註資料嗎</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-F-11" data-look="classic" transform="translate(549.3203125, 1100.59375)"><rect style="" x="-119.8515625" y="-44" width="239.703125" height="88" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-79.8515625, -24)"><rect></rect><foreignObject width="159.703125" height="48"><p>先用付費 API 驗證價值<br>同時開始累積標註</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-G-13" data-look="classic" transform="translate(855.171875, 1100.59375)"><rect style="" x="-136" y="-32" width="272" height="64" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-96, -12)"><rect></rect><foreignObject width="192" height="24"><p>兩條都試，用同一批題目比</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-H-15" data-look="classic" transform="translate(271, 1100.59375)"><polygon points="108.46875,0 216.9375,-108.46875 108.46875,-216.9375 0,-108.46875" transform="translate(-107.96875, 108.46875)" fill="none" stroke="currentColor"></polygon><g style="" transform="translate(-76.46875, -12)"><rect></rect><foreignObject width="152.9375" height="24"><p>選項數量超過 20 個嗎</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-I-17" data-look="classic" transform="translate(120, 1327.0625)"><rect style="" x="-112" y="-32" width="224" height="64" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-72, -12)"><rect></rect><foreignObject width="144" height="24"><p>改成兩層粗到細分類</p></foreignObject></g></g><g id="mermaid_r_0_-flowchart-J-19" data-look="classic" transform="translate(422, 1327.0625)"><rect style="" x="-140" y="-44" width="280" height="88" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -24)"><rect></rect><foreignObject width="200" height="48"><p>準備標註資料 + 微調 + 溫度校準</p></foreignObject></g></g></g></g></g><defs></defs><defs></defs><linearGradient id="mermaid_r_0_-gradient" gradientUnits="objectBoundingBox" x1="0%" y1="0%" x2="100%" y2="0%"><stop offset="0%" stop-color="#E5A600" stop-opacity="1"></stop><stop offset="100%" stop-color="#D1D9E6" stop-opacity="1"></stop></linearGradient></svg>

判斷點自動化時，選開源自架或付費 API 的決策路徑

### 適合自架的三種情況

- 資料不能離開自己的機房：政府標案、醫療、金融，或客戶合約裡直接寫明的情況。這是自架最無法被取代的價值
- 延遲必須壓在 100 毫秒內：使用者打字時就要即時判斷、串流內容過濾、遊戲或即時控制。遠端 API 光是跨太平洋來回就吃掉幾百毫秒
- 呼叫量大到 API 帳單有感：這個門檻比想像中高很多，我們實測 221 筆訊息的 API 成本是 0.0054 美元，換算一個月十萬封信不到新台幣 80 元

### 應該先用 API 的三種情況

- 手上還沒有標註資料：沒有資料就沒辦法微調，而沒微調的 Laya 在中文上不堪用
- 選項數量很多：官方建議控制在 20 個以內，超過要自己改 head\_max\_len 或做兩層分類
- 團隊沒有人養得動一台常駐推論機器：這不只是開機而已，還有監控、換版、記憶體與擴容

選項數量那條限制值得單獨記住，因為它是架構決定的、不是調參數能繞過的。所有選項共用一個固定的 token 預算（英文版 192、多語言版 256），選項越多每個分到的字數越少，多到一定程度時選項之間就變得難以區分。官方在 77 個選項的 Banking77 上只拿到 0.425，同一份測驗 Jev 是 0.870。如果你的分類表有幾十個類別，要嘛提高預算參數，要嘛拆成兩層。

我們自己的 [**客服分流流程**](https://foreverwebs.com/blog/internal-ai-workflow-series-03-customer-service-routing-sop-3-layers-4-confidence-2-human-handoff) 同時符合「即時性不高」與「已經有標註資料」兩個條件，所以現階段的取捨是繼續用 API 跑正式流程，同時在本機養一個自架版本測試資料不出境的可行性。如果你的進件來自 LINE 官方帳號，前面的接法照 [**LINE 官方帳號接 AI 的三種整合路徑**](https://foreverwebs.com/blog/line-oa-ai-integration-complete-guide-taiwan-smb) ，換模型只是換掉中間那個判斷節點。

### 公司要把決策模型接進自己的系統時

從「跑得起來」到「公司每天在用」之間，差的是一層客製：問題怎麼寫成它讀得懂的形式、訊息怎麼清成乾淨的狀態、門檻怎麼從你的資料畫出來、人工複審那條路怎麼接回現有系統。少了這一層，它就是一個很快的函式；有了這一層，它才是流程裡的一個節點。

實際接進公司時，最花工的通常落在模型外面：判斷要讀的資料從 ERP、CRM 或客服系統哪張表來，判斷結果寫回哪裡、誰有權限覆核，模型換版時舊的判斷紀錄怎麼留。這些屬於系統整合，常見的六種場景與三種架構整理在 [系統整合是什麼](https://foreverwebs.com/blog/system-integration-6-scenarios-3-architectures-prep-checklist) ；要從公司層級排優先順序與預算，先看 [企業 AI 導入完整指南](https://foreverwebs.com/blog/enterprise-ai-adoption-guide-2026) 。

不用一次到位，從你最頭痛的那一條流程開始就好。先聊一下你現在的情況，我們會直接告訴你「這個值得做嗎、大概怎麼做」，這個階段我們陪你一起想，後面真的要動手再談範圍跟費用。可以把你公司現在的流程丟過來，我們很樂意 [**聽你聊聊現況**](https://foreverwebs.com/services/ai-consult) ，一起看看哪幾個判斷點適合先拆出來。

![終端機畫面：Laya 多語言版對 221 筆台灣客服訊息 zero-shot 的報表，分類正確率 44.8%、混淆矩陣顯示類別塌陷](https://r2.foreverwebs.com/media/laya-zh-tw-zeroshot-report.jpg)

終端機畫面：Laya 多語言版對 221 筆台灣客服訊息 zero-shot 的報表，分類正確率 44.8%、混淆矩陣顯示類別塌陷

## 我們怎麼看：開源把成本從帳單搬到了工程

ℹ️ **我們怎麼看**

Laya 最值得注意的地方是它證明了一件事：把判斷從生成裡拆出來之後，這一層可以小到 421M 參數、跑在一台沒有顯示卡的 Mac mini 上。這個方向我們認為會留下來，三年後每個像樣的系統裡都會有一層便宜的判斷器，模型叫什麼名字反而不重要。但「開源免費」這句話要換算清楚：它把成本從 API 帳單搬到了標註資料與工程維運，對已經有資料團隊的公司是省錢，對還在驗證價值的公司是變貴。我們自己的取捨是：正式流程先用付費 API 跑起來、把價值驗證出來、順便累積標註資料，同時在本機養一個自架版本，等到「資料不能出境」這種需求真的出現時才切換。對中小企業老闆來說，現在該問的問題是：我的流程裡有哪些判斷點，是連送到境外都不行的？那幾個才是自架的理由。

生態系的反應也值得看一眼。這個專案開源四天累積了 14,736 顆星，但也同時開了 101 個 issue，其中 [**第 102 號**](https://github.com/NandhaKishorM/laya/issues/102) 就是使用者回報高基數選項的解法：先做一輪粗篩把選項縮到 20 個以內，準確率從 54.3% 拉到 60.8%。官方在說明文件裡誠實註明「那是回報者的數字，本專案沒有重測」。這種透明度在模型圈少見，也是我們願意認真評估它的原因之一。

想從頭理解企業導入 AI 該怎麼排優先序， [**企業 AI 導入指南**](https://foreverwebs.com/blog/enterprise-ai-adoption-guide-2026) 有整套框架；如果你考慮的是更大的自架題目， [**企業自建大型語言模型的技術路徑**](https://foreverwebs.com/blog/enterprise-self-host-llm-technical-pipeline-guide) 那篇談的是大上好幾倍的工程。

我們公司內部最省時間的三個 AI 流程，八成中小企業都用得上，而且它們的核心全部是選擇題。你公司現在最頭痛的是哪一塊？可以把現況丟過來，我們陪你一起看看適合的解法。

ℹ️ **我們做過這件事**

順帶說一下，這篇講的判斷層架構我們公司自己每天都在跑，目前內部有 20+ 個 AI 流程在工作中，客服分流就是其中一條，這次也是對著那條流程把 Laya 裝進一台 M1 Mac mini 實際跑過才寫的。我們自己每天在用的 SalesKing，AI 讀完 LINE 對話判斷客戶處在哪個銷售階段，那一層判斷跟本文講的選擇題是同一種形狀。看到這裡，如果你也在想「這套放在我們公司會是什麼樣子」，我們很樂意 [聽你聊聊現在的實際情況](https://foreverwebs.com/services/ai-consult) ，一起看看哪些做得起來、能從哪一塊開始。

**AI 導入評估表下載**

還沒決定該從哪個流程開始的話，這份評估表把「判斷點盤點、資料就緒度、風險等級」三件事做成可以自己填的表格，填完你會知道公司裡哪幾題選擇題最值得先讓 AI 接手。 [下載 AI 導入評估表（PDF）](https://r2.foreverwebs.com/media/ai-readiness-assessment.pdf)

QLaya 模型是什麼？和 Jev 有什麼關係？

Laya 是 Convai Innovations 在 2026 年 9 月 18 日開源的 System One 決策模型，介面與定位跟 TypeSafe 的 Jev 幾乎一樣：輸入狀態與問題、輸出選項或分數或是非加機率，全程不生成文字。最大差別是 Jev 只有付費 API，Laya 採 Apache 2.0 開放權重、可以完全跑在自己的機器上。

QLaya 有官方 API 可以呼叫嗎？

沒有。官方只提供 pip 套件與 Hugging Face 上的權重，另有一個線上體驗空間供試用。要在系統裡當 HTTP 端點使用，必須自己包一層服務，而且要常駐；預設設定下每次語言切換會重建模型，官方實測中位數是 CPU 7.4 秒。

QLaya 支援繁體中文嗎？

要用 laya-multilingual 這個 checkpoint，英文版遇到非拉丁文字會自信地答錯（官方舉例高棉文是 0.000 準確率配 0.952 信心）。官方沒有公布繁體中文的單獨數字，我們實測 221 筆台灣客服訊息的零樣本分類正確率是 44.8%。

Q官方說的 0.766 準確率可以直接期待嗎？

不可以。那個數字屬於在該 benchmark 自己的訓練集上微調過的 checkpoint。基礎 checkpoint 的零樣本成績是 0.362、多語言版 0.342，而隨機猜是 0.318、永遠選最大類是 0.461。官方自己的說法是「Laya 是可以快速特化的底座，不是零樣本決策引擎」。

Q自架 Laya 真的零成本嗎？

零的是 API 帳單。實際要算的是常駐機器的月費與電費、包一層服務的工時、監控與換版的維運，以及最大的一項：準備標註資料與微調的人力。對照組是我們實測 Jev 處理 221 筆訊息只花 0.0054 美元。

Q選項很多的分類任務適合用 Laya 嗎？

不太適合。所有選項共用一個固定的 token 預算，選項超過 20 個時每個選項分到的空間就不夠，官方在 77 個選項的 Banking77 上只有 0.425（Jev 是 0.870）。解法是提高 head\_max\_len，或把大分類拆成兩層粗到細。

讀到這裡，你手上應該已經有一份判斷標準了：資料能不能出境、延遲要不要壓在 100 毫秒內、手上有沒有標註資料。想知道這三題對你的系統各自是什麼答案， [**跟我們聊聊你的 AI 導入需求**](https://foreverwebs.com/services/ai-consult) ，我們會先幫你看哪一題最值得先動、大概怎麼做最划算，再談要不要真的動手。

## 留言(0)

尚無留言，成為第一個留言的人吧！

CONTENTS

- [1.Laya 模型是什麼：一個可以整包搬回家的判斷引擎](#laya-模型是什麼一個可以整包搬回家的判斷引擎)
- [1.1它是誰做的](#它是誰做的)
- [2.三個 checkpoint 與那個會自動選型的 Router](#三個-checkpoint-與那個會自動選型的-router)
- [2.1裝起來要多少空間](#裝起來要多少空間)
- [3.三種問題型態與 RLCD 訓練法](#三種問題型態與-rlcd-訓練法)
- [4.官方數字怎麼讀：哪些可信、哪些有前提](#官方數字怎麼讀哪些可信哪些有前提)
- [4.1最重要的那個前提：開箱即用接近亂猜](#最重要的那個前提開箱即用接近亂猜)
- [4.2那個 0.362 對台灣團隊的意思](#那個-0362-對台灣團隊的意思)
- [5.它沒有 API，這件事的實際後果](#它沒有-api這件事的實際後果)
- [5.1三個中文圈常見的誤讀](#三個中文圈常見的誤讀)
- [6.誰適合用 Laya，誰應該直接用 API](#誰適合用-laya誰應該直接用-api)
- [6.1適合自架的三種情況](#適合自架的三種情況)
- [6.2應該先用 API 的三種情況](#應該先用-api-的三種情況)
- [6.3公司要把決策模型接進自己的系統時](#公司要把決策模型接進自己的系統時)
- [7.我們怎麼看：開源把成本從帳單搬到了工程](#我們怎麼看開源把成本從帳單搬到了工程)

SERVICES

## 想了解更多？看看我們的相關服務

### [AI 自動化顧問](https://foreverwebs.com/services/ai-consult)

N8N + ChatGPT 企業流程整合

了解更多

### [企業形象網站架設](https://foreverwebs.com/services/seo-web)

融合 SEO 的品牌官網設計

了解更多

### [SEO 優化代操](https://foreverwebs.com/services/seo)

讓專業團隊為你操刀自然排名

了解更多