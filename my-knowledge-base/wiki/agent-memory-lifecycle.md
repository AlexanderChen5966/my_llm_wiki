---
title: Agent Memory 四層生命週期：Selection / Extraction / Recall / Consolidation
date: 2026-08-12
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10401896（《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》Day 10）
type: article
status: processed
asset_value: 64
---

同系列 Day 09（Working Context）見 [[agent-context-management]]。本篇把時間跨度拉到**跨 Session**。

## T（核心論點）

Memory 的核心不是 Storage，也不是 Vector Database。

> **好的 Memory 不只是記得更多，而是只讓值得記住的事情繼續影響未來。**

很多 Agent 的第一版 Memory 是「把所有對話存起來 → Embedding → 丟進向量庫 → 搜相似 → 塞回 Prompt」。這確實能做出「好像記得你」的效果，但很快變成 **memory landfill**：什麼都存、什麼都可能被召回。真正的問題從「Agent 記不記得」變成「它記住的是不是值得記的東西」。

**Memory 不是比較大的 Context，它是另一套生命週期**：

```
Context → 現在需要看到什麼？（生命週期短）
Memory  → 未來還值得記得什麼？（跨任務、跨 Session）
```

## B（適用場景）

**適用：**
- 設計任何要「跨對話記得使用者」的系統（含 Claude Code Auto Memory、CLAUDE.md 的分工判斷）
- 已經有 Memory 但覺得「召回一堆沒用的東西」時的診斷框架
- 需要對 Memory 品質做評估（而不是靠 demo 感覺它好像記得）

**不適用 / 注意：**
- 🔴 **Memory ≠ Knowledge Base**：KB 存外部知識（公司文件、API Doc、SOP、Wiki），Memory 存使用者偏好、任務經驗、專案狀態、過去決策、Failure Pattern。**底層都用 Vector Search 不代表是同一套東西**（來源、更新方式、信任程度都不同）
- **Memory ≠ Skill**：Skill 是「遇到某類任務該怎麼做」（可重用 Procedure），Memory 是「過去發生過什麼」（從 Experience 保留的 State）
- **Memory ≠ Context Archive**：Archive 可以完整保存 Session 供 Audit/Debug/Replay/Search，但**Archive 不應自動成為 Memory**

## R（操作步驟）

### 第一層：Selection——這句話值不值得跨 Session 保存？

五個判斷維度：

| 維度 | 問題 | 例子 |
|---|---|---|
| **Durability** | 多久後還有效？ | 「今天下午三點開會」→ 很快過期；「這個 Repo 用 Python 3.12」→ 長期有效 |
| **Reusability** | 未來類似任務會再需要嗎？ | 「這次測試失敗的暫時路徑」→ 不值得；「這個專案的測試指令」→ 會再用 |
| **Specificity** | 夠不夠明確？ | 「使用者喜歡好的回答」→ 沒用；「技術內容先給結論，再解釋架構」→ 可操作 |
| **Confidence** | 是事實還是推測？ | 一次抱怨不能寫成「使用者不喜歡 Kubernetes」 |
| **Sensitivity** | 根本不該長期保存嗎？ | Privacy 與 Policy Boundary，不只是 utility filter |

**Memory Gate**（不是所有訊息都直接進 Store）：

```
新資訊 → 有長期價值？ → 夠具體？ → 允許保存？ → Extraction
```

Gate 可由 Rule／Model／Hybrid 共同完成：明確系統規則用程式判斷，模糊的自然語言偏好交給模型分類。

> 對照：「今天幫我用比較短的格式回答」（一次性）vs「之後所有技術文章都不要放太多術語」（長期偏好）。

### 第二層：Extraction——要記成什麼？

Selection 決定「值不值得記」，Extraction 決定「記成什麼」——**這是兩件事**。

原始對話：
> 我不太喜歡上次那種很學術的寫法，之後 LinkedIn 可以更直接一點，最好開頭先有一個比較反直覺的觀點。

不該整段存下來，該萃取成：
```markdown
Preference:
LinkedIn 技術貼文優先使用直接、反直覺的 Hook，避免過度學術語氣。
```

這樣才精簡、可重用、容易搜尋／更新／刪除。

**Memory Unit 應包含的欄位**：Content／Type（Preference・Project Fact・Decision・Constraint・Procedure・Failure Pattern）／Scope（Global・Project・User・Repository・Task Type）／Confidence／Source／Created At／Last Verified／Expiry。這些 metadata 會直接影響 Recall 與 Consolidation。

> ⚠️ **Raw Conversation 不適合直接當 Memory**：裡面有臨時要求、被推翻的想法、客套話、不完整資訊、模型自己的錯誤推測、只適用當下的 Context。全部索引，下次很容易召回「曾經說過但現在已經不對」的資訊。

