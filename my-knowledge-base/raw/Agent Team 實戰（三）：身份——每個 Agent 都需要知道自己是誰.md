---
title: "Agent Team 實戰（三）：身份——每個 Agent 都需要知道自己是誰"
source: "https://emilwu.tw/articles/25-agent-team-identity/"
author:
  - "[[EMil Wu]]"
published: 2026-04-13
created: 2026-09-02
description: "Agent 的身份不是一句 prompt，是整個目錄結構。CLAUDE.md 告訴他「你是誰」，方法論告訴他「你怎麼判斷」，Commands 告訴他「你會做什麼」——五個 Agent、五種身份設計，從 Em 的 wiki 分類體系到 GM 的「不要做什麼」清單。"
tags:
  - "clippings"
status: processed
---
![五個 Agent 站在一起，胸前的名牌只有問號](https://emilwu.tw/images/articles/25-agent-team-identity/hero-light.webp)

你是 Agent GM 還是 Agent G？

> **一句話：** Agent 的身份不只是一個 CLAUDE.md，而是整個目錄。

---

「你是 Agent GM 還是 Agent G？」

這是我在某天晚上同時開著好幾個 Claude Code session 的時候，真的問 Agent 的一句話(因為都是語音輸入)，那個 session 的 first prompt 就是這句，不是什麼精心設計的指令，只是因為我在打造 Agent 的過程中，有太多的搬移跟切換，所以當時我突然不確定眼前這個 Agent 到底載入了誰的 CLAUDE.md？他現在覺得自己是誰？以及他手上有哪些工具可以用？如果連人類都分不清楚眼前的 Agent 是誰，那 Agent 自己分得清嗎？

我們以為可以，畢竟數位的世界應該是零跟一，但答案是：如果你沒有明確定義，他不會分清楚， **而且他不會告訴你他搞混了** ，他會用他碰巧載入的那個身份，去做你期望另一個身份該做的事\[2\]，然後產出一些看起來有點對但感覺有什麼不太對的東西，你要花點時間才會發現問題出在最源頭的身份，而不是他的推理或執行。

上一篇講了為什麼要建 Agent Team，這篇要講建了之後碰到的第一個問題： **身份** 。

---

## 身份不是只有 System Prompt

在 Subagent 模式下，身份不是問題，因為 Subagent 沒有身份，他就是 parent Agent 的延伸，做完就消失，不需要「知道自己是誰」，但在 Agent Team 裡，每個 Agent 是獨立的、持續存在的、有自己專業領域的個體，身份就變成了必須確認的第一個問題。

當然，最直覺的做法就是在 System Prompt 裡加一句「你是 Agent Em，負責知識管理」，但這遠遠不夠，因為 Agent 的行為不只會被一句話決定，他是被整個 Context 決定的，這包括他的工作目錄裡有什麼檔案、他能讀到什麼 Skill、他被設定了什麼 Commands，以及他的 CLAUDE.md 裡怎麼描述他的思考框架\[1\]。

在之前寫 Context Engineering 那篇的時候，我提過 Context 是 Agent 行為的決定因素，這個論點在 Agent Team 的場景下變得更具體： **身份就是 Context 的第一層，** 如果這一層歪了，後面不管你怎麼精心設計 dispatch、memory、coordination，都會因為 Agent 用錯誤的身份去理解每一條指令，而整個跟著歪掉。

所以實際上，在打造 Agent Team 的過程中，我為每個 Agent 設計的身份不是一句話，而是一整個目錄結構：

| 元件 | 以 Em 為例 | 告訴 Agent 什麼 |
| --- | --- | --- |
| CLAUDE.md | `agent-Em/CLAUDE.md` | 你是誰、你怎麼思考 |
| 方法論文件 | `agent-Em/METHODOLOGY.md` | 你判斷事情的原則 |
| Commands 目錄 | `agent-Em/.claude/commands/` | 你會做什麼（ingest、query、lint） |
| Skills 目錄 | `agent-Em/.claude/skills/` | 你知道什麼（wiki 格式規範） |
| 工作目錄 | `agent-Em/wiki/` 、 `agent-Em/raw/` | 你的東西在哪裡、別人給你的在哪裡 |

Em 的目錄結構就是他的身份：CLAUDE.md 告訴他「你是誰、你怎麼思考」\[4\]，方法論文件告訴他「你判斷事情的原則」，commands 目錄告訴他「你會做什麼」(ingest、query、lint)，skills 目錄告訴他「你知道什麼」(wiki 的格式規範)，而 wiki 和 raw 目錄告訴他「你的東西在哪裡、別人給你的東西在哪裡」。

五個面向合在一起才是完整的身份\[10\]，少任何一個，Agent 就會開始猜，而猜錯的代價是你事後花時間幫他收拾。

<svg id="mermaid-iezg0h4-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 1525.390625px;" viewBox="0 0 1525.390625 256" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-iezg0h4-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-iezg0h4-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-iezg0h4-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-iezg0h4-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-iezg0h4-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-iezg0h4-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g></g><g></g><g><g transform="translate(0, 0)"><g><g id="identity" data-look="classic"><rect style="" x="8" y="8" width="1509.390625" height="240"></rect><g transform="translate(709.75, 8)"><foreignObject width="105.890625" height="24"><p>Agent Identity</p></foreignObject></g></g></g><g><path d="M305.5,128L316.43,128C327.359,128,349.219,128,370.411,128C391.604,128,412.13,128,422.393,128L432.656,128" id="L_A_B_0" style=";" data-edge="true" data-et="edge" data-id="L_A_B_0" data-points="W3sieCI6MzA1LjUsInkiOjEyOH0seyJ4IjozNzEuMDc4MTI1LCJ5IjoxMjh9LHsieCI6NDM2LjY1NjI1LCJ5IjoxMjh9XQ==" marker-end="url(#mermaid-iezg0h4-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M696.656,93.52L707.665,90.6C718.674,87.68,740.693,81.84,762.044,78.92C783.396,76,804.081,76,814.423,76L824.766,76" id="L_B_C_0" style=";" data-edge="true" data-et="edge" data-id="L_B_C_0" data-points="W3sieCI6Njk2LjY1NjI1LCJ5Ijo5My41MTk4MjQ2NjYyNjgxOH0seyJ4Ijo3NjIuNzEwOTM3NSwieSI6NzZ9LHsieCI6ODI4Ljc2NTYyNSwieSI6NzZ9XQ==" marker-end="url(#mermaid-iezg0h4-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M696.656,162.48L707.665,165.4C718.674,168.32,740.693,174.16,765.951,177.08C791.208,180,819.706,180,833.954,180L848.203,180" id="L_B_D_0" style=";" data-edge="true" data-et="edge" data-id="L_B_D_0" data-points="W3sieCI6Njk2LjY1NjI1LCJ5IjoxNjIuNDgwMTc1MzMzNzMxODR9LHsieCI6NzYyLjcxMDkzNzUsInkiOjE4MH0seyJ4Ijo4NTIuMjAzMTI1LCJ5IjoxODB9XQ==" marker-end="url(#mermaid-iezg0h4-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M1054.766,76L1068.526,76C1082.286,76,1109.807,76,1137.328,79.366C1164.849,82.733,1192.37,89.465,1206.13,92.831L1219.891,96.198" id="L_C_E_0" style=";" data-edge="true" data-et="edge" data-id="L_C_E_0" data-points="W3sieCI6MTA1NC43NjU2MjUsInkiOjc2fSx7IngiOjExMzcuMzI4MTI1LCJ5Ijo3Nn0seyJ4IjoxMjE5Ljg5MDYyNSwieSI6OTYuMTk3NTg4OTQ0NDI4MX1d" fill="none" stroke="currentColor"></path><path d="M1031.328,180L1048.995,180C1066.661,180,1101.995,180,1133.422,176.634C1164.849,173.267,1192.37,166.535,1206.13,163.169L1219.891,159.802" id="L_D_E_0" style=";" data-edge="true" data-et="edge" data-id="L_D_E_0" data-points="W3sieCI6MTAzMS4zMjgxMjUsInkiOjE4MH0seyJ4IjoxMTM3LjMyODEyNSwieSI6MTgwfSx7IngiOjEyMTkuODkwNjI1LCJ5IjoxNTkuODAyNDExMDU1NTcxOX1d" fill="none" stroke="currentColor"></path></g><g><g transform="translate(371.078125, 128)"><g data-id="L_A_B_0" transform="translate(-28.078125, -12)"><foreignObject width="56.15625" height="24"><p>defines</p></foreignObject></g></g><g transform="translate(762.7109375, 76)"><g data-id="L_B_C_0" transform="translate(-25.359375, -12)"><foreignObject width="50.71875" height="24"><p>guides</p></foreignObject></g></g><g transform="translate(762.7109375, 180)"><g data-id="L_B_D_0" transform="translate(-28.5546875, -12)"><foreignObject width="57.109375" height="24"><p>informs</p></foreignObject></g></g><g transform="translate(1137.328125, 76)"><g data-id="L_C_E_0" transform="translate(-45.0625, -12)"><foreignObject width="90.125" height="24"><p>operates on</p></foreignObject></g></g><g transform="translate(1137.328125, 180)"><g data-id="L_D_E_0" transform="translate(-40.6640625, -12)"><foreignObject width="81.328125" height="24"><p>references</p></foreignObject></g></g></g><g><g id="flowchart-A-0" transform="translate(175.5, 128)"><rect style="fill:#6b8f71 !important" x="-130" y="-39" width="260" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-100, -24)"><rect></rect><foreignObject width="200" height="48"><div style="color: rgb(255, 255, 255) !important; display: table; white-space: break-spaces; line-height: 1.5; max-width: 200px; text-align: center; width: 200px;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>CLAUDE.md你是誰、你怎麼思考</p></span></div></foreignObject></g></g><g id="flowchart-B-1" transform="translate(566.65625, 128)"><rect style="fill:#58a6ff !important" x="-130" y="-39" width="260" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-100, -24)"><rect></rect><foreignObject width="200" height="48"><div style="color: rgb(255, 255, 255) !important; display: table; white-space: break-spaces; line-height: 1.5; max-width: 200px; text-align: center; width: 200px;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>METHODOLOGY.md你判斷事情的原則</p></span></div></foreignObject></g></g><g id="flowchart-C-2" transform="translate(941.765625, 76)"><rect style="fill:#c67a50 !important" x="-113" y="-27" width="226" height="54" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-83, -12)"><rect></rect><foreignObject width="166" height="24"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>Commands你會做什麼</p></span></div></foreignObject></g></g><g id="flowchart-D-3" transform="translate(941.765625, 180)"><rect style="fill:#c67a50 !important" x="-89.5625" y="-27" width="179.125" height="54" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-59.5625, -12)"><rect></rect><foreignObject width="119.125" height="24"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>Skills你知道什麼</p></span></div></foreignObject></g></g><g id="flowchart-E-4" transform="translate(1349.890625, 128)"><rect style="fill:#7c6db5 !important" x="-130" y="-39" width="260" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-100, -24)"><rect></rect><foreignObject width="200" height="48"><div style="color: rgb(255, 255, 255) !important; display: table; white-space: break-spaces; line-height: 1.5; max-width: 200px; text-align: center; width: 200px;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>Working Dirs你的東西在哪裡</p></span></div></foreignObject></g></g></g></g></g></g></g></svg>

