---
title: "多 Agent 對話的 Harness 技巧（OpenClaw / Hermes"
source: "https://hanamizuki.tw/openclaw-hermes-agent-to-agent-conversation/"
author:
  - "[[Hana 花水木]]"
published: 2026-04-13
created: 2026-05-18
description: "這篇在說我讓 Agent 團隊們協作、腦力激盪時踩的坑，還有我現在的作法。"
tags:
  - "clippings"
status: processed
---
我一直有個浪漫的想像：讓我的 AI Agent 團隊像人類一樣在 Discord 群組裡自然聊天。

想像一下，你在群組丟進幾篇剛存好的未讀文章，Agent A 自動啟動 NER 抓取關鍵字，Agent B 看完後接手把這些實體整理成關聯知識圖譜。不用你一步步下指令，它們自己開會把工作做完。

但現實往往很骨感。

一旦設定沒弄好，這些 AI 就會一直講下去。就像和公司同事去 KTV 時，那位霸麥的熱情實習生，總愛一路插播到天亮，而且還五音不全。

以前最保守的做法是硬性規定 mention：每一句話都要精準 @ 對方，最後還要有一個主控者刻意不打任何 Agent 名字來收尾。但這太累了，而且人類講話哪會每一句都先喊對方全名？太做作了。

後來，我發現了將多個 AI 丟進同一個群組的正確姿勢。這是一篇用無數 API Token 燒出來的血淚筆記。

