---
title: "Harness：決定 AI Agent 成敗的不是模型，是這個東西"
source: "https://yu-wenhao.com/zh-TW/blog/ai-harness/"
author:
  - "[[WenHao Yu]]"
published: 2026-03-16
created: 2026-05-15
description: "同一個 AI 模型，只換外面的系統，排名從 30 名外跳到 Top 5。2026 年 AI 圈最重要的共識不是哪個模型最強，而是 Harness — 包在模型外面、把原始能力轉化為可靠產出的那套系統。這篇文章用馬具的比喻，解釋什麼是 Harness、為什麼它比模型更重要。"
tags:
  - "clippings"
status: processed
---
🌐

這篇文章也有英文版本 [Read in English →](https://yu-wenhao.com/en/blog/ai-harness/)

同一個 AI，同樣聰明，但換一套系統包在外面，表現判若兩人。

這件事大多數人都體驗過 — 用 ChatGPT 問問題，有時候很厲害，有時候一塌糊塗。同一個模型，差別到底在哪？

2026 年，AI 圈裡終於有了一個共識： **決定 AI 能不能可靠交付成果的，不是模型本身，是模型外面包著的那套系統。**

他們把那套系統叫做 **Harness** — 馬具。

---

## 我們一直在比的，是對的東西嗎？

對 AI 有興趣的人，過去一年大概都參與過這種討論：

「GPT 還是 Claude 比較強？」「Gemini 追上來了嗎？」「o3 的推理能力是不是比 Sonnet 好？」

看排行榜、比較各家模型的分數、在不同工具之間換來換去 — 這些事我們都做過。

但 2026 年，一個共識正在 AI 圈裡成形。

Sean Goedecke 是 GitHub 的工程師，他直接參與了 GitHub Copilot 的開發。他說了一段話：

> 「近期很多進步，不只是模型變強了。是模型外面那套系統改善了。」

Martin Fowler — 軟體工程界的教父級人物 — 在 2026 年 2 月寫了一篇文章， [把這個東西命名為 Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html) 。

Anthropic（Claude 的開發商）發了一篇專文叫 [Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) 。

OpenAI 的 Codex 團隊用這套方法生成了超過 100 萬行程式碼，零手寫。

大家都在講同一件事。他們都在說： **決定 AI 能不能可靠地完成工作的，不是模型本身，是外面包著模型的那個東西。**

他們把那個東西叫做 **Harness** 。

---

## Harness 這個字從哪來？

Harness 的原意是馬具 — 套在馬身上、把力量導向特定方向的裝備。韁繩、馬鞍、胸帶、眼罩。一匹馬的原始力量很強大，但沒有馬具，它可能往任何方向跑、被路邊的聲音嚇到、跑到一半停下來吃草。馬具不會讓馬變得更強壯，它做的是： **讓馬的力量可靠地轉化為有用的工作。**

AI 模型也一樣。Claude Sonnet 4、GPT-4.1、Gemini 2.5 Pro — 這些模型的原始認知能力非常強大。但如果只是開一個 ChatGPT 聊天視窗，丟一個問題進去，得到的是一匹沒有韁繩的馬 — 有時候跑得很好，有時候跑偏了，有時候原地繞圈。多數人應該體驗過：

- 同一個問題問兩次，得到完全不同的答案
- AI 很有自信地給出錯誤的資訊
- 做到一半突然忘了前面的脈絡
- 給的程式碼能跑但有 bug，而它自己不知道

這些問題 **不是模型不夠強** ，是因為模型在裸跑，沒有馬具。

> 模型是引擎，harness 是整台車。

---

## 什麼是 AI Harness？

一句話： **包在 AI 模型外面、把原始認知能力轉化為可靠產出的整套系統。**

注意「整套系統」四個字。Harness 不是一段 prompt、不是一條指令、不是一個設定。它是一整個技術堆疊。

很多人問過一個問題：「為什麼 Claude Code 感覺比網頁版的 Claude 聰明那麼多？是不是用了不同的模型？」

答案是： **同一個模型。** Claude Chat 和 Claude Code 可以跑一樣的 Sonnet 或 Opus，差別完全在 harness。

網頁版的 Claude 收到一段文字，回一段文字，結束。但 Claude Code 在背後多做了一整套事：

1. 先讀取整個專案結構，理解脈絡
2. 根據指令決定要做什麼
3. 用工具（讀檔案、寫檔案、跑指令）實際執行動作
4. 檢查結果對不對
5. 如果不對，自己修正再來一次
6. 過程中遵守一組規則（不能刪重要檔案、不能洩漏密鑰…）

這整套流程，就是 harness。同一顆大腦，一個在考場裡裸考，一個有筆記、有計算機、有草稿紙、有檢查流程。產出品質自然不同。

---

## Harness 由哪六層組成？

Harness 聽起來抽象，但它可以拆成六個具體的層次：

| 層次 | 白話翻譯 | 在做什麼 |
| --- | --- | --- |
| 1\. 迴圈（Loop） | 不斷循環 | 觀察 → 決定 → 行動 → 驗證 → 更新，不斷循環直到完成 |
| 2\. 工具（Tools） | 能動手 | 讓 AI 不只能說話，還能讀檔案、跑程式、call API、搜尋網頁 |
| 3\. 上下文（Context） | 看到什麼 | 控制 AI 看到哪些資訊、不看哪些，避免資訊過載 |
| 4\. 持久化（Persistence） | 記得住 | 跨次對話、跨次執行，記得之前做過什麼 |
| 5\. 驗證（Verification） | 會檢查 | 做完之後跑測試、檢查語法、自我審查，確保品質 |
| 6\. 限制（Constraints） | 有邊界 | 哪些事不准做、哪些檔案不能碰、花費上限多少 |

**這六層全都是系統工程的改進，不是模型的改進。** 不需要等下一代模型出來，就可以讓現在的 AI 表現更好 — 只要把 harness 做好。

最近爆紅的 OpenClaw 就是一個好例子。OpenClaw 本身不是模型 — 它背後跑的是 Claude、GPT 或其他 LLM。OpenClaw 賣的其實就是一整套 harness：SOUL.md 定義角色和限制條件（第六層），Memory 機制讓 agent 跨對話記住上下文（第四層），Agent Loop 讓它持續運行接收指令（第一層），Shell 和 API 工具讓它能動手做事（第二層）。

而且 harness 可以疊加。OpenClaw 提供了基礎 harness，使用者還可以透過 SKILL.md 把自己的工作流程寫成 SOP，讓 AI 按照既定模式執行，而不是每次都從頭理解模糊的指令。SKILL.md 寫得越精準，AI 的執行品質就越穩定 — 這也是 harness 設計功力的體現。

最好的證明來自 LangChain 在 2026 年 2 月做的一個實驗。他們拿自家的 coding agent 去跑業界基準測試，成績是 52.8%，排名在 30 名之外。然後他們什麼都沒換 — 同一個模型、同一組 API — 只改了 harness。結果：66.5%，排名 [跳到 Top 5](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/) 。他們具體改了什麼？

1. **加了自我驗證迴圈** — 在 AI 說「我做完了」之前，強制它跑一輪 checklist 確認
2. **注入環境上下文** — 開始工作前，先把目錄結構掃一遍餵給 AI
3. **防迷路機制** — 偵測 AI 是否在重複編輯同一個檔案（陷入迴圈）
4. **調整推理預算** — 規劃和驗證時給多一點思考額度，實作時少一點
5. **失敗分析** — 自動分析每次失敗的模式，歸納改進方向

沒有一項是「換個更強的模型」。全部都是 harness 層面的改進。

Vercel 的案例更直接。他們把 AI agent 可以用的工具從 15 個砍到只剩 2 個。結果？

準確率從 80% 升到 100%。Token 用量少了 37%。速度快了 3.5 倍。

少即是多。限制本身就是 harness 的一部分。

---

## 為什麼 2026 年所有人突然都在談 Harness？

Harness 不是新概念，但為什麼所有人突然都在談？因為模型夠強了。2024 年之前，模型本身的能力就是瓶頸，再好的 harness 也拯救不了一個連基本邏輯都搞不定的模型。2025 年，GPT-4.1、Claude Sonnet 4、Gemini 2.5 Pro 把基本能力拉到了夠用的水準。

然後大家發現： **模型之間的差距越來越小，但同一個模型在不同 harness 下的表現卻天差地遠。** 兩個產品明明用同一個模型，體驗起來卻像完全不同的物種 — 不同的節奏、不同的穩定度、不同的質感。同一顆大腦，不同的 harness。

這就是為什麼 Aakash Gupta 寫了一篇文章叫： [「2025 年是 Agent 年，2026 年是 Agent Harness 年」](https://aakashgupta.medium.com/2025-was-agents-2026-is-agent-harnesses-heres-why-that-changes-everything-073e9877655e) 。

前沿正在從「prompt engineering」轉向 **「harness engineering」** — 設計迴圈、工具鏈、驗證流程、上下文策略的系統工程。

---

## Harness 跟我們的人生有什麼關係？

這不只是技術觀察。不寫程式、不碰 AI 工具的人，也能從 harness 的概念中得到啟發。想想這個類比：

- **大腦** = 模型（原始認知能力）
- **環境、習慣、工具、回饋機制** = harness

大多數人想變得更有生產力時，會怎麼做？閱讀更多、學更多技能、鍛鍊意志力 — 這些都是在試圖升級模型。

但 harness 的觀點告訴我們： **最快的改變不是升級大腦，是升級環境。**

把手機放到另一個房間，專注力立刻提升。不是因為意志力變強了，而是環境的限制條件（harness 的第六層）改變了。設計一個每天固定時間寫作的流程，產出會變穩定。不是因為突然變得更有才華，而是迴圈（harness 的第一層）建立了。

不自己設計 harness，就會活在別人設計的系統裡。社群媒體的演算法、公司的 KPI 制度、手機的通知系統 — 全都是別人設計的 harness，而且不是為了我們的目標設計的。我之前寫過 [打造個人全景監控系統](https://yu-wenhao.com/zh-TW/blog/personal-panopticon/) ，核心概念一樣：主動設計自己的資訊環境，而不是被動接受別人餵給我們的東西。

> 如果對「用 AI 打造自己的 harness」有興趣，我在 **5/16（六）** 有一場 [實體工作坊](https://www.solo.tw/courses/ai-command-center) ，6 小時帶你從零建立個人的 AI 指揮中心 — 不需要會寫程式。 [了解更多 →](https://www.solo.tw/courses/ai-command-center)

---

## 一個 Agent 的 Harness 做好了，然後呢？

一個 agent 的 harness 做好了，下一個問題來了 — 如果不只有一個 AI agent，而是有十個呢？誰做什麼？誰跟誰報告？預算怎麼分？品質誰把關？有人做重複的事怎麼辦？有人卡住了怎麼辦？

這些問題聽起來很熟，因為它們就是每一間公司、每一個團隊 leader、每一個 PM 每天都在處理的問題。只不過現在，員工變成了 AI。

我在下一篇文章裡實際試了 — [我建了一間 AI Agent 公司，第一天就想解散它](https://yu-wenhao.com/zh-TW/blog/multi-agent-hidden-cost/) 。把真實公司的管理方法搬到 AI agent 上，會發生什麼事？劇透：流程本身變成了瓶頸。

#AI #agent harness #harness engineering #agentic harness #agentic coding #Claude Code #AI Agent

## 常見問題

AI Harness 是什麼？

Harness 是包在 AI 模型外面、把原始認知能力轉化為可靠產出的整套系統。原意是馬具 — 套在馬身上，把力量導向特定方向的裝備。在 AI 語境下，模型是引擎，harness 是整台車。包含六個層次：迴圈、工具、上下文管理、持久化、驗證、限制條件。

為什麼 Harness 比模型重要？

LangChain 用同一個模型（gpt-5.2-codex），只改 harness，在 Terminal Bench 2.0 的成績從 52.8% 跳到 66.5%，從 Top 30 外跳到 Top 5。Vercel 把 agent 工具從 15 個砍到 2 個，準確率從 80% 升到 100%。真正的差異不在模型，在系統設計。

Harness Engineering 是什麼？

Martin Fowler 在 2026 年 2 月命名的新領域 — 不是寫 prompt，而是設計迴圈、工具鏈、驗證流程、上下文策略的完整系統工程。OpenAI Codex 團隊用這套方法，讓 AI agent 生成了超過 100 萬行程式碼，零手寫。