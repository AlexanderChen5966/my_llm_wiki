---
title: "【Day 3】Agent 選了工具，誰負責真的執行？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401298"
author:
  - "[[iThome]]"
published:
created: 2026-08-05
description: "前兩天我們先拆開了兩個核心概念。 Day 1 說明： 模型（LLM）負責判斷，Harness 負責讓判斷可以被執行、觀察與限制。 Day 2 則把 Agent 縮到最小，只留下： 呼叫模型 ↓ 檢查停..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 3

1

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 3 篇

## 【Day 3】Agent 選了工具，誰負責真的執行？

- 分享至

前兩天我們先拆開了兩個核心概念。

Day 1 說明：

> 模型（LLM）負責判斷，Harness 負責讓判斷可以被執行、觀察與限制。

Day 2 則把 Agent 縮到最小，只留下：

```markdown
呼叫模型
↓
檢查停止原因
↓
執行工具
↓
把結果交回模型
↓
進入下一輪
```

今天要繼續拆其中最容易被忽略的一步：

> **執行工具**

很多 Framework 把它包裝成一行 API，讓人感覺模型只要產生 Tool Call，工具就會自然完成。

但 Tool Call 只是一個請求。

它不是執行結果，也不是安全邊界，更不是完整的 Tool Runtime。

---

## Tool Call 只是模型提出的結構化意圖

假設使用者要求：

> 幫我讀取專案中的設定檔。

模型可能回傳：

```json
{
  "name": "read_file",
  "arguments": {
    "path": "config.yaml"
  }
}
```

這段內容表示：

> 模型認為下一步應該呼叫 `read_file` ，並把 `config.yaml` 當作參數。

但到這個時間點，檔案還沒有被讀取。

系統仍然不知道：

- `read_file` 是否真的存在
- `path` 是否符合工具 Schema
- `config.yaml` 是否位於允許的目錄
- 模型是否有權限讀取它
- 工具是否能在時限內完成
- 回傳結果是否太大
- 執行過程是否需要被記錄
- 發生錯誤時要如何回傳給模型

所以 Tool Call 更接近一張工作單，而不是實際工作。

```markdown
Tool Call
=
工具名稱
+
模型產生的參數
+
模型希望系統執行的意圖
```

真正把這張工作單轉換成環境變化的，是 Tool Runtime。

---

## Tool Runtime 是什麼？

Tool Runtime 是模型與真實環境之間的執行層。

它至少要處理：

1. 找到工具
2. 驗證參數
3. 檢查權限
4. 執行工具
5. 控制逾時
6. 正規化錯誤
7. 限制輸出大小
8. 記錄執行結果
9. 把 Observation 送回模型

完整流程比較像：

```markdown
模型產生 Tool Call
        ↓
Tool Registry 尋找工具
        ↓
Schema Validation 驗證參數
        ↓
Permission Gate 檢查權限
        ↓
Runtime 執行工具
        ↓
Timeout / Error Handling
        ↓
Output Normalization
        ↓
Tool Result 回到 Agent Loop
```

模型只參與第一步。

其他步驟都屬於 Harness。

---

## 第一層：Tool Registry

最簡單的 Tool Runtime，通常需要一個工具註冊表。

```python
TOOLS = {
    "read_file": read_file,
    "write_file": write_file,
    "run_command": run_command,
}
```

當模型要求某個工具時，Runtime 會根據名稱尋找對應實作。

如果工具不存在，系統不應該直接 Crash。

更好的做法是產生結構化錯誤：

```json
{
  "ok": false,
  "error": {
    "type": "unknown_tool",
    "message": "Tool is not registered."
  }
}
```

再把錯誤交回模型。

這樣模型可以：

- 修正工具名稱
- 改用另一個工具
- 說明系統不支援這個操作
- 向使用者請求其他方法

Tool Registry 看似簡單，但它決定了模型實際能看見哪些能力。

> 模型不應該能呼叫任何沒有被明確註冊的函式。

---

## 第二層：Schema Validation

找到工具之後，不能立刻執行。

模型產生的參數可能有很多問題：

- 缺少必要欄位
- 欄位名稱錯誤
- 型別錯誤
- 值超出允許範圍
- 加入工具不接受的額外欄位
- 字串包含危險內容

例如工具需要：

```json
{
  "path": "string",
  "max_lines": "integer"
}
```

模型卻產生：

```json
{
  "file": "config.yaml",
  "max_lines": "all"
}
```

如果 Runtime 直接把這組參數交給函式，可能得到難以理解的 Exception。

比較可靠的流程是：

```markdown
模型參數
↓
Schema Validation
├── 通過 → 進入下一步
└── 失敗 → 回傳結構化錯誤
```

