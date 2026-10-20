---
title: "Agent Team 實戰（九）：A7——不是再多一個 Agent，而是讓團隊開始被看見"
source: "https://emilwu.tw/articles/32-agent-team-a7/"
author:
  - "[[EMil Wu]]"
published: 2026-05-01
created: 2026-09-02
description: "A7 一開始只是 validation gap 的未來解法，後來在 silent failure、chain debug ownership、team drift 這些事故裡，長成一個真正的 Auditing Agent。這篇先談 A7 為什麼需要存在：它不是再多一個執行者，而是讓 Agent Team 開始看見自己的錯。"
tags:
  - "clippings"
status: processed
---
![Agent A7 站在 Agent Team 的工作流旁，觀察 dispatch、inbox、hook 與 audit log](https://emilwu.tw/images/articles/32-agent-team-a7/hero-light.webp)

A7 不是多一個執行者，而是讓團隊開始被看見的稽核者

> **一句話：** A7 不是再多一個 Agent，而是讓 Agent Team 開始有能力看見自己的錯。

---

前一篇說到 OS IPC 能教我什麼、不能教我什麼，講到最後有一個問題被留在檯面上：如果 Agent Team 裡每個 Agent 都是異構的 specialist、產出都不一樣，這表示 completion report 不能真正代表結果本身，那麼，誰來判斷這個團隊有沒有在正常運作？

在一開始，我以為 GM 可以做這件事，GM 是唯一的人類接口、是 coordinator、也是整個 Agent Team 的中樞，當 Em 做完 ingest，GM 去看 wiki，當 Dm scaffold 完一個新 Agent，GM 去看目錄結構，當 C7 provision 完 skill，GM 去看 catalog，這就是當初我期望 GM 進行的事情，同時也是我之前的工作模式，GM 就是依照這些模式建立的，GM 發派任務，Agent 在自己的 context 裡執行，GM 再用獨立 context 回頭檢查，相當符合 cross-audit principle。

延續之前在 Agent 建立時的討論與概念，Agent 最好由另外一個 Agent 稽核，但我一直在思考這個 Auditing Agent 應該長什麼樣子，我做過 ERP 的 Governance agent，也經手過 Website 的 QA-Agent，他們各自有不同的長相，那 Agent Team 的 Auditing Agent 應該又是什麼樣子？我也沒有答案，所以我想先用 GM 作為資料蒐集者，來看看會怎麼發展，而進行一陣子之後，我開始發現一些 Pattern 跟 Pain Point，比如 GM 可以檢查一個結果，檢查一條 pipeline，甚至可以在幾個 Agent 之間做 sense-making，但 GM 無法一直盯著所有事情、在每次 hook silent fail 的當下就發現，他也無法在每個 dispatch 的五分鐘後自動回頭看 inbox，在每個 skill 被覆寫時立刻判斷那是不是破壞了 catalog 的 read-only contract，更無法在整個團隊的 correction 越堆越多時問自己：這是不是某個 Agent 開始退化了？

這看起來似乎是 Auditing Agent 進場的時間了，而且這些問題並不是單純的監控或是稽核，而是「當一個 Agent Team 開始像團隊一樣運作時，哪些事應該開始獨立，而不是仰賴人類和 GM 順手做？」

---

## A7 最早不是監控器，而是一個 validation gap

A7 最早出現的時候，名字還沒有那麼明確，他只是 Em wiki 裡一個 future validation agent 的概念，原因也很簡單：所有 Agent 都在自我驗證。

Dm 建 Agent，然後 Dm 自己用 validator subagent 檢查 scaffold；C7 管 skill catalog，然後 C7 自己跑 `/validate-skill` 檢查 skill；Em 管 wiki，然後 Em 自己跑 `/lint` 檢查 wiki，大家都有驗證，但用 cross-audit principle 的角度來看， **產出和驗證標準都在同一個 context 裡，Agent 當然會覺得自己的產出符合自己的標準，** 因為他本來就是用同一套框架產出、也用同一套框架檢查。

這不是說 Agent 偷懶，也不是說他不可靠，而是模型的結構限制，你要一個模型檢查自己剛剛推理完並生成的東西，它只會看見一致性、看不見盲點，因為這是個雞生蛋、蛋生雞的難題，那個盲點本來就是讓他生成錯誤的原因，如果他看得見，就不會生成錯誤。

所以一開始我只是很簡單的希望 A7 做一個任務：專門做 cross-agent validation 的 L2 agent。

但我沒有立刻創建他，原因就是上面說的：團隊還小，我自己就是 cross-auditor，GM 的 context 跟其他 Agent 的 context 也有天然隔離，這件事暫時還不需要一個 Agent，先看看 Pattern 跟 Scenario，記錄整個過程再來歸納設計就好。

「先記錄，不急著建」這個邏輯其實蠻重要的，因為如果我在那個時候就把 A7 建出來，大概只會是一個很普通、標準的 QA Agent：讀結果、打分數、寫報告，看起來很完整，但不會解決真正的問題。

---

## 真正推動 A7 成形的不是需求，是事故

Agent Team 跑久了之後，問題開始變得具體。

![Agent Team 的工作流看起來完成，但 inbox、hook 與下一個 task 的燈沒有亮起](https://emilwu.tw/images/articles/32-agent-team-a7/silent-failure-light.webp)

silent failure 最麻煩的地方不是紅燈，而是原本該亮的燈沒有亮

第一類是 silent failure。

在 dispatch 那篇我提過各種的 silent failure，hook 格式錯誤、從錯誤目錄啟動、stop-hook 沒有寫 inbox 等等，因為任務看起來完成、Agent 有回覆、檔案也有改、session 也正常結束，一切都正常，但是後續事件沒有發生，這種錯誤很麻煩，因為它不是「紅燈」，它是「燈沒亮」，而且你還要先知道這個燈應該要亮才會發現問題。

第二類是 chain debug ownership。

當一條 pipeline 是 Em → C7 → Dm 這樣跑的時候，如果 Em 完成了但 C7 沒被觸發，這到底是 Em 的錯、C7 的錯、GM 的錯、hook 的錯、還是 task file schema 的錯？每個 Agent 都只看到自己的局部，GM 可以回頭查，但 GM 並沒有被設計成守著每條工作鏈，所以這裡出現了一個「沒有人負責的區塊」。

第三類是團隊退化。

一個 Agent Team 不會只在單次任務裡壞掉，他是慢慢壞掉的，completion report 越來越稀疏，某個 Agent 的 correction 越堆越多，某個 skill copy 跟 catalog master 越差越遠，某個 external service 的錯誤訊號反覆出現但還沒有嚴重到讓人注意，這些 operational drift 並不是會讓人警醒的 bug。

所以，這些問題加起來，A7 的角色開始從「驗證者」變成「稽核者」，超過原本的「監控者」的 Scope，「監控者」聽起來像是看 process、看 log，紅了就叫你，這當然是他的一部分工作，但 A7 要做的事比這更麻煩，我需要他看 Agent Team 的行為有沒有偏離設計意圖，這包含了很多 AI 判斷：這個 dispatch 是否真的完成？這個完成是不是符合原本的 intent？這個 deviation 是 execution defect，還是 spec 本身寫得不完整？這個 Agent 產出的東西是不是有 cross-agent implication？這些沒有辦法單純仰賴 shell script 來完成。

---

<svg id="mermaid-g5y5xnz-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 1960.171875px;" viewBox="0 0 1960.171875 326" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-g5y5xnz-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-g5y5xnz-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-g5y5xnz-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-g5y5xnz-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-g5y5xnz-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-g5y5xnz-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g><path d="M895.832,40.153L797.755,47.961C699.677,55.769,503.522,71.384,405.445,82.692C307.367,94,307.367,101,307.367,104.5L307.367,108" id="L_A7_C_0" style=";" data-edge="true" data-et="edge" data-id="L_A7_C_0" data-points="W3sieCI6ODk1LjgzMjAzMTI1LCJ5Ijo0MC4xNTMzODU2Nzk4NTY5NX0seyJ4IjozMDcuMzY3MTg3NSwieSI6ODd9LHsieCI6MzA3LjM2NzE4NzUsInkiOjExMn1d" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M960.566,62L960.566,66.167C960.566,70.333,960.566,78.667,960.566,86.333C960.566,94,960.566,101,960.566,104.5L960.566,108" id="L_A7_L_0" style=";" data-edge="true" data-et="edge" data-id="L_A7_L_0" data-points="W3sieCI6OTYwLjU2NjQwNjI1LCJ5Ijo2Mn0seyJ4Ijo5NjAuNTY2NDA2MjUsInkiOjg3fSx7IngiOjk2MC41NjY0MDYyNSwieSI6MTEyfV0=" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M1025.301,40.489L1116.719,48.241C1208.138,55.993,1390.975,71.496,1482.394,82.748C1573.813,94,1573.813,101,1573.813,104.5L1573.813,108" id="L_A7_R_0" style=";" data-edge="true" data-et="edge" data-id="L_A7_R_0" data-points="W3sieCI6MTAyNS4zMDA3ODEyNSwieSI6NDAuNDg5MTI5OTUwMTI0NTN9LHsieCI6MTU3My44MTI1LCJ5Ijo4N30seyJ4IjoxNTczLjgxMjUsInkiOjExMn1d" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M239.578,169.605L212.012,177.171C184.445,184.737,129.313,199.868,101.746,212.934C74.18,226,74.18,237,74.18,242.5L74.18,248" id="L_C_C1_0" style=";" data-edge="true" data-et="edge" data-id="L_C_C1_0" data-points="W3sieCI6MjM5LjU3ODEyNSwieSI6MTY5LjYwNTE5OTY3ODM3MDR9LHsieCI6NzQuMTc5Njg3NSwieSI6MjE1fSx7IngiOjc0LjE3OTY4NzUsInkiOjI1Mn1d" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M307.367,190L307.367,194.167C307.367,198.333,307.367,206.667,307.367,216.333C307.367,226,307.367,237,307.367,242.5L307.367,248" id="L_C_C2_0" style=";" data-edge="true" data-et="edge" data-id="L_C_C2_0" data-points="W3sieCI6MzA3LjM2NzE4NzUsInkiOjE5MH0seyJ4IjozMDcuMzY3MTg3NSwieSI6MjE1fSx7IngiOjMwNy4zNjcxODc1LCJ5IjoyNTJ9XQ==" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M375.156,167.401L407.945,175.334C440.734,183.267,506.313,199.134,539.102,212.567C571.891,226,571.891,237,571.891,242.5L571.891,248" id="L_C_C3_0" style=";" data-edge="true" data-et="edge" data-id="L_C_C3_0" data-points="W3sieCI6Mzc1LjE1NjI1LCJ5IjoxNjcuNDAxMTkzMTgzNDk2Mjh9LHsieCI6NTcxLjg5MDYyNSwieSI6MjE1fSx7IngiOjU3MS44OTA2MjUsInkiOjI1Mn1d" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M896.785,183.161L886.261,188.467C875.737,193.774,854.689,204.387,844.165,215.193C833.641,226,833.641,237,833.641,242.5L833.641,248" id="L_L_L1_0" style=";" data-edge="true" data-et="edge" data-id="L_L_L1_0" data-points="W3sieCI6ODk2Ljc4NTE1NjI1LCJ5IjoxODMuMTYwNTI2ODgyNzEzMn0seyJ4Ijo4MzMuNjQwNjI1LCJ5IjoyMTV9LHsieCI6ODMzLjY0MDYyNSwieSI6MjUyfV0=" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M1024.348,183.161L1034.872,188.467C1045.396,193.774,1066.444,204.387,1076.968,215.193C1087.492,226,1087.492,237,1087.492,242.5L1087.492,248" id="L_L_L2_0" style=";" data-edge="true" data-et="edge" data-id="L_L_L2_0" data-points="W3sieCI6MTAyNC4zNDc2NTYyNSwieSI6MTgzLjE2MDUyNjg4MjcxMzJ9LHsieCI6MTA4Ny40OTIxODc1LCJ5IjoyMTV9LHsieCI6MTA4Ny40OTIxODc1LCJ5IjoyNTJ9XQ==" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M1479.813,176.597L1456.307,182.997C1432.802,189.398,1385.792,202.199,1362.286,212.099C1338.781,222,1338.781,229,1338.781,232.5L1338.781,236" id="L_R_R1_0" style=";" data-edge="true" data-et="edge" data-id="L_R_R1_0" data-points="W3sieCI6MTQ3OS44MTI1LCJ5IjoxNzYuNTk2NTk2MTk3MzE0Mn0seyJ4IjoxMzM4Ljc4MTI1LCJ5IjoyMTV9LHsieCI6MTMzOC43ODEyNSwieSI6MjQwfV0=" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M1573.813,190L1573.813,194.167C1573.813,198.333,1573.813,206.667,1573.813,214.333C1573.813,222,1573.813,229,1573.813,232.5L1573.813,236" id="L_R_R2_0" style=";" data-edge="true" data-et="edge" data-id="L_R_R2_0" data-points="W3sieCI6MTU3My44MTI1LCJ5IjoxOTB9LHsieCI6MTU3My44MTI1LCJ5IjoyMTV9LHsieCI6MTU3My44MTI1LCJ5IjoyNDB9XQ==" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M1667.813,174.982L1693.956,181.651C1720.099,188.321,1772.385,201.661,1798.529,211.83C1824.672,222,1824.672,229,1824.672,232.5L1824.672,236" id="L_R_R3_0" style=";" data-edge="true" data-et="edge" data-id="L_R_R3_0" data-points="W3sieCI6MTY2Ny44MTI1LCJ5IjoxNzQuOTgxNTYzMzc1ODk1MzV9LHsieCI6MTgyNC42NzE4NzUsInkiOjIxNX0seyJ4IjoxODI0LjY3MTg3NSwieSI6MjQwfV0=" marker-end="url(#mermaid-g5y5xnz-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g><g data-id="L_A7_C_0" transform="translate(0, 0)"></g></g><g><g data-id="L_A7_L_0" transform="translate(0, 0)"></g></g><g><g data-id="L_A7_R_0" transform="translate(0, 0)"></g></g><g><g data-id="L_C_C1_0" transform="translate(0, 0)"></g></g><g><g data-id="L_C_C2_0" transform="translate(0, 0)"></g></g><g><g data-id="L_C_C3_0" transform="translate(0, 0)"></g></g><g><g data-id="L_L_L1_0" transform="translate(0, 0)"></g></g><g><g data-id="L_L_L2_0" transform="translate(0, 0)"></g></g><g><g data-id="L_R_R1_0" transform="translate(0, 0)"></g></g><g><g data-id="L_R_R2_0" transform="translate(0, 0)"></g></g><g><g data-id="L_R_R3_0" transform="translate(0, 0)"></g></g></g><g><g id="flowchart-A7-0" transform="translate(960.56640625, 35)"><rect style="" x="-64.734375" y="-27" width="129.46875" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-34.734375, -12)"><rect></rect><foreignObject width="69.46875" height="24"><p>Agent A7</p></foreignObject></g></g><g id="flowchart-C-1" transform="translate(307.3671875, 151)"><rect style="" x="-67.7890625" y="-39" width="135.578125" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-37.7890625, -24)"><rect></rect><foreignObject width="75.578125" height="48"><p>Capability<br>看什麼</p></foreignObject></g></g><g id="flowchart-L-3" transform="translate(960.56640625, 151)"><rect style="" x="-63.78125" y="-39" width="127.5625" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-33.78125, -24)"><rect></rect><foreignObject width="67.5625" height="48"><p>Lifecycle<br>怎麼跑</p></foreignObject></g></g><g id="flowchart-R-5" transform="translate(1573.8125, 151)"><rect style="" x="-94" y="-39" width="188" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-64, -24)"><rect></rect><foreignObject width="128" height="48"><p>Operational Role<br>誰觸發，輸出去哪</p></foreignObject></g></g><g id="flowchart-C1-7" transform="translate(74.1796875, 279)"><rect style="" x="-66.1796875" y="-27" width="132.359375" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-36.1796875, -12)"><rect></rect><foreignObject width="72.359375" height="24"><p>Log Audit</p></foreignObject></g></g><g id="flowchart-C2-9" transform="translate(307.3671875, 279)"><rect style="" x="-117.0078125" y="-27" width="234.015625" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-87.0078125, -12)"><rect></rect><foreignObject width="174.015625" height="24"><p>Cross-Agent Validation</p></foreignObject></g></g><g id="flowchart-C3-11" transform="translate(571.890625, 279)"><rect style="" x="-97.515625" y="-27" width="195.03125" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-67.515625, -12)"><rect></rect><foreignObject width="135.03125" height="24"><p>Health Monitoring</p></foreignObject></g></g><g id="flowchart-L1-13" transform="translate(833.640625, 279)"><rect style="" x="-114.234375" y="-27" width="228.46875" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-84.234375, -12)"><rect></rect><foreignObject width="168.46875" height="24"><p>Long-running daemon</p></foreignObject></g></g><g id="flowchart-L2-15" transform="translate(1087.4921875, 279)"><rect style="" x="-89.6171875" y="-27" width="179.234375" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-59.6171875, -12)"><rect></rect><foreignObject width="119.234375" height="24"><p>Periodic auditor</p></foreignObject></g></g><g id="flowchart-R1-17" transform="translate(1338.78125, 279)"><rect style="" x="-111.671875" y="-39" width="223.34375" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-81.671875, -24)"><rect></rect><foreignObject width="163.34375" height="48"><p>Role 1<br>Continuous Detection</p></foreignObject></g></g><g id="flowchart-R2-19" transform="translate(1573.8125, 279)"><rect style="" x="-73.359375" y="-39" width="146.71875" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-43.359375, -24)"><rect></rect><foreignObject width="86.71875" height="48"><p>Role 2<br>Static Audit</p></foreignObject></g></g><g id="flowchart-R3-21" transform="translate(1824.671875, 279)"><rect style="" x="-127.5" y="-39" width="255" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-97.5, -24)"><rect></rect><foreignObject width="195" height="48"><p>Role 3<br>Dispatched Event Monitor</p></foreignObject></g></g></g></g></g></svg>

