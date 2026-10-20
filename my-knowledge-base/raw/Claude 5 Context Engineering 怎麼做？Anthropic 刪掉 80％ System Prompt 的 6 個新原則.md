---
title: "Claude 5 Context Engineering 怎麼做？Anthropic 刪掉 80％ System Prompt 的 6 個新原則"
source: "https://www.aiposthub.com/claude-5-context-engineering-guide/"
author:
  - "[[Philo]]"
published: 2026-07-25
created: 2026-07-27
description: "Claude 5 Context Engineering 怎麼做？解析 Anthropic 刪除 80％ system prompt 的原因，整理 CLAUDE.md、Skills、memory、references 與 hooks 分工，附實作與安全檢查。"
tags:
  - "clippings"
status: processed
---
Anthropic 在 2026 年 7 月 24 日公開一項很反直覺的調整：面對 Claude Opus 5、Claude Fable 5 等新模型，團隊刪除了超過 80％ 的 Claude Code system prompt，內部 coding evaluations 卻沒有出現可測量的退步。

這不代表 Prompt 已經不重要，也不是叫你把所有規則刪光。真正的變化是：當模型的判斷能力提升，影響成果的重點逐漸從「單次 Prompt 寫得多詳細」，轉向「整個 context 是否清楚、分層，而且沒有互相衝突」。

Context engineering 可以翻成「上下文工程」，指的是有系統地設計模型在執行任務時會收到哪些資訊。除了你輸入的 Prompt，還包括 system prompt、 `CLAUDE.md` 、Skills、memory、工具說明、參考檔案與對話紀錄。

我的結論是：新一代模型需要的不是更少管理，而是更少干擾。該讓模型判斷的地方，不要用幾十條規則綁死；真正不能違反的安全界線，則不能只靠一句「禁止」，而要交給 permissions 或 hooks 強制執行。

## Claude 5 Context Engineering 是什麼？和 Prompt Engineering 差在哪裡？

Prompt Engineering（提示工程）處理的是眼前這一次任務，例如：「請把 8 份訪談整理成產品需求，列出問題、證據與優先級。」

Context Engineering 處理的範圍更大。它要決定模型在許多不同任務中，長期能看到哪些背景、規則、工具與參考資料。例如：

- System prompt：產品的角色、能力與基本行為。
- `CLAUDE.md` ：專案架構、常用指令、團隊慣例與特殊限制。
- Skills：只在特定任務出現時載入的工作流程。
- Memory：Claude 從歷次工作保留的偏好與專案經驗。
- References：規格、測試、範例、mockup、程式碼或資料文件。
- Tools：Claude 可以呼叫的搜尋、檔案、測試與外部服務介面。

兩者不是互相取代。Prompt 說明「這次要完成什麼」，context 則提供「你在哪個環境工作、有哪些資源、平常怎麼做」。如果 context 裡同時出現「視情況補文件」與「絕對不要加註解」，就算這次 Prompt 寫得很清楚，模型仍要先處理衝突。

這也是 Anthropic 所說的 over-constraining：問題不只是文字太多，而是太多重複、過時或互斥的指令，增加模型判斷成本。

![Claude 5 Context Engineering 中 system prompt、Skills 與使用者要求互相衝突的示意圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/07/01-over-constrained-claude.jpg)

同一個 context 同時要求「視情況補文件」「不要加註解」與「照舊版做」，Claude 必須先處理彼此衝突的指令。圖片來源： Thariq Shihipar 的 X Article 。

## Anthropic 為什麼敢刪掉 80％ 的 System Prompt？

[Anthropic 官方文章](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models?ref=aiposthub.com) 表示，團隊針對 Claude Opus 5、Claude Fable 5 移除超過 80％ 的 Claude Code system prompt 後，內部 coding evaluations 沒有觀察到可測量的損失。

這是 Anthropic 在自家模型與評估上的結果，不能直接推論成「每個 Agent 刪掉 80％ 都會一樣好」。但它反映出一個重要轉變：舊模型需要大量明文規則，才能避開常見失誤；新模型更能根據程式碼、使用者要求與周邊資訊做情境判斷，過度具體的通用規則反而可能在特殊任務中做出錯誤限制。

例如，舊式規則可能直接要求「程式碼不要寫多行註解」。這能減少模型產生冗長 docstring，卻也會讓複雜演算法或公開 API 缺少必要說明。Anthropic 新的方向不是列出更多例外，而是要求 Claude 配合現有程式碼的命名、慣例與註解密度。

兩種寫法的差異如下：

