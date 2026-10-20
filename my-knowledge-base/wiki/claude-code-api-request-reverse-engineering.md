---
title: 側錄 Claude Code：用 proxy 攔下真實 payload，親手做出記憶功能
date: 2026-08-03
updated: 2026-08-03
tags: [ai/claude, ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw（「為你自己手刻 Claude Code」系列 Day 02–03）
type: article
status: processed
asset_value: 85
---

整合〈為你自己手刻 Claude Code〉系列 **Day 02（側錄）+ Day 03（第一個 API 請求）+ Day 04（讓模型假裝有記憶）**。
與 [[claude-code-harness-internals]] 是**同主題的兩種視角**：那篇是規格書視角（官方文件 + 實測），本篇是**逆向工程視角**（攔真實流量驗證）。

## T（核心論點）

用一個 30 行的本機 proxy 攔下 Claude Code 送出的**真實 payload**，會發現 **agent 就是四個詞：模型、迴圈、工具、context**——而且**三個都在那一包請求裡，只有「迴圈」不在**（因為迴圈是程式的行為，不是送給模型的資料）。攔到的東西，就是「你要親手打造的規格書」。

## B（適用場景）

**適用：** 想確認 Claude Code 到底送了什麼出去；要自建 agent harness 想看業界標準長怎樣；除錯「模型為什麼沒看到某個東西」

**不適用 / 注意：**
- 攔到的工具數量**因你的設定而異**（作者那台裝了額外東西，攔到 83 個工具定義）
- ⚠️ Day 02 實測對 `haiku-4-5` 送的 thinking 參數仍是**舊形態**（`budget_tokens`）——與 [[claude-code-harness-internals]] 的「4.6 後改 adaptive」**不矛盾，是世代差異**，引用時要標模型

## R（三個關鍵發現）

### 一、🔑 為什麼看得到明文——沒有破解任何加密

這是最容易誤解的一點：

```bash
ANTHROPIC_BASE_URL=http://127.0.0.1:9527   # 注意是 http，不是 https
```

> **Claude Code 送到 proxy 這一段路是沒有加密的明文**；加密發生在 **proxy 轉交給 `api.anthropic.com`** 的那一段。
> 換句話說：**我們並沒有破解任何加密——Claude Code 把明文交給我，我看完之後才幫它加密送出去。**
> 這就是為什麼中間人手法攔的是**自己主動導流過來的流量**，而不是去破解別人的連線。

### 二、四個詞全在那封信裡（除了迴圈）

| 詞 | 在 payload 的哪裡 |
|---|---|
| **模型** | 第一行 `model` 明白寫著這次用哪一顆腦 |
| **工具** | `tools` 陣列——作者攔到 **83 個工具定義**。🔑 **工具是「連同這次請求一起送過去」的，不是模型自己內建的**——這代表工具由我們這邊定義、我們這邊送 |
| **context** | `messages` 陣列——對話剛開始只有一則，聊二十輪就塞滿二十輪來回。**「那本要重新念給模型聽的日記」就是這個陣列** |
| **迴圈** | 🔴 **不在請求裡**——迴圈是 Claude Code 這個程式的「**行為**」，不是送給模型的資料。「送出、拿回、再送出」的節奏就是迴圈；**攔到的是迴圈轉某一圈時手上正拿著的那包東西** |

### 三、工具定義長什麼樣（「跟 AI 許願」的真相）

以最單純的 `Read` 工具為例：
```json
{
  "name": "Read",
  "description": "Reads a file from the local filesystem... （這段說明書總共 1782 個字元）",
  "input_schema": {
    "type": "object",
    "properties": {
      "file_path": { "description": "The absolute path to the file to read", "type": "string" },
      "offset": { "description": "The line number to start reading from...", "type": "integer" }
    },
    "required": ["file_path"], "additionalProperties": false
  }
}
```

> **一個「讀檔」工具的 description 就寫了 1,782 個字元**——這就是所謂「跟 AI 許願」：工具能不能被正確使用，全看你這份說明書寫得多清楚。
> → 這也解釋了 [[claude-5-context-engineering-rules]] 為什麼強調「**與其堆範例，先設計清楚的工具介面**」——介面說明就是這 1,782 字元。

### 四、Day 03：手刻陽春版，證明「差別只是欄位多寡」

用官方 SDK 發第一個請求後，再用**同一個 proxy** 看自己送出去的 payload：**結構與 Claude Code 攔到的完全相同，差別只是欄位多寡**。

**兩個親手驗證的事實**：
- **大模型的記性很差**——Messages API 是**無狀態**的，不自己把歷史送回去，模型就不記得上一輪
- **記憶、帳單、聰不聰明是連在一起的**——你送多少歷史，就付多少錢、也決定它有多「記得」

## 五、Day 04：親手做出「記憶」——其實只是每次把日記重新念一次

### 🔑 核心洞察（整篇濃縮成一行）

> **你以為的「AI 的記憶」，其實只是自己每次不厭其煩地再跟模型把講過的話重新報告一次。**

用一個 Python list（作者稱「日記」）存對話，兩條規則：**我講一句記一筆 `user`，模型回一句記一筆 `assistant`**。每次呼叫模型，把整本日記當 `messages` 送出去。改動只有一行——`messages` 從塞一句話變成塞整個 `history`：

```python
history.append({"role": "user", "content": user})
resp = client.messages.create(model="claude-haiku-4-5", max_tokens=1024, messages=history)
history.append({"role": "assistant", "content": resp.content})   # 存 resp.content 不是純文字
```

**驗證**：先報名字「我是菜市場阿龍」，再問「我是誰？」——上一版（沒記憶）兩手一攤，這版接得住。差別只在**這次把整本日記念給它聽**。

### 🔑 為什麼存 `resp.content` 不是純文字

先把回覆整理成純文字 `text` 只是拿來顯示，**存進日記的卻是 `resp.content` 整個物件**——這是為工具呼叫鋪路：模型的回覆是「一疊積木」，之後加入工具後會回一塊 `tool_use` 積木，代表「我想用某個工具」，**日記必須完整保存這些積木，模型才會知道自己剛剛決定用哪個工具**。SDK 收到積木物件會自動序列化成正確 JSON，直接把 `resp.content` 塞回 `history` 即可。

### 🔬 用 proxy 驗證：對話越長，`messages` 怎麼長大

聊幾輪後翻 `captured/`，`request-N.json` 一輪一個檔：
- 第 1 輪：`messages` 只有 1 則
- 第 2 輪：變 3 則（第一輪的一問一答被**原封不動又送了一次**）
- 第 3 輪變 5 則、第 4 輪變 7 則——**每聊一句就多兩筆**

### 🔴 造假記憶實驗——模型分辨不出「真實」與「預先塞入」的歷史

既然模型看到的「過去」只是這次送過去的 `messages`，若在日記裡**預先塞兩則它根本沒講過的假對話**，它會發現嗎？

```python
history = [
    {"role": "user", "content": "我在寫一個 coding agent，叫什麼名字好？"},
    {"role": "assistant", "content": "叫 KeSi 吧，這個名字唸起來很有精神。"},
]
```

答案是**不會**——這與 Day 02/03 講的「模型只看 payload，不知道歷史是不是真的發生過」是同一件事的親手驗證，也呼應 [[claude-code-harness-internals]] 記載的「**合成歷史**」注入管道（harness 也會做同樣的事：讓 `@README.md` 提及變成一組假造的 `tool_use`/`tool_result` 配對）。

### 🔴 temperature 參數的世代分界（與 thinking 世代分界同一模式）

`temperature` 控制生成隨機程度，調低會讓輸出更收斂，但官方**從未保證完全相同**。這是 **Haiku 4.5 還支援**的做法，**Opus 從 4.7、Sonnet 從 5 開始不再接受**這個參數——硬填會直接吃 400，錯誤訊息 `temperature is deprecated for this model`。

> 這與 [[claude-code-harness-internals]] 記載的 **thinking 參數世代分界**（4.5 前 `budget_tokens` vs 4.6+ `adaptive`/`effort`）是**同一種模式、不同參數**——新世代模型把越來越多控制權從「參數微調」移向「模型自己判斷」。

### REPL 概念澄清

「一直等輸入、處理、回頭再等下一次輸入」的迴圈就是 **REPL**（Read-Eval-Print-Loop）——`python3` 的 `>>>`、瀏覽器 F12 console、甚至 **Claude Code 本身的互動介面**都是 REPL。KeSi 只是在每一圈裡多了「把問題送給模型、拿答案回來」的來回過程。

## C（關鍵行動）

- [ ] 用 `ANTHROPIC_BASE_URL=http://127.0.0.1:<port>` 架本機 proxy，實際看一次自己的 payload（**不需要破解加密**）
- [ ] 用 `/context` 對照 proxy 攔到的內容，確認 token 消耗來自哪一塊
- [ ] 設計自己的工具時，把 `description` 當「說明書」寫——**官方 `Read` 工具用了 1,782 字元**
- [ ] 引用 thinking 參數形態時**標明模型世代**（本篇對 haiku-4-5 攔到舊形態）

## 原始摘要

iThome〈為你自己手刻 Claude Code〉系列 Day 02–03。Day 01「就只是一個 while 迴圈」的論點已被 [[agent-loop-explained]] 覆蓋，未回補。工具鏈用 `uv`；proxy 以 Python `HTTPSConnection` 轉發。作者定位：「我們接下來 30 天要親手打造的東西，它的規格就長這個樣子。」

## 相關連結

- [[claude-code-harness-internals]] — **配對條目**：規格書視角（三層架構、快取 TTL、注入四管道、compaction 保留清單）。本篇的實測在**快取、thinking 參數、stateless** 三處與其互相佐證
- [[agent-loop-explained]] — Agent Loop 概念（Day 01 的論點已被覆蓋）
- [[multi-agent-papers-three-elements]] — 「迴圈 + 提示 + 集約」三要素；本篇的「四個詞」是同一拆法的另一版本
- [[claude-5-context-engineering-rules]] — 「設計清楚的工具介面勝過堆範例」——本篇的 1,782 字元 description 是具體證據
- [[api-cli-skills-agent-architecture]] — 工具介面設計原則
- [[awesome-agent-architecture-from-scratch]] — 從零拆解 Agent Harness 的完整教材
- [[synthesis-claude-code-internals]] — **本篇為其核心來源**（六篇整合精粹）；該精粹把本篇的 temperature 世代分界與 harness-internals 的 thinking 世代分界合併成一個**模式**：新世代把控制權從參數微調移向模型自己判斷
- [[hand-coding-agent-file-tools]] — **同系列 Day 05／07／08／09／10 續篇**：`tool_choice` 四模式、tool result 格式寫錯會教模型放棄平行呼叫、包 `rg` 子行程與結束碼三分法、編輯工具的唯一性要求、write_file 四道防呆；並記錄該系列**刻意不走 RAG** 的理由