A7 不是單一功能，而是三個正交軸：看什麼、怎麼跑、誰觸發以及輸出去哪

## 我怎麼拆 A7？從一條線到三個軸

一開始我把 A7 拆成幾個功能，像一般 spec 會寫的那樣：Log Audit、Cross-Agent Validation、Health Monitoring，這些我都剛好剛剛做過，沒有錯，但不夠。

回到方法論的三層，缺了場景跟情境(Playbook)，你就只是還沒有落地的方法論，因為功能只回答「看什麼」，沒有回答「什麼時候跑？」、「被誰觸發？」、「結果要去哪？」，這些問題如果混在一起，A7 就會變成一個萬用稽核怪物，什麼都看、什麼都報、什麼都混在同一個 report 裡，最後 GM Context 被塞滿，人類也不會看。

所以我把 A7 被拆成三個軸

第一個軸是 capability，也就是他能偵測什麼：

- **Log Audit** ：看執行紀錄、hook、dispatch、inbox/outbox，找 operational problem。
- **Cross-Agent Validation** ：用獨立 context 檢查其他 Agent 的產出，特別是 Dm、C7、Em 這些原本容易自我驗證的地方。
- **Health Monitoring** ：看跨時間的退化，比如 correction trend、skill drift、completion report 品質、dispatch volume spike。

