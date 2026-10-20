---
title: Anthropic Academy：Claude 官方免費學習平台（13 門課程）
date: 2026-06-15
tags: [ai/claude, ai/agent, ai/mcp]
source: https://www.koc.com.tw/archives/645273
type: reference
status: processed
asset_value: 72
---

Anthropic Academy 於 2026 年 3 月推出，整合 Claude 生態系所有學習資源為單一平台。**13 門課程全數免費、自學制，完成後可獲得官方結業證書**，基於 Skilljar 平台管理進度。課程分為一般使用者（6 門）和開發者（7 門）兩軌。

## T（核心論點）

注意兩種證書的差異：
- **結業證書**（課程完成）= 「上過課」
- **Claude Certified Architect（CCA）**（2026-03-12 宣布）= 「能設計生產級 Claude 應用」（2026-07 定案代碼 CCAR-F，詳見 [[claude-official-certifications]]）

兩者難度和價值完全不同。

## B（適用場景）

**一般使用者起點：** Claude 101 → AI Fluency Framework & Foundations → Claude Cowork
**開發者（Claude Code 新手）：** Claude Code 101 → Claude Code in Action → Building with the Claude API → MCP 系列
**開發者（已有經驗）：** 直接從 Claude Code in Action，依需求補 API 與 MCP
**企業團隊：** 基礎課 + Agent Skills + 雲端平台（Bedrock 或 Vertex AI）

## R（課程清單）

### 一般使用者路徑（6 門）

| 課程 | 內容重點 |
|------|---------|
| **Claude 101** | Projects / Artifacts / Skills / Connectors / Enterprise Search / Research Mode |
| **Introduction to Claude Cowork** | Cowork 任務迴圈 / Skills & Plugins / Chrome & Word 擴展 |
| **AI Fluency: Framework & Foundations** | 4E 框架（Effective / Efficient / Ethical / Safe）|
| **AI Fluency: For Students** | 學術研究、職涯規劃、學習策略 |
| **AI Fluency: For Educators** | 教學整合 AI、課程設計、評量 |
| **AI Fluency: Teaching AI Fluency** | 教別人用 AI 的場景設計 |

### 開發者路徑（7 門）

| 課程 | 內容重點 |
|------|---------|
| **Building with the Claude API** | API 設定 / 多輪對話 / Prompt Engineering / Tool Use / RAG / Extended Thinking / Prompt Caching / Agent 架構 |
| **Claude Code 101** | 安裝 / 核准模式 / Plan 模式 / Explore→Plan→Code→Commit 工作流 / 上下文管理 |
| **Claude Code in Action** | 工具系統互動 / 自訂指令 / MCP Server 整合 / Thinking Mode vs Planning Mode |
| **Introduction to Agent Skills** | SKILL.md 撰寫（frontmatter / 觸發條件）/ 目錄結構 / 進階配置（限制工具存取）/ 團隊共享 |
| **Introduction to MCP** | 用 Python 建構 MCP Server & Client / 三大原語（Resources / Tools / Prompts）/ Inspector 測試 |
| **MCP: Advanced Topics** | 生產環境部署 MCP Server |
| **Claude with Amazon Bedrock** | AWS Bedrock 部署 / Tool Use / RAG / Agent 架構 |
| **Claude with Google Vertex AI** | GCP Vertex AI / Vision / PDF 處理 / Citations |

### 平台連結

- Academy 入口：anthropic.skilljar.com
- Claude 101：anthropic.skilljar.com/claude-101
- Claude Code 101：anthropic.skilljar.com/claude-code-101
- Agent Skills：anthropic.skilljar.com/introduction-to-agent-skills

## C（關鍵行動）

- [ ] 開發者路徑：Introduction to Agent Skills 課程（直接對應 SKILL.md 實踐）
- [ ] 若有 MCP 需求：Introduction to MCP 課程（從零建 MCP Server）
- [ ] 完成 Claude Code in Action 並評估是否值得考 CCA 認證

## 補充資料

### 🔴 2026-08 改版：Anthropic Academy → Claude Academy

來源：數位時代 bnext（2026-08-24，https://www.bnext.com.tw/article/91963/claude-academy-chinese-courses）
＋ 赤紅翼零（2026-08-24 發布 / 2026-08-25 收錄，https://tools.wingzero.tw/articles/4034）

