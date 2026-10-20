---
title: Claude Code Dynamic Workflows：AI 自動編排平行 Subagent
date: 2026-06-02
tags: [ai/agent, ai/claude, ai/workflow]
source: https://www.oao.tw/ai-knowledge/claude-code-dynamic-workflows
source_2: https://muki.tw/claude-code-dynamic-workflows/（六種 pattern／三個失敗模式／官方用例，整理自 Thariq 的 X 貼文，2026-08-14）
type: note
status: processed
asset_value: 78
---

## T（核心論點）

Claude Code Dynamic Workflows（2026-05-28 發布）改變了 AI 的工作方式：從「聊天機器人逐輪回答」變成「AI 工程師寫編排腳本，管理一群平行 agent」。Claude 自動生成 JavaScript 編排腳本，同時派出最多 1,000 個平行 subagent（同時 16 個並行），將任務拆解、驗證、彙整全部自動化。

⚠️ 2026-10-19：Managed Agents 端（API）於 2026-10-09 推出同名能力的 public beta，據 aiposthub 轉述官方文件，一般 subagent 同時 25 子執行緒，與本文 Claude Code 端的 16 並行不同，屬不同產品表面（也可能是不同指標），待核對；動態流程上限兩端都稱 1,000，出處各自標示。詳見 [[ideas/claude-managed-agents-dynamic-workflows-every-2026-10]]（aiposthub 轉述，未見官方原文）。

## B（適用場景）

**適用：**
- 大規模程式碼審計與重構（每個 subagent 負責一個模組）
- 多角度深度研究（內建 `/deep-research` 即是典型案例）
- 批次重複性任務（500 個函數生成測試、100 篇翻譯）
- 多角度交叉審查（correctness / security / performance 分別審查同一份 PR）

**不適用 / 注意：**
- 單一線性流程或簡單問題不需要 workflow
- 需要人在每步介入的任務不適合
- Token 消耗大幅增加（10 個 subagent ≈ 10 倍消耗），建議先小範圍測試
- 需要 Max / Team / Enterprise 方案

## R（操作步驟）

1. **觸發方式一：Keyword** — 在 prompt 中包含 `workflow` 關鍵字即觸發（注意：只想討論概念時用 `multi-agent` 或 `parallel agents` 替代）
2. **觸發方式二：Ultracode** — `/effort ultracode` 開啟 xhigh reasoning，Claude 自動判斷是否適合 workflow
3. **內建 workflow** — `/deep-research` 可直接使用，無需設定
4. **三階段執行模型**：
   - Phase 1 Orchestration：Claude 生成 JS 編排腳本（定義拆解、依賴、驗證邏輯）
   - Phase 2 Execution：派遣 subagent 並行處理子任務
   - Phase 3 Synthesis：驗證 subagent 抓矛盾 → 通過後彙整最終答案
5. Workflow 腳本可儲存，之後在 `/` 自動補全中重複使用

## C（關鍵行動）

- [ ] 挑一個範圍明確的任務（如審查某目錄下的 API handler）試跑 Dynamic Workflows
- [ ] 觀察 Token 消耗量，建立對成本的直覺
- [ ] 評估是否將常用 workflow 腳本儲存為可重複使用的 slash command

## 原始摘要

Claude Code Dynamic Workflows（2026-05-28 發布）讓 Claude 自動生成 JavaScript 編排腳本，同時派出最多 1,000 個平行 subagent 完成任務，將 AI 的工作方式從逐輪回答轉變為「寫編排腳本、管理平行 agent」的兩階段執行模型。

## 相關連結

- [[claude-opus-4-8-release-notes]] — 與 Dynamic Workflows 同日發布
- [[claude-code-advanced-workflow]] — Boris Cherny 的進階技巧（/loop 排程、Hooks 等）
- [[claude-code-goal-command]] — /goal 指令讓任務自動跑到完成
- [[claude-code-commands-2026]] — 2026 上半年新命令總覽
- [[claude-code-three-modes-distinction]] — Plan/Auto/Dynamic Workflows 三模式定位區辨（Workflows 是最外層的 orchestration scale）
- [[claude-code-auto-mode-permissions]] — Auto Mode 權限機制深挖（Dynamic Workflows 搭配 Auto Mode 效果最強）
- [[ideas/claude-managed-agents-dynamic-workflows-every-2026-10]] — 📎 2026-10-19 新增：Managed Agents（API）端的動態工作流程 public beta 與 Every 案例；🔴 25 子執行緒 vs 本文 16 並行並列待核

## 補充資料（來源：https://muki.tw/claude-code-dynamic-workflows/ ，2026-08-14；整理自 Anthropic Thariq 的 X 貼文 status/2061907337154367865）

