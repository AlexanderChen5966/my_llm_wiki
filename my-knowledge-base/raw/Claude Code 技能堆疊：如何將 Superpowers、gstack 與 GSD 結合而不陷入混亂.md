---
title: "Claude Code 技能堆疊：如何將 Superpowers、gstack 與 GSD 結合而不陷入混亂"
source: "https://codelove.tw/@tony/post/37l8Za"
author:
published:
created: 2026-05-25
description: "> 本文旨在比較各種框架，找出它們的重疊之處，並最終確定一個穩定的三層實踐。介紹--Claude Code 已迅速成為應用最廣泛的 AI 編碼工具之一。無論是個人開發者、新創公司還是大型工程團隊，都已將其整合到日常工作流程中——編寫生產程式碼、審查 pull request、除錯以及發布..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月22日](https://codelove.tw/@tony/post/37l8Za "2026-05-22 10:02:38")

## Claude Code 技能堆疊：如何將 Superpowers、gstack 與 GSD 結合而不陷入混亂

> 本文旨在比較各種框架，找出它們的重疊之處，並最終確定一個穩定的三層實踐。

## 介紹

Claude Code 已迅速成為應用最廣泛的 AI 編碼工具之一。無論是個人開發者、新創公司還是大型工程團隊，都已將其整合到日常工作流程中——編寫生產程式碼、審查 pull request、除錯以及發布功能，其速度之快在一年前是難以想像的。隨著使用規模的擴大，其生態系統也隨之發展壯大。 Claude **Skills——** 可組合的、自動呼叫的指令集，用於控制智能體的規劃、建構和驗證方式——已成為 Claude Code 最重要的擴展點之一。它們讓您能夠超越一次性提示，將 **可重複的工作流程** 直接編碼到智能體的行為中。事實上，Anthropic 公司已加倍投入這一方向：最新版本的 Claude Code **將先前獨立的「斜杠命令」和「技能」系統整合為一個統一的技能格式** ，這表明技能現在已成為擴展智能體的標準方式。

技能如今已成為使用者體驗的核心，社群紛紛支持一些開源框架，這些框架將最佳實踐打包成現成的技能集。其中討論最多的兩個框架是 **Superpowers** 和 **gstack** 。安裝這兩個框架聽起來很簡單；但實際上它們可能會 **衝突** ，而且在沒有規劃的情況下堆砌框架往往會 **降低** 系統的穩定性，而不是提高穩定性。那麼它們之間究竟有何差別呢？又該如何選擇呢？

這篇文章包含了三個內容：

1. 從程式碼庫、功能和理念方面 **比較** Superpowers 和 gstack——以下材料涉及明星、技能清單和權衡取捨。
2. **加入許多指南忽略的第三層** ： **GSD** 作為 **上下文/規範** 穩定器，以防止長期工作偏離（ *受 Tricia Notes Editorial 的三層框架啟發* ）。
3. **最後制定一個統一的行動方案** ：誰負責 **決策** 、 **背景** 和 **執行** ，以及如何在不增加代幣使用或認知負荷的情況下挑選合適的技能。

真正有用的問題不僅是“超能力 **還是** gstack？”，而是： *你缺少的是什麼——決策能力、持久上下文還是執行力？*

**一句話概括：** *gstack 思考，GSD 穩定，Superpowers 執行。*

---

## 方向：三層，而非兩層

實務上保持穩定的往往 **不是選擇某個框架而不是** 另一個框架，而是 **三方分工** 。

| 圖層 | 堆疊 | 角色 |

| -------------------- | --------------- | -------------------------------------------------------------------------------- |

| **決策/角色** | **gstack** | 從CEO、設計、架構、QA等角度進行判斷－不僅僅是「如何寫程式」。 |

| **上下文/規範** | **GSD** | 防止規範、狀態、邊界和長期上下文資訊過時。 |

| **執行** | **超能力** | 需求澄清 → 計畫 → 測試驅動開發 → 驗收，形成一個 **閉環** 。 |

**各自的「優勢」體現在哪裡：**

1. **超能力** －工作完成 **方式** ；流暢的執行循環。
2. **gstack** — 做 **什麼** 以及 **是否** 應該做；更豐富的角色為基礎的判斷。
3. **GSD——不漂移** ；在長鏈上保持更穩定的規格和上下文。

Superpowers 和 gstack 都已風靡一時。表面上看，它們為人工智慧增添了流程；實際上，它們能幫助你 **清晰地思考真正重要的事情** 。當模型快速編碼時，正是你需要清晰的需求和穩定的脈絡的時候—— **而這** 正是大多數人仍然忽略的。

---

## Superpowers 比較 gstack：簡單說明

### Superpowers（GitHub 約 13.7 萬顆星）