**平台變更（本條目正文所述的 Skilljar 版本已過時）**

| 項目 | 舊（本條目正文 2026-06） | 新（2026-08 起） |
|---|---|---|
| 名稱 | Anthropic Academy | **Claude Academy** |
| 網址 | anthropic.skilljar.com（掛在 anthropic.com/learn） | **academy.claude.com**（獨立網域，繁中路徑 `/zh-TW`） |
| 登入 | Skilljar 另行註冊 | **直接用 Claude 帳號授權**（免費帳號即可） |
| 課程數 | 13 門 | **22 門、逾 400 單元** |

舊 Skilljar 平台的修課紀錄可用**相同信箱匯入**（分批開放，部分使用者尚未看到提示）；舊證書不受影響，仍留在舊平台。未登入也能瀏覽完整課程目錄，登入只影響進度記錄與證書發放。

**繁中化程度**：課綱、課程簡介、每一課內文皆為繁體中文；介面功能名稱（Projects / Artifacts / Skills / Connectors）與專有名詞（Constitutional AI）保留原文。**課程影片仍是英文原版**（內嵌 YouTube，講者為 Anthropic 教育團隊，未各語系重錄，可搭配 YouTube 中文字幕）。**Claude Platform 101 截至 2026-08 是唯一仍無繁中版的課程。**

**⚠️ 兩篇來源的單元數不一致——以較新的 bnext 為準**

| 課程 | 赤紅翼零（8/24） | **bnext（8/24，採用）** |
|---|---|---|
| Claude 101 | 13 堂 / 1 測驗 / 約 2.5h | **14 單元** |
| Claude Code 101 | 12 堂 / 1 測驗 / 約 1h | **13 單元** |
| Claude Code 實戰 (in Action) | 9 堂 / 約 1h | **10 單元** |
| Claude Cowork 簡介 | 14 堂 / 約 2.5h | **15 單元** |
| Claude Platform 101 | 13 堂 / 約 1.5h | **14 單元** |
| MCP 簡介 | 10 堂 / 約 1h | **11 單元** |
| MCP：進階主題 | 11 堂 / 約 1.5h | **12 單元** |
| AI 流暢度：框架與基礎 | 14 堂 / 約 4h | **15 單元** |
| AI 能力與限制 | 13 堂 / 約 3.5h | **14 單元** |
| 使用 Claude API 進行開發 | 67 堂 / 8 測驗 / 約 9h | **75 單元** |

差異呈現穩定的 +1 偏移（API 課為 +8），**最可能原因是官方在 8 月改版中為每門課補上單元（例如課程簡介／期末測驗被計為獨立單元），而非任一方記錯**；兩篇日期同為 8/24 但抓取時間不同，Academy 改版期間內容仍在滾動。子代理入門（4 單元）與代理技能簡介（6 單元）兩篇一致。

**四大分類（bnext 官方頁面分法）**
1. **開始使用 Claude**：Claude 101 / Claude Code 101 / Claude Cowork 簡介 / Claude Platform 101
2. **了解 Claude 產品**：Claude Code 實戰 / MCP 簡介 / MCP 進階主題 / 子代理入門 / 代理技能簡介
3. **精通 AI**：AI 流暢度框架與基礎 / AI 能力與限制 / 教授 AI 流暢度（8），另有七種對象版本——建構者(10)、教育工作者(5)、pK-12 教育者(11)、pK-12 培訓講師(4)、非營利組織(10)、小型企業(10)、學生(6)
4. **使用 API 開發**：使用 Claude API 進行開發(75) / Claude 與 Amazon Bedrock(73) / 透過 Vertex AI 使用 Claude(75)

🔑 **API 三門課單元數是其他課的五倍以上，佔全站過半內容量，但三者核心高度重疊，差異只在部署平台與認證方式——只需修第一門，再依自家雲端平台補一門即可。**

### 🔑 完課徽章 ≠ Pearson/Credly 付費認證（正文 T 層的邊界釐清）

本條目 T 層區分的「結業證書 vs CCA」在改版後有更明確的官方說法：

