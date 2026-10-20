---
title: "【AI Agent 10】關掉視窗就全部忘光，Agent 要怎麼記得事情？ － Memory - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401896"
author:
  - "[[iThome]]"
published:
created: 2026-08-12
description: "前一天我們談 Context Management。 核心觀念是： Storage 負責不遺失資料，Context 負責讓模型看見現在最重要的資料。 但這還只是在處理同一個任務裡的 Working C..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 10

1

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 10 篇

## 【AI Agent 10】關掉視窗就全部忘光，Agent 要怎麼記得事情？ － Memory

- 分享至

前一天我們談 Context Management。

核心觀念是：

> Storage 負責不遺失資料，Context 負責讓模型看見現在最重要的資料。

但這還只是在處理同一個任務裡的 Working Context。

當 Session 結束之後，下一次使用者回來，Agent 要不要記得：

- 使用者偏好的輸出格式
- 專案目前使用的技術棧
- 某個過去已經確認的決策
- 上次任務失敗的原因
- 某個長期有效的 Constraint
- 曾經被證明有效的操作方式

這時就會進入另一個問題：

> **Memory Management**

很多 Agent 的第一版 Memory 很簡單：

1. 把所有對話存起來
2. 建 Embedding
3. 丟進 Vector Database
4. 下次搜尋相似內容
5. 塞回 Prompt

這確實能做出「好像記得你」的效果。

但它也很容易變成：

> **Memory landfill。**

什麼都存，什麼都可能被召回。

最後真正的問題不再是「Agent 記不記得」，而是：

> 它記住的是不是值得記的東西？

今天我們把 Memory 拆成四個核心機制：

1. Selection
2. Extraction
3. Recall
4. Consolidation

---

## Context 和 Memory 不是同一件事

先把兩個概念分開。

### Context

目前這一輪模型可以直接看到的資訊。

它的生命週期通常很短。

例如：

- 使用者目前的需求
- 最近 Tool Result
- Current Plan
- 最新 Error
- 當前 Skill

### Memory

跨任務、跨 Session 之後，仍然可能有價值的資訊。

例如：

- 使用者長期偏好
- 專案固定架構
- 已確認的決策
- 常見失敗模式
- 過去有效的解法
- 長期 Constraint

所以：

```markdown
Context
回答：
現在需要看到什麼？

Memory
回答：
未來還值得記得什麼？
```

Memory 不是比較大的 Context。

它是另一套生命週期。

---

## 不是每句話都值得永久保存

假設使用者說：

> 今天幫我用比較短的格式回答。

這句話可能只適用這一輪。

但如果使用者說：

> 之後所有技術文章都不要放太多術語，優先用簡單例子。

這可能是一個長期偏好。

再例如：

> 這次先不要部署。

可能只是這個 Task 的 Constraint。

但：

> 這個專案永遠不能直接從 Agent 部署到 Production。

可能是長期 Policy。

所以 Memory 的第一個問題不是：

> 要存到哪裡？

而是：

> 這句資訊值得跨 Session 保存嗎？

這就是 Selection。

---

## 第一層：Selection

Selection 決定什麼資訊值得進入 Memory。

可以從幾個維度判斷。

### 1\. Durability

這個資訊多久後還可能有效？

例如：

```markdown
今天下午三點開會
→ 很快過期

這個 Repository 使用 Python 3.12
→ 可能長期有效
```

### 2\. Reusability

未來類似任務是否會再次需要？

例如：

```markdown
這次測試失敗的 Temporary Path
→ 可能不值得保存

這個專案的測試指令
→ 很可能會再次使用
```

### 3\. Specificity

資訊是否足夠明確？

例如：

> 使用者喜歡好的回答。

沒有什麼用。

但：

> 使用者偏好技術內容先給結論，再解釋架構。

比較可操作。

### 4\. Confidence

這個資訊是事實，還是推測？

Agent 不應該因為一次互動就寫入：

> 使用者不喜歡 Kubernetes。

如果只是一次抱怨，可能只是當下情境。

### 5\. Sensitivity

這個資訊是否根本不應該被長期保存？

Memory Selection 不只是 Utility Filter。

也需要 Privacy 與 Policy Boundary。

---

## 可以設計一個 Memory Gate

不是所有訊息都直接進 Memory Store。

先經過 Memory Gate。

