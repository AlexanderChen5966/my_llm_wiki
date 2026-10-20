---
title: "Agent Team 實戰（十一）：持久化不是讓 Agent 一直跑，而是讓他醒來後還知道怎麼工作"
source: "https://emilwu.tw/articles/34-agent-team-long-run-persistence/"
author:
  - "[[EMil Wu]]"
published: 2026-05-01
created: 2026-09-02
description: "從半夜排 remote agent 寫新聞，到 A7sus4 的 persistent identity，這篇整理 Agent Team 的 long-run persistence：記憶、任務、身份、觀察四層如何接上，讓 Agent 跨 session、跨機器、跨時間仍然可交接、可追蹤、可驗證。"
tags:
  - "clippings"
status: processed
---
![Agent Team 在不同時間醒來時，記憶、任務、身份和觀察訊號仍然保持連結](https://emilwu.tw/images/articles/34-agent-team-long-run-persistence/hero-light.webp)

Long-run persistence 不是讓 Agent 永遠在線，而是讓他醒來後仍然找得到狀態、邊界、身份和觀察訊號

> **一句話：** 持久化不是讓 Agent 一直跑，而是讓他停下來、換環境、醒來之後，仍然知道自己是誰、要去哪裡找狀態、怎麼交回成果。

晚上十一點二十二分，我對 Claude Code 下了一個 `/schedule` 指令，請他在凌晨三點半叫一個 remote agent 起來(因為我的 5 小時額度 3:30 才重置)，閱讀 Mitchell Hashimoto 關於 Ghostty 離開 GitHub 的聲明，搜尋 GitHub 最近不穩定的資料，整理 10 到 15 個來源，寫成一篇新聞草稿，然後把研究資料跟草稿 push 到一個 branch，等我早上起來再 pull 下來審稿。\[1\]\[2\]

這個場景聽起來是一個很普通的排程任務，但其實碰到的是 Agent Team 下一個階段的核心問題： **如果 Agent 不在我眼前、不在同一個 session、甚至不在同一台電腦，我要怎麼讓這件事可交接、可追蹤、可驗證？**

之前我們聊的都是 Agent Team 的 **記憶** 持久化，重點放在 State、Wiki、mem0 三層，State 記「正在做什麼」，Wiki 記「我們知道什麼」，mem0 記「為什麼這樣決定」，這套架構解決的是「Agent 換 session 之後還找得到脈絡」的問題，這是持久化的第一層，但要等到你開始排 remote agent 在半夜工作，開 SDK 在雲端工作，或讓 A7sus4 在 Overmind 裡頭不間斷的做 live audit，你才會遇到下一個問題： **記憶能持久，不代表執行能持久。** \[3\]

---

## 第一層：把任務排出去，但不要忘了它不在本機

使用 `/schedule` 的第一件事，就是要弄清楚他不是本機 cron job， `/schedule` 是 Anthropic 跑在雲端 sandbox 的 Remote Agent，有自己的 git checkout、工具環境、Session Context，他不會看到本機的目錄，也不會看到我電腦裡那些尚未 Commit & Push 的檔案，更不會繼承我剛剛跟 GM 討論到一半的脈絡。\[1\]\[2\]

當然，部分記憶可以透過前面提到的記憶持久化解決，但認清這個事實對於後面的設計很重要，因為如果你把 remote agent 當成本機 session 的延伸，你有很高的機率會寫出很多問題的 Prompt，比如「照剛剛討論的去做」「寫好放在那個 XXX 資料夾」「照平常格式處理」，這些 Prompt 對一個 remote agent 跟無字天書一樣，因為他被啟動醒來的時候沒有 **剛剛** ，沒有 **平常** ，也沒有 **那個 XXX 資料夾** 。

所以你需要做一些調整，把任務 prompt 寫成一份完整的工作規格：你是誰、這個 repo 是什麼、要讀哪些來源、要搜尋哪些方向、研究資料要存到哪裡、草稿要存到哪裡、寫作風格要讀哪幾份文件、最後要怎麼 commit、怎麼 push、失敗時要怎麼回報，全部寫清楚，換句話說，remote agent 的持久化不是靠 session memory，而是靠一份可以自我啟動的 prompt，或者說，告訴 Agent 去哪裏讀資料(如果你有 Commit 且推上 Github)並且運作的 Prompt。

這跟我前面幾篇講的 Context Engineering 其實是同一件事，只是形式有些不同：本機 Agent 可以靠 repo 裡的 CLAUDE.md、wiki、mem0 和當前 context 重建狀態，remote agent 則必須靠 prompt、git repo、branch 回傳結果，因為他跟我的本機之間沒有共享 filesystem，唯一穩定的交接面就是 Git。

所以 Remote Agent 完成工作的條件不是「在本機寫好檔案」，而是「在 remote sandbox 寫好檔案，commit 到對應的 branch，push 回 GitHub，讓我早上 `git fetch` 後 checkout 來審稿」，這聽起來繞了一圈，但整個交接面其實很乾淨，只要我們弄清楚環境邊界，沒有把 remote agent 和 local agent 當作是同一種東西。

這就是執行持久化的第一個原則： **跨環境執行時，不要把 Local 的ㄧ切當作理所當然，要共享 artifact。**

---

<svg id="mermaid-o8szawc-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 1497.125px;" viewBox="0 0 1497.125 177" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-o8szawc-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-o8szawc-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-o8szawc-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-o8szawc-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-o8szawc-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-o8szawc-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g><path d="M258.631,69L263.852,67.333C269.072,65.667,279.513,62.333,288.233,60.667C296.953,59,303.953,59,307.453,59L310.953,59" id="L_L_P_0" style=";" data-edge="true" data-et="edge" data-id="L_L_P_0" data-points="W3sieCI6MjU4LjYzMTM3NzU1MTAyMDQsInkiOjY5fSx7IngiOjI4OS45NTMxMjUsInkiOjU5fSx7IngiOjMxNC45NTMxMjUsInkiOjU5fV0=" marker-end="url(#mermaid-o8szawc-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M574.953,59L579.12,59C583.286,59,591.62,59,599.317,60.116C607.015,61.233,614.077,63.465,617.608,64.581L621.139,65.698" id="L_P_R_0" style=";" data-edge="true" data-et="edge" data-id="L_P_R_0" data-points="W3sieCI6NTc0Ljk1MzEyNSwieSI6NTl9LHsieCI6NTk5Ljk1MzEyNSwieSI6NTl9LHsieCI6NjI0Ljk1MzEyNSwieSI6NjYuOTAzMjI1ODA2NDUxNjJ9XQ==" marker-end="url(#mermaid-o8szawc-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M884.953,108L889.12,108C893.286,108,901.62,108,909.286,108C916.953,108,923.953,108,927.453,108L930.953,108" id="L_R_B_0" style=";" data-edge="true" data-et="edge" data-id="L_R_B_0" data-points="W3sieCI6ODg0Ljk1MzEyNSwieSI6MTA4fSx7IngiOjkwOS45NTMxMjUsInkiOjEwOH0seyJ4Ijo5MzQuOTUzMTI1LCJ5IjoxMDh9XQ==" marker-end="url(#mermaid-o8szawc-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M1194.953,108L1199.12,108C1203.286,108,1211.62,108,1219.286,108C1226.953,108,1233.953,108,1237.453,108L1240.953,108" id="L_B_H_0" style=";" data-edge="true" data-et="edge" data-id="L_B_H_0" data-points="W3sieCI6MTE5NC45NTMxMjUsInkiOjEwOH0seyJ4IjoxMjE5Ljk1MzEyNSwieSI6MTA4fSx7IngiOjEyNDQuOTUzMTI1LCJ5IjoxMDh9XQ==" marker-end="url(#mermaid-o8szawc-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M258.631,147L263.852,148.667C269.072,150.333,279.513,153.667,310.566,155.333C341.62,157,393.286,157,444.953,157C496.62,157,548.286,157,577.651,155.884C607.015,154.767,614.077,152.535,617.608,151.419L621.139,150.302" id="L_L_R_0" style=";" data-edge="true" data-et="edge" data-id="L_L_R_0" data-points="W3sieCI6MjU4LjYzMTM3NzU1MTAyMDQsInkiOjE0N30seyJ4IjoyODkuOTUzMTI1LCJ5IjoxNTd9LHsieCI6NDQ0Ljk1MzEyNSwieSI6MTU3fSx7IngiOjU5OS45NTMxMjUsInkiOjE1N30seyJ4Ijo2MjQuOTUzMTI1LCJ5IjoxNDkuMDk2Nzc0MTkzNTQ4Mzh9XQ==" marker-end="url(#mermaid-o8szawc-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g><g data-id="L_L_P_0" transform="translate(0, 0)"></g></g><g><g data-id="L_P_R_0" transform="translate(0, 0)"></g></g><g><g data-id="L_R_B_0" transform="translate(0, 0)"></g></g><g><g data-id="L_B_H_0" transform="translate(0, 0)"></g></g><g transform="translate(444.953125, 157)"><g data-id="L_L_R_0" transform="translate(-92.6953125, -12)"><foreignObject width="185.390625" height="24"><p>not shared automatically</p></foreignObject></g></g></g><g><g id="flowchart-L-0" transform="translate(136.4765625, 108)"><rect style="" x="-128.4765625" y="-39" width="256.953125" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-98.4765625, -24)"><rect></rect><foreignObject width="196.953125" height="48"><p>Local session<br>current files + live context</p></foreignObject></g></g><g id="flowchart-P-1" transform="translate(444.953125, 59)"><rect style="" x="-130" y="-51" width="260" height="102" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -36)"><rect></rect><foreignObject width="200" height="72"><p>Self-contained prompt<br>scope + sources + output paths</p></foreignObject></g></g><g id="flowchart-R-3" transform="translate(754.953125, 108)"><rect style="" x="-130" y="-51" width="260" height="102" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -36)"><rect></rect><foreignObject width="200" height="72"><p>Remote sandbox<br>independent checkout + tools</p></foreignObject></g></g><g id="flowchart-B-5" transform="translate(1064.953125, 108)"><rect style="" x="-130" y="-51" width="260" height="102" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -36)"><rect></rect><foreignObject width="200" height="72"><p>Git branch<br>commits + artifacts + failure notes</p></foreignObject></g></g><g id="flowchart-H-7" transform="translate(1367.0390625, 108)"><rect style="" x="-122.0859375" y="-39" width="244.171875" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-92.0859375, -24)"><rect></rect><foreignObject width="184.171875" height="48"><p>Local review<br>fetch + inspect + decide</p></foreignObject></g></g></g></g></g></svg>

