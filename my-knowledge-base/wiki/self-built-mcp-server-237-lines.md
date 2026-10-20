---
title: 自建 MCP server 237 行：SDK 2.0 的 API 遷移與「檢查器先檢查你的規則」
date: 2026-09-04
tags: [ai/mcp, ai/claude, type/how-to]
source: https://medium.com/@n913239/自己做一個-mcp-server-237-行-第一次跑就抓到我三個月前洩漏的東西-921040a17e7b
type: article
status: processed
asset_value: 75
---

> 🔴 **本條目含一項會讓既有教學失效的 API 變更**：**MCP Python SDK 2.0.0 已移除 `mcp.server.fastmcp.FastMCP`**，
> 類別改名為 `MCPServer` 並搬到 `mcp.server.mcpserver`。庫內 [[mcp-fastmcp-server]] 教的正是舊 import 路徑，
> 照抄第一行就會拿到 `ModuleNotFoundError`。以本條目的寫法為現行做法。

作者把「發布前檢查清單」做成一支 237 行的 MCP server（`medium-check`），第一次跑就掃出 82 個問題——其中 25 個是他自己的規則寫錯。

## T（核心論點）

> **一件事該做成脈絡、能力，還是強制？看它是「知識」還是「計算」、回傳的是「資料」還是「行為」。**

四層判準（比 [[agent-skills-vs-mcp]]、[[ai-skills-mcp-plugins-agents-comparison]] 多出 hook/CI 這一層）：

| 層 | 放什麼 | 為什麼 |
|---|---|---|
| `CLAUDE.md` | **知識**——不知道就會做錯的專案事實 | 官方文件寫明它是**脈絡不是強制執行**：會被讀進去、會影響行為，但**不保證被遵守** |
| **MCP server** | **計算**——每次答案都一樣的東西，回傳**資料** | 「這篇有幾張圖、幾行提示行、相不相等」是計算不是判斷。🔑 **讓模型每次重數一遍，它真的會數錯** |
| **skill** | **行為**——寫給 AI 讀的流程，產出是 AI 的行為 | 「照這幾步做」 |
| **hook / CI** | 不通過就擋下來 | 最後一道關；但寫作過程中更需要的是「隨時能問一句現在合格了嗎」 |

> 📎 2026-10-08 後續：作者一個月後把同一套檢查改寫成 Claude Code Mod，並記載 MCP 版此時已增至 307 行、九項檢查（是後來增加的，不是矛盾）。後續篇的結論是 MCP 與 Mod「不是同一類東西」，並在本表之外補上第四格「改 App 本身（Mods）」；另實測 MCP 工具定義常駐成本只有 174 tokens（deferred 載入），修正了「MCP 每輪吃 context」的直覺。見 [[mcp-server-vs-mod-rewrite]]。

第二個論點在方法論層更重要：**把默會的規矩寫成可執行的斷言，會逼你把規則第一次講清楚**。「圖片數要等於提示行數」寫在 `CLAUDE.md` 是一句話；要寫成程式，你必須回答「code block 裡的 `![]()` 算不算」「什麼叫已經去識別化過」。所以第一份報告 82 個問題裡有 25 個是自己的 bug，完全合理——**檢查器的第一個使用者，是它自己的規則**。

## B（適用場景）

**適用：**
- 想把重複的人工檢查（發布前 checklist、命名規範、格式一致性）交給機器，但還沒到要擋 commit 的階段
- 第一次自建 MCP server，且參考的是半年內的中文教學（幾乎全數已過時）
- 需要判斷手上這條規則該寫進 `CLAUDE.md`、skill、MCP server 還是 hook

**不適用 / 注意：**
- 🔴 **教材保鮮期已短於生態移動速度**。本篇踩到的三件事（`FastMCP` 改名、Python 側全面 snake_case、協定變 stateless）全發生在半年之內，且動手前都不知道。動手前先請 AI 撈一遍當前官方文件與 release note，比照搜尋結果第一頁抄划算
- ⚠️ **光問 AI 還不夠**：SDK 常數寫 `LATEST_PROTOCOL_VERSION = 2026-07-28`，實際協商出的卻是 `2025-11-25`。**文件會過期，連程式碼裡的常數都可能誤導你**
- 本篇是本機 stdio server 的經驗，HTTP 部署的 stateless 議題另見 [[mcp-stateless-architecture]]

