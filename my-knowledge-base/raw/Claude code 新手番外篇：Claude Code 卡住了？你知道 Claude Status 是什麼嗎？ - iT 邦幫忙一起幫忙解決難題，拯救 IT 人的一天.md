---
title: "Claude code 新手番外篇：Claude Code 卡住了？你知道 Claude Status 是什麼嗎？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10420348"
author:
  - "[[iThome]]"
published:
created: 2026-10-06
description: "在使用Claude code的時後突然卡住了？ 用 Claude Code 開發、修 bug 的時候，我大多開著 auto mode，交代完就去做別的事，不會一直盯著螢幕。 所以它卡住的時候，我通常不..."
tags:
  - "clippings"
status: processed
---


## Claude code 新手番外篇：Claude Code 卡住了？你知道 Claude Status 是什麼嗎？

- 分享至

## 在使用Claude code的時後突然卡住了？

用 Claude Code 開發、修 bug 的時候，我大多開著 auto mode，交代完就去做別的事，不會一直盯著螢幕。

所以它卡住的時候，我通常不會馬上發現。我以為它還在跑、快跑完了，回來一看，才發現它停在原地，轉圈一直轉，什麼都沒出來。

我會先重新送一次。它跑了一下，又卡在一樣的地方。這時我才會去看 Claude 官方的狀態頁，確認是不是 Claude 那邊出事了。

每次走到這一步，我最想知道的只有兩件事:**它到底怎麼了？我要繼續等，還是換個方式做？**

這兩個答案其實都找得到。Claude Code 卡住時，畫面上會寫它正在做什麼;Claude 官方的狀態頁，會寫現在哪裡出事、修到哪了。這篇就照卡住的順序走一遍:畫面上會出現什麼、怎麼確認是哪裡的問題、到狀態頁看最新情況、確認之後怎麼辦。

---

## 1\. 畫面上會出現什麼

Claude Code 遇到暫時性的錯誤，不會馬上跳出來告訴你，而是先自己重試，預設最多 10 次，每次等得比上一次久。你回來看到的「卡住」，很多時候就是它在重試。

重試的時候，轉圈旁邊會出現一個錯誤標籤，後面接著倒數:

```bash
Retrying in Ns · attempt x/y
```

N 是還要等幾秒，x/y 是現在第幾次、最多幾次。