Remote routine 的穩定交接面不是本機 session，而是自我完備的 prompt、repo checkout、branch 和 artifacts

![一個任務包裹從本機桌面跨過環境邊界進入雲端 sandbox，再以 branch artifact 回到本機審稿](https://emilwu.tw/images/articles/34-agent-team-long-run-persistence/task-handoff-light.webp)

任務持久化的核心不是把 local context 帶過去，而是把任務包成 remote agent 可以自我啟動的 artifact

## 第二層：讓身份持久，而不是讓 prompt 變長

不過 remote routine 只解決了「某個任務在未來某個時間醒來」的問題，它沒有解決「某個角色長期存在」的問題，這個差異在 A7sus4 裡說過：臨時 subagent 很容易把自己理解成等待者，因為他沒有自己的目錄、CLAUDE.md、state、work-log，也沒有持續的 lifecycle，所以 A7sus4 的修正不是把 prompt 寫得更長，而是讓他變成 persistent process，讓身份本身有地方可以延續，這個例子帶出持久化的第二個原則： **如果你要一個 Agent 長期承擔某種責任，不能只給他 Prompt，要給他身份、工作目錄、狀態和歷史。** \[6\]\[7\]

---

## 第三層：不是每件事都值得 live monitor

第三個原則也是從 A7sus4 借來的：不是每件事都值得 live monitor，就像前一篇說的，訊號太少的任務不適合現場監督，因為沒有足夠觀測點時，Agent 會變成等待者，並且試著開始把沉默硬要解讀成某種意義，同樣的，long-run system 也需要 signal-density preflight，判斷這件事該 live monitor、post-hoc audit、GM inspection，還是人類看一眼就好，簡而言之， **持久化不是一直盯著，而是在正確的時間尺度上留下正確的觀察。**

---

## 第四層：低風險可以自動，高風險需要 challenge

第四個原則是自動化的邊界，這或許第一眼會看不懂，但實際上的概念並不難，就是告訴我們該自動化到什麼程度，比如低風險的自我維護可以自動，比如 wiki 去重、skill drift 偵測、completion artifact 檢查、重複錯誤 pattern 收斂可以自動，因為這些東西錯了非常容易回復，但高風險的架構演進不能全自動，至少不能在現階段全自動(說不定半年一年後就可以了)，如果 Remote Agent 想改 Agent 邊界、改 dispatch 模式、調整 Memory Extraction Rules 或稽核標準時，就需要通過人類的 challenge 或有獨立的 cross-audit agent 介入，以避免風險， **你可以讓 Agent 在需要判斷的地方找你，但不要假裝判斷本身已經可以完全交給他。**

---

## 我的持久化規劃

如果把目前的規劃收斂成一張圖，它其實不是一個系統，而是四個互相接上的層次：記憶持久化、任務持久化、身份持久化、觀察持久化，Agent Team 前面幾篇其實都已經講過其中一部分，而現在，我們把它們放到同一張圖裡看，因為 long-run 的 Agent Team 不是某一層突然變強，而是這四層可以開始互相連結，Agent 醒來時找得到狀態，做事時有明確邊界，做完後留下 artifact，出錯時有人看見，下一次啟動時又能把這些東西重新載入。

<svg id="mermaid-gh60cdh-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 627.0546875px;" viewBox="0 0 627.0546875 574" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-gh60cdh-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-gh60cdh-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-gh60cdh-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-gh60cdh-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-gh60cdh-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-gh60cdh-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g><path d="M241.402,101.33L224.169,106.942C206.935,112.554,172.467,123.777,155.234,132.888C138,142,138,149,138,152.5L138,156" id="L_M_T_0" style=";" data-edge="true" data-et="edge" data-id="L_M_T_0" data-points="W3sieCI6MjQxLjQwMjM0Mzc1LCJ5IjoxMDEuMzMwMzM3NTY3NTcyMDl9LHsieCI6MTM4LCJ5IjoxMzV9LHsieCI6MTM4LCJ5IjoxNjB9XQ==" marker-end="url(#mermaid-gh60cdh-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M138,262L138,268.167C138,274.333,138,286.667,141.169,298.42C144.338,310.174,150.676,321.347,153.846,326.934L157.015,332.521" id="L_T_I_0" style=";" data-edge="true" data-et="edge" data-id="L_T_I_0" data-points="W3sieCI6MTM4LCJ5IjoyNjJ9LHsieCI6MTM4LCJ5IjoyOTl9LHsieCI6MTU4Ljk4ODIzNjg2MDc5NTQ0LCJ5IjozMzZ9XQ==" marker-end="url(#mermaid-gh60cdh-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M187.918,438L187.918,442.167C187.918,446.333,187.918,454.667,217.377,464.997C246.836,475.327,305.753,487.653,335.212,493.816L364.671,499.98" id="L_I_O_0" style=";" data-edge="true" data-et="edge" data-id="L_I_O_0" data-points="W3sieCI6MTg3LjkxNzk2ODc1LCJ5Ijo0Mzh9LHsieCI6MTg3LjkxNzk2ODc1LCJ5Ijo0NjN9LHsieCI6MzY4LjU4NTkzNzUsInkiOjUwMC43OTg4Mjc3NTA4OTA2N31d" marker-end="url(#mermaid-gh60cdh-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M561.451,488L568.677,483.833C575.903,479.667,590.354,471.333,597.579,454.5C604.805,437.667,604.805,412.333,604.805,385C604.805,357.667,604.805,328.333,604.805,299C604.805,269.667,604.805,240.333,604.805,213C604.805,185.667,604.805,160.333,588.205,142.261C571.605,124.19,538.405,113.379,521.806,107.974L505.206,102.569" id="L_O_M_0" style=";" data-edge="true" data-et="edge" data-id="L_O_M_0" data-points="W3sieCI6NTYxLjQ1MTQxNjAxNTYyNSwieSI6NDg4fSx7IngiOjYwNC44MDQ2ODc1LCJ5Ijo0NjN9LHsieCI6NjA0LjgwNDY4NzUsInkiOjM4N30seyJ4Ijo2MDQuODA0Njg3NSwieSI6Mjk5fSx7IngiOjYwNC44MDQ2ODc1LCJ5IjoyMTF9LHsieCI6NjA0LjgwNDY4NzUsInkiOjEzNX0seyJ4Ijo1MDEuNDAyMzQzNzUsInkiOjEwMS4zMzAzMzc1Njc1NzIwOX1d" marker-end="url(#mermaid-gh60cdh-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M459.218,238L464.985,248.167C470.752,258.333,482.286,278.667,488.053,303.5C493.82,328.333,493.82,357.667,493.82,385C493.82,412.333,493.82,437.667,493.82,453.833C493.82,470,493.82,477,493.82,480.5L493.82,484" id="L_H_O_0" style=";" data-edge="true" data-et="edge" data-id="L_H_O_0" data-points="W3sieCI6NDU5LjIxODA4NDE2MTkzMTgsInkiOjIzOH0seyJ4Ijo0OTMuODIwMzEyNSwieSI6Mjk5fSx7IngiOjQ5My44MjAzMTI1LCJ5IjozODd9LHsieCI6NDkzLjgyMDMxMjUsInkiOjQ2M30seyJ4Ijo0OTMuODIwMzEyNSwieSI6NDg4fV0=" marker-end="url(#mermaid-gh60cdh-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M410.826,238L398.371,248.167C385.916,258.333,361.007,278.667,338.742,294.66C316.476,310.653,296.855,322.305,287.045,328.131L277.234,333.958" id="L_H_I_0" style=";" data-edge="true" data-et="edge" data-id="L_H_I_0" data-points="W3sieCI6NDEwLjgyNTkwNTUzOTc3Mjc1LCJ5IjoyMzh9LHsieCI6MzM2LjA5NzY1NjI1LCJ5IjoyOTl9LHsieCI6MjczLjc5NDgzMzA5NjU5MDksInkiOjMzNn1d" marker-end="url(#mermaid-gh60cdh-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g><g data-id="L_M_T_0" transform="translate(0, 0)"></g></g><g><g data-id="L_T_I_0" transform="translate(0, 0)"></g></g><g><g data-id="L_I_O_0" transform="translate(0, 0)"></g></g><g><g data-id="L_O_M_0" transform="translate(0, 0)"></g></g><g transform="translate(493.8203125, 387)"><g data-id="L_H_O_0" transform="translate(-50.609375, -12)"><foreignObject width="101.21875" height="24"><p>decision gate</p></foreignObject></g></g><g transform="translate(345.39481, 291.41082)"><g data-id="L_H_I_0" transform="translate(-79.8359375, -12)"><foreignObject width="159.671875" height="24"><p>architecture changes</p></foreignObject></g></g></g><g><g id="flowchart-M-0" transform="translate(371.40234375, 59)"><rect style="" x="-130" y="-51" width="260" height="102" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -36)"><rect></rect><foreignObject width="200" height="72"><p>記憶持久化<br>State + Wiki + mem0 + QMD</p></foreignObject></g></g><g id="flowchart-T-1" transform="translate(138, 211)"><rect style="" x="-130" y="-51" width="260" height="102" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -36)"><rect></rect><foreignObject width="200" height="72"><p>任務持久化<br>Prompt + artifact + branch</p></foreignObject></g></g><g id="flowchart-I-3" transform="translate(187.91796875, 387)"><rect style="" x="-130" y="-51" width="260" height="102" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -36)"><rect></rect><foreignObject width="200" height="72"><p>身份持久化<br>Directory + CLAUDE.md + lifecycle</p></foreignObject></g></g><g id="flowchart-O-5" transform="translate(493.8203125, 527)"><rect style="" x="-125.234375" y="-39" width="250.46875" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-95.234375, -24)"><rect></rect><foreignObject width="190.46875" height="48"><p>觀察持久化<br>Audit + finding + channel</p></foreignObject></g></g><g id="flowchart-H-8" transform="translate(443.90234375, 211)"><rect style="" x="-116.359375" y="-27" width="232.71875" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-86.359375, -12)"><rect></rect><foreignObject width="172.71875" height="24"><p>Human / GM challenge</p></foreignObject></g></g></g></g></g></svg>