```markdown
新的資訊
↓
是否具有長期價值？
├── No → 不保存
└── Yes
     ↓
是否足夠具體？
├── No → 不保存或等待更多證據
└── Yes
     ↓
是否允許保存？
├── No → 丟棄
└── Yes → Extraction
```

這個 Gate 可以由：

- Rule
- Model
- Hybrid

共同完成。

例如非常明確的系統規則可以直接用程式判斷。

模糊的自然語言偏好，可以讓模型協助分類。

---

## 第二層：Extraction

Selection 決定：

> 值不值得記？

Extraction 決定：

> 到底要記成什麼？

這兩件事不一樣。

假設原始對話是：

> 我不太喜歡上次那種很學術的寫法，之後 LinkedIn 可以更直接一點，最好開頭先有一個比較反直覺的觀點。

不應該直接把整段 Conversation 永久存成 Memory。

更好的 Extraction 是：

```markdown
Preference:
LinkedIn 技術貼文優先使用直接、反直覺的 Hook，
避免過度學術語氣。
```

這樣 Memory 比較：

- 精簡
- 可重用
- 容易搜尋
- 容易更新
- 容易刪除

Extraction 是把 Raw Experience 轉換成可重用的 Memory Unit。

---

## Memory Unit 應該包含什麼？

一個 Memory 不一定只有一句文字。

可以包含：

### Content

真正要記住的內容。

### Type

例如：

- Preference
- Project Fact
- Decision
- Constraint
- Procedure
- Failure Pattern

### Scope

適用在哪裡？

例如：

- Global
- Project
- User
- Repository
- Task Type

### Confidence

有多確定？

### Source

來自哪一次 Interaction？

### Created At

什麼時候建立？

### Last Verified

最後一次確認是什麼時候？

### Expiry

是否會過期？

這些 Metadata 會影響 Recall 與 Consolidation。

---

## Raw Conversation 不適合直接當 Memory

把完整聊天紀錄丟進 Vector Database 很方便。

但 Raw Conversation 有很多雜訊。

例如：

- 臨時要求
- 被推翻的想法
- 客套話
- 不完整資訊
- 模型自己的錯誤推測
- 只適用當下的 Context

如果全部索引，下次 Retrieval 很容易召回：

> 曾經說過，但現在已經不對的資訊。

所以 Memory 不應該只是 Conversation Archive。

Archive 可以存在。

Memory 則應該是從 Archive 中抽出的長期訊號。

---

## 第三層：Recall

有 Memory Store 之後，下一個問題是：

> 什麼時候應該把 Memory 放回 Context？

這就是 Recall。

最簡單的方法是：

1. 取得目前 Query
2. 做 Embedding Search
3. 取 Top-K
4. 加回 Prompt

但這樣很容易召回「語意相似，實際不相關」的內容。

例如目前任務是：

> 幫我寫一篇 LinkedIn 貼文。

Vector Search 可能找回：

> 使用者之前寫過 LinkedIn Recruiter Message。

兩者都跟 LinkedIn 有關。

但任務完全不同。

所以 Recall 不應該只依賴 Semantic Similarity。

---

## Recall 至少需要幾個訊號

- Relevance: 與目前任務是否相關？
- Scope: Memory 是否適用目前 Project、User 或 Task？
- Freshness: 資訊是否過期？
- Confidence: 這個 Memory 是否可靠？
- Priority: 它是核心 Constraint，還是可選偏好？
- Conflict: 是否和更近期 Memory 衝突？

可以把 Recall 想成 Ranking：

```markdown
Candidate Memories
↓
Scope Filter
↓
Freshness Filter
↓
Relevance Ranking
↓
Conflict Resolution
↓
Top Memories
↓
Inject into Context
```

這比單純 Top-K Vector Search 更可靠。

---

## Recall 的目標不是找最多

Memory Retrieval 最常見的錯誤是：

> 多找一些比較安全。

但 Memory 和 Context 一樣有 Attention Cost。

如果每次都塞入 20 個 Memories：

- Prompt 變長
- 不相關偏好干擾當前任務
- 舊資訊和新資訊衝突
- 模型更難判斷優先順序

所以 Recall 最好追求：

> 最少但足夠。

不是：

> 越多越完整。

---

## Memory 應該如何注入 Context？

不要只是把所有 Memory 丟到 Prompt 最後。

