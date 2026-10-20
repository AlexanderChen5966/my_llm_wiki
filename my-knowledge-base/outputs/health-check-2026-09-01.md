---
title: Wiki 健康檢查報告 — 2026-09-01
date: 2026-09-01
tags: [pkm/workflow]
source: wiki-health-check v1.7.0
type: reference
status: processed
---

# Wiki 健康檢查報告 — 2026-09-01

> 🔴 **本報告的自我更正（2026-09-03，執行優先清單時發現）**
>
> 執行修復時比對了 `health-check-2026-08-12.md`，發現本報告有**兩項實質錯誤**，且兩項都源於同一個疏失——**沒有先讀前一份報告**（此缺陷已寫進 skill v1.8.0 的執行流程第 1 步）：
>
> | 項目 | 本報告寫的 | 實際 |
> |---|---|---|
> | **問題 4（懸空連結）** | 8 個，其中 2 個標為「🔴 真 bug」 | 🔴 **只有 2 個會在 Obsidian 顯示為斷鏈**——其餘 6 個都在 ```` ``` ```` 圍籬或行內 code 內（掃描未剝除程式碼區塊）。而剩下那 2 個（指向 skill 名稱與記憶檔）**08-12 報告已判定為「刻意的外部參照」**，本報告重複列出還升級成「真 bug」。**此項全部撤銷，無事可做** |
> | **問題 1（矛盾）** | 只找到 1 處矛盾 | 🔴 **08-12 報告列出 6 處，全部仍未修**，本報告只重新抓到其中 1 處（矛盾 4）。**其餘 5 處是漏報，不是已修** |
>
> 因此真正的優先順序是「先修 6 個矛盾」而非「修斷鏈 + 重跑精粹」。**六個矛盾與兩篇精粹皆已於 2026-09-03 處理完畢**，處置記錄見文末〈2026-09-03 執行紀錄〉。



## 整體評分

| 項目 | 數值 |
|---|---|
| wiki 條目數 | **450**（主層 431／synthesis 13／concepts 6，不含 INDEX）|
| research／ideas | 56／94 |
| raw 素材 | **943**（`.md` 919／非 md 24）｜**已處理 943 / 未處理 0** |
| 問題總數 | **12**（🔴 嚴重 2／🟡 警告 5／🟢 建議 5）|
| 健康指數 | **良好** |

> 本次是本庫首次出現 **raw/ 完全清空**（919/919 `.md` 皆 `status: processed`、24/24 非 md 皆有 `.processed` 標記、**0 個孤兒標記**）。

---

## 🔴 嚴重問題（需立即處理）

### 1. 矛盾：`claude-md-guide` 把未經證實的數字歸給官方文件

**⚠️ 矛盾：[[claude-md-guide]] vs [[claude-md-optimization-techniques]] / [[claude-5-context-engineering-rules]]**

`claude-md-guide`（2026-05-14 建立，**全篇無任何查證註記**）第 188 行寫：

> 「**Anthropic 官方文件明確指出** CLAUDE.md 本質上是建議性的，Claude 約 80% 的時間遵循它。**一旦超過 200 行，遵守率明顯下降**——重要規則被噪音淹沒。」

但本庫後續兩次查證的結論與此衝突：

| 來源 | 結論 |
|---|---|
| [[claude-md-optimization-techniques]]（2026-07-27 實際查證）| 「78%→76%→52% 遵守率…**全部無法外部複現**」；「≤ 14 條」標為**僅社群實測、無官方依據** |
| [[claude-5-context-engineering-rules]]（**官方一手來源**）| 「200 行的意義釐清：**不是超過就失效**，而是提醒檔案持續變長很可能混進只適用特定任務的流程」 |

**問題**：`claude-md-guide` 把「超過 200 行遵守率明顯下降」寫成官方明確指出，而官方一手條目已明白walk back「不是超過就失效」；且該篇沒有任何指向後續查證的註記，單獨讀到它的人會取得已被本庫推翻的認知。

（註：「約 80% 遵守率」這半句與 `claude-md-optimization-techniques:40` 一致，**不是問題所在**；問題只在後半句的因果強度與官方歸屬。）

**建議**：以 `claude-5-context-engineering-rules`（官方一手）為準。在 `claude-md-guide:188` 加上修正註記並連到兩篇查證條目，把「遵守率明顯下降」降級為社群實測傾向。

### 2. 精粹過時：兩篇核心精粹的來源已大幅增長

第 7 項偵測顯示 13 篇精粹**全部有 `source_count` 與 `synthesis_snapshot_date` 戳記**（無缺戳記者）。依「來源清單 vs 快照後主題相關新增」比對，以下兩篇已明顯過時：

```
🔄 精粹過時：[[synthesis/synthesis-agent-engineering-architecture]]
快照：8 篇（2026-07-24）→ 快照後主題相關新增約 11 篇（已剔除 glob 誤抓）
新增候選：[[agent-tool-runtime-seven-layers]]、[[agent-tool-authorization-gatekeeper]]、
          [[agent-control-layer-permission-hooks-planning]]、[[agent-permission-and-os-sandbox]]、
          [[subagent-four-boundaries]]、[[agent-error-recovery-taxonomy]]、
          [[agent-loop-stop-conditions]]、[[agent-context-management]]、
          [[agent-memory-lifecycle]]、[[agent-task-system-background-scheduling]]、
          [[concepts/agent-engineering-vocabulary-layers]]