- 儲存庫： **obra/superpowers**
- **代理技能** 架構和軟體開發方法：涵蓋腦力激盪、計畫、TDD、執行和驗證 **的 14 項內建技能** 。

### gstack（GitHub 上約有 65K 個 star）

- 倉庫： **garrytan/gstack**
- 來自 **YC CEO Garry Tan** ，開源。
- 理念： **團隊** 伴你左右——CEO、設計師、工程經理、發布經理、文件工程師、品質保證人員等等 **——23 種有主見的工具** （產品思維、CEO 評審、架構評審、真實瀏覽器測試、設計評審、安全審計等）。
- Garry 聲稱 **在 60 天內編寫了 60 萬多行生產程式碼（其中 35% 為測試程式碼）** ，而且他是在全職運營 YC 的同時兼職完成的。

星級評價只是一個不太可靠的指標：高星級並不代表每項技能都適合你的工作流程。

---

## 功能對比（Superpowers vs gstack）

| 類別 | 超能力 | gstack |

|----------------------------- |-------------------------------------------------------------------------------- |------------------------------------------------------------------------ |

| **產品創意構思** | 創意構思 | /辦公時間，/CEO計畫審核 |

| **建築規劃** | 規劃編寫 | /plan-eng-review, /autoplan |

| **設計** | — | /設計諮詢，/計畫設計審核，/設計速成，/設計HTML |

| **開發執行** | 執行計劃、子代理驅動開發、平行代理調度 | — |

| **測試** | 測試驅動開發 | /qa，/qa-only |

| **除錯** | 系統除錯 | /調查 |

| **程式碼審查** | 請求程式碼審查，接收程式碼審查 | /review，/codex |

|驗證 **與驗收| 完成前驗證，完成開發分會 | /ship，/land-and-deploy，/canary** ，/document-release |

| **安全** | — | /cso、/careful、/freeze、/guard、/unfreeze |

| **可觀測性** | — | /learn, /retro |

| **瀏覽器測試** | — | /browse、/connect-chrome、/setup-browser-cookies |

| **Git 工作樹** | 使用 Git 工作樹 | — |

| **技能管理** | 使用超能力、寫作技能 | /gstack-upgrade |

| **效能** | — | /基準測試 |

| **部署** | — | /setup-deploy |

覆蓋範圍差異很大； **數量不是重點－設計概念才是。**

---

## 設計理念：「如何做」與「做什麼」（以及GSD的定位）

### Superpowers－專注於程式碼的建構方式

工作流程的核心在於 **高品質產出** ：明確目標、制定計畫、測試驅動開發（ **TDD** ，即先測試後實現）、驗證。每個步驟都設有檢查點，幾乎沒有跳槽的餘地。實務中，這種流程 **嚴謹有序** ：你提出需求，系統就會產生相應的結果。對於那些已經明確目標的工程師來說，這個流程往往能讓他們更有掌控感。

*（實際使用中的執行層面細節：流程嚴謹，執行穩定；即使是微小的任務，也會感覺 **很吃力，** 因為整個流程的節奏即使是很小的要求也適用。）*

### gstack－專注於做什麼和不做什麼。

在進行大量編碼之前，需要先確定諸如 **辦公時間** 步行需求之類的流程； **CEO** 和 **工程師的** 評審會對該方法進行壓力測試。這不僅僅是程式碼測試——它還可以從用戶角度 **執行真實的瀏覽器測試** 。大致劃分如下：

- **決策層：** `/office-hours` 、 `/plan-ceo-review` 、 `/plan-eng-review`
- **執行層：** `/review` 、 `/qa` 、 `/ship` 等。

當需求尚不清楚時，gstack 的優勢就顯現出來了——無論是產品經理、獨立開發者，還是「邊思考邊開發」的開發者。但要注意的是：啟用 **所有** 角色可能會顯得 **臃腫** ；決策技能也會消耗大量代幣（詳見下文）。

### GSD——上下文/規範，而不是另一個“團隊圖表”

**GSD** 不是「再組一個團隊」。它是 **情境工程** ：目標、規範、狀態、邊界和摘要都經過精心設計，減緩 **情境的腐敗** 。簡短的演示掩蓋了這一點；而 **長期** 專案則揭示了這一點——當情境搖擺不定時，輸出結果也會隨之分散；這才是 **狀態問題** ，而不僅僅是「執行不力」。

- **gstack** 本身可以思考，但它本身並不是一個長期的上下文庫。
- **Superpowers** 本身可以執行，但它本身並不是一個規範/上下文系統。
- **GSD** 填補了這一空白，使鏈保持連貫性。

---

## 三方比較（關注問題，而非「誰勝誰負」）

| 維度 | 超能力 | gstack | GSD |

