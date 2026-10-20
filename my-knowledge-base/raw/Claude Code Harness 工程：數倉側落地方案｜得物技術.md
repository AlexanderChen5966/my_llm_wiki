---
title: "Claude Code Harness 工程：數倉側落地方案｜得物技術"
source: "https://codelove.tw/@tony/post/3bbOA3"
author:
published:
created: 2026-05-27
description: "Claude Code Harness 工程：數倉側落地方案｜得物技術  ======================================    一、AI Coding 現狀與痛點：為什麼需要 Harness-------------------------------###..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月25日](https://codelove.tw/@tony/post/3bbOA3 "2026-05-25 16:04:44")


## Claude Code Harness 工程：數倉側落地方案｜得物技術

```
一、AI Coding 現狀與痛點：為什麼需要 Harness
```

---

### 當前使用情況

得物離線數倉各小組已基本完成 AI Coding 工具的覆蓋，主力工具為 Claude Code，輔以資料平台的 IDE 外掛，應對重複性工作時效率提升明顯。

### 核心痛點

儘管整體提效已顯現，但團隊在實際使用中暴露出三類結構性痛點。

**痛點一** ：AI 不記得上下文約束，開發過程中反覆「失憶」。會話開始時告知了「金額欄位單位是千元」，對話進行到一半後 AI 忘了，生成的 SQL 把千元當元用，導致資料差了 1000 倍。這不是偶發問題，而是 Claude Code 的 context compact 機制（上下文壓縮）的系統性限制：當對話 token 接近上限（約 95%）時，歷史內容被自動壓縮為摘要，臨時口頭說的約束全部丟失。

**痛點二** ：規範執行不穩定，靠記憶兜底的部分最容易出問題。OneData 命名規範、註解三段式、INSERT 必須帶 PARTITION 子句……這些規範大家都知道。在專案工期緊張時，人工規範遵守率降至 60%～70%，AI 靠 prompt 記憶的規範遵守率也只有 70%～80%。真正需要的是：把規範從「LLM 記憶中的指導性內容」變成「每次執行時強制檢查的護欄」。

**痛點三** ：大型需求開發中，context 很快被撐滿，越到後期 AI 越不可靠。複雜需求（大型寬表、大量下游血緣）的典型開發過程：

```
objectivec 體驗AI代碼助手 代碼解讀複製代碼血緣查詢結果（500~3000 tokens） 自測 23 條 SQL 執行結果（5000~15000 tokens） SKILL 規範文件內容（~10000 tokens） 資料比對兩表樣本（大量行）= context 迅速膨脹 → compact 觸發 → 關鍵約束遺忘 → AI 開始犯低級錯誤
```

核心矛盾：越是複雜的需求，越依賴 AI；但越複雜的需求，context 越容易撐滿，AI 越容易「失憶」。

### 從「隨手問 AI」到「把能力封裝起來」

針對上述痛點，我們沿用了一條反覆驗證的結論：規範執行是人的短板、AI 的長板；業務判斷是 AI 的短板、人的長板。Harness 工程的目標，就是把「執行層」的不穩定因素系統性地消掉：把規範寫進 hooks，不再靠 AI 記憶，每次寫 SQL 檔案後自動觸發檢查；把迭代約束寫進持久化檔案，compact 後自動重新注入，不再靠臨時口頭說；把高 token 操作隔離到 subagent，主 context 只接收摘要，不被過程資料撐滿。

## 二、先搞清楚「Harness」是什麼

在 Claude Code 的 update-config skill 描述中有一句話：「Automated behaviors require hooks configured in settings.json --- the harness executes these, not Claude」。

Harness = Claude Code 的宿主執行框架，即 Claude Code 客戶端本身這個「工具鏈容器」。它：管理 context window 生命週期；在 LLM 推理迴圈之外確定性地執行 hooks；協調 subagents 的生命週期；不依賴模型判斷，直接執行配置的自動化行為。區別總結：

