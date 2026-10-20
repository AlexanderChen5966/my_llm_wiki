---
title: Wiki 健康檢查報告 2026-08-12
date: 2026-08-12
tags: [project/knowledge-base, pkm/workflow]
source: wiki-health-check v1.6.1
type: reference
status: processed
---

# Wiki 健康檢查報告 — 2026-08-12

> 七項檢查全部完成。

## 整體評分

- wiki 條目數：**396**（含 `synthesis/` 13、`concepts/` 5）｜ideas 87｜research 43
- raw 素材數：792（`.md` 768 / 非 .md 24）
- raw 處理狀態：**未處理 0**（`.md` 0、非 .md 0）
- 問題總數：**11**（嚴重 6 / 警告 3 / 建議 2）
- 健康指數：**良好**（一致性偏高，弱點集中在「修正傳播不完全」）

---

## 🔴 嚴重問題（需立即處理）

### 六個未處理的矛盾

共通模式：知識庫**已經知道正確答案**，修正寫進了某一篇條目，但**沒回頭同步到最初主張錯誤說法的那篇**，
而那幾篇往往資產值更高、被引用更多、是讀者的實際入口。六個裡有四個（1、3、4、5）屬此模式，且都伴隨單向或零連結。

**1. Session 長度上限已被官方移除，但高流量入口篇未同步**
- `wiki/claude-code-12-rules-error-reduction.md:30` — 「單 session 30,000 tokens，超限就總結重開」
- `wiki/claude-code-context-management.md:48-64` — Anthropic 已主動移除此建議，改為「依賴什麼而定」
- 性質：世代差異未標註（最嚴重）。12-rules 為 2026-06-01 建立、asset_value 80、被 `claude-code-7-usage-mistakes` 反向引用，且**未連向 context-management**
- 建議：以 context-management 為準，在 12-rules 加世代注記並雙向連結

**2. 子代理複查：Fable 5 與 Opus 5 建議相反（唯一的真缺口）**
- `wiki/claude-fable-5-usage-guide.md:27` — fresh-context verifier subagent 比主模型自我批評更好
- `wiki/claude-opus-5-prompting-guide.md:14` — Opus 5 已會主動驗證，交子代理複查「不一定有幫助，反而增加工具呼叫與 token」
- 佐證：`claude-md-optimization-techniques.md:214`（子代理上限 3 個）、`claude-opus-5-prompting-guide.md:97`
- 性質：**真矛盾，非世代差異**——兩篇都是 2026 當代一手文件；兩篇零交叉連結
- 建議：依模型家族分流（Fable 5 長時序仍需 verifier 迴圈；Opus 5 減少複查層並設上限），必須明文寫出。此為六項中**唯一尚無任何條目給出答案者，建議優先處理**

**3. Skill 列表有 1% context 預算上限，推論篇未修正**
- `wiki/agent-skill-design-patterns.md:99` — 「Skill 從 40 增到 200，context 成本幾乎不變」（推論）
- `wiki/skill-md-writing-techniques.md:27,35` — 官方 `skillListingBudgetFraction` 預設為 context window 的 1%，超過後冷門 Skill 的 description 被砍、觸發率無聲下降（官方查證）
- 連結：後者單向連向前者（`:255`），前者未回指
- 建議：以 skill-md-writing-techniques 為準，在 design-patterns 加前提注記並補回指

**4. CLAUDE.md「200 行 / 80% 遵守率」的來源歸屬錯誤**
- `wiki/claude-md-guide.md:188` — 掛在「Anthropic 官方文件明確指出」名下
- `wiki/claude-md-optimization-techniques.md:25,30,34` — 2026-07-13 查證官方當時無任何行數上限（200 行於 2026-07-27 才追認）；78%→76%→52% 遵守率無法外部複現
- 建議：修正 claude-md-guide 的來源標註為「社群實測參考值」
- 附帶落差（各有情境但未說明為何偏離 200 行共識）：
  `karpathy-knowledge-base-method.md:84` 寫 100 行、`claude-agents-skill-file-comparison.md:66` 寫 ≤120 行、`seven-agent-software-factory.md:86` 寫 100–300 行

**5. 「MCP 比 Skills 耗 token」的前提已因 Deferred Tool Loading 失效**
- `wiki/agent-skills-vs-mcp.md:13,42` — 「MCP 需持續連線且每次消耗 Context」「即使沒用到也得載入數百行工具定義」
- `wiki/claude-code-cost-optimization-7-tips.md:64` + `claude-code-context-management.md:24` — MCP 預設 Tool Search 延遲載入，只有工具名進上下文
- ⚠️ **不能用日期判誰對**：agent-skills-vs-mcp 那句是 2026-07-31 才併入的補充，比對方更新
- 另：「持續連線」與 `mcp-stateless-architecture.md`（新版協定每請求自帶內容、Server 不保存 Session）對不上
- 建議：T 層改寫為「**在工具定義全量載入的前提下** Skills 較省 token；啟用 Tool Search 後差距大幅縮小」

