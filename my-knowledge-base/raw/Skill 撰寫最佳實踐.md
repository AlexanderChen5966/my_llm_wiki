---
title: "Skill 撰寫最佳實踐"
source: "https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/best-practices"
author:
published:
created: 2026-05-18
description: "學習如何撰寫有效的 Skills，讓 Claude 能夠發現並成功使用。"
tags:
  - "clippings"
status: processed
---
良好的 Skills 應簡潔、結構清晰，並經過實際使用測試。本指南提供實用的撰寫決策，幫助您撰寫 Claude 能夠有效發現和使用的 Skills。

有關 Skills 運作方式的概念背景，請參閱 [Skills 概述](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/overview) 。

## 核心原則

### 簡潔是關鍵

[上下文視窗](https://platform.claude.com/docs/zh-TW/build-with-claude/context-windows) 是一種公共資源。您的 Skill 與 Claude 需要了解的所有其他內容共享上下文視窗，包括：

並非您 Skill 中的每個 token 都有即時成本。啟動時，只有所有 Skills 的元數據（名稱和描述）會被預先載入。Claude 只有在 Skill 變得相關時才會讀取 SKILL.md，並且只在需要時才讀取其他文件。然而，在 SKILL.md 中保持簡潔仍然很重要：一旦 Claude 載入它，每個 token 都會與對話歷史和其他上下文競爭。

**預設假設：** Claude 已經非常聰明

只添加 Claude 尚未擁有的上下文。對每條信息提出質疑：

- 「Claude 真的需要這個解釋嗎？」
- 「我可以假設 Claude 已經知道這個嗎？」
- 「這段文字值得其 token 成本嗎？」

**好例子：簡潔** （約 50 個 token）：

```
## 提取 PDF 文字

使用 pdfplumber 進行文字提取：

\`\`\`python
import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
\`\`\`
```

**壞例子：過於冗長** （約 150 個 token）：

```
## 提取 PDF 文字

PDF（可攜式文件格式）文件是一種常見的文件格式，包含文字、圖像和其他內容。
要從 PDF 中提取文字，您需要使用一個函式庫。有許多可用於 PDF 處理的函式庫，
但推薦使用 pdfplumber，因為它易於使用且能處理大多數情況。首先，您需要使用
pip 安裝它。然後您可以使用下面的程式碼...
```

簡潔版本假設 Claude 知道什麼是 PDF 以及函式庫的工作方式。

### 設定適當的自由度

將具體程度與任務的脆弱性和可變性相匹配。

**高自由度** （基於文字的指令）：

適用於：

- 多種方法都有效
- 決策取決於上下文
- 啟發式方法指導方式

範例：

```
## 程式碼審查流程

1. 分析程式碼結構和組織
2. 檢查潛在的錯誤或邊緣情況
3. 提出可讀性和可維護性的改進建議
4. 驗證是否符合專案慣例
```

**中等自由度** （帶參數的偽程式碼或腳本）：

適用於：

- 存在首選模式
- 某些變化是可接受的
- 配置影響行為

範例：

```
## 生成報告

使用此模板並根據需要自定義：

\`\`\`python
def generate_report(data, format="markdown", include_charts=True):
    # 處理數據
    # 以指定格式生成輸出
    # 可選地包含視覺化
\`\`\`
```

**低自由度** （特定腳本，少量或無參數）：

適用於：

- 操作脆弱且容易出錯
- 一致性至關重要
- 必須遵循特定順序

範例：

```
## 資料庫遷移

精確執行此腳本：

\`\`\`bash
python scripts/migrate.py --verify --backup
\`\`\`

不要修改命令或添加額外的標誌。
```

**類比：** 將 Claude 想像成一個探索路徑的機器人：

- **兩側有懸崖的窄橋：** 只有一條安全的前進路線。提供具體的護欄和精確的指令（低自由度）。範例：必須按精確順序執行的資料庫遷移。
- **沒有危險的開闊田野：** 許多路徑都能通向成功。給出大致方向並信任 Claude 找到最佳路線（高自由度）。範例：上下文決定最佳方法的程式碼審查。

### 使用您計劃使用的所有模型進行測試

Skills 作為模型的補充，因此有效性取決於底層模型。使用您計劃使用的所有模型測試您的 Skill。

**按模型的測試考量：**

- **Claude Haiku** （快速、經濟）：Skill 是否提供了足夠的指導？
- **Claude Sonnet** （平衡）：Skill 是否清晰且高效？
- **Claude Opus** （強大推理）：Skill 是否避免了過度解釋？

對 Opus 完美有效的內容可能需要為 Haiku 提供更多細節。如果您計劃在多個模型中使用您的 Skill，請以適合所有模型的指令為目標。

## Skill 結構

**YAML 前置元數據：** SKILL.md 前置元數據需要兩個欄位：

`name` ：

- 最多 64 個字元
- 只能包含小寫字母、數字和連字號
- 不能包含 XML 標籤
- 不能包含保留字：「anthropic」、「claude」

`description` ：

- 必須非空
- 最多 1024 個字元
- 不能包含 XML 標籤
- 應描述 Skill 的功能及何時使用它

有關完整的 Skill 結構詳情，請參閱 [Skills 概述](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/overview#skill-structure) 。

### 命名慣例

使用一致的命名模式使 Skills 更易於參考和討論。考慮使用 **動名詞形式** （動詞 + -ing）作為 Skill 名稱，因為這清楚地描述了 Skill 提供的活動或能力。

請記住， `name` 欄位只能使用小寫字母、數字和連字號。

**良好的命名範例（動名詞形式）：**

- `processing-pdfs`
- `analyzing-spreadsheets`
- `managing-databases`
- `testing-code`
- `writing-documentation`

**可接受的替代方案：**

- 名詞短語： `pdf-processing` 、 `spreadsheet-analysis`
- 動作導向： `process-pdfs` 、 `analyze-spreadsheets`

**避免：**

- 模糊的名稱： `helper` 、 `utils` 、 `tools`
- 過於通用： `documents` 、 `data` 、 `files`
- 保留字： `anthropic-helper` 、 `claude-tools`
- 在您的 skill 集合中使用不一致的模式

一致的命名使以下事項更容易：

- 在文件和對話中參考 Skills
- 一眼了解 Skill 的功能
- 組織和搜索多個 Skills
- 維護專業、有凝聚力的 skill 函式庫

### 撰寫有效的描述

`description` 欄位啟用 Skill 發現，應包括 Skill 的功能以及何時使用它。

**始終使用第三人稱撰寫** 。描述被注入系統提示中，不一致的視角可能導致發現問題。

- **好：** 「處理 Excel 文件並生成報告」
- **避免：** 「我可以幫您處理 Excel 文件」
- **避免：** 「您可以使用此功能處理 Excel 文件」

**具體說明並包含關鍵術語** 。包括 Skill 的功能以及何時使用它的具體觸發條件/上下文。

每個 Skill 只有一個描述欄位。描述對於 skill 選擇至關重要：Claude 使用它從可能超過 100 個可用 Skills 中選擇正確的 Skill。您的描述必須提供足夠的細節，讓 Claude 知道何時選擇此 Skill，而 SKILL.md 的其餘部分提供實作細節。

有效範例：

**PDF 處理 skill：**

```
description: 從 PDF 文件中提取文字和表格、填寫表單、合併文件。在處理 PDF 文件或用戶提到 PDF、表單或文件提取時使用。
```

**Excel 分析 skill：**

```
description: 分析 Excel 試算表、創建樞紐分析表、生成圖表。在分析 Excel 文件、試算表、表格數據或 .xlsx 文件時使用。
```

**Git Commit 助手 skill：**

```
description: 通過分析 git diff 生成描述性的 commit 訊息。當用戶請求幫助撰寫 commit 訊息或審查已暫存的更改時使用。
```

避免像這樣的模糊描述：

```
description: 幫助處理文件
```

```
description: 處理數據
```

```
description: 對文件做一些事情
```

### 漸進式揭露模式

SKILL.md 作為概述，根據需要將 Claude 指向詳細材料，就像入職指南中的目錄一樣。有關漸進式揭露工作原理的說明，請參閱概述中的 [Skills 的工作原理](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/overview#how-skills-work) 。

**實用指導：**

- 將 SKILL.md 主體保持在 500 行以下以獲得最佳性能
- 接近此限制時將內容拆分為單獨的文件
- 使用以下模式有效地組織指令、程式碼和資源

#### 視覺概述：從簡單到複雜

基本 Skill 從一個包含元數據和指令的 SKILL.md 文件開始：

![顯示 YAML 前置元數據和 markdown 主體的簡單 SKILL.md 文件](https://platform.claude.com/docs/images/agent-skills-simple-file.png)

隨著您的 Skill 增長，您可以捆綁 Claude 只在需要時載入的其他內容：

![捆綁其他參考文件，如 reference.md 和 forms.md。](https://platform.claude.com/docs/images/agent-skills-bundling-content.png)

完整的 Skill 目錄結構可能如下所示：

```
pdf/
├── SKILL.md              # 主要指令（觸發時載入）
├── FORMS.md              # 表單填寫指南（按需載入）
├── reference.md          # API 參考（按需載入）
├── examples.md           # 使用範例（按需載入）
└── scripts/
    ├── analyze_form.py   # 工具腳本（執行，不載入）
    ├── fill_form.py      # 表單填寫腳本
    └── validate.py       # 驗證腳本
```

#### 模式 1：帶參考的高層次指南

```
---
name: pdf-processing
description: 從 PDF 文件中提取文字和表格、填寫表單並合併文件。在處理 PDF 文件或用戶提到 PDF、表單或文件提取時使用。
---

# PDF 處理

## 快速開始

使用 pdfplumber 提取文字：
\`\`\`python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
\`\`\`

## 進階功能

**表單填寫**：完整指南請參閱 [FORMS.md](FORMS.md)
**API 參考**：所有方法請參閱 [REFERENCE.md](REFERENCE.md)
**範例**：常見模式請參閱 [EXAMPLES.md](EXAMPLES.md)
```

Claude 只在需要時載入 FORMS.md、REFERENCE.md 或 EXAMPLES.md。

#### 模式 2：特定領域的組織

對於具有多個領域的 Skills，按領域組織內容以避免載入不相關的上下文。當用戶詢問銷售指標時，Claude 只需要讀取與銷售相關的模式，而不是財務或行銷數據。這使 token 使用量保持低水平，上下文保持集中。

```
bigquery-skill/
├── SKILL.md（概述和導航）
└── reference/
    ├── finance.md（收入、帳單指標）
    ├── sales.md（機會、管道）
    ├── product.md（API 使用、功能）
    └── marketing.md（活動、歸因）
```

```
# BigQuery 數據分析

## 可用數據集

**財務**：收入、ARR、帳單 → 請參閱 [reference/finance.md](reference/finance.md)
**銷售**：機會、管道、帳戶 → 請參閱 [reference/sales.md](reference/sales.md)
**產品**：API 使用、功能、採用 → 請參閱 [reference/product.md](reference/product.md)
**行銷**：活動、歸因、電子郵件 → 請參閱 [reference/marketing.md](reference/marketing.md)

## 快速搜索

使用 grep 查找特定指標：

\`\`\`bash
grep -i "revenue" reference/finance.md
grep -i "pipeline" reference/sales.md
grep -i "api usage" reference/product.md
\`\`\`
```

#### 模式 3：條件性細節

顯示基本內容，連結到進階內容：

```
# DOCX 處理

## 創建文件

使用 docx-js 創建新文件。請參閱 [DOCX-JS.md](DOCX-JS.md)。

## 編輯文件

對於簡單的編輯，直接修改 XML。

**對於追蹤更改**：請參閱 [REDLINING.md](REDLINING.md)
**對於 OOXML 詳情**：請參閱 [OOXML.md](OOXML.md)
```

Claude 只有在用戶需要這些功能時才讀取 REDLINING.md 或 OOXML.md。

### 避免深度嵌套的參考

當文件從其他被參考的文件中被參考時，Claude 可能會部分讀取文件。遇到嵌套參考時，Claude 可能使用 `head -100` 等命令預覽內容而不是讀取整個文件，導致信息不完整。

**保持參考從 SKILL.md 只有一層深** 。所有參考文件應直接從 SKILL.md 連結，以確保 Claude 在需要時讀取完整文件。

**壞例子：太深** ：

```
# SKILL.md
請參閱 [advanced.md](advanced.md)...

# advanced.md
請參閱 [details.md](details.md)...

# details.md
這裡是實際信息...
```

**好例子：一層深** ：

```
# SKILL.md

**基本使用**：[SKILL.md 中的指令]
**進階功能**：請參閱 [advanced.md](advanced.md)
**API 參考**：請參閱 [reference.md](reference.md)
**範例**：請參閱 [examples.md](examples.md)
```

### 為較長的參考文件建立目錄

對於超過 100 行的參考文件，在頂部包含目錄。這確保 Claude 即使在部分讀取預覽時也能看到可用信息的完整範圍。

**範例：**

```
# API 參考

## 目錄
- 身份驗證和設置
- 核心方法（創建、讀取、更新、刪除）
- 進階功能（批次操作、webhooks）
- 錯誤處理模式
- 程式碼範例

## 身份驗證和設置
...

## 核心方法
...
```

Claude 然後可以讀取完整文件或根據需要跳到特定部分。

有關此基於文件系統的架構如何啟用漸進式揭露的詳情，請參閱下方進階部分中的 [執行環境](#runtime-environment) 部分。

## 工作流程和反饋循環

### 對複雜任務使用工作流程

將複雜操作分解為清晰、順序的步驟。對於特別複雜的工作流程，提供一個清單，Claude 可以將其複製到回應中並在進行時勾選。

**範例 1：研究綜合工作流程** （適用於沒有程式碼的 Skills）：

```
## 研究綜合工作流程

複製此清單並追蹤您的進度：

\`\`\`
研究進度：
- [ ] 步驟 1：閱讀所有來源文件
- [ ] 步驟 2：識別關鍵主題
- [ ] 步驟 3：交叉參考聲明
- [ ] 步驟 4：創建結構化摘要
- [ ] 步驟 5：驗證引用
\`\`\`

**步驟 1：閱讀所有來源文件**

審查 \`sources/\` 目錄中的每個文件。注意主要論點和支持證據。

**步驟 2：識別關鍵主題**

尋找來源之間的模式。哪些主題反覆出現？來源在哪些地方同意或不同意？

**步驟 3：交叉參考聲明**

對於每個主要聲明，驗證它出現在來源材料中。注意哪個來源支持每個觀點。

**步驟 4：創建結構化摘要**

按主題組織發現。包括：
- 主要聲明
- 來源的支持證據
- 衝突的觀點（如果有）

**步驟 5：驗證引用**

檢查每個聲明是否引用了正確的來源文件。如果引用不完整，返回步驟 3。
```

此範例展示了工作流程如何應用於不需要程式碼的分析任務。清單模式適用於任何複雜的多步驟流程。

**範例 2：PDF 表單填寫工作流程** （適用於有程式碼的 Skills）：

```
## PDF 表單填寫工作流程

複製此清單並在完成時勾選項目：

\`\`\`
任務進度：
- [ ] 步驟 1：分析表單（執行 analyze_form.py）
- [ ] 步驟 2：創建欄位映射（編輯 fields.json）
- [ ] 步驟 3：驗證映射（執行 validate_fields.py）
- [ ] 步驟 4：填寫表單（執行 fill_form.py）
- [ ] 步驟 5：驗證輸出（執行 verify_output.py）
\`\`\`

**步驟 1：分析表單**

執行：\`python scripts/analyze_form.py input.pdf\`

這會提取表單欄位及其位置，保存到 \`fields.json\`。

**步驟 2：創建欄位映射**

編輯 \`fields.json\` 為每個欄位添加值。

**步驟 3：驗證映射**

執行：\`python scripts/validate_fields.py fields.json\`

在繼續之前修復任何驗證錯誤。

**步驟 4：填寫表單**

執行：\`python scripts/fill_form.py input.pdf fields.json output.pdf\`

**步驟 5：驗證輸出**

執行：\`python scripts/verify_output.py output.pdf\`

如果驗證失敗，返回步驟 2。
```

清晰的步驟防止 Claude 跳過關鍵驗證。清單幫助 Claude 和您追蹤多步驟工作流程的進度。

### 實施反饋循環

**常見模式：** 執行驗證器 → 修復錯誤 → 重複

此模式大大提高了輸出質量。

**範例 1：風格指南合規性** （適用於沒有程式碼的 Skills）：

```
## 內容審查流程

1. 按照 STYLE_GUIDE.md 中的指南起草您的內容
2. 對照清單審查：
   - 檢查術語一致性
   - 驗證範例是否遵循標準格式
   - 確認所有必需部分都存在
3. 如果發現問題：
   - 記錄每個問題及具體部分參考
   - 修改內容
   - 再次審查清單
4. 只有在滿足所有要求時才繼續
5. 完成並保存文件
```

這展示了使用參考文件而不是腳本的驗證循環模式。「驗證器」是 STYLE\_GUIDE.md，Claude 通過讀取和比較來執行檢查。

**範例 2：文件編輯流程** （適用於有程式碼的 Skills）：

```
## 文件編輯流程

1. 對 \`word/document.xml\` 進行編輯
2. **立即驗證**：\`python ooxml/scripts/validate.py unpacked_dir/\`
3. 如果驗證失敗：
   - 仔細審查錯誤訊息
   - 修復 XML 中的問題
   - 再次執行驗證
4. **只有在驗證通過時才繼續**
5. 重新打包：\`python ooxml/scripts/pack.py unpacked_dir/ output.docx\`
6. 測試輸出文件
```

驗證循環可以及早發現錯誤。

## 內容指南

### 避免時效性信息

不要包含會過時的信息：

**壞例子：時效性** （將會變錯）：

```
如果您在 2025 年 8 月之前執行此操作，請使用舊 API。
2025 年 8 月之後，請使用新 API。
```

**好例子** （使用「舊模式」部分）：

```
## 當前方法

使用 v2 API 端點：\`api.example.com/v2/messages\`

## 舊模式

<details>
<summary>舊版 v1 API（2025-08 已棄用）</summary>

v1 API 使用：\`api.example.com/v1/messages\`

此端點不再受支持。
</details>
```

舊模式部分提供歷史上下文而不會使主要內容雜亂。

### 使用一致的術語

選擇一個術語並在整個 Skill 中使用它：

**好 - 一致：**

- 始終使用「API 端點」
- 始終使用「欄位」
- 始終使用「提取」

**壞 - 不一致：**

- 混用「API 端點」、「URL」、「API 路由」、「路徑」
- 混用「欄位」、「框」、「元素」、「控制項」
- 混用「提取」、「拉取」、「獲取」、「檢索」

一致性幫助 Claude 理解和遵循指令。

## 常見模式

### 模板模式

為輸出格式提供模板。將嚴格程度與您的需求相匹配。

**對於嚴格要求** （如 API 回應或數據格式）：

```
## 報告結構

始終使用此精確的模板結構：

\`\`\`markdown
# [分析標題]

## 執行摘要
[關鍵發現的一段概述]

## 關鍵發現
- 帶支持數據的發現 1
- 帶支持數據的發現 2
- 帶支持數據的發現 3

## 建議
1. 具體可行的建議
2. 具體可行的建議
\`\`\`
```

**對於靈活指導** （當適應有用時）：

```
## 報告結構

這是一個合理的預設格式，但請根據分析使用您的最佳判斷：

\`\`\`markdown
# [分析標題]

## 執行摘要
[概述]

## 關鍵發現
[根據您發現的內容調整部分]

## 建議
[針對具體上下文量身定制]
\`\`\`

根據具體分析類型調整部分。
```

### 範例模式

對於輸出質量取決於看到範例的 Skills，提供輸入/輸出對，就像在常規提示中一樣：

```
## Commit 訊息格式

按照這些範例生成 commit 訊息：

**範例 1：**
輸入：添加了使用 JWT token 的用戶身份驗證
輸出：
\`\`\`
feat(auth): implement JWT-based authentication

Add login endpoint and token validation middleware
\`\`\`

**範例 2：**
輸入：修復了報告中日期顯示不正確的錯誤
輸出：
\`\`\`
fix(reports): correct date formatting in timezone conversion

Use UTC timestamps consistently across report generation
\`\`\`

**範例 3：**
輸入：更新了依賴項並重構了錯誤處理
輸出：
\`\`\`
chore: update dependencies and refactor error handling

- Upgrade lodash to 4.17.21
- Standardize error response format across endpoints
\`\`\`

遵循此風格：type(scope): 簡短描述，然後是詳細說明。
```

範例幫助 Claude 比單純的描述更清楚地理解所需的風格和細節程度。

### 條件工作流程模式

引導 Claude 通過決策點：

```
## 文件修改工作流程

1. 確定修改類型：

   **創建新內容？** → 遵循下面的「創建工作流程」
   **編輯現有內容？** → 遵循下面的「編輯工作流程」

2. 創建工作流程：
   - 使用 docx-js 函式庫
   - 從頭構建文件
   - 導出為 .docx 格式

3. 編輯工作流程：
   - 解包現有文件
   - 直接修改 XML
   - 每次更改後驗證
   - 完成後重新打包
```

如果工作流程變得龐大或複雜，有許多步驟，考慮將它們推入單獨的文件，並告訴 Claude 根據手頭的任務讀取適當的文件。

## 評估和迭代

### 首先建立評估

**在撰寫大量文件之前創建評估。** 這確保您的 Skill 解決真實問題，而不是記錄想象中的問題。

**評估驅動開發：**

1. **識別差距：** 在沒有 Skill 的情況下對代表性任務執行 Claude。記錄具體的失敗或缺失的上下文
2. **創建評估：** 建立三個測試這些差距的場景
3. **建立基準：** 測量 Claude 在沒有 Skill 的情況下的性能
4. **撰寫最少指令：** 創建剛好足夠解決差距並通過評估的內容
5. **迭代：** 執行評估，與基準比較，並改進

此方法確保您解決實際問題，而不是預測可能永遠不會實現的需求。

**評估結構：**

```
{
  "skills": ["pdf-processing"],
  "query": "從此 PDF 文件中提取所有文字並保存到 output.txt",
  "files": ["test-files/document.pdf"],
  "expected_behavior": [
    "使用適當的 PDF 處理函式庫或命令行工具成功讀取 PDF 文件",
    "從文件的所有頁面提取文字內容，不遺漏任何頁面",
    "以清晰、可讀的格式將提取的文字保存到名為 output.txt 的文件中"
  ]
}
```

此範例演示了帶有簡單測試標準的數據驅動評估。目前沒有內建方式來執行這些評估。用戶可以創建自己的評估系統。評估是衡量 Skill 有效性的真實來源。

### 與 Claude 迭代開發 Skills

最有效的 Skill 開發流程涉及 Claude 本身。與一個 Claude 實例（「Claude A」）合作創建一個由其他實例（「Claude B」）使用的 Skill。Claude A 幫助您設計和改進指令，而 Claude B 在真實任務中測試它們。這之所以有效，是因為 Claude 模型既理解如何撰寫有效的代理指令，也理解代理需要什麼信息。

**創建新 Skill：**

1. **在沒有 Skill 的情況下完成任務：** 使用正常提示與 Claude A 一起解決問題。在工作過程中，您自然會提供上下文、解釋偏好並分享程序知識。注意您反覆提供的信息。
2. **識別可重用模式：** 完成任務後，識別您提供的哪些上下文對類似的未來任務有用。
	**範例：** 如果您完成了 BigQuery 分析，您可能提供了表名、欄位定義、過濾規則（如「始終排除測試帳戶」）和常見查詢模式。
3. **請 Claude A 創建 Skill：** 「創建一個捕獲我們剛剛使用的 BigQuery 分析模式的 Skill。包括表模式、命名慣例以及關於過濾測試帳戶的規則。」
	Claude 模型原生理解 Skill 格式和結構。您不需要特殊的系統提示或「撰寫 skills」的 skill 來讓 Claude 幫助創建 Skills。只需請 Claude 創建一個 Skill，它就會生成具有適當前置元數據和主體內容的正確結構化 SKILL.md 內容。
4. **審查簡潔性：** 檢查 Claude A 是否添加了不必要的解釋。詢問：「刪除關於勝率含義的解釋——Claude 已經知道這一點。」
5. **改善信息架構：** 請 Claude A 更有效地組織內容。例如：「組織這個，使表模式在單獨的參考文件中。我們以後可能會添加更多表。」
6. **在類似任務上測試：** 在相關用例上使用 Claude B（載入了 Skill 的新實例）使用 Skill。觀察 Claude B 是否找到正確的信息、正確應用規則並成功處理任務。
7. **根據觀察迭代：** 如果 Claude B 遇到困難或遺漏了某些內容，帶著具體信息返回 Claude A：「當 Claude 使用此 Skill 時，它忘記按日期過濾 Q4。我們應該添加一個關於日期過濾模式的部分嗎？」

**迭代現有 Skills：**

改進 Skills 時，相同的層次模式繼續。您在以下之間交替：

- **與 Claude A 合作** （幫助改進 Skill 的專家）
- **與 Claude B 測試** （使用 Skill 執行真實工作的代理）
- **觀察 Claude B 的行為** 並將見解帶回 Claude A
1. **在真實工作流程中使用 Skill：** 給 Claude B（載入了 Skill）實際任務，而不是測試場景
2. **觀察 Claude B 的行為：** 注意它在哪裡遇到困難、成功或做出意外選擇
	**觀察範例：** 「當我請 Claude B 提供地區銷售報告時，它寫了查詢但忘記過濾測試帳戶，即使 Skill 提到了這條規則。」
3. **返回 Claude A 進行改進：** 分享當前的 SKILL.md 並描述您觀察到的內容。詢問：「我注意到 Claude B 在我請求地區報告時忘記過濾測試帳戶。Skill 提到了過濾，但也許不夠突出？」
4. **審查 Claude A 的建議：** Claude A 可能建議重新組織以使規則更突出、使用更強的語言如「必須過濾」而不是「始終過濾」，或重構工作流程部分。
5. **應用並測試更改：** 使用 Claude A 的改進更新 Skill，然後在類似請求上再次使用 Claude B 測試
6. **根據使用情況重複：** 在遇到新場景時繼續此觀察-改進-測試循環。每次迭代都根據真實代理行為而不是假設來改進 Skill。

**收集團隊反饋：**

1. 與隊友分享 Skills 並觀察他們的使用情況
2. 詢問：Skill 是否在預期時激活？指令是否清晰？缺少什麼？
3. 納入反饋以解決您自己使用模式中的盲點

**為什麼此方法有效：** Claude A 理解代理需求，您提供領域專業知識，Claude B 通過真實使用揭示差距，迭代改進根據觀察到的行為而不是假設來改進 Skills。

### 觀察 Claude 如何導航 Skills

在迭代 Skills 時，注意 Claude 在實踐中實際如何使用它們。注意：

- **意外的探索路徑：** Claude 是否以您未預期的順序讀取文件？這可能表明您的結構不如您想象的那樣直觀
- **錯過的連接：** Claude 是否未能跟隨對重要文件的參考？您的連結可能需要更明確或更突出
- **對某些部分的過度依賴：** 如果 Claude 反覆讀取同一個文件，考慮該內容是否應該在主 SKILL.md 中
- **被忽略的內容：** 如果 Claude 從不訪問捆綁的文件，它可能是不必要的或在主要指令中信號不足

根據這些觀察而不是假設進行迭代。您 Skill 元數據中的「name」和「description」特別關鍵。Claude 在決定是否根據當前任務觸發 Skill 時使用這些。確保它們清楚地描述 Skill 的功能以及何時應該使用它。

## 要避免的反模式

### 避免 Windows 風格的路徑

在文件路徑中始終使用正斜線，即使在 Windows 上：

- ✓ **好：** `scripts/helper.py` 、 `reference/guide.md`
- ✗ **避免：** `scripts\helper.py` 、 `reference\guide.md`

Unix 風格的路徑在所有平台上都有效，而 Windows 風格的路徑在 Unix 系統上會導致錯誤。

### 避免提供太多選項

除非必要，否則不要呈現多種方法：

```
**壞例子：太多選擇**（令人困惑）：
「您可以使用 pypdf，或 pdfplumber，或 PyMuPDF，或 pdf2image，或...」

**好例子：提供預設值**（帶有逃生艙口）：
「使用 pdfplumber 進行文字提取：
\`\`\`python
import pdfplumber
\`\`\`

對於需要 OCR 的掃描 PDF，請改用 pdf2image 和 pytesseract。」
```

## 進階：帶有可執行程式碼的 Skills

以下部分重點介紹包含可執行腳本的 Skills。如果您的 Skill 只使用 markdown 指令，請跳到 [有效 Skills 的清單](#checklist-for-effective-skills) 。

### 解決問題，不要推卸責任

在為 Skills 撰寫腳本時，處理錯誤條件而不是推卸給 Claude。

**好例子：明確處理錯誤：**

```
def process_file(path):
    """處理文件，如果不存在則創建它。"""
    try:
        with open(path) as f:
            return f.read()
    except FileNotFoundError:
        # 創建帶有預設內容的文件而不是失敗
        print(f"文件 {path} 未找到，正在創建預設值")
        with open(path, "w") as f:
            f.write("")
        return ""
    except PermissionError:
        # 提供替代方案而不是失敗
        print(f"無法訪問 {path}，使用預設值")
        return ""
```

**壞例子：推卸給 Claude：**

```
def process_file(path):
    # 只是失敗並讓 Claude 解決
    return open(path).read()
```

配置參數也應該有理由並記錄，以避免「巫毒常數」（Ousterhout 定律）。如果您不知道正確的值，Claude 如何確定它？

**好例子：自我記錄：**

```
# HTTP 請求通常在 30 秒內完成
# 較長的超時時間考慮到慢速連接
REQUEST_TIMEOUT = 30

# 三次重試在可靠性和速度之間取得平衡
# 大多數間歇性故障在第二次重試時解決
MAX_RETRIES = 3
```

**壞例子：魔法數字：**

```
TIMEOUT = 47  # 為什麼是 47？
RETRIES = 5  # 為什麼是 5？
```

### 提供工具腳本

即使 Claude 可以撰寫腳本，預製腳本也有優勢：

**工具腳本的好處：**

- 比生成的程式碼更可靠
- 節省 token（無需在上下文中包含程式碼）
- 節省時間（無需生成程式碼）
- 確保跨使用的一致性

![將可執行腳本與指令文件捆綁在一起](https://platform.claude.com/docs/images/agent-skills-executable-scripts.png)

上圖顯示了可執行腳本如何與指令文件一起工作。指令文件（forms.md）引用腳本，Claude 可以執行它而無需將其內容載入上下文。

**重要區別：** 在您的指令中明確說明 Claude 應該：

- **執行腳本** （最常見）：「執行 `analyze_form.py` 以提取欄位」
- **作為參考讀取** （對於複雜邏輯）：「有關欄位提取算法，請參閱 `analyze_form.py` 」

對於大多數工具腳本，執行是首選，因為它更可靠且高效。有關腳本執行工作原理的詳情，請參閱下面的 [執行環境](#runtime-environment) 部分。

**範例：**

```
## 工具腳本

**analyze_form.py**：從 PDF 中提取所有表單欄位

\`\`\`bash
python scripts/analyze_form.py input.pdf > fields.json
\`\`\`

輸出格式：
\`\`\`json
{
  "field_name": {"type": "text", "x": 100, "y": 200},
  "signature": {"type": "sig", "x": 150, "y": 500}
}
\`\`\`

**validate_boxes.py**：檢查重疊的邊界框

\`\`\`bash
python scripts/validate_boxes.py fields.json
# 返回：「OK」或列出衝突
\`\`\`

**fill_form.py**：將欄位值應用於 PDF

\`\`\`bash
python scripts/fill_form.py input.pdf fields.json output.pdf
\`\`\`
```

### 使用視覺分析

當輸入可以渲染為圖像時，讓 Claude 進行分析：

```
## 表單版面分析

1. 將 PDF 轉換為圖像：
   \`\`\`bash
   python scripts/pdf_to_images.py form.pdf
   \`\`\`

2. 分析每個頁面圖像以識別表單欄位
3. Claude 可以視覺化地看到欄位位置和類型
```

在此範例中，您需要自行撰寫 `pdf_to_images.py` 腳本。

Claude 的視覺功能有助於理解版面和結構。

### 建立可驗證的中間輸出

當 Claude 執行複雜的開放式任務時，可能會出現錯誤。「計劃-驗證-執行」模式透過讓 Claude 先以結構化格式建立計劃，然後在執行前用腳本驗證該計劃，從而及早發現錯誤。

**範例：** 假設您要求 Claude 根據試算表更新 PDF 中的 50 個表單欄位。若沒有驗證，Claude 可能會引用不存在的欄位、建立衝突的值、遺漏必填欄位，或錯誤地套用更新。

**解決方案：** 使用上述工作流程模式（PDF 表單填寫），但新增一個中間的 `changes.json` 檔案，在套用變更前進行驗證。工作流程變為：分析 → **建立計劃檔案** → **驗證計劃** → 執行 → 驗證。

**此模式有效的原因：**

- **及早發現錯誤：** 驗證在套用變更前找出問題
- **機器可驗證：** 腳本提供客觀驗證
- **可逆規劃：** Claude 可以在不觸及原始檔案的情況下反覆修改計劃
- **清晰除錯：** 錯誤訊息指向具體問題

**使用時機：** 批次操作、破壞性變更、複雜驗證規則、高風險操作。

**實作提示：** 讓驗證腳本輸出詳細的具體錯誤訊息，例如「找不到欄位 'signature\_date'。可用欄位：customer\_name、order\_total、signature\_date\_signed」，以幫助 Claude 修正問題。

### 套件依賴

Skills 在具有平台特定限制的程式碼執行環境中運行：

- **claude.ai：** 可以從 npm 和 PyPI 安裝套件，並從 GitHub 儲存庫拉取
- **Claude API：** 沒有網路存取，也無法在執行時安裝套件

在您的 SKILL.md 中列出所需套件，並在 [程式碼執行工具文件](https://platform.claude.com/docs/zh-TW/agents-and-tools/tool-use/code-execution-tool) 中確認它們是否可用。

### 執行環境

Skills 在具有檔案系統存取、bash 命令和程式碼執行功能的程式碼執行環境中運行。有關此架構的概念說明，請參閱概述中的 [Skills 架構](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/overview#the-skills-architecture) 。

**這如何影響您的撰寫：**

**Claude 如何存取 Skills：**

1. **預先載入元資料：** 啟動時，所有 Skills 的 YAML 前置資料中的名稱和描述會載入到系統提示中
2. **按需讀取檔案：** Claude 在需要時使用 bash Read 工具從檔案系統存取 SKILL.md 和其他檔案
3. **高效執行腳本：** 工具腳本可以透過 bash 執行，無需將其完整內容載入上下文。只有腳本的輸出會消耗 token
4. **大型檔案無上下文負擔：** 參考檔案、資料或文件在實際讀取之前不會消耗上下文 token
- **檔案路徑很重要：** Claude 像瀏覽檔案系統一樣導航您的 skill 目錄。使用正斜線（ `reference/guide.md` ），而非反斜線
- **以描述性方式命名檔案：** 使用能表示內容的名稱： `form_validation_rules.md` ，而非 `doc2.md`
- **為便於發現而組織：** 按領域或功能組織目錄結構
	- 好的做法： `reference/finance.md` 、 `reference/sales.md`
		- 不好的做法： `docs/file1.md` 、 `docs/file2.md`
- **捆綁全面的資源：** 包含完整的 API 文件、大量範例、大型資料集；在存取之前不會有上下文負擔
- **對確定性操作優先使用腳本：** 撰寫 `validate_form.py` ，而非要求 Claude 生成驗證程式碼
- **明確說明執行意圖：**
	- 「執行 `analyze_form.py` 以提取欄位」（執行）
		- 「參閱 `analyze_form.py` 了解提取演算法」（作為參考讀取）
- **測試檔案存取模式：** 透過實際請求測試，驗證 Claude 能夠導航您的目錄結構

**範例：**

```
bigquery-skill/
├── SKILL.md（概述，指向參考檔案）
└── reference/
    ├── finance.md（收入指標）
    ├── sales.md（管道資料）
    └── product.md（使用分析）
```

當使用者詢問收入時，Claude 讀取 SKILL.md，看到對 `reference/finance.md` 的參考，並呼叫 bash 只讀取該檔案。sales.md 和 product.md 檔案保留在檔案系統中，在需要之前消耗零上下文 token。這種基於檔案系統的模型使漸進式揭露成為可能。Claude 可以導航並選擇性地載入每個任務所需的確切內容。

有關技術架構的完整詳情，請參閱 Skills 概述中的 [Skills 工作原理](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/overview#how-skills-work) 。

### MCP 工具參考

如果您的 Skill 使用 MCP（Model Context Protocol）工具，請始終使用完整限定的工具名稱，以避免「找不到工具」的錯誤。

**格式：** `ServerName:tool_name`

**範例：**

```
使用 BigQuery:bigquery_schema 工具來擷取資料表結構描述。
使用 GitHub:create_issue 工具來建立問題。
```

其中：

- `BigQuery` 和 `GitHub` 是 MCP 伺服器名稱
- `bigquery_schema` 和 `create_issue` 是這些伺服器中的工具名稱

若沒有伺服器前綴，Claude 可能無法找到工具，尤其是當多個 MCP 伺服器可用時。

### 避免假設工具已安裝

不要假設套件已可用：

```
**不好的範例：假設已安裝**：
「使用 pdf 函式庫來處理檔案。」

**好的範例：明確說明依賴**：
「安裝所需套件：\`pip install pypdf\`

然後使用它：
\`\`\`python
from pypdf import PdfReader
reader = PdfReader("file.pdf")
\`\`\`"
```

## 技術說明

### YAML 前置資料要求

SKILL.md 前置資料需要 `name` 和 `description` 欄位，並有特定的驗證規則：

- `name` ：最多 64 個字元，僅限小寫字母/數字/連字號，不含 XML 標籤，不含保留字
- `description` ：最多 1024 個字元，不得為空，不含 XML 標籤

有關完整結構詳情，請參閱 [Skills 概述](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/overview#skill-structure) 。

### Token 預算

將 SKILL.md 主體保持在 500 行以下以獲得最佳效能。如果您的內容超過此限制，請使用前面描述的漸進式揭露模式將其拆分為單獨的檔案。有關架構詳情，請參閱 [Skills 概述](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/overview#how-skills-work) 。

## 有效 Skills 的檢查清單

在分享 Skill 之前，請驗證：

### 核心品質

- 描述具體且包含關鍵術語
- 描述包含 Skill 的功能和使用時機
- SKILL.md 主體在 500 行以下
- 額外詳情在單獨的檔案中（如需要）
- 沒有時效性資訊（或在「舊模式」部分）
- 全文術語一致
- 範例具體，而非抽象
- 檔案參考只有一層深度
- 適當使用漸進式揭露
- 工作流程有清晰的步驟

### 程式碼和腳本

- 腳本解決問題，而非推給 Claude
- 錯誤處理明確且有幫助
- 沒有「魔法常數」（所有值都有說明）
- 所需套件列在說明中並確認可用
- 腳本有清晰的文件說明
- 沒有 Windows 風格路徑（全部使用正斜線）
- 關鍵操作有驗證/確認步驟
- 品質關鍵任務包含回饋迴圈

### 測試

- 至少建立三個評估
- 已使用 Haiku、Sonnet 和 Opus 測試
- 已使用真實使用場景測試
- 已納入團隊回饋（如適用）

## 後續步驟[開始使用 Agent Skills](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/quickstart)

[

建立您的第一個 Skill

](https://platform.claude.com/docs/zh-TW/agents-and-tools/agent-skills/quickstart)[

在 Claude Code 中使用 Skills

在 Claude Code 中建立和管理 Skills

](https://code.claude.com/docs/en/skills)[

透過 API 使用 Skills

以程式化方式上傳和使用 Skills

](https://platform.claude.com/docs/zh-TW/build-with-claude/skills-guide)

Was this page helpful?