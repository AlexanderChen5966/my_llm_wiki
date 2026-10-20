---
title: "Agent Team 實戰（番外篇）：從 OS 的 IPC 看 Agent 溝通——五層決策模型"
source: "https://emilwu.tw/articles/30-agent-team-os-ipc/"
author:
  - "[[EMil Wu]]"
published: 2026-04-20
created: 2026-09-02
description: "Agent 溝通不是新問題，OS 的 IPC 幾十年前就處理過了。Process = Agent、pipe = Subagent、file + flock = mailbox、context switch = TLB miss——連經典問題都一模一樣。五層決策模型（L0 Environment → L1 Transport → L2 Topology → L3 Protocol → L4 Content Contract）由下往上設計，每層限制下一層的選項。三個系統（Claude Teams / OpenClaw / A2A）在不同的 L0 環境下做了各自合理的選擇，但到了 L3-L4，OS 的框架開始不夠用了。"
tags:
  - "clippings"
status: processed
---
![OS 的 IPC 概念對應到 Agent 溝通架構](https://emilwu.tw/images/articles/30-agent-team-os-ipc/hero-light.webp)

Agent 溝通不是新問題——OS 的 IPC 幾十年前就處理過了

> **一句話：** Agent 溝通不是新問題，OS 的 IPC 幾十年前就處理過了，而且幾乎可以直接套用。 **提醒：** 這篇主要提了很多 OS 設計的概念，如果你不想看這麼技術的東西，直接看下一篇的實戰可能比較直覺

---

上週我前同事在公司的 Slack 寫了一篇文，分析了 Agent 之間的溝通架構，他把 Claude Code Agent Teams、OpenClaw、A2A Protocol 三個系統拆開來看，然後做了一件讓我眼睛一亮的事：他把整個分析框架建立在作業系統的 IPC（Inter-Process Communication）之上。

讀到一半的時候我停下來，因為我突然想起去年 11 月我也查過 OS 框架，當時在發展 Agent 的記憶跟溝通框架時覺得「這好像以前讀過，來查一下」，但查完沒有深入就忘了，直到看到前同事的文章才又接上這條線，他是本科系的，所以整個概念比我清楚很多，所以我請 Em 把這篇文章 ingest 進 wiki，看看能不能從裡面提取出一個可以用的分析框架。

Em ingest 完之後，wiki 裡多了一個概念頁面：agent-communication-layers.md，裡面整理出了一個五層模型，而這個模型可以用來檢視我自己的 Agent Team 溝通設計，這是後面要講的事（下一篇），但在那之前，讓我先把這個模型講清楚。

---

## Agent 溝通就是 OS 的 IPC

這不是比喻，是直接對應。

OS 裡的 process 之間怎麼傳遞資料？pipe、shared memory、message queue、file lock、signal，這些在學作業系統的時候都摸過（雖然那時候做夢也沒想到有一天會用來設計 AI Agent 的溝通架構），而在 Agent 系統裡，這些概念幾乎原封不動地重演了一遍：

| OS 概念 | Agent 對應 | 實例 |
| --- | --- | --- |
| Process | Agent | 每個 Claude Code teammate |
| OS kernel | Harness / Runtime | Claude Code、OpenClaw |
| fork() + exec() | spawn sub-agent | Agent tool、TeamCreate |
| pipe (stdin/stdout) | parent-child 委派 | Subagent 模式 |
| file + flock() | filesystem mailbox | Claude Code Agent Teams |
| TCP socket | WebSocket | OpenClaw Gateway |
| HTTP/RPC | JSON-RPC | A2A Protocol |
| shared memory | shared message pool | MetaGPT semantic layer |
| signal | hook events | TeammateIdle、Stop hook |
| process state | task lifecycle | pending → working → completed |
| permission 繼承 | permission mode 繼承 | Teammate 繼承 lead 的 permission mode |
| context switch | compaction + session summary | Agent 被喚醒時從 mailbox 重建 context |

連經典的 OS 問題都一模一樣，Scheduling、Memory isolation、Permission inheritance 這些在 Agent 系統裡各自有對應的挑戰，但最讓我有感覺的是 context switch：OS 的 process 被喚醒要從 register 重建狀態，Agent 被喚醒要從 mailbox 讀訊息重建 context，每次 session 開始的那幾秒鐘，Agent 在做的事情本質上就是 TLB miss，他必須把之前散落在各處的狀態重新載入，才能繼續工作，而這個重建的品質直接影響他接下來的判斷。

所以如果你正在設計 Agent 之間的溝通，不要從零開始，OS IPC 已經處理了幾十年，你可以直接站在巨人的肩膀上。

![OS 概念與 Agent 概念的一對一對應](https://emilwu.tw/images/articles/30-agent-team-os-ipc/os-agent-mapping-light.webp)

不是比喻，是直接對應——OS 幾十年的設計經驗可以直接站上去

---

## 五層決策模型

既然 Agent 溝通就是 IPC，那 IPC 的設計可以怎麼拆解？同事的文章整理出了一個五層模型，由下往上，每一層的選擇會限制下一層的選項：

```plaintext
L4  Content Contract    — 傳什麼（memory selection + compression）
L3  Protocol            — 什麼格式（message envelope、interaction pattern、task lifecycle）
L2  Topology            — 誰跟誰講（hierarchy / star / peer / pub-sub）
L1  Transport           — 怎麼傳（pipe / file / WebSocket / HTTP）
L0  Environment         — Agent 在哪（same process / machine / network / internet）
```
<svg id="mermaid-pi73n6x-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 229.5625px;" viewBox="0 0 229.5625 606" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-pi73n6x-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-pi73n6x-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-pi73n6x-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-pi73n6x-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-pi73n6x-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-pi73n6x-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g><path d="M114.781,86L114.781,90.167C114.781,94.333,114.781,102.667,114.781,110.333C114.781,118,114.781,125,114.781,128.5L114.781,132" id="L_L0_L1_0" style=";" data-edge="true" data-et="edge" data-id="L_L0_L1_0" data-points="W3sieCI6MTE0Ljc4MTI1LCJ5Ijo4Nn0seyJ4IjoxMTQuNzgxMjUsInkiOjExMX0seyJ4IjoxMTQuNzgxMjUsInkiOjEzNn1d" marker-end="url(#mermaid-pi73n6x-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M114.781,214L114.781,218.167C114.781,222.333,114.781,230.667,114.781,238.333C114.781,246,114.781,253,114.781,256.5L114.781,260" id="L_L1_L2_0" style=";" data-edge="true" data-et="edge" data-id="L_L1_L2_0" data-points="W3sieCI6MTE0Ljc4MTI1LCJ5IjoyMTR9LHsieCI6MTE0Ljc4MTI1LCJ5IjoyMzl9LHsieCI6MTE0Ljc4MTI1LCJ5IjoyNjR9XQ==" marker-end="url(#mermaid-pi73n6x-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M114.781,342L114.781,346.167C114.781,350.333,114.781,358.667,114.781,366.333C114.781,374,114.781,381,114.781,384.5L114.781,388" id="L_L2_L3_0" style=";" data-edge="true" data-et="edge" data-id="L_L2_L3_0" data-points="W3sieCI6MTE0Ljc4MTI1LCJ5IjozNDJ9LHsieCI6MTE0Ljc4MTI1LCJ5IjozNjd9LHsieCI6MTE0Ljc4MTI1LCJ5IjozOTJ9XQ==" marker-end="url(#mermaid-pi73n6x-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M114.781,470L114.781,474.167C114.781,478.333,114.781,486.667,114.781,494.333C114.781,502,114.781,509,114.781,512.5L114.781,516" id="L_L3_L4_0" style=";" data-edge="true" data-et="edge" data-id="L_L3_L4_0" data-points="W3sieCI6MTE0Ljc4MTI1LCJ5Ijo0NzB9LHsieCI6MTE0Ljc4MTI1LCJ5Ijo0OTV9LHsieCI6MTE0Ljc4MTI1LCJ5Ijo1MjB9XQ==" marker-end="url(#mermaid-pi73n6x-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g><g data-id="L_L0_L1_0" transform="translate(0, 0)"></g></g><g><g data-id="L_L1_L2_0" transform="translate(0, 0)"></g></g><g><g data-id="L_L2_L3_0" transform="translate(0, 0)"></g></g><g><g data-id="L_L3_L4_0" transform="translate(0, 0)"></g></g></g><g><g id="flowchart-L4-0" transform="translate(114.78125, 559)"><rect style="fill:#c67a50 !important" x="-106.78125" y="-39" width="213.5625" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-76.78125, -24)"><rect></rect><foreignObject width="153.5625" height="48"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>L4 Content Contract<br>傳什麼</p></span></div></foreignObject></g></g><g id="flowchart-L3-1" transform="translate(114.78125, 431)"><rect style="fill:#c67a50 !important" x="-73.140625" y="-39" width="146.28125" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-43.140625, -24)"><rect></rect><foreignObject width="86.28125" height="48"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>L3 Protocol<br>什麼格式</p></span></div></foreignObject></g></g><g id="flowchart-L2-2" transform="translate(114.78125, 303)"><rect style="fill:#6b8f71 !important" x="-76.8203125" y="-39" width="153.640625" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-46.8203125, -24)"><rect></rect><foreignObject width="93.640625" height="48"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>L2 Topology<br>誰跟誰講</p></span></div></foreignObject></g></g><g id="flowchart-L1-3" transform="translate(114.78125, 175)"><rect style="fill:#6b8f71 !important" x="-76.28125" y="-39" width="152.5625" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-46.28125, -24)"><rect></rect><foreignObject width="92.5625" height="48"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>L1 Transport<br>怎麼傳</p></span></div></foreignObject></g></g><g id="flowchart-L0-4" transform="translate(114.78125, 47)"><rect style="fill:#6b8f71 !important" x="-89.09375" y="-39" width="178.1875" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-59.09375, -24)"><rect></rect><foreignObject width="118.1875" height="48"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>L0 Environment<br>Agent 在哪</p></span></div></foreignObject></g></g></g></g></g></svg>

五層決策模型：由下往上設計，每層限制下一層——綠色（L0-L2）OS 框架完全適用，橘色（L3-L4）開始碰壁

最底層的 L0（Environment）通常不是你選的，是被 harness 決定的：Agent 跑在同一個 process 裡就用 function call（Smolagents、LangGraph），same machine 就用 file 或 pipe（Claude Code），跨網路就用 WebSocket 或 HTTP（OpenClaw、A2A），我的 Agent Team 全在同一台 Mac 上，所以 L0 = same machine，這直接決定了往上每一層的選項。

### L1 — Transport：怎麼傳？

| Transport | 延遲 | 耦合度 | 持久化 | Debug 友善度 |
| --- | --- | --- | --- | --- |
| pipe (stdin/stdout) | 最低 | 最高（parent-child） | 無 | 低 |
| file + flock() | 低 | 低（任意拓撲） | 有 | **最高** （ `cat inbox/` ） |
| WebSocket | 低 | 中（需 server） | 連線期間 | 中 |
| HTTP | 中 | 最低（stateless） | 無 | 中 |

越往左延遲越低但耦合越高，越往右越獨立但越慢，這跟 OS 選 IPC 機制的 trade-off curve 一模一樣。

Claude Code Agent Teams 選 file 是因為精準判斷了自己的 environment，所有 Agent 保證在 same machine，file I/O 比任何 message queue 都好 debug，直接 `cat` 那個 inbox JSON 就能看到發生什麼事，這個 debug 優勢在實際開發中價值很高（我在第五篇就體會過了，silent failure 的時候，能直接看 inbox 的內容省了很多猜測的時間）。

### L2 — Topology：誰跟誰講？

| 拓撲 | 控制方式 | 瓶頸 | 實例 |
| --- | --- | --- | --- |
| Hierarchy | Parent 完全控制 | Parent | Subagent 模式 |
| Star | 中央協調者 | 協調者 | OpenClaw Gateway、我的 GM |
| Peer | 無中央控制 | 無 | A2A Protocol |
| Pub/Sub | 事件驅動 | 無 | MetaGPT message pool |

一個重要的觀念： **transport 跟 topology 是解耦的，** WebSocket 不必然是 star，HTTP 不必然是 P2P，選 star 常常是出於營運簡化（不用每個 Agent 各自開 port），不是技術限制。

我的 Agent Team 用的是 **star（GM 做中央協調）+ limited peer** （Agent 之間可以透過 filesystem 直接溝通做核心工作），這個混合拓撲是從實際需求長出來的：GM 負責協調和判斷，但 Em 和 C7 做核心工作的時候不需要經過 GM 才能讀到彼此的檔案。

### L3 — Protocol：什麼格式？

Protocol 定義三件事：message envelope（寄件人、收件人、時間戳、message ID）、interaction pattern（fire-and-forget / request-response / streaming / 多輪對話）、task lifecycle（狀態機：submitted → working → input-required → completed / failed）。

**Protocol 的複雜度跟信任邊界的距離成正比：**

| 系統 | Protocol 厚度 | 原因 |
| --- | --- | --- |
| Claude Teams | 薄（JSON-in-JSON） | 內部、same machine、trusted |
| OpenClaw | 中（tool params + callback） | Same server、semi-trusted |
| A2A | 厚（JSON-RPC 2.0 + OAuth 2.0） | Open internet、untrusted |

內部用薄 protocol，對外用厚 protocol，同一套系統不同場景用不同厚度，這也跟 OS 的做法一致。

### L4 — Content Contract：傳什麼？

前四層只是把 bytes 送到，L4 決定對方能不能用。

Agent 之間傳的不是隨便一段文字，是經過 memory selection 和 compression 的結構化內容，你可以有完美的 transport，但如果 Agent A 傳了超大的 raw context dump，Agent B 的 context window 直接爆掉，推理品質斷崖式下降，溝通就失敗了。

**L4 是 Agent 溝通成敗的真正關鍵，** 用我前同事的說法，這是目前所有系統做得最粗糙的一層，之前在 Context Engineering 那篇講的 JIT loading、token budgets 這些原則，其實都應該延伸到 Agent 之間的溝通：你不會把所有東西塞進一個 Agent 的 Context 裡，你也不應該把所有東西塞進一則 Agent 間的訊息裡。

---

## 順便釐清一個常被搞混的東西：MCP vs A2A

這兩個常常被放在一起比較，但它們解決的是完全不同層面的問題：

| 維度 | MCP | A2A |
| --- | --- | --- |
| 方向 | **垂直** （agent → tool / data） | **水平** （agent → agent） |
| OS 類比 | System call | Cross-process IPC |
| 用途 | 讓 Agent 呼叫外部資料庫、API | 讓 Agent 跟另一個 Agent 協商、委派 |

兩者是 **互補的** ，不是競爭的，A2A 的官方 spec 裡明確說了這是 complementary protocols，一個 Agent 可以用 MCP 呼叫工具，同時用 A2A 跟另一個 Agent 溝通，在同一個工作流裡。

---

## 三系統比較：Claude Teams vs OpenClaw vs A2A

把三個系統攤開來用五層模型對照，每一層的設計選擇都很清楚：

| 維度 | Claude Teams | OpenClaw | A2A |
| --- | --- | --- | --- |
| L0 Environment | Same machine | Same server | Open internet |
| L1 Transport | File I/O | WebSocket | HTTP |
| L2 Topology | Star + peer | Hub | Peer |
| L3 Protocol | JSON-in-JSON（薄） | Tool params（中） | JSON-RPC 2.0（厚） |
| Discovery | config.json | Session key | Agent Card（well-known URL） |
| Task 狀態 | 3 狀態 | async + callback | 完整狀態機 |
| Streaming | 無 | reply-back loop | SSE + webhook |
| 權限控制 | 繼承 lead | role + scope + cap | OAuth 2.0 + Bearer |

三個系統不是「哪個比較好」的關係，而是「在不同的 L0 環境下做了合理的設計選擇」，Claude Teams 的 file I/O 在 same machine 的場景下是最好的選擇（好 debug、零基礎建設、持久化免費），但你不會用 file I/O 做跨網路的 Agent 溝通，那時候 A2A 的 HTTP + JSON-RPC 才是合理的。

---

## 帶著模型回頭看

有了五層模型之後，我做的第一件事是拿它來對照我自己的 Agent Team 設計。

L0 到 L2，我發現自己已經在不知不覺中，做了正確的選擇：same machine、file I/O、star + limited peer，這些都跟 OS 的最佳實踐一致，這些決定現在做事後驗證後確認方向是對的。

但到了 L3 和 L4，事情開始變得有趣了。

我的 Level 1 Completion Report 設計得很標準，有格式、有欄位、有 Flag 機制，但它從來沒有被真正使用過，我的 dispatch 機制有去有回，但回來的那條路只走了一半，五層模型幫我看到了這些 gap，但同時也讓我意識到， **OS 的框架在 L0-L2 完全適用，但到了 L3-L4，我的 Agent 跟 OS 的 process 有一個根本性的差異，這個差異讓 OS 的解法不能直接套用。**

![L0-L2 亮綠燈，L3-L4 亮紅燈](https://emilwu.tw/images/articles/30-agent-team-os-ipc/cliffhanger-light.webp)

底層做對了，但上層碰壁了——OS 的 process 長一樣，我的 Agent 不一樣

這個差異是什麼？我又做了什麼？在 Agent Team 狀況下又該怎麼拆解跟思考？到這，番外篇結束，下一篇讓我們回到 Agent Team 實戰繼續說。

---

**參考資料**

\[1\] arXiv — Solving Context Window Overflow in AI Agents（context window 對推理品質的影響） [https://arxiv.org/html/2511.22729v1](https://arxiv.org/html/2511.22729v1)

\[2\] AI Pace — Context Engineering: Mitigating Context Rot in AI Systems（「context 越大，模型可靠性越低」） [https://medium.com/ai-pace/context-engineering-mitigating-context-rot-in-ai-systems-21eb2c43dd18](https://medium.com/ai-pace/context-engineering-mitigating-context-rot-in-ai-systems-21eb2c43dd18)

\[3\] Anthropic — Effective Context Engineering for AI Agents [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)

\[4\] FINOS — Multi-Agent Trust Boundary Violations（scope violation 的級聯效應） [https://air-governance-framework.finos.org/risks/ri-28\_multi-agent-trust-boundary-violations.html](https://air-governance-framework.finos.org/risks/ri-28_multi-agent-trust-boundary-violations.html)

\[5\] Google — A2A Protocol Specification（complementary to MCP） [https://google.github.io/A2A/](https://google.github.io/A2A/)