|----------------- |----------------------------------------------------------------- |---------------------------------------------------------------- |---------------------------------------------------------------- |

| **核心問題** | 如何完成任務 | 應該做什麼；是否應該做 | 如何防止專案偏離軌道 |

| **層** | 執行 | 決策/角色 | 情境/規範 |

| **最佳匹配** | 規劃、測試驅動開發、驗收循環 | 多視角判斷、審查、品質保證 | 情境工程；穩定狀態 |

| **最適合** | 需求明確 | 邊思考邊建構 | 長鏈/多次迭代 |

| **常見問題** | 前期投入較大，流程繁瑣（詳情見下文） | 完全啟用後，功能臃腫且消耗大量令牌（詳情見下文） | 本身獨立「交付」價值不高（詳情見下文） |

| **角色** | 自主 **執行** | 自主 **決策** | 自主考慮 **長遠發展方向** |

### 常見痛點詳解

**強大的功能－前期投入過多的流程可能會讓人感覺很繁瑣。** 每個任務，無論大小，都要經歷完整的流程：明確需求、制定計畫、先寫測試、然後實現、最後驗證。對於大型功能來說，這種節奏確實能帶來豐厚的回報。但即使是兩行配置修改或快速的文字更改，同樣的流程也會啟動，最終導致你花在流程上的時間比實際修改的時間還要多。這種開銷並不會隨著任務規模的增加而減少，因此即使是小的請求也會感覺異常緩慢。

**gstack－完全啟用後會變得臃腫且極度消耗代幣。** 每個 gstack 角色（CEO、設計師、架構師、QA 等）都會在上下文中註入其獨特的視角和提示。如果全部啟用，即使只是執行層的技能，在編寫任何實際程式碼之前，也可能消耗 **超過 1 萬個代幣** 。日常使用會迅速消耗代幣，多個「虛擬團隊成員」之間的來回溝通甚至會讓簡單的任務都顯得緩慢而重複。此外，在程式碼庫掃描過程中，你可能還會遇到一些無關的元問題（例如「你是否正在申請成為 YC 孵化的公司？」）——這些都是框架主觀角色層帶來的產物。

**GSD－獨立使用時「交付」價值不高。 GSD** 的優點在於能夠長時間維持規範、目標和狀態的穩定。但如果 **單獨** 使用，它不會直接產生程式碼、執行測試或提交 PR。它是一個穩定器，而非建構器。如果沒有執行層（Superpowers）或決策層（gstack）的配合，GSD 管理的上下文就沒有任何實際作用——只是一些有用的基礎架構，沒有可見的輸出。只有當它與真正用於交付的工具結合使用時，其價值才能顯現出來。

**實際意義：** 它們是 **互補的** ，而不是替代的——Superpowers 執行，gstack 決定，GSD 隨著時間的推移穩定規範和上下文。

---

## 優勢、劣勢和摩擦

### 超強國

- **優點：** 腦力激盪和整體工作流程感覺很紮實；即使是小的需求，一旦習慣了，整個流程也能變得順暢；執行力和測試驅動開發能力很強。
- **缺點：** 與 gstack 的決策層相比，Superpowers 的 **早期** 決策能力（例如計劃/腦力激盪）往往較弱——因此許多人將 gstack 的前端與 Superpowers 的執行相 **結合** 。

### gstack

- **優點：決策層** — `/office-hours` 、 `/plan-ceo-review` 、 `/plan-eng-review` 因其定位和方法審查而脫穎而出。
- **缺點：** 與超級能力相比，執行起來感覺更粗糙；代幣成本是實實在在的 **——一個執行層技能可能需要 10000 多個代幣** ，而且大量的掃描感覺像是嘈雜的“過程”，而不是幫助。

### 類比

> **超能力就像一把手術刀** ——精準而有效率。
> 
> **gstack是一家提供全方位服務的診所** —從診斷到術後護理。

用比喻來選擇深度：狹義的執行與全方位的產品和審查。

---

## 綜合最佳實踐

### 1\. 有意識地選擇技能－不要什麼都安裝。

技能數很容易螺旋式成長（今天學Superpower，明天學gstack，下週可能又學另一個堆疊）。 **有選擇地學習** 比盲目追求數量更重要；隨意呼叫既不穩定，又會徒增表面上的“技能數量”，而缺乏清晰度。

其核心概念：這兩個技術堆疊都是 **「優勢互補工程」** 的實驗。其想法是 **發揮優勢，彌補劣勢** ，而不是「我想要全部」。

### 2\. 決策與執行（經典的劃分）－然後根據需要加入背景訊息

**決策層的 gstack（精選）：**

- 優先考慮高價值流程：例如 `/office-hours` 、 `/plan-ceo-review` 、 `/plan-eng-review` ，用於需求和協調——避免在每個角色上投入過多資源。

