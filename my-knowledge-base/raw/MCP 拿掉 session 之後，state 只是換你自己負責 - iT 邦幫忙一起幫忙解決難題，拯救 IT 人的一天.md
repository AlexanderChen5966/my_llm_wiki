---
title: "MCP 拿掉 session 之後，state 只是換你自己負責 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400585"
author:
  - "[[iThome]]"
published:
created: 2026-07-30
description: "看到 MCP 要改成 stateless，工程師很容易冒出一個直覺：太好了，session store 可以刪了。 先別急。 目前公開的 MCP 2026-07-28 release candidat..."
tags:
  - "clippings"
status: processed
---
2

## MCP 拿掉 session 之後，state 只是換你自己負責

- 分享至

看到 MCP 要改成 stateless，工程師很容易冒出一個直覺：太好了，session store 可以刪了。

先別急。

目前公開的 MCP `2026-07-28` release candidate，確實拿掉了 `initialize` 、 `initialized` 、 `Mcp-Session-Id` ，也不再要求 sticky routing 與協定層的 shared session storage。每個 request 要帶足協定版本、client identity 和 capabilities，伺服器不能再靠「前一個 request 已經講過」來補齊上下文。

但 browser 還是開著，長任務還是會跑十分鐘，購物籃裡也還是有東西。這些 state 沒有消失。MCP 只是停止替應用把它們藏在 transport session 裡。

這個差別很小，實作責任卻差很多。

## 先找出你原本藏在 session 裡的東西

我會先做一次很不浪漫的盤點，把所有「只要同一條 session 還在就沒問題」的假設找出來。

常見的藏身處包括：

- `initialize` 時順便建立的使用者或 workspace context
- 綁在單一 server instance 記憶體裡的 browser、task、cache
- 依賴 `Mcp-Session-Id` 查詢的 Redis 資料
- 只有原本那台 instance 看得到的訂閱與進度
- 把 SSE reconnect 當成原 request 繼續執行的程式

最容易漏掉的是 initialization side effect。很多 server 表面上只是在 handshake 交換能力，實際上同時載入憑證、選 tenant、建立暫存目錄，甚至啟動 browser。拿掉 handshake 後，這些動作不會自動找到新家。

盤點時不要只搜尋 `session` 這個字。去看 load balancer 為什麼需要黏住流量、process restart 後哪些工作會不見、哪一段程式會假設「這個 client 剛才已經驗證過」。那些才是真正的 state。

## Explicit handle 不是換一個 session ID

新版設計鼓勵 stateful application 回傳明確的 handle，例如 `browser_id` 、 `basket_id` 或 task handle，再由後續 tool call 當成普通參數帶回來。

一個簡化的 browser workflow 可以長這樣：

```markdown
browser.open()
  -> { browser_id, expires_at }

browser.navigate(browser_id, url)
  -> { current_url, title }

browser.close(browser_id)
  -> { closed: true }
```

如果實作只把 `Mcp-Session-Id` 改名成 `browser_id` ，其他事情完全不管，系統只是換了欄位名稱。

Handle 應該是一份 domain contract。至少要回答：

1. 誰可以建立它？
2. 它屬於哪個 user、tenant 或 workspace？
3. 能存取哪些操作，能不能轉交？
4. 多久失效，誰可以提前撤銷？
5. 過期、已關閉或跨 tenant 使用時，各自回什麼錯誤？

其中授權最容易被做反。不要先用 `browser_id` 找到 browser，再假設拿得到 ID 的人就有權限操作。應該先用目前 request 的 identity 與 tenant 驗證 handle ownership，通過後才載入資源。

Handle 也不適合直接帶出可猜測的內部流水號。即使使用隨機值，log、錯誤訊息和 tracing 系統仍可能把它留下來。比較安全的做法是記錄 handle type、tenant、lifecycle transition 與不可逆的識別摘要，不要把可重放的原值灑進每一層 log。

## Stateless request 會把重試問題推到檯面上

Release candidate 對中斷後的行為說得很直接：response stream 壞掉，client 會用新的 request 重試。這時 server 不能再靠原本那條連線判斷「這是同一次操作」。

讀取 `browser.current_page(browser_id)` 通常可以重送。 `order.submit(basket_id)` 、 `email.send(draft_id)` 或 `task.start(workspace_id)` 就沒那麼單純。Client 沒收到 response，不代表 server 沒完成副作用。

我會把 tool 分成兩類處理：

- 天生可重試的讀取操作，讓相同輸入得到相容結果。
- 會產生副作用的操作，要求 idempotency key，並保存足夠久的執行結果或明確拒絕重複呼叫。

第二類還要定義失敗語意。假設第一次 `order.submit` 已成功，但回應途中斷線，第二次帶同一把 idempotency key 進來，server 應該回傳原結果，而不是再送一次訂單。若 key 已過保存期限，也別假裝從沒看過，否則重複副作用只是時間問題。

