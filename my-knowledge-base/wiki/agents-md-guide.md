---
title: AGENTS.md 完整指南（設計原則、效果研究、AI-Ready 倉庫）
date: 2026-05-14
tags: [ai/agent, ai/claude, dev/architecture]
source: https://zhuanlan.zhihu.com/p/2009034141291788197
type: article
status: processed
asset_value: 80
---

AGENTS.md 是跨工具通用標準（Claude Code/Cursor/Copilot/Codex 皆支援），放在專案根目錄作為 AI 的「README」。研究顯示 LLM 自動生成的 AGENTS.md 負收益，人工撰寫約 +4% 成效——關鍵在於寫「AI 無法從程式碼直接推斷的業務知識」，而非泛泛的目錄概覽。

## T（核心論點）
AGENTS.md 的價值不在長度，而在**不可推斷的隱性知識密度**——只寫 AI 反覆犯錯的那一類資訊（工具鏈指令、限制條件、任務路由），而不是目錄結構概覽。LLM 自動生成的版本反而讓成本增加 20%+、成功率下降 3%。

## B（適用場景）
**適用：** 多人團隊混用不同 AI 工具；新成員接手；需要統一 AI 行為的大型專案
**不適用：** 所有人都用 Claude Code 的小專案（直接用 CLAUDE.md 就好）；文檔已很完善的開源專案

## R（操作步驟）
5 個設計原則（適用 AGENTS.md / CLAUDE.md）：
1. **規則附上「為什麼」**：`使用 App Router（Pages Router 正在淘汰）` 而非只寫「使用 App Router」
2. **明確 NEVER 區塊**：根據 AI 實際犯過的錯誤逐步補上
3. **提供 2–3 個程式碼範例**：用專案真實程式碼，不用通用範例
4. **一行啟動命令**：`npm run validate`（typecheck + lint + test 一次跑完）
5. **明確權限邊界**：可自由修改 / 需確認 / 絕對不可以

少寫「目錄導覽」，多寫「任務路由」（新增 API → 看哪個檔案）

## C（關鍵行動）
- [ ] 🔴 **2026-09-23 修訂**（依據：Claude Code 2.1.277 changelog，2026-09-18；實測見 [[claude-code-agents-md-loading-rules]]）——
  原斷言「Claude Code 不會自動讀取 AGENTS.md，需在 CLAUDE.md 用 `@AGENTS.md` 引入」**自 2.1.277 起已過時**。
  正確規則是：**專案內沒有 `CLAUDE.md` 時，Claude Code 會自動讀 `AGENTS.md`**；有 `CLAUDE.md`（或 `CLAUDE.local.md`）時才只讀 CLAUDE 那一份。
  要兩份都讀，設 `pluginConfigs["agents-md@builtin"].options.instructionFiles = "claude-md-and-agents-md"`，
  且**必須寫在 `~/.claude/settings.json`**（專案／本機設定會被忽略）。
  ⚠️ 兩個靜默失效：升級後**第一次 session 不讀**；專案裡多一個 `CLAUDE.local.md` 會讓 `AGENTS.md` 不再被讀取。
  `@AGENTS.md` 引入仍然可用，但已不是唯一手段——若你本來就維護 `CLAUDE.md`，行為與過去相同。
- [ ] 只在 AI 犯錯後才添加針對性補丁，避免冗長文件的副作用
- [ ] 支援子目錄嵌套 AGENTS.md 實現「按需加載」，避免全局污染
- [ ] 目錄結構改用 Vertical Slice（功能切片）讓 AI 更好定位相關檔案

## 效果研究摘要
arxiv 論文《Evaluating AGENTS.md》結論（2026-02）：
- LLM 生成版：成功率平均 **-3%**，成本增加 **+20%**
- 人工撰寫版：成功率平均 **+4%**（但不穩定，不跨模型一致）
- 根本原因：context file 讓 Agent 更「謹慎/折騰」而不是更「聰明」，增加步驟但不增加命中率

## AI-Ready 倉庫自我檢查清單（15 項）
- 文件（5）：AGENTS.md 在根目錄、專案概要+架構、NEVER 區塊、一鍵 Setup、2-3 程式碼範例
- 結構（5）：Vertical Slice 目錄、JSDoc 型別、結構化錯誤碼、子目錄 README、validate 腳本
- 驗證（5）：測試覆蓋 80%+、pre-commit hook、CI 自動執行、覆蓋率閾值、PR 模板

