---
title: Cognition 實測：Fable 5 當「主管」比 Opus 4.8 更便宜——委派策略決定 AI 代理成本
date: 2026-07-27
updated: 2026-08-05
tags: [ai/claude, ai/agent, ai/workflow]
source: https://www.bnext.com.tw/article/91566/ai-management-fable-5-opus
type: article
status: processed
asset_value: 80
---

## T（核心論點）

AI 新創 Cognition 用 3,000 個評估工作階段實測發現：把 Devin 的「領導模型」從 Claude Opus 4.8 換成單價貴兩倍的 Claude Fable 5 後，**搭配同一個便宜 Sidekick 副代理，整體任務成本反而下降、分數更高**。差異不在模型執行能力，而在**當主管的判斷力**——何時委派、如何交辦、如何驗收；Fable 平均只需 11.5 回合、Opus 要 26.5 回合（token 用量 Fable 約為 Opus 的三分之一）。

## B（適用場景）

**適用：**
- 設計 Multi-Agent 委派架構（領導模型 + 便宜副代理）時決定用哪個模型當 Supervisor
- 評估「升級到更貴的模型」是否划算——本案例證明**貴模型 + 好委派 < 貴模型自幹**，也 **< 便宜模型自幹**
- 設計 Agent 交辦說明的品質標準（規格完整度直接影響下游代理表現）

**不適用 / 注意：**
- **委派不是萬用解法**：短任務（少數回合可完成）或依賴連續判斷、累積脈絡本身就是解題關鍵的序列式除錯（如序列式 debug），Fable 也幾乎不委派——先判斷任務是否可拆解
- 推理強度不同（Fable low / Opus medium）是變因之一，非唯一控制變數
- **未經第三方獨立驗證**：數據來自 Cognition 自建 FrontierCode 1.1 評測與自家 Devin Fusion 架構，「規格完整才拿 94 分 vs 25 分」的雜湊任務案例是**單一挑選案例**，非整體平均

## R（關鍵數據與機制）

**單獨執行（無 Sidekick）：**
| 模型 | 分數 | 平均成本 |
|---|---|---|
| Fable 5 | 60.8 | $4.03 |
| Opus 4.8 | 55.4 | $3.06 |

**搭配同一個 Sidekick 後（成本排序反轉）：**
| 配置 | 分數 | 平均成本 |
|---|---|---|
| Fable + Sidekick | 60.7 | $1.86 |
| Opus + Sidekick | 54.6 | $2.04 |

→ Fable 搭配後成本**降 54%**（$4.03→$1.86），分數僅微降（60.8→60.7）；Fable 版本比 Opus 版本**便宜 8.8%、分數高 6.1 分**。

**行為差異（排除「委派更頻繁」的解釋——兩者委派次數相近，平均每任務約 3 次）：**
- **回合數**：Fable 11.5 回合 vs Opus 26.5 回合；**輸出 token**：Fable ~6,100 vs Opus ~19,000（約 1/3）
- **親自動手比例**：81% 的 Fable 主導測試中，領導模型**未親自修改任何一行程式碼**；Opus 只有 24%
- **完全不讀程式碼**：13% 的 Fable 測試中，領導模型從頭到尾未讀取任何程式庫檔案
- **交辦說明品質**：Fable 通常列出限制條件、邊界情境、完成標準（規格完整的設計文件）；Opus 常先自行探索、實作 20-45 回合才交辦收尾
- **驗收行為**：Opus 把副手檔案拉回自己脈絡重看的次數多 2 倍、親自修改次數多 4 倍，極端時整個推翻重寫——但**投入更多成本檢查，並未展現更高正確率**

**比喻**：Opus 像緊盯實習生的微管理主管；Fable 像信任能幹工程師、懂得委派的經理。

## C（關鍵行動）

- [ ] 設計 Supervisor/領導模型時，優先評估「委派判斷力」而非只看單價或單獨執行分數
- [ ] 交辦任務前先寫清楚限制條件、邊界情境、完成標準（規格完整的設計文件），別讓副代理猜
- [ ] 先判斷任務可拆解性：短任務/序列式除錯不委派，可拆解的任務盡早在早期就交辦而非先自己做 20+ 回合
- [ ] 驗收副代理成果時避免無差別重新檢查/重寫——先做一次成本低的差異檢查，抓到問題再精準修正