第二個軸是 lifecycle，也就是他怎麼跑：

- **Long-running daemon** ：常駐在 Overmind 裡，看即時訊號，抓 chain break、hook failure、process crash、dispatch spike。
- **Periodic auditor** ：由 `/team-review` 或固定 cadence 觸發，做比較深的統計和趨勢稽核。

第三個軸是 operational role，也就是這件事最後被整理成的 Three Roles：

- **Role 1：Continuous Dynamic Detection** ，常駐偵測，抓 live dispatch / inbox / hook event，輸出到 live operations queue。
- **Role 2：Periodic / Manual Static Audit** ，定期或手動檢查 architecture / methodology artifacts，輸出成報告和 wiki knowledge。
- **Role 3：Dispatched Event Monitor** ，由 GM 或其他 Agent 主動 dispatch，去旁邊觀察某一個正在進行的 work unit，輸出自己的 work-log。

這三個軸很重要，因為它讓 A7 從一個概念模糊的「稽核 Agent」，變成一個可以被拆解、被局部建造、被局部驗證的系統。

**A7 的方法論第一步，就是把「他要看什麼」和「他怎麼運作」拆開，** 否則你會把 capability、lifecycle、trigger、output channel 全部混成同一個設計，最後每個功能都看似合理，但整體跑不起來。

