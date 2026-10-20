---
title: SKILL.md 撰寫技巧與最佳化指南
date: 2026-06-05
tags: [ai/claude, ai/agent]
source: 整合自 claude-skills-deep-dive / agent-skill-design-patterns / synthesis-practitioner-skills-design / claude-agents-skill-file-comparison
type: reference
status: processed
asset_value: 85
---

SKILL.md 與 CLAUDE.md 的根本差異：**按需載入**。描述符合才進 context，不符合就完全不佔空間。這個機制讓 Skill 可以比 CLAUDE.md 詳細 5-10 倍，但也讓 `description` 欄位的品質比任何內容都更重要。

---

## 硬性規格（有官方數據，2026-07-13 對照官方文件複核全部有效）

| 指標 | 建議值 | 依據 |
|------|--------|------|
| SKILL.md 最大行數 | **≤ 500 行** | Anthropic 官方；超過則拆 references/ |
| description + when_to_use 合計 | **≤ 1,536 字元** | 官方截斷上限（可用 `skillListingMaxDescChars` 設定調整）|
| 參考文件引用深度 | **一層**（SKILL.md → ref，不能 ref → ref）| Anthropic 官方 |
| 超過 100 行的 ref 文件 | **必須有 TOC** | 確保 Claude 部分讀取時能看到全貌 |
| Compaction 後保留 token | **最多 5,000 tokens / Skill**（所有活躍 Skill 共 25,000）| 官方 |

### 補充：Skill 列表的整體字元預算機制（官方文件，2026-07 新確認）

1,536 字元是**單一 Skill** 的上限；所有 Skill 的 name+description 列表還受一個**整體預算**約束——預設為**模型 context window 的 1%**。裝了很多 Skill 導致列表溢出時，Claude Code 會**從你最少呼叫的 Skill 開始丟棄 description**（只剩名稱），這可能剛好砍掉觸發需要的關鍵字。三個應對手段：

| 手段 | 做法 |
|------|------|
| 提高整體預算 | `skillListingBudgetFraction` 設定（如 `0.02` = 2%）或環境變數 `SLASH_COMMAND_TOOL_CHAR_BUDGET`（固定字元數）|
| 讓出預算給重要 Skill | 低優先 Skill 在 `skillOverrides` 設 `"name-only"`，只列名稱不佔 description 空間 |
| 從源頭精簡 | **key use case 放 description 最前面**——截斷是從尾部截，開頭的觸發關鍵字最安全 |

實務含義：「裝 50 個 Skill 也不會擠爆 context」的說法要加上前提——超過 1% 預算後，冷門 Skill 的觸發率會因 description 被砍而無聲下降，這是**大量裝 Skill 的隱性成本**（與 [[claude-code-skill-survivors-20-picks]] 的「不要貪多」建議互相佐證）。

---

## 完整 Frontmatter 欄位

```yaml
---
name: skill-name                    # 小寫 + 連字號，最長 64 字；省略則用目錄名
description: |                      # 最重要欄位，決定何時觸發
  第三人稱說明。Use when ...
when_to_use: |                      # 補充觸發時機，與 description 合計 ≤ 1,536 字元
  也可以用 ...
argument-hint: "[issue-number]"     # autocomplete 提示
arguments:                          # 具名位置參數，對應 $name 佔位符
  - name: target
    description: 目標路徑
disable-model-invocation: false     # true = 只能手動觸發（有副作用的流程用這個）
user-invocable: true                # false = 隱藏於 / 選單（背景知識型 Skill 用這個）
allowed-tools: Read, Grep, Bash     # 預批准工具（不限制可用工具，只是免確認）
model: claude-sonnet-4-6            # 強制使用特定模型
effort: high                        # low / medium / high / xhigh / max
context: fork                       # 在獨立 subagent 跑，不污染主 context（最強隔離）
agent: Explore                      # context: fork 時指定 subagent 類型
paths: "src/**/*.ts"                # Glob 路徑限制，只在操作匹配路徑時自動啟用
---
```

**最重要的三個欄位**：`description`（觸發條件）、`context: fork`（context 隔離）、`disable-model-invocation`（副作用防護）

---

## description 撰寫技巧（最高優先）

description 決定 Skill 是否被自動觸發，是最重要的欄位。

### 格式模板

```
第三人稱說明這個 Skill 做什麼。
Use when [具體情境1], [具體情境2], or when the user mentions [關鍵詞].
Do NOT use when [排除情境].
```

**具體差異：**

```yaml
❌ 壞例：description: Helps with PDFs.
✅ 好例：description: |
  Extracts text, tables, and structured data from PDF files.
  Use when working with PDF documents, forms, or when the user mentions
  PDFs, document extraction, or needs to parse file attachments.
  Do NOT use for image-only PDFs without embedded text.
```

