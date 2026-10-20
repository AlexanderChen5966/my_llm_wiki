---
title: Harness 層研究兩則：AgentBug-Smith 把框架錯誤變成可重現案例，MOSiB 用多分支＋路由器最佳化流程（增益高度依模型）
date: 2026-10-19
tags: [ai/agent, dev/testing, type/concept]
source:
  - https://www.aiposthub.com/agentbug-smith-live-harness-bench-agent-bugs/
  - https://www.aiposthub.com/mosib-agent-harness-branches-optimization/
type: idea
status: processed
asset_value: 19
---

> 🔴 **來源集中：aiposthub／Philo**。兩篇同作者、同日（2026-10-05）發布，寫作模板一致；同站文章互引不構成獨立印證。作者兩篇都**未實測**。
> 🔴 **arXiv 經轉述，未核原文**：AgentBug-Smith＝arXiv 2609.37864、MOSiB＝arXiv 2609.37834，本條所有論文數字與結論都是「論文自報，經 aiposthub 轉述」。
> 來源等級標示：**（論文，媒體轉述）**／**（作者觀點）**／**（作者示意情境）**／**（作者計算）**。

## T（核心論點）

兩篇都在講同一件事的兩面：**模型周圍的 harness（代理執行框架）本身就決定表現，需要獨立於模型的評測與最佳化**——一篇處理「harness 壞了怎麼穩定重現」，一篇處理「不改模型參數、只改流程能拉多少分」。兩者分開讀，不合成單一結論。

- **AgentBug-Smith**：🔑 **重現 ≠ 修復**。把真實錯誤報告轉成可執行案例，是可靠的起點，不代表能自動修好。
- **MOSiB**：🔴 **增益高度依模型**。標題的「46%→62%」只是 Gemini 3 Flash 一組；Claude Sonnet 4.5 同題只多 1.5 個百分點。

## B（背景）

**適用：** 自己在做 agent harness／代理框架，想建回歸測試集；評估「換流程 vs 換模型」的研究時，用來校正對標題數字的解讀。

**不適用 / 注意：**
- 兩篇的成效描述有的**沒有具體數字**（見下），不可自行補數字
- 為何放 ideas：單一媒體轉述論文、未核原文，依 [[ideas/openai-agents-api-public-beta]] 的先例處理；日後若剪藏到論文原文或第二來源，再考慮併入 [[harness-engineering]] 或另立 wiki

## R（關鍵細節）

### 一、AgentBug-Smith 與 Live-Harness-Bench

- 研究 2026-09-29 公開，之後在 X 被分享（媒體轉述）
- 官方 repo：`github.com/EaminC/AgentBug-Smith`（一手連結；原文建議使用時記錄所採用的版本）
- 做法：從開源 harness 的真實錯誤報告取得線索，轉成含環境與觸發步驟的可執行重現案例（論文，媒體轉述）
- **Live-Harness-Bench**：**論文版**含 200 個可重現的 harness 錯誤；屬**持續擴充型**評測集，總數之後會變——不可寫成固定總數（論文，媒體轉述）
- 四階段（原文整理表）：

| 階段 | 產出 | 能回答的問題 |
|---|---|---|
| 蒐集報告 | 真實問題與線索 | 哪些框架行為失敗 |
| 重現 | 可執行的錯誤案例 | 問題能否穩定出現 |
| 修復評估 | 修改與驗證結果 | 程式代理能否處理原因 |
| 知識整理 | 可重用的修復經驗 | 類似問題如何更快定位 |

- 「在不同基礎模型下，重現成功率優於比較方法」——🔴 **媒體轉述，文中無具體數字**
- 「用這些案例評估軟體代理，修復真實 harness 錯誤仍有明顯限制」——🔴 **媒體轉述，文中無具體數字**
- 200 個案例針對 harness 層的真實錯誤，不能全部歸因於模型推理（媒體轉述）
- 持續擴充的資料集要保留案例版本，新舊結果不能當同一張排行榜比（作者觀點）
- 「保存最小重現案例，比只留聊天紀錄更能支援後續修復與回歸檢查」（作者觀點）

---

### 二、MOSiB（Mixture of Self-Improving Branches）

