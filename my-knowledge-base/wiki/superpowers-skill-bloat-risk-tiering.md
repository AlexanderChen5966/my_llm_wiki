---
title: Superpowers：GPT-5.6 時代下的流程毒瘤
date: 2026-07-16
updated: 2026-08-05
tags: [ai/agent, pkm/framework]
source: https://codelove.tw/@tony/post/aWK6Ja
type: article
status: processed
asset_value: 82
---

日均消耗約 3 億 token 的重度使用者，實測後把 Superpowers（14 個 skill、核心規則約 3207 行）從工作流中拆掉：問題不在模型被拖慢，而是流程被拖慢——強模型時代，為不穩定、低自主性代理設計的長流程被當成所有任務的統一宗教，反而壓縮了模型的有效自由度。

## T（核心論點）

Superpowers 的硬門檻措辭（「只要有 1% 的可能就必須呼叫 skill」「任何專案都必須先設計」「沒看到失敗測試就不能寫生產程式碼」）把「通常有幫助」寫成了「永遠必須」，導致模型無法根據任務風險與規模做正常工程取捨。解法是把規範分級（Lite/Standard/Strict），而非統一套用同一套審查+子代理+worktree 儀式鏈。實測把 14 個 skill 精簡到 6 個輕量版本後，核心 SKILL.md 從 3207 行降到 110 行。

## B（適用場景）

**適用：** 高頻使用 AI 編程工具、需要重新評估既有工程紀律框架是否與任務規模脫節的重度開發者。
**不適用 / 注意：** 安全、付款、權限、資料遷移、公開 API 等高風險任務仍應保留完整審查流程；本文是單一重度使用者的實測經驗，非團隊協作場景的定論。

## R（風險分級與判斷框架）

**三級分工**：
- **Lite（預設）**：文件/設定/單檔修改/簡單 Bug — 理解目標→修改→針對性測試→說明結果，不寫長設計、不建 worktree、不派代理、~~不做自動審查~~
  > 🔴 **「不做自動審查」這半句已於 2026-08-05 修正**——上游立場是 **ceremony scales, approval never does**：該砍文件與儀式，**不該砍人工核准閘門**。詳見下方複查第四節。
- **Standard（普通複雜任務）**：多檔改動、有順序依賴 — 短計畫→實作→相關測試→一次自我檢查
- **Strict（高風險任務）**：安全/付款/權限/資料遷移/公開 API/重大重構 — 設計討論→計畫→實作→獨立審查→整合驗證，且應由使用者明確選擇而非自動觸發

**成本模型**：總成本 = 主代理上下文 + 實作代理次數×任務上下文 + 審查代理次數×diff與需求上下文 + 失敗重試×全部上下文 + 流程狀態訊息。子代理架構下，同一份需求會被主代理、實作代理、規格審查、品質審查各讀一次，重複越多任務越多。

**正確的控制變數**：不是「有沒有呼叫某個 skill」，而是變更風險高低、失敗代價大小、是否存在獨立知識盲區、是否有可執行驗證手段、使用者是否明確需要額外審查。

## C（關鍵行動）

- [ ] 檢視自己使用的工程紀律框架，是否把「任何專案都要先設計」等規則寫成無差別的硬門檻
- [ ] 依任務類型（文件/單檔修復 vs 多檔改動 vs 高風險）分級套用流程強度，而非統一走完整儀式鏈
- [ ] 只在使用者明確要求或任務屬於高風險類別時才啟用子代理與強制審查

## 2026-08-05 有效性複查：論點強化，但有一處主張要修正

上網交叉比對（官方 README／`skills/` 目錄／issue tracker／Anthropic 官方文件）後的結論：**核心論點不但還站得住，且從「單一重度使用者的意見」升級為有第一方數據支持的判斷**。但條目本身有一處主張被上游證據修正。

### 一、原始證據逐字驗證通過（v6.2.0 現況）

條目引用的三句硬門檻**現在還在，且比條目引述的更強硬**：