## 補充視角：AI-Ready 倉庫設計三大支柱

來源：日本工程師文章（codelove.tw 譯本，2026-03-22）：

### 支柱一：AGENTS.md / CLAUDE.md 設計

**五原則的具體範例：**
- 規則附「為什麼」：`使用 App Router（Pages Router 正在淘汰）` 而非只寫「使用 App Router」
- NEVER 區塊要根據 AI 實際犯過的錯補上，不要空想
- CLAUDE.md 為主管理，AGENTS.md 建 symlink 共用：`ln -s CLAUDE.md AGENTS.md`

**AGENTS.md 與 CLAUDE.md 的角色差異：**
- `AGENTS.md`：通用，適用 Copilot / Codex 等（建議必備）
- `CLAUDE.md`：Claude Code 每次會自動讀取的設定檔（使用 Claude Code 必備）
- `.cursorrules`：給 Cursor 的對應設定檔

### 支柱二：目錄結構最佳化

**Vertical Slice（功能切片）vs 分層式架構：**
- 分層式：`controllers/` + `services/` + `repositories/` → AI 要跨多資料夾找相關檔案
- Vertical Slice：`features/users/`（含 api.ts + service.ts + repository.ts + types.ts + tests/）→ AI 一個資料夾就拿到完整功能所需 context

**AI 真正看的五個東西：**
1. 根目錄文件（AGENTS.md / CLAUDE.md / README.md / package.json）
2. 目錄結構（等同 `tree` 命令的整體概況）
3. 型別定義（TypeScript .d.ts / OpenAPI 規格）
4. 測試程式碼（行為規格書）
5. Linter / Formatter 設定

### 支柱三：驗證策略

**`npm run validate` 的重要性：** 把 typecheck + lint + test 合成一個指令，AI 改完後自動知道有沒有破壞東西，不需口頭每次要求

**覆蓋率與 AI 輸出品質正相關：** 測試越完整，AI 越清楚每個函式對哪些輸入應回傳什麼，減少猜測

**CI 覆蓋率閾值：** 設定 80% 門檻後，AI 新增程式碼必須同時寫測試才能讓 CI 通過，免去口頭提醒

## 補充資料（來源：cross-tool-ai-setup.md，B2B Manager 實戰蒸餾）

### 跨工具 Context 檔案對照表

| 工具                   | 原生讀取檔案                                        | 支援 @import        | 說明                                         |
| -------------------- | --------------------------------------------- | ----------------- | ------------------------------------------ |
| **Claude Code**      | `CLAUDE.md`、`.claude/CLAUDE.md`               | ✅ `@path/to/file` | + `.claude/rules/*.md` 自動載入                |
| **Gemini CLI**       | `GEMINI.md`                                   | ✅ `@path/to/file` | 支援 5 層遞迴 import                            |
| **OpenAI Codex CLI** | `AGENTS.md`                                   | ❌                 | Linux Foundation AAIF 標準                   |
| **VS Code Copilot**  | `.github/copilot-instructions.md`、`AGENTS.md` | ❌                 | + `.github/instructions/*.instructions.md` |
| **Cursor**           | `.cursorrules`、`.cursor/rules/*.mdc`          | ❌                 | 有 token 上限（~2000 tokens）                   |
| **Windsurf**         | `.windsurfrules`、`AGENTS.md`                  | ❌                 | 兩者都讀，優先 `.windsurfrules`                   |

> **趨勢：** `AGENTS.md` 正成為跨工具事實標準，Windsurf、Codex、VS Code Copilot 均支援。

### 推薦的多工具標準架構（單一維護點）

```
AGENTS.md               ← 單一維護點：完整共用規則
GEMINI.md               ← @./AGENTS.md（+ Gemini 專屬補充）
CLAUDE.md               ← @AGENTS.md（+ Claude 專屬補充）
.claude/rules/          ← Claude Code 自動載入的分類規則
.github/
  └── copilot-instructions.md  ← 精簡版，指向 AGENTS.md 為主
.cursorrules            ← 精簡版（有 token 限制）
```

