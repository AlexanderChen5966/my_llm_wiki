---
title: "全網爆紅的 Loop 到底是什麼？"
source: "https://codelove.tw/@tony/post/qenW4x"
author:
published:
created: 2026-06-26
description: "全網爆火的 Loop 到底是什麼？  ==================    前言--這兩天 AI 圈有個詞特別火，叫做**Loop**。起因是 OpenClaw 創辦人 Peter Steinberger 發了條推文，說「你不應該再給程式設計 Agent 寫提示詞了。你應該設計..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月25日](https://codelove.tw/@tony/post/qenW4x "2026-06-25 15:05:27")

## 全網爆火的 Loop 到底是什麼？

```
前言
```

\--

這兩天 AI 圈有個詞特別火，叫做 **Loop** 。

起因是 OpenClaw 創辦人 Peter Steinberger 發了條推文，說「你不應該再給程式設計 Agent 寫提示詞了。你應該設計循環來提示你的 Agent。」

這條推文直接引爆了全網，瀏覽量已經超過 800 萬。

很多人可能第一次接觸這個概念的時候，心裡會冒出一連串的疑問：

Loop 不就是程式設計裡的迴圈嗎？為啥突然就火了？

Loop、Prompt、Context、Harness 這些詞到底是什麼關係？

今天就專門跟大家一起聊聊這個話題，希望對你會有所幫助。

