---
title: "不靠 Fable 5，日本最速獨角獸 Sakana 用多模型調度比肩頂尖 AI"
source: "https://www.inside.com.tw/article/41618-sakana-fugu-multi-model-orchestration-export-controls"
author:
  - "[[Claire]]"
published: 2026-06-23
created: 2026-06-24
description: "東京新創 Sakana AI 推出 Fugu 調度系統，以單一 API 指揮多款頂尖模型協作。#人工智慧,Sakana AI,多代理調度,Fugu,集體智慧,模型供應鏈 (sakana-fugu-multi-model-orchestration-export-controls)"
tags:
  - "clippings"
status: processed
---
東京 AI 新創 Sakana AI 推出名為 Fugu（日文「河豚」）的多代理（multi-agent）調度系統，主打以單一、相容 OpenAI 的 API，動態調度市面上多款頂尖模型協同作戰，宣稱在多項程式設計、推理與科學基準測試上達到前沿水準。這項產品鎖定的痛點：當企業與政府越來越擔心被單一模型供應商綁定，或因地緣政治出口管制突然失去存取權，一個能即時抽換底層模型的調度層，反而比任何單一模型更具韌性。

### 一個 API，指揮一整支模型艦隊

Fugu 本身是一個經過訓練的語言模型，扮演「指揮官」角色，學會判斷何時自己直接回答、何時把任務拆解後分派給池中的專家模型，再彙整各方輸出成單一答案。模型選擇、任務分派、結果驗證、回應合成等整套流程都在內部完成，對開發者而言只看到一個端點。Sakana 將產品分為兩個版本，標準版 Fugu 主打低延遲，適合日常寫程式、程式碼審查與聊天服務；高階版 Fugu Ultra 則動用更深的代理池，處理多步驟的複雜任務。其技術源於 Sakana 兩篇獲 ICLR 2026 收錄的論文 TRINITY 與 The Conductor，後者以強化學習讓模型自行摸索協調策略，而非由人工寫死路由規則。

公司取名也藏著隱喻，河豚是需要持照廚師才能處理的食材，料理不當足以致命，Sakana 想傳達的隱喻，正是把「手動管理一群 AI 代理」這件既脆弱又危險的工作，藏進一次乾淨的 API 呼叫背後。

### 數字漂亮，但要看是誰在報

Fugu 的賣點，是用集體智慧（collective intelligence）勝過單一模型。在軟體工程基準 SWE-Bench Pro 上，Fugu Ultra 拿下 73.7 分，明顯領先 Anthropic 的 Claude Opus 4.8（69.2）與 OpenAI 的 GPT-5.5（58.6）；在 LiveCodeBench、GPQA-Diamond（研究所程度的科學選擇題）等測試，Fugu 系列也多半壓過目前公開可用的頂尖模型。

不過這些數字目前由 Sakana 自行公布，競品分數則採用各家供應商自報數值，尚未經第三方獨立複現驗證，其中 SWE-Bench Pro 採用 mini-swe-agent scaffolding；而且 Fugu 是調度系統，它的分數可能來自呼叫 Opus 4.8、GPT-5.5 等模型再合成的結果，本質上是「模型的模型」成績，與單一模型不能直接畫上等號。另一方面，Sakana 並未宣稱贏過 Anthropic 最頂規的 Fable 5，官方用語是 Fugu Ultra 與 Fable 5、Mythos Preview「並駕齊驅」（shoulder-to-shoulder），僅對公開可用的模型聲稱領先。事實上在 SWE-Bench Pro 上，未列入比較池的 Fable 5 仍居各模型之冠（VentureBeat 列出的分數為 80.0），而 Fable 5 與 Mythos Preview 之所以缺席，正是因為兩者已不對外開放。

### 補上出口管制抽走的拼圖

Anthropic 於 6 月 12 日因應美國政府出口管制命令，撤回旗下最強的 Claude Mythos 5 與 Fable 5 公開存取權，對仰賴這些模型的開發者形同一夕失去關鍵依賴。

Sakana 共同創辦人暨執行長 David Ha（前 Google Brain 成員、Stability AI 研究主管）抓住目前 Fable 5 暫不對大眾市場開放的這個缺口，把 Fugu 定位成比任何單一供應商都更可靠的選項：當一家供應商斷線或突遭管制，調度層能動態換上其他模型，避免整套系統停擺。

定價上，Fugu Ultra 依用量計費（pay-as-you-go），每百萬 token 輸入 5 美元、輸出 30 美元，上下文超過 27.2 萬 token（272,000）後分別漲至 10 與 45 美元；另有每月 20、100、200 美元三檔訂閱方案，7 月底前訂閱可享次月免費。另一項限制是，礙於 GDPR 合規，Fugu 目前不在歐盟與歐洲經濟區（EEA）提供服務，但已於美國、英國等地上線。針對有資料駐留（data residency）與合規需求的企業，Fugu 也允許把特定供應商排除在代理池外。

### 日本選了第三條路

把視角拉高，Fugu 透露的是一種有別於中美的路線選擇；美國持續堆高單一巨型模型的規模，中國則以開放原始碼模型回應封鎖，Sakana 的賭注是，與其從零打造一座前沿模型，不如成為協調所有模型的那一層。這條路與這家由《Attention Is All You Need》共同作者 Llion Jones 與 David Ha 共創的公司一貫理念相符，他們向來主張未來不會是「贏家通吃」，而是大量各有專長的模型協作互補。

不過，調度系統的天花板，終究受限於池中模型的原始能力。當最強的 Fable 5 被排除在外，Fugu 能逼近卻難以超越前沿，這一點在 Sakana 自家的數據裡已寫得很明白。

核稿編輯：Mia

**加入 INSIDE 會員，獨享 INSIDE 科技趨勢電子報，** [**點擊立刻成為會員**](https://member.inkmaginecms.com/?console_id=1&client_id=9da69691-a221-4a2b-945c-c845c9999070&redirect_uri=https%3A%2F%2Fwww.inside.com.tw%2Fmember%2Fprocess) **！**

延伸閱讀：

- [日本 AI 新創新星 sakana.ai 首次來台！這家獨角獸如何跟中美巨頭抗衡？](https://www.inside.com.tw/article/39125-sakana-ai-aws-summit-taipei)
- [AI 也要天擇？Google Transformer 作者的新創 Sakana AI「演化」出三款模型](https://www.inside.com.tw/article/34548-sakana-ai-three-models)
- [日本 AI 新創公司「Sakana AI」獲 NVIDIA 投資，成為日本史上最快獨角獸](https://www.inside.com.tw/article/36122-nvidia-sakana-aI)