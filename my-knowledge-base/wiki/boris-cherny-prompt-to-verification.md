---
title: Boris Cherny 談砍 80% 系統提示：ablation 刪除法、把賭注從指令移到驗證
date: 2026-07-30
tags: [ai/claude, ai/agent, ai/prompt-engineering]
source: https://www.bnext.com.tw/article/91670/claude-code-prompt-reduction-agent-verification
type: article
status: processed
asset_value: 75
---

## T（核心論點）

Claude Code 創造者兼負責人 **Boris Cherny** 在 YC Startup School 2026 對談（2026-07-28 公開）說明砍掉 80% 系統提示的原因與後續方向：**那些指令原本是在修正模型「本來就該會、但當時不會」的行為，Opus 5 已經會了**。刪完之後該投資的不是更好的提示詞，而是**驗證**——「怎麼給 Claude 一個看起來有點太難的任務，再讓它有辦法在過程中自己檢查成果」，他形容驗證是「**大多數人沒做對的那件最重要的事**」。

## B（適用場景）

**適用：**
- 已維護一整套 CLAUDE.md / skills / hooks，想知道該怎麼汰舊
- 想理解「刪 8 成」背後的**方法論**而不只是結論
- 規劃長時間自走 Agent 任務

**不適用 / 注意：**
- **「編碼問題已經解決」他自己收窄過**——只對他做的那類程式成立；深層系統程式庫、分散式系統、像素級介面驗證，Claude 都還會卡住
- 這是對談內容的二手報導，非官方文件
- 產品情境仍需保留部分提示（見下方界線）

## R（四個關鍵內容）

### 一、🔬 ablation 刪除法——「刪 8 成」背後的方法

> **把整段系統提示刪掉，再一行一行加回去，逐行看它的實際影響。**

還有一個**沒寫進官方文件**的做法：Claude Code 有 **simple mode**，開啟後改用極簡系統提示與精簡工具組，**團隊拿它當對照組**，測試那些提示到底有沒有用。

Boris 的說法是，模型在沒有這些提示的狀態下「**其實聰明一點**」。但他劃了界線：**Claude Code 作為給人使用的產品，仍需留一部分提示讓行為符合使用者預期**——所以「模型表現最好」與「產品該長怎樣」是兩件事。

> 💡 這部分**部分回答了** [[claude-5-context-engineering-rules]] 記載的缺口「官方未公開評測方法」——至少揭露了 ablation + simple mode 對照組的做法，雖然仍未公開 benchmark 與樣本數。

### 二、為什麼過去非得把指令寫細（兩個限制）

| 限制 | 內容 |
|---|---|
| **外部：工具形態** | 做 Claude Code 時最強編碼模型是 Sonnet 3.5，市面產品還在做單行自動補完、聊天只能讀不能寫。**模型有能力一次寫完整個檔案，卻沒有產品讓它這麼做**——Claude Code 的起點就是把擋路的框架拿掉 |
| **內部：人的習慣** | 🔴 **最常見的失敗模式來自寫了十幾二十年程式的資深工程師**——他們習慣先把系統設計想清楚、把單元測試鋪好，然後**要求模型完全照自己會做的方式執行**。這套訓練在模型身上失效，因為模型不像系統，比較像一個活的東西，**每一代個性都不一樣**，得花時間認識它再調整 |

> 兩邊拉扯方向相反：**工具越成熟越不需要人補位；但人的經驗越深，越傾向把流程規定死。**

### 三、給一般使用者的具體做法

**汰舊節奏**：
> **每六個月刪掉自己的 `CLAUDE.md`、skills 與 hooks，看看模型會怎麼做。**

**重建提示的三動作**：
1. **先刪掉**
2. **然後照常使用**
3. **只有在模型重複踩同一個坑時，才把那條指令加回去**

（工具面同理：Claude Code 團隊經常下架工具、刪掉框架程式碼；框架裡留下的大部分是**安全、權限、靜態分析與介面**）

### 四、⚠️ 評測集會折舊——投資驗證不是一次性建設

這是他自己講的代價，也是最少人提的一點：

> **評測集並不比框架長壽多少，大概只能活一到三個模型世代。模型進步太快，分數很快就滿分飽和，飽和了就得整組丟掉重做。**

也就是說「投資驗證」**是一筆會持續折舊的資產**，不是建好就一勞永逸。

**長時 Agent 實驗案例**：用 Claude Tag（Slack 裡的 Claude）把 Claude 的 Electron 桌面版改寫成 Swift 原生版，指令大意是「改寫、在 Mac 虛擬機裡跑起來、截圖、**逐像素比對**，沒做完不要停」。對談當時已跑 **14–15 天未停**，估計衍生**數千到數萬個代理**；Claude 還自己開了一個 Slack 頻道，每幾分鐘貼一次進度截圖。

## C（關鍵行動）

- [ ] 用 ablation 心法檢視自己的 CLAUDE.md：**整段刪掉，一行一行加回去**，逐行確認實際影響
- [ ] 建立六個月汰舊節奏——刪掉 CLAUDE.md / skills / hooks 看模型會怎麼做
- [ ] 規則只在「模型**重複**踩同一個坑」時才加回去（不是踩一次就補）
- [ ] 把力氣從雕提示詞移到**設計可自我檢查的驗證機制**
- [ ] 建評測集時心裡有數：**一到三個模型世代就要重做**，別當永久資產規劃
- [ ] 別找「一招見效」的技巧——Boris 明說「不要聽 LinkedIn 網紅，不要看 Twitter」，做法只能是經驗性的：**給一個太難的任務 → 給它能自我檢查的工具 → 看它卡在哪 → 用更好的提示或一個 skill 補上**

