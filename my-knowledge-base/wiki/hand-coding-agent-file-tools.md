---
title: 手刻 agent 的檔案工具：從 tool_choice 到 write_file 四道防呆
date: 2026-08-07
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw（〈為你自己手刻 Claude Code〉Day 05／07／08／09／10，作者 kaochenlong）
type: article
status: processed
asset_value: 80
---

整合〈為你自己手刻 Claude Code〉系列 **Day 05（模型怎麼使用工具）＋ Day 07（看見整個專案）＋ Day 08（在專案裡搜尋）＋ Day 09（字串替換改碼）＋ Day 10（write_file 防呆）**。與 [[claude-code-api-request-reverse-engineering]]（同系列 Day 02–04）是同一條學習線的續篇：那邊拆 payload 與記憶，這邊拆**工具實作**。

## T（核心論點）

**模型只會許願。** 它產生的 `tool_use` 只是意圖，所有真正的工作——找工具、驗參數、擋路徑、跑子行程、判斷結束碼、防止覆寫——都是你這邊要寫的。這五天的內容可以濃縮成一句：**agent 的難處不在讓模型會用工具，在於讓工具在被誤用時不會炸掉你的專案。**

## B（適用場景）

**適用：** 自建 agent 的檔案／搜尋／編輯工具；理解 Claude Code 那幾個內建工具為什麼長那樣；設計工具的錯誤回傳與防呆

**不適用 / 注意：** 實作語言為 Python（`pathlib`／子行程）；規格層的責任分工見 [[agent-tool-runtime-seven-layers]]，本篇是動手層

## R（五天的重點）

### Day 05：tool_choice 四種模式與「說明書也要錢」

工具定義要寫清楚四件事：**這工具做什麼／什麼時候該用（以及什麼時候不該用）／每個參數是什麼意思／有什麼限制跟不會回傳的東西**。

🔑 **說明書也是要錢的**——官方文件明載，帶 `tools` 時 API **會自動追加一段啟用工具使用的系統提示**：
> When you use `tools`, the API also automatically includes a special system prompt for the model which enables tool use

**`tool_choice` 四種模式**：
| 值 | 行為 |
|---|---|
| `auto` | 模型自己決定用不用——**有帶工具時的預設值** |
| `any` | 強制一定要用工具，但用哪個它自己決定 |
| `tool` | 強制用你指定的那一個 |
| `none` | 禁止使用任何工具——**沒帶工具時的預設值** |

⚠️ **平行呼叫失效的最常見原因**（官方說法）：
> The most common issue is formatting tool results incorrectly in the conversation history. This "teaches" Claude to avoid parallel calls.

→ **把 tool result 格式寫錯，會「教」模型不要再平行呼叫**。這是很容易忽略的自傷。

官方也說明了模型何時會呼叫工具：
> It calls a tool when the request maps to that tool's described capability and the answer isn't already in context. It responds directly for stable knowledge, creative tasks, and conversational turns.

### Day 07：兩隻眼睛（list_files / glob）與看不見卻摸得到

- **`list_files`** 與 **`glob`** 是 agent 的兩隻眼睛
- ⚠️ **`**` 通配符會走訪整棵樹**，官方警告大型目錄樹可能很慢
- 🔴 **有些東西不能讓它看**：路徑檢查要擋 `..\\..\\etc\\*` 這類穿越
- 🔑 **最重要的一節：「看不見但手還是摸得到」**——把某些路徑從 `list_files`／`glob` 的結果裡濾掉，**不等於它讀不到**。過濾顯示與限制存取是兩件事，必須各自實作

### Day 08：不自己寫搜尋，包一個 `rg` 子行程

**為什麼用 ripgrep 而不自己寫**：
- 在 Git repo 裡 `rg` 會讀 `.gitignore`（另讀 `.ignore`、`.rgignore`），要走訪的檔案就少很多
- 小數點開頭的路徑遞迴時預設不進去（`.git`、`.env` 都是）
- 🔑 **遇到疑似二進位就停下**——判斷方式是看內容有沒有值為 0 的 byte（`NUL`）；純文字檔幾乎用不到它，圖片與執行檔則不少。**它不是真的去認 PNG／ZIP 格式**

**結束碼三分法**（這是工具回傳設計的好範例）：
| 結束碼 | 回傳 | `is_error` |
|---|---|---|
| `0` | 有找到，回「檔名:行號:內容」 | `False` |
| `1` | 沒找到，回「找不到含有 xxx 的內容。」——**這不算錯誤** | `False` |
| 其他 | 執行出問題，回「搜尋失敗：」＋ rg 的錯誤訊息 | `True` |

⚠️ **子行程逾時的陷阱**（官方）：行程建立本身在許多平台 API 上無法被中斷，**所以至少要等到行程建立完成後才可能看到 timeout 例外**。

### 🔑 Day 08 的一節：為什麼這個系列不做 RAG

作者明確選擇不走向量檢索，理由值得記：

> RAG 厲害的地方是能找到字面上沒有共同關鍵字的段落（搜「登入」也能撈出「驗證身分」），這是 grep 做不到的事。
> **但要多出來的不是只一個函式**——內容怎麼切塊、拿什麼算 embedding、向量存哪裡、檔案改了索引怎麼跟著更新都得考慮，**這些沒有一項屬於 agent 的骨架**。

