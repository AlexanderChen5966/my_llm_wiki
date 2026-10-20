---
title: "【Day 4】Agent 能使用工具，不代表它應該擁有所有權限 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401436"
author:
  - "[[iThome]]"
published:
created: 2026-08-06
description: "前一天我們拆解了 Tool Calling 和 Tool Runtime。 模型產生 Tool Call，只代表它提出一個行動請求。 真正的 Tool Runtime 還要處理工具查找、參數驗證、執行..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 4

1

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 4 篇

## 【Day 4】Agent 能使用工具，不代表它應該擁有所有權限

- 分享至

前一天我們拆解了 Tool Calling 和 Tool Runtime。

模型產生 Tool Call，只代表它提出一個行動請求。

真正的 Tool Runtime 還要處理工具查找、參數驗證、執行、逾時、錯誤與輸出管理。

但就算工具存在、參數正確、執行方式也沒有問題，仍然不能直接開始執行。

因為還有一個更重要的問題：

> 這個操作被允許嗎？

Agent 能做某件事，不代表它應該被允許做某件事。

讀取公開文件和讀取私人金鑰，都可能使用同一個讀檔工具。

修改測試檔案和刪除整個專案，也可能使用同一組檔案權限。

傳送一封草稿 Email 和真的寄出 Email，對使用者造成的影響完全不同。

因此，Production Agent 必須把能力和權限拆開。

今天要介紹三個核心機制：

1. **Permission**
2. **Approval**
3. **Sandbox**

它們分別回答三個不同問題：

- 這個 Agent 原則上可以做什麼？
- 這一次高風險操作是否需要人類確認？
- 即使操作出錯，影響範圍能否被限制？