Long-run Agent Team 不是單一 always-on session，而是記憶、任務、身份與觀察四層互相回流

**第一層是記憶持久化。** 這是 Article 29 已經講過的 State、Wiki、mem0、QMD，State 用 Git 記「現在做到哪裡」，Wiki 用 Git 記「我們知道什麼」，mem0 用 Qdrant Cloud 記「為什麼這樣決定」，QMD 讓 wiki 可以 search-first，而不是每次都把所有頁面塞進 Context，這一層的重點不是讓 Agent 記得所有事情，而是讓 Agent 知道遇到流程問他「現在」時去 State 翻，問「什麼」時去 Wiki 找，問「為什麼」時去 mem0 看，問「相關頁面在哪」時去 QMD 查，知道不同問題該去哪裡找答案。

但只有這層並不能單獨構成 long-run，因為記憶能找回來，只代表 Agent 醒來後有資料可以讀，不代表任務可以跨時間完成，不代表遠端執行環境知道怎麼把成果交回來，更不代表一個長期角色會自然維持自己身份，所以，記憶持久化解決的是「回來後知道什麼」，接著的任務持久化才是解決「離開時怎麼交代」。

**第二層是任務持久化。** local session 靠 repo artifact、hooks、inbox/outbox、git diff 來交接，remote routine 則靠一份自我完備的 prompt、獨立 sandbox、git branch、完成回報來交接，這兩者不能混為一談，本機任務的 filesystem 是共有的，所以 GM 可以 dispatch Em 去改 wiki，C7 可以 provision skill 到 agent 目錄，A7 可以看 outbox 和 task archive，但 remote routine 沒有這些狀態，他只能從 git repo clone 出來的世界開始工作，然後把結果再透過 git push 回來，當這些 Agent 各自活在雲端時，你會更麻煩。

