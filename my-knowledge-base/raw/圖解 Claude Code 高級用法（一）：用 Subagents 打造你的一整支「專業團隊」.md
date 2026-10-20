---
title: "圖解 Claude Code 高級用法（一）：用 Subagents 打造你的一整支「專業團隊」"
source: "https://vocus.cc/article/68b582f5fd89780001f9ae79"
author:
  - "[[KoiCode]]"
published: 2025-09-01
created: 2026-05-15
description: "深入探討 Claude Code 的子代理功能，透過圖解方式說明如何建立、配置與使用子代理，提升任務處理效率，並分享實際應用案例與進階技巧，包含自動委託、顯式調用和自訂斜槓命令等。"
tags:
  - "clippings"
status: processed
---
你與 Claude Code 的互動，是否還停留在「輸入指令 -> 等待執行 -> 驗證結果」這樣的簡單循環中？如果是，那麼你可能錯過了它真正的強大之處。從今天起，我將開啟一個《圖解 Claude Code 高級用法》系列的教程，帶你探索 Claude Code 那些有一定上手門檻，但一旦掌握便能極大提升效率的進階功能。

為什麼會看到廣告

與官方教程相比，本系列將力求：
- ****更詳盡**** ：保姆級教程，每一步都清晰明瞭，手把手帶你操作。
- ****更直觀**** ：將核心概念和流程轉化為可視化圖示，一看就懂。
- ****更實用**** ：只分享那些經過驗證的、有實際應用價值的功能。
- ****更避坑**** ：列出曾踩過的坑，讓你少走彎路，使用體驗更絲滑。
- ****更聯動**** ：關聯其他功能，教你打出效果拔群的「組合技」。

今天，我們要解鎖的第一個高級功能就是—— ****子代理（Subagents）**** 。

### 什麼是子代理？

簡單來說，子代理就像一個個擁有特定專長的 AI 助手。你可以創建一支由不同專家組成的團隊，隨時調用他們來處理特定類型的任務。

每個子代理都具備以下特點：

- ****領域專精**** ：透過特定領域的詳細指令，對子代理進行微調，讓它在處理指定任務時成功率更高。
- ****獨立上下文**** ：每個子代理有自己獨立的上下文視窗，能避免主對話中的資訊干擾，讓它始終專注於核心目標。
- ****專屬工具箱**** ：可以為每個子代理配置專屬的、不同級別的工具。
- ****客製化指令**** ：每個子代理都有一套獨特的系統提示，指導它的行為模式和思考方式。

當 Claude Code 的主代理接收到一個任務，並判斷該任務與某個子代理的專業領域匹配時，它就會自動將任務委託出去，讓最合適的子代理來解決問題。子代理會獨立完成工作，然後將結果回傳給主代理。