**注意事項：**
- **第三人稱**（「處理 Excel 文件」，非「我可以幫您…」）
- description 不要說「做什麼」，要說「什麼時候用」——否則 AI 讀描述就跳過正文
- Claude 傾向**少觸發**而非多觸發，description 可以稍微「強勢」一點
- 多個 Skill description 重疊時：在 description 加「不適用場景」區分

---

## 三大核心原則（Anthropic 官方）

### 1. 簡潔原則

Claude 已經很聰明，只加「Claude 尚未擁有的上下文」。每行都問：「Claude 真的需要這個嗎？」

實測：50 tokens vs 150 tokens 的 instruction，輸出品質相近，但成本差 3 倍。

### 2. 自由度匹配

依任務的脆弱性選擇指令格式：

| 自由度 | 格式 | 適用場景 |
|--------|------|---------|
| **低**（一致性關鍵） | 精確腳本、無參數 | 資料庫 migration、deploy checklist |
| **中**（有首選但允許變化） | 帶參數偽程式碼 | 報告生成、PR review |
| **高**（多種方法都有效） | 文字描述 | 程式碼審查、研究綜合 |

類比：窄橋上只有一條安全路線（低自由度）vs 開闊田野（高自由度）。

### 3. 跨模型測試

Opus 完美的 Skill 可能需要為 Haiku 提供更多細節。**同一個 Skill 必須在 Haiku / Sonnet / Opus 都測試**。

---

## 目錄結構最佳實踐

```
.claude/skills/
  my-skill/
    SKILL.md            ← 主體，≤ 500 行
    references/
      conventions.md    ← 詳細規則（需要目錄 TOC）
      examples.md
    scripts/
      validate.py       ← 執行不進 context，只有輸出進 context
```

**動態 context 注入（`!` 語法）：**

```yaml
---
name: pr-summary
context: fork
allowed-tools: Bash(gh *)
---
PR diff: !`gh pr diff`
PR comments: !`gh pr view --comments`
```

`` !`command` `` 在 Skill 送給 Claude 前執行，輸出替換佔位符——Claude 收到的是真實數據，不是命令。

---

## 五大設計模式（Google Cloud）

| 模式 | 適用場景 | 說明 |
|------|---------|------|
| **1. 單一工具型** | 只呼叫一個外部 API / 框架 | 把 FastAPI / Drizzle 等框架的慣例封裝成 Skill |
| **2. 流程串接型** | 多步驟依序執行（A→B→C）| migration、deploy 等有明確順序的流程 |
| **3. 路由型** | 根據輸入條件選不同子流程 | 判斷是 bug fix 還是新功能，走不同 checklist |
| **4. 並行型** | 多個子任務同時執行 | review 場景（安全 + 效能 + 可讀性並行）|
| **5. 反思型** | 執行後自我評估，不符合則重試 | 品質要求高的場景（耗 token，謹慎使用）|

**軟編排型 Skill** 通常結合**流程串接 + 並行**兩種模式。

---

## Eval 驅動開發（Anthropic 官方方法）

**先建立評估再寫 Skill**，確保解決真實問題：

1. 不用 Skill 完成任務，記錄具體失敗點
2. 建立 3 個測試場景作為 eval
3. 測量 Claude 的基準表現
4. 撰寫**最少指令**（剛好通過 eval）
5. 迭代：跑 eval → 比較基準 → 改進

**Claude A + Claude B 迭代法：**
- **Claude A（同對話）**：幫你設計、改進 Skill
- **Claude B（新對話）**：用真實任務測試 Skill
- 觀察 B 的行為 → 帶具體問題返回 A → 迭代

### 2026-07-13 補充：上述手動流程已被工具化

skill-creator（官方，`/skill-creator`）現在把整套 Eval 驅動開發自動化（完整實測見 [[agent-skill-evaluation-tools-comparison]]）：

- **有/無 Skill 自動對比**：`evals.json` 定義 prompt/expected_output/assertions，同一任務在有/無 Skill 兩種情況各跑多次，結果以 `mean ± stddev` 呈現——**stddev 大 = flaky，代表 Skill 效果不明確或依賴模型**
- **description 觸發率量測**（本條目「description 撰寫技巧」的量化驗證工具）：約 20 個真實 prompt 混合 should-trigger 與不該觸發案例，切 train/test，失敗案例交給 Claude 重寫 description 再評估——「該觸發卻沒觸發」和「不相關卻誤觸發」都能用數字壓下去
- 進一步的寫法品質評分可用 darwin-skill（9 維度爬山式改善），大規模最佳化用 SkillOpt

