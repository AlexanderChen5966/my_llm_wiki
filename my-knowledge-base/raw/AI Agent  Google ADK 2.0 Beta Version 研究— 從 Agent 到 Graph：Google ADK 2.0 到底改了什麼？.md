---
title: "[ AI Agent ] Google ADK 2.0 Beta Version 研究— 從 Agent 到 Graph：Google ADK 2.0 到底改了什麼？"
source: "https://medium.com/@simon3458/google-adk-2-0-beta-version-2026-757c0e8d6968"
author:
  - "[[Simon Liu]]"
published: 2026-05-18
created: 2026-05-19
description: "TL;DR — ADK 1.x 是「以 Agent 為中心」：透過 LlmAgent讓協調者 LLM 自己決定誰要講話。ADK 2.0 是「以圖為中心」：你畫一張 Workflow(edges=[...])，把平行、路由、迴圈、human-in-the-loop 全部寫死在 co"
tags:
  - "clippings"
status: processed
---
> ***TL;DR —*** *ADK 1.x 是「以 Agent 為中心」：透過* `*LlmAgent*` *讓協調者 LLM 自己決定誰要講話。ADK 2.0 是「以圖為中心」：你畫一張* `*Workflow(edges=[...])*` *，把平行、路由、迴圈、human-in-the-loop 全部寫死在 code 裡。前者上手快但流程不受控；後者可測試、可審核，但目前還是* ***Beta、session 跟 1.x 不相容*** *。*

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*oUahOayZHVNcAEbRQk8Rcg.png)

\[ AI Agent \] Google ADK 2.0 Beta Version 研究 — 從 Agent 到 Graph：Google ADK 2.0 到底改了什麼？

## 一個標準 SOP 的場景

先講個你大概很熟的情境。你要做一個「主題簡報產生器」：給一個題目，先 **並行** 做兩件事 — — 找切角、找事實；合流後寫草稿；再讓一個「嚴格編輯」agent 評分； **沒過就帶著回饋重寫** ，過了才輸出。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_VOVdEJXIz_4MtCPj7MN-g.png)

不過這流程在 ADK 1.x 寫起來會相對彆扭：因為 `ParallelAgent` 包兩個研究 agent 還好，但「合流後依結構化的評審結果決定要不要回去重寫」就開始跟協調者 LLM 的自由意志搏鬥——它有時乖乖 transfer，有時自己腦補做完，迴圈次數還得靠 `LoopAgent` 加 prompt 哄。流程對不對，幾乎等於 prompt 寫得好不好。

ADK 2.0 就是為了這個痛點而生。

## 設計哲學：協作的骨架，從 prompt 搬回程式碼

ADK 1.x 把「協作」交給語言模型：root agent 讀指令、決定要不要 `transfer` 給某個 `sub_agent` 。Demo 很漂亮，可是用在 Production 相對就會很辛苦。主要還是在 ADK 2.0 的核心轉變：

> **流程不再是 prompt，而是一張圖。LLM 只負責節點裡的智慧，圖負責流程的紀律。**

用心智模型對比：

- **1.x** 是「我有一群專家，誰說話讓大型語言模型協調決定」，流程藏在 prompt 與 `SequentialAgent` 之類的容器裡，控制流隱式、由模型決定。
- **2.0** 是「我畫流程圖：哪裡並行、哪裡分支、哪裡迴圈、哪裡等人」，流程就是程式碼裡的 `edges` ，明確且決定性。

比較表如下：

## 同一條 pipeline，寫兩次給你看

以下是 ADK 1.0 和 2.0 針對同樣情境，所做的程式碼：

兩個重點差異：

(1) import 從 `from google.adk.agents import LlmAgent` 變成 `from google.adk import Agent, Workflow, Event`

(2) 節點可以是 **純 function** —— `process_input` 不是 agent、也不是 tool，就是一個 `yield Event(...)` 的 Python function，而且它在圖裡是一等公民。

## Get Simon Liu’s stories in your inbox

Join Medium for free to get updates from this writer.

**真正展現 2.0 威力的那段（前面 SOP 簡報製作的場景）：**