## R（操作步驟與三個坑）

### 坑一：`FastMCP` 已經不叫 FastMCP

```python
# ❌ 網路教學九成的開頭，SDK 2.0.0 會 ModuleNotFoundError
from mcp.server.fastmcp import FastMCP

# ✅ SDK 2.0 起改名 MCPServer，住在 mcp.server.mcpserver
from mcp.server.mcpserver import MCPServer

mcp = MCPServer(
    "medium-check",
    instructions="這個 repo 的發布前檢查清單。貼文到 Medium 之前用 check_article 跑一遍。",
    version="0.1.0",
)
```

介面幾乎沒變（一樣 `.tool()` 裝飾器加 `.run()`），只是名字換了。

### 坑二：Python 側物件屬性全面 snake_case

協定在線上傳的仍是 camelCase（JSON-RPC 規格不能動），但 Python SDK 的物件屬性全改了：`serverInfo` → `server_info`、`inputSchema` → `input_schema`。錯誤訊息會提示新名字，但照舊範例寫會連撞兩次。

### 好消息：schema 不用手寫

**type hint + docstring 就夠了**，SDK 會翻成 JSON Schema。🔑 **寫得好的 docstring 直接變成模型看得到的說明書**——與 `CLAUDE.md` 是同一件事：你寫的每一句都在替模型擋掉一種做錯的方式。

### 接上 Claude Code

```bash
uv run --with mcp python server.py          # 不汙染全域環境，換版本／丟掉重來都不痛

claude mcp add medium-check --scope project -- \
  uv run --with mcp python "$PWD/tools/mcp/medium-check/server.py"
```

`--scope project` 會生出進版控的 `.mcp.json`。兩件必做的修正：

1. `claude mcp add` 產出的是**絕對路徑**，要手動改成 `${CLAUDE_PROJECT_DIR:-.}`——寫死絕對路徑等於別人 clone 下來一定跑不起來，還把你的目錄結構一起公開（見 [[claude-code-file-management]]）
2. ⚠️ **那個 `:-.` 預設值是必要的**：`CLAUDE_PROJECT_DIR` 設在 server 的環境裡，不是 Claude Code 自己的環境，沒有預設值會展開成空字串

首次會顯示 `⏸ Pending approval`，**不會自動跑**。🔑 這不是麻煩，是 `.mcp.json` 的**供應鏈防線**：設定檔進版控，任何人 clone 都會拿到；若自動啟動，「clone 一個 repo」就等於「在自己機器上執行陌生人指定的指令」（與 [[agent-permission-and-os-sandbox]]、[[prompt-injection-vs-jailbreak]] 同一防線）。

### 回傳結構化資料，不是散文

```json
{ "article": "...", "passed": false, "failed": 1,
  "checks": [ { "name": "fullwidth[zh]", "ok": false, "detail": "123 處：L3：:, L11：,…" } ] }
```

**Claude 拿到的是可以再拿去做判斷的東西**，不必重新解析一段話。

## C（關鍵行動）

- [ ] 手上重複做的人工檢查逐條分類：知識→`CLAUDE.md`／計算→MCP server／行為→skill／必須擋→hook + CI
- [ ] 動手前先請 AI 撈一次當前官方文件與 release note，不要照搜尋結果第一頁抄
- [ ] 要確定實際跑的協定版本，**自己對 server 發一個原始 JSON-RPC request**，不要相信 SDK 常數
- [ ] `.mcp.json` 進版控前把絕對路徑改成 `${CLAUDE_PROJECT_DIR:-.}`（預設值不可省）

## 補充資料

### 實測：SDK 常數與實際協商出的協定版本不符

2026-07-28 的 MCP 規格是成立以來最大改動：協定變 stateless、拿掉 `initialize` / `notifications/initialized` 握手、移除 Streamable HTTP 的 `Mcp-Session-Id`、新增規格寫明 server **MUST** 實作的 `server/discover`。作者沒有猜，直接對自己的 server 發原始 JSON-RPC：

```
→ {"method": "server/discover"}
← {"error": {"code": -32601, "message": "Method not found"}}

→ {"method": "initialize", "params": {"protocolVersion": "2026-07-28", …}}
← {"result": {"protocolVersion": "2025-11-25", …}}
```