## 原始摘要

bnext（李先泰，2026-07-30）報導 Y Combinator Startup School 2026 對談（2026-07-28 公開，主持人 Diana Hu）。Boris Cherny 進 Anthropic 前在 Meta 當了五年首席工程師，著有《Programming TypeScript》。文末留下一個開放問題：**如果每一代模型都要重刪一次規範檔、每一到三代要重做一次評測集，真正會累積下來的資產是什麼？** Boris 的線索是建議 CS 學生別只學電腦科學，還要養出設計判斷、商業判斷與跟使用者對話的能力——**會折舊的是規範檔與評測集，留得住的是判斷該往哪裡優化的那個人**。（bnext 業配與延伸閱讀段落已略）

## 補充：Addy Osmani 的目標＋完成標準＋停止條件模板（2026-11-04 併入）

來源：aiposthub（Philo，2026-11-04）〈Claude 提示詞越長越好？Addy Osmani 建議把重點放回目標與驗收〉（https://www.aiposthub.com/claude-overprompting-goals-verification-addy-osmani/ ），原文為 Addy Osmani 2026-10-29 X 貼文〈You're probably overprompting Claude〉。⚠️ **二手整理**：Addy 原貼為轉述，範例表格為作者依原則自編，非官方測試。

與本條 T（「刪完之後該投資的是驗證」）是同一論點的不同發言人。這一段補的是「驗證」怎麼寫成一次可交代的提示：

| 項目 | 內容 |
|---|---|
| 目標 | 一句話講清楚要達成什麼 |
| 完成標準 | 什麼狀態才算做完（可被檢查的條件） |
| 停止條件 | 何時該停、**不可改動的內容**、**需先詢問才能動的界線** |
| 要求附證據 | 要求模型附上**實際執行的檢查與輸出**，而不是只說「已檢查」 |

- **不再重複叮嚀**：新模型（來源以 Opus 5.5＋Claude Code 為例）不需要「仔細思考、逐步分析」這類重複提醒。
- **需求未明時**：先讓模型訪談、產出規格，再開新對話執行。
- **界線提醒**：`/goal` 由另一個模型依「對話中已呈現的證據」判斷是否達成；Auto mode 管的是**動作審核**，不管**成果是否正確**——兩者不能互相取代，見 [[claude-code-goal-command]]、[[claude-code-auto-mode-permissions]]。
- **刪舊指令**：用「同一任務前後比較」，與本條的 ablation 同精神。
- ⚠️ 來源引述「Claude Code 曾對 Opus 5、Fable 5 移除超過八成系統提示、評測無可測退步」——**限於特定模型與評測範圍，不可類推到其他模型或任務**（本條 T 的「80%」亦同）。

### /effort 建議值並陳（不裁決）

| 出處 | 建議 | 脈絡 |
|---|---|---|
| 本段（Addy／aiposthub 二手） | `/effort medium` 起試 | Claude Code 的 `/effort`，以 Opus 5.5 為例 |
| [[claude-opus-5-prompting-guide]]「補充資料：effort 階梯」 | 起點預設 `high`，再依自己的評測往兩邊調 | Opus 5，官方《Prompting Claude Opus 5》（TechNews 轉述） |
| [[claude-code-model-and-thinking-depth]] | 記載 Opus 5.5 API 預設 effort 由 high 改為 medium | Opus 5.5，API 預設值 |

三者對應的模型版本（Opus 5 與 5.5）與介面（Claude Code `/effort` 與 API 預設）不同，**不能直接視為互相矛盾**；日後以官方現行文件為準再定。

## 相關連結

- [[claude-opus-5-prompting-guide]] — effort 建議值並陳的另一端（起點 high）；官方 8 重點
- [[claude-code-model-and-thinking-depth]] — Claude Code 的 `/model` 與 thinking／effort 兩個旋鈕
- [[claude-5-context-engineering-rules]] — Thariq Shihipar 的官方部落格版（**同一事件的另一視角**：那篇講「新規則是什麼」，本篇講「為什麼刪、怎麼測、刪完往哪走」）
- [[claude-md-optimization-techniques]] — CLAUDE.md 技巧；本篇的「六個月汰舊」「重複踩坑才加回」可直接套用其技巧 6
- [[synthesis/synthesis-claude-md]] — CLAUDE.md 方法論精粹
- [[synthesis/synthesis-agentic-engineering]] — Agentic Engineering 精粹；本篇「賭注從指令移到驗證」強化其「審查能力比寫程式更重要」論點
- [[obligations-as-code-tedium-is-stability]] — 「verifier 必須可判定」與本篇「讓它有辦法自己檢查成果」是同一命題
- [[loop-engineering-6-components]] — 長時自走 Agent 的迴圈設計
- [[anthropic-claude-ai-perf-sprint-metric-first-loop]] — Anthropic 官方實證案例：先證明量尺與用戶感受相關，再讓 AI 跑「只降不升」的最佳化循環——「賭注從指令移到驗證」的生產環境版本（2026-09-29 新增）
- 📎 2026-11-04：[[claude-md-guide]] 補充段 — 「Stop and ask when」5 條具體停止條件（CLAUDE.md 層；本篇 Addy 模板是提示詞層，方向一致）