最好有明確區域。

例如：

```markdown
Relevant memory:

Project facts:
- Python 3.12
- Tests use pytest

User preferences:
- Prefer concise technical explanations

Constraints:
- Never deploy directly to Production

Past failure patterns:
- Integration tests often fail because of missing env vars
```

分類有助於模型理解：

> 這些不是這一輪新產生的 Observation，而是長期保存的背景資訊。

---

## Recall Failure 有兩種

### Under-recall

該記得的沒有被叫回來。

例如使用者明明有長期格式偏好，系統卻每次都重新問。

### Over-recall

叫回太多不需要的東西。

例如每次寫文章都帶入大量舊專案細節。

兩種都會降低體驗。

所以 Memory Evaluation 不只看：

> 有沒有 Recall 到正確 Memory？

還要看：

> 有沒有避免 Recall 不相關 Memory？

---

## 第四層：Consolidation

Memory 不應該只會一直增加。

隨著時間過去，Memory Store 會出現：

- 重複
- 衝突
- 過期
- 被新資訊取代
- 太細碎
- 長期沒有使用

這時就需要 Consolidation。

例如原本有三筆：

```markdown
User prefers concise answers.

User prefers concise technical answers.

User prefers concise technical posts with a strong opening.
```

可以合併成：

```markdown
User prefers concise technical writing with a strong opening.
```

這樣 Recall 更乾淨。

---

## Consolidation 不只是 Summary

Consolidation 可以做幾件事：

- Merge: 合併重複 Memory。
- Update: 用較新的資訊更新舊 Memory。
- Supersede: 新 Memory 取代舊 Memory。
- Decay: 長期未使用、低信心 Memory 降低權重。
- Expire: 超過有效時間後不再 Recall。
- Delete: 確定錯誤或不應保存的資訊直接移除。

Memory Store 如果沒有 Consolidation，最後會變成另一個 Context Landfill。

---

## Memory Conflict 怎麼處理？

假設舊 Memory 是：

> 使用者偏好詳細回答。

新的 Interaction 是：

> 之後請盡量簡短。

哪一個應該優先？

最簡單的方法是：

> 新的永遠覆蓋舊的。

但也不一定正確。

因為新的要求可能只適用當前任務。

所以 Conflict Resolution 要考慮：

- Scope
- Explicitness
- Recency
- Confidence
- Persistence

例如：

```markdown
"這次回答簡短一點"
Scope: Current Task

"之後技術問題都盡量簡短"
Scope: Persistent
```

Scope 比單純時間更重要。

---

## Memory 也可能記錯

Agent Memory 有一個危險特性：

> 錯誤會跨 Session 傳播。

如果模型錯誤抽取：

> 這個專案使用 PostgreSQL。

但實際上專案已經改成 MySQL。

未來每次 Recall 都可能讓模型在錯誤前提下工作。

所以高影響 Memory 應該可以被重新驗證。

例如 Project Fact 可以有：

```markdown
Source:
pyproject.toml

Last Verified:
2026-08-07
```

下次 Repository 明顯更新後，可以重新確認。

---

## Memory Write 和 Memory Read 都需要 Trace

當 Agent 因為 Memory 做出某個決定時，我們應該能回答：

- 哪個 Memory 被 Recall？
- 為什麼被 Recall？
- 來源是什麼？
- 什麼時候建立？
- 是否曾經被更新？
- 哪個 Memory 被忽略？
- 哪個 Memory 造成衝突？

同樣地，寫入 Memory 時也應記錄：

- 原始來源
- Selection Result
- Extracted Content
- Scope
- Confidence
- Retention Policy

否則 Memory 系統很難 Debug。

---

## Memory 不是 Knowledge Base

Knowledge Base 通常保存的是外部知識。

例如：

- 公司文件
- 產品說明
- API Documentation
- SOP
- Wiki

Memory 通常保存的是：

- 使用者偏好
- 任務經驗
- 專案狀態
- 過去決策
- 個人化資訊
- 過去 Failure Pattern

兩者都可以使用 Retrieval。

但來源、更新方式與信任程度不同。

不要因為底層都使用 Vector Search，就把它們視為同一套東西。

---

## Memory 不是 Skill

Skill 是：

> 遇到某類任務時，應該怎麼做。

Memory 是：

