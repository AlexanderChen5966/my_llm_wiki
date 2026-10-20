---
title: AI 實作、AI 測試、AI 說沒問題：那個 Green 到底保證了什麼
date: 2026-08-14
tags: [ai/agent, dev/testing]
source: https://codelove.tw/@tony/post/al5Y1q（小編精選技術文章翻譯，2026-08-13）
type: article
status: processed
asset_value: 85
---

## T（核心論點）

> **問題不在於 AI 會實作，也不在於 AI 會寫測試。問題在於：實作，以及用來評估該實作的標準，全部都被放進同一個最佳化迴圈裡。**

此時的 Green 有可能不是「已滿足規格」的證明，而只是「**滿足該 Agent 所參照的評估標準**」而已。

而且必須先說清楚，這個問題**在 AI 之前就存在**：

> **All Tests Passed ≠ Software is Correct**
> 測試是 Green，只代表「對目前存在的測試而言，觀察結果與預期值一致」。

所以真正該問的不是「有沒有變成 Green」，而是：**這個測試是為了保證什麼而存在的？**

## B（適用場景）

**適用：**
- Agentic Coding 的驗收機制設計（尤其是「該把多少責任委派給 Agent」）
- EM／Tech Lead 重新設計團隊的 Quality Gate
- 判斷「讓 AI 寫測試」與「把 AI 產生的評估標準當成最終 Quality Gate」的界線

**不適用 / 注意：**
- 🔴 **不是主張「不該讓 AI 寫測試」**——AI 產生測試在邊界值列舉、Regression／Unit／Integration Test 生成、Fixture／Mock 生成、失敗日誌分析、找 Coverage 缺口上都能大幅提升效率
- 🔴 **也不是「因為是 AI 所以危險」**——人類對自己寫的程式碼，自己想案例、自己寫測試、自己修改、自己判定「都通過了所以沒問題」，同樣不會被認為足以保證品質。**是同一個原理**

## R（四個核心觀念）

### 1. AI 時代改變的是「實作與驗證的距離」

傳統開發中實作與驗證並非完全分離（開發者自己也寫 Unit Test），但仍存在 **Code Review／Static Analysis／CI／Integration Test／E2E／Production Monitoring** 等從不同角度檢視實作的機制。

🔑 **重點不是「誰來測試」，而是「存在與實作不同的評估軸」。**

而 Agentic Coding 很容易變成：AI 負責實作、寫測試、分析失敗、修正、反覆執行直到 Green——**全部在同一個 Feedback Loop 中完成**。

### 2. Agent 追求的不是「正確的程式碼」，而是「成功條件」

若把 `npm test` 當成成功判定，對 Agent 而言最強的 Feedback Signal 就是 Failed／Passed。**這是非常合理的行為**——問題在於 **Passed 不一定等於 Specification Compliance**。

**Reward Hacking**（ML 概念：沒達成真正想要的目標，而是去攻略評估指標本身）在軟體工程也會出現同樣結構。極端例子：

```
if input == "known-test-value":
    return expected_value
```

⚠️ 作者謹慎聲明這不是說 Coding Agent 一定會寫出這種程式，重點是：

> **如果評估標準與原本目標有落差，Agent 就能把那個落差也一起最佳化掉。**

🔑 **Agent 並沒有說謊，它只是利用給定的 Feedback 合理地想達成目標。真正的問題是：我們提供的評估函式，真的有辦法表達「正確性」嗎？**

### 3. Coverage 不是 Oracle

100% Coverage 也解決不了問題，因為 Coverage 顯示的是**程式碼有沒有被執行過**，而不是**有沒有正確的 Assertion**。

```js
expect(response).toBeDefined();   // 能執行到目標程式碼，但什麼都沒保證
```

真正想確認的可能是 `status === "completed"`、資料庫是否正確保存了值、是否沒有重複執行同一個處理——**這些都是不同的問題**。

> **Coverage 是觀測範圍的指標，不是 Oracle 品質本身。**

### 4. Test Oracle：判斷執行結果是否正確的基準