建議：確認後重跑 knowledge-synthesizer agent-engineering-architecture
```

```
🔄 精粹過時：[[synthesis/synthesis-claude-md]]
快照：5 篇（2026-07-27）→ 新增高相關條目 [[apple-claude-md-teardown]]（av 85，
      **高於現有 5 篇來源的任何一篇**），且它**修正了來源之一** claude-md-optimization-techniques 的技巧 3
建議：確認後重跑 knowledge-synthesizer claude-md
```

**另外五篇為中度過時**（新增 1–4 篇主題相關條目，可延後）：`synthesis-claude-code-internals`（+`steering-claude-seven-carriers`、`system-prompt-assembly-pipeline`）／`synthesis-multi-agent`（+`subagent-four-boundaries`、`git-worktree-parallel-agents`）／`synthesis-prompt-engineering-evolution`（+`context-engineering-stacking`、`system-prompt-assembly-pipeline`、`context-engineering-hiring-gap`）／`synthesis-karpathy-llm-wiki-method`（+`karpathy-opus5-lotr-3d-case`）／`synthesis-agentic-engineering`（+`ai-engineering-terms-genealogy`、`graph-engineering-node-edge-state`）。

> 🔴 **本次發現偵測方法本身有一個 false negative**，值得回寫進 skill：
> `synthesis-claude-md` 用 skill 現行的「`source_count` vs glob 計數」比對得到 **5 → 5「OK」**，但實際是巧合——glob `wiki/*claude-md*` 抓到了新條目 `apple-claude-md-teardown`，卻**漏掉來源清單裡的 `claude-5-context-engineering-rules`**（檔名不含 claude-md）。**兩邊同時偏差、方向相反，互相抵銷成假陰性。**
> **建議把 skill 第 7 項的判準改為「解析精粹自己的『來源條目』清單 → 找快照日後新增且未列入的同主題條目」**，而不是比對 glob 總數。本次報告已改用後者。

---

## 🟡 警告（建議本週處理）

### 3. 🏝️ 孤立筆記 27 篇（wiki 內 0 入連結，不計 INDEX）

其中 **3 篇是本次新建**、尚未被其他條目回連：

- `ai-learning-series-mirror-and-method`、`ai-prompts-for-unclear-boss-instructions`（本日新增，屬性偏獨立主題）
- `ai-dev-workflow-taxonomy`（concepts 層，⚠️ **這篇是導航地圖卻沒有任何條目連進來**，值得優先補）

其餘 24 篇多為工具型／單點條目：`agent-readiness-cloudflare-audit`、`ai-agent-reverse-workflow-decomposition`、`ai-memory-cross-agent-handoff`、`cc-gui-intellij-claude-code-plugin`、`chatgpt-blender-mcp-3d-dashboard-case-study`、`chatgpt-deep-research-guide`、`claude-code-best-practices-index`、`claude-code-output-styles-concise`、`claude-tag-slack`、`claude-text-watermark`、`context7-mcp-server-guide`、`cursor-origin-git-forge`、`kotlin-toolchain-amper-merger`、`loom-ai-async-screen-recording`、`microsoft-mcp-for-beginners-course`、`obsidian-visual-skills`、`odysseus-self-hosted-ai-workspace`、`okf-open-knowledge-format`、`open-source-web-scraping-tools-2026`、`paper-radar-literature-monitor`、`system-prompts-leaks-repo`、`warp-agentic-dev-environment`。

⚠️ `wiki/b2b-manager/tasks.md`、`troubleshooting.md` 是專案文件而非知識條目，**不應列入孤立統計**（掃描時 `wiki/**` 遞迴誤收）。

### 4. 🔗 懸空連結 8 個——其中 5 個是誤用語法而非真缺頁

| 連結 | 出處 | 判定 |
|---|---|---|
| `[["$cmd" =~ ^(npm\ test]]` | `claude-code-cost-optimization-7-tips` | 🔴 **真 bug**：bash 程式碼未包 code fence，被當成 wikilink |
| `[[feedback_framework_design_decisions]]` | `notebooklm-kit-academic-expansion-plan` | 🔴 **真斷鏈**：指向 Claude 記憶檔（不在 vault 內），Obsidian 永遠解析不到 |
| `[[skills.config]]` | `codex-desktop-2026-onboarding-guide` | 應改為行內 code |
| `[[wiki-health-check]]` | `claude-fable-5-power-tasks` | 指的是 skill 不是筆記，應改為行內 code |
| `[[wikilink]]`／`[[wikilinks]]` | `karpathy-knowledge-base-method`、`karpathy-llm-wiki-improvements`、`llm-wiki-karpathy-vs-nashsu` | 舉例用語，建議改行內 code 免得 Obsidian 顯示斷鏈 |
| `[[Previous Analysis]]`／`[[Redis Paper]]` | `karpathy-llm-wiki-improvements` | 原文示意用，同上 |

### 5. 📥 未處理素材：**0 篇**

`.md` 919/919 皆 `processed`；非 md 24/24 皆有 `.processed`；**孤兒標記 0 個**。

ℹ️ 另有 **360 個舊制殘留的 `.md.processed` 旁車標記**——依 `rules/inbox-detection-protocol.md` 不具判斷效力。目前無害（不會造成誤判），但屬可清理的雜物。

### 6. ⚠️ 待人工確認（cross-reference 覆核）

已標 `processed` 的 919 篇中，**398 篇的 `source` URL 在 wiki/research/ideas 未命中**（命中率 **57%**）。

> ⚠️ 依 skill v1.7.0 明訂，此項**不可當結論**：多篇素材常合併成一條條目、素材可能未達存入門檻而刻意不建條目、URL 也未必一致。協議記錄的實測基線是三到四成，**本次 57% 高於基線，屬正常範圍**，不需行動。

### 7. 過度集中節點（可能需要拆分）

| 入連結數 | 條目 |
|---|---|
| 49 | `claude-code-context-management` |
| 46 | `harness-engineering` |
| 34 | `loop-engineering-6-components`／`agent-skills-vs-mcp` |
| 32 | `karpathy-knowledge-base-method` |

前兩篇已是事實上的樞紐條目。⚠️ `claude-code-context-management` 同時承載「上下文管理」與「Auto Memory 機制（MEMORY.md 200 行/25KB 載入限制）」兩個主題，49 個入連結中兩類混雜——**可考慮把 Auto Memory 段落拆出**，或確認 `claude-code-hooks-memory` 是否已足夠承接。

---

## 🟢 建議（下次處理）

### 8. 📭 知識缺口：7 個概念被提及但無專篇

由本日新建的 [[concepts/agent-engineering-vocabulary-layers]] 系統性標出，**其中 3 個是真缺口**：

```
📭 World Models for Agents      被 2 篇提及，無任何實質內容
📭 Long-Horizon Agent           被 2 篇提及，僅 claude-model-selection-guide 出現過「長時程」一詞
📭 RACG（檢索增強程式生成）      被 2 篇提及，相鄰的只有通用 RAG 與程式庫索引條目
```

🔑 **三者指向同一方向：本庫強在「單次任務的可靠性」，弱在「跨時程的自主性」**——列為下一輪選材優先方向。

另 4 個為**分散有覆蓋、缺專篇**：`Eval Harness`（散在 search-eval-set-design 等 4 篇）／`Agent Observability`（散在 production-agent-architecture 等 4 篇）／`Native Multimodal Agents`／`GRPO`（被 4 篇提及，本庫對齊演算法只到 RLHF vs DPO）。

### 9. 📅 內容過時：**0 篇**

無任何條目建立日期早於 6 個月前（本庫最早條目 2026-05-13，尚未進入 6 個月門檻）。

**版本號硬編碼**可留意的 3 篇：`claude-code-cross-session-messaging`（v2.1.224／225）、`hermes-agent`（v0.4.3→v2026.4.8）、`omniroute-local-ai-gateway`（v3.8.49–v3.9.0）——目前皆為「記錄於該版本」的事實陳述，非規範性主張，暫不需處理。

### 10. 🔍 缺乏來源：**0 篇**

全部 431 篇主層條目 frontmatter `source` 皆有值。

### 11. 建議補一處可信度交叉連結

`apple-claude-md-teardown` 引用「Opus 5 上線時把 Claude Code 自己的 system prompt 砍掉超過 80%」時**未帶可信度註記**，而 `claude-5-context-engineering-rules:135` 已記錄第三方指認：官方唯一給出的量化說法只有「移除超過 80%」與「評測無可測量損失」，社群流傳的「800 token→164 token」等具體數字**並未經官方確認**。建議在新條目補一句指向該註記。

### 12. 同前綴叢集提醒

`claude-code*` 已達 **47 篇**。目前由 `synthesis-claude-code-internals` 與 `claude-code-best-practices-index`（索引型）分擔導航，但後者本身是孤立條目（見問題 3）。建議把 `claude-code-best-practices-index` 接進主要 claude-code 條目的相關連結，讓 47 篇有明確入口。

---

## 優先處理清單

1. **修正 `claude-md-guide:188` 的官方歸屬**（🔴 問題 1）——這是唯一會讓讀者取得錯誤認知的矛盾，且該篇是 29 個入連結的高流量條目
2. **重跑 `knowledge-synthesizer agent-engineering-architecture`**（🔴 問題 2）——快照後新增 11 篇，是本庫成長最快的主題
3. **重跑 `knowledge-synthesizer claude-md`**（🔴 問題 2）——新來源 av 85 且修正了既有來源的判準，精粹的 A/V 層必然已偏
4. **把 skill 第 7 項判準改為「來源清單 vs 快照後新增」**——本次已證實現行 glob 計數法會產生假陰性
5. **修掉 2 個真斷鏈**（問題 4）：`claude-code-cost-optimization-7-tips` 的 bash code fence、`notebooklm-kit-academic-expansion-plan` 指向記憶檔的連結

---

## 🔄 5A+ 階段評估

**當前階段：ADJUST**

理由：raw/ 積壓為 **0**（排除 ACQUIRE）、無斷裂知識缺口且矛盾僅 1 處已定位（ATTEMPT 基本完成），但**同主題已有精粹而來源大幅增長**——13 篇精粹中 2 篇嚴重過時、5 篇中度過時，正是 ADJUST 的定義條件。

下一步：依優先清單先修矛盾（1）與斷鏈（5），再重跑兩篇嚴重過時的精粹（2、3）。三個真知識缺口（World Models／Long-Horizon Agent／RACG）屬下一輪 ACQUIRE 的選材方向，**但不要現在開始收集**——先讓精粹層追上，否則新素材會再拉開落差。

⚠️ **同時成立的次階段：ATTEMPT**（矛盾 1 處）。依 skill「優先處理最低階段」原則，問題 1 的修正應排在精粹重跑之前。

---

## 2026-09-03 執行紀錄（優先清單處置結果）

### ✅ 六個矛盾全部修復（含 08-12 漏報的 5 項）

| # | 矛盾 | 處置 |
|---|---|---|
| 1 | `claude-code-12-rules-error-reduction` 的「單 session 30,000 tokens 就重開」 | 加世代注記，改判準為「**依賴什麼而定**」（依賴 `paths:` rules 者仍該重開），與 [[claude-code-context-management]] 雙向連結 |
| 2 | Fable 5 說「派 verifier 子代理更好」vs Opus 5 說「不要用子代理複查」 | 🔑 **本次最有價值的一項**。拆開原文後發現**衝突不在模型家族，而在驗證的對象**：Opus 5 反對的是「複查剛剛那一次的產出」，Fable 5 建議的是「**fresh-context 對照原始規格**檢查長流程漂移」。判準寫成「**看你要驗的是『這一步對不對』還是『整段有沒有走偏』**」，兩篇雙向連結 |
| 3 | `agent-skill-design-patterns` 的「40→200 個 Skill 成本幾乎不變」 | 加前提注記：**Skill 列表整體預算 = context window 1%**，超限後冷門 Skill description 被砍、觸發率無聲下降 |
| 4 | `claude-md-guide` 把「超過 200 行遵守率下降」歸給官方 | 見下方獨立段（查證後發現問題比誤標來源更大）|
| 5 | `agent-skills-vs-mcp` 的「MCP 需持續連線且每次消耗 Context」 | **T 層直接改寫**（非僅加註）：兩個前提都已失效——Tool Search 延遲載入 ＋ MCP 新版協定無狀態。與 [[mcp-stateless-architecture]] 雙向連結 |
| 6 | `/compact` 時機建議相反 | 調和為「**時機歸 `claude-code-7-usage-mistakes`、工具選擇歸 `claude-code-session-handoff`**」，兩篇雙向連結 |

### 🔴 矛盾 4 上網查證後的實質發現（超出原本的「改一句話」）

| 發現 | 依據 |
|---|---|
| **200 行的硬截斷屬 `MEMORY.md`，不是 `CLAUDE.md`**（超出**靜默截斷**、未文件化）；「Claude 只讀 `CLAUDE.md` 前 200 行」是**社群誤解** | [GitHub issue #25006](https://github.com/anthropics/claude-code/issues/25006)（closed as not planned）|
| 官方對 `CLAUDE.md` 的原話是 *"loaded in full regardless of length"* ＋ *"target under 200 lines"*——**是 target 不是 limit** | 官方文件措辭 |
| 🔴 **檔案長度 25／100／250／500 行對遵守率無可測量差異**（χ²(3)=5.16, p=0.16；BF₁₀=0.096）；**規則位置、檔案架構、內部矛盾同樣零差異**。真正的落差是**有沒有這個檔案：0/524（0%）→ 67.7%** | 1,650 sessions／16,050 個 function 實驗，⚠️ 經 [Alex Dunlop 2026](https://www.alexdunlop.com/writing/claude-md-best-practices) 二手轉述，未找到一手出處 |
| 最弱環節是**編輯既有程式碼**（71.3% vs **45.1%**）；長 session 每多寫一個 function 遵守 odds **−5.6%** | 同上 |

> 🔑 **對本庫的影響**：`CLAUDE.md` 精簡技巧的效益應改記為**省 context 成本與維護成本**，而不是「提升遵守率」——**67.7% 是結構優化拉不上去的天花板**。已同步更新 [[claude-md-optimization-techniques]]（九項技巧的定位修正）、[[apple-claude-md-teardown]]、[[agents-md-guide]]、[[claude-code-12-rules-error-reduction]]。
> 擴散盤點：全庫提及「200 行」共 51 處，**真正做出「影響遵守率」因果宣稱的只有 3 處**（已全數修正）；其餘 33 處只把它當軟性目標、11 處正確指向 `MEMORY.md`。

### ✅ 兩篇精粹重跑

- **[[synthesis/synthesis-claude-md]]**（5→6 篇，av 85→88）：**E 層實質翻轉**——新增 [[apple-claude-md-teardown]] 的「不寫會怎樣」測試與推導成本判準，並依上述實證把方法論的效益定位從「提升遵守率」改為「省成本」。V 層新增三個缺口（結構優化已被否證但本庫技巧仍以此為前提／45.1% 最弱環節無對策／session 衰減無人處理）
- **[[synthesis/synthesis-agent-engineering-architecture]]**（8→16 篇，av 88→90）：**新增一整個維度**——原版把 Guardrails 當 20 模組之一，新來源顯示控制層是獨立平面。E 層新增三層推進（模型只參與第一步／裁決權要搬出提示詞／🔑 **Capability scaffolding vs Control plane 去留邏輯相反**）＋ 一項反面證據（加記憶反而變差）。並修正原 A 層殘留的**錯誤四層順序**（Prompt→Context→**Loop→Harness** → 應為 **Harness→Loop**）

### ✅ skill 升級 v1.7.0 → v1.8.0（三項方法缺陷）

1. 第 7 項精粹過時改用**來源清單集合差**，廢止 `source_count` vs glob 總數比大小（該法會**假陰性**：`synthesis-claude-md` 5→5 判成 OK，實際漏抓一篇來源又多抓一篇新條目）
2. 第 4 項連結掃描新增三道前置：**剝除程式碼區塊**、**解析路徑寫法與外部 vault**、**與前次報告對照**
3. 執行流程新增第 1 步「**先讀前一份報告**」，第 1 項矛盾偵測**必須逐項覆核前次矛盾是否已修**，未修者標「連續 N 次指認」

三處外部追蹤點已同步（Skill存檔快照／`SKILL_INDEX.md`／`skills-list.md`）。

### 尚未處理（維持在清單上）

- 🟡 孤立筆記 27 篇（其中 `concepts/ai-dev-workflow-taxonomy` 是導航地圖卻無人連入，值得優先補）
- 🟢 三個真知識缺口：**World Models for Agents／Long-Horizon Agent／RACG**——同指「本庫強在單次任務可靠性、弱在跨時程自主性」
- 🟢 五篇中度過時的精粹（`claude-code-internals`／`multi-agent`／`prompt-engineering-evolution`／`karpathy-llm-wiki-method`／`agentic-engineering`）
- 🟢 `claude-code-context-management`（49 入連結）承載兩個主題，可考慮把 Auto Memory 段拆出
