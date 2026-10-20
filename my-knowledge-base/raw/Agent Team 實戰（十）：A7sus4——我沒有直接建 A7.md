---
title: "Agent Team 實戰（十）：A7sus4——我沒有直接建 A7"
source: "https://emilwu.tw/articles/33-agent-team-a7sus4/"
author:
  - "[[EMil Wu]]"
published: 2026-05-01
created: 2026-09-02
description: "A7 的角色定義清楚後，我沒有直接建 A7，而是先建了 A7sus4：一個承擔 Role 3 production duty 的 production-precursor。這篇談 persistent identity、signal-density preflight、第一次 live audit 抓到的東西，以及我從 A7 發展出來的稽核 Agent 方法論。"
tags:
  - "clippings"
status: processed
---
![A7sus4 作為一個常駐稽核身份站在 Overmind process 與工作流旁邊](https://emilwu.tw/images/articles/33-agent-team-a7sus4/hero-light.webp)

A7sus4 不是 throwaway prototype，而是會承擔 production duty 的 production-precursor

> **一句話：** 我沒有直接建 A7，因為 Role 3 需要的不是更好的 monitor prompt，而是一個真正持久的稽核身份。

---

## A7sus4：我沒有直接建 A7

既然 A7 的 spec 已經清楚了，那…直接來？並沒有。

我先建了 A7sus4。

A7sus4 在和弦裡是 A7 的 suspended 4th，一個還沒解決、等著回到主和弦的狀態，所以 A7sus4 不是 throwaway prototype，也不是玩具，他是 production-precursor，意思是：他會先在 production duty 裡承擔 A7 的某一部分能力，等 A7 1.x 真的具備那個能力並且跑過實際 audit cycle 之後，他才退休。

為什麼要這樣做？

因為 Role 3，也就是 dispatched event monitor，已經有很明確的需求和資料，但完整 A7 的 Role 1 daemon 還卡在 long-running crash policy、rate-limit checkpoint、heartbeat、watchdog 這些基礎問題上，如果我硬要等 A7 全部想清楚才做，Role 3 會一直停在紙上，可是 Role 3 的問題已經在現場發生了。

更麻煩的是，Role 3 的原型一開始是用 Agent-tool ephemeral spawn 做的，也就是臨時叫一個 monitor subagent 出來看某個工作，結果連續幾次出現同一類問題：這個 monitor 很容易把自己理解成「等的人」。

他看到 prompt 裡有 `sleep N && stat` ，看到短時間內沒有明顯 filesystem signal，就進入一種 wait-mode，回覆裡開始出現 passive idiom，像「wait」、「let me do nothing and wait」、「watcher running, waiting for completion notification」，甚至有時候直接 spawn-and-exit，像是他把「監督」理解成「我等你們搞完再看」。

兄弟，這樣不對吧？

明明我們要的是一個 **主動** 稽核者，但 prompt 的形狀讓他變成一個 **等待者** ，而這不是寫一句「請你不要等」就能解決的事，因為根因不是文字，而是身份沒有錨點。

臨時 subagent 沒有自己的工作目錄，沒有自己的 CLAUDE.md，沒有自己的長期 state，沒有持續的 lifecycle，他只是一段 prompt 裡被叫出來的短暫角色，所以當任務訊號不夠密集時，他最穩定的心因模型就是：等。

A7sus4 的核心修正就是把這個角色變成 persistent process，放進 Overmind，讓 process 本身就是 Agent，讓他有自己的 `agent-A7sus4/` 、自己的 CLAUDE.md、自己的 audit-cycle skill、自己的 state、自己的 work-log，換句話說， **A7sus4 解決的不是「monitor prompt 怎麼寫」，而是「建立並且實驗一個持久身份」。**

---

<svg id="mermaid-mihvx64-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 1363.984375px;" viewBox="0 0 1363.984375 118" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-mihvx64-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-mihvx64-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-mihvx64-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-mihvx64-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-mihvx64-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-mihvx64-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g><path d="M268,59L272.167,59C276.333,59,284.667,59,292.333,59C300,59,307,59,310.5,59L314,59" id="L_F_P_0" style=";" data-edge="true" data-et="edge" data-id="L_F_P_0" data-points="W3sieCI6MjY4LCJ5Ijo1OX0seyJ4IjoyOTMsInkiOjU5fSx7IngiOjMxOCwieSI6NTl9XQ==" marker-end="url(#mermaid-mihvx64-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M516.672,59L520.839,59C525.005,59,533.339,59,541.005,59C548.672,59,555.672,59,559.172,59L562.672,59" id="L_P_S_0" style=";" data-edge="true" data-et="edge" data-id="L_P_S_0" data-points="W3sieCI6NTE2LjY3MTg3NSwieSI6NTl9LHsieCI6NTQxLjY3MTg3NSwieSI6NTl9LHsieCI6NTY2LjY3MTg3NSwieSI6NTl9XQ==" marker-end="url(#mermaid-mihvx64-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M826.672,59L830.839,59C835.005,59,843.339,59,851.005,59C858.672,59,865.672,59,869.172,59L872.672,59" id="L_S_H_0" style=";" data-edge="true" data-et="edge" data-id="L_S_H_0" data-points="W3sieCI6ODI2LjY3MTg3NSwieSI6NTl9LHsieCI6ODUxLjY3MTg3NSwieSI6NTl9LHsieCI6ODc2LjY3MTg3NSwieSI6NTl9XQ==" marker-end="url(#mermaid-mihvx64-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M1073.156,59L1077.323,59C1081.49,59,1089.823,59,1097.49,59C1105.156,59,1112.156,59,1115.656,59L1119.156,59" id="L_H_R_0" style=";" data-edge="true" data-et="edge" data-id="L_H_R_0" data-points="W3sieCI6MTA3My4xNTYyNSwieSI6NTl9LHsieCI6MTA5OC4xNTYyNSwieSI6NTl9LHsieCI6MTEyMy4xNTYyNSwieSI6NTl9XQ==" marker-end="url(#mermaid-mihvx64-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g><g data-id="L_F_P_0" transform="translate(0, 0)"></g></g><g><g data-id="L_P_S_0" transform="translate(0, 0)"></g></g><g><g data-id="L_S_H_0" transform="translate(0, 0)"></g></g><g><g data-id="L_H_R_0" transform="translate(0, 0)"></g></g></g><g><g id="flowchart-F-0" transform="translate(138, 59)"><rect style="" x="-130" y="-51" width="260" height="102" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -36)"><rect></rect><foreignObject width="200" height="72"><p>F-saa-1 N=3<br>ephemeral monitor wait-mode</p></foreignObject></g></g><g id="flowchart-P-1" transform="translate(417.3359375, 59)"><rect style="" x="-99.3359375" y="-39" width="198.671875" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-69.3359375, -24)"><rect></rect><foreignObject width="138.671875" height="48"><p>Persistent identity<br>Overmind process</p></foreignObject></g></g><g id="flowchart-S-3" transform="translate(696.671875, 59)"><rect style="" x="-130" y="-51" width="260" height="102" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -36)"><rect></rect><foreignObject width="200" height="72"><p>A7sus4<br>Role 3 production-precursor</p></foreignObject></g></g><g id="flowchart-H-5" transform="translate(974.9140625, 59)"><rect style="" x="-98.2421875" y="-39" width="196.484375" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-68.2421875, -24)"><rect></rect><foreignObject width="136.484375" height="48"><p>A7 1.x<br>Role 3 operational</p></foreignObject></g></g><g id="flowchart-R-7" transform="translate(1239.5703125, 59)"><rect style="" x="-116.4140625" y="-39" width="232.828125" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-86.4140625, -24)"><rect></rect><foreignObject width="172.828125" height="48"><p>Retire A7sus4<br>lessons inherit forward</p></foreignObject></g></g></g></g></g></svg>