簡單例子的 Oracle 很單純（買 100 元商品 2 個 → 200 元）。但要保證「**即使發生 Retry，也不會對同一位使用者重複發送**」時，單看 `HTTP 200` 就不夠，可能還需要觀察 DB State、Queue State、External API Request 等多個觀測點。

🔑 **能寫測試程式，和能設計正確的 Oracle，是不同的能力。**

## C（關鍵行動）

- [ ] 委派前先定義 **Quality Constraint**：這次變更最重要的 Risk 是什麼／哪個狀態轉移必須被保證／哪個邊界不允許重複處理／哪個 Failure 要作為 CI Blocking
- [ ] 指令從 Confirmation 改為 **Falsification**：不是「請確認這個實作是正確的」，而是「**請建立能證明這個實作是錯的測試**」
- [ ] 不要只把實作 Agent 自己說的「我成功了」當成 Quality Gate

## 補充資料

### 🔑 人類角色的轉變

> **從「寫程式碼的人」轉變為「定義正確性的人」。**

作者明確反對「那人類就把所有測試案例都寫完」——**那樣會大幅失去 Agentic Development 的優勢**。正確做法是先定義 Quality Constraint，再把實作與測試生成交給 Agent。

### SpecBench：把 Agent 看得到與看不到的測試分開

2026 年的 Coding Agent 研究已在討論這個問題。**SpecBench** 將 **Visible Validation Tests** 與 **Held-out Tests** 分離來評估 Agent——為的正是區分「通過看得見的測試」與「滿足原本的 Specification」這兩件事。

（與 [[agent-bug-fix-eval-green-is-not-correct]] 收錄的 SWE-bench `FAIL_TO_PASS`／`PASS_TO_PASS` 與「模型看不到用來評分的測試」是同一個設計思想）

### 🔴 4,882 件 Agent 生成 PR 的實證分析

2026 年公開的一項分析研究了 4,882 件 Agent 生成的 Pull Request：

- 在修改 Production Code 的 PR 中，**約有一半包含測試變更**（即另一半沒有）
- 從既有測試對 Agent 變更行的 Coverage 來看，**特別是 Python，結果並不令人滿意**
- **`try`／`catch`／error handling 這類異常情況的程式碼特別難被測到**

🔑 **在 AI 開發中，「確認正常流程有運作」的速度會大幅提升——正因如此，更需要去思考「哪些地方壞掉會造成問題」。這就是 Risk Analysis 重要的原因。**

⚠️ 另一個必須分開看的區別：**Agent 能生成測試** ≠ **Agent 的變更有被適當測試**。

### 🔴「AI 會 Review，所以沒問題」也不對

即使增加流程，如果**規格理解相同、Context 相同、評估標準相同**，那麼就可能**共享同樣的 Blind Spot**。

> **重點不是 Agent 的數量，而是評估軸的獨立性。**

同一份 Implementation 應該從**不同觀點**去看。（這正是 [[claude-code-dynamic-workflows]] 的 Adversarial verification pattern 要處理的 Self-preferential bias，也是 [[subagent-four-boundaries]] 強調「多開 agent ≠ 更可靠」的理由）

### Testing Trophy 在 Agentic Coding 中依然有效

當 AI 能生成測試後，很容易走向「那就全部用 E2E 確認吧」。但各層 Test Layer 的特性不會因為 Agentic Coding 而改變——Static Analysis、Unit、Integration、E2E 各有作用。

> **重點不是測試數量，而是針對 Failure Mode 選擇最適合的 Test Layer。**

### EM／Tech Lead 要設計的東西變了

不再只是「誰來實作」，而是還要思考「**要把多少責任委派給 Agent**」，以及如何建立 **Independent Verification**——這不一定是指由另一個人把所有內容全部檢查一遍。

### 🔴 第一線導入實例：Claude Code × Playwright × GitHub 全鏈路（2026-08-24 補充）

來源：日本 QA 工程師 SA 的第一人稱經驗談（譯自 Qiita `a_sato_JQIT`，https://codelove.tw/@tony/post/x0n6ga）