更多專案實戰在我的技術網站： [susan.net.cn/project](http://susan.net.cn/project)

## 一、Agent Loop 到底是什麼？

Loop 這個詞直譯過來就是循環。

在傳統程式設計裡，循環做的事情很明確：你寫一個 for 迴圈遍歷陣列，機器就會老老實實從第一個元素走到最後一個元素。

傳統迴圈的本質，是讓機器 **重複執行明確的指令序列** 。

但在 AI Agent 的語境裡，Loop 的含義完全不同。

Agent 裡的 Loop 不是執行「指令」，而是執行 **目標** 。

透過反覆循環，把輸出的結果不斷接近目標。當結果符合目標時，循環才終止。

**傳統迴圈 vs Agent Loop：**

| 對比維度 | 傳統迴圈 | Agent Loop |
| --- | --- | --- |
| 核心任務 | 執行明確指令 | 追求模糊目標 |
| 執行方式 | 固定邏輯，重複相同操作 | 自適應調整，每一步都可能不同 |
| 處理能力 | 只能處理預設分支 | 能應對未知情況，自主摸索 |
| 失敗處理 | 按預設邏輯報錯 | 觀察結果，調整策略，重新嘗試 |
| 適用場景 | 確定性任務 | 開放性、不確定性任務 |

舉個例子：你給一個 Agent 寫程式、修 bug 的任務。

它不是按照一條固定的路子走，而是會觀察當前狀態，判斷應該採取什麼行動，執行後觀察結果，評估是否達到了預期，然後決定下一步怎麼走。

這個過程的每一步都不是固定的。

Agent 可能需要嘗試多種方案，走錯路會自己修正，從失敗中學習，在多輪迭代中逐漸逼近正確答案。

這其實就是大家常說的 `ReAct` 範式（Reasoning + Acting）：交替進行推理和行動，直到任務完成。

具體流程是這樣的：

- **推理（Reason）** ：LLM 分析當前狀態，決定下一步做什麼
- **行動（Act）** ：執行選定的操作（呼叫工具、生成程式碼、查詢資料等）
- **觀察（Observe）** ：取得行動結果作為新的狀態
- **循環（Loop）** ：重複直到滿足停止條件

你會發現，每次 CLI 指令回傳非零狀態碼時，Agent 會自動讀取錯誤，嘗試修復，再執行測試——這本身就是 Agent Loop 在起作用。

它已經深入到了我們日常開發流程的骨髓裡，只是你沒注意到而已。

## 二、Agent Loop 的完整運行流程

### 2.1 一條訊息的完整旅程路線

一個訊息進入 Agent 系統後，大概會經歷這樣的旅程：

**接收與解析** → **上下文組裝** → **模型推理** → **工具執行** → **串流回覆** → **狀態持久化**

這是智能體完整的真實運行過程，是整個系統的權威執行路徑。

從狀態機角度看，Agent Loop 本質上是一個 **事件驅動的有限狀態機** 。

它在以下狀態之間來回切換：

[![image.png](https://i.imgur.com/vbQUJ1g.jpeg)](https://i.imgur.com/vbQUJ1g.jpeg)

### 2.2 兩種不同的 Loop

需要特別說明一點，AI 圈裡關於 Loop 的討論，其實包含了兩個層面：

**單 Agent Loop** ：一個獨立的 Agent，不斷在自己的 ReAct 循環裡迭代，直到達成目標。這是單個智能體內部的自我完善循環。

**多 Agent 協作** ：多個不同角色的 Agent 在同一個工作空間裡協同工作，各自推進自己的任務，共享上下文，互相呼叫。這是多個智能體之間的團隊協作循環。

兩者都被稱為「Loop」，但關注點完全不同。

單 Agent Loop 關心的是「一個 Agent 怎麼把事情做對」，多 Agent 協作關心的是「多個 Agent 怎麼一起把事情做完」。

### 2.3 多 Agent 協作的核心

Loop 不只是讓一個 Agent 反覆嘗試，它還提供了多 Agent 協作的能力。

多個專業化的 Agent 可以同時工作，各司其職，並行推進，互不阻塞。

具體來說，這個協作體系包括三個核心組件：

- **頻道（Channel）** ：按專案或團隊劃分工作空間。每個頻道有獨立的任務板、上下文和成員列表，不同頻道的資訊完全隔離
- **執行緒（Thread）** ：在頻道內針對某條訊息發起子討論。保持主頻道清爽，同時讓相關討論集中在一起
- **任務板（Task Board）** ：每個頻道都有獨立的任務看板，支援 todo → in\_progress → in\_review → done 全流程追蹤。Agent 可以自動認領、執行和提交任務

## 三、從 Prompt 到 Loop：AI 工程化的四個層級

AI Agent 的工作方式一直在進化。

我們用過一個很形象的框架來理解這個過程：Prompt、Context、Loop、Harness，這四個詞代表了 AI 工程化從入門到精通的四個關鍵階段。

[![image.png](https://i.imgur.com/dZnNXDS.jpeg)](https://i.imgur.com/dZnNXDS.jpeg)[![image.png](https://i.imgur.com/oUa7aqw.jpeg)](https://i.imgur.com/oUa7aqw.jpeg)### 3.1 Prompt Engineering：怎麼問

最早大家關心的是「提示詞怎麼寫」——讓 AI 扮演什麼角色、補充什麼範例、用什麼格式輸出。這是最基礎的，目標是讓 AI 準確理解需求。

但這個階段的侷限性也很明顯：適合相對明確的任務（寫文案、摘要文章、擷取要點），一旦任務變複雜，Prompt 就不夠用了。

### 3.2 Context Engineering：給它看什麼

當任務複雜之後，AI 需要知道的更多。它要看的是專案背景、程式碼結構、目標指令、歷史決策、團隊規範……把哪些資訊放進模型上下文裡，是這個階段要解決的問題。

資訊給少了，AI 缺少判斷依據；資訊給錯了，它可能在錯誤的方向上越走越遠；資訊給多了，它又抓不住重點。

這裡面的平衡很講究。

### 3.3 Loop Engineering：怎麼讓它持續往前

Context Engineering 讓 AI「做對」，Loop Engineering 解決的是「做完成」。Agent 處理長任務時，需要反覆執行、自行檢查成果、失敗後修改、試錯並累積經驗。

Loop Engineering 要回答的問題是：

- 如何持續推進任務？
- 執行結果怎麼檢查？
- 任務失敗後怎麼修正？
- 修正後的經驗能不能沉澱？
- 什麼時候應該停下來，把決策權交給人？

這些問題，單純靠寫好 Prompt、給足 Context 是不夠的，需要在更上層設計一套 **循環系統** 來管理。

### 3.4 Harness Engineering：在哪裡安全運行

Prompt、Context、Loop 這三層解決了「怎麼完成任務」的問題，但還有一個更底層的問題需要考慮：這些任務在哪裡運行？

Harness 指的是 Agent 的運行環境：提供工具呼叫能力、權限控制、隔離沙箱、可觀測日誌，以及人工接管的機制。一個 Agent 再聰明，如果沒有合適的運行環境，也沒法真正落地。Harness 是讓 Agent 能安全可靠地在生產環境中工作的基礎設施。

## 四、Loop Engineering：六塊積木搭起來的工程體系

Google 工程師 Addy Osmani 最近把 Loop Engineering 拆解成了六塊核心積木：

### 第一塊：自動化機制（Automations）

Loop 的心跳。按照設定的時間或條件自動觸發，負責發現問題、發起任務。你不用手動去問 AI 該幹活了，系統會自動接管。

### 第二塊：工作區隔離（Worktrees）

多個 Agent 並行運行時，不能讓它們互相踩到對方的程式碼。Worktrees 就是用來隔離工作空間的機制，每個 Agent 有自己的獨立目錄，互不干擾。

### 第三塊：技能（Skills）

把團隊裡的知識、開發規範、最佳實踐，寫成可重複使用的文件或腳本。Agent 可以直接呼叫這些 Skill，不用每次都從頭解釋。「你終於不用每次都重講你的專案了」——這是 Skills 的核心價值。

### 第四塊：外掛與連接器（Connectors）

讓 Agent 能連接外部系統——GitHub、Slack、資料庫、Jira、飛書文件等。沒有這層連接能力，Agent 就是「睜眼瞎」，拿不到真實資料，也幹不了實事。

### 第五塊：子代理（Sub-agents）

把不同角色拆給不同的 Agent：一個負責探索方案、一個負責實作、一個專門驗證結果。原因很簡單：寫程式的 Agent 給自己的作業打分時太仁慈了，需要一個獨立的 Agent 來做交叉驗證。

### 第六塊：外部記憶（Memory）

一份保存在單次對話之外的狀態記錄——可以是 Markdown 檔案，也可以是 Linear 看板。模型在每次執行之間會忘光一切，所以記憶必須存在磁碟上，用來記錄「做了什麼、下一步是什麼」。這是每一個長時間運行的 Agent 都依賴的核心設計。

## 五、產品化案例：PingCAP Loop

2025 年以來，PingCAP 推出的 Loop 平台引起了廣泛關注。

它不是又一個更強的聊天機器人，而是一個完整的工作空間——讓人類和多個 AI Agent 在同一環境中高效協同。

這是一個非常直白的產品定義： **「從 1 對 1 聊天，到 1 對 N 團隊協作」** 。

### 5.1 核心能力一：多 Agent 並行協作

多個專業化的 Agent 可以同時工作，各司其職，並行推進，互不阻塞。

架構師 Agent 出方案 → 開發工程師 Agent 寫程式 → 測試工程師 Agent 驗證 → 領域專家 Agent 生成文件。

全程使用者只需審核和回饋決策點，不用再在每個環節都事必躬親。

### 5.2 核心能力二：持久化記憶

Agent 不是一次性的問答機器。

每個 Agent 都有自己的：

- **Soul（身分文件）** ：定義角色、預設行為和技能配置，跨會話保持一致
- **Memory** ：透過 [MEMORY.md](http://memory.md/) 記錄歷史工作、使用者偏好、頻道角色，Agent 重新啟動後能自動恢復上下文
- **Skills** ：可載入不同的技能模組，讓 Agent 專注自己擅長的事
- **獨立工作空間** ：每個 Agent 有自己的檔案目錄，可以存筆記、寫程式、管理狀態

這意味著什麼？

Agent 會用得越來越懂你的專案、團隊規範和業務邏輯。

### 5.3 核心能力三：任務驅動的工作流

每個頻道都有一個任務板，掛著所有待辦任務。

和一般看板最大的不同是——Agent 可以認領任務。

完整流程是這樣的：

- 你在頻道建立任務：「分析使用者行為資料，產生週報」
- Agent 自動認領任務
- Agent 查知識庫、跑 SQL、分析結果，然後提交報告
- 報告進入審核狀態，等你確認
- 你確認後任務標記完成

整個過程你只做了兩件事：提出需求和驗收結果。

中間的調研、分析、落地、複盤，全部由 Agent 接力完成。

## 六、一個完整的 Loop 長什麼樣

把上面這些概念串在一起，一個完整的 Loop 大概長這樣：

[![image.png](https://i.imgur.com/25yiJbD.jpeg)](https://i.imgur.com/25yiJbD.jpeg)

從這個圖可以看出來，一個 Loop 不僅僅是「循環執行」，它包含了目標拆解、多 Agent 分工、結果驗證、外部記憶、工具呼叫，以及明確的終止條件。

每一個環節都可以獨立設計、獨立優化。

## 七、優點和侷限

### 7.1 Loop 的優勢

**第一，處理開放性任務** ：不需要把所有情況都寫死，Agent 可以在 Loop 裡自己摸索。對於寫程式、修 bug、做研究這類沒有唯一正確答案的任務，效果尤其明顯。

**第二，持續優化和累積** ：傳統對話式 AI 是一次性的——你說一句，它答一句，下次全忘。Loop 可以透過外部記憶（ [MEMORY.md](http://memory.md/) 等）持續沉澱經驗，越用越聰明。

**第三，並行效率翻倍** ：一個 Loop 裡可以跑多個 Agent 同時工作，分工明確，互相補位。這種「團隊作戰」的效率遠超單打獨鬥。

**第四，可稽核可干預** ：每一步操作、每一個決策都有記錄。需要人工介入的關鍵節點可以暫停等待確認，避免 AI「自作主張」。

### 7.2 Loop 的問題

**第一，Token 消耗是個硬傷** ：Loop 靠的就是不斷迭代逼近目標。如果設計不好，Agent 可能在一個循環裡來回繞幾十圈，Token 嘩啦啦地燒。沒有清晰的終止條件，甚至可能跑出無限循環——框架層面已經有保護機制（最大迭代次數、超時中斷），但設計 Loop 時還是要謹慎。

**第二，回饋機制是命門** ：設計 Loop 只完成了一半，另一半是往 Loop 裡放入能夠說「不」的機制——測試、型別檢查或真實錯誤資訊。一個沒有回饋機制的 Loop，就像一個沒有裁判的比賽，結果可想而知。

**第三，概念疊概念，容易消化不良** ：從 Prompt Engineering 到 Context Engineering 到 Harness Engineering 再到 Loop Engineering，概念堆疊得太快。大家還沒來得及消化前面的，新的又來了。這可能是 AI 技術迭代太快帶來的副作用。

**第四，成熟度問題** ：Loop 工程還是個很新的東西。很多工具、框架還在不斷演進中。現在用它，需要有一定的容忍度和探索心態。

## 八、適用場景

### 8.1 強烈推薦用的場景

**自動化程式開發與維護** ：讓 Agent 完成從設計→寫程式→測試→審查→迭代的全流程。你自己只需要定義目標和審核關鍵決策。Loop 會驅動 Agent 反覆迭代，直到任務完成。

**持續整合／持續部署（CI/CD）優化** ：當測試失敗時，Agent 自動分析錯誤、修復程式碼、重新執行測試。一個 Loop 可以完成「程式碼品質閉環」。

**智能體團隊協作開發** ：多個 Agent 分工協作，架構師、開發者、測試者同時推進，並行效率遠超單個智能體。

**資料分析和報告生成** ：Agent 定期查詢資料庫、分析資料、生成報告。Loop 可以持續迭代分析結果，確保資料的準確性和時效性。

### 8.2 不太適合的場景

**一次性的簡單對話** ：如果只需要回答問題或生成簡短內容，用 Loop 屬於大砲打蚊子。

**成本敏感的專案** ：Loop 需要不斷迭代，Token 成本比單次呼叫高不少。如果專案預算緊張，用 Loop 需要仔細評估投入產出。

**任務目標非常模糊的場景** ：Loop 需要清晰的目標和驗證標準。如果目標本身都定義不清楚，Loop 也不知道該什麼時候停下來。

更多專案實戰在我的技術網站： [susan.net.cn/project](http://susan.net.cn/project)

## 九、總結

寫到這裡，回頭梳理一遍，Loop 之所以能在 2026 年成為 AI 圈的熱門話題，是因為它恰好回答了 AI 落地中的一個核心問題：從「AI 能聽懂」到「AI 能做完」，中間需要什麼？

| 階段 | 核心問題 | 解決的是什麼 |
| --- | --- | --- |
| Prompt Engineering | 怎麼問？ | 讓 AI 聽懂 |
| Context Engineering | 給它看什麼？ | 讓 AI 做對 |
| Loop Engineering | 怎麼讓它持續往前？ | 讓 AI 做完成 |
| Harness Engineering | 在哪裡安全運行？ | 讓 AI 安全交付 |

四個工程化的關鍵詞，正好對應了 AI 從「答題器」進化到「任務執行者」的完整路徑。

Loop 關心的是第三個問題：怎麼讓 AI 持續往前。

一個 Agent 的完整工作流程，就是一次次的循環：接收輸入 → 理解需求 → 呼叫工具 → 觀察結果 → 修正方案 → 繼續推進。

直到目標達成，或者需要人類介入。

每一次循環，都是 AI「智慧」與「行動」的結合。

Loop 是一個工程化的選擇，不是炫耀的概念。

用得好，Token 花得值；用不好，就是在燒錢。

設計 Loop 的關鍵在於：給 Agent 清晰的回饋、明確的邊界、可追溯的狀態，以及合理的停下來的理由。

其實 Loop 這個概念，歸根結底可以理解為：把你的思維過程設計成一個可重複、可累積、可稽核的自動化系統。

你不再是那個事必躬親的「提示工程師」，而是變成了定義規則、設計邊界的「系統架構師」。

從手動到自動，從單一到協作，這可能是 AI 工程化正在經歷的底層變革。

這大概就是 Loop 能在這幾天引發全網討論的原因。

---

原文出處： [https://juejin.cn/post/7654044236049367066](https://juejin.cn/post/7654044236049367066)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qenW4x) [9瀏覽](https://codelove.tw/@tony/post/qenW4x)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qenW4x)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝14 💬2 ❤️1

745

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

68

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782464798&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqenW4x&amp;pra=5&amp;asro=0&amp;itsi=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782464798164&amp;bpp=2&amp;bdt=94&amp;idt=36&amp;shv=r20260625&amp;mjsv=m202606180101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782464765%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782464765%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782464765%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=8461540012897&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1605&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099345&amp;oid=2&amp;pvsid=6436740607909586&amp;tmod=1647572126&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1930%2C30%2C1930%2C30%2C1920%2C30%2C1845%2C985%2C1605%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=46" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260625/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>