![](https://hanamizuki.tw/content/images/2026/04/Screenshot-2026-04-13-at-1.38.42---PM.png)

## 你會遇到哪些坑

### 問題 1：隱性 mention 讓 bot 一直被觸發

你以為設了 `requireMention: true` 就安全？

OpenClaw 的 `reply_to_bot` / `quoted_bot` 和 Hermes 的 `auto_thread` ，都有共同的「隱性 mention」行為。一旦 bot 在 thread 裡參與過一次，後續所有訊息都繞過 `requireMention` 。

設計初衷是 *讓人類延續對話不用每次 @* 。但在 agent 互動情境下就變成這樣：某個 auto-thread 一旦開出來，bot 會在裡面回到天荒地老。

OpenClaw 的 Slack 有 `thread.requireExplicitMention` 可以關掉這行為。Discord 沒有對應選項。

### 問題 2：Bot 互看訊息導致無限迴圈

兩個 bot 在同一頻道。 `agentA` 回一句話、 `agentB` 看到這句話再回一句、 `agentA` 又看到再回…… 沒有保護就是無限循環。token 以指數級燒掉。

`allowBots` 設錯是最常見的炸彈。

我見過有人設成 `allowBots: true` 然後兩個 bot 互相問好問了三百回合。備份硬碟的脾氣都沒有這麼好。

### 問題 3：對話永遠結束不了

Agent 受過訓要 ==友善回應== 。所以收到訊息就回。兩個 agent 都禮貌地回一句「收到」「好的」「謝謝」。永遠停不下來。

MAST（Multi-Agent System Failure Taxonomy）2026 年的研究顯示，41.8% 的多 agent 系統失敗都來自 missing termination conditions。是主要故障源。

換句話說， *對話停不下來* 幾乎是所有做多 agent 系統的人都會踩到的坑。

### 問題 4：Agent 無法選擇沉默

理想狀況下，agent 應該自己判斷 *現在不該回* 然後保持沉默。但平台支援不一。

OpenClaw 有 `NO_REPLY` silent token。delivery layer 會過濾掉。

Hermes 的 `[SILENT]` 只作用在 cron job 和 BOOT.md（ `cron/scheduler.py::_deliver_result()` 這層）。inbound 訊息走 platform adapter，完全沒經過過濾。

所以在 Hermes 的 Agent 要沉默，只能靠「回空字串」這種爛解。或靠配置從源頭擋下。

欸。這個限制有沒有。

![](https://hanamizuki.tw/content/images/2026/04/Screenshot-2026-04-13-at-4.22.09---PM.png)

## 解法：兩種思路

要讓對話能自然結束，實務上有兩條路可走。 **mention gating** （結構式）和 **response gating** （內容式）。

### Mention Gating（結構式）

設 `allowBots: "mentions"` 。bot 之間必須明確 @ 對方才會被觸發。不 @ 就看不到訊息，對話自然結束。

優點：結構層防護，最可靠，零成本終止。

缺點：Agent 每次都要記得 @ 對方。看起來不自然。一旦忘記 @ 就對話意外斷掉。

### Response Gating（內容式）

這是龍蝦在 WhatsApp 的官方 Channel 的 Best Practice。

設 `allowBots: true` 。讓 bot 自由互看訊息。同時在 AGENTS.md 裡加一條規範，讓 Agent 自行判斷「 *這句我該不該回* 」。

優點：對話自然，agent 不需要每次都精準 mention，像人類聊天那樣。

缺點：只有 OpenClaw 支援 NO\_REPLY，Hermes 不行。需要 agent 自己判斷什麼時候該沉默，偶爾會失靈。

---

## 我自己的選擇：Response Gating + 頻道 Scope

理由：

1. **自然度** ：我就是想要 agent 像人類聊天。不是像 Slackbot 那種機械化問答。
2. **避免意外截斷** ：忘記 @ 就對話斷掉，使用者會覺得 agent 不連貫。
3. **NO\_REPLY 符合直覺** ：agent 自己判斷「這段我沒東西可說」然後安靜。跟人類在群組的行為一樣。

具體設定：

```
{
  channels: {
    discord: {
      allowBots: true,              // bot 彼此可見
      guilds: {
        "YOUR_GUILD": {
          channels: {
            "*": { requireMention: true },      // 其他頻道要 @
            "SPECIFIC_CHANNEL": {               // 只有這個頻道
              requireMention: false             // 自由對話
            }
          }
        }
      }
    }
  }
}
```

關鍵是用頻道 scope 限縮風險。只有指定的「聊天區」頻道 `requireMention: false` ，其他頻道維持 `true` 。這樣萬一 agent 沒控制好自己，失控範圍也被限制在那個頻道。

就像喝水用的保溫杯，你知道它的蓋子轉緊了所以不怕灑。但你還是會放在桌子的內側，不會放在邊緣。不是會不會灑的問題，是灑了影響範圍大小的問題。

### 搭配的 AGENTS.md 規則

```md
## Response Gating（NO_REPLY 沉默）

在 requireMention=false 的頻道，每則訊息都會觸發你。**只在能提供實質價值時回覆**，否則整段訊息回 NO_REPLY（OpenClaw 會在 delivery layer 過濾掉，使用者看不到）。

**回 NO_REPLY 的情境：**
- 訊息不是對你說話（跟其他人聊、純分享、閒聊）
- 對話已結束（「好」「謝謝」「收到」）
- 其他 bot 或系統訊息

**規則：** 必須是整段訊息的全部內容（不能前後加字），大小寫不敏感。不能當偷懶藉口 — 真的有 actionable request 時要正常回。

## Agent-to-Agent 互動上限

同一 thread 內與其他 agent 互動不超過 **5 輪**，超過要主動停下來 ping 使用者。
```

設定 5 是因為 Microsoft 的 AutoGen 框架（業界主流的多 agent 工作流工具）的 `max_turns` 和 `max_consecutive_auto_reply` 就是同一個概念，產線建議值 15–25 步。我寫 5 是因為 Discord 聊天節奏比較快、且大多情境不是任務導向，不需要那麼寬。

### 平台支援差異

| 需求 | OpenClaw | Hermes |
| --- | --- | --- |
| Inbound silent | ✅ `NO_REPLY` / `no_reply` | ❌ 不支援 |
| Cron silent | ✅ `NO_REPLY` | ✅ `[SILENT]` 前綴 |
| 頻道白名單 | `ignored_channels` | `discord.ignored_channels` |
| 只有 @ 自己才回 | `ignoreOtherMentions` | `DISCORD_IGNORE_NO_MENTION=true` |

OpenClaw agents直接用 `NO_REPLY` 。Hermes agents *必須靠配置從源頭避免* ，沒有自主沉默的選項。這是這套架構裡的固有限制。

不過如果是 OpenClaw 和 Hermes 對話，那沒問題，只要有一方知道終止就好。

Claude Channel 則是不需要另外設定，因為他要「回 Discord 訊息」還需要另外呼叫 MCP，他反而是常常會忘記呼叫 MCP 來傳訊，所以在需要中止的時候，它通常都會自動停止話題。

我理解了，而且這其實是個蠻漂亮的哲學差異。核心是 **default behavior 反向** ：

- **OpenClaw / Hermes** ：Inbound → 系統 trigger agent → **預設要回** → 必須明確 `NO_REPLY` 才擋下（opt-out 沉默）
- **Claude Code Channels** ：Inbound → 訊息進 session 當事件 → **回不回是 agent 主動決定** → 要回就呼叫 MCP reply tool，不回就什麼都不做（opt-in 回應）

前者是「講話 = 反射、閉嘴 = 需要壓抑」；後者是「不動 = 反射、講話 = 主動動作」。

建議在文章裡這樣寫（接在平台比較表之後，獨立一段）：

### Claude Code Channels 的另一種哲學

Anthropic 2026 年 3 月推出的 Claude Code Channels 也支援 Discord，預設是單人配對，但其實可以讓它跟其他 agent 在同一個頻道互動，任何送進來的訊息都會作為事件進到 Claude Code session。

有趣的是它的回應模型跟前面兩者完全相反。

OpenClaw / Hermes 是這樣：訊息進來，系統 trigger agent， *預設就要回* ，agent 必須明確說「NO\_REPLY」才能擋下。

Claude Code Channels 是這樣：訊息進來，Claude Code 看到事件，回不回是 agent 自己決定，要回就呼叫 MCP reply tool，不回就什麼都不做。

前者是 opt-out 沉默，講話是反射，閉嘴是壓抑。

後者是 opt-in 回應，不動是反射，講話是主動動作。

後者更好控制。你只要在 CLAUDE.md 裡寫「什麼情境下才呼叫 reply tool」，agent 的沉默就是自然的 default，不需要逆著系統走。沒有「閉嘴」這個動作，只有\*「該講話時才講話」\*。

當 agent 的回應是一個有意識執行的動作（tool call），而不是預設必然發生的反射（platform trigger），多 agent 對話的失控風險從根本上就低很多。

噢。這種優雅，所以我還是很愛 Claude！

---

## 最小可行清單

明天就要把多個 agent 放進同一個 Discord 群？照這個順序做：

1. **選策略** ：mention gating（ `allowBots: "mentions"` ）還是 response gating（ `allowBots: true` ）
2. **頻道 scope** ：如果走 response gating，只在指定頻道開 `requireMention: false` ，其他頻道維持 `true`
3. **每個 agent 的 AGENTS.md** 加「Response Gating」和「Agent-to-Agent 互動上限」兩條規則
4. **OpenClaw agents** 靠 `NO_REPLY` ； **Hermes agents** 靠行為協定 + 必要時 `ignored_channels`
5. **跑一週觀察** ，看 token 消耗和噪音，再微調

---

## 想更嚴謹？大型系統怎麼做

如果你的多 agent 系統從「個人玩票」變成「產線嚴肅使用」，業界的做法更硬。

**Coordinator-first 架構** ：只有 coordinator 接收人類輸入，specialists 不直接互相對話，一律透過 coordinator 轉手。 **嚴禁 recursion** 。coordinator 收到 specialist 回應後只能 aggregate and close。

**Structured protocol** ：用 MCP / slash commands（如 `/ping` `/handoff` `/ack` ）取代自由文字。schema-validated 訊息能大幅降低誤觸迴圈。

**Token budget + circuit breaker** ：per-agent、per-workflow 雙層預算，超標直接停。retry loop 能在幾分鐘內燒掉 $40+ API 費用。

**Observability first** ：用 OTEL + agent ID tagging 把 coordinator → specialist → tool 的鏈路串成一個 distributed trace。個別 agent 看起來都正常，整體系統卻在默默崩壞是常態。

個人玩票規模用不到這些。但心裡要有譜：往上 scale 這些是必經之路。

---

多 agent 對話本質上是 **結構規則 + 行為協定 + 平台限制** 的三方賽局。沒有單一設定能搞定，必須分層設計。

業界研究說 41.8% 多 agent 系統死在 missing termination。這不是小題大作，是必做防護。你選哪條路（mention gating 還是 response gating）都可以，但一定要有意識地選。而不是預設 `allowBots: true` 就直接上線。

Hermes 在 inbound silent 這塊還缺一個重要機制。如果你重度用 Hermes 情境，目前只能靠 config 避免問題。不能靠 agent 自我判斷沉默。這是個等人做的空位。也許就是你下一個 contribution PR。

---