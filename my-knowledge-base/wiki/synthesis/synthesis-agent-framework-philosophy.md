---
title: Agent 框架設計哲學 知識精粹
date: 2026-05-22
source_count: 5
synthesis_snapshot_date: 2026-09-03
tags: [ai/agent, dev/architecture]
source: synthesis
type: synthesis
status: processed
sources: 5 篇
asset_value: 86
---

整合四篇從不同層次討論 Agent 框架設計的條目（Harness 環境工程、Hermes 學習迴圈、Hermes 完整架構解析、四框架穩定性比較），萃取框架選擇的真正判斷標準與各框架「穩定性哲學」的根本分歧。

## E（精粹論點）

Agent 框架的核心分歧不在功能列表，在**對「穩定性」的哲學定義**：Claude Code 說穩定 = 執行前預防（不讓壞事發生）；OpenClaw 說穩定 = crash 後恢復（發生了能修補）；CrewAI 說穩定 = 動態重派（失敗了能改路由）；Hermes 說穩定 = 持續學習（越用越少失敗）。選框架前必須先回答：**「在你的場景裡，Agent 壞掉最壞的情況是什麼？」**——這個問題的答案，才是正確的框架選擇依據。

---

> 🔴 **2026-09-03 重跑（4→5 篇）：OpenClaw 那一派的「crash 後恢復」被拆開了。**
>
> 原 E 層把四種穩定性哲學並列，其中 OpenClaw = 「發生了能修補」。[[agent-error-recovery-taxonomy]] 顯示**「能修補」本身不是一個能力，而是八種策略**，而且最常見的實作是錯的：
>
> 🔑 **Retry 是 Recovery 的一種，不是全部——Retry 解決暫時失敗，Recovery 解決「下一步應該怎麼改」。**
> 🔴 **如果所有錯誤都用 Retry，系統看起來很有韌性，但它沒有真正理解失敗，只是把同一個錯誤多執行幾次。**
>
> 因此 Recovery 的第一步不是執行而是**分類**。這對本精粹的選型問題有直接影響：**問「這個框架有沒有錯誤恢復」是問錯了，該問「它把錯誤分成幾類、每類的動作是什麼」**——只提供 retry 的框架，在「crash 後恢復」這個維度上其實是空的。

## C（共識核心）

> 出現於 4/4 篇來源

- **可插拔機甲是所有框架的共同基礎**：工具 / 記憶 / 模型 / Skills / 子 Agent / Plugin 均可替換——可插拔是結構共識，插什麼才是真正分歧（[[agent-framework-stability-comparison]]）；Hermes 以「自我註冊式 Registry（工具 import 即掛載）+ 可插拔記憶供應器（Honcho/mem0/supermemory）+ MCP 作工具池」把可插拔落實到實作層（[[hermes-agent-architecture-deep-dive]]）
- **正確的 Harness 比更好的模型影響更大**：Manus 同一模型重寫 5 次 Harness，每次提升 > 換模型（[[harness-engineering]]）；四個框架的差異本質是 Harness 設計哲學不同（[[agent-framework-stability-comparison]]）
- **環境設計的品質決定 Agent 的天花板**：Guides × Sensors 四象限（[[harness-engineering]]）；「可驗證」的任務有 RL 優化，框架能在此類任務提供更穩定的 Harness（[[harness-engineering]]、[[agent-framework-stability-comparison]]）

---

## A（獨特視角）

| 來源 | 獨特貢獻 |
|------|---------|
| [[harness-engineering]] | **Guides（前饋）× Sensors（回饋）四象限**：大多數團隊只有左下角（linter + 測試），缺乏右上（系統性前饋）和右下（語義審查）；**Harness 越做越薄**的反直覺洞察（更精準，不是更複雜）；Vercel 把工具從 15 砍到 2 → 準確率 80%→100%、Token 少 37%；**第一讀者從人變成 AI** 的設計哲學轉變；多 Agent 四大陷阱（無限迴圈、無終止條件、觸發繞過、無法沉默） |
| [[hermes-agent]] | **「執行即學習」**：每次任務後自動萃取技能寫回 Skills Hub，學習是持續自動的；**Honcho 動態使用者建模**（跨 session 分析對話風格、拒絕過的選項）；**stateless vs stateful 的根本分歧**——傳統框架每次從零開始，Hermes 跨 session 累積；長效記憶效果在一個月後才顯現 |
| [[agent-framework-stability-comparison]] | **四維度決策框架**：context 管理（信任 LLM vs 確定性算法）/ 控制流（扁平迴圈 vs 有狀態樹 vs 宣告式 DAG）/ 錯誤恢復（執行前預防 vs 執行中偵測 vs 執行後修補 vs 層次升級）/ 反饋迴路（自動學習 vs 社群插件 vs 人設計）；**OpenClaw 的 Identifier Preservation Policy**（LLM 壓縮時傾向改寫 UUID/IP，strict 模式強制保留） |
| [[hermes-agent-architecture-deep-dive]] | **唯一把哲學拆到可實作層**：六大設計原則（平台無關核心 / **提示詞穩定性**——system prompt 一次組裝不變動以命中 Cache 降 10 倍成本 / **漸進式揭露 3 層**支撐 47 個工具 / 自我註冊勝過中央清單 / 設定檔隔離 / Agent 自己擁有學習素材）；**分層防 Prompt Injection**（XML 強制解析 → Tool Approval → Sandbox）；迭代預算退還、壓縮而非截斷、Ctrl-C 乾淨中斷等迴圈細節；**14 天 9 階段自建清單**（把「選框架」轉成「自己蓋一個」的第三條路）|
| 🆕 [[agent-error-recovery-taxonomy]] | 🔑 **把「錯誤恢復」這個維度從一個勾選項拆成八種策略**：Retry ≠ Recovery（前者解決暫時失敗、後者解決「下一步怎麼改」）；🔴 **全用 Retry 的系統只是把同一個錯誤多執行幾次**；Recovery 第一步是**分類**而非執行 |

