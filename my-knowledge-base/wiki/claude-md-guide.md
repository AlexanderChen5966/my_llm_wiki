---
title: CLAUDE.md 撰寫技巧：讓 AI 真正理解你的專案規則
date: 2026-05-14
tags: [ai/claude, dev/architecture]
source: https://israynotarray.com/ai/20260310/3737444063/
type: article
status: processed
asset_value: 70
---

CLAUDE.md 是給 AI 讀的專案規則文件，寫法決定 AI 是否真正照做。核心原則：避免抽象指令（「請好好寫」），改成可驗證的具體規則（檔案命名格式、commit 格式、技術選型原因）。

## T（核心論點）
AI 不遵守規則，90% 是規則文件本身寫得不夠好——太模糊、太冗長、或缺乏「為什麼」。具體可驗證的規則 > 抽象的最佳實踐說教。

## B（適用場景）
**適用：** 剛開始寫 CLAUDE.md；規則寫了但 AI 還是不聽話的情況
**不適用：** 已有完善 CLAUDE.md 的成熟專案

## R（8 個撰寫技巧）
各家工具的規則文件：
- Claude Code：`CLAUDE.md`（專案根目錄）
- Cursor：`.cursor/rules/*.mdc`
- GitHub Copilot：`.github/copilot-instructions.md`

**核心技巧**：
1. **具體可驗證**：`commit 訊息格式：type(scope): description` 而非「注意 commit 格式」
2. **附上理由**：`使用 App Router（Pages Router 正在淘汰）` 而非只寫「使用 App Router」
3. **明確 NEVER 區塊**：根據 AI 實際犯過的錯誤逐步補上禁止事項
4. **控制長度 200 行內**：只放 AI 無法從程式碼推斷的東西
5. **路徑限定規則**：`.claude/rules/` 加 `paths` 屬性，避免不相關規則干擾
6. **提供 2-3 個程式碼範例**：用真實程式碼，不用通用範例
7. **一鍵啟動命令**：`npm run validate`（typecheck + lint + test）
8. **把規則當活文件**：AI 犯錯就補 NEVER，持續演進

## C（關鍵行動）
- [ ] 個人偏好（語言、風格）寫在 `~/.claude/CLAUDE.md`，不改專案的
- [ ] NEVER 區塊從第一個 AI 犯的錯開始寫，空白比亂寫好
- [ ] 控制在 200 行以內，剩下靠路徑限定規則分層載入

## 相關連結

- [[synthesis/synthesis-claude-md]] — CLAUDE.md 方法論精粹；🔴 本篇的 200 行查證是該精粹 2026-09-03 重跑時 E 層翻轉的依據

- [[claude-5-context-engineering-rules]] — 🔴 **官方一手來源**：200 行為官方目標值、且「不是超過就失效」；本篇上方的來源歸屬更正即以此為準
- [[apple-claude-md-teardown]] — Apple 官方 91 行實例與「不寫會怎樣」測試；並修正「可推導就刪」的判準
- [[synthesis-claude-md]] — CLAUDE.md 撰寫方法論知識精粹（本主題 4 篇整合）
- [[agents-md-guide]] — AGENTS.md 跨工具通用版（含 5 個設計原則）
- [[claude-code-file-management]] — 哪些設定檔該 commit
- [[claude-code-context-management]] — 長 CLAUDE.md 消耗 Context 的問題
- [[claude-code-five-layer-architecture]] — CLAUDE.md 是五層架構的第一層（記憶層）

## 補充資料（來源：Claude Code 之父分享的內部工作流：一份 CLAUDE.md 讓你晉升 10 倍工程師）

### Boris Cherny 的 CLAUDE.md 六大核心引擎

這份設定來自 Claude Code 創作者 Boris Cherny 在 X 上分享的 Anthropic 內部最佳實踐：

**1. 工作流程編排（Workflow Orchestration）**

- 任何「非平凡任務」（3 步以上或架構決策）都先進 Plan Mode
- 出問題立刻停下重新規劃，不要繼續硬推
- 用 Plan Mode 做驗證步驟，不只是建構階段