**設計原則：**
- **不要讓每份工具文件都自行維護完整規則** → 容易失同步
- **@import 不節省 token**：imported 檔案在啟動時展開進入 context，等同複製貼上
- **官方建議**：每份 CLAUDE.md 控制在 200 行以內（**target 不是 limit**）（🔴 2026-09-03 更正：**理由是 context 成本，不是遵守率**——1,650 session 實測顯示 25→500 行對遵守率無可測量影響；「只讀前 200 行」的硬截斷屬 `MEMORY.md`。詳見 [[claude-md-guide]]）

### `.claude/rules/` 路徑專屬規則（`paths` frontmatter）

```markdown
---
paths:
  - "src/api/**/*.ts"
---
# API 相關規則
[只在處理上述路徑時才載入]
```

當 Claude 操作匹配路徑的檔案時才載入，不是每次 tool call 都觸發。

### B2B Manager 實際數據（架構重整前後對比）

| 指標 | 改前（v4.0）| 改後（v5.0）|
|------|------------|------------|
| CLAUDE.md 行數 | 215 行 | 84 行（@import 後展開 ~184 行）|
| 維護點數量 | 3 份 | 1 份（AGENTS.md）+ 輕度同步 |
| Claude Code 總 context | ~315 行 | ~250 行 |

## 相關連結

- [[claude-code-agents-md-loading-rules]] — 🔴 Claude Code 2.1.277 起的 AGENTS.md 判定表與兩個靜默失效（本條目 C 層首項的修訂依據）
- [[claude-md-guide]] — 🔴 200 行的來源歸屬查證：硬截斷屬 `MEMORY.md`、`CLAUDE.md` 的 200 行是成本目標值而非遵守率門檻
- [[claude-code-context-management]] — Context 管控策略與 AGENTS.md 整合
- [[claude-code-file-management]] — 哪些 Claude Code 設定檔該 commit
- [[addyosmani-engineering-skills]] — Addy Osmani 的工程師 Skill 套件（含 AGENTS.md 結構）

## 補充資料

來源：知乎文章《AGENTS.md 真的对 AI Coding 有用吗？》（2026-02-22），論文《Evaluating AGENTS.md》的詳細解析

### 研究補充細節

**測試條件（更完整版本）**

- **SWE-Bench Lite**：300 個 Python repo-level 任務，原本沒有開發者寫的 context file，用來評測「自動生成 context file」效果
- **AGENTBENCH**：12 個帶開發者自撰 context file 的 Python 倉庫，從真實 PR/issue 構造 138 個實例（bugfix + feature）
- **三種對照設置**：NONE（無 context file）/ LLM（自動生成）/ HUMAN（開發者手寫，僅 AGENTBENCH）
- **四套測試組合**：Claude Code + Sonnet-4.5、Codex + GPT-5.2/5.1 Mini、Qwen Code + Qwen3-30B-Coder

**量化的「更貴」**

- LLM context file 讓平均步驟增加 2.45-3.92 個/任務
- Claude Opus 4.6 輸入超過 200K Token 時計費單價直接翻倍
- GPT-5.2 的平均推理 Token 增加 22%，GPT-5.1 Mini 增加 14%

**為什麼 LLM 生成的版本會讓 Agent「謹慎/折騰」但不「聰明/精準」**

三個機制：
1. Context file 讓 Agent 更頻繁跑測試、grep/read/write 更多、更常使用 repo 特定工具——聽話但被加了不必要的工作
2. 「倉庫 overview」並沒有讓 Agent 更快找到 PR 真正修改過的檔案（甚至更慢）
3. Context file 讓推理 Token 上漲，說明任務對模型而言「變難了」

**例外：文件本來很差的情況**

文件缺失的專案，LLM 生成的 context file 反而有用——補上「怎麼裝依賴、怎麼跑測試、目錄干嘛的」這類基礎資訊。類比：20 分到 70 分 vs 70 分到 90 分的差異。

### AGENTS.md 最值得寫的內容

> 不可從代碼直接推斷、但會導致反覆踩坑的東西。

具體範例：
- 只能跑 `uv` 而不是 `pip`，以及原因
- 必須用某個腳本啟動集成環境，具體哪些 env var
- 關鍵約束（兼容性矩陣、部署目標、性能紅線、安全規則）

**最有效的結構**：常見任務 → 對應入口文件；常見 bug 類型 → 排查路徑（日誌在哪、feature flag 在哪）

**小 tip**：「不要做 xxx」的效果很一般，在 AI 上更建議使用正面引導或結合 Pre-commit Hooks 進行硬性攔截。