> 過去發生過什麼，未來可能還值得知道。

例如：

```markdown
Skill:
如何寫一篇高品質 LinkedIn 技術貼文

Memory:
使用者偏好 LinkedIn 開頭先提出反直覺觀點
```

Skill 比較像可重用 Procedure。

Memory 比較像從 Experience 中保留的 State。

---

## Memory 不是 Context Archive

Context Archive 可以保存完整 Session。

這對：

- Audit
- Debug
- Replay
- Search

很有價值。

但 Archive 不應該自動成為 Memory。

可以把架構拆成：

```markdown
Raw History
完整保存

Memory Extraction
挑出值得長期保存的部分

Memory Store
保存結構化 Memory

Recall
只取目前相關 Memory
```

這樣才能避免「有保存」和「值得記住」混在一起。

---

## Memory 的生命週期

一筆 Memory 可以經過：

```markdown
Observed
↓
Selected
↓
Extracted
↓
Stored
↓
Recalled
↓
Revalidated
↓
Merged / Updated / Expired
```

這和 Context Lifecycle 很像。

差別是 Memory 的時間跨度更長。

---

## 哪些資訊通常值得 Memory？

常見類型包括：

### Stable Preference

例如：

- 回答風格
- 格式習慣
- 常用工具
- 工作方式

### Project Fact

例如：

- Tech Stack
- Repo Structure
- Build Command
- Deployment Rule

### Decision

例如：

- 已決定使用某個架構
- 已淘汰某個方案

### Long-term Constraint

例如：

- 不允許自動部署 Production
- 某類資料不能外傳

### Proven Procedure

例如：

- 某個專案特定 Debug 方法

### Failure Pattern

例如：

- 某個環境常見錯誤與原因

但每一類都需要 Scope 與有效期限。

---

## 哪些資訊通常不值得 Memory？

例如：

### Ephemeral State

- 今天的暫時錯誤
- 一次性的路徑
- 已完成 Task 的中間輸出

### Low-confidence Guess

- 模型推測但未確認的資訊

### Duplicate Detail

- 已被更完整 Memory 包含

### Expired Information

- 過期 Schedule
- 舊版本設定

### Sensitive Information

- 不應長期保存的資料

Memory 的品質往往取決於：

> 你敢不敢不存。

---

## Memory Evaluation 要測什麼？

Memory 系統不應該只用幾個 Demo 感覺「它好像記得」。

可以分成幾個指標：

- Selection Precision: 寫入的 Memory 中，有多少真的值得保存？
- Selection Recall: 應該保存的重要資訊，有多少沒有漏掉？
- Recall Precision: 被召回的 Memory，有多少真的與目前任務相關？
- Recall Recall: 真正需要的 Memory，有多少成功被召回？
- Contradiction Rate: 舊 Memory 與新事實衝突的頻率。
- Staleness Rate: 被 Recall 的 Memory 中，有多少已經過期？
- Cost per Useful Recall: 每次真正有幫助的 Recall，需要多少額外 Context 與 Retrieval 成本？

這些指標比單純「有沒有 Vector Database」更能反映 Memory 品質。

---

## Memory 是否應該每輪都 Extraction？

不一定。

每輪都跑 Memory Extraction 會增加：

- 模型呼叫
- Token
- Latency
- 重複 Memory

可以採取：

### End-of-turn Extraction

每輪檢查。

適合高價值互動，但成本較高。

### End-of-task Extraction

任務結束後集中抽取。

通常更有效率。

### Event-triggered Extraction

只有出現特定事件才抽取。

例如：

- 使用者明確表達偏好
- Project Fact 被確認
- 新 Decision 形成
- 重要 Failure Pattern 被驗證

Event-triggered 往往比較容易控制。

---

## 不要讓 Memory 自動成為最高優先級

Memory 是背景。

它不應該覆蓋更高優先級資訊。

例如：

```markdown
Memory:
使用者通常喜歡詳細回答

Current User Request:
這次請一句話回答
```

這次應該一句話。

Priority 可以理解成：

```markdown
Current explicit instruction
>
Current task constraint
>
Verified project state
>
Relevant memory
>
General preference
```

Memory 不是命令。

它只是輔助目前決策的背景。

---

## 常見的錯誤設計

### 1\. 所有對話都進 Vector Database

Memory Store 最後變成 Conversation Dump。

