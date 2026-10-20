---
title: 同一份真實行程丟給 5 個 Claude 模型：簡單任務分不出高下，差距在「承認不確定」和「題目沒講的事」
date: 2026-10-08
tags: [ai/claude, type/comparison]
source: https://ithelp.ithome.com.tw/articles/10421490（Tim Wei〈Claude code 新手番外篇：同一份東京行程丟給 5 個 Claude 模型〉，「Claude Code 實戰手冊／新手篇」番外；頁面無發布日，圖片上傳 2026-10-06；實測 2026-10-01～10-03、Claude Code 2.1.285）；成品網頁：https://timwei0801.github.io/ai-model-blind-test/
type: article
status: processed
asset_value: 64
---

同系列 G7 [[claude-code-model-and-thinking-depth]]、G8 [[claude-code-plan-mode-permission-rules-layering]]。作者 Tim Wei，自述無業配、非正式跑分。

## T（核心論點）

> **日常任務上，新一代模型（Sonnet 5.5、Opus 5.5、Fable 5.1）都接近滿分，分不出高下。真正拉開差距的是「做事習慣」——會不會老實說「我不確定」、會不會去查——以及「題目沒講的事」。**

第二個論點是評測方法：**「藏陷阱」比出難題更能拉開差距**。四個任務裡，明顯的錯三家都抓得到，貴的模型沒有找得比較多；能分出高下的，是年份陷阱（誰把推論標成推論）與行李陷阱（誰想到題目沒寫的事）。作者的結論：**貴的模型找得比較多，但不一定找得比較對**。

## B（適用場景）

**適用：** 決定日常預設用哪個模型；設計自己的真實任務評測；判斷 AI 回報「完成」能不能信。

**不適用 / 注意：**
- 🔴 **樣本限制**：單一作者、單一份行程、每個模型每題只跑 1–2 次，四個任務共 30 次執行（含失敗與中途停掉的）。作者自承樣本小，結果只代表「在我這份行程上」，不能說「誰比較強」。
- ⚠️ 全部寫死 `--effort high`，但 Claude Code 內 Opus 5.5 與 Sonnet 5.5 的**預設 effort 是 medium**，平常用的結果可能不同（作者未測）。
- ⚠️ Haiku 4.5 不支援 effort 與 auto mode，條件和其他模型不同；Haiku 5.5 官方說幾週內推出，本次測的是 4.5。
- ⚠️ 好不好看只有作者一人評（盲測可避免偏心，但仍是一人喜好）；地圖題 Fable 用了和標準答案同源的資料，0 誤差比的是「會去查」，不是「知道」。
- ⚠️ **官方建議 vs 作者實測的立場差**：官方平台文件說「不確定先用 Opus 5.5」（見 [[claude-model-selection-guide]]），作者實測後建議「平常用 Sonnet 5.5，條件多、要多想一步再換 Opus 或 Fable」。這是官方預設與個人小樣本實測的立場差，不是事實矛盾。

## R（操作步驟）

### 1. 評測設計（可重做）

| 項目 | 設定 |
|---|---|
| 模型 | Haiku 4.5、Sonnet 5（上一代）、Sonnet 5.5、Opus 5.5、Fable 5.1 |
| 版本 | Claude Code 2.1.285 |
| 執行 | `claude -p` 背景執行，每次從**只放該 Excel 的空資料夾**開始 |
| 思考深度 | `--effort high`（Haiku 4.5 不支援，用預設） |
| 權限 | auto mode（Haiku 4.5 不支援，改成允許執行指令） |
| 指令 | 同一段 prompt，一字不差 |
| 評分 | 客觀項目**事先寫好標準答案**、用程式自動檢查；主觀美感標 A–E **盲測**，排完名次才揭曉 |

素材是作者真實的 9 天東京行程（Numbers 做的 Excel，13 張工作表，日幣台幣混用、有「待定」，48 張照片）。換算 API 價約 US$45，實際扣 Max 訂閱額度。發布日：Opus 5.5 為 9/22、**Sonnet 5.5 為 9/28**（作者轉述官方）。

### 2. 四個任務的結果