A7sus4 的定位：不是 throwaway prototype，而是會承擔 production duty、累積 lessons、再交棒給 A7 的 production-precursor

## Signal density：不是所有任務都值得 live monitor

A7sus4 建出來後，另一個 Learning 是 signal-density precondition，一開始我想要 A7sus4 觀察所有任務，後來發現不行。

如果目標任務本身只是一個 GM-direct single-file edit，可能只有一個 file delta，沒有 dispatch archive、沒有 outbox、沒有 done JSON，這種任務拿去 live monitor，就像叫人坐在空房間裡聽海聲，然後叫他聽到一個大浪時紀錄下來，他當然會開始懷疑自己到底要做什麼。

所以 A7 Role 3 必須有 preflight：這個被觀察的 work unit 在 audit window 裡至少要有兩個以上預期 filesystem signals，比如 task archive、outbox、done JSON、primary artifact，才值得啟動 live monitor，如果低於這個 signal floor，A7 應該拒絕 spawn 或標記 degraded-confidence，好過硬上，這件事看起來雖然很小，但它是方法論上的轉折。

因為它承認了一件事： **不是每個工作都適合被同一種監督方式觀察，** 有些工作適合 live monitor，有些工作適合 post-hoc static audit，有些工作只需要 GM inspection，有些工作甚至只是人類看一眼就好。