```c
from google.adk import Agent, Event, Workflow
from google.adk.workflow import JoinNode
from pydantic import BaseModel, Field
from typing import Literal

class Verdict(BaseModel):
    grade: Literal["good", "revise"]
    feedback: str = Field(default="")
research_a = Agent(name="research_angles", model=MODEL,
                   instruction="角度：{topic}", output_key="angles")
gather_f   = Agent(name="gather_facts", model=MODEL,
                   instruction="事實：{topic}", output_key="facts")
join       = JoinNode(name="join_research")
write = Agent(name="write_brief", model=MODEL,
              instruction="用 {angles} 與 {facts} 寫；評語：{feedback?}",
              output_key="draft")
grade = Agent(name="grade_brief", model=MODEL, instruction="評審 {draft}",
              output_schema=Verdict, output_key="verdict")
def route_quality(verdict: Verdict):
    yield Event(state={"feedback": verdict.feedback}, route=verdict.grade)
def process_input(node_input: str):
    yield Event(state={"topic": node_input, "feedback": ""})
def finalize(draft: str):
    yield Event(message=f"✅ {draft}")
root_agent = Workflow(
    name="smart_brief",
    edges=[
        ("START", process_input,
            (research_a, gather_f),   # fan-out（平行）
            join,                     # fan-in
            write, grade, route_quality),
        (route_quality, {
            "revise": write,          # ← 退回去重寫的迴圈
            "good":   finalize,
        }),
    ],
)
```

`edges` 讀一遍整條流程就懂了：

平行跑兩個研究 → JoinNode 合流 → 寫稿 → 評審 → 看結構化結果決定路由； `"revise"` 就把邊指回 `write` 形成迴圈， `"good"` 才走 `finalize` 。這條路在 1.x 幾乎沒辦法寫得乾淨；在 2.0，它就只是圖裡的一段宣告而已。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*GBpNqlY9fw_bo_BSKKmoQg.png)

## 底層到底差在哪：執行模型

## 那我到底該不該在此時間升 ADK 2.0？

> 筆者就 5/18 的這個時間點做比較，當然後續等 ADK GA 之後，我相信最終應該是要上到 ADK 2.0，只是正式版本發佈後，理論上猜測應該會有更好 Migration 的方法。

判斷其實很單純：

- **要上 production、或會動到既有 session 資料 → 留在 ADK 1.x** （2.0 目前仍是 Beta 版本，pre-release 之間就可能有破壞性改動，而且千萬不要把 2.0 指到 1.x 的資料庫）。
- **在做原型、評估、或想先熟悉新的流程模型，而且需要明確 / 平行 / 迴圈 / HITL → 那就上 ADK 2.0** ，但請在獨立 venv、版本鎖死的前提下玩。

## 結語

總結來說，Google ADK 2.0 的改版核心，就是把 Agent 開發從「Prompt Engineering」拉回了「踏實的軟體工程」。

ADK 1.x 讓我們見識到了大型語言模型自主協作的潛力，寫幾個 Agent 就能跑出令人驚豔的 Demo。然而，當我們試圖將這些框架導入真實的業務場景，或建立龐大的 Agent 生態系時，往往會因為流程失控、除錯困難而感到痛苦。

ADK 2.0 引入的 Workflow 與 Graph 概念，正是為了解決從「玩具」走向「Production」的痛點。它讓平行處理、條件路由、複雜迴圈以及人類介入（HITL）這些在企業級應用中不可或缺的環節，變得 **明確、可預期且可測試** 。即便目前它仍處於 Beta 階段，架構上的大轉彎無疑是面對開發者真實需求所交出的誠意之作。

## 範例專案

大家可以根據我和 Coding Agent 撰寫出來的專案，測試看看 ADK 2.0 帶來的優點吧！

## [GitHub - LiuYuWei/adk-two-example: \[ Research \] ADK 2.0 Project - A2A Protocol Research Project](https://github.com/LiuYuWei/adk-two-example/tree/main?source=post_page-----757c0e8d6968---------------------------------------)

### Research \] ADK 2.0 Project - A2A Protocol Research Project - LiuYuWei/adk-two-example

github.com

## I am Simon

大家好，我是 Simon 劉育維，是一位 AI 領域解決方案專家，目前也擔任 Google Cloud AI 領域開發者專家 (GDE)，期待能夠幫助企業導入人工智慧相關技術解決問題。如果這篇文章對您有幫助，請在 Medium 上按一下鼓勵，並追蹤我的個人帳號，這樣您就可以隨時閱讀我所撰寫的文章。歡迎在我的 Linkedin 上留言提供意見，並與我一起討論有關人工智慧的主題，期待能夠對大家有所幫助！

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*0lBbniNHiJjFZVMN.png)

My Personal Website: https://simonliuyuwei.my.canva.site/link-in-bio