- **完課徽章（completion badge，官方也稱完課證明）**：在 Claude Academy 完課取得。**不是看完所有單元就發，而是要通過課程的所有測驗**——每單元隨堂測驗加上部分課程的期末測驗全數通過才發。「課都上完卻找不到證書」最常見的原因就是某份測驗沒過。**測驗可無限次重考，無次數上限與冷卻時間，且只需重考沒過的那一份**，已通過的不會被清掉。徽章**不會過期**，記錄的是完成當下的版本，驗證頁標示發放日期；Claude 出新版不需重新認證，課程改版也不追回。每張徽章有**公開驗證連結**可貼 LinkedIn。
- **認證考試**：**付費的監考制度，由 Pearson 監考、透過 Credly 發證**，與 Academy 完全是不同系統，且**目前只開放給 Anthropic 的合作夥伴**，入口不在 academy.claude.com 上。

🔑 **一句話分界：完課徽章證明「上過這門課」（學習紀錄，放履歷沒問題），認證考試證明「通過了考試」（專業資格）——前者不宜當成已取得專業資格。**

> ⚠️ 2026-10-19 名稱區隔：**Claude Frontier Academy**（2026-10-02 宣布）是企業提名、數天實體課程＋12 週組織專案的工程培訓計畫，首批「能力徽章」預計 2027 年初，與本條目的個人免費 Academy 完課徽章、[[claude-official-certifications]] 的 Pearson 認證皆不同，見 [[ideas/anthropic-enterprise-push-frontier-academy-barclays-2026-10]]。

### 學習路線與方法論補充（赤紅翼零）

- **三種顆粒度**：Course（含 Quiz、學習時數、Completion Badge）／Tutorial（數分鐘短篇，如 Getting started with Claude.ai、Intro to Projects、Claude Code Skills 的建立與除錯）／Use Case（實戰案例，如準備一週工作、比較供應商 Proposal、建 Portfolio Website）。這種結構支援「**遇到問題再學**」——想知道 Skill 是什麼先看 3 分鐘 Tutorial，確定要建自己的 Skills 再投入約一小時的完整課程。
- **三條路線**：一般使用者 Claude 101 → AI Fluency → AI Capabilities and Limitations；Claude Code 開發者 Claude Code 101 → Claude Code in Action → Agent Skills／Subagents → MCP；API 產品開發者 Claude Platform 101 → Building with the Claude API → MCP，再依雲端環境補 Bedrock（約 8h）或 Vertex AI（約 8.5h）。
- **AI Fluency 的 4D Framework**：Delegation（決定哪些工作交出去）、Description（把需求說清楚）、Discernment（判斷結果可不可信）、Diligence（做完該做的查核）。
- 🔑 **與第三方 Prompt 課的關鍵差異**：AI Capabilities and Limitations 不把所有問題歸因於「Prompt 寫得不夠好」，而是從 Next Token Prediction、Knowledge、Working Memory、Steerability 四面向建立心智模型，要求先判斷問題出在模型知識不足、工作記憶限制、Steerability 還是生成機制本身，再針對真正原因改工作方式（涵蓋 Knowledge Cutoff、Context Window、何時該用 Web Search 或 Retrieval 補足）。
- **MCP 課涵蓋範圍**：Introduction 用 Python SDK 從頭建 Server 與 Client，涵蓋 Tools / Resources / Prompts 三個 Primitive；Advanced Topics 處理 Sampling、Notifications、Roots 等部署階段才會碰到的機制。

⚠️ **時效提醒**：Claude Academy 改版仍在滾動（單元數、繁中覆蓋率、匯入功能開放批次），本節數字為 2026-08-24／25 兩篇來源的快照。

## 相關連結

- [[claude-official-certifications]] — 🔑 付費認證考試（Pearson 監考 / Credly 發證）那一側；與本篇的完課徽章屬不同系統
- [[free-ai-courses-6-platforms-2026]] — 免費 AI 課程平台橫向比較
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧（Introduction to Agent Skills 課程的對應知識）
- [[skill-md-9-types-and-gotchas]] — Skills 九大類型（課程進階內容）
- [[claude-skills-guide]] — Claude Skills 完整指南
- [[claude-code-context-management]] — Context 管控（Claude Code 101 + in Action 的核心主題）
- [[ccar-f-exam-prep-architecture-judgment-rules]] — 一位考生實際採用的四門課組合（Claude Code in Action／Intro to MCP／Intro to Agent Skills／Claude with the Anthropic API）與 CCAR-F 備考方法（2026-09-23 新增）
