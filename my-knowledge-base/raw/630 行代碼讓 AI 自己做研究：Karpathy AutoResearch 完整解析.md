---
title: "630 行代碼讓 AI 自己做研究：Karpathy AutoResearch 完整解析"
source: "https://hackmd.io/@BASHCAT/BkHvMMXcWe"
author:
published:
created: 2026-06-04
description: "AutoResearch 封面"
tags:
  - "clippings"
status: processed
---
凌晨兩點，你盯著 terminal 上緩慢跳動的 loss 曲線，等著這一輪訓練跑完，好手動調整 learning rate 再來一次。你喝了第四杯咖啡，眼睛乾澀，但腦袋裡還在盤算：「batch size 要不要再大一點？depth 12 會不會比 8 好？Muon optimizer 的 momentum 要不要降？」

這個場景，做過 ML 研究的人都不陌生。

然後 Andrej Karpathy 在三月初丟出了一個 630 行的 Python 腳本，告訴你： **去睡覺吧，讓 AI 自己來。**

這個叫做 [autoresearch](https://github.com/karpathy/autoresearch) 的開源專案，在兩天內吸引了超過 **860 萬次瀏覽** ，整個 AI 社群炸開了鍋。它不是什麼龐大的框架，不是什麼企業級平台 — 它就是一個極簡的自動化迴圈，讓 AI agent 在你睡覺的時候跑完 100 個完整的 ML 實驗。

更讓人不安的是：agent 找到的改進，比 Karpathy 自己十多年的深度學習經驗還要好。

---

## 到底在幹嘛：五分鐘理解 AutoResearch

AutoResearch 的核心概念可以用一句話說完：

**AI agent 讀你的訓練代碼，想辦法改進它，跑一次實驗看看結果，有效就留、沒效就丟，然後重來。**

整個 repo 的核心就兩個檔案：

| 檔案 | 幹嘛用的 |
| --- | --- |
| `train.py` | Agent 唯一能編輯的檔案 — 裡面有完整的 GPT 模型定義、優化器（Muon + AdamW）、訓練迴圈，所有東西都在這 |
| `program.md` | 你寫給 agent 的「研究方向指引」— 純 Markdown，告訴 agent 該往哪個方向思考 |

對，就這樣。沒有複雜的配置檔，沒有分散式設定，除了 PyTorch 之外幾乎沒有外部依賴。

每一次實驗的完整流程長這樣：

```
1. Agent 讀取 train.py，理解當前的模型架構和超參數
2. 根據 program.md 的指引，形成一個改進假設
   （例如：「把 learning rate 從 3e-4 降到 1e-4 試試」）
3. 修改 train.py 的代碼
4. Git commit 到 feature branch
5. 執行訓練 — 嚴格跑 5 分鐘，不多不少
6. 檢查 val_bpb 有沒有變好
7. 變好 → 保留這個 commit；沒變好 → 丟掉
8. 回到步驟 1
```

五分鐘一輪，一小時 12 個實驗，一個晚上大約 100 個。Karpathy 跑了兩天，累積了 650 個完整實驗。

這裡有個重要的設計選擇值得注意： **固定時間預算** 。不管 agent 怎麼改 — 把模型變大、把 batch size 調小、換一個全新的架構 — 訓練永遠只跑 5 分鐘。這讓所有實驗直接可比較，同時也意味著 autoresearch 會自動找到「在你的硬體上，5 分鐘能訓練出的最優模型」。

代價呢？你跑出來的結果跟別人的不能直接比，因為大家的 GPU 不一樣。但 Karpathy 顯然覺得這個取捨完全值得。

---

## 這 630 行代碼裡最聰明的設計

說實話，「讓 AI 自動跑實驗」這個想法本身不算新。AutoML、超參數搜索、NAS（Neural Architecture Search）已經存在好幾年了。AutoResearch 真正讓我覺得精妙的地方，是一個看似不起眼的設計決策：

**Frozen Metric — 凍結指標。**

Agent 可以改 `train.py` 裡的任何東西 — 模型架構、優化器、learning rate、batch size、甚至整個訓練迴圈的邏輯。但有一件事它絕對碰不到：評估標準 `val_bpb` （validation bits per byte）。

這為什麼重要？

Carlo Iacono 在他的 [深度評論](https://hybridhorizons.substack.com/p/the-frozen-metric-of-autoresearch) 裡寫了一段我覺得非常精準的話：

> *"A system that can rewrite both the exam and the answers will always pass. The intelligence of autoresearch lies less in the agent's ability to propose clever changes than in Karpathy's ability to build a world where cleverness is constrained by an external standard the agent cannot corrupt."*

翻成白話：如果 agent 可以同時改考卷和改答案，那它永遠都能考滿分 — 但這毫無意義。AutoResearch 的智慧不在於 agent 有多聰明，而在於 Karpathy 建了一個「agent 無法作弊」的世界。

怎麼說呢，這其實是一個 AI alignment 問題的微縮模型。在更大的場景裡 — 比如讓 AI 優化商業指標、最大化用戶參與度 — 我們一直在擔心 AI 會找到「技術上滿足指標但實質上作弊」的捷徑（想想推薦演算法把人推向極端內容來提高點擊率）。AutoResearch 用一個極簡的方式展示了答案的一部分： **把評估標準放在 agent 的能力邊界之外。**

這個設計還有第二層含義。因為指標是凍結的，所以你作為人類可以完全信任實驗結果的比較 — 每一個 commit 的 val\_bpb 變化都是真實的、可靠的、沒有被污染的。這讓你可以放心地去睡覺，早上起來直接看哪些改動被保留了。

---

## 成效：Agent 比十年經驗更強？

來看一下實際的數字：

| 指標 | 數據 |
| --- | --- |
| val\_bpb 改善 | 從 1.0 降到 0.97 |
| 總實驗數 | ~650（兩天） |
| 單次實驗時長 | 固定 5 分鐘 |
| 每小時實驗量 | ~12 個 |

0.97 看起來好像沒降多少？在 BPB 這個指標上，0.03 的改善其實已經相當顯著 — 尤其考慮到這是在一個已經被 Karpathy 本人手動優化過的基線上再往下壓的。

但真正讓人印象深刻的不是數字本身，而是 Karpathy 後來說的這句話：

> *"I just confirmed that the improvements autoresearch found over the last 2 days of (~650) experiments on depth 12 model transfer well to depth 24."*

在 depth 12 的小模型上找到的改進， **可以轉移到 depth 24 的更大模型** 。這意味著 agent 發現的不是某個特定模型大小下的奇技淫巧，而是更通用的架構和訓練改進。

而且 Reddit 上有人指出一個更驚人的細節：agent 找到的這些改進，「比一個在這個領域有十多年經驗的專家（也就是 Karpathy 本人）做得更好」。

當然，這裡有個重要的 caveat：agent 做的改進主要是超參數和架構微調層面的。它不會提出全新的理論框架，不會發明 Transformer 這種東西。但在「給定一個訓練腳本，把它調到最好」這件事上，暴力搜索 + AI 直覺確實可以超越人類專家。

---

## 社群炸了

AutoResearch 的傳播速度，即使以 AI 圈的標準來看也算離譜。

Karpathy 在 X 上發了那條推之後， **兩天內累積了 860 萬次瀏覽** 。整個科技圈開始用 "Karpathy Loop" 來稱呼這個自動實驗迴圈的概念。 [VentureBeat 的報導](https://venturebeat.com/technology/andrej-karpathys-new-open-source-autoresearch-lets-you-run-hundreds-of-ai) 直接用了「revolutionary implications」這個詞。Shopify CEO Tobi Lutke 也在推特上表達了高度關注。

Reddit 上至少三個子版炸開了討論：

- **r/LocalLLaMA** ：232 upvotes、84 條評論，技術討論為主
- **r/singularity** ：對「early singularity」的措辭有激烈辯論
- **r/AgentsOfAI** ：232 upvotes，聚焦在 agent 框架的設計

但社群反應也不是一面倒的歌頌。有幾個值得記錄的批評聲音：

**「這概念不新啊」** — 有 Reddit 用戶指出，類似的多 agent 研究框架（比如 `james_library` ）早在 Karpathy 之前就已經開源了。這話不算錯，但多數人的回應也很直接：重點不是誰先做，而是誰做到了讓大家真正注意到。630 行代碼的極簡設計 + Karpathy 的名人效應，讓這個概念從小圈子裡的實驗變成了全球討論。

**「範圍太窄了吧」** — 目前 AutoResearch 只能優化小型 GPT 模型的訓練。它不能做 CV、不能做 NLP 下游任務、不能做多模態。這確實是事實。但 Karpathy 的定位很明確：這是一個概念驗證，而不是一個通用框架。

**「Karpathy 越來越奇怪了」** — r/singularity 有用戶對 Karpathy 越來越頻繁地使用 "singularity" 相關措辭表示不安。怎麼說呢，這更像是對整個 AI hype 文化的疲勞感，不完全是對 AutoResearch 本身的批評。

---

## 更大的野心：當一個 PhD 變成一個研究社群

AutoResearch 發佈幾天後，Karpathy 在 X 上拋出了他的下一步構想：

> *"The next step for autoresearch is that it has to be asynchronously massively collaborative for agents (think: SETI@home style). The goal is not to emulate a single PhD student, it's to emulate a research community of them."*

翻譯：現在是一個 agent 在一張 GPU 上跑。下一步，是讓全世界的 agent 在各自的 GPU 上跑，然後把結果匯聚起來 — 就像當年 SETI@home 用全球志願者的電腦搜尋外星訊號一樣。

他甚至建了一個叫 AgentHub 的 repo（後來刪了，說還在開發中），願景是：多個 agent 在不同機器上認領實驗、跑完後發佈結果（不管成功或失敗）、拉取目前全域最佳的配置，然後繼續迭代。

社群反應更快。Karpathy 話音剛落：

- **`autoresearch-at-home`** 已經有人做了分散式版本的 fork
- **Hyperspace AI** 的 CEO 直接把它搬上了 P2P 網路
- 有人在 **Apple Neural Engine** 上嘗試跑
- 有人在用 **本地 LLM** （而不是 Claude/GPT-4）驅動 agent，想降低 API 成本
- **[autoresearchhub.com](http://autoresearchhub.com/)** 已經有人部署上線

這個速度，說實話，有點嚇人。從一個人的 side project 到一個全球分散式研究基礎設施的雛形，只用了不到一週。

但這裡有一個技術挑戰值得注意：目前 AutoResearch 的固定 5 分鐘時間預算意味著不同硬體的結果不能直接比較。在分散式場景下，你需要找到一種方式讓 H100 上跑出的實驗結果和 RTX 4090 上跑出的結果能夠有意義地匯聚在一起。這不是一個小問題。

---

## 這件事真正在說什麼

退一步看，AutoResearch 最有意思的地方不在技術細節，而在它暗示的角色轉變。

過去做 ML 研究，你的日常是：讀論文、想假設、寫代碼、跑實驗、看結果、調參數、再跑一次。你是 **實驗者** — 手在鍵盤上，眼睛盯著 loss 曲線。

AutoResearch 之後，你的工作變成：寫 `program.md` 。

就這樣。你的工作變成了用自然語言描述「我覺得值得探索的研究方向」，然後讓 agent 去執行。你從 **實驗者** 變成了 **實驗設計者** 。VentureBeat 的報導用了一個很到位的說法：

> *"The role of the human shifts from 'experimenter' to 'experimental designer.'"*

這跟我們在軟體開發領域看到的趨勢是一致的。隨著 AI coding assistant 越來越強，開發者的核心價值正在從「寫代碼的能力」轉向「定義問題和設計架構的能力」。AutoResearch 只是把同樣的趨勢推到了 ML 研究領域。

但我覺得最值得深思的，還是 frozen metric 這個設計。它提醒我們： **約束不是限制，約束是智慧。**

630 行代碼，兩個檔案，一個不能被修改的指標。Karpathy 沒有試圖建一個什麼都能做的 AI 研究平台，他建了一個「AI 只能做正確的事」的小世界。在 AI alignment 的大辯論裡，這可能比任何論文都更有說服力。

下次凌晨兩點如果你又在盯 loss 曲線，想想看：也許你真正該做的，不是調那個 learning rate，而是寫一份好的 `program.md` ，然後去睡覺。

---

## 延伸閱讀

- [karpathy/autoresearch GitHub Repo](https://github.com/karpathy/autoresearch) — 官方原始碼
- [VentureBeat: AutoResearch lets you run hundreds of AI experiments a night](https://venturebeat.com/technology/andrej-karpathys-new-open-source-autoresearch-lets-you-run-hundreds-of-ai) — 最完整的媒體報導
- [The Frozen Metric of Autoresearch](https://hybridhorizons.substack.com/p/the-frozen-metric-of-autoresearch) — Carlo Iacono 對 frozen metric 的深度哲學分析
- [MarkTechPost: 630-Line Python Tool for Autonomous ML Experiments](https://www.marktechpost.com/2026/03/08/andrej-karpathy-open-sources-autoresearch-a-630-line-python-tool-letting-ai-agents-run-autonomous-ml-experiments-on-single-gpus/) — 技術架構解析
- [Karpathy on X: SETI@home Vision](https://x.com/karpathy/status/2030705271627284816) — 分散式願景原文
- [r/LocalLLaMA Discussion](https://www.reddit.com/r/LocalLLaMA/comments/1rowp28/karpathy_autoresearch/) — Reddit 社群最熱烈的技術討論串
- [Getting Started Full Guide](https://medium.com/modelmind/getting-started-with-andrej-karpathys-autoresearch-full-guide-c2f3a80b9ce6) — 手把手上手教學