**2. 子代理策略（Subagent Strategy）**

- 大量使用 Subagents 保持主 context window 乾淨
- 把研究、探索、並行分析外包給 Subagents
- 複雜問題就丟更多算力（via Subagents）
- 每個 Subagent 只做一件事（One task per subagent）

**3. 自我完善迴圈（Self-Improvement Loop）**

- 任何被用戶糾正後：更新 `tasks/lessons.md`，記錄這個模式
- 為自己寫規則，防止重犯相同錯誤
- 每次 session 開始時回顧 lessons

**4. 完成前的驗證（Verification Before Done）**

- 永遠不把任務標記為完成，除非能證明它有效
- 自問：「Staff engineer 會批准這個嗎？」
- 跑測試、查 log、展示正確性

**5. 要求優雅（Demand Elegance）**

- 非平凡的改動：先問「有沒有更優雅的方式？」
- 感覺很 hacky 時：「知道我現在所知道的一切，實作優雅解法」
- 簡單明顯的修正就跳過，不過度工程化

**6. 自主修復 Bug（Autonomous Bug Fixing）**

- 給 Bug report 就直接修，不要求手把手引導
- 指向 log、錯誤、失敗的測試——然後解決它們
- 不需要用戶切換 context，去修 CI 失敗不需要被告知怎麼做

### 任務管理流程（Task Management）

```
1. Plan First：把計劃寫到 tasks/todo.md，用 checkable items
2. Verify Plan：開始實作前先確認計劃
3. Track Progress：執行時標記完成的項目
4. Explain Changes：每一步提供高層次摘要
5. Document Results：在 tasks/todo.md 加上 review 段落
6. Capture Lessons：被糾正後更新 tasks/lessons.md
```

### 核心原則（Core Principles）

- **Simplicity First**：讓每個改動盡可能簡單，影響最少的程式碼
- **No Laziness**：找根本原因，沒有臨時修復，Senior developer 標準
- **Minimal Impact**：改動只觸碰必要的部分，避免引入 bug

### 複利效應（Compounding System）

這份 CLAUDE.md 最大的價值：每次被糾正都把模式寫進 `tasks/lessons.md`，系統從你的回饋中學習。隨著專案推進，Claude 犯相同錯誤的機率呈指數下降——越用越像懂你習慣的專屬資深工程師。

## 補充資料（來源：CLAUDE.md 與 Rules for AI 撰寫技巧：讓 AI 更懂你的專案.md）

### 三種常見錯誤寫法

**1. 寫得太模糊**：「請寫好的程式碼」「使用最佳實踐」「注意效能」——AI 無法從中得到具體指引

**2. 寫得太冗長**：把專案歷史、選型原因等背景故事全塞進去——浪費 Token，稀釋真正重要的資訊

**3. 規則互相矛盾**：「所有函式都要寫 JSDoc 註解」+「程式碼應該要能自我解釋，不需要多餘的註解」——AI 會隨機選擇，導致風格不一致

### 跨工具通用性

雖然各工具規則文件名稱不同（Claude Code：`CLAUDE.md`；Cursor：`.cursor/rules/*.mdc`；GitHub Copilot：`.github/copilot-instructions.md`），但撰寫技巧完全通用。核心原則：**可驗證的具體規則 > 抽象的最佳實踐說教**。

---

## 補充資料（來源：Claude 寫程式瘋狂犯錯裝傻？改造 Andrej Karpathy 的12 條規則幫你把錯誤率從 41% 砍到 3％.md）

### Karpathy 4 條規則的由來與爆紅背景

2026 年 1 月，Andrej Karpathy 在 X 上抱怨 Claude 寫程式的三個問題：
1. 遇到不確定情境時不問，自行假設繼續執行
2. 傾向以複雜架構解決簡單問題
3. 修改指定程式碼時順手「整理」周邊不相關的程式碼