並引 Anthropic 的 just-in-time 路線作為佐證：**正版 Claude Code 手上先拿著檔名、路徑這類輕量線索，需要時才用工具把內容載進 context**——先載入 `CLAUDE.md`，接著用 glob、grep 探索。

> Letting agents navigate and retrieve data autonomously also enables progressive disclosure—in other words, allows agents to incrementally discover relevant context through exploration.

→ 這與 [[llm-wiki-over-rag-claude-code]]、[[rag-retrieval-augmented-generation]] 形成明確張力：**小型、持續變動的專案用關鍵字搜尋就夠；RAG 的成本主要在 ingestion 與索引維護，不在檢索本身。**

### Day 09：為什麼改 code 用字串替換而非整檔重寫

**唯一性要求**（官方原則）：
> Unique matching: Make sure replacements match exactly one location to avoid unintended edits.

**兩種必須擋掉的輸入**：
- `old_string` 是**空字串**——空字串到處都對得上，根本沒辦法拿來指位置
- `old_string` 與 `new_string` **一模一樣**——🔴 這種替換不會讓檔案有任何改變，**卻會被回報成功，模型還以為事情辦好了**

⚠️ 另有換行符陷阱：`write_bytes(b"x = 1\r\ny = 2\r\n")` 後用 `read_text()` 與 `read_bytes().decode()` 讀出來的結果不同——**改碼工具必須意識到 CRLF/LF 的差異**。

### Day 10：write_file 的四道防呆

`with open(path, "w")` 三行就能清空舊檔（官方定義：**open for writing, truncating the file first**）。所以 `write_file` 需要：

| # | 規則 |
|---|---|
| 1 | 檔案還不存在 → 用 `'x'` 模式建立（**open for exclusive creation, failing if the file already exists**）。萬一空檔中突然冒出同名檔，**寧可建立失敗也不要蓋掉它** |
| 2 | 檔案已存在 → **模型得先讀過現在的內容**才准整份蓋掉 |
| 3 | 讀完之後檔案又被別的程式改過 → **不准覆寫**，請它重讀一次再來 |
| 4 | 路徑跑到工作目錄外面／在 ignore 名單裡／指向一個目錄 → **直接拒絕** |

> 第 2、3 條合起來就是 **read-before-write ＋ 樂觀鎖**——這正是 Claude Code「必須先 Read 才能 Write」限制背後的機制。

## C（關鍵行動）

- [ ] 工具 description 寫齊四件事，特別是「**什麼時候不該用**」與「不會回傳什麼」
- [ ] 🔴 檢查 tool result 的格式——**寫錯會教模型放棄平行呼叫**
- [ ] 分清「過濾顯示」與「限制存取」：從 glob 結果濾掉不等於讀不到，兩者都要實作
- [ ] 搜尋直接包 `rg` 子行程，別自己寫（免費拿到 gitignore 處理、二進位偵測）
- [ ] 「沒找到」要回 `is_error=False`，只有**執行失敗**才算錯誤
- [ ] 編輯工具擋掉空 `old_string` 與 `old_string == new_string`（後者會假成功）
- [ ] `write_file` 上四道防呆：`'x'` 建立／先讀才准覆寫／讀後被改就拒絕／路徑越界拒絕
- [ ] 小型變動專案先用 grep 而非 RAG——**RAG 的成本在 ingestion 與索引維護**

## 相關連結

- [[claude-code-api-request-reverse-engineering]] — **同系列 Day 02–04**：proxy 攔 payload、四個詞、造假記憶實驗；本篇是其工具實作續篇
- [[agent-tool-runtime-seven-layers]] — Tool Runtime 七層責任的規格層；本篇 Day 08 的結束碼三分法是其第 5 層（Timeout/Error）、write_file 四道防呆是第 3 層（Permission Gate）的具體實作
- [[claude-code-harness-internals]] — 官方 Read 工具 description 1,782 字元、`<persisted-output>` 等機制；與本篇 Day 05「說明書也要錢」互為印證
- [[rag-retrieval-augmented-generation]] — ⚠️ **張力對照**：本篇 Day 08 明確論證「小型變動專案不該上 RAG」，並引 Anthropic just-in-time 路線佐證
- [[llm-wiki-over-rag-claude-code]] — LLM Wiki 優於 RAG 的同向論點
- [[agent-control-layer-permission-hooks-planning]] — Capability vs Permission；本篇「看不見但手還是摸得到」是該區分的具體案例
- [[agent-loop-explained]] — Agent Loop 概念（同系列 Day 01 已被其覆蓋）
- [[handcraft-agent-shell-tool]] — **同系列 Day 11（續篇）**：第七個工具 `run_command`。🔴 **本篇建立的所有柵欄（`safe_path()`、`IGNORE` 名單、先讀後寫、原子替換）在那一天全部繞得過去**——`read_file(".env")` 被拒絕，`run_command("cat .env")` 照印不誤。該篇補上執行時間上限、輸出雙上限、process group 收行程、stdout/stderr 同管序、結束碼三分法