| 舊式做法 | 新式做法 | 真正改善的地方 |
| --- | --- | --- |
| 列出大量固定規則 | 提供判斷原則與成果標準 | 能配合不同專案情境 |
| 用多個範例教工具 | 把工具參數與狀態設計清楚 | 減少模型被範例限制 |
| 所有資訊一開始全部載入 | 需要時再載入 Skills 與 references | 降低無關 context |
| 在多處重複同一指令 | 讓每條規則只有一個主要來源 | 減少衝突與維護成本 |
| 把專案知識全塞進 `CLAUDE.md` | `CLAUDE.md` 、memory、Skills 分工 | 每種資訊有適合的位置 |
| 用文字描述理想結果 | 提供測試、rubric 或可執行規格 | 讓模型能自行驗證 |

真正值得學的不是「80％」這個數字，而是刪除的方法：先找出模型已能從環境判斷的內容，再移除重複規則，最後用固定評估確認品質沒有下降。

![Claude 5 Context Engineering 從大量規則轉向判斷、介面、漸進式揭露、自動記憶與豐富參考資料](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/07/02-then-and-now.jpg)

Thariq 將 Claude 5 的 context engineering 變化整理成 6 組 Then／Now 對照。圖片來源： Thariq Shihipar 的 X Article 。

延伸閱讀： [Claude Fable 5 Field Guide：工程師教你發現自己的盲點](https://www.aiposthub.com/fable-guide-find-your-unknowns/)

## 原則一：不要用規則取代模型判斷

規則最容易膨脹的原因，是每次模型犯錯，團隊就在 `CLAUDE.md` 或 system prompt 補上一條「永遠要」或「絕對不能」。時間一久，模型面對的不是一套清楚原則，而是一份歷史事故清單。

比較實際的做法，是把通用風格改成可依環境判斷的原則。

例如：

```
較弱：
不要寫多行註解。每個函式最多只能有一行註解。

較好：
遵循現有程式碼的命名、慣例與註解密度。
只有在意圖無法從程式碼本身看出時，才補充必要註解。
```

後者仍然有方向，但不預先假設所有程式都需要同一種註解密度。對 PM、內容與營運工作也一樣：與其寫「永遠用表格」，不如說明什麼資訊需要比較、讀者最後要做什麼決策，讓模型選擇適合的表達方式。

不過，判斷原則不適合用來保護高風險界線。禁止刪除 production 資料、限制可寫入目錄、保護 API key 等要求，應透過 permissions、sandbox 或 hooks 落實。官方文件也明確區分： `CLAUDE.md` 是行為指引，不是保證執行的安全層。

## 原則二：少給工具範例，多設計好工具介面

過去常見的工具教學，是在 Prompt 中塞入多組呼叫範例，希望模型照著做。但範例同時也會縮小模型的探索空間，讓它誤以為只有範例中的參數組合才有效。

更好的方式是讓介面自己說清楚：

- 參數名稱能不能直接看出用途？
- 必填與選填欄位是否明確？
- 狀態是否使用有限且互斥的選項？
- 錯誤訊息能否告訴模型怎麼修正？
- 工具回傳值能否用來驗證成功，而不只回覆 `ok` ？

以任務管理工具為例，將狀態限制成 `pending` 、 `in_progress` 、 `completed` ，再規定同一時間只能有一項 `in_progress` ，模型就能從 schema 理解工作流程，不需要另外閱讀五個範例。

這對 Agent 產品尤其重要。當模型表現不穩定時，先檢查工具名稱、參數、回傳格式與錯誤訊息，往往比繼續加 Prompt 更容易維護。

![TodoWrite 工具從約 9100 字元範例縮減為明確狀態與單一 in progress 任務限制](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/07/03-tool-interface.jpg)

與其提供約 9,100 個字元的使用時機和範例，清楚的狀態 schema 與限制就能提示 Claude 正確使用 TodoWrite。圖片來源： Thariq Shihipar 的 X Article 。

## 原則三：用 Progressive Disclosure，別把所有資訊一次塞滿

Progressive Disclosure（漸進式揭露）是指先提供足夠判斷方向的資訊，只有任務真的需要時，再載入更完整的內容。

[Anthropic 的 Agent Skills 說明](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills?ref=aiposthub.com) 把 Skills 分成三層：

1. 啟動時只載入 Skill 的名稱與 description，讓 Claude 知道有哪些能力。
2. 任務符合時，才讀取完整 `SKILL.md` 。
3. 需要特定細節時，再讀取 references、scripts 或其他支援檔案。

這種設計的價值不只是節省 token。當模型只看到當前任務需要的規則，真正重要的指令比較不會被大量無關內容稀釋。

延伸閱讀： [Claude Skills 是什麼？免費就能用的 AI 客製化功能完整教學](https://www.aiposthub.com/claude-skills-complete-guide/)

一個實際的專案可以這樣拆：

```
CLAUDE.md
├── 專案用途與架構
├── 常用 build／test 指令
├── 團隊共同慣例
└── 指向特定 Skills

.claude/skills/
├── deploy/
│   ├── SKILL.md
│   └── references/
│       └── rollback.md
├── code-review/
│   └── SKILL.md
└── verify-ui/
    ├── SKILL.md
    └── scripts/
        └── run-e2e.sh
```

部署、code review 與 UI 驗收不需要在每次對話全部載入。讓 Claude 在任務符合時才讀取，context 會更乾淨，流程也更容易分別維護。

## 原則四：讓每條資訊只有一個主要來源

同一條規則若同時出現在 system prompt、 `CLAUDE.md` 、Skill 與工具說明，看似加強提醒，實際上會產生兩個問題。

第一，修改時很容易只改到其中一份，舊版本仍在其他位置生效。第二，不同層級的句子稍有差異，模型就必須判斷哪一條優先。

可以依用途分配：

| 資訊類型 | 建議放置位置 |
| --- | --- |
| 產品角色與所有任務都適用的行為 | System prompt |
| 專案架構、build 指令、團隊慣例 | `CLAUDE.md` |
| 特定檔案或目錄才適用的要求 | Path-scoped rules |
| 可重複執行的多步驟流程 | Skills |
| Claude 從工作中累積的專案經驗 | Auto-memory |
| API、規格、mockup、測試與長篇資料 | References |
| 絕對禁止或必須強制執行的限制 | Permissions、hooks、sandbox |

Anthropic 目前建議將 `CLAUDE.md` 控制在 200 行以內。這不是超過 200 行就會失效，而是提醒團隊：如果檔案持續變長，裡面很可能混入只適用特定任務的流程，應該拆成 rules 或 Skills。

![Claude 5 Context Engineering 的 Prompt、References、System prompt、CLAUDE.md、Skills 與 Memory 分層](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/07/04-context-layers.jpg)

Prompt 只是 Claude 實際 context 的其中一層，References、system prompt、 CLAUDE.md 、Skills 與 memory 也會共同影響結果。圖片來源： Thariq Shihipar 的 X Article

## 原則五：Memory 留經驗，CLAUDE.md 留共同規則

Claude Code 現在把 `CLAUDE.md` 與 auto-memory 分成兩套互補機制。

`CLAUDE.md` 由人撰寫，適合團隊共同遵守的專案資訊；auto-memory 則由 Claude 根據工作過程保存 build 指令、除錯經驗、架構筆記與偏好。根據 [官方 memory 文件](https://code.claude.com/docs/en/memory?ref=aiposthub.com) ，兩者都會進入後續 session 的 context，但用途不同。

這個分工能避免 `CLAUDE.md` 變成雜亂的專案日記。以下內容適合留在 `CLAUDE.md` ：

- 專案使用哪個套件管理工具。
- 測試與 lint 的正式指令。
- API handler 固定放在哪個目錄。
- 團隊共用的命名與 review 規則。

以下內容更適合讓 memory 保存：

- 某個測試在特定環境失敗的原因。
- 上次除錯確認過的特殊依賴關係。
- 使用者反覆修正過的個人輸出偏好。
- 某項流程實際有效的操作順序。

Memory 不是永久真相。版本、路徑與外部服務都可能改變，重要資訊仍應在使用前重新驗證；過時記憶也需要定期清理。

## 原則六：用 Rich References 和驗證迴圈取代長篇說明

模型不一定需要一份把所有需求改寫成文字的簡化 spec。能直接檢查的參考資料，通常更接近真實需求，例如：

- 可執行的 test suite。
- 實際 API schema。
- HTML mockup 或設計元件。
- 既有程式碼中的正確實作。
- 評分 rubric 與驗收案例。

對 Claude Code 來說，一份測試不只是說明文件，也是一個能反覆執行的驗收標準。模型修改程式後可以跑測試、讀取錯誤、修正，再重新驗證。

Anthropic 將這類流程稱為 verification loop。與其在 Prompt 中反覆提醒「請仔細檢查」，不如直接告訴 Claude 要執行哪個測試、什麼結果才算通過，以及失敗後是否允許修正重試。

這個原則也適用於非程式工作。內容團隊可以提供已發布範例、禁用詞與 QA 腳本；產品團隊可以提供可操作的 prototype、事件追蹤表與 acceptance criteria。參考資料越接近最終成果，模型越容易理解你真正要的品質。

## CLAUDE.md、Skills、References 該怎麼重新整理？

如果現有 context 已經累積很多規則，不需要一次重做。比較實際的 MVP 做法，是先整理最常使用的一個專案。

### 第一步：先盤點，不要立刻刪除

列出目前所有 context 來源，包括 system prompt、根目錄與子目錄的 `CLAUDE.md` 、rules、Skills、memory、工具說明與常用參考檔案。

在 Claude Code 中，可以用：

```
/context
/memory
/skills
/doctor
```

`/context` 用來確認 context window 實際載入了什麼； `/memory` 與 `/skills` 分別檢查記憶和 Skills； `/doctor` 則診斷設定、schema、安裝狀態，以及 Skill description 是否因預算不足而被截短。

### 第二步：把內容分成四類

逐條標記：

1. 所有任務都需要知道。
2. 只有特定目錄或工作流程需要。
3. 可以由程式、測試或 hook 強制執行。
4. 已重複、過時，或模型能從環境直接判斷。

第一類留在精簡的 `CLAUDE.md` ；第二類移到 path-scoped rules 或 Skills；第三類改成確定性工具；第四類先列入刪除候選。

### 第三步：用真實任務做刪除測試

不要一口氣刪掉 80％。先建立 10～20 個代表性任務，記錄完成率、人工修正次數、token 用量與常見失敗，再一次移除一組規則。

如果品質不變，代表那組內容可能沒有提供實際價值；如果特定案例退步，就確認問題是規則真的必要，還是缺少更清楚的工具、reference 或驗收標準。

這一步才是 Anthropic 經驗中最容易被忽略的部分。官方能刪除大量 system prompt，是因為後面有 coding evaluations，不是只靠團隊覺得「看起來更簡潔」。

## 哪些規則不能刪？安全界線要比以前更明確

「讓模型自行判斷」不等於把高風險決策也交給模型。

以下要求不應只放在自然語言指令中：

- 禁止存取 secrets 或個人資料。
- 禁止寫入 production。
- 禁止執行特定破壞性指令。
- 財務、醫療或法規流程中的強制審核。
- 只能修改指定目錄或特定資源。

如果違反一次就會造成不可接受的損失，應使用權限、隔離環境、allowlist、deny rule 或 `PreToolUse` hook 攔截。自然語言適合引導判斷；安全控制負責保證邊界。把兩者混在一起，才是最危險的 context engineering。

## FAQ

### Context Engineering 是 Prompt Engineering 的新名稱嗎？

不是。Prompt Engineering 主要設計單次任務的指令；Context Engineering 還要管理 system prompt、專案規則、Skills、memory、工具與參考資料。Prompt 是 context 的其中一部分。

### Claude 5 真的不需要詳細 Prompt 了嗎？

仍然需要清楚說明目標、限制與驗收標準。可以減少的是重複規則、過度指定步驟，以及模型能從環境自行判斷的內容，不是把需求縮成一句模糊指令。

### CLAUDE.md 應該放哪些內容？

適合放專案用途、架構、正式 build／test 指令、團隊慣例與常見陷阱。只適用特定流程的長篇步驟應移到 Skills，只適用特定路徑的要求則用 path-scoped rules。

### Skills 和 References 有什麼差別？

Skills 是可重複執行的工作流程，告訴 Claude 如何完成某類任務；References 是執行時要查閱的詳細資料，例如 API 文件、設計稿、測試規格與範例。Skill 可以在需要時再載入相關 references。

### /doctor 會自動幫我刪除規則嗎？

不應把它理解成一鍵刪除工具。依目前官方文件， `/doctor` 主要用於診斷 Claude Code 的安裝與設定，也能顯示 Skill description 預算與截短情況。真正的精簡仍需要搭配 `/context` 、真實任務與評估結果逐步進行。

### 規則越少，Claude 的結果一定越好嗎？

不一定。有效的目標、限制、專案慣例與安全邊界仍要保留。重點是刪掉不必要或衝突的內容，並把其餘資訊放到正確層級，而不是追求最短的 Prompt。

## 結論：不是少寫 Prompt，而是把 Context 放對位置

Claude 5 帶來的最大變化，不是模型從此不需要規則，而是團隊不能再把每次失誤都用一條新規則處理。

我的建議很直接：先讓 `CLAUDE.md` 回到專案地圖與共同慣例；把多步驟流程移進 Skills；把完整規格、測試與 mockup 留在 references；最後用 hooks、permissions 與 sandbox 守住不可違反的界線。

如果只能先做一件事，就打開最常使用專案的 `/context` ，找出重複出現、彼此衝突或每次都載入卻很少使用的內容。先整理一個真實工作流程，再用相同任務比較前後結果，比一次改造整套 Agent 架構更快，也更能確認哪些 context 真的有價值。

延伸閱讀： [Claude 2026 完整學習指南：從入門到進階功能一次掌握](https://www.aiposthub.com/claude-tutorial-2026/)