🔴 **SDK 2.0.0 常數寫 `LATEST_PROTOCOL_VERSION = 2026-07-28`，高階 `MCPServer` 實際協商出的是 `2025-11-25`，且未實作 `server/discover`。** 規格定案五週，高階 API 還沒跟上。所以今天寫本機 stdio server，跑的仍是有握手的舊協定。（此實測應與 [[mcp-stateless-architecture]]、[[mcp-stateless-migration-state-ownership]] 的規格描述對讀——那是「文件寫的」，這是「實際跑出來的」）

### MRTR 與「tool 寫得愈純愈不會被協定改動掃到」

同版新增 **MRTR（Multi Round-Trip Requests）**：server 需要更多資訊時不再回頭跟 client 要，而是回 `resultType: "input_required"`，由 client 補答案後**重送同一個 request**。它出現正是因為 stateless——沒有 session，server 就沒有一條屬於某 client 的連線可以主動推東西，只能把流程反過來。

🔑 **需要中途跟使用者互動的 tool 這次全被重新設計；純粹「輸入 → 輸出」的那種從頭到尾沒被動過。**

### 兩條寫錯的規則：檢查範圍必須逐條決定

- **佔位符檢查**誤判 2 篇：文中出現的 `// TODO: remove` 是文章討論的**對象**，不是殘留。修法是先剔除 fenced code（可選連行內 code span）再檢查
- **去識別化檢查**相反：洩漏全都在 code block 裡，所以**不能**排除程式碼

🔑 **同一份文字，不同的檢查要看不同的範圍**——這與 [[ai-code-review-safeguards]] 的死角清單互補。

另一個易忽略的細節：**樣式必須同時編碼「什麼是洩漏」和「什麼叫已經消毒過」**。`/Users/[a-z0-9]+/` 連官方佔位符 `/Users/you/` 也照抓，最後寫成負向前瞻：

```
/Users/(?!you/|username/|USER/)[a-z0-9]+/
```

（作者為了展示那行洩漏樣式，把真名原封不動貼進草稿——**修一個洩漏的過程本身製造了新的洩漏**。清單只在你記得看的時候有用，工具是每次都看。）

### 檢查器抓到的三件真事

1. **三個月前公開了自己的使用者名稱**——`/Users/<真名>/projects` 出現在教學文的 JSON 範例裡，中英各 4 處共 8 處，每次貼文前都「檢查過」，每次都沒看到
2. **手動統一標題漏了三種**——上次是 grep `結語`／`Conclusion` 去找的，而有一篇**沒有任何收尾標題可以被搜到**。🔑 **你沒辦法 grep 一個「不存在」**；檢查器斷言的是**結構**（最後兩個 H2 必須是「總結」和「參考資料」），對「缺少」與「多餘」一樣敏感
3. **慣例被採用的那一刻在數據上看得見**——全形標點數量逐篇排開，某篇 212 是全系列最高，下一篇直接掉到 12，那個斷崖就是規則寫進 `CLAUDE.md` 的那一天

## 相關連結

- [[mcp-fastmcp-server]] — 🔴 **該條目教的 `mcp.server.fastmcp.FastMCP` 已在 SDK 2.0.0 移除**，操作步驟需改用本條目的 `mcp.server.mcpserver.MCPServer` 寫法
- [[mcp-stateless-architecture]] / [[mcp-stateless-migration-state-ownership]] — 2026-07-28 stateless 規格；本篇補上「實際協商出的是 2025-11-25、未實作 `server/discover`」的實測落差
- [[agent-skills-vs-mcp]] / [[ai-skills-mcp-plugins-agents-comparison]] — 選型比較；本篇多出 **hook/CI 這一層**與「模型真的會數錯」的具體理由
- [[agent-permission-and-os-sandbox]] — `.mcp.json` 首次人工核准＝供應鏈防線
- [[claude-code-file-management]] — 絕對路徑改 `${CLAUDE_PROJECT_DIR:-.}`
- [[ai-code-review-safeguards]] — 自動檢查的死角；本篇的「去識別化不能排除 code block、佔位符檢查相反」是具體反例
- [[apple-claude-md-teardown]] — 系列前篇，「脈絡／能力／強制」三層的出處
- [[mcp-server-vs-mod-rewrite]] — 系列後續：同一套檢查改寫成 Claude Code Mod（MCP 版已增至 307 行）；差別在「站在哪裡」，不在省 token
