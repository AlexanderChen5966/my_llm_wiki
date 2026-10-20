---
title: "Claude Code 五層架構解析：打造高效能 AI 智慧體開發套件"
source: "https://vocus.cc/article/69facfcffd8978000168ef78"
author:
  - "[[HKT實驗室]]"
published: 2026-05-06
created: 2026-05-18
description: "本篇文章詳細介紹 Claude Code 的五層架構（記憶層、知識層、護欄層、委派層、分發層）透過分層權責與隔離機制，系統性地解決 AI 智慧體開發中的上下文汙染、權限控管與任務委派等痛點。"
tags:
  - "clippings"
status: processed
---
![zoomable](https://images.vocus.cc/ab3c855a-b56f-413b-936d-ce8077592088.jpg)

> 從記憶層到分發層，徹底理解智慧體開發的全貌

為什麼會看到廣告

最近在研究 ****Claude Code**** 的「Agent Development Kit」（智慧體開發套件）時，發現一個非常實用的五層架構設計。這套架構不僅讓 AI 智慧體的開發更有條理，還能有效解決長年以來困擾開發者的「上下文汙染」、「權限控管」、「任務委派」等痛點。

以下就根據官方架構圖，為大家拆解這五層的設計理念與實務應用。

---

## 層級總覽：從核心記憶到生態分發

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F12df5259-2d98-46c6-8032-a7df3121e786.png&width=740&sign=0yoOVx9edVekWNqe8nTbZe0m71SVr0psSLxpsVx3aJM) ![zoomable](https://images.vocus.cc/12df5259-2d98-46c6-8032-a7df3121e786.png)

---

## Layer 1 – CLAUDE.md：記憶層（The Memory Layer）

這是整個架構的 ****基礎層**** ，也是所有規則被載入的起點。CLAUDE.md 分為三個層次：

- ****全域層級（~/.claude/CLAUDE.md）**** ：永遠被載入，定義全專案通用的架構規則、命名慣例、測試期望值。
- ****專案層級（.claude/CLAUDE.md）**** ：放在各專案根目錄，描述該專案的「憲法」與 repository 地圖。
- ****代理層級（CLAUDE.md）**** ：給特定智慧體使用的規則檔。

> 💡 這層就像是 ****系統的 BIOS + 作業系統核心**** ，一開機就存在，定義了所有行為的基本邊界。

---

## Layer 2 – SKILLS：知識層（The Knowledge Layer）

當任務需要特定領域知識時，智慧體不會自己瞎猜，而是呼叫 ****SKILL**** 。每個 SKILL 是一個獨立的知識包，內含：

- ```
	SKILL.md
	```
	（說明文件 + 觸發條件描述）
- 腳本（scripts）
- 範本（templates）
- 參考文件（reference docs）

關鍵機制是 ****自動比對**** ：系統會根據任務描述觸發對應的 SKILL，然後在一個 ****隔離的子智慧體（subagent）**** 中執行，用完就釋放，不會長期占用主對話的上下文。

> 💡 這就像 ****隨身碟或外接知識庫**** ，需要時插上，用完就拔掉，主記憶體永遠保持乾淨。

---

## Layer 3 – HOOKS：護欄層（The Guardrail Layer）

這層是最有「工程感」的設計。Hook 機制類似 Git hooks，但改為針對 ****AI 智慧體的生命週期事件**** 。支援的事件包括：

- ```
	PreToolUse
	```
	```
	PostToolUse
	```
	– 工具執行前後
- ```
	SessionStart
	```
	```
	SessionStop
	```
	– 會話開始與結束
- ```
	SubagentStop
	```
	– 子智慧體結束時

每個事件觸發後，會經過 ****匹配器（Matcher）**** 檢查，然後執行對應的命令（Command）。常見應用場景：

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fc6d7da13-0113-4487-8b38-424b1d9ca801.png&width=680&sign=lPOkJhSUivoiDad8Td6cmKxmYwUue8IpGFjVA9Vz1CU) ![zoomable](https://images.vocus.cc/c6d7da13-0113-4487-8b38-424b1d9ca801.png)

> 💡 這層可以想像成 ****CI/CD 的 pipeline + 資安防線**** ，用確定性邏輯（deterministic）補足 AI 的不確定性。

---

## Layer 4 – SUBAGENTS：委派層（The Delegation Layer）

這是解決 ****上下文無限膨脹**** 的關鍵設計。主智慧體可以將子任務委派給專門的 ****子智慧體（subagent）**** ，例如：

- ```
	code-reviewer
	```
	– 專門負責程式碼審查
- ```
	test-runner
	```
	– 專門執行測試
- ```
	explorer
	```
	– 專門探索未知的程式碼庫

每個子智慧體擁有 ****自己的上下文視窗**** ，不會汙染主智慧體。更重要的是，子智慧體 ****不能再產生子智慧體**** （no infinite recursion），確保遞迴深度可控。

> 💡 這就像 ****將工作拆分給不同部門的同事**** ，每個人各自在自己的會議室裡作業，最後只把結果回報給專案經理。

---

## Layer 5 – PLUGINS：分發層（The Distribution Layer）

最後一層負責 ****封裝與分享**** 。你可以把以上所有東西 —— 自訂的 SKILL、HOOK、SUBAGENT、CLAUDE.md —— 打包成一個 ****Plugin**** ，透過 Marketplace 發布，讓團隊成員用一行指令安裝。

Plugin 的結構類似 npm 套件：

```javascript
my-agent-plugin/
├── skills/
├── agents/
├── commands/
├── hooks/
└── CLAUDE.md
```

> 💡 這層讓 AI 智慧體的能力可以像 npm 或 GitHub Actions 一樣 ****共享、版本控制、團隊協作**** 。

---

## 總結：五層架構如何協同運作？

1. ****啟動時**** ：Layer 1 (CLAUDE.md) 永久載入，定義全局規則。
2. ****遇到任務**** ：Layer 2 (SKILLS) 自動匹配需要的知識，並在隔離環境執行。
3. ****執行動作前後**** ：Layer 3 (HOOKS) 攔截事件，進行檢查、通知、防護。
4. ****需要分工**** ：Layer 4 (SUBAGENTS) 產生子智慧體，各自處理子任務，保持主上下文乾淨。
5. ****成果發布**** ：Layer 5 (PLUGINS) 將可重用的能力封裝，分享給整個團隊。

這套架構最聰明的地方在於： ****讓 AI 發揮非確定性創造力的同時，用工程化的手段加上邊界與護欄**** 。不再是「對話機器人」，而是真正可以落地到企業開發流程的 ****AI 智慧體平台**** 。

如果你也在設計 AI Agent 系統，非常推薦參考這個五層模型。特別是 Layer 3 的 Hook 與 Layer 4 的 Subagent，幾乎是所有生產級應用的必備機制。

**本文內容整理自 Claude Code Agent Development Kit 官方架構圖，**