這跟前面 dispatch mode 的判斷很像：不是選一個最強模式打天下，而是建立判斷標準，讓系統知道什麼情境該用什麼。

---

![一個稽核者坐在訊號很少的房間裡，旁邊是 task archive、outbox、done JSON 與 primary artifact 的訊號燈](https://emilwu.tw/images/articles/33-agent-team-a7sus4/signal-density-light.webp)

Signal density：不是每個工作都適合 live monitor，訊號太少時應該改用 static audit 或 GM inspection

## A7sus4 第一次真的抓到東西

A7sus4 scaffold 完、跑起來之後，他很快就不只是「看起來能跑」。

第一個 live audit cycle 是 PASS，三個 cycle、沒有 self-alert，驗證了 persistent monitor 的基本框架可行，而後面連續多次 audit 也產生了更細緻的訊號：有些是 PASS-with-deviation - 產出長度低於或高於 spec；有些是 PASS-with-anomaly - spec 本身不完整但 execution 滿足了列出的要求；也有 FAIL-silent-completion - stop-hook fired，但 primary artifact 沒有出現，這些都是 A7 要抓的那種「看起來完成了，但實際結果不在」的問題，同時，A7sus4 不只是判斷 pass/fail，他也開始把 deviation 分類。

如果一個任務沒有產出 artifact，那是 execution failure；如果 artifact 有產出，但 word cap 超過，是 deviation；如果 spec 說 6-section 但其實列了 7 個項目，Agent 滿足了 7 個列舉項，那問題不是 Agent 沒做好，而是 spec 寫得不一致；如果某個 wave 的長度一直偏離，但後來被修正，A7sus4 可以看到 streak broken。

這些都不是一般監控者會做的事，監控者通常會回報紅燈、黃燈、或綠燈，A7 回報的是：紅在哪？是執行錯誤、規格不完整、任務邊界模糊、還是系統性趨勢？需要現在修，還是進 team-review 變成方法論修正？

這就是我說 A7 是稽核者，不只是監控者的原因。

---

## 從 A7 發展出來的方法論

A7 的演化可以整理成一套建稽核 Agent 的方法論。

第一， **不要從角色名稱開始建，從失敗資料開始建。**

如果一開始說「我要做一個 auditing agent」，你很容易寫出一個抽象 spec：負責監控、負責驗證、負責報告，這些都沒錯，但離落地還有距離。A7 會變清楚落地，是因為累積了 silent failure、self-validation、SAA wait-mode、completion report 不足、skill drift、external-service footgun 這些實際案例，每一個案例都讓他多一個邊界、多一種判斷、多知道一件 Agent 不該做的事。

第二， **把 capability、lifecycle、trigger/output 分開。**

Log Audit / Cross-Agent Validation / Health Monitoring 是 capability，daemon / periodic 是 lifecycle，Role 1 / Role 2 / Role 3 是 operational mode，這三個軸一分開，設計就從「一個很大的 Agent」變成「一個可以分階段驗證、上線的系統」。

第三， **稽核者必須 observe-only。**

一旦 A7 直接修正被稽核的 Agent，他就失去了獨立性，除非你再建一個 A9 來稽核 A7，然後再建一個 A13 來稽核 A9，很快你就從輕快的曲風變成藍調爵士了的 Agent 疊疊樂了，所以 A7 的邊界要很清楚：observe、report、recommend，修正由 GM 和 Dm 做。

第四， **Live monitor 要有 signal-density preflight。**

不是所有任務都值得現場監督，訊號太少時，monitor 會變成等待者，等待者會開始編故事，或者直接退出，這不是 prompt engineering 可以完全解掉的問題，這是任務場景不適合那個監督模式。

第五， **可以先建 production-precursor，不要假裝 prototype 沒有生產責任。**

A7sus4 最漂亮的地方不是它先建了 Role 3，而是它承認自己不是 throwaway prototype，它會真的執行 audit duty，真的累積 lessons，真的有 retirement clause，等 A7 1.x 能接手且跑過 production audit cycle 才退場(我猜還要一個 A7sus2)，這比「先做個 prototype 看看」誠實得多，因為很多所謂 Prototype 最後都會活很久(把 Prototype 當做 Production 是另外一個問題，我們擇期再議)。

---

## A7 想解決哪些事？

如果把 A7 用更落地的語言收斂，它想解決的是下面這五件事。

**第一，讓 silent failure 變成 visible failure。**

hook 沒觸發、inbox 沒產生、chain 沒接上、primary artifact 沒出現，A7 要把「沒有發生的事」變成可以被看見的訊號，因為看到做錯相對簡單，但是要看到原本該看到、卻沒看到的東西，有點難。

**第二，讓自我驗證變成跨 context 驗證。**

Dm 不應該永遠只由 Dm 驗證，C7 不應該永遠只由 C7 驗證，Em 不應該永遠只由 Em lint，至少在關鍵方法論、Agent scaffold、skill contract、wiki consistency 這些地方，需要有另一個 Agent 帶著另外一組 context 回頭看。

**第三，讓團隊退化有趨勢資料。**

跟第一項說的一樣，單次任務失敗容易看見，但慢慢變差很難看見，A7 要看這些慢慢的變化，correction 越來越多、completion 越來越薄、dispatch 越來越常重試、skill copy 越來越舊，這些趨勢是一個應該要注意的訊號。

**第四，讓 GM 不用把所有監督責任扛在 context 裡。**

GM 還是決策者，但 GM 不應該是唯一的檢查器。當 GM 既要對話、又要派工、又要讀產出、又要稽核趨勢，他的 context 會滿、會漏東西，而 A7 就是負責把觀察責任 artifact 化 讓 GM 做判斷，而不是搶著取代 GM 的眼睛。

**第五，讓方法論可以從操作現場回流。**

A7sus4 的 audit-history 裡每一次 PASS-with-deviation、每一次 anomaly、每一個 design lesson range，最後都會回到 Em wiki、Dm plan、A7 baseline dataset，這樣 Agent Team 的改進才不是靠人類記憶，而是靠現場的操作不斷把問題轉換成知識、然後變成機制。

---

![大量低階 log 被整理成 anomaly、finding 與正確 channel，GM 和人類站在整理後的面板前判斷](https://emilwu.tw/images/articles/33-agent-team-a7sus4/visibility-light.webp)

A7 不是把人移出迴路，而是把雜訊整理成可以判斷的入口

## 不是把人移出 Loop，而是讓人看對地方

A7 最容易被誤解的地方，是它看起來像是把人類從 Human in the loop 裡移出去。

但其實剛好相反。

沒有 A7 的時候，人類要看所有東西，但是當所有東西混在一起，人類看起來很有掌控感，但其實只是被大量雜訊淹沒，所以 A7 的目標不是替人做判斷，而是讓人類不用在雜訊裡找訊號。

他把低階事件整理成 anomaly，把 anomaly 變成 finding，把 finding 送到正確的 channel：live bug 進 team-status，方法論洞察進 Em wiki，特定 dispatch 的觀察留在 work-log，這樣 GM 和人類看到的不是一片 log 海或 Finding 海，沒有厚度跟重要性，而是一組有分類、有證據、有來源的判斷入口。

所以 A7 不是「監督自動化」，而是「監督結構化」。

這其實也回到整個 Agent Team 系列最早的命題：我建的不是一群會自動做事的 Agent，而是一套讓 AI 協作可以被理解、被交接、被檢查、被修正的工作系統。

A7 是這套系統裡相對後期才長出來的 Agent，因為他需要前面所有東西先存在：要有 Agent 身份，才有身份載入錯誤；要有 dispatch，才有 chain break；要有 hook，才有 silent failure；要有 wiki，才有 knowledge drift；要有 Dm/C7/Em/G7 各自的產出，才有 cross-agent validation 的必要。

也因此 A7 不是 Agent Team 的起點更不是終點，而是 Agent Team 跨過 1.0、開始成熟後、才會需要的眼睛，因為一個真正會運作的 AI 團隊，不是看它能不能把任務做完，而是看它出錯的時候，系統裡有沒有人、或有沒有一個角色，能夠說出「這裡怪怪的，而且我知道怪在哪裡」，我相信 AI 的自動化，各個大廠都會有自己的答案卷，OpenAI 最近也持續把 Agents SDK 往 handoff、tracing、sandbox execution 這些生產化基礎設施推進，但找到中間的邏輯跟漏洞，你才能知道套用這些框架時，你該怎麼依照你的需求調整。

> **一個 Agent Team 的成熟，不是它不再犯錯，而是它開始有能力看見自己的錯。**

而 A7，就是我為這件事留出來的位置。

---

**參考資料**

<sup>[1]</sup> OpenAI — [The next evolution of the Agents SDK](https://openai.com/index/the-next-evolution-of-the-agents-sdk/) （2026-04-15；支持 Agents SDK 往 harness、sandbox execution、state rehydration 等生產基礎設施推進）

<sup>[2]</sup> OpenAI Agents SDK — [Handoffs](https://openai.github.io/openai-agents-python/handoffs/) （官方文件；handoff 的 context / guardrail 邊界，支持「chain 不是有 handoff 就自然安全」）

<sup>[3]</sup> LangChain — [AI Agent Observability: Tracing, Testing, and Improving Agents](https://www.langchain.com/articles/agent-observability) （2026；支持「single trace 不夠，production agent 需要 thread-level / multi-turn evaluation 與 continuous improvement loop」）

<sup>[4]</sup> arXiv — [TraceSIR: A Multi-Agent Framework for Structured Analysis and Reporting of Agentic Execution Traces](https://arxiv.org/abs/2603.00623) （2026-02-28；支持「raw trace 太長且難以直接診斷，需要結構化 trace analysis / report」）

<sup>[5]</sup> arXiv — [AgentCollab: A Self-Evaluation-Driven Collaboration Paradigm for Efficient LLM Agents](https://arxiv.org/abs/2603.26034) （2026-03-27；作為反面補充：self-evaluation 可作為 escalation signal，但不等於可以取代 cross-context audit）

<sup>[6]</sup> CrewAI Docs — [Observability Overview](https://docs.crewai.com/en/observability/overview) （官方文件；支持 production agent 需要 monitoring、evaluation、debugging、cost 與 degradation tracking）