## 補充資料（來源：codelove.tw 譯《一份 AGENTS.md，讓程式碼規範率 60%→95%》，juejin 原文，2026-05-17）

前端實戰角度的「怎麼寫」教學，與本條目主論點（不可推斷的隱性知識密度）一致，補上量化效果數據與六章節可複製範本。判定與 agents-md-guide 同角度高度重疊，故併入補充而非另建條目。

### 效果數據（3 週實戰、50+ 次 AI 程式碼生成人工抽檢）
| 指標 | 無 AGENTS.md | 有 AGENTS.md | 變化 |
|------|------|------|------|
| 規範遵循率 | ~60% | **95%+** | ↑35% |
| 一次通過率 | ~40% | **80%+** | ↑40% |
| 每次手動修正時間 | ~8 分 | ~1 分 | ↓87.5% |
| 新人上手時間 | 3-5 天 | 1 天 | ↓70%+ |

> 附帶效益：AGENTS.md 也是給新人看的「專案文件超級濃縮版」，5 分鐘掌握技術棧/目錄/規範/禁止事項。

### 六章節寫法（30-100 行，5 分鐘可複製）
1. **技術棧聲明（必填）**：用**列表**而非段落（AI 解析列表準確率更高）、寫明**版本號**（React 18 vs 19 寫法不同）、寫明**套件管理器**（否則 AI 給 `npm install` 而非 `yarn add`）
2. **目錄結構（必填）**：每個目錄加註解、只列到**一級目錄**（太深 AI 反而困惑）、寫清楚命名規範
3. **編碼規範（必填）**：越具體越好（寫「用 interface 不用 type」而非「遵循最佳實踐」）、**給正確用法範例**（AI 讀範例比讀規則更準）
4. **建置命令（建議）**：`yarn serve` / `yarn build` / `yarn lint`（AI 除錯時需執行命令）
5. **Never 規則（最重要）**：告訴 AI「絕對不能做什麼」比「應該怎麼做」更重要——AI 犯錯成本遠高於人類（會認真把自動生成目錄改了導致建置炸掉）
6. **約定與協作（團隊建議）**：Commit 規範（`type(scope): description`）、多 Agent 並發（禁 `git stash`、禁切分支、只 commit 自己改的檔）

### 三個進階技巧
- **DESIGN.md 聯動**：AGENTS.md 管「程式碼怎麼寫」、DESIGN.md 管「UI 長什麼樣」（品牌色/文字色/間距系統），避免 AI 用 `color: blue` 硬編碼
- **AGENTS.local.md 個人偏好**：加入 `.gitignore`，寫回覆語言/註解語言/元件寫法偏好，AI 同時讀專案規範 + 個人偏好
- **Never 規則演進日誌**：維護「日期 + 新增規則 + 原因」的變更記錄——每條 Never 規則背後都是一個真實踩坑故事（「AI 改了 src/.umi/ 導致建置報錯」「AI 用 #333 硬編碼切主題全崩」）

> 已驗證識別工具：Cursor / Claude / GitHub Copilot / Windsurf。建議長度 60-100 行、不超過 200 行；最小可用版 30 行（技術棧+目錄+3 條 Never）。

## 補充資料（來源：掘金 周末程序猿〈AGENTS.md 深度解析：为 AI 代理而生的 README〉，2026-06-07）

本篇的六共性、三檔邊界（Always/Ask first/Never）、Monorepo 嵌套發現、prompt injection、CLAUDE.md/GEMINI.md 標準之爭、符號連結變通等，與既有補充高度重疊不再重複。**唯一新料是兩個「定位」框架**，補足本條目缺的「AGENTS.md 到底在系統裡處於哪個位置」的心智模型：

### 價值的四層遞進（從淺到深）

1. **統一入口，消除碎片**：把 N 份私有規則檔（`.cursorrules` / `CLAUDE.md` / `GEMINI.md`…）收斂成一份，換工具不必重教 AI
2. **實用的上下文管理**：給提示詞降噪，把有限 token 用在最相關資訊，等於配一份「專案長期記憶/操作手冊」
3. **倒逼工程文化（Forcing Function）**：AI 會**立即讀取並執行**，表現好壞直接反映指令品質——傳統 `CONTRIBUTING.md` 寫爛沒人立刻報錯，AGENTS.md 卻是不知疲倦、絕對客觀的「文檔消費者」，反過來推動團隊把建置/測試流程標準化、文件與程式碼同步
4. **代理技術棧的「知識與上下文層」**：見下