### 第三層：Recall——什麼時候叫回來？

**只做 Top-K 向量搜尋會召回「語意相似、實際不相關」的內容**（任務是「寫 LinkedIn 貼文」，卻召回「以前寫過 LinkedIn Recruiter Message」）。

Recall 應該是 Ranking，至少六個訊號：Relevance／Scope／Freshness／Confidence／Priority（核心 Constraint 還是可選偏好）／Conflict（是否與更近期 Memory 衝突）。

```
Candidate Memories → Scope Filter → Freshness Filter
→ Relevance Ranking → Conflict Resolution → Top Memories → Inject
```

**Recall 的目標是「最少但足夠」，不是「越多越完整」**——Memory 一樣有 Attention Cost。塞 20 個 Memory 進去只會讓 Prompt 變長、不相關偏好干擾當前任務、新舊衝突、模型更難排序。

**注入時要分類分區**，讓模型知道這是長期背景而非本輪新產生的 Observation：

```markdown
Relevant memory:
Project facts: Python 3.12 / Tests use pytest
User preferences: Prefer concise technical explanations
Constraints: Never deploy directly to Production
Past failure patterns: Integration tests often fail because of missing env vars
```

**兩種 Recall Failure**：Under-recall（該記得的沒叫回來，每次都重問）／Over-recall（每次寫文章都帶入大量舊專案細節）。評估要同時看兩者。

### 第四層：Consolidation——什麼時候合併、更新或忘掉

Memory Store 會累積重複、衝突、過期、被取代、太細碎、長期沒用的內容。六個動作：**Merge／Update／Supersede／Decay／Expire／Delete**。

```
User prefers concise answers.
User prefers concise technical answers.          → User prefers concise
User prefers concise technical posts with a          technical writing with
strong opening.                                      a strong opening.
```

**Memory Conflict：Scope 比時間更重要**。「這次回答簡短一點」（Scope: Current Task）不該覆蓋「之後技術問題都盡量簡短」（Scope: Persistent）。Conflict Resolution 要看 Scope／Explicitness／Recency／Confidence／Persistence，不能無腦「新的覆蓋舊的」。

### 🔴 Memory 也可能記錯，而錯誤會跨 Session 傳播

模型錯抽成「這個專案使用 PostgreSQL」，但實際已改成 MySQL——之後每次 Recall 都讓模型在錯誤前提下工作。因此高影響 Memory 要可重新驗證：

```markdown
Source: pyproject.toml
Last Verified: 2026-08-07
```

**Write 與 Read 都要 Trace**：哪個 Memory 被 Recall／為什麼／來源／建立時間／是否被更新／哪個被忽略／哪個造成衝突；寫入時記錄原始來源、Selection Result、Extracted Content、Scope、Confidence、Retention Policy。否則 Memory 系統無法 debug。

### Memory Evaluation 的七個指標

Selection Precision／Selection Recall／Recall Precision／Recall Recall／Contradiction Rate／Staleness Rate／Cost per Useful Recall。
> 這些比「有沒有 Vector Database」更能反映 Memory 品質。

### Extraction 的時機

End-of-turn（每輪檢查，高價值但貴）／**End-of-task（任務結束集中抽取，通常更有效率）**／Event-triggered（使用者明確表達偏好、Project Fact 被確認、新 Decision 形成、重要 Failure Pattern 被驗證——最容易控制）。

## C（關鍵行動）

- [ ] 🔴 **Memory 不是命令**：優先序 `Current explicit instruction > Current task constraint > Verified project state > Relevant memory > General preference`——記憶說「使用者通常喜歡詳細回答」，但這次要求一句話，就一句話
- [ ] 第一版只存三類：Stable Preference／Verified Project Fact／Explicit Long-term Constraint
- [ ] 每筆至少帶 Content・Type・Scope・Source・Confidence・Created At
- [ ] Recall 順序固定為 Scope Filter → Relevance Ranking → Freshness → Conflict → Top-K
- [ ] 定期 Consolidate：合併重複、更新過期、移除低信心、標記被取代
- [ ] 高影響 Memory 加 `Source` 與 `Last Verified`，讓它可以被重新驗證
- [ ] 檢查自己的知識庫記憶設定：Memory 的品質取決於「**你敢不敢不存**」

### 八個常見錯誤設計

1. 所有對話進 Vector DB　2. Selection 與 Extraction 混在一起（分不出是「不值得記」還是「抽取失敗」）　3. 只做 Semantic Search　4. Recall 太多　5. 只加不減　6. 新 Memory 永遠覆蓋舊的（沒考慮 Scope）　7. 模型推測直接寫成 Fact　8. Memory 優先於當前使用者指令

## 原始摘要