**6. `/compact` 時機建議相反**
- `wiki/claude-code-7-usage-mistakes.md:27` — 只在工作分界點（提交後）compact
- `wiki/claude-code-session-handoff.md:37` — 同對話續跑「`/compact` 就好」
- 建議：調和為「時機歸前者、工具選擇歸後者」，並互連

### 已確認「不是矛盾」而排除者（處理得很好，記錄備查）

thinking `budget_tokens`→`adaptive`/`effort`、temperature 世代棄用、三層／五層／四機制分層法、
Superpowers 硬門檻之爭、`codex-advanced-tips-11.md:212` 頁內張力——
這些都已主動標註世代或並存理由，且 `synthesis/synthesis-claude-code-internals.md:96-99`
把「世代分界」本身抽象成可複用模式並寫進引用紀律。**矛盾偵測機制本身是有效運作的。**

---

## 🟡 警告（建議本週處理）

### 連結品質

- **懸空連結：12 個目標／13 次引用**。逐一檢視後，真正需要修的只有 1 個：
  - `ideas/reflection-pattern-basics-day9.md:13` 寫成 `[[reaction-memory-learning-loop]]`，
    正確檔名為 `reflection-memory-learning-loop`（**錯字**）
  - 其餘非問題：`[[sanlong/architecture]]`、`[[sanlong/bugs]]`、`[[sanlong/dev-workflow]]`、
    `[[b2b-manager/architecture]]` 指向外部專案 vault；
    `[["$cmd" =~ ^(npm\ test]]` 是程式碼區塊內的 bash regex 被誤抓；
    `[[wiki-health-check]]`、`[[wikilinks]]`、`[[Previous Analysis]]`、`[[Redis Paper]]`、
    `[[skills.config]]`、`[[feedback_framework_design_decisions]]` 為 skill 名／通用詞／外部參照

- **孤立條目（0 入連結）：26 篇**。多為工具型或單點主題條目：
  agent-readiness-cloudflare-audit、ai-agent-reverse-workflow-decomposition、
  ai-presentation-four-layers-human-value、cc-gui-intellij-claude-code-plugin、
  chatgpt-blender-mcp-3d-dashboard-case-study、chatgpt-deep-research-guide、
  claude-official-certifications、claude-tag-slack、free-ai-courses-6-platforms-2026、
  gemini-spark-personal-agent、graphify-codebase-knowledge-graph、jenkins-github-copilot-cli-pr-review、
  kotlin-toolchain-amper-merger、llm-fundamentals-tokenizer-to-reasoning、loom-ai-async-screen-recording、
  notion-ai-workspace-analysis、obsidian-visual-skills、odysseus-self-hosted-ai-workspace、
  okf-open-knowledge-format、open-source-web-scraping-tools-2026、openmontage-ai-video-agent-system、
  paper-radar-literature-monitor、system-prompts-leaks-repo、warp-agentic-dev-environment、
  yc-designer-claude-curation-workflow、concepts/ai-dev-workflow-taxonomy
  - 特別值得補連結的：`llm-fundamentals-tokenizer-to-reasoning`（整合 Day 01–04 的基礎條目，
    卻無人引用）、`concepts/ai-dev-workflow-taxonomy`（概念框架層條目孤立，不合理）

- **樞紐條目**（入連結最多，結構健康的指標）：
  claude-code-context-management 45×、harness-engineering 40×、loop-engineering-6-components 31×、
  karpathy-knowledge-base-method 30×、claude-skills-guide 29×

### 缺乏來源：9 篇

全部為 `synthesis/`（4）與 `concepts/`（5）的衍生條目，來源記在「來源條目」段落而非 frontmatter，**屬正常設計**，不需處理。

### 未處理素材：0

- `raw/` `.md`（雙條件）：**0**
- `raw/` 非 `.md`：**0**
- 非 `raw/` 的 `status: inbox`：**1**（`ideas/notebooklm-kit-academic-expansion-plan.md`）

### 幽靈標記偵測：0 ✅

`raw/` 共 386 個 `.processed` 標記。機械比對（source URL + 標題字串）篩出 84 個「查無對應」候選，
其中 7 個為本日 raw-pipeline 的刻意排除（已有紀錄），其餘 **77 個經 Agent 逐一實查**：

| 分類 | 筆數 |
|---|---|
| MERGED（已併入條目，可指出出處） | **58** |
| EXCLUDED（有明確排除紀錄） | **19** |
| **GHOST（真幽靈）** | **0** |
| UNCERTAIN | 0 |

**結論：沒有任何「標記了卻沒建條目」的情形。** 機械比對誤報率 100%。