所以 remote routine 的 prompt 必須像一份小型 HANDOFF.md，不只是「幫我寫一篇新聞」，而是要包含任務背景、目標、資料來源、輸出路徑、寫作風格、驗收方式、commit/push 流程，以及失敗時該回報哪些資訊，這不是 prompt engineering 的潔癖，而是執行持久化的基本條件，因為一個三點半醒來的 remote agent 沒有「剛剛那段對話」，只有你在排程那個瞬間交給他的 artifact。

這層也讓我們重新評估 Git 的角色，以前 Git 在 Agent Team 裡主要是 State 和 Wiki 的同步機制，現在卻變成 remote execution 的回收機制：branch 是 remote agent 把成果交回本機世界的方式，commit history 是人類審稿前的稽核材料，push 成功與否是任務是否跨環境完成的訊號，對於 long-run 來說，Git 不只是版本控制，它是跨時間、跨環境的交接介面，所以前一篇新聞說到 Github 的問題時，才有這麼多網路上的迴響。

**第三層是身份持久化。** 如果一個 Agent 只是被排程叫醒一次，他可以靠完整 prompt 完成任務，但如果一個 Agent 要長期承擔某種責任，他需要的不只是 prompt，而是身份可以落地的地方，這是從 A7sus4 來的觀察：當一個 Agent 需要持續 Monitor、累積 work-log、保留 state、調整自己的 audit cycle，他就不能只是臨時 subagent，他必須有自己的目錄、CLAUDE.md、skills、state、log 和 lifecycle。

