---
title: ChatGPT Prompt 四步驟框架：Task / Context / Output / Iteration
date: 2026-07-17
tags: [ai/prompt-engineering, ai/workflow]
source: https://tools.wingzero.tw/article/sn/3984
type: article
status: processed
asset_value: 62
---

整理自 DeTools 對 OpenAI 官方 Prompt Engineering 指南的中文化教學。核心不是背「萬用 Prompt」，而是建立一套可重複的溝通流程：明確任務（Task）→ 提供背景（Context）→ 指定輸出（Output）→ 持續修正（Iteration）。OpenAI 官方強調「沒有完美 Prompt，把 ChatGPT 當同事不斷對話修正」才是關鍵心法。

> **與 [[gpt-5-6-prompting-4-elements-framework]] 的差異**：本篇是**新手通用對話**的四步驟框架，第 4 步「Iteration（持續修正）」是核心心法；那篇綁定 **GPT-5.6 Sol 特定模型**，主張「別微管理強模型」、第 4 要素是「Boundaries（邊界）」而非迭代。輸出以程式碼/一般對話為主看本篇，用 GPT-5.6 Sol 跑複雜任務看那篇。

## T（核心論點）
Prompt 品質而非模型差異決定回答好壞。與其尋找「萬能 Prompt」，不如掌握四步驟框架把需求講清楚，並透過反覆追問逐步逼近理想結果——Prompt Engineering 是一個「反覆改善的過程」，不是固定格式。

## B（適用場景）
- **適用**：一般 ChatGPT/Claude 對話任務（寫作、資料分析、簡報、翻譯、會議紀錄），新手建立提示詞基本功
- **不適用**：需要機器可讀規範檔的 agent 工作流（那屬於 [[agent-era-prompt-structure]] / [[gpt-5-6-prompting-4-elements-framework]] 的任務邊界層次）；一次性簡單改動硬套框架反而浪費

## R（四步驟框架 + 官方最佳實務）
**四步驟骨架：**
1. **Task（明確任務）**：用動詞開頭（Plan/Draft/Summarize/Compare），說明「做什麼、給誰、為什麼」
2. **Context（提供背景）**：AI 不知道你的身份/公司/目的，背景越完整回答越貼合；可附文件/PDF/Excel/圖片
3. **Output（指定輸出）**：明確格式（Markdown/JSON/CSV/表格/Email/簡報），與字數、口吻、讀者對象
4. **Iteration（持續修正）**：第一次回答→修正需求→追問→再改善（「再專業一點」「改成主管版本」「縮短一半」「改成表格」）

**官方五條最佳實務：**
- 大任務拆小步（Break big tasks into smaller steps），別一次要求做完所有工作
- 精確但別過度複雜（Be specific but keep it simple），過多資訊反而干擾
- 一次要多種方案（Ask for options）：「請提供三種風格，各附適合情境」
- 設定優先順序（Set priorities）：告訴 AI 最重要的是正確性/創意/可讀性/簡潔哪一項
- 避免常見錯誤：只說「幫我整理」卻不給對象/格式/長度/情境

## C（關鍵行動）
- [ ] 把 Task/Context/Output 三塊做成罐頭模板，下指令前先花 30 秒填一填
- [ ] 得到回答後別重開對話，用 Iteration 追問修正（同一對話補脈絡最有效）
- [ ] 一次性小改動不必硬套框架，一句話講清楚即可

## 相關連結
- [[gpt-5-6-prompting-4-elements-framework]] — GPT-5.6 官方 4 要素（目標/背景/輸出/邊界），模型越強步驟越精簡的進階版
- [[agent-era-prompt-structure]] — Agent 時代 Prompt 五要素，強調任務邊界與 Checkpoint
- [[codex-prompt-4-elements-agents-md]] — 把同一套 4 要素心法套進 Codex 寫程式工作流
- [[synthesis-prompt-engineering-evolution]] — 提示詞演進三次認知躍遷（指令細節→結果契約→評估驅動）