唯一證據等級偏低的一筆：`CodexPets`（DeTools）——Batch 30 的排除紀錄只記數量未具名，
是用批次帳目消去法推導出來的（該日 32 個標記 = Batch 30 的 24 + Loop 去重的 8，數字吻合，
逐一對回後未列名者僅剩 3 篇，恰為 3 個排除名額）。**若只想人工複核一筆，建議看這筆。**

---

## 🟢 建議（下次處理）

### 精粹過時偵測

13 篇 synthesis **全部具備** `source_count` 與 `synthesis_snapshot_date` 戳記，內部數字一致，機制健全。
其中 **9 篇的快照後有同主題新條目**（glob 比對寧可過報，需人工確認）：

| 精粹條目 | 快照 | 快照後新增同主題 |
|---|---|---|
| `synthesis-agent-engineering-architecture` | 8 篇（07-24） | **16 篇** ← 最嚴重 |
| `synthesis-llm-cost-routing` | 6 篇（07-24） | 3 篇 |
| `synthesis-prompt-engineering-evolution` | 10 篇（08-04） | 3 篇 |
| `synthesis-ai-dev-workflow-frameworks` | 6 篇（07-24） | 2 篇 |
| `synthesis-claude-code-internals` | 6 篇（08-04） | 2 篇 |
| `synthesis-karpathy-llm-wiki-method` | 7 篇（07-24） | 1 篇 |
| `synthesis-multi-agent` | 7 篇（07-24） | 1 篇 |
| `synthesis-practitioner-skills-design` | 6 篇（08-04） | 1 篇 |
| `synthesis-spec-driven-workflow` | 7 篇（08-04） | 1 篇 |

### 內容過時

知識庫僅 3 個月（05 月 78 篇、06 月 150、07 月 111、08 月 40），**無 6 個月以上未更新者**。
但有一批 5–6 月條目提及具體版本（Opus 4.8、Flutter 3.44、Android 17、Claude Code 2.x），
依 `synthesis-claude-code-internals` 已確立的紀律，引用參數行為時應標明世代。

---

## 優先處理清單

1. **矛盾 2（Fable 5 vs Opus 5 子代理複查）**——唯一尚無任何條目給出答案的真缺口
2. **矛盾 1（session 長度上限）**——錯誤資訊位於高資產值、高引用的讀者入口
3. **矛盾 5（MCP 延遲載入）**——影響 `agent-skills-vs-mcp` 的整個 T 層論點，該篇入連結 27×
4. **重跑 `synthesis-agent-engineering-architecture`**——快照後新增 16 篇，落差最大
5. **修正錯字** `ideas/reflection-pattern-basics-day9.md:13`

---

## 🔄 5A+ 階段評估

**當前階段：ADJUST**

理由：raw/ 未處理為 0（非 ACQUIRE）；無嚴重斷鏈、結構健康（非 ATTEMPT 的品質崩壞情境）；
但 9 篇精粹的來源已增長、且存在六個「修正未傳播」的矛盾——這正是 ADJUST 的典型徵狀：
知識已累積足夠，需要的是回頭整合與校準，而非繼續攝入。

**下一步**：
1. 先修矛盾（優先清單 1–3），這是校準
2. 再跑 `knowledge-synthesizer` 重刷 `synthesis-agent-engineering-architecture`
3. 建立一條新紀律：**凡新增推翻既有說法的內容，必須在被推翻那篇加注記與雙向連結**——
   本次六個矛盾有四個源於缺少這個步驟

---

## 方法論備註（供下次健檢改進）

本次幽靈標記偵測的機械比對（source URL + 標題字串）**誤報率 100%**（77/77 皆有實際去向）。
失效模式有五種固定樣態：

1. 系列彙整條目只記系列層級 `source:`，個別文章網址不會出現
2. 條目 `source:` 常改記**原始出處**而非實際抓取的轉載／翻譯站
   （codelove.tw 譯文 → 回記 Qiita/dev.to；techorange 轉載 → 回記原始 URL）
3. 併入既有條目「補充資料」段落時不改 frontmatter，正文只寫「來源：某媒體」
4. **同主題多來源合併**（中文報導 + GitHub README + 譯文各一份），條目只留主幹來源
5. URL 編碼差異（raw 存 percent-encoded、wiki 存解碼後中文）

**改進建議（依成本效益排序）**：

1. **在 `.processed` 旁車檔內記一行去向**（目前為空檔）——併入哪篇／排除理由。
   這是最根本的改法，能讓幽靈檢查直接讀取而非反推，下次健檢可完全免除 Agent 實查
2. **raw-pipeline 的排除清單逐篇具名**——Batch 39 已是此寫法，但 Batch 30/31 只記數量，
   導致本次有 6 筆須靠消去法推導
3. 合併型條目採用 `source_2` / `source_3` 欄位（`wiki/hermes-agent.md` 已是此格式）
4. 機械篩選加「標題核心詞 + 系列名/Day 數」與「`INDEX.md` 全文」兩道過濾

## 相關

- [[inbox-detection-protocol]]
- [[decision-framework]]