| 條目引述 | 官方原文（2026-08-05 查證）|
|---|---|
| 「只要有 1% 的可能就必須呼叫 skill」| `using-superpowers`：**"If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill."** ＋ **"IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT. This is not negotiable."** |
| 「沒看到失敗測試就不能寫生產程式碼」| `test-driven-development`：**"NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST"** ／ **"Write code before the test? Delete it. Start over."** ／ **"Thinking 'skip TDD just this once'? Stop. That's rationalization."** |
| 「任何專案都必須先設計」| `brainstorming` 的 `<HARD-GATE>`：**"This applies to EVERY project regardless of perceived simplicity"** |

**細微補正**：TDD 其實**有三項窄例外**（throwaway prototypes／generated code／configuration files），但**須先取得人類夥伴許可**，非全面豁免；且**小改動明確不在例外內**。條目原本讀起來像毫無例外，實際是「例外存在但要報備」。

**規模數據更新**：`skills/` 目錄仍是 **14 個**（與批評當時相同，未膨脹也未瘦身）；266.6k stars；plugin.json 已是 **v6.2.0**（市集快取仍顯示 6.1.1）。

### 二、🔑 論點被三個獨立來源大幅強化（條目寫成當時都還沒出現）

| 來源 | 日期 | 補強了什麼 |
|---|---|---|
| **Anthropic 官方 Claude 5 context engineering**（[[claude-5-context-engineering-rules]]）| 2026-07-27（晚本篇 11 天）| **刪掉 Claude Code 逾 8 成系統提示詞、內部編碼評測零可測量下滑**，並明確建議把「防最壞情況的規則」改成「基於判斷的指示」——**第一方 ablation 數據，正是本篇論點** |
| **官方 Opus 5 提示詞指南**（[[claude-opus-5-prompting-guide]]）| 2026-07-29 | 「Opus 5 已會主動驗證與修正產出，所以『完成後再檢查一次』『**交給子代理複查**』這類舊習慣**不一定有幫助，反而增加工具呼叫與 token**」——直接支持本篇的成本模型 |
| **上游自己的效率審計**（issue #2063 引用）| 2026-07-28 | 一人真實 Codex window **兩週 2,240 筆 session 紀錄、24.21 GB rollout log 全數解析**；Finding 4：**「process cost was not proportional to risk or uncertainty」** |

### 三、🔴 上游最強的一項數據：絕對措辭在**主動壓制**模型原生判斷力

upstream PR #2063 做的分類微實驗，比本篇原有的論證強一個層級：

> 把現行入口決策文字**逐字**拿去跑三個任務 brief，它把 bounded 級任務推到 FULL ceremony **5/5**；
> 而**一個完全沒有任何指引的 null prompt**，與一份草擬的三路徑 router，**兩者都能完美且一致地區分三個類別**。
> **絕對措辭（"EVERY project regardless of perceived simplicity"）正在主動壓制模型原生就會做的區辨。**
> **改寫閘門文字才是干預手段；在它之上疊加更多指引不是。**

配套的 ceremony 普查（spike/bounded/architectural 各 3 reps）：ceremony 量確實隨複雜度有些縮放（首次改碼前平均工具呼叫 16.7 vs 24.0，差 30.4%），**但 bounded 與 architectural 兩類的每一個 rep 都無條件寫了剛好兩份文件**（design spec → plan）——**這就是那筆固定成本**。

**行為上的徵兆**（審計原文）：整個 window 裡人類反覆要求 agent「push through」「用 turbo mode」「別記帳了去 spike 決定性行為」。
> **當你的人類夥伴得一直叫你停止做流程，這個流程就是校準錯了。**

### 四、🔴 本篇有一處主張被上游修正：Lite 層不該把核准也砍掉

上游 #2063 的標語是 **「ceremony scales, approval never does」**——三路徑（spike／bounded／architectural）**縮放的是產出物，但每一條路徑都保留人工核准**；其 anti-pattern 段落被重寫為**針對「跳過核准」而非「跳過文件」**。

對照本篇 R 層的 Lite 定義：「不寫長設計、不建 worktree、不派代理、**不做自動審查**」——**把核准閘門一起砍掉了**。

> **修正後的立場**：該砍的是**文件與儀式**（design spec、plan、worktree、子代理鏈），**不是核准閘門**。本篇原本把兩者綁在同一個 Lite 層降級，是可以拆開的——這也解釋了為什麼上游能同時接受「小任務別寫兩份文件」又拒絕「小任務不用人看過」。