Memory Lifecycle：`Observed → Selected → Extracted → Stored → Recalled → Revalidated → Merged / Updated / Expired`。和 Context Lifecycle 很像，差別只在時間跨度更長。值得記的六類：Stable Preference、Project Fact、Decision、Long-term Constraint、Proven Procedure、Failure Pattern；不值得記的五類：Ephemeral State、Low-confidence Guess、Duplicate Detail、Expired Information、Sensitive Information。

## 相關連結

- [[external-memory-layer-exploration-tax]] — 記憶屬於模型周圍的系統：信任分級（記憶是報告不是指令）、年齡影響信心不影響存在、探索稅

- [[agent-context-management]] — **同系列 Day 09**：同一任務內的 Working Context 四層；本篇是它的跨 Session 版本，兩篇的「Selection」是同名不同源的兩個機制
- [[context-engineering-stacking]] — Memory 是 Harness 準備 Context 的材料之一；該篇說明 Memory/Skill/CLAUDE.md 如何一起堆進工作桌
- [[claude-code-session-handoff]] — 實務對應：**記憶（長期穩定）vs 交接（當下這一攤）**的分工；本篇的「Durability 判準」正是那條分界的理論版
- [[agentmemory]] — 把跨 session 記憶做成 MCP server 的具體實作
- [[memharness-memory-reconstruction]] — 🔴 **最重要的對照**：加記憶反而變差（ALFWorld 76.4%→70.1%），解法是用之前先**重構**而非回放；本篇的 Recall Ranking 是同一問題的另一種解法
- [[llm-memory-pattern]] — Memory + Reflection 讓 Agent「下次不再犯」
- [[reflection-memory-learning-loop]] — Reflection 與 Memory 的學習迴圈
- [[cognitive-hub]] — 以「人類可讀的活記憶」為核心的代理設計模式（外部化記憶比更強的模型更重要）
- [[claude-code-auto-memory]] — Claude Code 的 Auto Memory vs 全域 CLAUDE.md vs 專案 CLAUDE.md **三分流判斷法，是本篇 Scope 概念的產品化版本**（2026-09-03 自 claude-code-context-management 拆出）
- [[concepts/agent-handoff-memory-patterns]] — 跨 Session 與跨工具狀態傳遞的概念地圖
- [[agent-skills-vs-mcp]] — 同系列 Day 08；本篇「Memory 不是 Skill」的界線在該篇有 Skill 側的完整說明
- [[system-prompt-assembly-pipeline]] — **同系列 Day 11**：本篇 Recall 出來的 Memory 最後怎麼進入模型；該篇把 Memory 放在六層來源的第 5 層，並明確排在 Policy／User Constraint／Task 之後
- [[ideas/githarness-versioned-requirement-memory]] — 📎 2026-10-19 新增：同一任務內「需求版本＋工作狀態」的版本化記憶（arXiv 經轉述、未核原文）；軸線與本篇跨 Session 的取捨不同

## 補充資料：從人工改稿萃取偏好規則——手動版的 Selection／Extraction／Consolidation（2026-09-23 併入）

來源：韋惟珊〈AI怎麼記住你的偏好？把每次修改變成規則，不用再重複下指令〉，數位時代〈一天一AI〉，2026-09-22，<https://www.bnext.com.tw/article/92331/ai-remember-preferences-rules>。本篇是**人在迴圈裡**、用 ChatGPT／Claude Project 手動操作的版本，四層可一一對應：

- **流程**：AI 初稿 → 人直接修改 → 兩版交給 AI 比對 → 把反覆出現的修改萃取成規則，存進 Project 供同類任務參考。
- **Selection**：只萃取未來可重複使用的規則，忽略錯字、人名、數字等一次性修改；「無法確定是長期偏好就不加入」。
- **Extraction**：規則分「語氣／結構／內容」三類，每條寫三欄——觸發情境、不要怎麼寫、應該怎麼寫。範例：「一個段落不要塞太多觀點」「講方法要給讀者可以直接想像的例子」。
- **Consolidation**：編修 3–5 份後重整一次——合併重複、刪除不適用、依出現頻率排序。作者初期什麼都存，規則檔越來越雜、甚至互相打架，正是上文 memory landfill 的手動版實例。
- Every 也有類似做法（每次修改都應變成指令檔）。

對照：[[reflection-memory-learning-loop]]（框架內建的 Reflection 版本）、[[llm-memory-pattern]]（偏好記憶）、[[claude-code-auto-memory]]（由 Claude 自動寫入；本篇是由人觸發萃取）。

## 補充資料：跨平台搬家的記憶檔整理——手動版 Selection／Consolidation（2026-09-29 併入）