### 技術棧三層定位（心智 / 神經系統 / 身體）

| 層 | 角色 | 對應組件 |
|---|---|---|
| 心智（推理與知識）| 提供先驗知識與行為準則 | **AGENTS.md ＝ 專案長期記憶/操作手冊**；LLM ＝ 推理引擎 |
| 神經系統（編排）| 協調推理/感知/行動的循環 | LangChain / LangGraph / AutoGen / CrewAI |
| 身體（行動與感知）| 對外部世界產生實際影響 | Function Calling / Tool Calling / MCP |

關鍵洞見：AGENTS.md 既不是框架也不是執行引擎，而是**靜態、聲明式的上下文**——在編排迴圈開始「之前」預先設定好 agent 的心智。這決定了控制、邏輯與安全措施該放在技術棧的哪一層，也呼應本條目主論點（只寫不可推斷的隱性知識）。作者也提出一個前瞻判斷：AGENTS.md 很可能是**過渡性/腳手架技術**，為當代 LLM 上下文窗口與推理侷限而生；未來模型若能直接理解整個組織良好的程式庫，專門指令檔或許不再必要，但它所倡導的原則（標準化/關注點分離/明確指令）正為下一代協作（PLAN.md / ARCHITECTURE.md / 規範驅動 / A2A）鋪路。

## 補充資料（來源：〈GPT-6 Astra 太耗 Token？4 個設定讓 Codex 跑得更快、更省〉，tools.wingzero.tw，2026-09-06，https://tools.wingzero.tw/articles/4052）

本條目主論點是「價值不在長度，而在不可推斷的隱性知識密度」。該文從**載入成本**這一側給出同一結論的操作面：
AGENTS.md／SKILL.md 若寫得太長、規則重複、範例過長、觸發條件過於寬鬆，Agent 每次工作都會載入大量用不到的內容。

🔑 **核心原則不是把文件「砍短」，而是讓工具「只在需要時讀取需要的資訊」。** 這把「短」與「精」分開——
同樣 200 行，一份是常駐全載，一份是 30 行主文件加按需引用，兩者的實際 context 成本差距很大。

### 五招精簡（適用 AGENTS.md 與 Skill 描述）

1. 合併重複規則
2. 精簡 Skill 的**觸發描述**（description 才是每次都載入的部分）
3. 收窄容易誤觸發的觸發條件——誤觸發等於整份 Skill 白載入一次
4. 把長篇範例、模板與特殊流程拆到**獨立參考文件**，主文件只留清楚的「按需讀取」入口
5. 記憶／索引檢索改成**先定位真正相關的條目，再回頭讀取少量必要原文**，不要預設完整讀取整份索引

（這與本條目原有的「子目錄嵌套 AGENTS.md 實現按需加載」是同一件事的兩個層次：一個靠目錄範圍分割，一個靠引用分層。）

### 精簡時的三條防線

- **不得為省 Token 而降低任務品質**：必要功能、使用者明確要求的交付數量、權限與操作邊界、驗收標準、
  失敗時的停止條件，一律必須保留；若精簡會改變預設工具、輸出數量或其他既有行為，要明確說明
- **先備份再改，改完驗證引用關係**：驗證語法 → 驗證文件之間的引用仍成立 → 驗證重要流程仍可運作 →
  重新讀取實際來源檔案確認結果（並保留既有軟連結與必要 metadata，如 `ln -s CLAUDE.md AGENTS.md`）
- ⚠️ **誠實度**：沒有實際 Token 用量前後對照時，**不得把「字元數下降比例」宣稱為「Token 節省比例」**

工具側的對應設定（Codex 的 `config.toml` 與智慧程度／速度）見 [[codex-onboarding-seven-layer-config]] 的
「Token 效率」補充——同一篇來源拆兩處：本節收跨工具通用的文件設計原則，該條目收 Codex 特定的執行預設。

## 補充資料｜skill 清單的量化預算，與 Astra 世代的「寫準而非寫少」（2026-09-10 併入）

