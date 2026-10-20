---
title: "【AI Agent 08】Skill 越加越多，要怎麼讓 Agent 只在需要時才載入？ － Skills - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401893"
author:
  - "[[iThome]]"
published:
created: 2026-08-11
description: "前一天我們談 Subagent。 Subagent 的價值，不是單純增加 Agent 數量，而是建立更乾淨的 Context、責任、權限與結果邊界。 今天要處理另一個很常見的問題： Agent 需要很..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 8

1

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 8 篇

## 【AI Agent 08】Skill 越加越多，要怎麼讓 Agent 只在需要時才載入？ － Skills

- 分享至

前一天我們談 Subagent。

Subagent 的價值，不是單純增加 Agent 數量，而是建立更乾淨的 Context、責任、權限與結果邊界。

今天要處理另一個很常見的問題：

> Agent 需要很多知識和操作方法時，要不要全部塞進 System Prompt？

直覺上，好像應該。

如果 Agent 可能需要：

- 寫 SQL
- 部署服務
- 分析 Log
- 操作 Git
- 產生報表
- 回覆 Email
- 處理 Kubernetes
- 執行 Security Review

那我們是不是應該把所有規則、範例、流程和最佳實踐都放進 Prompt？

這樣模型什麼都看得到，看起來也最完整。

但實際上，這很容易變成另一種問題：

> **Context hoarding。**

Agent 把所有可能有用的知識都帶在身上，但真正執行任務時，只需要其中很小一部分。

結果就是：

- System Prompt 越來越長
- Token 成本持續增加
- 重要規則被大量內容稀釋
- 不相關指令互相干擾
- Prompt 更新變得困難
- 模型每一輪都重新讀取根本用不到的內容

Skills 提供的是另一種思路：

> 不要預先載入所有能力細節，而是讓 Agent 先知道「有哪些能力」，需要時再載入對應內容。

這就是 **Progressive Disclosure**

---

## Skill 不是一段 Prompt

最簡單的 Skill，可以理解成一個可被發現與載入的能力模組。

它通常包含：

- 名稱
- 描述
- 什麼情況應該使用
- 執行步驟
- 需要的工具
- 限制
- 範例
- 驗證方式
- 相關資源

例如：

```markdown
Skill: database-migration

Purpose:
安全規劃與執行資料庫 Schema Migration

Use when:
需要新增欄位、修改索引、搬移資料

Contains:
Planning rules
Rollback strategy
Validation checklist
Allowed tools
Approval requirements
```

Agent 一開始不需要看到完整內容。

它只需要知道：

> 系統裡存在一個 database-migration Skill，而且它適合處理這類任務。

當任務真的涉及 Migration，再把詳細內容載入 Context。

這和把所有規則永久放在 System Prompt 裡是完全不同的策略。

---

## Progressive Disclosure 解決什麼問題？

Progressive Disclosure 的核心概念是：

> 先暴露最少資訊，只有在需要時才逐步展開更多細節。

在 Agent 中，可以分成幾層：

```markdown
Level 1
Skill 名稱與一句描述

Level 2
使用條件與能力摘要

Level 3
完整 Skill 指令

Level 4
必要的範例、文件與資源
```

模型一開始只需要 Level 1 或 Level 2。

如果判斷這個 Skill 相關，再載入 Level 3。

真的需要深入細節時，再取得 Level 4。

這樣做的目的不是節省幾個 Token 而已。

它也在控制模型當下的注意力。

---

## 第一層：Discovery

Agent 必須先知道有哪些 Skill 可以使用。

但這不代表要把所有 Skill 的全文放進 Context。

可以只提供一份 Skill Index：

```markdown
database-migration
安全規劃與執行 Schema Migration

incident-response
分析 Production Incident 並建立處理流程

github-pr-review
Review Pull Request 並檢查風險

release-checklist
準備 Release 前的驗證與發布步驟
```

這一層的任務只有一個：

> 讓模型知道有哪些能力存在。

Skill 描述要足夠清楚，讓模型能判斷是否相關。

但不要在 Discovery 階段就塞入完整教學。

---

## 第二層：Selection

知道 Skill 存在之後，下一步是判斷哪一個 Skill 適合目前任務。

這裡有幾種方式。

### 模型自己選

把 Skill Index 提供給模型，讓模型判斷。

優點：

- 彈性高
- 可以處理模糊任務
- 不需要太多固定規則

缺點：

- 可能選錯
- 可能選太多
- 可能忽略真正需要的 Skill

### Rule-based Routing

