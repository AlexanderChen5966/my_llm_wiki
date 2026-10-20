---
title: "【AI Agent 架構】Agent 不是越複雜越好：哪些機制該留，哪些可以拿掉？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400653"
author:
  - "[[iThome]]"
published:
created: 2026-08-03
description: "我在 San Francisco 參加一場 AI Agent 講座時，一位 Anthropic 工程師分享了這張投影片。 隨著模型能力提升，他們移除了部分原本用來維持長任務穩定性的機制，例如拆分多個..."
tags:
  - "clippings"
status: processed
---
5

## 【AI Agent 架構】Agent 不是越複雜越好：哪些機制該留，哪些可以拿掉？

- 分享至

我在 San Francisco 參加一場 AI Agent 講座時，一位 Anthropic 工程師分享了這張投影片。

隨著模型能力提升，他們移除了部分原本用來維持長任務穩定性的機制，例如拆分多個 sprint，以及限制單次 session 長度

為什麼？

因為模型變強後，某些原本用來補足能力的設計，不再帶來足夠幫助，甚至可能限制模型發揮

這也帶出一個重要觀念：

> **AI Agent 架構不是越複雜越好**

Planner、Evaluator、Memory、Subagent 和 Workflow 看起來都很先進，但每個元件都會增加 token 消耗、延遲、協調成本與除錯範圍

真正要問的不是「這個功能要不要加」，而是：

> **它正在解決哪個可以被觀察與驗證的問題？**

---

## 從真實使用情境決定架構

不要因為 Multi-Agent、Memory 或 Loop Engineering 看起來厲害，就直接加進系統

![https://ithelp.ithome.com.tw/upload/images/20260728/20183246xkULCc0As7.png](https://ithelp.ithome.com.tw/upload/images/20260728/20183246xkULCc0As7.png)

比較合理的做法，是 **先從真實任務、執行紀錄和過去發生的問題建立測試案例**

但很多 Agent 處理的是研究、軟體開發、資料分析或複雜決策，沒有唯一標準答案

因此，測試不一定只是判斷答案正確或錯誤，也可以評估：

- 任務完成度與結果品質
- Grounding 與限制遵守
- 工具使用是否合理
- 錯誤恢復與人工接管率
- 延遲、token 消耗與成功任務成本

接著使用同一組測試案例，比較兩個版本：

一個保留該機制，一個移除

假設你想知道 Evaluator 是否真的有用，就分別測試有 Evaluator 和沒有 Evaluator 的版本

如果加入後沒有帶來可衡量的改善，就不應該只因為它看起來先進而保留

只有實際測試，才能知道一個架構元件真的解決了問題，還是只增加延遲、成本和除錯難度

---

## 但不是所有機制都能用這套邏輯移除

模型變強後，可以拿掉部分能力機制，但不能因此拿掉控制機制

![https://ithelp.ithome.com.tw/upload/images/20260728/20183246hqYBh9lVDT.png](https://ithelp.ithome.com.tw/upload/images/20260728/20183246hqYBh9lVDT.png)

更強的模型可能不再需要：

- 過度拆解任務
- 多次重試
- 每個步驟都呼叫 Evaluator
- 為了避免 Context 過長而強制切斷流程

這些設計主要是在補足模型當時做不到的事情。模型升級後，可以重新用測試案例判斷是否仍有必要

但 permissions、sandbox、audit log、budget limits 和 human approval 不一樣

它們不是為了讓模型更聰明，而是為了限制系統能做什麼，以及出事時由誰負責

模型變強，可能降低犯錯機率，但不會降低一次嚴重錯誤造成的影響

一個更聰明的 Agent，也不代表它應該擁有更大的資料庫刪除權限

即使測試案例顯示 Agent 很少需要人工核准，也不代表可以取消高風險操作前的核准流程。這只能說明模型表現變好了，不代表不可逆操作的風險消失了

---

## Capability Scaffolding 與 Control Plane

我會把 Agent Harness 分成兩類：

| 類型 | 目的 | 常見機制 |
| --- | --- | --- |
| **Capability scaffolding** | 幫助模型完成任務 | Task decomposition、Retry、Evaluator、Memory、Subagent |
| **Control plane** | 管理權限、狀態、成本與副作用 | Permissions、Sandbox、Audit log、Budget limits、Human approval |

兩者的判斷方式不同

如果一個機制是在補模型能力，就應該用測試案例測試。沒有帶來改善，就可以考慮移除

如果一個機制是在管理政策、究責或不可逆風險，那就不是在補足模型能力，而是系統邊界

不能因為模型變強，就把這些邊界一起拿掉

---

## 總結

設計 Agent 架構時，可以依序問三個問題：

1. 這個機制要解決哪一種實際問題？
2. 加入後，能否在真實任務的測試中帶來明顯改善？
3. 它是在幫助模型完成任務，還是在限制權限與風險？

好的 Agent 架構，不是功能最多的架構

而是用最少的必要機制，穩定完成任務，同時把權限、成本和副作用控制在可接受範圍內。

我在 **Awesome Agent Architecture** 裡，把 Agent Harness 拆成七層、21 個核心機制，從最小的 Agent Loop、Tool Use、Memory、Context，一路講到 Multi-Agent、Observability、Evaluation 和 Loop Engineering

**只有理解每個機制在解決什麼問題，才能判斷它是否適合自己的使用情境，而不是看到新的架構就直接加進系統**

覺得有幫助的話，歡迎到 GitHub 幫我按個 Star：

[https://github.com/hardness1020/awesome-agent-architecture](https://github.com/hardness1020/awesome-agent-architecture)

歡迎一起交流！

---

## 延伸閱讀：30 天 AI Agent 架構鐵人賽

如果你想更有系統地理解 AI Agent 架構，我最近也把 Awesome Agent Architecture 整理成 30 天 iT 邦幫忙鐵人賽系列。

這個系列會依照 repo 的內容，從最小可運作的 Agent Loop 開始，逐章拆解工具使用、Context、Memory、Planning、Multi-Agent、Harness 與 Graph Engineering，並搭配 Python 範例。

系列入口：

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/10/5a8abfe5-156d-4f08-a876-62c23d668038_small.png)

[Flowmon - 結合MITRE 與 Netflow 達成CP值最高的資安事件應變手段](https://itplus.ithome.com.tw/vod-page/489?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/21/2a8be1d3-b145-48c6-9f47-81bd4605bb02.png)

[Implement Engineering Operations Dashboard with Faros CE](https://itplus.ithome.com.tw/vod-page/786?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/ead58431-65f0-494c-b0bd-a95f3d606b84_small.png)

[導入 OPA 在金融業監控 k8s 部署檔的安全性](https://itplus.ithome.com.tw/vod-page/1054?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/25/4fc60419-2101-4dff-9d0c-aa8ec1053505_small.png)

[從制品管理到供應鏈防護：用 JFrog 打造現代 DevSecOps 流程](https://itplus.ithome.com.tw/vod-page/1157?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

37 分