這層目前有兩種形態：一種是 scheduled identity，也就是 remote routine 這種「每次啟動都靠 prompt 重建身份」的角色，它適合一次性研究、定期掃描、固定格式的報告，因為任務可以被完整寫在 prompt 裡；另一種是 persistent identity，也就是 A7sus4 這種「身份存在於本機 process 和 repo artifact 裡」的角色，它適合需要累積趨勢、觀察現場、回寫 history 的工作，因為角色本身需要在多次任務之間成長。

這兩種形態的差別沒有哪一個比較高級，重點是哪一個持久化成本比較合理，remote routine 的好處是不用佔本機 process，時間到了自己跑，缺點是每次都要靠 prompt 重建世界；persistent process 的好處是身份和狀態連續，缺點是要處理 lifecycle、crash policy、heartbeat、rate limit、watchdog 這些營運問題，所以我沒有特別選哪一邊，我讓它們各自負責不同的工作。

**第四層是觀察持久化。** Agent Team 一旦開始 long-run，最危險的不是單次任務失敗，而是錯誤發生了卻沒有發現、Finding 沒有變成知識，hook 沒有觸發、branch 沒有 push、artifact 沒有出現、completion 越來越薄、skill copy 越來越舊，這些都不會在當下爆炸，但會讓整個 agent team 慢慢偏移，觀察持久化要解決的不是「永遠不要錯」，而是「錯誤要可以被察覺，不要無聲無息的消失」。