---

## A7 解決的不是錯誤，而是「誰知道錯誤發生了」

A7 的核心並不是「防止錯誤」，而是「縮短錯誤變成知識、再變成機制的距離」，也同時縮短了「錯誤到修復的時間」。

比如 hook failure，A7 沒辦法保證 hook 永遠不壞，但他可以看到 task archived 卻沒有對應 inbox JSON。

比如 chain break，A7 不能保證下一個 Agent 一定會被觸發，但他可以看到 task 裡有 chain，卻在時間界限內沒有下一個 task file。

比如 skill override anti-pattern，A7 不一定阻止某個 Agent 改 SKILL.md，但他可以看到 `override` fence 出現在一個 read-only master 應該被保護的區塊。

比如 external-service footgun，A7 不會自己去修 OAuth，不會重發 token，不會把 Telegram bot 接回來，但他可以在多個 Agent 的 stop-hook output 裡看到 `unauthorized` 、 `token expired` 、 `refresh failed` 這些反覆出現的症狀，先把它升級成一個候選問題。

這些事情如果靠我來抓，通常是出事才會回頭考古、翻 log、找 commit、對時間、猜 Root Cause，所以我設計 A7 在發生時就捕捉上面的訊號，先留下痕跡，就算當下沒有時間修，也能在 `/team-review` 的時候被看見。