- 機制（論文，媒體轉述）：多條 harness 分支各自累積**開發題目**與**改善筆記**，形成不同專長；再由 **router 在事前**依輸入特徵與開發階段資訊選擇分支。**不改模型參數**。原文描述的數學案例是兩條分支：一條偏「驗證答案」、一條偏「完整推導」
- 比較基準：**Meta-Harness**，同模型、同測試情境（論文，媒體轉述）。四組數字並列，**不可只取第一列**：

| 測試與模型 | Meta-Harness | MOSiB | 差距 |
|---|---|---|---|
| 奧林匹亞數學，Gemini 3 Flash | 46.0% | 62.0% | +16.0 pt |
| 奧林匹亞數學，Claude Sonnet 4.5 | 29.0% | 30.5% | **+1.5 pt** |
| Terminal-Bench 2.0（模型原文未標） | 44.8% | 50.0% | +5.2 pt |
| SWE-bench Lite（模型原文未標） | 63.6% | 66.0% | +2.4 pt |

- 讀數要點：
  - 46→62 是 **+16 個百分點**；相對增幅約 +34.8% 是**作者計算**，不是「準確率翻倍」
  - 同一份數學題，換成 Sonnet 4.5 只 +1.5 pt → **增益高度依模型**
  - 四個情境的任務、工具、錯誤處理不同，不能壓成「AI 整體變強三成」（作者亦如此提醒）
  - 🔴 原文**沒有給出分支開發、維護與路由的成本數據**；只提醒開發成本不會因單次只跑一條分支而消失
- 「每題**事後**挑最佳答案（oracle）」只估潛力，「**事前** router 選流程」才接近實際使用——閱讀同類研究要分清楚（作者觀點）
- 導入四步：固定評分標準 → 開發／測試資料分離 → 記錄各分支權限 → 保留失敗案例而非只存最佳得分（作者觀點）；客服信件／報表分析的分流例子是**作者示意情境**，不是論文實驗

> ⚠️ **不可與 Harness-Zero 的 meta-harness 數字直接比較**：[[agent-trajectory-distillation-harness-zero-perplexity]] 表中的「evolved meta-harness」用的是 SpreadsheetBench／AppWorld／USPTO，與 MOSiB 的奧林匹亞數學／Terminal-Bench 2.0／SWE-bench Lite **評測集不同**；而且 **Harness-Zero 改權重（LoRA SFT），MOSiB 不改權重**。兩邊的「meta-harness」是否同一方法，原文都沒交代，待核對。

## C（連結與應用）

- [ ] 自己的 agent／skill 出錯時，留一份「最小重現案例＋環境版本」，而不只是聊天紀錄（對應 AgentBug-Smith 的重現階段）
- [ ] 讀任何 harness 最佳化研究，先找「不同模型的同題數字」與「是事前路由還是事後挑最佳」兩件事，再看標題數字
- [ ] 若日後取得 arXiv 原文，補上 AgentBug-Smith 的重現成功率與修復率具體數字、MOSiB 的分支成本

## 相關連結

- [[harness-engineering]] — 「Harness 是護城河、同模型重寫 harness 提升比換模型大」的總綱；本條兩篇是 2026-09 底的研究層佐證（皆經轉述）
- [[agent-bug-fix-eval-green-is-not-correct]] — 「重現 ≠ 修復」與「綠燈 ≠ 改對」互相呼應；⚠️ 但一邊是 harness 錯誤評測集、一邊是 agent 修 bug 的驗收，不要混為一談
- [[agent-trajectory-distillation-harness-zero-perplexity]] — 另一路線：把 harness 行為蒸餾進權重；與 MOSiB 不改權重相反，數字不可互比（見上方 ⚠️）
- [[concepts/environment-over-model-principle]] — 「環境優先於模型」的概念框架
- [[agent-tool-runtime-seven-layers]] — harness 實際包含哪些層，對照 AgentBug-Smith 說的「工具結果沒傳遞、步驟狀態不同步」
- [[devin-two-years-intelligence-saturation]] — Frontier Code 可合併性評測：benchmark 分數之外的另一種驗收角度