另補一個本篇提出的解法在**機制層面為什麼必須從框架外部施加**（來自 issue #2079）：
> 「brainstorming skill 沒有輕量路徑，而且**我認為模型無法自己建構一條，因為該 skill 明確把『想要一條輕量路徑』列為 anti-pattern**。」

### 五、缺少的對立證據（本篇偏一面）

| 對立證據 | 內容 |
|---|---|
| [[claude-code-skill-survivors-20-picks]]（2026-07-09，早本篇一週）| GitHub Issue 對照實驗：12 個同複雜度任務，**裝 Superpowers 的會話 token 用量平均低 14%**——因為先問清需求避免返工 |
| [[superpowers-framework]] 的 MikeWang 實測（2026-07-07）| 兩個真任務實測**修正了三個擔憂**：不亂開 worktree、不擅自 push、**越難越穩** |

**兩組數據如何與本篇並存**（本篇成本模型未區分的關鍵）：
> 它們量測的是**不同規模區間**。任務數少時，「先問清楚避免返工」節省 > 子代理鏈開銷；到本篇作者那種 **3 億 token/日** 的量級，同一份需求被主代理＋實作＋規格審查＋品質審查各讀一次的**乘法效應才反過來主導**。上游 #2078（已合併）正是在批次化小任務以壓低這筆固定開銷。

### 六、跨 agent 適用性

- **論點機制與平台無關**（硬門檻壓制原生判斷、子代理成本乘法），且上游證據本身就跨平台：#2063 的審計來自 **Codex** window、eval 跨 **三個 harness**
- **但本篇的成本模型是 Claude Code 形狀的**（子代理／worktree／SessionStart hook）。Superpowers 現支援 11 個平台，各家子代理與 worktree 實作不同；`Pi` 甚至因原生 skill 支援而不需相容層——**在這類 harness 上，成本結構要重新估**
- ⚠️ 官方另載：**「If you use more than one, install Superpowers separately for each one.」** 多 agent 使用者的維護成本是 N 倍，這是本篇未涵蓋的另一筆成本

### 🔴 六之二、Claude 5 世代會不會也一樣？——會，而且是最嚴重的地方

本篇標題錨在「GPT-5.6 時代」，容易被誤讀成 OpenAI 側的特例。**實際上這不是廠商特例，而 Claude 5 世代恰恰是問題最明顯的地方。**

**證據跨三廠商**：上游 PR #2063 的 65 次評分實測跨**三個 harness**，body 內提及次數為 **Codex ×19、Gemini ×14、Claude Code ×5**（Cursor ×1），模型明確列出 **`claude-fable-5`、`gpt-5.5`、`gpt-5.6`**；而該 PR 的控制端本身就是 **Claude Fable 5 + Claude Code CLI 2.1.220 + superpowers 6.2.0**。→ **非單一廠商現象。**

**四個理由讓 Claude 5 世代比舊世代更嚴重，而非更輕微**（前三項有一手來源，第四項為推論）：

| # | 理由 | 依據 |
|---|---|---|
| 1 | **Anthropic 自己證明了餘裕存在**：為 Claude 5 世代刪掉 Claude Code 逾 8 成系統提示詞、內部編碼評測零可測量下滑，並明確建議把「防最壞情況的規則」改成基於判斷的指示 | 一手（[[claude-5-context-engineering-rules]]）|
| 2 | **Claude 5 已會自我驗證，使審查類 skill 變成純開銷**：官方指「Opus 5 已會主動驗證與修正產出，所以『完成後再檢查一次』『交給子代理複查』這類舊習慣不一定有幫助，反而增加工具呼叫與 token」——而 Superpowers 的 `verification-before-completion`、`requesting-code-review`、子代理兩階段審查正是這類 | 一手（[[claude-opus-5-prompting-guide]]）|
| 3 | 🔑 **強調詞在 Claude 5 世代已從技巧翻轉為負債**：官方對照表把「重要指令重複講」改為「只寫在工具描述，講一次就好」。而 `using-superpowers` 的前言正是一個 **`EXTREMELY-IMPORTANT` 區塊 ＋ 全大寫封套 ＋ 12 列 Red Flags 表**，耗約 450 字在強制執行上——**這是官方新規明確點名反效果的那一類寫法** | 一手（[[claude-5-context-engineering-rules]] ＋ upstream #2087）|
| 4 | **越聽話的模型受害越深**（推論）：#2087 指強制措辭「對它當初所針對的模型世代是合理設計，那些世代**確實需要規則形狀的壓力才會遵守**」。反過來說，指令遵循能力更強的 Claude 5 會**更忠實地執行**同一段硬門檻——於是儀式跑得更完整、成本更高，且被壓制的原生判斷力也更多 | 推論（由 #2087 的世代對比反推）|