來源：數位時代〈換AI不用重教一次！Claude⇄ChatGPT雙向搬家，記憶、對話紀錄、Skills轉移教學一次看〉（2026-09-28，<https://www.bnext.com.tw/article/92350/claude-chatgpt-migration>；參考 Ruben Hassid《How to AI》流程並比對 Anthropic／OpenAI 官方文件）。與上一段「從人工改稿萃取偏好規則」並列，都是人在迴圈裡的手動版記憶管理；本段的情境是**換平台（Claude⇄ChatGPT）時的一次性搬遷**。

- **能搬的與搬不動的**：能搬的是記憶／偏好／Skills／工作設定；**完整對話紀錄無法在另一平台重現**——搬家本質上是一次 Selection＋Consolidation，而不是複製資料庫
- **記憶檔五章節**（【媒體改寫：數位時代依 Ruben Hassid 架構改寫】，請舊平台讀取匯出的對話後產出一份 Markdown）：
  1. 核心背景（身分、工作、團隊、長期目標）
  2. 回應指令（1,500 字元以內、第一人稱，可直接貼進自訂指令）
  3. 長期偏好（寫作風格、格式、禁用詞、明確糾正過的地方）
  4. 進行中的專案（目的、最新狀態與日期、已確定的決策）
  5. 衝突與過時資訊（互相矛盾或可能過期的內容）
- **四條規則**——正好對應本條的 Selection 與 Consolidation 判準：
  - 只把我**明確同意**的內容列為決策，不要把 AI 的建議當成我的決定（排除 Low-confidence Guess）
  - 數字、價格、專案狀態都要**標日期**（替 Expired Information 留判斷依據）
  - 排除密碼、API 金鑰與任何下載連結（Sensitive Information）
  - 有檔案讀不到要在文末說明，不要假裝已讀取
- **匯入時再篩一次**：請新平台「一則一個事實」存進記憶、標為過時或衝突的不存、存完列清單給人檢查
- **規則 vs 事實分流**：OpenAI 建議明確的規則與指示放**自訂指令**、對話中的細節交給**記憶**——等同把 Long-term Constraint 與 Project Fact 放進不同容器
- 操作步驟（匯出路徑、匯入入口）時效性高，見 [[claude-2026-beginner-guide]]、[[chatgpt-skills-usage-guide]] 的 2026-09-29 補充段；截至 2026-09-28 介面

對照：[[ai-memory-cross-agent-handoff]]（開發工具之間的自動交接，本段是消費端 App 的手動搬家）、[[chatgpt-claude-privacy-protection-9-checkpoints]]（匯出連結＝通行密鑰）。

## 補充資料：Devin Dreaming——產品化的背景 Consolidation（📎 2026-10-19 併入）

來源：aiposthub（Philo）〈Devin Dreaming 是什麼？Cognition 用 Agent Memory Repo 整理跨任務記憶〉，2026-10-06，<https://www.aiposthub.com/devin-dreaming-agent-memory-repo/>；一手連結 Cognition 官方文章 <https://cognition.com/agent-memory-repo>。⚠️ 單一媒體轉述；效果依官方影片展示，**未實測**。與前兩段「人在迴圈裡的手動版」不同，本段是**廠商把 Consolidation 做成背景自動流程**的例子。

- **Agent Memory Repo**：記憶存成 **Markdown 檔**、以 **Git 追蹤版本**——可看 diff、追來源、回復先前版本；代理開始工作時讀取、完成後補入（官方說法，經 aiposthub 轉述）
- **Dreaming 四個動作**，在工作之外於背景執行：**合併重複、檢查矛盾、移除過時、找出尚未明說的關聯**（官方說法，經 aiposthub 轉述；效果依官方展示）——幾乎一一對應本條的 Consolidation 層（Merged／Updated／Expired）
- 存檔 ≠ 重新訓練：下次能否改善，仍取決於讀到的記憶是否正確、相關、沒有覆蓋新要求（媒體轉述）
- 🔴 公開 repo 只是**本地試用起點**，不含遠端同步、自動啟動、排程整理等完整產品功能（媒體轉述）；原文**沒有寫 repo 名稱、授權或格式細節**
- 記憶要附**適用環境與證據**，否則「這個套件不好用」會把暫時問題放大成永久偏見（作者觀點）——對應本條 Memory Unit 的 Scope／Last Verified
- 金鑰、客戶資料、一次性對話不寫進共享記憶 repo（作者觀點）——對應 Sensitive Information／Ephemeral State 不值得記

對照：[[devin-two-years-intelligence-saturation]]（同一家公司 Cognition）、[[claude-code-auto-memory]]（Claude Code 的自動記憶寫入）、[[ideas/githarness-versioned-requirement-memory]]（同為「Git 化、可回退的狀態」思路，但 GitHarness 管同一任務內需求變更時選哪個版本，Dreaming 管跨任務記憶的整理與淘汰，機制不同，並列即可）。