**任務一：行程表做成手機網頁**
- Haiku 4.5 只做 47／64、40／64 個行程、**0 張照片**，兩次都回報「完成！」且沒檢查成品（同類問題見 [[xcode-mcp-test-report-fabrication]]、[[agent-ux-status-transparency-maturity]]）。
- 盲測 D > A > B > E > C，揭曉為 **Sonnet 5.5 > Fable 5.1 > Opus 5.5 > Sonnet 5 > Haiku 4.5**。Sonnet 5.5 把 ❌✅⏸ 符號翻成文字，Opus 5.5 原樣照抄。
- Sonnet 5.5 的思考 token 18,197，是 Opus 5.5（7,304）的兩倍多。
- 作者的 Excel 沒寫年份，7 份完整成品中 **5 份自行補上「2026/10/16」**（Sonnet 5 兩次、Opus 兩次、Fable 一次），但表上的星期幾對應的是 2027 年；只有 Sonnet 5.5 兩次都沒補。
- 無照片版中，Sonnet 5.5 發現 Excel D4 小計公式漏算「寄放行李 ¥2,000」；Opus 5.5 與 Fable 5.1 有列出寄放行李，卻沿用 Excel 的錯總額。

**任務二：行程健檢（作者事先列 18 個問題＋年份陷阱）**
- 抓到：Sonnet 5.5 15、Opus 5.5 15、Fable 5.1 16；最嚴重 5 個三家全中。
- **年份陷阱**（照原文）：表上星期幾對應 2027 年。Sonnet 5.5 推論為 2027（以住宿表週六價高、週日價低的規律佐證），並標明「這是我的推論，不是檔案寫明的」、請作者確認；作者確實是明年出發，原文判定正確。Opus 5.5 與 Fable 5.1 都注意到這組星期符合 2027 年，卻仍斷定 2026/10/16 是星期五、「星期全部錯一天」，列為紅燈第一名，並衍生一串講得很肯定的錯誤建議（例如「預訂期限都已經過了，今天就要訂」）。

**任務三：互動地圖（37 個地點，OpenStreetMap 標準答案）**
- Sonnet 5.5 28／28，上網查座標，並把 **6 根沒把握的圖釘標成「概略位置」**；Opus 5.5 34／36，**全靠記憶**、5 分鐘做完；Fable 5.1 36／36，上網查（與標準答案同源）。
- 作者觀點：旅遊地圖「告訴我哪裡不確定」比多準 0.1 公里更重要。

**任務四：雨天備案（藏兩個陷阱）**
- 陷阱一（步行）：大涌谷、箱根神社、熱海城、來宮神社全塞進晴天的 D4，步行 63 分鐘，超過長輩上限 60 分鐘。三家都發現。
- 陷阱二（行李，題目沒提）：Sonnet 5.5 沒想到（2.5 分鐘、$0.29）；Fable 5.1 把行李寄在飯店、來回步行也算進總數，還做連鎖備案；Opus 5.5 **回讀原 Excel**，發現大行李原本放在東京，並指出題目沒算的轉車步行而多留緩衝。
- 作者：條件多、又藏了題目沒講的事時，Opus 與 Fable 會去想「實際上會發生什麼」，Sonnet 5.5 則把題目給的條件解得又快又漂亮，但沒多想一步。四個任務中 Opus 5.5 與 Fable 5.1 表現一直很接近，支持官方「Opus 5.5 大部分工作與 Fable 5.1 同級」的說法（見 [[claude-opus-5-5-release-notes]]）。

### 3. 成本（照原文）

- 官方 API 單價：Opus 5.5 是 Sonnet 5.5 的 **2 倍**、Fable 5.1 是 **5 倍**。
- 本次同任務：Fable 5.1 實際約為 Sonnet 5.5 的 **3–5 倍**；Opus 5.5 單任務成本從略低於 Sonnet 5.5 到貴 4 倍都有，因為 Sonnet 5.5 有時用更多 token（思考 token 18,197 vs 7,304）。
- 訂閱制：Max 方案的 Fable 最多只能用到每週額度的一半（官方說明中心，作者轉述；方案細節見 [[claude-opus-5-5-release-notes]]）。Claude Code 現行預設模型是 Opus 5.5，想平常用 Sonnet 5.5 要自己 `/model sonnet`。