### 2\. Selection 和 Extraction 混在一起

不知道是「不值得記」還是「抽取失敗」。

### 3\. 只做 Semantic Search

相似不代表適用。

### 4\. Recall 太多

Memory 重新變成 Context Pollution。

### 5\. Memory 只加不減

重複、衝突與過期資訊越來越多。

### 6\. 新 Memory 永遠覆蓋舊 Memory

沒有考慮 Scope。

### 7\. 模型推測直接寫成 Fact

錯誤會跨 Session 傳播。

### 8\. Memory 優先於目前使用者指令

背景資訊不應該壓過當前明確需求。

---

## 如何設計第一版 Memory？

可以先只保存三類：

1. Stable Preference
2. Verified Project Fact
3. Explicit Long-term Constraint

每一筆至少包含：

- Content
- Type
- Scope
- Source
- Confidence
- Created At

Recall 時先做：

1. Scope Filter
2. Relevance Ranking
3. Freshness Check
4. Conflict Check
5. Top-K 限制

然後定期 Consolidate：

- 合併重複
- 更新過期
- 移除低信心
- 標記被取代內容

這已經比「整段 Conversation 全部 Embedding」可靠很多。

---

## 今天新增了什麼能力？

Day 9 我們管理同一個任務中的 Working Context。

今天加入跨 Session 的 Memory Lifecycle：

- Selection
- Extraction
- Memory Unit
- Recall
- Scope
- Confidence
- Consolidation
- Expiry
- Conflict Resolution
- Memory Evaluation

因此，Agent 不再只是「把過去找回來」。

它開始回答更重要的問題：

> 哪些過去，真的值得影響未來？

---

## 今天的結論

Memory 的核心不是 Storage。

也不是 Vector Database。

真正的 Memory System 至少需要：

- Selection：什麼值得記
- Extraction：要記成什麼
- Recall：什麼時候叫回來
- Consolidation：什麼時候合併、更新或忘掉

最重要的原則是：

> 好的 Memory 不只是記得更多，而是只讓值得記住的事情繼續影響未來。

下一篇會進入 System Prompt Assembly：

> System Prompt 真的應該是一大段固定文字嗎？還是應該根據任務、工具、Permission、Skill 與 Memory 動態組裝？

**完整系列與範例收錄於： [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401895)

[

【AI Agent 09】聊到一半 Agent 開始忘東忘西，上下文該留什麼、丟什麼？ － Context Management

](https://ithelp.ithome.com.tw/articles/10401895)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 10 篇

目錄

1. 6
	[【AI Agent 06】Agent 列了一份計畫，計劃如何影響執行流程？ － Planning](https://ithelp.ithome.com.tw/articles/10401557)
2. 7
	[【AI Agent 07】事情太多做不完，多開幾個 Agent 就會比較快嗎？ － Subagents](https://ithelp.ithome.com.tw/articles/10401890)
3. 8
	[【AI Agent 08】Skill 越加越多，要怎麼讓 Agent 只在需要時才載入？ － Skills](https://ithelp.ithome.com.tw/articles/10401893)
4. 9
	[【AI Agent 09】聊到一半 Agent 開始忘東忘西，上下文該留什麼、丟什麼？ － Context Management](https://ithelp.ithome.com.tw/articles/10401895)
5. 10
	[【AI Agent 10】關掉視窗就全部忘光，Agent 要怎麼記得事情？ － Memory](https://ithelp.ithome.com.tw/articles/10401896)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1786008610_6a745422552c3.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/21/1c9eeb19-694e-4ee4-a53b-004a9e59460a_small.png)

[醫療業導入微服務架構的策略方向](https://itplus.ithome.com.tw/vod-page/684?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣醫院資訊主管會議 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/b76c9aca-d863-4baa-9f9b-7452175a1770_small.png)

[Istio-Powered Observability](https://itplus.ithome.com.tw/vod-page/982?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/ccb12077-d9be-4d5d-88cf-12a232f64877_small.png)

[如何在多點開發和離岸進行 LeSS](https://itplus.ithome.com.tw/vod-page/1102?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

52 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/18/f5688738-7fff-4cb1-8a26-e9aa7c4ccf3d_small.jpg)

[用 Web API 打造文件閱讀器](https://itplus.ithome.com.tw/webinar-page/258?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

38 分