**兩個實測結論直接支持本條目的既有原則**：
1. **模糊的 Skill 會輸給 baseline**——充滿「依情況」「盡量」「彈性處理」的弱 Skill 實測 pass 率 92%，反而低於完全不用 Skill 的 100%。這是反模式清單「用 LLM 生成整份 Skill（冗長泛用比沒有更差）」的直接量化證據
2. **強模型下 Skill 的價值在「強制一致性」而非提升上限**——底層模型夠強時 pass 率差距只有 ±8pt，Skill 真正買到的是「scope 判定、格式規範、寫出原因」這些不能靠運氣、每次都要穩定做到的一致性

---

## 反模式清單

| 反模式 | 正確做法 |
|--------|---------|
| description 說「做什麼」 | description 說「什麼時候用」——否則 AI 讀描述就跳過正文 |
| 把所有流程塞進 SKILL.md | 超過 500 行的部分移到 references/，SKILL.md 寫「何時載入哪個 ref」|
| 提供太多選項 | 提供一個預設值 + 特殊情況說明 |
| 時效性資訊 | 用「舊模式」折疊區塊，不寫具體年份 |
| 巫毒常數（`TIMEOUT = 47`）| 每個值都說明理由 |
| 深度嵌套參考（A→B→C）| 只從 SKILL.md 一層引用 |
| Windows 路徑（`scripts\helper.py`）| 一律使用正斜線 |
| 用 LLM 生成整份 Skill | 手寫（LLM 生成的冗長、泛用，效果比沒有更差）|
| 模糊軟化詞（「依情況」「盡量」「彈性處理」）| 明確標準 + 格式範例——實測模糊 Skill pass 率反而低於不用 Skill（92% vs 100%）|
| 沒有失敗分支 | 寫出「如果 X 失敗就改成 Y」的 if-then 分支表（darwin-skill 評分維度之一）|
| 沒有反例/黑名單 | 明確列出「不可做的事」（如 `chore: update stuff` 這種零資訊摘要）|

---

## 何時把重複動作封裝為 Skill

**黃金規則**：同一個動作出現 **≥ 3 次** → 考慮封裝。

優先封裝的候選：
- **最容易出錯的工程判斷**（Addy Osmani 原則）
- **多步驟依序流程**（AI 最容易跳過某步驟）
- **包含隱性知識**的流程（團隊默契、專案特殊規則）
- **需要特定模型或工具組合**的任務

**不需要封裝的情況：**
- 一次性任務（直接對話即可）
- 簡單單步操作（custom command 就夠）
- 規則尚在探索期、不穩定

---

## 完整優先順序

```
最先做：
1. 寫好 description（第三人稱、說何時用、含排除情境）
2. 確認 ≤ 500 行，超過的移到 references/

其次：
3. 設定 context: fork（需要隔離的 Skill）
4. 設定 disable-model-invocation: true（有副作用的 Skill）
5. 對應 Google 5 大模式選架構

最後：
6. 用 /skill-creator 跑「有/無 Skill」對比 + 觸發率量測（取代純手動 A+B 迭代），
   Haiku/Sonnet/Opus 三模型驗證
```

---

## 相關連結

- [[claude-md-optimization-techniques]] — CLAUDE.md 對應的精簡化技巧（與本條目互為配對）
- [[claude-agents-skill-file-comparison]] — SKILL.md vs CLAUDE.md vs AGENTS.md 完整比較
- [[claude-skills-deep-dive]] — Progressive Disclosure 三層機制 + 官方 Frontmatter 完整欄位
- [[agent-skill-design-patterns]] — Google 5 大設計模式完整說明
- [[synthesis-practitioner-skills-design]] — Addy Osmani / agent-skills / Matt Pocock 三套設計哲學精粹
- [[claude-skills-guide]] — 能力包型 vs 軟編排型兩種 Skill 類型
- [[agent-skill-evaluation-tools-comparison]] — skill-creator/darwin-skill/SkillOpt 三工具實測（Eval 驅動開發的工具化落地）
- [[claude-code-skill-survivors-20-picks]] — 「純 prompt 包裝」型 Skill 的淘汰訊號（與「模糊 Skill 輸給 baseline」實測互相佐證）
- [[custom-gpt-gems-to-skills-migration]] — 跨平台佐證（2026-10-08）：Gemini 官方撰寫指南同樣先看名稱與描述、再讀全文、必要時才開附檔（與本條按需載入原則一致）；⚠️ Gemini 的 Skill 名稱會自動轉成小寫加連字號，Skill 內腳本不能對外部網站發請求