## 補充資料：成本拆分表與「為何要並行」（2026-08-07 併入）

來源：〈如何讓 Fable 比 Opus 更便宜：用委派改寫 Agent 的成本結構〉（同一 Cognition 分析的另一轉述）＋ Kaplan 訪談（見 [[devin-two-years-intelligence-saturation]]）。補上本頁原本缺的兩塊。

### 🔑 主導／副手成本拆分（原本只有總成本）

| | 主導 $ | 副手 $ | 每次總成本 $ | 主導回合數 | **主導累計輸入 token** |
|---|---|---|---|---|---|
| **Fable + 副手** | $1.28 | **$0.58** | **$1.86** | 11.5 | **545k** |
| **Opus + 副手** | $1.73 | **$0.31** | $2.04 | 26.5 | **1,679k** |

**兩個反直覺的讀法**：
1. 🔴 **Opus 的副手成本更低（$0.31 vs $0.58）——因為它委派得少**。它把錢花在自己身上（$1.73 vs $1.28）。所以「副手花得多」反而是**委派健康**的訊號，不是浪費
2. **累計輸入 token 差 3.08 倍**（545k vs 1,679k）——比本頁原記的「輸出 token 約 1/3」更能說明差距來源：Opus 反覆把脈絡拉回自己身上重讀

推理強度標註同時確認了本頁的變因警語：**Fable 5(low) vs Opus 4.8(medium)**。

### 🔑 為什麼 sidekick 要「並行」而非按需切換

Kaplan 訪談給出本頁原本沒有的設計理由：

> **兩個模型並行的用意是讓雙方都保有任務脈絡**——若只在需要時才切換，**接手者很難立即取得同等理解**。

兩者共享大量脈絡，也頻繁把資訊寫回檔案系統以避免超出脈絡視窗；再配合**注意快取的路由決策**，使 Devin Fusion 在品質略有提高的同時，價格效能改善約 **35%**。

→ 這正是下一節「架構不可類比」的根本原因：**Claude Code subagent 每次 fresh context，結構上做不到「脈絡對等的接手」**。

### 委派幫不上忙的兩種任務（原文明列）

- 只包含少數幾個主導模型回合的**短任務**——在「決定」與「交付」之間沒有東西可以委派
- **序列式的除錯任務**，其根因追查是一長串連續判斷——**累積起來的上下文本身就是工作**

## ⚠️ Sidekick ≠ Claude Code subagent：架構不可直接類比（2026-08-05 查證官方文件後補）

本篇最容易被誤讀成「指定 Opus 當指揮者，它就會依難度自動指派模型給 subagent」。**查 Cognition 與 Claude Code 兩邊官方文件後，這個推論不成立**——兩邊的「副手」是不同的東西。

### 三個實質差異

| | **Devin Fusion Sidekick**（Cognition 官方）| **Claude Code subagent**（官方文件）|
|---|---|---|
| **並行性** | **常駐並行**：「run two parallel agents… Both are fully capable agents with their own toolsets」 | **按需生成**，父代理等它回傳摘要 |
| **context** | 「Both the main model and sidekick model maintain their own **persistent, cached contexts**」 | 「Each subagent starts with a **fresh, isolated context window**… **Claude composes a delegation message that summarizes the task**」 |
| **模型路由** | 🔴 **有自動難度路由**：「**lightweight classifiers during task execution** to signal when we need to switch to the main agent or use a different model entirely」；切換發生在 **context compaction 點**（本就要刷 cache），所以換模型 free、複雜度上升可 mid-session 升級 | **無自動判斷**。四層靜態解析：① `CLAUDE_CODE_SUBAGENT_MODEL` 環境變數 → ② 呼叫時 `model` 參數 → ③ agent frontmatter `model:` → ④ 主對話模型（**省略即 `inherit`**）|

### 🔴 因此「省 54%」不能假設會轉移到 Claude Code

Devin 的省錢有一部分來自**持久化 cached context**——委派時不必每次重付「解釋任務」的成本。Claude Code 的 subagent **每次都是全新 context，父代理每次都要重新 compose 一份 delegation message**。