這些規則原本可能被 session 與連線壽命遮住。協定變成 stateless 後，它們終於必須寫進 application contract。

## 新舊協定相容，放在薄薄的一層

這次變更不是只刪幾個 header。新版 request 會明確宣告版本與 capabilities，server 也要實作 `server/discover` ；舊版則仍有 initialization lifecycle。若產品需要同時服務兩個世代，最好把差異收在 protocol adapter。

```markdown
legacy MCP request ----\
                        -> protocol adapter -> domain service -> state store
stateless MCP request --/
```

Adapter 負責版本判斷、欄位轉換與舊版 session 對應。Domain service 只認明確的 identity、capability 與 handle，不應知道這次 request 曾經走過 `initialize` 。

這條線如果沒切乾淨，legacy session 很快會滲回核心邏輯。最後大家嘴上說 stateless，domain service 還是偷偷讀取某個 process-local map，failover 一來就露餡。

GitHub MCP Server 的遷移很值得看，但不要照抄結論。GitHub 移除了 Redis-backed session，以及配合 `initialize` 與後續呼叫所需的資料庫存取；新的保證 header 也讓部分 logging 與 secret scanning 不必再解析 payload。他們還用 Go SDK wrapper 同時支援新舊 elicitation flow。

這能證明大型 remote server 確實可以拿掉協定 session，不能證明每個 MCP server 都該刪 Redis。你的 browser、task 或 workspace 若需要跨 instance 存活，持久化儲存仍然有用。真正該刪的是模糊的 ownership，不是某一種資料庫。

## Conformance 通過後，還有一半要自己測

MCP conformance suite 可以分別測 client 與 server，也能指定 `2026-07-28` 或 draft suite。它會區分 legacy initialization lifecycle 與新的 stateless lifecycle，很適合放進遷移 gate。

但 conformance 驗的是協定行為，不會替你證明 `browser_id` 沒有跨 tenant 洩漏。

應用層至少還要補這些情境：

1. 同一個副作用 request 被送兩次。
2. Server 完成工作後，response 在途中遺失。
3. Handle 過期、撤銷或已關閉。
4. 另一個 tenant 拿到 handle 後嘗試操作。
5. 執行中的 task 遇到 instance failover。
6. Legacy client 與新版 client 同時連入。

測試時也要故意把 sticky routing 關掉。若只有同一台 instance 連續收到 request 才會成功，那套實作仍然依賴隱藏 session，只是基礎設施暫時幫你把問題蓋住。

## 無狀態真正換來的是責任邊界

協定層 stateless 會讓 state 顯形。它未必變少，卻開始有名字。

Browser 有 `browser_id` ，task 有自己的 lifecycle，workspace 有 tenant owner。建立、使用、失效、撤銷和重試都能放進明確的 contract，也比較容易跨 instance 路由與追蹤。

反過來說，如果遷移完成後，團隊仍答不出某個 handle 歸誰、可以活多久、重送會不會做兩次，那就還沒真的完成 stateless migration。你只是把原本看得見的 Redis session，換成一張更難追的記憶體地圖。

MCP 拿掉 session，是協定替應用清出一條界線。State 沒有被刪掉，它只是終於不能再躲了。

## Source notes

- [The 2026-07-28 MCP Specification Release Candidate](https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/)
- [MCP draft specification: versioning](https://modelcontextprotocol.io/specification/draft/basic/versioning)
- [MCP conformance suite](https://github.com/modelcontextprotocol/conformance)
- [GitHub MCP Server supports the next MCP specification](https://github.blog/changelog/2026-07-23-github-mcp-server-supports-the-next-mcp-specification/)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1784885238_6a632ff62c5c0.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9004)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/11/7a3e83f3-714c-4860-917a-a58a8f683d8a_small.png)

[2021 Q4 - Progress MFT 安全檔案傳輸管理軟體 - MOVEit Transfer 培訓課程 (1)](https://itplus.ithome.com.tw/vod-page/511?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

EC NETWORKER |

128 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/14/bfb5bc28-183a-4440-a1ef-aee3c6a7ea5b_small.png)

[Aruba 安全網路架構保護公共資料](https://itplus.ithome.com.tw/vod-page/649?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/20/a63ccd8a-d1d0-4921-8edc-c94636c6899e_small.png)

[開發者的匠心](https://itplus.ithome.com.tw/vod-page/1393?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/10/31/82880d3a-c666-452d-93c6-c8d4c22ae9ca_small.png)

[從容應對量子時代：NetApp AI 驅動的現代化資料中心安全藍圖](https://itplus.ithome.com.tw/vod-page/1281?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣醫院資訊主管會議 |

36 分