[![](https://i.imgur.com/TSUK6Fa.jpeg)](https://i.imgur.com/TSUK6Fa.jpeg)

## 三、核心問題：compact 到底丟掉什麼？

每次數倉開發 context 接近滿時，auto-compact 觸發（預設 95% 時觸發），會把整個對話歷史替換為一份摘要，token 縮減到原來的約 12%。哪些內容 compact 後遺失：

[![](https://i.imgur.com/vGRIQ3F.jpeg)](https://i.imgur.com/vGRIQ3F.jpeg)

數倉場景最痛的點：對話中說的「這次用 OVERWRITE 模式」、「先忽略 field\_a 欄位」→ compact 後全忘；SKILL 檔案讀了一半，compact 後前幾個 step 的內容沒了 → Claude 重複詢問；自測跑出來 50 行結果，加上血緣查詢的幾十行表結構 → context 很快膨脹到 compact 閾值。

## 四、五層防禦體系（從簡單到複雜）

### 第一層：寫死進 CLAUDE.md（立即可用）

**機制** ：專案根目錄.claude/ [CLAUDE.md](http://claude.md/) 每次 compact 後從磁碟重新注入，是最可靠的持久化位置。將當前迭代的關鍵資訊寫入，格式建議：

```
sql 體驗AI代碼助手 代碼解讀複製代碼# 當前迭代狀態（每次迭代手動更新）## 正在開發- 表：db_a.dws_table_a
 版本：V1.0
 node_id：1000000001
 狀態：ETL開發階段（Step 3/8）

## 本次迭代約束- 禁止修改：dwd_table_b（已上線，只讀）
 分區欄位：partition_dt（格式 yyyyMMdd，不是 dt）
 amount 欄位單位：千元（不是元）

## 數倉全域規範
 建表：分區欄位必須是 partition_dt string
 禁止：SELECT *，UPDATE/DELETE 無 WHERE
 金額欄位用 DECIMAL(20,4)，不用 DOUBLE
 INSERT 必須帶 PARTITION 子句
```

**操作規則** ：進入新迭代時，更新「正在開發」和「本次迭代約束」兩節；上線後清空「本次迭代約束」；全域規範長期保留，控制在 100 行以內。

### 第二層：Auto Memory 自動積累（已在運行）

機制：Claude 自動將跨會話發現寫入 ~/.claude/projects [//memory/MEMORY.md，每次](https://memory/MEMORY.md%EF%BC%8C%E6%AF%8F%E6%AC%A1) compact 後重新注入。數倉場景下主動觸發 Claude 寫記憶的時機：「這張表的 amount 欄位單位是千元，請記住」；「field\_a 在特定場景下會為空，請記住這個踩坑」；「本次 V1.0 的關鍵變更是 field\_b 邏輯調整，請記住」。Claude 會自動寫入 [MEMORY.md](http://memory.md/) ，下次會話或 compact 後自動恢復。

### 第三層：hooks 自動驗證（核心防禦，解決「忘了檢查」的問題）

這是解決「每次寫完 SQL 自動檢查」的關鍵機制。

#### 配置檔位置

```
markdown 體驗AI代碼助手 代碼解讀複製代碼數倉專案根目錄/
└── .claude/
    ├── settings.json          ← hooks 在這裡配置
    ├── CLAUDE.md              ← 數倉規範上下文
    └── hooks/
        ├── validate_sql.sh          ← SQL 規範自動檢查
        ├── block_dangerous_ddl.sh   ← 危險 DDL 攔截
        └── inject_context.sh        ← compact 後重注入上下文
```

#### settings.json 完整配置

```
swift 體驗AI代碼助手 代碼解讀複製代碼{"hooks": {"PostToolUse": [{"matcher": "Write|Edit","hooks": [{"type": "command","command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/validate_sql.sh","timeout": 60,"statusMessage": "檢查 SQL 規範..."}]}],"PreToolUse": [{"matcher": "Bash","hooks": [{"type": "command","command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block_dangerous_ddl.sh"}]}],"SessionStart": [{"matcher": "compact","hooks": [{"type": "command","command": "cat \"$CLAUDE_PROJECT_DIR\"/.claude/context/dw_conventions.md","statusMessage": "重注入數倉規範..."}]}],"Stop": [{"hooks": [{"type": "prompt","prompt": "檢查使用者要求的所有任務是否都已完成。如果還有未完成項，返回提示但不要重新開始。檢查 stop_hook_active 是否為 true，如是則直接 exit。","model": "claude-haiku-4-5-20251001"}]}]}}
```

#### SQL 規範自動檢查腳本

.claude/hooks/validate\_sql.sh：

```
bash 體驗AI代碼助手 代碼解讀複製代碼#!/bin/bash
INPUT=$(cat)
FILE_PATH=$(echo"$INPUT" | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('tool_input',{}).get('file_path',''))" 2>/dev/null)

# 只處理 .sql 檔
[[ "$FILE_PATH" != *.sql ]] && exit 0
[[ -z "$FILE_PATH" ]] && exit 0

SQL=$(cat "$FILE_PATH" 2>/dev/null)
[[ -z "$SQL" ]] && exit 0

ERRORS=()

# 規範1：禁止 SELECT *
echo "$SQL" | grep -iqE 'SELECT\s+\*' && ERRORS+=("CRITICAL: 發現 SELECT *，必須明確欄名")

# 規範2：INSERT 必須帶 PARTITION
if echo "$SQL" | grep -iqE 'INSERT\s+(INTO|OVERWRITE)'; then
  echo "$SQL" | grep -iqE 'PARTITION\s*\(' || ERRORS+=("CRITICAL: INSERT 缺少 PARTITION 子句")
fi

# 規範3：DOUBLE 類型金額
echo "$SQL" | grep -iqE '\bDOUBLE\b' && ERRORS+=("WARNING: 金額欄位建議用 DECIMAL(20,4)，不用 DOUBLE")

# 規範4：UPDATE/DELETE 必須有 WHERE
if echo "$SQL" | grep -iqE '\b(UPDATE|DELETE)\b'; then
  echo "$SQL" | grep -iqE '\bWHERE\b' || ERRORS+=("CRITICAL: UPDATE/DELETE 缺少 WHERE 條件")
fi

if [ ${#ERRORS[@]} -gt 0 ]; then
  echo "=== SQL 規範檢查失敗：$FILE_PATH ===" >&2
  for err in "${ERRORS[@]}"; do
    echo "  $err" >&2
  done
  exit 2
fi

echo "SQL 規範檢查通過: $(basename $FILE_PATH)" >&2
exit 0
```

### 危險 DDL 攔截腳本

.claude/hooks/block\_dangerous\_ddl.sh：

```
bash 體驗AI代碼助手 代碼解讀複製代碼#!/bin/bash
INPUT=$(cat)
CMD=$(echo"$INPUT" | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('tool_input',{}).get('command',''))" 2>/dev/null)

# 攔截生產表 DROP/TRUNCATE（放行 _dev/_test/_stg 後綴）
if echo "$CMD" | grep -iqE '\b(DROP\s+TABLE|TRUNCATE\s+TABLE)\b'; then
  if ! echo "$CMD" | grep -qiE '(_dev|_test|_stg)\b'; then
    echo "BLOCKED: 檢測到生產表 DROP/TRUNCATE 操作，請確認表名是否正確" >&2
    exit 2
  fi
fi

exit 0
```

#### hook 通訊協議關鍵規則

[![](https://i.imgur.com/llC1vsH.jpeg)](https://i.imgur.com/llC1vsH.jpeg)

關鍵陷阱：阻斷必須用 exit 2，用 exit 1 不會阻止 Claude 繼續執行。

### 第四層：subagents 做上下文隔離（防 context 膨脹）

核心原則：把「高 token 消耗但結果只需要摘要」的操作放到 subagent 的獨立 context 中執行。

數倉場景適合下放 subagent 的操作

[![](https://i.imgur.com/AnN47eE.jpeg)](https://i.imgur.com/AnN47eE.jpeg)

#### 建立 subagent 檔案

.claude/agents/ [sql-validator.md](http://sql-validator.md/) （SQL 語法驗證）：

```
yaml 體驗AI代碼助手 代碼解讀複製代碼---
name: sql-validator
description: ODPS/MaxCompute SQL 語法驗證與規範檢查專用 agent。當使用者生成或修改 SQL 檔案後需要驗證時呼叫。在獨立 context 執行，防止大量驗證日誌污染主對話。
tools: Read, Bash, Grep, Glob
model: haiku
permissionMode: dontAsk
---

你是數倉 SQL 規範專家，只做驗證，不修改檔案。

驗證項（按優先級）：
1. SELECT * 禁止
2. INSERT 必須帶 PARTITION
3. 欄位用 snake_case 命名
4. 金額欄位用 DECIMAL 不用 DOUBLE
5. 多表 JOIN 必須有 ON 條件
6. 笛卡兒積風險檢測

輸出格式：
- 狀態：PASS / FAIL
- 問題列表（CRITICAL / WARNING / INFO）
- 修改建議（具體到行號）

不超過 50 行，只返回結構化報告。
```

.claude/agents/ [dw-explorer.md](http://dw-explorer.md/) （防止血緣查詢撐爆主 context）：

```
yaml 體驗AI代碼助手 代碼解讀複製代碼---
name: dw-explorer
description: 數倉結構探索 agent。當需要大量讀取表結構、DDL、欄位資訊、血緣關係時自動呼叫，避免大量檔案內容污染主 context。只讀，不修改任何檔案。
tools: Read, Glob, Grep, Bash
model: haiku
permissionMode: dontAsk
---

你是數倉探索專家，只讀操作。

當被呼叫時：
1. 讀取指定表的 DDL、欄位資訊、分區策略
2. 分析上下游血緣（一層）
3. 識別關鍵欄位口徑（金額、日期、狀態類欄位）

輸出：不超過 80 行的結構化摘要，包含：
- 表基本資訊（層級、粒度、分區策略）
- 核心欄位定義（含口徑說明）
- 上下游血緣（只列表名，不展開內容）
- 發現的特殊口徑或踩坑點
```

#### 使用方式

在主對話中自然觸發：

```
sql 體驗AI代碼助手 代碼解讀複製代碼用 dw-explorer 分析 db_a.dwd_table_a 的結構

對剛生成的 insert_dws_table_a.sql 用 sql-validator 驗證
```

或強制指定（避免 Claude 自行判斷）：

```
lua 體驗AI代碼助手 代碼解讀複製代碼@"sql-validator (agent)" 驗證 path/to/insert.sql
```

### 第五層：SKILL 檔案改造（減少 context 消耗）

當前的問題：每次調用 SKILL 檔案（01~08.md），內容全部載入進主 context，加速 compact 觸發。 **改造方向** ：把 SKILL 檔案的「執行步驟」提煉成 subagent 指令，subagent 內部讀完整 SKILL 檔案，主 context 只接收結果摘要；用 path-scoped rules 取代 SKILL 檔案中的規範章節，按需載入：

```
yaml 體驗AI代碼助手 代碼解讀複製代碼---
# .claude/rules/etl-rules.md
paths:
  - "**/*insert*.sql"
  - "**/*_di.sql"
  - "**/*_df.sql"
---

# ETL 開發規範（按檔案路徑自動載入）
- 必須有 partition_dt 分區
- INSERT OVERWRITE 前檢查分區是否已存在
- 不允許跨庫 JOIN
```

SKILL 檔案保持現狀，但改變調用方式：不要在主對話中直接觸發，而是啟動一個 subagent 來讀 SKILL 檔案執行 ETL，主對話只接收最終產出的 SQL 檔案路徑。

## 五、可行落地方案：數倉 Harness 架構

先讓我們看一下整體的架構設計（整體架構圖）：

[![](https://i.imgur.com/qk58SAy.jpeg)](https://i.imgur.com/qk58SAy.jpeg)

這張架構圖的核心邏輯是 **職責分層** ：不同類型的工作交給最合適的機制去做，而不是全部壓在 Claude 的推理迴圈裡。

**持久化層** 解決的是「失憶」問題，任何臨時口頭說的約束，compact 後都會消失；但寫進.claude/ [CLAUDE.md](http://claude.md/) 的內容，每次會話啟動和 compact 後都會從磁碟重新注入------這是整套方案裡最簡單也最可靠的一層。

**Harness 層（Hooks）** 解決的是「規範靠記憶」的問題，PostToolUse hook 在每次寫.sql 檔案後確定性觸發，不依賴 Claude 有沒有記住規範要求；違規時 exit 2 強制阻斷，Claude 必須修正後才能繼續，規範遵守率從 70%～80% 提升到 95%+。

**Subagent 層** 解決的是「context 被撐滿」的問題，血緣查詢、23 項自測、資料比對這類操作會產生大量 token，放到獨立 context 的 subagent 裡執行，主會話只接收一份摘要，compact 觸發頻率預計降低 50%～70%。

三層機制分工明確，互不干擾：Hooks 處理「寫動作觸發的規範檢查」，subagent 處理「讀操作的 context 隔離」， [CLAUDE.md](http://claude.md/) 處理「跨會話狀態持久化」。為了配合這套架構落地，數倉的研發流程可以拆解為 8 個大的步驟，流程圖如下：

[![](https://i.imgur.com/v8U4qwk.jpeg)](https://i.imgur.com/v8U4qwk.jpeg)

從 Harness 架構的視角來看，8 個步驟可以按「對 context 的影響」分成兩類：一類是 **直接在主會話處理** （內容量有限，context 壓力低）：需求分析（讀 PRD）、技術設計（寫規範說明）、SR 導入（產生配置）、SLA/DQC（產生規則）。另一類 **必須透過 Harness 機制處理** （否則會加速 compact 或規範失控）：ETL 開發每次寫.sql 檔案，PostToolUse hook 自動觸發規範檢查，不依賴人工提醒；自測時 23 條 SQL 的執行結果體量大，交給 data-quality-checker subagent 隔離，主會話只收 PASS/FAIL 摘要；資料比對時兩表樣本資料量大，交給 data-comparator subagent 隔離；效能優化時血緣 + 多層 DDL 每次 500～3000 tokens，交給 dw-explorer subagent 隔離。這種分工不是把步驟拆開獨立執行，而是在同一個工作流裡，讓每個步驟以最合適的方式運行------context 壓力小的步驟留在主會話保持流暢，context 壓力大的步驟透過 subagent 隔離保持乾淨，規範檢查透過 hook 自動執行不需要人工介入。

## 六、基於 SKILL 規範的數倉工作流設計

數倉 8 步 SKILL 規範（需求分析→技術設計→ETL→自測→資料比對→SR 導入→效能優化→SLA/DQC）天然對應 Harness 的三層機制。核心思路是：SKILL 檔案不變，改變調用方式------主對話只讀規範結論，實際執行由 subagent 或 hook 完成。

[![](https://i.imgur.com/adwLnqM.jpeg)](https://i.imgur.com/adwLnqM.jpeg)

### 各步驟推薦提示詞與工作流

#### Step 1：需求分析

推薦提示詞：

```
markdown 體驗AI代碼助手 代碼解讀複製代碼用 dw-explorer subagent 先讀取上游表結構（只返回摘要），
然後按需求分析規範生成：
1. 需求摘要（≤5行）
2. 表欄位口徑草稿
3. 待確認問題清單（按優先級排序）
需求文件 URL：[貼上PRD連結]
```

Hook 配合：SessionStart 注入當前迭代約束（版本號/表名/禁止修改的表）。

#### Step 2：技術設計

推薦提示詞：

```
css 體驗AI代碼助手 代碼解讀複製代碼基於上一步確認的需求，按 OneData 規範完成技術設計：
 表名：[按 層級_域_主題_粒度_週期 格式命名]
 粒度：[描述]
 分區：partition_dt string（格式 yyyyMMdd）
 禁止：任何與上游不一致的欄位命名
輸出 OneData 建模說明，不超過 60 行
```

[CLAUDE.md](http://claude.md/) 寫入（設計完成後手動更新）：

```
diff 體驗AI代碼助手 代碼解讀複製代碼## 當前迭代技術設計決策- 表名：db_a.dws_table_a
- 主鍵：order_no + partition_dt
- 特殊約束：amount 欄位繼承上游千元單位，不做轉換
```

#### Step 3：ETL 開發

這是 Harness 工程價值最高的步驟，PostToolUse hook 在每次 SQL 檔案儲存時自動觸發。

推薦提示詞：

```
sql 體驗AI代碼助手 代碼解讀複製代碼按 ETL 開發規範生成建表 DDL + Insert SQL：
- 建表文件：ddl_[表名].sql
- 插入文件：insert_[表名].sql
- 要求：INSERT 用 OVERWRITE 模式，PARTITION 子句必須包含 partition_dt
- 金額欄位：DECIMAL(20,4)，單位繼承上游（千元）
生成完畢後，用 sql-validator subagent 驗證兩個文件
```

Hook 自動執行（無需手動觸發）：每次寫入.sql 檔案 → PostToolUse hook 自動運行規範檢查。若發現 SELECT \* 或缺少 PARTITION → 返回 exit 2，Claude 收到錯誤自動修正。

#### Step 4：自測

推薦提示詞（防止自測結果撐爆主 context）：

```
css 體驗AI代碼助手 代碼解讀複製代碼用 data-quality-checker subagent 對 [表名] 執行 23 項標準自測，
bizdate = [日期]
補充口徑約束：[如"is_perform=1 只取履約訂單"]
只返回：PASS/FAIL 彙總 + FAIL 項詳情（≤50行），不返回原始 SQL 執行結果
```

效果：23 條 SQL 的執行結果全在 subagent context 裡，主對話只收到一份摘要報告。

#### Step 5：資料比對

推薦提示詞：

```
ini 體驗AI代碼助手 代碼解讀複製代碼用 data-comparator subagent 對比：
 新表：[新表名] partition_dt = [日期]
 參考表：[舊表名/線上表]
 比對欄位：[核心金額欄位列表]
 容差：≤ 0.01%（金額類）
只返回：差異超過容差的欄位列表 + 差值，不返回全量對比資料
```

#### Step 6：SR 資料庫導入

推薦提示詞（自動生成最優資料庫建表參數）：

```
markdown 體驗AI代碼助手 代碼解讀複製代碼用 dw-sr SKILL生成建表任務， 先查以下表的 DDL 和一層上下游血緣（只返回摘要）：
- 源表：[ODPS表名]
- 目標表：[SR表名]

然後基於 DDL 摘要，分析當前 SR 同步任務的配置風險：
1. 欄位類型是否有精度丟失風險（DECIMAL/DOUBLE → DECIMAL(38,18)）
2. Key 欄位選擇是否合理（重複率是否過高導致 DUPLICATE KEY 膨脹）
3. 分區數量是否合理（partition_live_number 與下游查詢視窗是否匹配）
4. DISTRIBUTED BY HASH 的 bucket 數與資料量是否匹配
5. 是否有 DATETIME 欄位在 SR 端用了 VARCHAR 存儲（會導致時間過濾走全表掃描）

輸出同步任務配置建議（按風險高低排序），不超過 20 行。
每條格式：[風險等級 高/中/低] 問題描述 → 建議修改方式
```

#### Step 7：效能優化

推薦提示詞（防止血緣查詢撐爆主 context）：

```
sql 體驗AI代碼助手 代碼解讀複製代碼用 dw-explorer subagent 先查 [表名] 的一層上下游血緣和 DDL（只返回摘要），
然後分析當前 Insert SQL 的效能瓶頸：
1. 是否有全表掃描
2. 是否有笛卡兒積風險
3. 是否可以用 MAP JOIN 取代 HASH JOIN
輸出優化建議（按收益排序），不超過 30 行
```

#### Step 8：SLA/DQC

推薦提示詞：

```
css 體驗AI代碼助手 代碼解讀複製代碼按 SLA/DQC 規範為 [表名] 生成 9 類 DQC 規則：
 完整性：主鍵非空、分區資料量
 準確性：核心金額欄位與上游比對（容差 0.01%）
 一致性：is_perform 與 perform_flag 聯動邏輯
 時效性：產出時間 SLA ≤ 次日 8:00
輸出 DQC 配置 JSON，可直接使用
```

### SKILL 調用方式改造（減少主 context 消耗）

當前問題：觸發 SKILL 時，SKILL 檔案全文（約 10KB）載入進主 context，加速 compact。改造方向：

[![](https://i.imgur.com/3oJTn1J.jpeg)](https://i.imgur.com/3oJTn1J.jpeg)

核心原則：主對話只接收決策級資訊（PASS/FAIL、差異欄位、優化方案），不接收過程資料（SQL 執行結果、原始血緣列表、完整 DDL 內容）。

### 精準對話流設計：控制 AI 思考的藝術

文章中多次提到 /dw-etl、/dw-自測 等命令，這是數倉研發全流程 SKILL 套件的核心觸發詞，每個命令對應一個封裝了規範、產出格式和工具調用的標準化執行單元。核心理念：把每次都要重複講的要求寫進 SKILL，把每次都怕忘的檢查點寫進 SKILL，把每次都需要的結構化輸出也寫進 SKILL------這樣誰來做需求都行，底座是一致的。

#### 8 個 SKILL 命令一覽

[![](https://i.imgur.com/Ya727af.jpeg)](https://i.imgur.com/Ya727af.jpeg)

以 /dw-etl 為例：一條命令封裝了什麼？ETL 開發（Step 3）是 Harness 工程價值最高的步驟，SKILL 檔案內封裝了：① 規範內容（寫死，不靠記憶）：建表規範：分區欄位必須是 partition\_dt STRING（格式 YYYYMMDD）；金額欄位：DECIMAL(26,4)，不用 DOUBLE；INSERT 模式：必須使用 INSERT OVERWRITE，必須帶 PARTITION 子句；禁止：SELECT \*、跨庫 JOIN（非血緣關係）。② 產出格式（結構化，不走樣）：

```
sql 體驗AI代碼助手 代碼解讀複製代碼輸出文件：
├── ddl_[表名].sql      ← ODPS 建表語句（含註解三段式、生命週期配置）
├── insert_[表名].sql   ← ODPS Insert SQL（含分區裁剪、JOIN 規範）
└── ddl_sr_[表名].sql   ← SR 建表語句（Key 欄位順序、DECIMAL 精度）
```

③ 自動護欄（Hook 配合）：每次.sql 檔案寫入後，PostToolUse hook 自動執行 validate\_sql.sh；發現 SELECT \* 或缺少 PARTITION → exit 2 阻斷，Claude 強制修正。④ subagent 卸載（防 context 膨脹）：「生成完畢後，用 sql-validator subagent 驗證兩個文件」；驗證結果全在 subagent context 裡，主對話只收到「PASS/FAIL + 問題列表」。綜合效益資料：

[![](https://i.imgur.com/magt7ky.jpeg)](https://i.imgur.com/magt7ky.jpeg)

## 七、落地步驟

**步驟一** ：專案級上下文持久化。在數倉專案目錄下建立.claude/ [CLAUDE.md](http://claude.md/) ，寫入當前迭代狀態。每次新迭代開始時更新「正在開發」和「本次迭代約束」兩節，上線後清空約束節。全域規範永久保留，控制在 100 行以內。

**步驟二** ：配置 hooks 自動驗證。建立.claude/settings.json + hooks/ 目錄，配置 PostToolUse hook（每次寫.sql 後自動規範檢查）和 PreToolUse hook（攔截危險 DDL）。效果：不需要每次手動說「幫我檢查 SQL 規範」，hook 在 Harness 層自動觸發，不佔用 Claude 推理資源。

**步驟三** ：建立 subagents 隔離高 token 操作。建立三個核心 subagent 檔案： [sql-validator.md](http://sql-validator.md/) 、 [dw-explorer.md](http://dw-explorer.md/) 、 [data-quality-checker.md](http://data-quality-checker.md/) 。將 Step 4/5/7 的執行全部下放，預計主 context compact 頻率降低 50%～70%。

## 八、Harness 工程能解決的核心問題

這一節是整個方案的出發點，也是對「為什麼要這麼做」的直接回答。

### 數倉 AI 開發當前的本質瓶頸：語義理解

在實際數倉 AI 開發中，技術能力不是瓶頸，語義理解才是。需求理解偏差占總返工的 40%～60%，大約 40%～50% 的工作時間花在理解需求和與業務核對口徑上。精準血緣探查：準確率提升顯著，遠超傳統方式。這兩個觀察揭示了同一個問題：AI 在數倉場景的不準確，根本原因不是「不會寫 SQL」，而是「不理解業務語義」------不知道這張表的 amount 單位是千元還是元，不知道 is\_perform=1 在這個版本裡意味著什麼，不知道某欄位在特定場景下會為空。

### 語義 × 資料 = 準確率

數倉 AI 開發的準確率，可以用一個公式來理解：

```
體驗AI代碼助手 代碼解讀複製代碼準確率 = 語義理解深度 × 資料規範覆蓋度
```

[![](https://i.imgur.com/qlsWgT5.jpeg)](https://i.imgur.com/qlsWgT5.jpeg)

結論：Harness 工程的本質，是把「語義」和「規範」從不可靠的 LLM 記憶中，遷移到確定性的 hooks + 持久化檔案裡，從而讓 語義 × 規範 = 準確率 這個等式兩邊的變數都變得穩定。

### 具體能解決的四類問題

#### 問題一：欄位口徑遺忘導致的計算錯誤

現狀：對話開始時告知了「amount 欄位單位是千元」，compact 後 Claude 忘了，生成的 SQL 把千元當元用，導致資料差 1000 倍。 **Harness 解法** ：欄位口徑寫進.claude/ [CLAUDE.md](http://claude.md/) （compact 後重新注入）；踩坑經驗寫進 Auto Memory（跨會話持久化）；結果：這類錯誤從「時常發生」降到「基本不出現」。

#### 問題二：需求理解偏差導致的返工

現狀：需求文件描述的是「用戶視角的 GMV」，但 AI 生成了「交易視角的 GMV」，兩者口徑不同，資料對不上，需要返工。 **Harness 解法** ：需求分析的產出（口徑草稿 + 待確認問題清單）寫進 [CLAUDE.md](http://claude.md/) 的迭代約束節；Stop hook 檢查任務完整性，確認待確認問題清單已全部明確後才放行；結果：需求一次交付通過率從約 50% 提升到 90%。

#### 問題三：SQL 規範執行不一致

現狀：規範文件寫了「INSERT 必須帶 PARTITION」，但 Claude 有時會忘，或在 compact 後規範內容被清除後生成不合規的 SQL。 **Harness 解法** ：PostToolUse hook 在每次寫.sql 檔案後自動執行規範檢查，不依賴 Claude 記憶；違規時 exit 2 返回錯誤，Claude 強制修正後才能繼續；結果：規範執行率從「依賴 LLM 記憶的 70%～80%」提升到「hook 強制的 95%+」。

#### 問題四：大型需求開發中的 context 耗盡

現狀：複雜需求（大型寬表、大量下游血緣）開發過程中，血緣查詢 + 自測結果 + SKILL 檔案內容堆滿 context，compact 觸發後丟失關鍵約束，Claude 開始犯低級錯誤。 **Harness 解法** ：血緣查詢 → dw-explorer subagent（獨立 context，只返回摘要）；23 項自測 → data-quality-checker subagent（獨立 context，只返回 PASS/FAIL 報告）；SKILL 檔案內容 → subagent 內部讀取，主 context 只接收產出的 SQL 檔案路徑；結果：主 context compact 頻率預計降低 50%～70%。

### 與傳統數倉 AI 開發方式的對比

[![](https://i.imgur.com/Aq5lgxS.jpeg)](https://i.imgur.com/Aq5lgxS.jpeg)

### 從「AI 幫我寫代碼」到「AI 嵌入研發流水線」

Harness 工程的最終目標，不是讓 Claude 更聰明，而是讓整個研發流水線更可靠。Claude（LLM）負責：理解需求、設計方案、生成代碼------這些需要語義理解的事；Harness（hooks）負責：規範檢查、危險攔截、任務完整性驗證------這些需要確定性執行的事；Subagents 負責：大量檔案讀取、血緣查詢、自測執行------這些會消耗大量 context 的事； [CLAUDE.md](http://claude.md/) + Memory 負責：欄位口徑、迭代約束、踩坑經驗------這些需要跨會話持久化的事。這四層分工，讓數倉 AI 開發從「對話驅動的一次性輔助」升級為「規則嵌入的流水線自動化」。

## 往期回顧

1.[BP Claw 破解 AI 編碼輸入難題 ------FlinkSpec 需求智慧化實踐｜得物技術](https://juejin.cn/post/7639261453330874410)

2.[基於 Harness + SDD + 多倉管理模式的 AI 全棧開發實踐｜得物技術](https://juejin.cn/post/7636724456984428607)

3.[通用 AI Agent 驅動網關路由安全審計實踐｜得物技術](https://juejin.cn/post/7634073298897125439)

4.[AI驅動：從營運行為到自動化用例的智慧化實踐｜得物技術](https://juejin.cn/post/7631495035478589450)

5.[生成式召回在得物的落地技術分享與思考](https://juejin.cn/post/7628888137545859081)

## 文 /丹克

關注得物技術，每週更新技術乾貨

要是覺得文章對你有幫助的話，歡迎評論轉發點讚～

未經得物技術許可嚴禁轉載，否則依法追究法律責任。

---

原文出處： [https://juejin.cn/post/7641896054783639571](https://juejin.cn/post/7641896054783639571)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3bbOA3) [25瀏覽](https://codelove.tw/@tony/post/3bbOA3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3bbOA3)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [50 多個必備 Telegram 機器人的終極清單](https://codelove.tw/@tony/post/37oZV3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝20 💬11 ❤️1

599

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

84

🥉

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

#4

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779847247&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3bbOA3&amp;pra=5&amp;asro=0&amp;itsi=3&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779847247901&amp;bpp=1&amp;bdt=88&amp;idt=55&amp;shv=r20260521&amp;mjsv=m202605210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779847014%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779847014%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779847014%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1217333233911&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=3&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=19808&amp;eid=95390667%2C95390787%2C31098762%2C95391804&amp;oid=2&amp;pvsid=3353703713850424&amp;tmod=941033422&amp;uas=0&amp;nvt=3&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=60" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260521/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>