Agent 的身份是五個層級的組合，不是一句 System Prompt——CLAUDE.md 定義方法論，方法論引導 Commands 和 Skills，兩者都操作在工作目錄上

---

![一句話的身份 vs 完整目錄結構的身份](https://emilwu.tw/images/articles/25-agent-team-identity/directory-identity-light.webp)

左邊是一句 System Prompt，右邊是完整的身份目錄——少任何一層，Agent 就開始猜

## 五個 Agent，五種身份設計

Agent Team 裡有五個 Layer 2 的 Specialist Agent，每一個的身份設計都不一樣，因為他們的職責完全不同，需要的思考方式也不同。我不打算把五個都平鋪直敘地列出來(那太像技術文件了)，而是每個挑一個設計時的決策時刻來講。

### Em：wiki 結構定義了思考方式

Em 是 Knowledge Agent，負責管理整個 Agent Team 的知識庫，他的核心職責是 ingest（把原始資料轉化成結構化的 wiki 頁面）、query（從 wiki 裡找到相關知識回答問題）、lint（檢查 wiki 的一致性和完整性）。

設計 Em 的時候，最重要的關鍵不是他的 CLAUDE.md 寫什麼，而是 **wiki 的目錄結構怎麼設計，** 因為 Em 的思考方式是被他的 wiki 結構決定的，wiki 裡有 concepts（概念頁）、entities（人事物頁）、sources（來源紀錄），每次 ingest 的時候，Em 不是把資料丟進去就好，他要判斷「這段知識屬於哪個 concept」「這個 concept 跟哪些既有的 concept 有交叉引用」「有沒有新的 concept 需要建立」，這些判斷的框架完全來自 wiki 的結構設計。

換句話說， **Em 的身份不是「你是知識管理員」這七個字，而是整個 wiki 的分類體系，** 這個分類體系決定了他怎麼理解新資訊、怎麼連結舊知識、怎麼判斷什麼值得記錄、什麼可以忘掉。

### C7：一個 canonical location 的規則

C7 是 Asset Manager，管理 Skills、Tools、Plugins、MCP Servers、Commands 這些可以執行的資產，設計核心是一個看似簡單但影響深遠的規則： **每個資產只有一個 canonical location(標準位置)，** 其他地方如果需要用，就 provision(佈建) 過去，不做複製。

這個規則決定了 C7 的整個工作方式：查詢永遠只需要看一個地方（catalog/INDEX.md），佈建是 provision 不是複製，更新只需要改 canonical location。C7 第一次獨立運作的時候，11 分鐘處理了 10 個新 skill 的入庫，中間碰到 4 次錯誤（路徑找不到、指令失敗），但他都自己解決了，因為他的 CLAUDE.md 裡定義了清楚的方法論：遇到路徑問題就回去查總目錄，遇到格式問題就回去查品質合約， **他的身份給了他解決問題的框架，而不只是「你該做什麼」的清單。**

### Dm：三個角色在一個身份裡

Dm 是 Builder Agent(建築師)，負責設計和建造新的 Agent，但 Dm 的身份設計有一個特殊之處：他不是一個角色，而是三個角色的組合。

- Architect 負責設計，用 Opus 模型因為需要完整的推理能力來交叉分析 wiki 知識和設計約束
- Scaffolder 負責搭建，我用 Sonnet 主要因為這不需要深度推理，需要的是快速準確地產出檔案
- Validator 負責驗證，對照 build checklist 檢查 scaffold 的產出是否合規。

設計 Dm 的時候我面對的決策是：這三個角色要設計成三個獨立的 Agent，還是一個 Agent 底下的三個 Subagent\[7\]？答案是後者，因為他們之間的 Context 需要高度互相參照，Architect 的設計 plan 要完整傳給 Scaffolder，Scaffolder 的產出要完整傳給 Validator，如果是獨立的 Agent，每次交接都會遺失 Context，而在 Subagent 模式下，parent Agent (Dm 本身) 可以確保 Context 在三個角色之間完整傳遞。

**這也是之前在 Skill vs Subagent 那篇討論過的判斷標準：Context 是否需要留在主對話裡？** Dm 的情況是需要的，所以三個角色是 Subagent，不是獨立 Agent。

<svg id="mermaid-755iyu4-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 346px;" viewBox="0 0 346 475" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-755iyu4-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-755iyu4-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-755iyu4-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-755iyu4-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-755iyu4-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-755iyu4-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g></g><g></g><g><g transform="translate(0, 0)"><g><g id="Dm" data-look="classic"><rect style="" x="8" y="8" width="330" height="459"></rect><g transform="translate(96.6171875, 8)"><foreignObject width="152.765625" height="24"><p>Dm — Builder Agent</p></foreignObject></g></g></g><g><path d="M173,99.5L173,107.75C173,116,173,132.5,173,148.333C173,164.167,173,179.333,173,186.917L173,194.5" id="L_A_B_0" style=";" data-edge="true" data-et="edge" data-id="L_A_B_0" data-points="W3sieCI6MTczLCJ5Ijo5OS41fSx7IngiOjE3MywieSI6MTQ5fSx7IngiOjE3MywieSI6MTk4LjV9XQ==" marker-end="url(#mermaid-755iyu4-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M173,276.5L173,284.75C173,293,173,309.5,173,325.333C173,341.167,173,356.333,173,363.917L173,371.5" id="L_B_C_0" style=";" data-edge="true" data-et="edge" data-id="L_B_C_0" data-points="W3sieCI6MTczLCJ5IjoyNzYuNX0seyJ4IjoxNzMsInkiOjMyNn0seyJ4IjoxNzMsInkiOjM3NS41fV0=" marker-end="url(#mermaid-755iyu4-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g transform="translate(173, 149)"><g data-id="L_A_B_0" transform="translate(-91.3671875, -12)"><foreignObject width="182.734375" height="24"><p>完整 Contextdesign plan</p></foreignObject></g></g><g transform="translate(173, 326)"><g data-id="L_B_C_0" transform="translate(-96.328125, -12)"><foreignObject width="192.65625" height="24"><p>完整 Contextscaffold 產出</p></foreignObject></g></g></g><g><g id="flowchart-A-0" transform="translate(173, 72.5)"><rect style="fill:#6b8f71 !important" x="-125.109375" y="-27" width="250.21875" height="54" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-95.109375, -12)"><rect></rect><foreignObject width="190.21875" height="24"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>ArchitectOpus · 設計 plan</p></span></div></foreignObject></g></g><g id="flowchart-B-1" transform="translate(173, 237.5)"><rect style="fill:#58a6ff !important" x="-130" y="-39" width="260" height="78" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-100, -24)"><rect></rect><foreignObject width="200" height="48"><div style="color: rgb(255, 255, 255) !important; display: table; white-space: break-spaces; line-height: 1.5; max-width: 200px; text-align: center; width: 200px;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>ScaffolderSonnet · 搭建檔案</p></span></div></foreignObject></g></g><g id="flowchart-C-2" transform="translate(173, 402.5)"><rect style="fill:#c67a50 !important" x="-115.109375" y="-27" width="230.21875" height="54" stroke="currentColor"></rect><g style="color:#fff !important" transform="translate(-85.109375, -12)"><rect></rect><foreignObject width="170.21875" height="24"><div style="color: rgb(255, 255, 255) !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;" xmlns="http://www.w3.org/1999/xhtml"><span style="color:#fff !important"><p>Validator驗證 checklist</p></span></div></foreignObject></g></g></g></g></g></g></g></svg>

Dm 的三個角色是 Subagent，確保 Context 在 Architect → Scaffolder → Validator 之間完整傳遞

### G7：sacred boundary

G7 是 Fleet Manager(艦隊管理員？但 Fleet 很少特意翻成中文)，管理所有 Layer 3 的開發、操作、或工作用的 Agent，我在設計 G7 身份時，最重要的一個決定是「sacred boundary」，意思是： **只有 G7 可以跟 Layer 3 的 Agent 說話，** 其他 Layer 2 的 Agent 不行。

這不是技術限制，是治理決策，如果每個 Layer 2 Agent 都可以直接 dispatch Layer 3 的 Agent，那當你的 fleet 從 5 個長到 50 個的時候，誰負責追蹤哪個 Agent 在做什麼？誰負責確保兩個 Agent 沒有同時在改同一份檔案？fleet management 需要一個統一的入口點，所以在我的設計裡，G7 的 sacred boundary 不是限制，是保護，保護整個 fleet 不會因為多頭管理而陷入混亂。

### GM：最晚成形，因為他的身份最複雜

GM 是最後一個被設計的 Agent，原因在上一篇提過：在 GM 被建出來之前，他的角色一直是我自己在扮演，但這也意味著 GM 的身份設計有一個獨特的挑戰：他的身份需要從我的行為模式裡提煉出來。

GM 設計的過程走了一條跟其他 Agent 完全不同的路，其他四個 Agent 的設計是「先有需求/目的，再依此設計身份」，但 GM 的設計是「先有大量的行為紀錄，再從裡面提煉身份」，更具體一點來說，我跟 Agent G(GM 的前身，bootstrap agent)討論了好幾輪，每一輪都在釐清類似的問題：「我現在手動做的這件事，到底是什麼性質的工作？他應該被分給某個 Agent？創建一個 Agent 接手？創建一個 Skill 來執行？留在 GM 身上？或者應該由人類把關？」討論到最後，精煉的結果，才產生了 GM Agent，類似從 DoD (Definition of Done, 完成定義)來定義 GM。

所以 GM 的核心身份被約束為「對話 before dispatch」，意思是 GM 收到任何指令的時候，第一個動作不是立刻派工，而是先跟人類對話，確認他理解了需求的全貌、確認資訊足夠了，才開始協調其他 Agent 進行，這個原則是從一個我反覆修正 Agent G 的行為裡提煉出來的：Agent G 總是太急著動手，我每次都要拉住他說「先等一下，你太快下結論了」、「不要急著生成 Prompt Dispatch，先跟我討論」、「請先參考 Em wiki 裡面的方法論跟知識思考之後再回答我」。

**所以 GM 的身份本質上反而是一連串「不要做什麼」的清單：** 不要急著派工、不要跳過對話直接動手、不要沒諮詢 Em 的知識就做設計決策、不要假設你已經理解了需求的全部，這跟之前在校準那篇裡討論的「減法」概念很像(你要先知道什麼不該做，才能精確地做該做的事)。

![GM 的身份是一連串「不要做什麼」的清單](https://emilwu.tw/images/articles/25-agent-team-identity/gm-negative-space-light.webp)

GM 的身份本質是負空間——被刻意排除的行動定義了他是誰

而 GM 的設計過程本身也是一個 Agent Team 協作的案例：Dm 的 Architect 讀了 Em 交叉分析 wiki 所有相關 concepts 後產出的 GM Methodology 建議、讀了 16 個 use case scenario、讀了所有架構決策，然後設計出 GM 的 plan，同時 GM 的 CLAUDE.md 被要求「零 references 到 Agent G」，也就是 clean start，不繼承 bootstrap agent 的 build context，因為 GM 需要從自己的身份出發思考，而不是帶著 Agent G 的舊包袱。

說了這麼多，那定義好了身份後，建立 Agent 的第一階段就結束往前邁進了嗎？不，身份是會過期的，我們常常聽到 Agent 要用閉環自我進化，但從我的概念來說，與其說會進化，不如該說是會過期，而你需要更新，下一篇讓我們來看這個概念。

---

**參考資料**

\[1\] OpsClaw.ai — The SOUL Framework: Why Your AI Agent Needs an Identity (Not Just a Prompt)（區分 system prompt vs. SOUL file：SOUL 創造 operator，system prompt 只創造 tool） [https://opsclaw.ai/blog/soul-framework-ai-agent-identity](https://opsclaw.ai/blog/soul-framework-ai-agent-identity)

\[2\] arXiv — Why Do Multi-Agent LLM Systems Fail?（ICLR 2025，1,600+ traces 中 “Disobey role specification” 為關鍵失敗模式） [https://arxiv.org/abs/2503.13657](https://arxiv.org/abs/2503.13657)

\[4\] HumanLayer — Writing a Good CLAUDE.md（CLAUDE.md 撰寫最佳實踐） [https://www.humanlayer.dev/blog/writing-a-good-claude-md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)

\[7\] arXiv — Multi-Agent Teams Hold Experts Back（反面觀點：多 agent 團隊在某些情況下反而拖累專家級 agent） [https://arxiv.org/html/2602.01011v1](https://arxiv.org/html/2602.01011v1)

\[10\] Levi Ezra — Building AI Agents with Personas, Goals, and Dynamic Memory [https://medium.com/@leviexraspk/building-ai-agents-with-personas-goals-and-dynamic-memory-6253acacdc0a](https://medium.com/@leviexraspk/building-ai-agents-with-personas-goals-and-dynamic-memory-6253acacdc0a)