所以 A7 的 observe-only scope 在這裡就開始變得重要，他看見 silent failure、chain break、skill drift、team degradation，但不直接修，他回報之後讓修正回到 GM、Dm 和人類的決策鏈裡，A7 負責的是把低階事件的雜訊過濾、整理成 finding，把 finding 送到正確的 channel：live bug 進 team-status，方法論洞察進 Em wiki，特定 dispatch 的觀察留在 work-log，架構級問題進下一次 human review。\[4\]\[5\]

![許多安靜失敗訊號被整理成 findings，交給 GM 和人類在正確的決策入口判斷](https://emilwu.tw/images/articles/34-agent-team-long-run-persistence/observation-loop-light.webp)

觀察持久化不是盯著所有 log，而是讓 silent failure、deviation 和 drift 變成可以討論、可以寫回系統的 finding

這四層接起來之後，Agent Team 才比較接近我想要的 long-run 形態：睡前可以用 remote routine 做一個清楚封裝的研究任務，早上用 branch 回收成果；本機可以用 persistent process 觀察高訊號密度的工作單位，不是靠臨時 prompt 假裝監督；執行結果可以變成 findings，findings 可以回流到 wiki 和設計流程；真正需要判斷的地方停在 GM 和人類面前，而不是藏在自動化流程裡自己往下跑。

---

## 持久化的目的不是無人化

我知道 Agent Team 的持久化很容易被誤解成「讓 AI 不需要人也能一直跑」，但我真正想做的不是這個，不需要人也能一直跑有太多的方法可以達成，AWS、GCP、n8n、Zeabur 都在提供各種服務協助你達成這個目標，你不需要自己造輪子，放棄業界幾十年來的 Practice，幻想毫無經驗的你加上 AI 會打敗有數十年經驗的業界高手加上 AI。

我想做的是當這些雲端、服務不管怎麼轉換、變動，都能夠持續運作、操作、交付工作的框架，他應該是：當 Agent 不在我眼前工作時，他的工作仍然有邊界；當 AI 跨 session、跨機器、跨時間繼續工作時，仍然找得到狀態；當 AI 失敗時，錯誤資訊會變成可以看見、討論、寫回系統的訊號，不會安靜地消失在某個 log 裡；當 AI 真的執行到需要判斷的地方時，他知道要停下來，把問題交回 GM 和人類。

所以，持久化不是把人移出 loop，而是讓人不用待在錯的 loop 裡。

我不需要半夜三點半坐在電腦前盯著 remote agent 搜資料，但我需要早上看到他留下的 branch、研究檔案、草稿、錯誤資訊和失敗原因；我不需要 GM 把所有 hook log 都塞進 context，但我需要 A7 把真正異常的地方整理成 finding；我不需要每個 Agent 記住所有事情，但我需要他在醒來時知道要去哪裡找正確的狀態、知識、決策理由。

或許，這就是 Agent Team 持久化裡頭我們一直搞錯的地方： **我要的是 AI 停下來之後，還能穢土轉生、被叫回來的能力，不是讓他永遠不要停。**

一個真正 long-run 的 Agent Team，不是一直開著的 session，而是一組在 session 消失之後仍然存在的身份、記憶、artifact、稽核和決策通道。

這才是我真的想建立的東西。

---

**參考資料**

\[1\] Claude Code Docs — Run prompts on a schedule：說明 session-scoped scheduled tasks、cloud routines、fresh clone 和本機檔案存取邊界。 [https://code.claude.com/docs/en/scheduled-tasks](https://code.claude.com/docs/en/scheduled-tasks)

\[2\] Claude Code Docs — Automate work with routines：說明 routines 是 prompt、repositories、connectors 與 triggers 的封裝，並在 Anthropic-managed cloud infrastructure 執行。 [https://code.claude.com/docs/en/web-scheduled-tasks](https://code.claude.com/docs/en/web-scheduled-tasks)

\[3\] OpenAI — The next evolution of the Agents SDK：說明 agent harness、native sandbox execution、manifest 與 long-running task 的 production infrastructure。 [https://openai.com/index/the-next-evolution-of-the-agents-sdk/](https://openai.com/index/the-next-evolution-of-the-agents-sdk/)

\[4\] OpenAI Agents SDK — Tracing：說明 agent run 的 traces/spans、tool calls、handoffs、guardrails 與 custom events。 [https://openai.github.io/openai-agents-python/tracing/](https://openai.github.io/openai-agents-python/tracing/)

\[5\] LangChain — AI Agent Observability: Tracing, Testing, and Improving Agents：說明 production agent observability、tracing、testing 與 continuous improvement。 [https://www.langchain.com/articles/agent-observability](https://www.langchain.com/articles/agent-observability)

\[6\] Springdrift: An Auditable Persistent Runtime for LLM Agents with Case-Based Memory, Normative Safety, and Ambient Self-Perception：長期 agent runtime、auditable substrate、git-backed recovery 與 persistent operation 的研究案例。 [https://arxiv.org/abs/2604.04660](https://arxiv.org/abs/2604.04660)

\[7\] On the Use of Agentic Coding Manifests: An Empirical Study of Claude Code：討論 CLAUDE.md / agentic manifests 如何提供 project context、identity 與 operational rules。 [https://arxiv.org/abs/2509.14744](https://arxiv.org/abs/2509.14744)