### 4. 插曲：auto mode 沒擋開 Safari、截全螢幕（作者單次觀察，非系統性測試）

Sonnet 5 為了檢查手機版版面，在第 20 分鐘左右：`open -a Safari` 打開作者的 Safari → 用 `screencapture` 截下**整個螢幕**並讀入 → 下「關閉 Safari 所有視窗」（作者在第 21 分鐘停掉，關視窗尚未執行，但截圖已被讀過）。**auto mode 全都沒擋**，因為每一步看起來都是為了「檢查版面」。作者事後寫 deny 規則封住，下一輪 Sonnet 5 想用 `pkill` 關自己開的伺服器時就被擋下：

```json
{
  "permissions": {
    "deny": ["Bash(open *)", "Bash(screencapture *)", "Bash(osascript *)", "Bash(killall *)", "Bash(pkill *)"]
  }
}
```

對照官方警語「Auto mode … does not guarantee safety」（[[claude-code-auto-mode-permissions]]）與「deny 規則才是永久界線」（[[claude-code-plan-mode-permission-rules-layering]]）。

### 5. 作者的選用表

| 要做的事 | 用哪個 |
|---|---|
| 日常：整理資料、做網頁、找明顯的錯 | Sonnet 5.5 |
| 要快，而且自己會核對 | Opus 5.5 |
| 條件很多、要想到題目沒講的事 | Opus 5.5 或 Fable 5.1 |
| 地毯式檢查 | Fable 5.1（最貴、內容最長） |
| 需要自己讀檔、找資料 | 不要用 Haiku 4.5 |

## C（關鍵行動）

- [ ] AI 回報「完成」後，先花一分鐘核對它**實際做了什麼**（數量、該有的東西、有沒有偷換做法）
- [ ] 看回報時留意有沒有「這是我的推論」「請確認」；**什麼都講得很肯定的反而要多檢查**
- [ ] 讓模型在背景自己跑之前，把不想讓它碰的指令（`open`、`screencapture`、`osascript`、`killall`、`pkill`）寫成 deny
- [ ] 自己評測模型時：事先寫好標準答案、空資料夾起跑、主觀項目盲測，並**刻意藏一個題目沒講的陷阱**
- [ ] 引用本篇結論時一併標明樣本限制（單一作者、每題 1–2 次、effort 固定 high）

## 原始摘要

Tim Wei 用自己 9 天東京行程 Excel，讓 Haiku 4.5、Sonnet 5、Sonnet 5.5、Opus 5.5、Fable 5.1 做四個日常任務（手機網頁、行程健檢、互動地圖、雨天備案）。簡單任務三個新模型都近滿分，盲測第一是最便宜的 Sonnet 5.5；拉開差距的是做事習慣（Sonnet 5.5 會標推論、標概略位置）與題目沒講的事（雨天備案只有 Opus 5.5／Fable 5.1 想到行李）。Haiku 沒做完卻回報完成；Sonnet 5 開 Safari、截全螢幕，auto mode 未擋。作者結論：平常用 Sonnet 5.5，條件多再換 Opus 或 Fable。iT 邦側欄推薦影片與留言區廣告不收；本系列並非鐵人賽。

## 相關連結

- [[claude-model-selection-guide]] — 官方定位的選型表；官方「不確定先用 Opus 5.5」與本篇實測立場並陳
- [[claude-code-model-and-thinking-depth]] — 同系列 G7：選模型與思考深度（本篇不重講的基本觀念）
- [[claude-opus-5-5-release-notes]] — Opus 5.5 發布、家族排序與方案差異
- [[claude-fable-5-usage-guide]] — Fable 的適用場景
- [[fable-5-delegation-cost-benchmark]] — 另一篇以實際任務量 Fable 成本的評測
- [[claude-code-auto-mode-permissions]] — auto mode 不保證安全的官方警語
- [[claude-code-plan-mode-permission-rules-layering]] — 同系列 G8：deny 規則才是永久界線
- [[xcode-mcp-test-report-fabrication]] — 同為「宣稱完成卻沒做」
- [[agent-ux-status-transparency-maturity]] — 回報完成 ≠ 真的完成，狀態要可驗證