![https://ithelp.ithome.com.tw/upload/images/20261003/20182796ngjXRgaZTB.png](https://ithelp.ithome.com.tw/upload/images/20261003/20182796ngjXRgaZTB.png)

如果 20 秒都沒收到任何回應，還沒開始重試，會先出現這一行:

```ruby
Waiting for API response · will retry in … · check your network
```

重試完還是失敗，才會跳出錯誤。最常見的是這三種:

```swift
API Error: 500 Internal server error. This is a server-side issue, usually temporary — try again in a moment. If it persists, check https://status.claude.com.
API Error: Repeated 529 Overloaded errors. The API is at capacity — this is usually temporary. Try again in a moment. If it persists, check https://status.claude.com.
Can't reach the API server — check your internet or DNS (ENOTFOUND)
```

某個模型特別忙的時候，Claude Code 還會主動提醒你換模型，例如 `Opus is experiencing high load, please use /model to switch to Sonnet` 。

**如果卡住的是 Claude Desktop 或網頁版的一般聊天** ，比較常看到的是這一句:

```markdown
Due to unexpected capacity constraints, Claude is unable to respond to your message. Please try again soon.
```

意思是現在太多人在用。官方說這不算當機， **也不會出現在狀態頁上** ，過幾分鐘再試就好。

---

## 2\. 怎麼確認是哪裡的問題

![https://ithelp.ithome.com.tw/upload/images/20261003/20182796fcNvjyF13n.png](https://ithelp.ithome.com.tw/upload/images/20261003/20182796fcNvjyF13n.png)

看那行字寫什麼，就知道是誰的問題:

| 畫面上的字 | 是誰的問題 | 等有沒有用 |
| --- | --- | --- |
| `500` 、 `529 Overloaded` | Claude 那邊 | 有，通常過一下就好 |
| `ENOTFOUND` 、 `ECONNRESET` 、 `Unable to connect` | 你這邊:網路、VPN、公司 proxy | 偶爾斷一下，它會自己重試接上;一直失敗才要自己處理 |
| `Your computer went to sleep mid-response` | 你這邊:電腦睡著了 | 回一句 `continue` ，從斷掉的地方接著做 |
| `You've hit your session limit` | 額度用完 | 等到訊息寫的時間;換模型沒用 |

剛卡住時，標籤可能只寫 `API error` 。官方文件說，網路斷了這類原因從第一次重試就會寫出來;其他錯誤要到第三次重試才換成具體原因。前幾次重試只隔一兩秒，所以多等一下就會看到答案。

不要以為卡住一定是 Claude 的問題。我翻了自己電腦的紀錄，8/19 到 10/3 一共 76 次重試，全部是網路問題，沒有一次是 529。

如果寫的是 500、529，或是你看不出來，下一步就是去看狀態頁。

---

## 3\. 到 Claude Status 看最新情況

打開 [status.claude.com](https://status.claude.com/) ，這是 Anthropic 官方的狀態頁。

![https://ithelp.ithome.com.tw/upload/images/20261003/20182796qeGwzgb7Km.png](https://ithelp.ithome.com.tw/upload/images/20261003/20182796qeGwzgb7Km.png)

看三個地方。

**每個服務一條。** claude.ai、Claude Code、Claude API、Cowork 各有一條，看得出是哪個產品出事。Desktop 和手機 App 沒有自己一條:9/29 那次，公告內文寫了 desktop 和手機 App，狀態頁上標的是 claude.ai 那一條。

**事件標題。** 標題通常會直接寫是哪個模型或哪個產品出事。我挑了最近三則(台灣時間):

| 日期 | 事件標題 | 代表什麼 |
| --- | --- | --- |
| 9/3 | Elevated errors for Claude Sonnet 5 | 只有 Sonnet 5 出事 |
| 8/29 | Elevated errors on Claude Code and Claude Cowork | 只有網頁版 Claude Code 和 Cowork 出事 |
| 9/29 | Elevated errors on claude.ai, Claude Code, Claude Cowork and the Claude API | 全部一起出事 |

**往下滑，看事件說明。** 頁面下方的 Past Incidents 會列出每則事件，每次更新都附一段說明，寫遇到什麼問題、哪些功能還不能用、你該怎麼做，最新的更新在最上面。以 9/29 那次為例，其中一則更新是這樣寫的:

> "A second issue is preventing many users from signing in…, starting new chats, voice conversations and Claude Code or Cowork sessions… If you're signed in, please don't sign out."
> 
> 另一個問題讓很多人無法登入、開新對話、使用語音對話，也開不了 Claude Code 或 Cowork 的 session……已經登入的人請不要登出。

![https://ithelp.ithome.com.tw/upload/images/20261003/201827960DLANG2KR3.png](https://ithelp.ithome.com.tw/upload/images/20261003/201827960DLANG2KR3.png)

更新會照這個順序:Investigating(調查中)→ Identified(找到原因)→ Monitoring(修好了，觀察中)→ Resolved(解決)。看到 Monitoring，代表修正已經上線，可以再試一次。

---

## 4\. 確認之後怎麼辦

**只有某個模型出事:換模型。** 在 Claude Code 打 `/model` 換一個。官方說容量是每個模型分開算的，所以換一個就能繼續做。Desktop 和網頁版的聊天，點送出按鈕旁邊的模型名稱就能換。

**全部一起出事:只能等。** 原本的訊息還在對話裡，修好之後長 prompt 不用重貼，在 Claude Code 打 `try again` 就好。如果連登入都出問題，照公告的說明做，像 9/29 那次就是請大家先別登出。

**是你這邊:重試到最後還是失敗，就別再等了。** 先在終端機跑這一行:

```bash
curl -I https://api.anthropic.com
```

有跑出回應，代表你的電腦連得到 Claude 的伺服器。有開 VPN 的話先關掉再試;公司網路要透過 proxy 才能上網的，要先設定好 `HTTPS_PROXY` ，再打開 Claude Code。跟我一樣交代完就離開座位的話，記得別讓電腦睡著。

**不想每次都自己去查:訂閱。** 狀態頁右上角的 Subscribe to Updates，出事、修好都會寄 Email 給你。

> 小提醒:Claude Code 裡的 `/status` 指令，看的是你的帳號和計費方式(實戰手冊 G2 講過)，不是服務有沒有出事。要看服務狀態，請開 status.claude.com。

---

## 結語

下次 Claude Code 卡住，先別急著重送。看一眼轉圈旁邊那行字，判斷是誰的問題;是 Claude 那邊，就到狀態頁看是哪個模型、哪個產品出事，能換模型就換，不能換就等它修好。

你用 Claude Code 最常卡在哪？留言告訴我，下一篇卡關筆記可能就寫它。

**參考資料**

- Claude Code 官方〈Error reference〉:https://code.claude.com/docs/en/errors
- Claude 說明中心〈Troubleshoot Claude error messages〉:https://support.claude.com/en/articles/12466728-troubleshoot-claude-error-messages
- Claude 官方狀態頁:https://status.claude.com

> 本文依 2026-10-03 的官方文件與 Claude Code v2.1.287 撰寫。錯誤訊息的文字常常改，以你畫面上看到的為準。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1789123100_6aa3da1cbf61d.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/14/f565b41a-0f14-4861-b56f-0e5b0d044bb5_small.jpg)

[小孩子才做選擇：又廣、又深、又快的網路分析，誰說不能全都要？](https://itplus.ithome.com.tw/vod-page/374?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/25/38614962-bf87-4e91-8846-8bfa557b9227_small.png)

[【Bootcamp Workshop】初探 LLM 可觀測性：從概念到實踐](https://itplus.ithome.com.tw/vod-page/1149?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

57 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/c3b8f442-3e18-4404-b7e7-abbf11207ada_small.png)

[資安的下一個戰場：從網路邊界發起的攻擊鏈演變與防禦之道](https://itplus.ithome.com.tw/vod-page/1241?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/9/29/441ba86d-de5a-4d7b-96ce-e5a4f1fdf78c_small.png)

[地端 K8s 叢集的自主運維實踐：結合自動自愈與限制性 LLM 轉發的巡檢 Agent 架構](https://itplus.ithome.com.tw/vod-page/1519?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

30 分