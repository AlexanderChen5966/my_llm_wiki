---
title: MCP 拿掉 session 之後：state 沒消失，只是換你自己負責
date: 2026-07-30
tags: [ai/mcp, ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10400585
type: article
status: processed
asset_value: 75
---

## T（核心論點）

看到 MCP 改 stateless，工程師容易冒出直覺：「太好了，session store 可以刪了」——**先別急**。`2026-07-28` RC 確實拿掉了 `initialize`、`initialized`、`Mcp-Session-Id`，也不再要求 sticky routing 與協定層 shared session storage。但 **browser 還開著、長任務還跑十分鐘、購物籃裡還有東西**。這些 state 沒有消失，**MCP 只是停止替應用把它們藏在 transport session 裡**。差別很小，實作責任差很多。

> **協定層 stateless 真正換來的是責任邊界**——state 未必變少，卻開始有名字。

## B（適用場景）

**適用：** 維運 remote MCP Server 要遷移到新規格的團隊；設計 stateful 工具（browser、長任務、workspace）的開發者

**不適用 / 注意：**
- 純本機 stdio MCP server 受影響較小
- ⚠️ **不要照抄 GitHub 的遷移結論**（見下方）——他們刪 Redis 不代表你也該刪

## R（遷移五步）

### 一、先盤點「原本藏在 session 裡的東西」

把所有「只要同一條 session 還在就沒問題」的假設找出來。常見藏身處：

- `initialize` 時順便建立的 user / workspace context
- 綁在**單一 server instance 記憶體**裡的 browser、task、cache
- 依賴 `Mcp-Session-Id` 查詢的 Redis 資料
- 只有原本那台 instance 看得到的訂閱與進度
- 把 SSE reconnect 當成原 request 繼續執行的程式

> 🔴 **最容易漏掉的是 initialization side effect**：很多 server 表面上只是在 handshake 交換能力，實際同時**載入憑證、選 tenant、建立暫存目錄、甚至啟動 browser**。拿掉 handshake 後，這些動作不會自動找到新家。

**盤點技巧：不要只搜尋 `session` 這個字**。去看——
- load balancer 為什麼需要黏住流量
- process restart 後哪些工作會不見
- 哪一段程式假設「這個 client 剛才已經驗證過」

### 二、Explicit handle 不是換一個 session ID

新版鼓勵 stateful application 回傳明確 handle（`browser_id`、`basket_id`、task handle），由後續 tool call 當成普通參數帶回：

```
browser.open()                    -> { browser_id, expires_at }
browser.navigate(browser_id, url) -> { current_url, title }
browser.close(browser_id)         -> { closed: true }
```

> 如果只把 `Mcp-Session-Id` 改名成 `browser_id`，其他都不管，**系統只是換了欄位名稱**。

**Handle 應該是一份 domain contract**，至少要回答五個問題：
1. 誰可以建立它？
2. 它屬於哪個 user、tenant 或 workspace？
3. 能存取哪些操作，能不能轉交？
4. 多久失效，誰可以提前撤銷？
5. 過期、已關閉或跨 tenant 使用時，各自回什麼錯誤？

🔴 **授權最容易被做反**：
> ❌ 先用 `browser_id` 找到 browser，再假設拿得到 ID 的人就有權限操作
> ✅ **先用目前 request 的 identity 與 tenant 驗證 handle ownership，通過後才載入資源**

**Handle 不適合直接帶出可猜測的內部流水號**。即使用隨機值，log、錯誤訊息與 tracing 仍可能留下。安全做法：記錄 handle type、tenant、lifecycle transition 與**不可逆的識別摘要**，不要把可重放的原值灑進每一層 log。

### 三、⚠️ Stateless 會把重試問題推到檯面上

RC 明講：response stream 壞掉，client 會用**新的 request** 重試。此時 server **不能再靠原本那條連線判斷「這是同一次操作」**。

| 類型 | 處理 |
|---|---|
| 天生可重試的**讀取**操作（`browser.current_page`）| 讓相同輸入得到相容結果 |
| 有**副作用**的操作（`order.submit`、`email.send`、`task.start`）| **要求 idempotency key**，保存足夠久的執行結果，或明確拒絕重複呼叫 |

**失敗語意要定義清楚**：第一次 `order.submit` 已成功但回應途中斷線，第二次帶同一把 key 進來 → **server 應回傳原結果，而不是再送一次訂單**。
若 key 已過保存期限，**也別假裝從沒看過**，否則重複副作用只是時間問題。

> Client 沒收到 response，**不代表 server 沒完成副作用**。這些規則原本被 session 與連線壽命遮住，協定 stateless 後終於必須寫進 application contract。

### 四、新舊相容收在薄薄的 protocol adapter

新版 request 明確宣告版本與 capabilities、server 要實作 `server/discover`；舊版仍有 initialization lifecycle。同時服務兩世代時：

```
legacy MCP request ----\
                        -> protocol adapter -> domain service -> state store
stateless MCP request --/
```

Adapter 負責版本判斷、欄位轉換、舊版 session 對應；**domain service 只認明確的 identity、capability 與 handle，不應知道這次 request 曾經走過 `initialize`**。

> 這條線沒切乾淨，legacy session 很快滲回核心邏輯——**最後大家嘴上說 stateless，domain service 還是偷偷讀取某個 process-local map，failover 一來就露餡**。

### 五、Conformance 通過後，還有一半要自己測

MCP conformance suite 可分別測 client/server、指定 `2026-07-28` 或 draft suite，會區分 legacy 與新 stateless lifecycle，適合放進遷移 gate。
**但 conformance 驗的是協定行為，不會替你證明 `browser_id` 沒有跨 tenant 洩漏。**

應用層至少要補這六個情境：
1. 同一個副作用 request 被送兩次
2. Server 完成工作後，response 在途中遺失
3. Handle 過期、撤銷或已關閉
4. **另一個 tenant 拿到 handle 後嘗試操作**
5. 執行中的 task 遇到 instance failover
6. Legacy client 與新版 client 同時連入

> 🔴 **測試時要故意把 sticky routing 關掉**。若只有同一台 instance 連續收到 request 才會成功，那套實作**仍然依賴隱藏 session**，只是基礎設施暫時幫你把問題蓋住。

## C（關鍵行動）

- [ ] 盤點時**不要只搜 `session` 字串**——查 LB 為何黏流量、restart 後什麼會不見、哪裡假設「剛才已驗證過」
- [ ] 特別找出 **initialization side effect**（載憑證／選 tenant／建暫存目錄／啟動 browser）
- [ ] 每個 handle 寫成 domain contract，回答五個問題（誰建／屬於誰／能做什麼／多久失效／錯誤語意）
- [ ] **授權順序：先驗 ownership 再載入資源**，不要反過來
- [ ] 副作用操作一律加 **idempotency key**，並定義 key 過期後的行為
- [ ] 新舊相容收在 protocol adapter，**別讓 legacy session 滲進 domain service**
- [ ] 測試時**關掉 sticky routing**，驗證跨 instance 真的能跑

## 原始摘要

iThome（2026-07-30）對 MCP `2026-07-28` Release Candidate 的遷移分析。引用 GitHub MCP Server 遷移案例：移除 Redis-backed session 與配合 `initialize` 的資料庫存取，新的保證 header 讓部分 logging 與 secret scanning 不必再解析 payload，並用 Go SDK wrapper 同時支援新舊 elicitation flow。

> ⚠️ **作者明確警告不要照抄結論**：GitHub 的案例證明「大型 remote server 確實可以拿掉協定 session」，**不能證明每個 MCP server 都該刪 Redis**。你的 browser、task 或 workspace 若需跨 instance 存活，持久化儲存仍然有用。
> **真正該刪的是模糊的 ownership，不是某一種資料庫。**

結語：遷移完成後若團隊仍答不出某個 handle 歸誰、可以活多久、重送會不會做兩次，那就還沒真的完成 stateless migration——**只是把原本看得見的 Redis session，換成一張更難追的記憶體地圖**。

## 補充資料：規格定案 ≠ SDK 已跟上（2026-09-04 實測）

來源：〈自己做一個 MCP server:237 行，第一次跑就抓到我三個月前洩漏的東西〉（MikeWang，Medium，2026-09-04）
https://medium.com/@n913239/自己做一個-mcp-server-237-行-第一次跑就抓到我三個月前洩漏的東西-921040a17e7b

本條目第四步假設「新版 request 明確宣告版本與 capabilities、server 要實作 `server/discover`」。
作者用 **Python SDK 2.0.0** 寫了一台本機 stdio server，直接發原始 JSON-RPC 去問，結果打臉這個假設：

| 問了什麼 | 規格說 | 實際回什麼 |
|---|---|---|
| `{"method": "server/discover"}` | `2026-07-28` 寫明 server **MUST** 實作 | `-32601 Method not found`（**根本沒實作**） |
| `initialize` 帶 `protocolVersion: 2026-07-28` | 舊握手已移除 | 協商回 **`2025-11-25`**——**舊的有握手協定** |

🔴 **關鍵落差**：SDK 2.0.0 的常數明明寫著 `LATEST_PROTOCOL_VERSION = 2026-07-28`，
但它的高階 `MCPServer` 實際協商出來的是 `2025-11-25`。**規格定案五週，高階 API 還沒跟上。**

對本條目的三個實務修正：

1. **遷移排程不能只看規格發布日**。你的 server 用的是哪個協定，取決於 SDK 高階 API 而非規格版本號；
   把「規格定案」當成遷移起跑槍會誤判——第五步的 conformance gate 可能根本測不到你以為的那條 lifecycle。
2. **`server/discover` 短期內不能當成可依賴的能力探測手段**。第四步 protocol adapter 的版本判斷若假設
   對方會回應 `server/discover`，遇到 SDK 高階 API 建的 server 會直接拿 `Method not found`——
   adapter 需要把「不實作 discover」當成正常分支，而不是錯誤。
3. 🔑 **驗證方法論**：文件會過期，**連程式碼裡的常數都可能誤導你**。
   唯一能確定「現在到底跑的是哪個版本」的方法，是**對它發一個 request 看它回什麼**。
   這正是本條目「關掉 sticky routing 實測」精神的延伸——別信宣稱，去打它。

另一個與本條目呼應的觀察：同版新增的 **MRTR**（Multi Round-Trip Requests，server 回
`resultType: "input_required"` 由 client 補答案後**重送同一個 request**）之所以出現，
正是因為沒有 session、server 沒有一條屬於某 client 的連線可以主動推送，只能把流程反過來。
作者的推論值得記：**tool 寫得愈純（純輸入→輸出），愈不會被協定這類改動掃到**——
這次被重新設計的全是需要中途與使用者互動的 tool。這也是本條目「handle 要有明確 ownership」的另一種說法：
需要跨回合記住東西的 tool，才是遷移成本所在。

📌 **stdio vs HTTP 的影響範圍**：本機 stdio server 一開一關就是一個 process，本來就沒有 session 要維護，
所以受影響很小（呼應 B 層的「純本機 stdio 受影響較小」）；stateless 真正影響大的是**部署在 HTTP 後面的 server**。

## 相關連結

- [[self-built-mcp-server-237-lines]] — 本補充的來源：實測「規格寫的 vs 實際協商出來的」落差
- [[mcp-stateless-architecture]] — **同一次改版的另一面**：那篇講「規格改了什麼」（Tasks／MCP Apps／棄用政策／治理），本篇講「**遷移時你要負什麼責任**」
- [[mcp-fastmcp-server]] — FastMCP Server 實作
- [[production-agent-architecture]] — 生產級 20 模組；本篇的 **idempotency key** 正是其冪等模組在 MCP 場景的具體化
- [[graph-engineering-node-edge-state]] — Graph 的 **Structured State** 強調「Schema、版本與欄位 Owner」，與本篇「handle 要有明確 ownership」是同一原則
- [[ai-skills-mcp-plugins-agents-comparison]] — MCP 在四概念中的定位