→ 同一筆「子代理成本乘法」在 Claude Code 上**結構性地更嚴重**（Cognition 是專門工程繞過了這件事）。引用本篇數據談自己的 Claude Code 成本時，這是必須先扣掉的部分。

### 另一個常見誤讀：本篇的變數不是「選模型」

實驗設計是**把 Sidekick 固定住**，只換領導模型（Opus 4.8 → Fable 5）。變數是**領導者的委派判斷力**（何時委派、交辦說明完整度、要不要親自重看），**不是「它會挑哪個模型」**。

### Claude Code 裡對應 Sidekick 的是 agent teams，不是 subagent

官方文件自己分流：subagent＝單一 session 內一次性委派／**agent teams＝「for sessions that communicate with each other」「give each worker its own independent context」（Sidekick 的結構對應物）**／background agents＝平行跑多個獨立 session 並集中監看／fork＝繼承整份對話（放棄輸入隔離換取不必重新解釋）。

→ 要在 Claude Code 手動實作類似分工，配方見 [[fable5-subagent-model-routing-design]]（judge/builder/worker 三角色，`model:` 只寫在 agent frontmatter、不寫死在 skill）；最省事的單一槓桿是 `CLAUDE_CODE_SUBAGENT_MODEL` 環境變數（優先序最高）。

> 📌 **順帶記一個反向事實**：Claude Code 內建 `Explore` agent **原本固定跑 Haiku，v2.1.198 起改為繼承主對話模型**（Claude API 上以 Opus 為上限）——唯一存在自動降級路由的地方被官方移除了。這與 Devin 往「加一層 classifier 自動路由」的方向**相反**。

## 原始摘要

Cognition 工程師 Joon Hee Lee 於 2026-07-13 在公司官方部落格發表分析。Cognition（Scott Wu 創立，Devin 全自動 AI 軟體工程師開發商）用自建 FrontierCode 1.1 評測，針對四種模型配置合計執行 3,000 個評估工作階段，衡量程式碼能否通過測試及是否具備可合併品質。核心結論：隨著 Sidekick 副代理模型越來越便宜、能力越來越強，未來值得為高階模型付高價的可能不只是親自執行能力，更是「當主管」的判斷力——何時委派、設什麼限制、交給誰。（bnext 業配與延伸閱讀段落已略）

## 相關連結
- [[synthesis/synthesis-llm-cost-routing]] — **本篇為其來源之一**（2026-09-03 重跑，6→8 篇）：槓桿一（任務路由）的新實測——委派策略決定代理成本
- [[synthesis/synthesis-llm-cost-routing]] — LLM 成本效率知識精粹；**本篇是「弱規劃者+強執行者優於純強模型」AgentOpt 論點的最新實測案例**，且補上「領導模型的委派判斷力」這個新變數
- [[synthesis/synthesis-multi-agent]] — Multi-Agent 系統知識精粹；本篇是 Supervisor 模式的真實生產數據，直接回應該精粹 V 層「評測與失敗模式全面缺席」的缺口——但仍是廠商自評，未解決該缺口
- [[claude-pilot-suite]] — Claude Pilot 執行手冊；AgentOpt 論文的「弱規劃者+強執行者」與本篇「貴模型當主管+便宜副代理」是同一策略的不同案例
- [[claude-model-selection-guide]] — Claude 模型選擇指南；本篇補上「選模型不只看單價，要看委派判斷力」的實測依據
- [[claude-fable-5-usage-guide]] — Fable 5 使用指南
- [[claude-fable-5-power-tasks]] — Fable 5 高難度任務案例
- [[opensquilla-intelligent-model-router]] — 🔑 **同機制的開源實作**：Devin Fusion 的「lightweight classifiers during task execution」＝ SquillaRouter 的 on-device LightGBM+ONNX 逐 turn 難度分類（T0–T3）。**想要自動難度路由不必用 Devin，但兩者共同的缺口相同——路由準確率均無公開驗證**；且 OpenSquilla 是微核心 agent 本體、非 Claude Code 外掛，採用等於換 harness
- [[fable5-subagent-model-routing-design]] — Claude Code 上**手動**實作角色分級路由的配方（judge/builder/worker，`model:` 只寫在 agent frontmatter）；零遷移成本的層級 1 做法