---

## V（知識缺口）
- 🆕 🔴 **四種穩定性哲學都缺「錯誤分類表」（2026-09-03）**：[[agent-error-recovery-taxonomy]] 提供了八種 Error Type 對應八種策略的分類法，**但四個框架各自把錯誤分成幾類、覆蓋哪幾種，沒有任何來源做過對照**。這使得「選框架前先問最壞情況是什麼」目前只能問到哲學層，問不到覆蓋率

- ❓ **Hermes 學習迴圈的 Model Collapse 風險**：自動萃取技能並反覆讀回，長期是否有細節磨平、風格單一化的問題？（Nature 2024 論文已在 LLM Wiki 場景警告此風險，Agent 場景未見對應討論）
- ⚠️ **Harness 「越做越薄」的邊界**：何時 Harness 是必要設計，何時是過度工程化補償模型能力？缺乏可操作的判斷標準
- ❓ **框架混用的整合複雜度**：comparison 提到分流使用（OpenClaw 接 Telegram、Hermes 接 Discord）在理論上可行，但跨框架的 Skills、記憶、工具共享的具體整合成本沒有評估
- ❓ **「自建」這條路缺乏成本對照**：[[hermes-agent-architecture-deep-dive]] 給出 14 天自建清單，證明核心並不神祕；但**自建 vs 選現成框架的長期維護成本**（跟上模型/協定演進、安全修補）沒有任何一篇評估——14 天蓋起來之後呢？
- ⚠️ **安全層級的落差沒被拿來比較**：只有 Hermes 明確列出三層 Prompt Injection 防護，其餘框架的對應機制在比較篇中未被當作維度——**安全其實是「穩定性」的一部分，卻沒進四維度框架**

---

## 決策速查：按「最壞情況」選框架

| 最怕的失敗模式 | 建議框架 | 理由 |
|--------------|---------|------|
| AI 執行造成不可逆傷害 | Claude Code | 執行前預防，exit 2 永遠攔截 |
| 多 Agent 協作中途崩潰 | OpenClaw | Orphan Recovery，crash 後仍能追蹤 subagent 狀態 |
| 複雜工作流難以表達 | CrewAI | 宣告式 DAG，AND/OR 觸發，靜態渲染路由圖 |
| Agent 學不到新技能、永遠重複失敗 | Hermes | 程序性記憶，每次任務後自動萃取技能 |

---

## 整合行動清單

- [ ] 設計 Agent 系統前，先填答「最壞情況」問題，再對照決策速查表選框架
- [ ] 檢查現有 Harness 覆蓋的象限：是否只有左下角（linter + 測試）？右上（系統性前饋）和右下（語義審查）是否空白？
- [ ] Harness 設計原則：約束架構決策（API 走哪層），不約束實作細節（變數命名）——給 Agent 解題自由
- [ ] 多 Agent 系統必須在設計初期定義：① 誰能觸發誰 ② 什麼條件下任務結束 ③ Agent 看到不該回應的訊息時怎麼辦

---

## 來源條目

- [[harness-engineering]] — Guides × Sensors 框架，護城河在環境（Manus 5 次重寫數據、越做越薄原則）
- [[hermes-agent]] — 三層長效記憶 + 學習迴圈，stateless vs stateful 根本分歧
- [[agent-framework-stability-comparison]] — 四框架四維度比較：context / 控制流 / 錯誤恢復 / 反饋迴路
- [[hermes-agent-architecture-deep-dive]] — Hermes 完整架構：六大設計原則、漸進式揭露 3 層、分層防注入、14 天自建清單（AV 76）
- [[agent-error-recovery-taxonomy]] — 🆕 🔑 Error Recovery 不是 Retry：八種 Error Type 對應八種策略（AV 80）