對模型來說，結構化錯誤比一大段 Stack Trace 更容易修正。

對系統來說，Schema Validation 也能在工具真正執行前攔截問題。

---

## 第三層：Permission Gate

參數合法，不代表操作應該被允許。

例如模型要求讀取：

```markdown
../../.ssh/id_rsa
```

它可能完全符合 `path: string` 的 Schema，但仍然不應該執行。

所以 Schema Validation 和 Permission Check 是兩個不同問題：

```markdown
Schema Validation
回答：這個請求格式正確嗎？

Permission Check
回答：這個請求被允許嗎？
```

一個基本 Permission Gate 可能檢查：

- 路徑是否位於 Workspace
- 指令是否包含禁止操作
- 工具是否需要使用者批准
- 模型是否能連接外部網路
- 目前任務是否允許寫入
- 是否能存取敏感資訊
- 是否超過成本或時間預算

這不是在告訴模型「不要讀取 Workspace 外的檔案」。

而是讓程式碼保證它做不到。

> Prompt 是行為指引，Permission Gate 才是執行邊界。

---

## 第四層：真正執行工具

通過工具查找、參數驗證與權限檢查後，Runtime 才能真的執行。

但工具仍可能：

- 執行很久
- 卡住
- 丟出 Exception
- 產生副作用
- 消耗大量資源
- 回傳無法序列化的物件

因此，Production Runtime 通常需要一個共同的 Execution Wrapper。

它的價值不是讓錯誤消失。

而是把不同工具的錯誤轉換成 Agent Loop 可以處理的共同格式。

例如：

```json
{
  "ok": false,
  "error": {
    "type": "FileNotFoundError",
    "message": "config.yaml does not exist."
  }
}
```

模型看到這個結果後，才有機會：

- 修正路徑
- 改用搜尋工具
- 詢問使用者
- 結束任務並說明原因

---

## 第五層：Timeout

如果模型呼叫一個永遠不會結束的工具，Agent Loop 也會被卡住。

例如：

- 網路請求沒有回應
- Shell 指令等待輸入
- 資料庫查詢掃描過多資料
- 外部 API 一直沒有回傳
- 子程序死鎖

所以 Runtime 不能只限制 Agent 有幾輪，也要限制每個 Tool Call 可以執行多久。

```markdown
Turn Budget
限制 Agent 可以進行幾輪

Tool Timeout
限制單一工具可以執行多久
```

兩者解決不同問題。

當工具超過時限，Runtime 應該回傳一個可觀察的錯誤。

模型可以再決定：

- 改用較小的查詢
- 換一個工具
- 詢問使用者
- 結束任務

但 Retry 次數仍然應由 Harness 控制，而不是讓模型無限嘗試。

---

## 第六層：Output Normalization

工具成功執行後，也不能把任何結果直接塞回 Context。

工具可能回傳：

- 幾萬行 Log
- 大型 JSON
- Binary File
- Database Cursor
- Python Object
- 含有敏感資訊的內容
- 不合法的編碼
- 無法序列化的資料

如果全部加入 `messages[]` ，可能立刻耗盡 Context Window。

所以 Runtime 通常需要把輸出轉換成共同格式：

```json
{
  "ok": true,
  "content": "...",
  "metadata": {
    "truncated": false,
    "duration_ms": 42
  }
}
```

當內容太大時，可以：

- 截斷
- 摘要
- 分頁
- 儲存成 Artifact
- 只回傳索引或檔案位置
- 讓模型再選擇要讀取哪一段

Tool Result 應該提供足夠的 Observation，但不一定要把所有原始資料放進 Context。

---

## 第七層：Audit 與 Trace

只要 Agent 可以改變真實環境，就需要回答：

- 模型呼叫了哪個工具？
- 使用了哪些參數？
- 權限檢查是否通過？
- 實際執行了多久？
- 工具回傳什麼？
- 是否產生副作用？
- 哪一輪發生錯誤？
- 最後為什麼停止？

這些紀錄對三件事很重要：

1. Debug
2. Evaluation
3. Security Review

如果沒有 Trace，Agent 失敗時往往只剩下最終輸出，很難知道問題發生在哪一層。

---

## Tool Calling 與 Tool Runtime 的責任邊界

| 層級 | 主要責任 |
| --- | --- |
| Model | 選擇工具並產生參數 |
| Tool Schema | 描述工具可接受的輸入 |
| Tool Registry | 將工具名稱對應到實作 |
| Validator | 檢查參數是否合法 |
| Permission Gate | 判斷操作是否允許 |
| Runtime | 執行工具並處理逾時 |
| Normalizer | 將輸出轉成模型可讀格式 |
| Agent Loop | 把結果交回模型並進入下一輪 |