由程式根據任務類型、工具或關鍵事件選擇。

例如：

```markdown
如果涉及 Production Deployment
→ 載入 release-checklist

如果涉及 Schema Migration
→ 載入 database-migration
```

優點是穩定。

缺點是規則可能變多。

### Hybrid

先用程式處理明確條件，再讓模型選擇模糊部分。

這通常最實用。

確定性的選擇交給程式。

需要語意判斷的部分交給模型。

---

## 第三層：Loading

選到 Skill 後，系統才把內容加入 Context。

這裡最重要的是：

> Loading 應該是可追蹤的事件。

系統最好記錄：

- 哪個 Skill 被載入
- 為什麼被選中
- 在哪一輪載入
- 載入多少內容
- 是否包含額外檔案
- 是否影響 Permission
- Skill 什麼時候失效

因為 Skill 會改變模型接下來看到的規則。

如果 Debug 時不知道 Skill 是否被載入，很難判斷問題來自：

- 模型本身
- System Prompt
- Skill Selection
- Skill 內容
- Tool Runtime

---

## 第四層：Execution

Skill 被載入後，不代表 Agent 就一定要逐字照做。

Skill 更像一組任務專屬的操作知識。

例如一個 Incident Response Skill 可能要求：

1. 先確認影響範圍
2. 不要立刻修改 Production
3. 先收集最近錯誤與部署資訊
4. 建立候選原因
5. 驗證後才提出修正
6. 高風險操作需要 Approval

這些規則會影響 Agent 的執行策略。

但真正的 Tool Permission、Approval 和 Sandbox，仍然應該由 Harness 強制執行。

Skill 可以告訴模型：

> 在部署前要取得批准。

Permission System 則負責保證：

> 沒有批准就真的不能部署。

Skill 是操作知識。

Policy 是執行邊界。

兩者不能混在一起。

---

## Skill 和 Tool 有什麼差別？

這兩個概念很容易混淆。

### Tool

提供一個可以執行的能力。

例如：

- 讀檔
- 搜尋
- 執行 Shell
- 查詢資料庫
- 寄送 Email

### Skill

告訴 Agent 在某類任務中，應該如何組合工具、判斷步驟與驗證結果。

例如：

```markdown
Tool:
run_sql

Skill:
database-migration

Skill 可能會告訴 Agent：
先建立 Migration Plan
檢查資料量
確認 Lock Risk
建立 Rollback
執行 Staging 驗證
取得 Approval
最後才執行 Production Migration
```

Tool 是能力。

Skill 是使用能力的方法。

---

## Skill 和 Subagent 有什麼差別？

昨天我們談 Subagent。

Skill 不是另一個 Agent。

它沒有自己的 Loop。

它也不會自己執行任務。

Skill 只是被載入到目前 Agent Context 的能力模組。

可以這樣區分：

```markdown
Tool
做一個操作

Skill
告訴目前 Agent 如何處理一類問題

Subagent
建立另一個獨立 Loop 處理子問題
```

如果只需要新的操作方法，Skill 通常比 Subagent 更便宜。

如果需要獨立 Context、Budget 與責任，才考慮 Subagent。

---

## Skill 和 Memory 有什麼差別？

Skill 是通用能力。

Memory 是從過去經驗或使用者互動中保留下來的資訊。

例如：

```markdown
Skill
公司標準 Deployment 流程

Memory
這個使用者偏好每次 Deploy 前先看完整 Diff
```

Skill 通常可以被多個使用者、任務重複使用。

Memory 通常與特定使用者、專案或過去執行有關。

兩者都會進入 Context，但來源和生命週期不同。

之後談 Memory 時，我們會再深入。

---

## 為什麼不是把所有 Skill 都載入？

假設系統有 50 個 Skills。

每個 Skill 平均 1,500 Tokens。

如果全部放進 Context：

```markdown
50 × 1,500
=
75,000 Tokens
```

Agent 甚至還沒開始處理使用者任務，就已經消耗大量 Context。

更糟的是，這 50 個 Skill 可能互相包含不同規則。

例如：

```markdown
Debug Skill
優先快速嘗試

Production Incident Skill
先收集證據，不要急著修改

Prototype Skill
可以接受快速實驗

Security Review Skill
所有外部輸入視為不可信
```

每個規則單獨都合理。

全部一起存在時，模型卻必須判斷哪一套現在最重要。

Context 越多，不代表判斷越準。

---

## Context 的成本不只是 Token

Context Cost 至少有四種：