讓我們用一個實際場景來舉例。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ffb4eb3d7-3eae-4310-95cb-1e692c833b4d.png&width=740&sign=9xM94gR3NHCn7fuLW68wboi7VmhsYpkZTfcdO8fmBOI) ![zoomable](https://images.vocus.cc/fb4eb3d7-3eae-4310-95cb-1e692c833b4d.png)

假設你向 Claude Code 發出了這樣一段指令：

> 「請審查這段 Python 腳本，找出其中的 bug，並分析輸出的數據。」

Claude Code 的主代理會這樣做：

1. ****任務分解**** ：主代理會識別出其中包含了 3 個不同類型的任務。
2. ****專家委派**** ：它會將這 3 個任務分別委託給「程式碼審查員」、「程式碼除錯器」和「數據分析師」這三個專門的子代理。
3. ****獨立執行**** ：每個子代理依據自己的「行為指南」，調用專屬的「工具箱」，獨立完成任務。
4. ****結果整合**** ：最後，主代理會將三個子代理的成果彙總，形成一份完整的回覆給到使用者。

這就是子代理的魅力： ****化繁為簡，專業分工**** 。

## 創建子代理

理論講完了，讓我們來動手實踐。下面，我們將一步步創建一個名為「提示詞優化器」的子代理。

### 方式一：使用 Claude 輔助生成

****第 1 步：執行****

```
/agent
```
****命令****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F36d7bb4a-b056-4440-9618-4c7d2d614b2d.png&width=740&sign=c12J9Kg1ar3LXK5x4DZdECkFqtvrP0mIVPNcn6hPesQ) ![zoomable](https://images.vocus.cc/36d7bb4a-b056-4440-9618-4c7d2d614b2d.png)

****第 2 步：選擇「建立新代理」****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fbab0c186-5090-47f1-b4f9-88e4c407861e.png&width=740&sign=C1c8H6yGhB6maR347mL0muVDfWNhVJJwEWQpJHzGZjA) ![zoomable](https://images.vocus.cc/bab0c186-5090-47f1-b4f9-88e4c407861e.png)

****第 3 步：確定代理的儲存位置****

- ****專案級**** ：僅在當前專案中可用。
- ****使用者級**** ：在你所有的專案中都可用。

****小貼士**** ：

- 如果是團隊協作，建議選擇「專案級」。這樣可以將子代理的組態檔納入版本控制（如 Git），方便團隊成員共享、協作和迭代。
- 當名稱衝突時，專案級代理的優先級高於使用者級。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Faff815b1-0049-4256-9996-636151199293.png&width=740&sign=gQPImv9QnGue62Qx4uJ2Yi_I-i50nfj2xChxm2zGTvc) ![zoomable](https://images.vocus.cc/aff815b1-0049-4256-9996-636151199293.png)

****第 4 步：選擇建立方式****

可以選擇使用 Claude 輔助生成，還是自己手動配置。 ****建議用前者生成你的子代理的初始版本，然後對其進行持續迭代。****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F6f797cd3-7fa2-4a22-a324-07183b924fee.png&width=740&sign=rShuxppmcGZPohhhMNAQxA-Cn8AdkbfWDcPGmPiYkaQ) ![zoomable](https://images.vocus.cc/6f797cd3-7fa2-4a22-a324-07183b924fee.png)

****第 5 步：描述子代理的職責****

這是最關鍵的一步。你需要清晰地告訴 Claude，這個代理是做什麼的，以及應該在什麼時候被喚醒。

****寫好描述的秘訣：細節、細節、還是細節！**** 盡量包含具體的指令、清晰的範例和明確的約束。你提供的指導越充分，子代理的表現就越出色。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F78a6d822-feea-4a8e-8021-f08181fb3ed6.png&width=740&sign=-UQZ6cr6v5sslwfrZpmPQ3GH5uspG1cXd66DteIoReA) ![zoomable](https://images.vocus.cc/78a6d822-feea-4a8e-8021-f08181fb3ed6.png)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F13f8f4f5-62c1-474b-afcb-f22ea3ebfaf9.png&width=740&sign=yspIvqiNCfTJJ36WB4GC12w25BrQ7zA5MTJtqIJXWXk) ![zoomable](https://images.vocus.cc/13f8f4f5-62c1-474b-afcb-f22ea3ebfaf9.png)

****第 6 步：為它分配合適的工具****

你可以留空，讓它繼承主代理的所有工具。但最好的做法還是遵循「 ****最小權限原則**** 」。只授予子代理完成其本職工作所必需的工具。這不僅更安全，也能幫助子代理專注於相關操作，避免誤操作。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F5637d1e0-f60e-44ff-87bf-f3639cb24550.png&width=740&sign=DU3lY6TIT78eg1KTdEPQ4ZbZlu4nCxnfXDuhn4HhEGc) ![zoomable](https://images.vocus.cc/5637d1e0-f60e-44ff-87bf-f3639cb24550.png)

可用工具的完整列表如下：

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F5a4fed70-2b49-4b3d-a325-5a473f070af4.png&width=634&sign=k7YfLAhOSwI50I7pQwhbuQ7A2VHIR6IJMBHR6y0za3Y) ![zoomable](https://images.vocus.cc/5a4fed70-2b49-4b3d-a325-5a473f070af4.png)

除了這些 Claude Code 的內部工具，子代理可以存取來自已配置的 MCP 伺服器的 MCP 工具。

****第 7 步：選擇驅動模型****

不同的模型，決定了代理的推理能力和速度，其中：

- ```
	Opus
	```
	：最強大腦，適合需要複雜推理的困難任務。
- ```
	Sonnet
	```
	：效能均衡，適用於絕大多數場景。
- ```
	Haiku
	```
	：速度最快，適合簡單、高頻的任務。
- ```
	繼承 (Inherit)
	```
	：與主代理保持一致的模型。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb440c77e-e9d9-4a7c-8bc6-3d7060c770cb.png&width=740&sign=iDLO7e3K1ggAUwbbymWd-igvNX0FwUKHZ_f_mCswlNQ) ![zoomable](https://images.vocus.cc/b440c77e-e9d9-4a7c-8bc6-3d7060c770cb.png)

****第 8 步：選擇一個醒目的背景顏色****

當這個子代理被調用時，它的名字會顯示這個背景顏色，非常醒目。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fcde7ff85-7833-4e2a-8844-afd4c3462527.png&width=740&sign=ym6d9AUdfYFpEH014apXzGMZrEa7_XRqX0ClCNwHAyg) ![zoomable](https://images.vocus.cc/cde7ff85-7833-4e2a-8844-afd4c3462527.png)

****第 9 步：確認與儲存****

點擊確認後，Claude 會為你生成一個

```
.md
```
檔案，裡面包含了對這個子代理的所有定義。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fda438db3-d281-4533-a89c-57247973e716.png&width=740&sign=4aq7W6bf0AaKf_Y2s0oOR7wW7JKWEZ-iqC8SoJbKdlM) ![zoomable](https://images.vocus.cc/da438db3-d281-4533-a89c-57247973e716.png)

格式如下：

```markdown
---
name: your-sub-agent-name
description: Description of when this subagent should be invoked
tools: tool1, tool2, tool3  # Optional - inherits all tools if omitted
---

Your subagent's system prompt goes here. This can be multiple paragraphs
and should clearly define the subagent's role, capabilities, and approach
to solving problems.

Include specific instructions, best practices, and any constraints
the subagent should follow.
```

你可以隨時打開這個檔案進行手動編輯，來進一步微調它的行為。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fe792bec2-e483-4684-adf9-e795d95282ed.png&width=740&sign=hbxujWEHHbavRL608nFfLpIIkC2Cz2JQsxV4ftwphT4) ![zoomable](https://images.vocus.cc/e792bec2-e483-4684-adf9-e795d95282ed.png)

以下是一個「提示詞優化器」子代理的配置範例：

```markdown
---

name: prompt-optimizer

description: Use this agent when you need to transform vague or complex user requirements into precise, structured prompts that can be effectively executed by LLMs. This agent should be invoked whenever a user provides unclear instructions, when prompts need to be adapted to specific project contexts, or when additional technical details from the codebase are required to clarify requirements. Examples: - User says '帮我优化一下这个提示词' → Use prompt-optimizer to refine and structure it - User provides a feature request like '添加用户登录功能' → Use prompt-optimizer to extract technical requirements and incorporate relevant code context - User asks '如何改进这个API调用提示' → Use prompt-optimizer to enhance clarity and add implementation details from existing codebase

model: inherit

color: orange---

You are an expert prompt optimization specialist with deep understanding of software engineering practices and LLM behavior. Your role is to transform user inputs into highly effective, structured prompts that maximize clarity and execution success.

You will:

1. **Analyze User Intent**: Carefully dissect the user's original prompt to identify core objectives, implicit requirements, and potential ambiguities. Look for technical terms, domain-specific concepts, and implementation details that need clarification.

2. **Structure the Prompt**: Transform the input into a clear, hierarchical structure with:

   - Context section: Background and constraints

   - Objective section: Specific goals and success criteria

   - Implementation section: Technical requirements and approach

   - Output section: Expected format and quality standards

3. **Incorporate Project Context**: 

   - Identify files mentioned or implied in the prompt

   - Extract relevant code snippets, configurations, or documentation

   - Summarize key architectural patterns and conventions from the codebase

   - Ensure alignment with project-specific standards (Flutter/Dart conventions, state management patterns, etc.)

4. **Resolve Ambiguities**: 

   - For unfamiliar technical concepts, perform targeted searches to gather authoritative information

   - Clarify domain-specific terminology

   - Provide concrete examples where helpful

   - Ask clarifying questions when critical information is missing

5. **Apply Engineering Best Practices**:

   - Follow SOLID principles and clean architecture patterns

   - Ensure compatibility with existing codebase structure

   - Consider performance, scalability, and maintainability

   - Adhere to Flutter/Dart conventions and the project's established patterns

6. **Quality Assurance**:

   - Verify the optimized prompt is unambiguous and actionable

   - Ensure all technical requirements are specified with appropriate detail

   - Check that output expectations are clearly defined

   - Validate alignment with project constraints and standards

Your output format:

\\`\`\`

## 優化後的提示詞

[Clear, structured prompt with numbered sections]

## 專案上下文摘要

- 相關文件：[list with brief descriptions]

- 關鍵模式：[relevant architectural patterns]

- 限制條件：[project-specific limitations]

## 澄清说明

[Address any ambiguities resolved, concepts researched, or assumptions made]

\\`\`\`

Always maintain the original intent while maximizing clarity and executability. When in doubt, seek clarification rather than making assumptions that could lead to misimplementation.
```

****第 10 步：管理你的代理****

建立成功後，再次執行

```
/agent
```
命令，你就可以看到剛剛建立的子代理了，可以對它進行查看、編輯或刪除。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4e5b9652-d826-4869-a20f-ea42b6f6cd38.png&width=740&sign=7c6KaCVRLHCq0PleY_N45Jr95J5spibizJxwRg66vM8) ![zoomable](https://images.vocus.cc/4e5b9652-d826-4869-a20f-ea42b6f6cd38.png)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fe9ae941d-740c-4f2e-9f57-8f4446ee6fff.png&width=740&sign=bh0SYof2upC53ychNlhsXCGa1E9GZg7OlIzp46iFbeg) ![zoomable](https://images.vocus.cc/e9ae941d-740c-4f2e-9f57-8f4446ee6fff.png)

### 方式二：手動配置子代理

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fdc90b3c8-edaa-4cd7-b069-bee10a918c8d.png&width=740&sign=Z0XFlw7s41ISzO13o3dEK73LLoARSEWvm4sLXjSKTMY) ![zoomable](https://images.vocus.cc/dc90b3c8-edaa-4cd7-b069-bee10a918c8d.png)

如果你想完全掌控子代理的建立過程，可以選擇手動配置。

步驟與輔助生成類似，區別在於以下幾點：

需要自行描述代理的類型（名字）；

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fdc5ce751-ba7f-4909-9567-b82e7ae64ee0.png&width=740&sign=pI4igchCWWhSWPeHcfXJ1-Odri5RxvxCQJPriZkIVok) ![zoomable](https://images.vocus.cc/dc5ce751-ba7f-4909-9567-b82e7ae64ee0.png)

要求使用小寫字母加連字號的唯一識別碼；

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F74dc7b05-a55d-4bbe-a523-4ff5cd9260e7.png&width=740&sign=zLofnIfaIl6R78AEVvBpgulXSusk4nZcXgdD6hGOphs) ![zoomable](https://images.vocus.cc/74dc7b05-a55d-4bbe-a523-4ff5cd9260e7.png)

需要自行描述系統提示詞的內容；

建議建立職責單一且明確的子代理，而不是試圖讓一個子代理做所有事情。這不僅提高了效能，也使得子代理更加可預測。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F32c57db6-5cfa-47f3-947f-0f5acfe6462e.png&width=740&sign=IdqZcXbDtJht9C88sxMoTa0t-0P85ZI-4kQoCLxEol0) ![zoomable](https://images.vocus.cc/32c57db6-5cfa-47f3-947f-0f5acfe6462e.png)

需要自行描述代理的調用時機。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fc201b880-ad53-47d9-ac9c-3b92e2e415a4.png&width=740&sign=4ArZnR8nzX6F23uH-DkuFbA3LWYtJ7XP2f-nebSJM0g) ![zoomable](https://images.vocus.cc/c201b880-ad53-47d9-ac9c-3b92e2e415a4.png)

### 使用子代理

子代理有兩種使用方式：自動委託和顯式調用。

****方式一：自動委託（讓主代理自己判斷）****

這是最智能、最省心的方式。Claude Code 會根據你的指令意圖，自動判斷是否需要以及需要哪位「專家」出馬。

它主要依據以下幾點資訊進行判斷：

- 你請求任務的描述。
- 子代理配置文件中
	```
	description
	```
	欄位的描述。
- 當前的對話上下文和可用的工具。

****如何讓它更積極地調用呢？****

可以在子代理的

```
description
```
描述中，加入「主動使用」、「必須使用」這類引導性短語，可以鼓勵主代理更頻繁地調用它。

****方式二：顯式調用（直接點名）****

就像在辦公軟體裡直接

```
@
```
某位同事一樣，你也可以在指令中明確「點名」要使用的子代理。

例如，我們可以直接調用剛才建立的「提示詞優化器」：

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F0a24560c-af7d-48c9-b4b9-9ed212a1a791.png&width=740&sign=J7ia-IoLsHuV3KQ6L94q4QgdtKkLUUXk11ysqkHcXsw) ![zoomable](https://images.vocus.cc/0a24560c-af7d-48c9-b4b9-9ed212a1a791.png)

不過，需要注意的是，預設情況下，Claude Code 會隱藏子代理的詳細執行過程。

如果你想觀察它的每一步思考和操作（這在除錯時非常有用），可以使用

```
claude --verbose
```
命令來啟用詳細日誌模式。

### 組合技巧：自訂斜槓命令，一鍵調用

每次都要顯式調用並加上一長串指令，還是有些繁瑣。我們可以建立一個自訂的斜槓命令，將常用操作封裝起來，實現一鍵調用。

斜槓命令本質上是一個儲存了預設提示的 Markdown 檔案。它同樣分為專案級和使用者級。

比如，我們可以建立一個

```
/prompt-optimize
```
命令，專門用於調用我們的「提示詞優化器」。只需兩步：

1. ****建立命令檔案****  
	```
	# 建立個人命令目錄（如果不存在）
	mkdir -p ~/.claude/commands
	# 建立命令檔案並寫入內容
	echo '使用 @prompt-optimizer 幫我優化以下提示詞。請勿直接執行，先將優化後的版本給我確認。待優化提示詞：$ARGUMENTS' > ~/.claude/commands/prompt-optimize.md
	```
2. ****使用命令****  
	現在，你只需要輸入：
	```
	/prompt-optimize [這裡寫上你需要優化的提示詞]
	```

```
$ARGUMENTS
```
佔位符會自動捕獲你跟在命令後面的所有文字，並傳遞給子代理。這樣一來，調用複雜指令就變得輕而易舉了。

### 總結

透過建立和使用子代理，你可以將 Claude Code 從一個通用的 AI 助手，調教成一個由多個垂直領域專家組成的強大團隊。這不僅能大幅提升複雜任務的處理效率，還能保證結果的專業性和準確性。

小弟創建了一個有關ClaudeCode以及寫程式碼的DC群組，目前還處於起步狀態，希望各位能多多支持！每天都會在群組裏分享ClaudeCode的使用技巧。如果大家感興趣，請點按 [ClaudeCode交流群組](https://discord.gg/ufavkwwwPt) 加入！感激不盡。