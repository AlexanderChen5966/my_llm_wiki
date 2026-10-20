---
title: Claude Code 卡住時先看錯誤字樣分流：Claude 端、自己網路、電腦睡眠，還是額度用完
date: 2026-10-08
tags: [ai/claude, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10420348（Tim Wei〈Claude code 新手番外篇：Claude Code 卡住了？你知道 Claude Status 是什麼嗎？〉，「Claude Code 實戰手冊／新手篇」番外；頁面無發布日，圖片上傳 2026-10-03）
type: article
status: processed
asset_value: 36
---

> 🔴 **時效警示**：依 2026-10-03 官方文件與 Claude Code v2.1.287 撰寫，**錯誤字樣可能變更**，以畫面上實際看到的為準。

## T（核心論點）

Claude Code「卡住」很多時候是它在自動重試。**先看轉圈旁邊那行錯誤字樣判斷是誰的問題**，再決定是等、換模型，還是自己排查；不要一卡住就重送。

## B（適用場景）

**適用：** Claude Code 轉圈不動、跳出 API 錯誤；判斷要不要去看狀態頁。

**不適用 / 注意：**
- ⚠️ 作者自己 8/19–10/3 共 76 次重試**全是網路問題、0 次 529**，這是單人樣本，但提醒「卡住不一定是 Claude 的問題」。
- ⚠️ Claude Code 的 `/status` 看的是**帳號與計費**，不是服務狀態（見 [[claude-code-commands-reference]]、[[claude-code-billing-usage]]）。服務狀態要看 status.claude.com。
- 為何定位在 ideas：可重用的只有一張分流表，時效極高；庫內 [[agent-error-recovery-taxonomy]] 是自建 agent 的錯誤分類框架，T 不同，這裡是它「Transient Failure → Retry」的具體實例。之後累積到其他維運排查素材再整合。

## R（操作步驟）

### 1. 畫面上會出現什麼

- 暫時性錯誤預設**最多重試 10 次、間隔遞增**，顯示 `Retrying in Ns · attempt x/y`。
- 20 秒都沒回應、還沒開始重試時，先顯示 `Waiting for API response · will retry in … · check your network`。
- 剛卡住時標籤可能只寫 `API error`：**網路類原因第一次重試就會寫出來，其他錯誤要到第三次重試才換成具體原因**（官方）。前幾次重試只隔一兩秒，多等一下就有答案。

### 2. 分流表

| 畫面上的字 | 誰的問題 | 怎麼辦 |
|---|---|---|
| `500`、`529 Overloaded` | Claude 端 | 可以等，通常過一下就好；持續就看狀態頁 |
| `ENOTFOUND`、`ECONNRESET`、`Unable to connect` | 自己的網路、VPN、proxy | 偶爾斷線會自己接上；一直失敗才自己處理 |
| `Your computer went to sleep mid-response` | 電腦睡著 | 回一句 `continue`，從斷掉的地方接續 |
| `You've hit your session limit` | 額度用完 | 等到訊息寫的時間；**換模型沒用** |
| 網頁／Desktop 聊天的「unexpected capacity constraints」 | 太多人在用 | 官方說**不算當機，也不會出現在狀態頁**，過幾分鐘再試 |

模型特別忙時 Claude Code 會提示用 `/model` 換模型。

### 3. 確認之後

- **只有某個模型出事**：`/model` 換一個。官方說**容量按模型分開算**。
- **全部一起出事**：只能等；修好後在 Claude Code 打 `try again`，不用重貼長 prompt。
- **是自己這邊**：`curl -I https://api.anthropic.com` 確認連得到；有 VPN 先關掉；公司網路要走 proxy 的，先設好 `HTTPS_PROXY` 再開 Claude Code；交代完要離開座位就別讓電腦睡著。
- **狀態頁 status.claude.com**：claude.ai、Claude Code、Claude API、Cowork 各一條（Desktop／手機 App 歸在 claude.ai）；事件狀態 Investigating → Identified → Monitoring → Resolved，**看到 Monitoring 就可以再試**；右上角 Subscribe to Updates 可收 Email。近期事件日期：9/3、8/29、9/29（不展開）。

## C（關鍵行動）

- [ ] 卡住時先讀那行錯誤字樣，對照分流表，再決定等、換模型或自己排查
- [ ] 看到網路類錯誤先跑 `curl -I https://api.anthropic.com`、檢查 VPN 與 `HTTPS_PROXY`
- [ ] 要看服務狀態開 status.claude.com，不是打 `/status`

## 相關連結

- [[claude-code-commands-reference]] — `/status`、`/model` 等指令
- [[claude-code-billing-usage]] — 額度與 session limit
- [[agent-error-recovery-taxonomy]] — Transient Failure → Retry 的通用框架，本篇是具體實例
- [[agent-ux-status-transparency-maturity]] — 狀態應來自真實狀態（重試倒數、事件說明）
- [[claude-code-model-and-thinking-depth]] — 同系列 G7：`/model` 切換

> 本系列為「Claude Code 實戰手冊／新手篇」，並非鐵人賽；iT 邦側欄推薦影片廣告不收。