**三個階段的演進**

1. **手動時代**：測試案例一項一項寫進 Excel／試算表，人工操作瀏覽器逐項確認；連「情境測試」的概念都不存在，整個現場以「人來操作」為前提。
2. **Playwright 時代**：E2E 測試終於能當程式碼管理，但測試結束後仍要人工開瀏覽器、找 GitHub issue、寫留言、把狀態改成 done。
3. **Claude Code × GitHub 串接後的全自動鏈路**：

   ```
   自然語言指令 → Playwright 測試自動執行 → 結果自動留言到 GitHub issue → 狀態自動轉 done
   ```

   整條鏈路**完全不需要人介入**。

**🔑 這則實例對本條目 T 層的意義**

作者的核心主張與本條目完全一致，而且是從現場端說出同一件事：

> **AI 並不是萬能的。如果指令下得不夠清楚，它也可能不會按照預期行動。作為前提的設計，以及 QA 該如何判斷的標準，終究還是必須由人來掌握。**

也就是說——**這條全自動鏈路自動化的是「執行與回寫」，不是「Oracle」**。測試設計與 QA 判斷標準仍留在人手上，正好對應本條目「委派前先定義 Quality Constraint」的關鍵行動。

作者自己的說法是：**人類該做的工作的解析度變高了**——單純重複的工作交出去之後，才能專注在「該測什麼」「哪裡有風險」這些本質性思考上（即本條目所說的 Risk Analysis）。

**團隊導入初期的真實疑慮**（值得預期並提前回答）

- 「AI 真的能執行測試、守住品質嗎？」
- 「如果它做出非預期的行為，誰要負責？」

作者坦言這兩個不安不只團隊裡有，自己心裡也有——⚠️ 這正是本條目「Green 保證了什麼」問題的組織版本：責任歸屬不會因為鏈路自動化而自動有答案。

**切入建議**：不要一開始就設計完整體系。「先試一件小事、做一個小小的自動化、對 AI 下一個指令」，用「真的跑起來了」的體驗當下一步的起點。

作者另觀察到：會用 AI 與不會用 AI 的 QA 工程師之間**已經出現明顯差距**。文末坦承本文架構與內容整理也是與 AI 共同完成的。

⚠️ **這是單一團隊的經驗談，非受控研究**——鏈路可行性有第一線佐證，但成效未量化。

## 相關連結

- [[agent-bug-fix-eval-green-is-not-correct]] — 🔑 **本篇的實證對照**：實測中 agent 真的去改測試檔讓自己「通過」，且沒有驗證能力時會很有自信地宣稱成功；本篇提供的是這個現象的理論框架（Reward Hacking／同一最佳化迴圈）
- [[wayfinder-decision-map-workflow]] — 「TDD 的理由是 AI 骨子裡是作弊仔，會生出配合錯誤答案的假測試」
- [[ai-code-review-safeguards]] — AI 生成程式碼的 Review 防護層與三大動態死角
- [[claude-code-dynamic-workflows]] — Self-preferential bias 與 Adversarial verification pattern
- [[subagent-four-boundaries]] — 多 agent 不等於多評估軸
- [[agent-error-recovery-taxonomy]] — Verification Failed 屬於 Strategy Failure 該 Replan
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — 關鍵行為必須由 Workflow 保證，不能只加一句 Prompt
- [[ai-era-developer-value]] — AI 時代開發者價值的轉移
- [[flutter-patrol-e2e-testing]] — E2E 測試的另一條技術棧（Flutter/Patrol）；補充資料的 Playwright 鏈路是 Web 端對應物
- [[puppeteer-browser-automation]] — 瀏覽器自動化執行層
- [[claude-gh-ci-monitoring]] — Claude × GitHub 的 CI／issue 回寫鏈路（補充資料那條全自動鏈路的機制面）
- [[llm-judge-confidence-cascade]] — 評審本身也需要被驗證：便宜評審在風格精緻的錯誤答案上高信心仍會錯