**完整系列與程式碼範例收錄於 [GitHub](https://github.com/hardness1020/awesome-agent-architecture)**

---

## Capability 和 Permission 不是同一件事

先區分兩個常被混在一起的概念。

### Capability

Agent 是否具備完成某個行動的能力。

例如：

- 是否有讀檔工具
- 是否能執行 Shell
- 是否能呼叫 Email API
- 是否能寫入資料庫
- 是否能存取網路
- 是否能建立新的雲端資源

### Permission

Agent 在目前任務、使用者和環境中，是否被允許執行這個行動。

例如：

- 可以讀專案目錄，但不能讀取家目錄
- 可以建立 Email 草稿，但不能直接寄出
- 可以修改測試檔案，但不能修改 Production 設定
- 可以查詢資料庫，但不能刪除資料
- 可以呼叫內部 API，但不能對外公開資料

同一個 Capability，可以根據情境得到不同 Permission。

```markdown
有工具
不等於
有權限

有權限
也不等於
這一次不需要確認
```

如果系統只控制工具是否存在，卻沒有控制工具可以對哪些資源做什麼，權限模型仍然非常粗糙。

---

## Prompt 不是安全邊界

很多 Agent 會在 System Prompt 中加入：

> 不要刪除重要檔案。

> 不要存取使用者的敏感資料。

> 在進行危險操作前先詢問使用者。

這些指令有價值。

它們可以幫助模型理解預期行為，降低錯誤決策的機率。

但它們不能取代真正的權限控制。

模型可能：

- 誤解什麼是重要檔案
- 忘記先前規則
- 在 Context 過長時忽略指令
- 因為工具結果而改變判斷
- 被外部內容中的 Prompt Injection 影響
- 在多步任務中錯估某個行動的風險

所以真正的安全規則必須由模型外面的程式碼強制執行。

可以把兩者分成：

```markdown
Prompt
告訴模型應該怎麼做

Permission System
限制模型實際能做什麼
```

一個可靠的系統不只希望 Agent 不會越界。

它還要讓 Agent 即使判斷錯誤，也無法越界。

---

## Permission 應該檢查什麼？

Permission 不只是「允許使用哪個工具」。

完整的 Permission Check 通常要看多個維度。

### 1\. Tool

Agent 是否能使用這個工具？

例如：

- Review Agent 只能讀檔，不能寫檔
- Research Agent 可以搜尋網路，但不能發送訊息
- Draft Agent 可以產生內容，但不能直接發布

### 2\. Resource

Agent 可以對哪些資源執行操作？

例如：

- 只能讀取指定 Workspace
- 只能查詢目前使用者有權限的資料
- 只能修改某個 Git Branch
- 只能操作測試環境

### 3\. Action

同一個工具可能包含不同風險的行動。

例如資料庫工具可能同時支援：

- SELECT
- INSERT
- UPDATE
- DELETE

允許查詢，不代表允許刪除。

### 4\. Scope

Agent 可以一次影響多少內容？

例如：

- 只能修改一個檔案
- 一次最多寄送一封 Email
- 一次最多更新十筆資料
- 一次最多消耗固定預算

### 5\. Identity

目前是誰要求執行？

例如：

- 使用者本人
- 管理員
- 自動排程
- 子 Agent
- 第三方整合

同一個行動在不同身份下，權限可能不同。

### 6\. Context

目前任務的目的是否允許這個操作？

例如：

- 程式碼審查任務不應該修改檔案
- Email 摘要任務不應該寄出信件
- 財務分析任務不應該執行交易

Permission 是一個與情境相關的判斷，而不只是固定的開關。

---

## Allow、Deny 和 Ask

一個實用的 Permission System，通常至少需要三種結果。

### Allow

操作可以直接執行。

例如：

- 讀取 Workspace 內的程式碼
- 執行測試
- 搜尋公開文件
- 建立本地暫存檔

### Deny

操作永遠不允許。

例如：

- 讀取私人金鑰
- 修改系統檔案
- 關閉 Audit Log
- 存取不屬於目前使用者的資料
- 執行明確禁止的指令

### Ask

操作可以執行，但需要人類批准。

例如：

- 刪除檔案
- 發送 Email
- 發布社群貼文
- 進行付款
- 部署到 Production
- 建立公開連結
- 修改大量資料

這三種狀態非常重要，因為不是所有高風險操作都應該直接禁止。

有些操作是任務必要的一部分，只是需要把最後決定交還給人類。

---

## Approval 解決什麼問題？

Approval 是 Agent 的人類確認機制。

它通常出現在：

```markdown
模型選擇行動
↓
系統判斷需要批准
↓
向使用者顯示操作內容
↓
使用者批准或拒絕
↓
系統執行或終止
```

Approval 的目的不是讓使用者確認每一個步驟。

如果 Agent 每讀一個檔案都要詢問一次，系統會變得難以使用。

Approval 應該集中在有明顯副作用、不可逆或高成本的操作。

可以使用幾個問題判斷是否需要批准：

1. 操作是否會改變外部世界？
2. 操作是否不可逆？
3. 操作是否涉及金錢、隱私或公開內容？
4. 操作是否影響大量資源？
5. 操作失敗後是否難以恢復？
6. 使用者是否合理預期自己擁有最後決定權？

如果多個答案是肯定的，通常就值得加入 Approval Gate。

---

## 好的 Approval 必須讓人看得懂

Approval 並不是單純顯示：

> Agent 想使用 `send_email` ，是否允許？

使用者真正需要知道的是：

- 寄給誰
- 主旨是什麼
- 內容是什麼
- 是否包含附件
- 是否會影響其他人
- 是否能撤回
- 為什麼 Agent 想執行

因此，好的 Approval 需要呈現具體副作用。

例如：

```markdown
準備寄出一封 Email

收件者：客戶 A
目的：回覆退款申請
內容：同意退款 120 美元
附件：無

是否批准？
```

Approval 的價值來自可理解性，而不是多一個按鈕。

如果使用者看不懂 Agent 要做什麼，批准就只是形式。

---

## Approval 也需要狀態

當 Agent 暫停等待批准時，任務不能消失。

系統需要保存：

- 哪一個 Tool Call 正在等待
- 原始參數
- 風險摘要
- 誰可以批准
- 批准是否有期限
- 使用者批准、拒絕或修改了什麼
- 恢復後從哪一輪繼續

這表示 Approval 不只是一個 UI 問題。

它也是 Task State 問題。

```markdown
Running
↓
Waiting for Approval
├── Approved → Resume
├── Rejected → Return Observation
└── Expired → Stop or Ask Again
```

如果沒有明確狀態，Agent 可能在重新啟動後重複執行、遺失批准結果，或把舊批准套用到新的操作。

---

## 一次批准應該批准多少？

假設 Agent 說：

> 我需要修改五個檔案並重新執行測試。

批准可以有不同粒度。

### 單一操作批准

每個 Tool Call 都要確認。

優點是控制細。

缺點是使用者很快會疲乏。

### 任務範圍批准

使用者批准 Agent 在某個限定範圍內操作。

例如：

- 可以修改 `tests/` 目錄
- 可以執行非破壞性指令
- 可以在這個 Branch 建立 Commit
- 可以寄信給指定收件者

這種方式通常更實用。

### 長期權限

使用者允許某類操作在未來自動執行。

例如：

- 每天整理公開新聞
- 自動建立 Email 草稿
- 自動更新測試環境

這類權限必須搭配：

- 明確 Scope
- 到期時間
- 可撤銷性
- Audit Log
- 例外處理

批准不是越少越危險，也不是越多越安全。

重點是批准範圍是否清楚。

---

## Sandbox 解決什麼問題？

Permission 和 Approval 都在決定：

> 這個操作可不可以執行？

Sandbox 則在處理另一個問題：

> 即使允許執行，出錯時最多能影響多少？

Sandbox 是隔離的執行環境。

它可以限制：

- 可見的檔案系統
- 可使用的 CPU 與記憶體
- 執行時間
- 網路連線
- 系統呼叫
- 環境變數
- Secret
- 可啟動的程序
- 寫入位置

例如 Coding Agent 可以在一個臨時 Workspace 中修改專案。

即使它刪除所有檔案，也只會影響隔離副本，而不是原始 Repository。

這種設計不是因為我們預期模型一定會犯錯。

而是承認任何複雜系統都可能犯錯，並限制錯誤半徑。

---

## Permission、Approval 和 Sandbox 的差別

| 機制 | 核心問題 |
| --- | --- |
| Permission | 這個操作是否被允許？ |
| Approval | 是否需要人類做最後決定？ |
| Sandbox | 即使執行失敗，影響能否被限制？ |

例如 Agent 想執行一個安裝套件的指令：

- Permission 判斷它能不能執行 Shell
- Approval 判斷是否要先讓使用者確認
- Sandbox 限制它只能修改隔離環境

三者可以同時存在。

```markdown
允許使用工具
不代表
允許所有參數

允許某個操作
不代表
不需要人類批准

獲得人類批准
也不代表
不需要 Sandbox
```

Approval 不能取代 Sandbox。

使用者批准一個看似合理的指令，不代表指令執行後一定安全。

Sandbox 也不能取代 Permission。

隔離環境仍然可能包含敏感資料或昂貴資源。

---

## Least Privilege

Permission System 最常見的原則是 Least Privilege。

也就是：

> Agent 只獲得完成目前任務所需的最小權限。

如果任務只是摘要文件，就不需要寫入權限。

如果任務只是建立草稿，就不需要寄送權限。

如果任務只處理一個 Repository，就不需要讀取整台電腦。

如果子 Agent 只負責搜尋，就不需要繼承父 Agent 的所有工具。

Least Privilege 可以降低：

- 誤操作影響
- Prompt Injection 風險
- 工具被濫用的可能
- Secret 洩漏範圍
- Debug 複雜度

這也代表 Tool Scope 應該根據任務動態建立，而不是永遠把所有工具全部提供給模型。

---

## 子 Agent 不應該自動繼承所有權限

當系統開始使用 Subagent 或 Multi-Agent 時，權限問題會變得更複雜。

父 Agent 可能擁有：

- 讀寫檔案
- Shell
- 網路
- Email
- 部署權限

但它建立的研究子 Agent，可能只需要網路搜尋與讀取資料。

如果子 Agent 自動繼承所有能力，就會放大風險。

更好的方式是：

```markdown
父 Agent
擁有完整任務權限
        ↓
建立子 Agent
        ↓
只下放完成子任務所需的工具與 Scope
```

權限應該被明確委派，而不是默認繼承。

---

## Permission Failure 也是 Observation

當操作被拒絕時，Agent Loop 不一定要直接結束。

系統可以把拒絕原因作為 Observation 交回模型。

例如：

```markdown
操作被拒絕

原因：目前任務只有唯讀權限
可選擇：
1. 改用不需要寫入的方法
2. 向使用者請求提升權限
3. 產生建議但不實際修改
```

模型可以根據這個限制調整策略。

這比只回傳「Permission denied」更有用。

但有一點不能讓模型決定：

> 模型不能因為任務需要，就自行提升自己的權限。

權限變更必須來自可信任的系統或人類。

---

## 常見的錯誤設計

### 1\. 所有工具都預設開放

方便 Demo，但 Production 風險很高。

工具應根據任務、使用者與環境縮小 Scope。

### 2\. 只在 Prompt 中描述禁止事項

Prompt 可以輔助，不能執行安全規則。

### 3\. 每一步都要求批准

會造成 Approval Fatigue，使用者最後可能不再仔細閱讀。

### 4\. Approval 畫面只顯示工具名稱

使用者需要看到真正副作用，不只是內部函式名稱。

### 5\. 批准後直接在真實環境執行

高風險操作即使獲得批准，也可能需要 Sandbox。

### 6\. 子 Agent 繼承父 Agent 所有工具

應該根據子任務進行最小權限委派。

### 7\. Permission Denied 直接讓整個任務失敗

有些限制可以作為 Observation，讓模型改用安全方法。

---

## 如何設計第一版權限模型？

如果系統還在早期階段，可以先從簡單分類開始。

### Low Risk

可以自動執行：

- 讀取允許範圍內的資料
- 搜尋公開資訊
- 執行無副作用檢查
- 產生草稿
- 在 Sandbox 中計算

### Medium Risk

限制 Scope 或記錄 Audit：

- 修改 Workspace 內的檔案
- 建立本地 Commit
- 更新測試資料
- 呼叫有成本的 API
- 讀取內部資料

### High Risk

需要 Approval，並考慮 Sandbox：

- 發送訊息
- 公開發布
- 刪除資源
- 部署 Production
- 付款或交易
- 修改大量資料
- 存取敏感資訊

### Forbidden

永遠拒絕：

- 越過使用者權限
- 關閉 Audit
- 存取未授權 Secret
- 修改系統安全設定
- 自行提升權限

先建立清楚的風險層級，比一開始設計非常複雜的 Policy Language 更實用。

---

## 今天新增了什麼能力？

Day 3 的 Tool Runtime 已經可以：

- 找到工具
- 驗證參數
- 執行操作
- 處理錯誤與逾時
- 正規化輸出

今天加入：

```markdown
Permission
Approval
Sandbox
Least Privilege
Risk Level
Permission Observation
```

因此，系統不再只問：

> 這個工具能不能執行？

而是進一步問：

> 這個 Agent、在這個任務、對這個資源、使用這組參數，是否應該被允許執行？

---

## 今天的結論

一個 Agent 是否可靠，不只取決於它能不能完成任務。

還取決於它犯錯時能造成多少影響。

Permission、Approval 和 Sandbox 分別建立三道邊界：

- Permission 限制可以做什麼
- Approval 把高風險決定交還給人類
- Sandbox 限制錯誤的影響範圍

最重要的原則是：

> 能力應該由工具提供，權限應該由系統控制，風險應該由隔離環境限制。

下一篇會繼續拆解 Hooks：

> 如何在不修改 Agent Loop 核心邏輯的情況下，加入驗證、紀錄、阻擋與自訂行為？

**完整系列與程式碼範例收錄於 [GitHub](https://github.com/hardness1020/awesome-agent-architecture)**

- [留言1](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401298)

[

【Day 3】Agent 選了工具，誰負責真的執行？

](https://ithelp.ithome.com.tw/articles/10401298)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 4 篇

目錄

1. 1
	[【Day 1】AI Agent 的架構到底在哪裡？模型之外的 Harness 才是關鍵](https://ithelp.ithome.com.tw/articles/10401140)
2. 2
	[【Day 2】Agent 為什麼能一直持續運行？ 實作最小的 Agent Loop](https://ithelp.ithome.com.tw/articles/10401146)
3. 3
	[【Day 3】Agent 選了工具，誰負責真的執行？](https://ithelp.ithome.com.tw/articles/10401298)
4. 4
	[【Day 4】Agent 能使用工具，不代表它應該擁有所有權限](https://ithelp.ithome.com.tw/articles/10401436)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/0e394f25-886c-417e-9d03-56eb506f17bd_small.jpg)

[駭客你比我想得閣較厲害](https://itplus.ithome.com.tw/vod-page/154?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

35 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/4bebd06e-cdd8-4c2c-a360-2d8305060a7d_small.png)

[AI模型的理想化和實用性妥協](https://itplus.ithome.com.tw/vod-page/1078?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/25/3ad7eb9c-1401-4e05-a3aa-e8e8d77d2fa1_small.png)

[使用 GitLab 和人工智慧加強現代軟體供應鏈安全 Securing the Modern Software Supply Chain with GitLab and AI](https://itplus.ithome.com.tw/vod-page/1156?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

76 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/a09fa9f5-f703-4d2f-9bbf-4a1144fa59c3_small.png)

[DataOps + ChatOps: 打造跨團隊協作的控制面](https://itplus.ithome.com.tw/vod-page/1473?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

25 分