1. Token Cost: 輸入越長，成本越高
2. Latency Cost: 模型處理更多內容需要更多時間
3. Attention Cost: 真正重要的規則更容易被大量不相關內容稀釋
4. Conflict Cost: 不同 Skill、規則與範例可能互相衝突

所以 Context Engineering 的目標不是：

> 把能找到的資訊都塞進去。

而是：

> 讓模型在這一輪只看到做出下一個正確決定需要的資訊。

---

## Skill Index 本身也可能太大

如果 Skill 數量從 10 個成長到 1,000 個，就算每個 Skill 只放一句描述，Index 也會很大。

這時 Discovery 本身也需要分層。

可以加入：

### Category

```markdown
Coding
Data
Security
Communication
Operations
Research
```

先選 Category，再看 Skill。

### Search

根據任務語意搜尋最相關的 Skill。

### Metadata Filter

根據：

- Tool
- Domain
- Permission
- Environment
- Risk Level

縮小候選範圍。

### Usage History

根據過去相似任務中有效的 Skill 進行 Ranking。

Progressive Disclosure 不只是 Skill 內容的載入策略。

連 Skill Discovery 本身也可以逐層縮小。

---

## Skill Selection 也需要 Evaluation

如果模型一直選錯 Skill，系統並不可靠。

可以評估：

- Precision: 被載入的 Skill 中，有多少真的有幫助？
- Recall: 真正需要的 Skill，有多少成功被選到？
- Over-selection: 是否一次載入太多 Skill？
- Under-selection: 是否漏掉必要 Skill？
- Cost per Success: 加入 Skill Selection 後，成功率提升多少？增加多少 Token 與延遲？

這些指標可以用真實任務建立 Dataset。

例如：

```markdown
Task
修正 Kubernetes Deployment 一直 CrashLoopBackOff

Expected Skills
kubernetes-debugging
incident-response

Not Needed
email-writing
database-migration
frontend-review
```

這樣就可以測 Skill Router。

---

## Skill 需要版本管理

Skill 本身也是程式的一部分。

如果 Deployment Skill 更新了流程，可能直接改變 Agent 行為。

所以 Skill 應該有：

- Version
- Changelog
- Owner
- Last Updated
- Compatibility
- Evaluation Result

尤其 Production Skill，不應該偷偷更新而完全沒有 Trace。

當 Agent 行為改變時，你需要知道：

> 是模型更新了，還是 Skill 更新了？

---

## Skill 也需要 Scope

Skill 不應該永遠有效。

例如 Agent 載入 `database-migration` 後，任務後半段可能已經進入報告階段。

如果 Skill 仍然一直存在 Context 裡，它可能繼續影響不相關決策。

所以 Skill 可以有 Scope：

- Turn Scope:只在目前一輪使用。
- Task Scope:整個子任務有效。
- Session Scope:整段 Session 有效。
- Persistent:長期存在，但通常比較少見。

載入之後，也要考慮何時 Evict。

這就是 Context Management 的一部分。

---

## Loading 和 Eviction 必須一起設計

很多系統只考慮：

> 什麼時候載入 Skill？

卻沒有考慮：

> 什麼時候移除？

如果每次找到新的 Skill 都只加不減，Context 最後仍然會膨脹。

所以完整 Lifecycle 應該是：

```markdown
Discover
↓
Select
↓
Load
↓
Use
↓
Evaluate
↓
Keep or Evict
```

例如：

- 子任務完成後移除
- 連續多輪未使用後移除
- Context Budget 不足時優先移除低相關 Skill
- Replan 後重新評估 Skill 是否仍然有效

Progressive Disclosure 不只是延遲載入。

它也包含適時卸載。

---

## Skill Loading 不能偷偷改 Permission

假設某個 Skill 需要使用 Deployment Tool。

這不代表載入 Skill 後就自動得到部署權限。

仍然要經過 Day 4 的 Permission System。

可以把它分成：

```markdown
Skill
宣告需要 deploy capability

Permission System
判斷目前 Agent 是否真的可以 deploy

Approval
判斷這一次 deploy 是否需要人類確認
```

Skill 可以描述需求。

它不能提升權限。

---

## 常見的錯誤設計

### 1\. 把所有知識放進 System Prompt

內容完整，但每一輪都付出全部 Context 成本。

### 2\. Skill 描述太模糊

模型不知道什麼時候應該選。

### 3\. 一次載入太多 Skill

Selection 沒有真正降低 Context。

### 4\. Skill 永遠不 Evict

Progressive Disclosure 最後又變回 Context Hoarding。

### 5\. Skill 包含真正 Permission

