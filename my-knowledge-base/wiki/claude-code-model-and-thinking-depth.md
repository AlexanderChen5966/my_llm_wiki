---
title: 兩個旋鈕：選模型與調思考深度——「think hard」在 Claude Code 裡沒有用
date: 2026-09-01
tags: [ai/claude, ai/prompt-engineering]
source: https://ithelp.ithome.com.tw/articles/10406403（《Claude Code 實戰手冊》系列第七篇 G7）
type: article
status: processed
asset_value: 66
---

## T（核心論點）

Claude Code 有兩個**分開的**旋鈕，大部分新手裝好之後就再也沒打開過：**用哪一顆腦袋幫你做事（`/model`）、以及它願意花多少時間想（thinking / effort）。**

不調的後果不是「壞掉」，而是很微妙的兩件事：

> **改一個錯字，它派出最貴、最慢的模型慢慢深思；而真的需要它想清楚的架構問題，它卻可能想得不夠久。大砲打蚊子，蒼蠅拍打老虎。**

🔴 而本篇最值得記的是一個**很多教學都寫錯的地方**：

> **`think` / `think hard` / `think more` 在 Claude Code 裡只會被當成普通文字，不會加深推理。唯一被程式碼認得的關鍵字是 `ultrathink`。**

## B（適用場景）

**適用：** 設定 Claude Code 的預設模型；判斷單一任務要不要臨時加深推理；解釋為什麼「催它想久一點」的咒語沒效果。

**不適用 / 注意：**
- 🔴 **價目比例 ≠ 訂閱額度倍率**（見下表的警告）——這是本篇作者特別標注的誠實邊界。
- ⚠️ `/model` 選單的長相與操作方式**因介面而異**（終端機／VS Code 擴充／桌面版），有些地方 `/model` 只會印出目前狀態。打了沒跳選單是正常的，改用帶名字的寫法即可。

## R（操作步驟）

### 一、第一個旋鈕：`/model`

最保險的用法是**直接把名字打在後面**（`/model sonnet`），這種寫法在哪裡都能用。

| 選項 | 它是什麼 |
|---|---|
| `sonnet` | 一般同事。**日常寫程式的主力**——寫功能、修一般 bug 都很夠用，而且快 |
| `opus` | 資深的那位。**複雜推理**——架構決策、難纏的 bug、大範圍重構。想得深，但慢、也比較耗用量 |
| `haiku` | 手腳最快的。**簡單、重複的小事**——改錯字、調文案 |
| `opusplan` | **本篇最推薦**——規劃時用 Opus 想，真的動手做時自動換成 Sonnet |
| `default` | 清掉設定，回到帳號預設值 |
| `fable` · `best` | 最強的一批，給「一次做很久、很大」的任務。**新手先不用碰** |

**兩個容易漏掉的細節**：
- ⚠️ **用 `/model` 換掉之後它會記住**——之後開的新對話都會用你選的，不是只有當下這次。要改回來打 `/model default`
- `opus` / `sonnet` 這種名字是**別名**，會自動指向目前最新的一代，官方換代時你什麼都不用改

### 二、量級感：官方 API 價目比例

| 模型 | 官方 API 價目的比例 |
|---|---|
| `haiku` | **1x**（基準）|
| `sonnet` | 約 **2x** |
| `opus` | 約 **5x** |
| `fable` | 約 **10x** |

（輸入與輸出的比例一致，換算乾淨。）

> 🔴 **這張表是「價目比例」，不是「額度倍率」。** 數字來自官方公開的 API 價目表、可以查證；但走**訂閱制**（Pro／Max）時，**額度到底怎麼扣，官方並沒有公布確切倍率**。請當「量級參考」而非精算——重點不是「剛好五倍」，而是**「差一個等級就差好幾倍，不是差幾 %」**。

### 三、🔑 `opusplan`：如果只想選一個

> **在「規劃」階段用 Opus，開始「動手做」之後自動換成 Sonnet。**

為什麼聰明：一個任務裡，**難的通常是「想清楚要怎麼做」，而不是「照著做」**。規劃階段最需要那顆貴的腦袋；等方向定了、開始一個檔案一個檔案改的時候，Sonnet 完全做得來。結果是在最需要的地方用了最好的、在不需要的地方省了下來，而且不用手動切換。

> ⚠️ **它可能不在你的選單裡——要用打的。** `opusplan` 在圖形選單裡不一定看得到，但確實可以用（打 `/model` 時它自己印出的可用清單裡就有）。直接打完整指令：`/model opusplan`

### 四、第二個旋鈕：思考深度，以及那些沒用的咒語

**真的有效的三種手段**：

| 需求 | 做法 |
|---|---|
| 臨時想深一點（就這一題比較難）| 在訊息裡加 `ultrathink`（放哪裡都行）——**只在那一輪生效，不會改到你的設定** |
| 長期調整 | `/effort` |
| 乾脆不要它想（都是很簡單的雜事）| `Option` + `T` 關掉 thinking |

**沒用的**：`think`、`think hard`、`think harder`、`think more`、`think step by step`——這些**只會被當成普通文字送出去，Claude Code 不認得它們**。

> 🔑 **用設定，不要用咒語。**

⚠️ 作者也把界線畫清楚了：thinking 開關與 effort 等級是**真的設定**、確實有效；沒用的是「把某些字打在訊息裡能不能催它想更久」這件事。

### 五、訂閱制的人真正該在意的

> **設好一個合理的預設（例如 `opusplan`），然後忘掉它**——只在明顯不對勁的時候（小事想太久、難題想不出來）才伸手去轉旋鈕。

## C（關鍵行動）

- [ ] 不確定就 `/model opusplan`，別每次糾結（它在選單裡看不到，要打指令）
- [ ] 別再打 `think hard`——要這一輪想深就寫 `ultrathink`，長期調整用 `/effort`
- [ ] 記得 `/model` 的切換會**留到之後的新對話**，不是只有當次
- [ ] 引用成本差異時說「差一個等級差好幾倍」，不要把 API 價目比例當訂閱額度倍率
- [ ] 兩個旋鈕分開看：模型決定「誰來做」，thinking／effort 決定「想多久」

## 相關連結

- [[claude-model-selection-guide]] — 模型選型的既有整理；本篇補上 `opusplan` 的選單陷阱與 API 價目量級
- [[claude-opus-5-prompting-guide]] — effort 五階（low→max）與 `xhigh`／`max` 停用 thinking 的官方行為
- [[claude-fable-5-usage-guide]] / [[claude-fable-5-power-tasks]] / [[fable-5-delegation-cost-benchmark]] — Fable 的適用場景與方案可用性
- [[claude-code-billing-usage]] / [[claude-code-cost-optimization-7-tips]] — 用量與省錢
- [[claude-code-three-modes-distinction]] — Plan 模式的機制，`opusplan` 正是綁在這個模式邊界上
- [[claude-code-model-boomerang]] — 追蹤掉出主模型並一鍵切回
- [[chain-of-thought-reasoning]] — ⚠️ 對照：CoT 作為**提示技巧**在一般 LLM 上有效，但在 Claude Code 裡「催它想久一點」要走設定而非字面咒語
- [[claude-code-glossary]] — 指令與名詞速查
- [[claude-opus-5-5-release-notes]] — Opus 5.5 API 預設 effort 由 high 改為 medium，thinking 完全不能關
- [[claude-code-plan-mode-permission-rules-layering]] — 同系列 G8：`opusplan` 判斷『規劃階段』的依據就是是否在 Plan Mode
- [[claude-models-real-task-eval-tokyo-trip]] — 同系列番外：5 個模型的真實任務評測