Forrest Chang 將這三個問題整理成 4 條行為規則放進 CLAUDE.md 發到 GitHub：第一天 5,828 顆星，兩週 60,000 星，目前超過 120,000 星——2026 年成長最快的單一檔案開源專案。

**4 條基礎規則**：
1. **編碼前先思考**：說明假設，暴露權衡點，在猜測之前先提問
2. **簡單優先**：用最少程式碼解決問題，不加想象中的功能
3. **外科手術式修改**：只改必須改的部分，不順手「最佳化」相鄰程式碼
4. **以目標為導向執行**：定義成功標準，讓 Claude 自己迭代，不要手把手指導

### 原版 4 條規則的 4 個盲點（2026 年 5 月才出現的新問題）

| 盲點 | 症狀 | 新增規則 |
|------|------|---------|
| 無 Token 預算 | 90 分鐘 debug 會話，模型圍繞同一段錯誤反覆迭代 | 單任務 4,000 tokens；單 Session 30,000 tokens；接近上限就總結重開 |
| 無邊界判斷 | 模型決定 API 是否重試（本該由確定性程式碼處理）| 明確區分「Claude 負責的」vs「確定性程式碼負責的」|
| 程式碼衝突時試圖平均 | async/await + 全域錯誤邊界混用，錯誤被吞掉兩次 | 選一種模式，說明理由，標記另一種需要清理 |
| 寫了會通過的廢測試 | 把數值寫死讓測試亮綠燈，掩蓋真實邏輯錯誤 | 每個測試必須編碼「為什麼這個行為重要」，而非只測「它做了什麼」|

### 12 條規則完整列表

原版 Karpathy 4 條 + 新增 8 條（針對 Agent 協作、多步驟任務、大型程式庫）：

1. 編碼前先思考（Karpathy）
2. 簡單優先（Karpathy）
3. 外科手術式修改（Karpathy）
4. 以目標為導向執行（Karpathy）
5. 明確 AI 職責邊界（不讓 AI 做確定性程式碼能做的事）
6. Token 預算約束（單任務/單 Session 上限）
7. 程式碼衝突時選擇一種，不試圖平均
8. 修改前先讀相關程式碼的匯出內容和呼叫方
9. 測試必須編碼業務邏輯的「為什麼」
10. 原型開發模式 vs 正式環境模式（靈活性 vs 嚴格性）
11. 多步驟任務必須有 Checkpoint 計畫
12. Agent 衝突時的仲裁規則

實測結果：30 個程式庫、6 週測試，錯誤率從 41% → 11%（4 條）→ 3%（12 條）。

---

## 補充資料（來源：CLAUDE.md這樣寫才對！12條規則一次整理，讓Claude Code錯誤率從41%降至3%.md）

### 數位時代整理版重點

本文是 blocktempo 文章的更精簡版，補充一個關鍵數據：

- **原版 4 條（Karpathy/Forrest）**：錯誤率從 41% → 11%
- **擴充到 12 條（Mnimiy）**：錯誤率從 11% → 3%

兩次躍升的本質：
- 第一次（4條）：解決「單次對話中的寫程式錯誤」
- 第二次（12條）：解決「多步驟 Agent 協作與大型專案的系統性問題」

重要提醒：Anthropic 官方文件明確指出 CLAUDE.md 本質上是**建議性的**，Claude 約 80% 的時間遵循它。Karpathy 原版 65 行 4 條規則的精簡設計本身也是一種最佳實踐。