補上原條目完全未收的三大塊：**要解決的三個失敗模式**、**六種 workflow pattern**、**官方實際用例**。版本門檻 **v2.1.154 以上**，付費方案（Pro／Max／Teams）皆可。

### 🔑 為什麼需要它：預設 harness 的三個失敗模式

預設 Claude Code harness 是**為寫程式設計**的，且在**一個 context window 內同時負責規劃 + 執行**。這對一般 coding 很好用，但遇到「任務超長需反覆嘗試／高度平行（上百上千小項）／結構複雜且需 adversarial 驗證」時會撞到：

1. **Agentic laziness（懶人上身）** — 任務太大太多步時，Claude 會在部分進展後**提早說完成了**。例：安全檢查該看 50 項，只看了 20 項就收工
2. **Self-preferential bias（自我偏好偏差）** — 要它檢查自己的產出時容易**偏袒自己原本的結論**，尤其是請它照 rubric 評分或自我驗證時，容易高估自己的方案
3. **Goal drift（偏離目標）** — 長對話中每次壓縮、整理、總結都會掉一些細節，極端情況只記得大方向，**邊界條件與「千萬不要做 X」的設定都會消失**

解法是用**多個分離的 Claude × 多個 context window × 專注的小目標**來對付：不再呼叫一個 Claude 做完所有事，而是讓多個 subagent 各自扮演角色來產生、驗證、對抗、比稿、整理。

（🔑 第 2 點正是 [[agent-bug-fix-eval-green-is-not-correct]] 那次「改測試讓自己變綠」的機制性解釋；第 3 點對應 [[agent-context-management]] 的「壓錯東西比 Overflow 更危險」）

### 六種 workflow pattern（理解後可任意組合）

1. **Classify-and-act（分類後再行動）** — 先讓分類 agent 判斷「這是哪一種任務」，再分派給對應 agent 或策略。⚠️ 也可以**反過來**：跑完整個流程，最後才讓分類 agent 判斷輸出格式
2. **Fan-out-and-synthesize（先分開再合併）** — 大任務拆成小步驟，每步在自己的乾淨 context 裡單獨跑完，最後由彙總 agent 合併結構化結果。適合**上百個小項目**或**每一步都該獨立思考**的情境
3. **Adversarial verification（對抗式驗證）** — 每開一個產出 agent，就再開一個專門挑錯的 agent，依既定 rubric **嚴格檢查甚至刻意攻擊**前者的輸出
4. **Generate-and-filter（產生＋篩選）** — 先大量生成想法，再用 rubric 或 verifier 篩選去重，只留品質最好且經檢驗的
5. **Tournament（錦標賽）** — 多個 agent 同時解同一問題，評審 agent 比較淘汰，冠軍即最佳解
6. **Loop until done（做到真的沒東西為止）** — **不預設迭代次數**，只給目標，讓 workflow 自己規劃並持續產生新 agent 直到滿足目標

### 執行模型的三個技術細節

- workflow 本身就是一支 **JS 腳本**，旁邊有內建工具（JSON、Math、Array 等）方便處理資料
- 可決定**每個 subagent 用哪個模型**，以及**要不要在獨立 worktree 中跑**（上下文隔離）
- 🔑 **執行中被中斷（例如關掉 terminal），下次 resume session 後可從中斷處接著跑**

### 官方實際用例

- **大型 Migration／Refactor** — **Bun 把核心從 Zig 改寫成 Rust** 就是用 workflows 完成的：先把任務拆成 callsite／失敗測試／module 等多個維度 → 每個 fix 開一個 subagent 在自己的 worktree 修改 → 另一個 agent 做 adversarial review（找問題、比對規格）→ 通過後 merge。⚠️ 可限制它不要亂用吃資源的指令，以在機器資源內最大化平行度
- **Deep Research** — 內建的 `/deep-research` 技能本身就是一個動態 workflow：同時展開多個網路搜尋 → 驗證 agent 檢查來源品質並對抗式查核 → 彙整成附引用來源的報告。同架構可用於「從 Slack 抽一段期間的狀態報告」「深入探索某 feature 在 codebase 裡怎麼實作」
- **Deep Verification（全量事實查核）** — 先整理出報告裡所有需查證的事實陳述 → 每條派獨立 agent 找來源佐證 → 另一組驗證 agent 檢查來源品質

⚠️ 原文脈絡：Anthropic 團隊過去為了做到最佳表現，會**另外在 Claude Code 上疊自訂 harness**（用於 Research／Security analysis／Agent teams／Code Review），而動態工作流程讓這些能在 Claude Code 裡**原生打造、重用與分享**。（對照 [[harness-engineering]] 的第三維度論）