操作知識不應該取代系統權限。

### 6\. Skill 內容沒有版本

行為改變後無法追蹤原因。

### 7\. 把一切都做成 Skill

固定 Tool 操作不需要 Skill。

固定 Workflow 也不一定需要模型讀一份 Skill 再決定。

### 8\. 只看 Skill 是否被選中，不評估任務結果

Selection 正確不代表真的改善完成率。

---

## 什麼時候值得做成 Skill？

可以問四個問題。

### 1\. 這是不是可重複的任務知識？

如果只會用一次，不一定值得抽象化。

### 2\. 它是否需要多個步驟或規則？

如果只是單一操作，Tool 可能就夠。

### 3\. 它是否只有特定任務才需要？

如果所有任務都必須遵守，可能應該放在 System Policy 或 Harness。

### 4\. 載入後是否能明顯改善執行？

如果沒有可觀察效果，就只是增加 Context。

---

## 一個簡單的分類

```markdown
所有任務都必須遵守
→ System / Policy

單一固定操作
→ Tool

特定任務才需要的操作知識
→ Skill

需要獨立 Context 與 Loop
→ Subagent

從過去使用者或任務保留的資訊
→ Memory

固定且強制的流程
→ Graph / Workflow
```

這能避免把所有東西都塞進 Prompt，或把所有能力都叫做 Skill。

---

## 今天新增了什麼能力？

Day 7 我們加入 Subagent 與 Delegation Boundary。

今天加入：

- Skill Index
- Skill Discovery
- Skill Selection
- Progressive Disclosure
- Dynamic Loading
- Scope
- Eviction
- Skill Versioning
- Skill Evaluation

因此，Agent 不再需要一開始就知道所有細節。

它只需要先知道：

> 哪些能力存在，以及什麼時候值得載入。

---

## 今天的結論

Skills 的價值不是把 Prompt 拆成很多檔案。

真正的價值是：

> 讓 Agent 在需要時才取得需要的操作知識。

完整流程是：

```markdown
Discover
↓
Select
↓
Load
↓
Use
↓
Evaluate
↓
Evict
```

最重要的原則是：

> Context 不是能力倉庫，而是當下決策的工作空間。

不要因為某些知識可能有用，就讓模型每一輪都重新讀一次。

下一篇會進入 Context Management：

> 當 Context 真的開始變長，我們應該刪掉什麼、保留什麼，又如何避免把最重要的 Observation 一起壓縮掉？

**完整系列與範例收錄於： [https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401890)

[

【AI Agent 07】事情太多做不完，多開幾個 Agent 就會比較快嗎？ － Subagents

](https://ithelp.ithome.com.tw/articles/10401890)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 8 篇

目錄

1. 4
	[【AI Agent 04】要怎麼讓 Agent 能動手做事，又不會把東西弄壞？ － Permission](https://ithelp.ithome.com.tw/articles/10401436)
2. 5
	[【AI Agent 05】每次想多加一道檢查，都要去改 Agent 的主流程嗎？ － Hooks](https://ithelp.ithome.com.tw/articles/10401629)
3. 6
	[【AI Agent 06】Agent 列了一份計畫，計劃如何影響執行流程？ － Planning](https://ithelp.ithome.com.tw/articles/10401557)
4. 7
	[【AI Agent 07】事情太多做不完，多開幾個 Agent 就會比較快嗎？ － Subagents](https://ithelp.ithome.com.tw/articles/10401890)
5. 8
	[【AI Agent 08】Skill 越加越多，要怎麼讓 Agent 只在需要時才載入？ － Skills](https://ithelp.ithome.com.tw/articles/10401893)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/f5ad0407-4493-40f8-ba0c-5a454ad6116e_small.jpg)

[企業資安治理新思維](https://itplus.ithome.com.tw/vod-page/328?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

33 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/812f75d6-3b83-495c-8be8-255e3eb61b7f_small.jpg)

[一勞永逸的工控資安? OT Security - Changed Forever](https://itplus.ithome.com.tw/vod-page/373?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/19/605e4c30-d51b-46ea-bdc2-0968e0c7d662_small.png)

[Greenplum gpfdist - PB 級 ETL 獨門秘器｜歐立威科技](https://itplus.ithome.com.tw/vod-page/671?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

歐立威科技 |

50 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/7/1/a04ab13d-4609-4a2c-bef7-38958b247432_small.jpg)

[一刀殺進生成式AI的浪潮，七進七出語言模型的七路劍法](https://itplus.ithome.com.tw/webinar-page/228?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

38 分