**執行層的超能力：**

- 優先選擇 Superpowers 作為 TDD、計劃執行和驗證的 **基礎** ——如果 gstack 已經涵蓋了該階段，則可以選擇性地 **弱化** 其自身的繁重決策技能，這樣小任務就不會繼承雙重進程。

**當鏈發散時，GSD：**

- 如果工作 **跨越** 會話和線程，則加入 **GSD** 以使規範和狀態保持錨定——不是為了閃退， **而是為了防止漂移** 。

### 3\. 穩定的工作流程（三步驟）

1. **決策 → gstack** — 首先執行 `/office-hours` 來對想法進行壓力測試，然後執行 `/plan-ceo-review` 進行創始人級別的健全性檢查，最後執行 `/plan-eng-review` 來鎖定架構和資料流。如果設計很重要，則加入 `/plan-design-review` 。目標：在編寫程式碼之前，決定要建置 **什麼** 以及 **是否** 要建置。
2. **上下文 → GSD** — 一旦做出決策，就使用 GSD (v2) 來指導計劃： `PROJECT.md` 用於描述專案內容， `DECISIONS.md` 用於架構選擇， `KNOWLEDGE.md` 用於跨會話規則和模式，里程碑路線圖 ( `M001-ROADMAP.md` ) 用於分階段執行。這些 v2 文件可以保持規範、狀態和邊界的穩定，從而避免上下文在不同會話之間失效。 （原始 GSD 使用的是 `REQUIREMENTS.md` 、 `ROADMAP.md` 和 `STATE.md` 。）
3. **執行 → Superpowers** — 在明確需求和穩定上下文的基礎上，將專案移交給 Superpowers 的執行循環： `brainstorming` （如果仍需進行輕量級細化）、 `writing-plans` → `executing-plans` 以實現、 `test-driven-development` 用於 RED-GREEN-REFACTOR 循環）、 `requesting-code-review` / `receiving-code-review` 以進行審查，以及 `verification-before-completion` → `finishing-a-development-branch` 前驗證→關閉。對於並行工作，請使用 `dispatching-parallel-agents` 或 `subagent-driven-development` 。

**合併後的標語：** *gstack 負責思考，Superpowers 負責執行，GSD 確保長篇上下文的準確性。* 將 gstack **強大的決策能力** 與 **Superpowers 的執行力** （必要時輔以 GSD）相結合，可以有效控制技能需求和衝突——這與作者利用周末時間，透過 **精心挑選的** 插件組合開發小型工具的經驗類似。

### 4\. 最終啟發式方法

1. 需求仍不清楚 → **從 gstack 開始** （決策）。
2. 工作在鏈條上不斷分歧 → **新增 GSD** （上下文）。
3. 你想要 **穩定且閉環的** 執行 → **依賴超級力量** （執行）。

**不要只問：** 「超能力還是 gstack？」\*\*而應該問：\*\* *我是否缺少決策、背景資訊或執行步驟？*

## 結束語：

技能的提升並非取決於你學習的內容更多，而是取決於你 **將正確的部分組合起來，彌補你實際存在的差距** ，並理解每一層的作用，然後建立出屬於你自己的工作流程。

---

## 參考

- **超能力** — [github.com/obra/superpowers](https://github.com/obra/superpowers)
- **gstack** — [github.com/garrytan/gstack](https://github.com/garrytan/gstack)
- **GSD（Get Shit Done）** — [github.com/gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done) （原始版本）| [github.com/gsd-build/gsd-2（v2](https://github.com/gsd-build/gsd-2) ，獨立命令列版本）

---

原文出處： [https://dev.to/imaginex/a-claude-code-skills-stack-how-to-combine-superpowers-gstack-and-gsd-without-the-chaos-44b3](https://dev.to/imaginex/a-claude-code-skills-stack-how-to-combine-superpowers-gstack-and-gsd-without-the-chaos-44b3)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/37l8Za) [131瀏覽](https://codelove.tw/@tony/post/37l8Za)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/37l8Za)

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

🔥 [50 多個必備 Telegram 機器人的終極清單](https://codelove.tw/@tony/post/37oZV3)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝18 💬11 ❤️1

550

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

82

🥉

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

#4

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779686630&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37l8Za&amp;pra=5&amp;asro=0&amp;itsi=7&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779686630919&amp;bpp=1&amp;bdt=74&amp;idt=21&amp;shv=r20260520&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779686067%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779686067%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779686067%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2791181835319&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=8463&amp;eid=31098655%2C42532523%2C95391807%2C95391232&amp;oid=2&amp;pvsid=4194878687075139&amp;tmod=1302172221&amp;uas=0&amp;nvt=3&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37l8Za&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=25" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260520/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>