所以 A7 的 observe-only scope 很簡單：觀察、回報、建議，但不 dispatch 其他 Agent、不修檔、不碰其他 Agent 的產出。

這聽起來保守，但其實是故意設計的安全邊界，如果 A7 一邊稽核一邊修正，他就又變成另一個 self-validation loop，稽核者和行動者共享同一個 context，最後他又會開始合理化自己的修正，這剛好是 A7 原本要解決的問題。

**A7 不是裝潢、設計團隊裡的設計師甚至屋主，他比較像外聘的驗屋驗收團隊，** 指出牆腳有水痕、電箱裡頭電線接法不對、天花板有縫隙，但要不要重拉線、要不要拆牆、要不要停工，還是 GM 和人類決定。

---

![外聘驗屋驗收團隊指出牆面水痕與電箱問題，屋主和 GM 決定是否修復](https://emilwu.tw/images/articles/32-agent-team-a7/observe-light.webp)

observe-only：A7 觀察、回報、建議，但不自己修正被稽核的對象

這篇先停在這裡：A7 從 validation gap 開始，最後變成一個 observe-only 的 Auditing Agent。它要解決的不是「讓團隊不犯錯」，而是讓錯誤有人看見、有人分類、有人知道下一步該進 live repair、team-review，還是方法論修正。

但這也留下另一個問題：如果 A7 的完整 spec 還沒成熟，可是 Role 3 的現場需求已經出現，我應該等完整 A7，還是先建一個能承擔 production duty 的前身？下一篇就是 A7sus4。

---

**參考資料**

<sup>[1]</sup> OpenAI — [How we monitor internal coding agents for misalignment](https://openai.com/index/how-we-monitor-internal-coding-agents-misalignment/) （2026-03-19；支持「coding agent 需要獨立 monitoring infrastructure」）

<sup>[2]</sup> OpenAI Agents SDK — [Tracing](https://github.com/openai/openai-agents-python/blob/main/docs/tracing.md) （官方文件；Agents SDK 內建 LLM generation、tool call、handoff、guardrail 與 custom event tracing）

<sup>[3]</sup> Microsoft Open Source Blog — [Introducing the Agent Governance Toolkit](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/) （2026-04-02；支持「agent 自主性提高後，governance / runtime security 變成獨立問題」）

<sup>[4]</sup> OWASP GenAI Security Project — [OWASP Top 10 for Agentic Applications for 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) （2025-12-09；支持 memory/context poisoning、insecure inter-agent communication、cascading failures、rogue agents 等 agentic failure mode）

<sup>[5]</sup> arXiv — [AgentTrace: A Structured Logging Framework for Agent System Observability](https://arxiv.org/abs/2602.10133) （2026-02-07；支持「agent observability 需要 continuous, introspectable trace capture」）

<sup>[6]</sup> arXiv — [AgentHallu: Benchmarking Automated Hallucination Attribution of LLM-based Agents](https://arxiv.org/abs/2601.06818) （2026-01-10；支持「agent failure attribution 本身很難，不能只看最終結果」）