> 🔴 **來源歸屬更正（2026-09-03，兩次健檢連續指認 ＋ 上網查證）**：本段原寫「**一旦超過 200 行，遵守率明顯下降**」並掛在「Anthropic 官方文件明確指出」名下。查證後**這個歸屬與這個因果都不成立**，而且問題比單純誤標來源更大——
>
> **① 200 行的硬限制屬於 `MEMORY.md`，不是 `CLAUDE.md`**
> `MEMORY.md` **只有前 200 行載入 context，超出部分靜默截斷**，且此行為未寫進官方文件（[GitHub issue #25006](https://github.com/anthropics/claude-code/issues/25006)，最終 closed as not planned）。本庫 [[claude-code-auto-memory]] 與 [[claude-code-hooks-memory]] 記的「前 200 行或 25KB」是**這個**限制，屬正確。
> 🔴 而「**Claude 只讀 `CLAUDE.md` 的前 200 行**」被明確列為**社群誤解**——`CLAUDE.md` **不論多長都整份載入**。
>
> **② 官方對 `CLAUDE.md` 的原話是軟性目標，不是門檻**
> 官方措辭為 *"CLAUDE.md files are loaded in full regardless of length, though shorter files produce better adherence"* ＋ *"target under 200 lines per CLAUDE.md file"*。是 target，不是 limit，也沒說「超過就下降」。
>
> **③ 目前唯一的量化實測，結論是「長度無可測量影響」**
> 一項 **1,650 個 Claude Code session** 的實驗（兩個真實 TypeScript codebase、5 個任務、共 16,050 個 function，單一可檢核規則）把檔案長度設為 **25／100／250／500 行**：
>
> | 變項 | 結果 |
> |---|---|
> | 檔案長度 | 🔴 **無可測量差異**（χ²(3)=5.16, p=0.16；Bayesian BF₁₀=0.096，即「沒事發生」比對立假設好約十倍）|
> | 規則位置、檔案架構、內部矛盾 | 同樣**零可測量差異** |
> | **有沒有這個檔案** | 🔑 **無 CLAUDE.md：0/524（0%）→ 有：67.7%** |
> | 寫新程式 vs 編輯既有程式 | 71.3% vs **45.1%** |
> | Session 衰減 | 每多寫一個 function，遵守 odds 約 **−5.6%**（OR=0.944, p=1.08×10⁻⁴⁹）|
>
> ⚠️ **可信度標註**：此研究經 [Alex Dunlop《CLAUDE.md Best Practices: What the Evidence Supports (2026)》](https://www.alexdunlop.com/writing/claude-md-best-practices) 轉述，**本次未能直接檢索到原始研究**，屬二手引用；引用前宜再找一手出處。但它與官方措辭方向一致，而本庫先前引用的 78%→76%→52% 那組數字**同樣無法外部複現**（[[claude-md-optimization-techniques]] 2026-07-27 查證），且對應的是**規則條數**而非行數。
>
> **🔑 對本庫的實質影響（比改一句話更重要）**
> 如果「長度／位置／架構／內部矛盾」都不影響遵守率，那 **67.7% 是天花板，結構優化拉不上去**——本庫收集的多數 `CLAUDE.md` 精簡技巧，真正的效益應改記為**省 context 成本**（這點官方措辭也是這樣講的），而不是「提升遵守率」。
> 真正有槓桿的三處反而是：**① 檔案存在與否**（0% → 67.7%）、**② 編輯既有程式的情境**（45.1%，最弱環節）、**③ 長 session 的衰減**（要靠分段而非改檔案）。
>
> **結論**：引用 200 行時說「**官方目標值，理由是 context 成本**」；不要說「超過就會降低遵守率」；`MEMORY.md` 的 200 行才是真截斷。

## 補充資料（來源：Karpathy 馴服 Claude 的秘密武器「claude.md」全公開！，HKT 實驗室）

### Karpathy 的 CLAUDE.md 設計哲學：負面清單優於正面清單

Karpathy 把 CLAUDE.md 設計為「AI 使用說明書」，核心是避免常見錯誤與過度設計的一組約束規則，傾向「謹慎重於速度」。

**關鍵設計思路**：不是告訴 AI「要做什麼」，而是告訴 AI「不要做什麼」——負面清單（NEVER 區塊）比正面清單更有效，因為 AI 的預設行為往往是好的，需要約束的是它會過度做的事。

這與本條目的核心技巧（明確 NEVER 區塊）一致，也與 [[impeccable-style-ai-coding-guide]] 的設計哲學相同。實際規則內容詳見 GitHub：`andrej-karpathy-skills/CLAUDE.md`。