**#2087 記錄的具體傷害**（Claude 5 世代專屬）：
> 現行前言「**其字面解讀甚至禁止模型在做 skill 檢查前讀取附上的連結或提出釐清問題**」。
> 而該政策「壓縮成幾句話也不會損失任何東西」。

### 🔑 重要的不對稱：對舊世代模型，這個框架仍是對的設計

這不是「Superpowers 寫錯了」，而是**它為一個已經過去的世代校準**。#2087 的收尾正好點出正確處理方式：

> 「Happy to PR this if there's interest — **possibly gated per-model if older-generation support still needs the stronger framing**.」

→ **強制措辭的必要性是世代函數**。因此本篇的風險分級主張，在 Claude 5 世代應再加一個維度：**不只依任務風險分級，也依模型世代分級**。同一份硬門檻對 Claude 3.5 可能是必要護欄，對 Claude 5 是淨損失。

> 這與 [[synthesis-prompt-engineering-evolution]] 記錄的「官方自己的保鮮期警告——提示詞技巧的折舊速度就是新常態」是同一件事：**Superpowers 就是一個正在折舊中的技巧集合的實例**。

### 七、仍然開放的問題（本篇與上游都未解）

- ❓ **誰來判定風險等級**：本篇主張 Strict「應由使用者明確選擇而非自動觸發」，但這把判斷責任推回人；若人不會判斷風險（正是最需要框架的族群），分級制反而失效
- ❓ **路徑選對了，但「做多少」沒人管**：#2079 關閉時的留言指出——「router 選的是**路徑**，但**架構路徑裡沒有任何東西在問要建多少**」；那個一段話的規則變更被正確分類為 architectural，**仍然產出 3,061 行 / 14 commits / ~10 小時**，而承載邏輯只是一次 `git rev-parse --git-common-dir` 比對

## 結論：這篇條目的狀態

**論點成立且已強化，asset_value 由 70 上調至 82。** 它從「一個重度使用者的實測意見」變成「有第一方 ablation（Anthropic 刪 8 成零下滑）＋ 上游自家 24.21 GB 審計 ＋ null-prompt 對照實驗三重支持的判斷」。**唯一需要改的是 Lite 層別把人工核准一起砍掉。**

⚠️ **但標題的世代錨點要留意**：「GPT-5.6 時代」是寫作當時的世代標記，現行世代已是 Opus 5／Sonnet 5／Fable 5／Sol 5.6。論點本身不因此失效（反而更成立），但引用時別讓標題誤導成過期內容。

## 相關連結

- [[superpowers-framework]] — Superpowers **14 個** Skill 原始設計（TDD 工程紀律；原記「15 個」已於 2026-08-05 依官方 `skills/` 目錄更正），本篇是對其在強模型時代的批判性實測反思
- [[ponytail-lazy-senior-dev-skill]] — ponytail 六梯級決策樹「先問這需要存在嗎」，與本篇風險分級精神一致的另一種實作
- [[synthesis-ai-dev-workflow-frameworks]] — AI 開發工作流框架精粹，可對照本篇「風險校準優於統一紀律」的論點
- [[synthesis-practitioner-skills-design]] — 本篇為其來源之一；本篇的三級風險分工是該精粹「框架輕重之爭」的第三條路答案
- [[dev-workflow-optimization-tools]] — **本知識庫的採用決策紀錄（2026-08-05）**：依五情境檢核後**暫不採用整套**（「可以漸進採用」為硬性否決＋與已決定不納入的 TDD 直接衝突），含六項風險登記與三個值得摘用的子模組、以及未來重評的三個觸發條件
