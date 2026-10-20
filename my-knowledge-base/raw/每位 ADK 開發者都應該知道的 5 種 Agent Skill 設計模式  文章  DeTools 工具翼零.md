---
title: "每位 ADK 開發者都應該知道的 5 種 Agent Skill 設計模式 | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3829"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-03-19
created: 2026-05-18
description: "本文整理 AI Agent SKILL.md 的 5 種常見設計模式，包含 Tool Wrapper、Generator、Reviewer、Inversion 與 Pipeline，說明如何設計 skill 的內容邏輯，而不只是遵循格式。透過這些模式，開發者可以建立更可靠、可維護的 AI Agent workflow。"
tags:
  - "clippings"
status: processed
---
這是 Google 分享的 [5 Agent Skill design patterns every ADK developer should know](https://x.com/GoogleCloudTech/status/2033953579824758855) 技術文章，可以給要設計 Agent Skills 的朋友參考。以下為翻譯內容:

\--

談到 **SKILL.md** 時，開發者往往會過度專注於格式——例如 YAML 是否正確、資料夾結構如何安排、是否遵循規範。  
但現在已有 **超過 30 種 agent 工具** （例如 Claude Code、Gemini CLI、Cursor）採用相同的技能檔案結構，因此「格式問題」幾乎已經不存在。

現在真正的挑戰是 **內容設計（content design）** 。

規範只說明了如何「封裝」一個 skill，但 **完全沒有告訴你 skill 裡的邏輯該怎麼設計** 。例如：

- 一個封裝 FastAPI 開發規範的 skill
- 一個四步驟文件生成流程的 skill

雖然它們的 **SKILL.md** 外觀完全相同，但實際運作方式卻截然不同。

透過研究整個生態系中的 skill 設計方式——從 **Anthropic 的 repo、Vercel 到 Google 的內部指南** ——可以發現有 **五種常見的設計模式** ，能幫助開發者打造更好的 agent。

本文會介紹每一種模式，並附上可運作的 ADK 範例：

- **Tool Wrapper** ：讓 agent 對任何函式庫立即成為專家
- **Generator** ：用模板產生結構化文件
- **Reviewer** ：依嚴重程度用 checklist 評分程式碼
- **Inversion** ：agent 在行動前先「訪談」你
- **Pipeline** ：用檢查點強制執行多步驟流程

![](https://farm66.static.flickr.com/65535/55156007679_32d226b705_b.jpg)

## 模式 1：Tool Wrapper

**Tool Wrapper** 讓 agent 可以在需要時取得某個函式庫的上下文。

與其把 API 規範硬寫進 system prompt，不如把它們打包成一個 **skill** 。  
這樣 agent **只有在使用該技術時才載入相關規則** 。

![](https://farm66.static.flickr.com/65535/55155772641_346693c0c5_b.jpg)

這是最簡單的一種模式。

**SKILL.md 的運作方式：**

1. 監聽使用者 prompt 中的特定函式庫關鍵字
2. 從 `references/` 目錄動態載入內部文件
3. 把這些規則當作「絕對標準」

這個機制常用於：

- 發佈團隊的內部 coding guideline
- 將 framework best practices 直接整合到開發流程

下面是一個 **FastAPI 專家 Tool Wrapper** 的例子：

\# skills/api-expert/SKILL.md  
  
\---  
name: api-expert  
description: FastAPI development best practices and conventions. Use when building, reviewing, or debugging FastAPI applications, REST APIs, or Pydantic models.  
metadata:  
pattern: tool-wrapper  
domain: fastapi  
\---  
  
You are an expert in FastAPI development. Apply these conventions to the user's code or question.  
  
\## Core Conventions  
  
Load 'references/conventions.md' for the complete list of FastAPI best practices.  
  
\## When Reviewing Code  
1\. Load the conventions reference  
2\. Check the user's code against each convention  
3\. For each violation, cite the specific rule and suggest the fix  
  
\## When Writing Code  
1\. Load the conventions reference  
2\. Follow every convention exactly  
3\. Add type annotations to all function signatures  
4\. Use Annotated style for dependency injection

## 模式 2：Generator

如果 **Tool Wrapper 是套用知識** ，  
那 **Generator 的目的是強制輸出一致格式** 。

![](https://farm66.static.flickr.com/65535/55156007674_9d6fc1c219_b.jpg)

很多人遇到的問題是：

> Agent 每次生成的文件結構都不同

Generator 透過 **模板填空流程** 解決這個問題。

它會用到兩個資料夾：

- `assets/` → 輸出模板
- `references/` → 風格指南

Skill 的指令像是一個 **專案經理** ：

1. 載入模板
2. 讀取 style guide
3. 向使用者詢問缺少的資訊
4. 填入模板生成文件

適用場景：

- API 文件生成
- commit message 標準化
- 專案架構 scaffold

範例：

\# skills/report-generator/SKILL.md  
  
\---  
name: report-generator  
description: Generates structured technical reports in Markdown.  
metadata:  
pattern: generator  
output-format: markdown  
\---  
  
You are a technical report generator.  
  
Step 1: Load 'references/style-guide.md'  
  
Step 2: Load 'assets/report-template.md'  
  
Step 3: Ask the user for missing information  
\- Topic  
\- Key findings  
\- Target audience  
  
Step 4: Fill the template  
  
Step 5: Return the completed report

## 模式 3：Reviewer

**Reviewer 模式把「檢查什麼」與「怎麼檢查」分離。**

與其在 system prompt 寫一長串規則，不如把檢查標準放在：

references/review-checklist.md

![](https://farm66.static.flickr.com/65535/55155772626_9ce761d425_b.jpg)

當使用者提交程式碼時：

1. agent 載入 checklist
2. 逐條檢查
3. 依嚴重程度分類

例如把 checklist 從

- Python style guide

換成

- OWASP security checklist

就能變成 **安全稽核 agent** 。

這種模式非常適合：

- 自動 PR review
- 提前發現漏洞

範例：

\# skills/code-reviewer/SKILL.md  
  
\---  
name: code-reviewer  
metadata:  
pattern: reviewer  
severity-levels: error,warning,info  
\---  
  
You are a Python code reviewer.  
  
Step 1: Load 'references/review-checklist.md'  
  
Step 2: Analyze the code  
  
Step 3: For each violation  
\- Note line number  
\- Classify severity  
\- Explain why  
\- Suggest fix  
  
Step 4: Output  
\- Summary  
\- Findings  
\- Score  
\- Top recommendations

## 模式 4：Inversion

Agent 天生會「立即回答」。  
**Inversion 模式會反過來。**

agent 不先生成答案，而是先 **訪談使用者** 。

![](https://farm66.static.flickr.com/65535/55154869212_12a7fe8c9a_b.jpg)

它透過強制規則控制流程，例如：

DO NOT start building until all phases are complete

流程：

1. agent 逐步提問
2. 等待回答
3. 收集完整需求
4. 才開始生成結果

範例：專案規劃 skill

\# skills/project-planner/SKILL.md  
  
metadata:  
pattern: inversion  
interaction: multi-turn

流程：

**Phase 1：問題探索**

- 專案解決什麼問題？
- 目標使用者是誰？
- 預期規模？

**Phase 2：技術限制**

- 部署環境？
- 技術 stack？
- 不可妥協需求？

**Phase 3：生成計畫**

- 載入模板
- 填入資訊
- 與使用者確認
- 迭代修改

## 模式 5：Pipeline

當任務非常複雜時，  
不能讓 agent 跳過步驟。

**Pipeline 模式會強制執行順序流程。**

**![](https://farm66.static.flickr.com/65535/55155772631_c285a19a79_b.jpg)**

核心概念：

- 每一步都有 **checkpoint**
- 未完成不能進入下一步

範例：API 文件生成流程

```
# skills/doc-pipeline/SKILL.md
---
name: doc-pipeline
description: Generates API documentation from Python source code through a multi-step pipeline. Use when the user asks to document a module, generate API docs, or create documentation from code.
metadata:
  pattern: pipeline
  steps: "4"
---

You are running a documentation generation pipeline. Execute each step in order. Do NOT skip steps or proceed if a step fails.

## Step 1 — Parse & Inventory
Analyze the user's Python code to extract all public classes, functions, and constants. Present the inventory as a checklist. Ask: "Is this the complete public API you want documented?"

## Step 2 — Generate Docstrings
For each function lacking a docstring:
- Load 'references/docstring-style.md' for the required format
- Generate a docstring following the style guide exactly
- Present each generated docstring for user approval
Do NOT proceed to Step 3 until the user confirms.

## Step 3 — Assemble Documentation
Load 'assets/api-doc-template.md' for the output structure. Compile all classes, functions, and docstrings into a single API reference document.

## Step 4 — Quality Check
Review against 'references/quality-checklist.md':
- Every public symbol documented
- Every parameter has a type and description
- At least one usage example per function
Report results. Fix issues before presenting the final document.
```

## 如何選擇合適的 Agent Skill 模式

每種模式都解決不同問題：

| 模式 | 解決問題 |
| --- | --- |
| Tool Wrapper | 如何讓 agent 精通某個技術 |
| Generator | 如何讓輸出格式一致 |
| Reviewer | 如何系統化審查 |
| Inversion | 如何先收集需求 |
| Pipeline | 如何強制流程順序 |

## 模式可以組合使用

這些模式 **不是互斥的** 。

例如：

- Pipeline 最後加入 **Reviewer** 檢查結果
- Generator 開頭用 **Inversion** 收集資訊

透過 **ADK 的 SkillToolset 與 progressive disclosure** ：

agent 只會載入 **當前需要的 pattern** ，節省 context token。

## 不要再把所有指令塞進一個 System Prompt

複雜工作流程不應該塞在單一 prompt。

正確方式是：

## 現在就開始

**Agent Skills 規範是開源的，並原生支援 ADK。**

你已經知道如何封裝格式。  
現在你也知道 **如何設計內容** 。

使用 **Google Agent Development Kit**  
打造更聰明、更可靠的 AI Agent。