來源：〈GPT-6 Astra 怎麼用？Codex Skill 優化 5 步驟，AGENTS.md 也該大掃除〉，李先泰，數位時代，
2026-09-07，<https://www.bnext.com.tw/article/92145/gpt-6-astra-skill-cleanup-less-is-more>
（整理自 Codex 團隊 Eric Provencher 的 X 長文與 OpenAI Codex skills 文件）

上一則補充給的是**方法**（五招精簡），本節補的是**量化約束與官方背書**——
本條目 T 層主張「價值不在長度，而在不可推斷的隱性知識密度」，過去缺一個可據以判斷「多長算太長」的硬數字。

### 🔑 硬預算：初始 skill 清單最多佔 context 的 2%，context 大小未知時以 8,000 字元為上限

依 OpenAI Codex 文件，開工時載入的**初始 skill 清單**（每個 skill 的名稱、描述與檔案路徑）受此上限管制。
超出後的降級行為是：**先把描述截短，再不夠就直接省略部分 skill 並顯示警告**。

這把「寫太長」的代價從抽象變具體——超出預算時模型看到的是**一排被截短的殘缺說明**，
於是更難判斷該挑哪一個；描述之間若互相矛盾、或寫得太有「選我」的味道，還會載入一堆對任務沒幫助的指令。

📌 **邊界**：這個預算**只管初始清單**。模型一旦選中某個 skill，仍會讀完整份 `SKILL.md`——
所以「描述要極短」與「本文可以較長」是兩套預算，不能混為一談。

### 官方自己先做了減法：skill-creator 從 416 行砍到 229 行

依 openai/codex 更新紀錄，OpenAI 於 2026-08-13 把內建的 skill-creator（教 Codex 怎麼寫 skill 的官方範本）
**416 行砍到 229 行**，開頭第一條原則改成「**假設 Codex 已經很有能力，只寫會改變它決策的資訊**」。
這句話與本條目 T 層是同一主張的官方版措辭，可直接引用。

### 官方三層漸進式揭露：名稱與描述 → `SKILL.md` 本文 → 支援檔案

新版 skill-creator 把 skill 資訊分成三層，**越後面的層級越晚才讀**。對應到寫法是
「路由器 ＋ references」：主檔只留共同原則與「什麼情況去讀哪份附件」的路由，細節放 `references/` 或 `scripts/`。

```
cloud-deploy/
|-- SKILL.md          只放選擇條件：用 AWS 讀 aws.md，用 GCP 讀 gcp.md
`-- references/
    |-- aws.md
    |-- gcp.md
    `-- azure.md
```

⚠️ **簡單且自成一體的 skill 不必硬拆**，一份 `SKILL.md` 就夠——拆分本身也有維護成本。
（與上一則「拆到獨立參考文件」是同一件事，本節補的是**官方的三層命名與不必拆的但書**。）

### 兩條可直接套用到 AGENTS.md 條目層級的刪法

- **「每次編輯前先讀完 A、B、C」改成「哪份文件管哪件事」**：
  `處理服務邊界時參考 architecture.md，變更 schema 時參考 database.md，準備部署時參考 deployment.md`。
  改一個錯字也要讀完整份架構文件，只是在燒 context。同理，「一律要跑測試」這種催促舊模型的指令，
  在新世代模型上可能導致不必要的測試。
- **禁令改成授權，並開工前定義完成**：為舊模型加的強硬「先問再做」，新世代模型會把邊界看得很重，
  可能在你希望它繼續的地方停下來。與其堆禁令，不如寫明「哪個工作流程是安全的」加上完成條件。
  這與本條目原有的「明確 NEVER 區塊」不衝突——NEVER 針對**AI 實際犯過的錯**，
  不該用來預防性地把整類正常操作也擋掉。

### ⚠️ 但書：重點是「寫準」，不只是「寫少」

Provencher 自己保留兩點，都直接限縮上述刪法的適用範圍：

1. **repo 裡的 skill 會被同事的 agent 讀到**，對方可能用別的（較舊的）模型——
   對新模型剛好的指令，對它們可能不夠。跨模型共用的 repo 不宜照著最新模型的能力砍到底。
2. **新世代模型對 skill 檔內的指令更敏感**，寫得模糊或互相衝突會讓它**提前停工**。
   所以砍掉細節之後，留下的每一句都必須更精確——刪錯的成本比留著冗詞高。

→ 建議順序是「**先刪描述，再拆結構**」：先檢視過時或重複的限制，再把完成條件寫清楚，剩下交給模型判斷。
