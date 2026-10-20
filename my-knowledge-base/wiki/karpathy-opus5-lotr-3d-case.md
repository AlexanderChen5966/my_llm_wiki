---
title: Karpathy 的《魔戒》3D 實驗：10 美元、兩小時、5,500 行程式碼，也暴露「AI 看不到自己作品」
date: 2026-08-03
tags: [ai/claude, ai/agent, type/concept]
source: https://www.bnext.com.tw/article/91711/karpathy-claude-opus-lord-of-the-rings
type: article
status: processed
asset_value: 62
---

Karpathy 2026-08-02 在 X 貼出的實驗：給 Claude Opus 5 一段《魔戒》開頭文字 ＋ 100 萬 token 預算（約 10 美元），要它用 three.js 把場景渲染出來。Opus 5 跑約兩小時、寫出 **5,500 行程式碼**，用程序化生成長出一個能在瀏覽器裡走動的《魔戒》世界（[試玩與原始碼](https://karpathy.ai/lotr-movie/)）。

## T（核心論點）

這個案例真正的價值不是「AI 會寫 3D」，而是**同時示範了能力躍進與能力缺口**：

- **能力面**：模型沒有建模軟體介面，只能靠寫 JavaScript 算出每塊多邊形落在 (x, y, z) 的哪個位置、彼此相對關係如何，再寫出動畫邏輯——**空間推理靠純程式碼完成**。
- **缺口面**：LLM **無法有效且原生地「看」影片，也不能親自把遊戲玩一遍**，所以沒辦法審核自己的成品。Opus 5 的替代方案是在不同時間點截圖，再從靜態圖反推動態場景哪裡出錯——Karpathy 形容「非常緩慢且費力」，中途搞砸好幾次。

> 差別不在模型會不會寫程式，而在它缺少人類設計師最基本的那道回饋迴圈：**做完之後看一眼，發現不對再改。**

Karpathy 把「多模態感知」與「遊戲操作」列為仍相當欠缺的原始能力。

## B（適用場景）

**適用：**
- 評估「AI 能不能做 X」時，用來校準判斷：**產出能力 ≠ 驗收能力**，缺少感知回饋迴圈的任務品質天花板明顯較低
- 需要向他人說明「為什麼 AI 做出來的 UI／視覺／遊戲總是差一截」時的具體案例
- 思考長時任務的成本結構（100 萬 token ≈ 10 美元換兩小時自主工作）

**不適用 / 注意：**
- 這是**單次個人實驗**，非系統性評測；Karpathy 自評成果「有點粗糙但很有趣」
- 「5,500 行」「兩小時」「10 美元」是他自述的單次數字，不可當作可複現基準

## R（三個可帶走的觀察）

### 1. 評測題型正在換代：從鵜鶘 SVG 到長時多輪任務

Karpathy 開頭寫道，業界「正開始脫離那種你會用『畫一隻騎自行車的鵜鶘的 SVG』來測試 LLM 的階段」。

| | 鵜鶘 SVG 測試（Simon Willison） | 《魔戒》類任務 |
|---|---|---|
| 形式 | 一次性生成 | 反覆推理與修改 |
| 考的是 | 無視覺回饋下的構圖合理性 | 能否**持續執行、維持程式結構一致、依截圖回饋反覆修正** |

### 2. 成本結構的轉折：「沒人會做這種事」→「幾乎是免費的」

Karpathy 真正興奮的點在這裡——沒有人會花時間寫這麼客製化的東西，「但 LLM 擁有世界上所有的耐力和耐心」。他設想的下一步是超客製化娛樂：玩家被丟進任何一個故事裡，當《魔戒》的路人 NPC 或其中一個角色，「像是隨選的短暫 GTA 版本」。

### 3. 專家回饋補上模型不知道的效能常識

three.js 官方帳號在留言區建議「應該教 AI 盡可能使用**實例化網格（instanced meshes）**」——同一模型重複出現很多次時大幅節省效能的做法。Karpathy 把這條回饋轉給了 Opus，「長知識了」。這是個小但重要的觀察：**領域效能慣例是模型的常見盲點，靠提示補進去比期待它自己想到更可靠。**

## C（關鍵行動）

- [ ] 交給 AI 的視覺／動態產出任務，**自己保留最後那道「看一眼」的驗收**，不要期待它能自我審核
- [ ] 需要模型自我修正的長任務，主動提供可機器讀取的回饋管道（測試、log、結構化 diff），比只給截圖有效
- [ ] 有領域效能慣例（如 instanced meshes、索引策略、批次寫入）時，直接寫進提示或 CLAUDE.md，別假設模型會想到

## 📌 作者身分注記：Karpathy 已在 Anthropic（2026-05）

本知識庫的 `karpathy-*` 系列多以「OpenAI 創始成員／Tesla 前 AI 總監」介紹作者。**補上一項事實**：

> Karpathy 已於 **2026 年 5 月加入 Anthropic 的預訓練團隊**（來源：[TechCrunch](https://techcrunch.com/2026/05/19/openai-co-founder-andrej-karpathy-joins-anthropics-pre-training-team/)）。

⚠️ **對解讀既有條目的影響**：他測 Claude Opus 5 時，**測的是自己新東家的模型**。這不代表結論不可信（他在本篇同時點出模型的致命弱點，並非一味推崇），但涉及模型優劣比較的 Karpathy 發言，**應把利益相關性納入判讀**。目前 `karpathy-*` 系列僅 [[karpathy-voice-prompting]] 已註明此身分變動，其餘 6 篇仍只寫舊頭銜——不逐篇改寫，以本節作為統一注記。

## 相關連結
- [[synthesis/synthesis-karpathy-llm-wiki-method]] — **本篇為其來源之一**（2026-09-03 重跑）：🔴 本篇的「LLM 無法自審成品」是該精粹「兩層 Lint 必須分開」的機制層依據
- [[claude-opus-5-prompting-guide]] — Opus 5 提示詞官方調校建議；本篇是同一模型的長時任務實測案例
- [[karpathy-voice-prompting]] — Karpathy 語音碎念法（**唯一已註明 Anthropic 身分**的既有條目）
- [[karpathy-no-more-coding]] — Karpathy 對「不再寫程式」的看法，與本篇「LLM 有世界上所有的耐力」呼應
- [[karpathy-autoresearch]] — Karpathy 630 行 AutoResearch，同屬他的小型自造實驗系列
- [[agentic-engineering-mindset]] — 品味與驗收能力的重要性；本篇為「AI 缺回饋迴圈」提供具體例證
- [[claude-code-review-harness-principles]] — 驗證迴圈設計，對照本篇「模型無法自審」的結構性成因
- [[ai-code-review-safeguards]] — AI 產出的驗收護欄，同一命題的工程解法
- 📎 2026-10-19：[[claude-code-pure-code-animation-practice]] — 同一缺口的工具化解法：先請 Claude 寫渲染小工具輸出截圖總表＋連續格膠卷，讓模型自己看樣片再改（rar.design 作者實測）
- 📎 2026-10-19：[[ideas/ai-3d-code-structure-vs-appearance-acceptance]] — 3JSBench（**單一社群來源**，僅 X 討論串、無排名）把「產出 ≠ 驗收」做成 100 任務／697 項結構確定性檢查，外觀另交偏好投票，兩者不混成總分

## 原始摘要

數位時代（李先泰，2026-08-03）整理 Karpathy 2026-08-02 X 貼文。含實驗參數（Opus 5、three.js、100 萬 token／約 10 美元、約兩小時、5,500 行）、程序化生成手法、three.js 官方 instanced meshes 建議、鵜鶘 SVG 測試沿革（Simon Willison）、以及對「多模態感知與遊戲操作仍欠缺」的判斷。bnext 業配與延伸閱讀段落已略。