模型可以提出：

> 我想讀取這個檔案。

但它不應該同時決定：

- 自己是否有權限
- 參數是否可信
- 工具要執行多久
- 輸出是否可以完整放進 Context
- 這次操作是否需要 Audit

這些是系統責任。

---

## 常見的錯誤設計

### 1\. 直接相信模型產生的參數

模型輸出是非確定性的。

即使 Tool Schema 寫得很好，Runtime 仍應該驗證。

### 2\. 把 Permission 寫在 Prompt 裡

「不要做危險操作」不是安全邊界。

真正的限制必須存在於模型外面。

### 3\. 所有錯誤都回傳 Stack Trace

Stack Trace 可能太長、包含敏感路徑，也不一定能幫助模型修正。

應該先正規化成簡短、結構化的錯誤。

### 4\. 將完整 Tool Output 放回 Context

大量輸出會增加 Token 成本，也可能讓真正重要的資訊被淹沒。

### 5\. 工具失敗後無限重試

工具錯誤可以回到模型，但 Retry Budget 應由 Harness 限制。

### 6\. 把所有工具放進同一個 Registry

不同任務、使用者與環境可能需要不同的 Tool Scope。

模型只應該看到目前真的可以使用的工具。

---

## 一個簡單的判斷方法

設計工具時，可以逐層問：

- Tool Call：模型想做什麼？
- Validation：請求格式正確嗎？
- Permission：這個操作允許嗎？
- Execution：工具成功完成了嗎？
- Observation：模型真正需要看到什麼？
- Verification：工具完成後，任務是否真的更接近成功？

這六個問題不要混在一起。

它們分別對應不同的 Failure Mode。

---

## 今天新增了什麼能力？

Day 2 的 Agent Loop 已經可以：

- 讓模型選擇工具
- 執行工具
- 把結果送回模型
- 進入下一輪

但它把工具執行當成一個黑盒。

今天加入：

```markdown
Tool Registry
Schema Validation
Permission Gate
Timeout
Error Normalization
Output Normalization
Audit Trace
```

因此，系統不再只是「模型要求什麼就執行什麼」。

而是先把 Tool Call 經過一條可控制、可觀察、可驗證的執行管線。

---

## 今天的結論

Tool Calling 和 Tool Runtime 是兩件不同的事。

Tool Calling 是模型提出的結構化意圖：

> 我想使用哪個工具，並帶入哪些參數。

Tool Runtime 則負責：

- 工具是否存在
- 參數是否合法
- 操作是否允許
- 工具如何執行
- 何時應該逾時
- 錯誤如何回傳
- 輸出如何進入 Context
- 執行過程如何被記錄

可以把它濃縮成一句話：

> 模型可以選擇工具，但不能自己定義工具的執行邊界。

下一篇會繼續拆解 Permission、Approval 與 Sandbox：

> Agent 可以使用工具，不代表它應該被允許執行所有操作。

**完整程式碼與系列內容：https://github.com/hardness1020/awesome-agent-architecture**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401146)

[

【Day 2】Agent 為什麼能一直持續運行？ 實作最小的 Agent Loop

](https://ithelp.ithome.com.tw/articles/10401146)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 3 篇

目錄

1. 1
	[【Day 1】AI Agent 的架構到底在哪裡？模型之外的 Harness 才是關鍵](https://ithelp.ithome.com.tw/articles/10401140)
2. 2
	[【Day 2】Agent 為什麼能一直持續運行？ 實作最小的 Agent Loop](https://ithelp.ithome.com.tw/articles/10401146)
3. 3
	[【Day 3】Agent 選了工具，誰負責真的執行？](https://ithelp.ithome.com.tw/articles/10401298)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/20/3308d3cf-5c87-4aed-ac39-efb7ae693e3b_small.png)

[HashiCorp Vault 自建金鑰管理最佳入坑姿勢](https://itplus.ithome.com.tw/vod-page/773?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/18/027c1857-d82a-45ed-b4f6-70ed12f65384_small.png)

[告別渾沌專案管理方式，擁抱 Smartsheet 解放未來工作新時代](https://itplus.ithome.com.tw/vod-page/601?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/cd281286-da35-4e38-8a20-9940054a5016_small.png)

[Beyond NGINX：重新定義 Ingress Controller 的部署邊界與彈性架構](https://itplus.ithome.com.tw/vod-page/1310?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/53c00416-e5f2-41f8-b086-754db8bb1ea9_small.png)

[告別傳統辦公室！Azure Virtual Desktop 打造高效、安全的雲端辦公體驗](https://itplus.ithome.com.tw/vod-page/1220?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

28 分