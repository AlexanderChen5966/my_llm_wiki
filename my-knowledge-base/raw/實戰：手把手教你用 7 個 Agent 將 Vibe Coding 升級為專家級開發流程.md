---
title: "實戰：手把手教你用 7 個 Agent 將 Vibe Coding 升級為專家級開發流程"
source: "https://www.blocktempo.com/claude-code-software-factory-7-agents/"
author:
  - "[[flip]]"
published: 2026-05-31
created: 2026-06-15
description: "作者 @sairahul1 拆解了從「Vibe Coding」升級到「軟體工廠」的工作流革命：把單一 AI 對話拆成 7 個專責代理人：研究員、故事撰寫者、規格撰寫者、後端建造者、前端建造者、測試驗證者、實作驗證員，每個只擁有單一職責、乾淨的上下文與嚴格邊界。（前情提要：串聯萬物的 MCP 加上 Web3，能成下一波 AI 百倍敘事嗎？）（背景補充：最強投資大師幫你打工！集結巴菲特、蒙格、Cathie Wood…19 個 AI Agent 幫你分析市場 ） 我以為我在用 AI 寫程式。實際上，我只是打字打得比較快而已。這篇要說的就是兩者的差異——以及徹底改變一切的「7 代"
tags:
  - "clippings"
status: processed
---
[![實戰：手把手教你用 7 個 Agent 將 Vibe Coding 升級為專家級開發流程](https://image.blocktempo.com/2026/05/ai-workflow-1140x815.png "ai workflow | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://image.blocktempo.com/2026/05/ai-workflow.png)

作者 [@sairahul1](https://x.com/sairahul1/status/2058832033628241931) 拆解了從「Vibe Coding」升級到「軟體工廠」的工作流革命：把單一 AI 對話拆成 7 個專責代理人：研究員、故事撰寫者、規格撰寫者、後端建造者、前端建造者、測試驗證者、實作驗證員，每個只擁有單一職責、乾淨的上下文與嚴格邊界。  
（前情提要： [串聯萬物的 MCP 加上 Web3，能成下一波 AI 百倍敘事嗎？](https://www.blocktempo.com/web3-plus-mcp-the-next-wave-of-ai-100x-narrative/) ）  
（背景補充： [最強投資大師幫你打工！集結巴菲特、蒙格、Cathie Wood…19 個 AI Agent 幫你分析市場](https://www.blocktempo.com/ai-hedge-fund-19-agents-buffett-burry-open-source-57k/) ）

我以為我在用 AI 寫程式。實際上，我只是打字打得比較快而已。

這篇要說的就是兩者的差異——以及徹底改變一切的「7 代理人系統」。

把這篇存下來。它會幫你省下好幾個月。

### 沒有人在談的那個問題

那個看起來很有產能、其實沒有的迴圈:

→ 要 Claude 幫你做一個功能 → 它生出程式碼 → 某個地方壞了 → 把錯誤訊息貼回去 → 它修補 → 又壞了另一個地方 → 再問一次

**第 1 天:這像魔法。**

**第 30 天:你花在監督 AI 的時間,比過去自己寫程式還多。**

同樣的邏輯出現在三個不同的地方。Claude 忘了你兩週前訂下的慣例。新功能弄壞舊功能。測試不是缺少就是寫得很淺。

你某天醒來才意識到:**不是 AI 在失敗,是你的工作流在失敗。**

![](https://pbs.twimg.com/media/HJJZuz-bUAAIciP?format=jpg&name=large)

問題的本質是結構性的。

當你在 Claude Code 裡輸入「幫我做這個功能」,你其實是在要求一個 AI 對話同時扮演:

→ 產品分析師 → 架構師 → 後端工程師 → 前端工程師 → 測試工程師 → 程式碼審查員

全部一次到位。在同一個一團亂的對話裡。

計畫裡錯的假設,會變成錯的資料庫模型。錯的資料庫模型,會變成錯的 API。錯的 API,會變成錯的 UI。

等你發現時,錯誤已經擴散到處都是。

這就是所謂的 **vibe coding** (憑感覺寫程式)。

**它有一道很硬的天花板。**

### 轉折:從 Vibe Coding 到軟體工廠

真正改變一切的關鍵:

真正的工程團隊,不會在一個大型對話裡工作。

不同的人擁有不同的工作:

→ 有人釐清使用者問題 → 有人思考架構 → 有人寫 API → 有人寫 UI → 有人思考邊界情況 → 有人做審查

當你把所有這些塌縮成一個 AI 對話,錯誤就會悄悄地累加。

**修正之道,是把工作拆給專門化的代理人。**

每一個代理人會得到:

→ 一個聚焦的工作 → 自己乾淨的上下文視窗 → 只擁有它真正需要的工具 → 對它「不可碰觸的範圍」有嚴格規則

結果:**一座軟體工廠。**

一個開發者 + 七個聚焦的代理人 = 一支協調的團隊。

以下就是讓這件事運作的七個代理人。

![](https://pbs.twimg.com/media/HJJZ1qDaYAANkx0?format=jpg&name=large)

### 七個代理人

#### 代理人 1:程式碼庫研究員(Codebase Researcher)

開發者用 AI 時最大的錯誤是什麼?

**把「要程式碼」當成第一步。**

AI 接受 prompt,做出猜測去填補空白,然後開始生成。糟糕的設計就是在這時偷偷溜進來。

研究員修正這件事。

它唯一的工作:**檢視程式碼庫並解釋現況——在一行程式被寫下來之前。**

**它做什麼:**

- 標出相關檔案與它們的角色
- 記錄既有需要遵循的模式
- 找出已經建過的類似功能
- 標示風險(時區、多租戶、重試邏輯)
- 列出哪些測試會需要更新

**它不能做什麼:**

- 編輯檔案(只有讀取許可權)
- 執行任何會改變狀態的指令
- 做假設——它應該改成發問

**工具:** Read、Grep、Glob,僅此而已。

**規則:** 每一次,在動工之前都要先探索。

**研究員永遠先跑。**

![](https://pbs.twimg.com/media/HJJakAcbsAAEelQ?format=jpg&name=large)

#### 代理人 2:故事撰寫者(Story Writer)

大部分功能失敗,並不是因為程式碼錯了。

**而是因為問題從來沒有被清楚定義。**

故事撰寫者把粗略的功能構想,變成一個真正的使用者故事——在任何技術決策被做出之前。

**輸入:**

- 你的粗略功能描述
- 研究員的調查結果

**產出:**

- **一個使用者故事**:「作為 \[角色\],我想要 \[行為\],這樣 \[結果\]。」
- **驗收標準**:可以被測試直接驗證的敘述。Happy path、失敗路徑、商業規則。
- **邊界情況**:邊界、重試、多租戶顧慮。
- **不在範圍內**:什麼是明確「不會做」的。
- **未解問題**:它真心不知道的東西——絕不亂猜。

**它不能做什麼:**

- 發明商業規則
- 寫任何程式碼或技術設計
- 在有東西真心不清楚時繼續推進

**工具:** Read,僅此而已。

**規則:** 你要讀過這份故事並核准,才會發生下一件事。

這是讓下游一切都不出錯的關鍵—— **人類審核點 1** 。

![](https://pbs.twimg.com/media/HJJasbubsAA7kJn?format=jpg&name=large)

#### 代理人 3:規格撰寫者(Spec Writer)

故事核准之後,規格撰寫者把它變成一份技術簡報。

這份簡報就是每個建造代理人會遵循的藍圖。

**輸入:**

- 你核准過的使用者故事
- 研究員的調查結果
- 你專案的 CLAUDE.md 規則

**產出:**

- 資料模型變動(欄位、型別、遷移)
- 背景流程 / 處理流程
- API 變動(端點、request/response 格式)
- 前端變動(元件、頁面、hooks)
- 需要的測試(成功、失敗、邊界)
- 風險與未解問題
- 每一個會被改動的檔案

**它不能做什麼:**

- 編輯任何檔案
- 發明新的基礎設施——而是要明確指出
- 跳過租戶隔離或時區考量
- 留下沒有答案的問題

**工具:** Read、Grep、Glob,僅此而已。

**規則:** 這份簡報是 **人類審核點 2** 。

你讀過、核准,才會有任何一個檔案被動。

如果你看見「把 ID 存在記憶體裡」——那就是紅旗。

**現在抓出來。不要等到 10 個檔案被改完。**

![](https://pbs.twimg.com/media/HJJa0pra4AAE8NU?format=jpg&name=large)

#### 代理人 4:後端建造者(Backend Builder)

現在才開始建造。

後端建造者實作功能的「後端那一半」——而且只負責後端那一半。

**輸入:**

- 核准的技術簡報
- 研究員的調查結果
- 你專案的 CLAUDE.md

**它建造:**

- API 路由
- Services 與商業邏輯
- 資料庫存取與遷移
- 背景任務
- 它所寫東西的單元測試

**它不能做什麼:**

- 碰任何 React 元件、頁面或 client-side hooks(那是代理人 5 的工作)
- 在沒有指示下發明新依賴
- 修改範圍外的檔案
- 沒跑 typecheck、lint 與測試套件就停下

完成後,它會回傳一份摘要:每個新增或編輯的檔案、每個被重用的既有 helper 或模式、任何「如果有 CLAUDE.md 規則會更好」的觀察。

**工具:** Read、Edit、Write、Bash——只限後端資料夾。

**重點就在分離本身。**

**後端建造者永遠不可能不小心弄壞前端。**

![](https://pbs.twimg.com/media/HJJa6daacAAhfq5?format=jpg&name=large)

#### 代理人 5:前端建造者(Frontend Builder)

前端建造者實作 UI 那一半——只負責 UI 那一半。

**它會先讀後端建造者的摘要。**

這很重要。

它會依照後端產出的樣子去使用 API。它不會發明新的端點。

如果 API 的形狀對 UI 而言是錯的,**它會把不符回報出來——而不是自己打補丁。**

**輸入:**

- 核准的技術簡報
- 研究員的調查結果
- 後端建造者的摘要(API 合約)

**它建造:**

- React 元件與頁面
- Client-side hooks 與狀態
- Loading 與 error 狀態
- 它所寫東西的元件與單元測試

**它不能做什麼:**

- 碰 services、API 路由、workers 或遷移(那是代理人 4 的工作)
- 發明端點或回應格式
- 在沒有指示下加依賴
- 沒跑 typecheck、lint 與測試套件就停下

**工具:** Read、Edit、Write、Bash——只限前端資料夾。

**兩個建造者。兩個乾淨的上下文視窗。零機率一個會弄壞另一個的工作。**

![](https://pbs.twimg.com/media/HJJbDHRaYAAWqpz?format=jpg&name=large)

#### 代理人 6:測試驗證者(Test Verifier)

兩個建造者都幫自己寫了單元測試。

**那不夠。**

測試驗證者只做一件事:**證明這個功能真的做了使用者故事說它要做的事。**

它寫的是「驗收測試」(acceptance tests),不是單元測試。

驗收測試從外部測試功能——就像一個真實使用者會去體驗它的方式。

**輸入:**

- 核准的使用者故事(包含所有驗收標準)
- 核准的技術簡報
- 兩個建造者的摘要

**產出:**

- 一份覆蓋每個驗收標準的驗收測試檔
- 一份報告:哪些透過、哪些失敗、哪些無法乾淨覆蓋

**它不能做什麼:**

- 修改任何後端或前端程式碼
- 為無法測試的標準發明繞道方案
- 把實際沒覆蓋到的標準標為已覆蓋

如果一個測試失敗:這個功能就不滿足故事。

它會回報「哪一條標準失敗了」。 **它不會去修補程式碼。**

修補會被丟回正確的建造者那邊。

**工具:** Read、Edit、Write(僅測試檔)、Bash。

**規則:在驗收測試透過之前,你還沒有這個功能。**

![](https://pbs.twimg.com/media/HJJbJ2SbIAEmcNL?format=jpg&name=large)

#### 代理人 7:實作驗證員(Implementation Validator)

這是那個會抓出大家漏掉的東西的代理人。

驗證員會把目前的實作,對照核准的故事與簡報,**回報落差** 。

**它從不修任何東西。它只說真話。**

每一次它會跑的檢查:

- 故事的驗收標準有沒有還沒實作的
- 沒有測試覆蓋的失敗路徑
- 安全議題:缺漏的許可權檢查、租戶隔離漏洞、log 裡的金鑰、原始錯誤外洩到 client
- 範圍外被改動的檔案
- 與 CLAUDE.md 或既有程式碼不一致的模式
- 應該重用既有 helper 卻被寫成重複邏輯的地方
- 簡報中被悄悄跳過的時區或多租戶考量

**輸出永遠按嚴重度分組:**

- **Critical** — 合併前必修
- **Important** — 合併前應修
- **Minor** — 偏意見,審查者自己決定

每一個發現都會附上檔案路徑與行號。

如果沒有問題,它就直接說沒問題。 **它不會為了顯得認真而發明問題。**

**工具:** Read、Grep、Glob,僅此而已。

**這個代理人,是讓整座工廠值得信任的原因。**

自評的成績單沒有價值。一個只看「磁碟上有什麼」、不看「怎麼被寫出來」的驗證員,才是誠實的。

![](https://pbs.twimg.com/media/HJJbQW2awAAyrBn?format=jpg&name=large)

### 整條鏈是怎麼跑的

完整流程——一個 prompt 啟動一切:

你開啟 Claude Code,輸入:

> 「幫我做『超過 7 天未付的發票提醒』功能。」

接下來你不需要再多打任何字,它會這樣發生:

**Step 1** → 研究員把你的發票、付款與 email 程式碼掃過一輪。回傳相關檔案、既有模式與風險。

**Step 2** → 故事撰寫者產出使用者故事與驗收標準。

⏸ **暫停:你閱讀並核准故事。**

**Step 3** → 規格撰寫者把核准的故事變成技術簡報。

⏸ **暫停:你閱讀並核准簡報。** (就在這裡抓出「把 ID 存在記憶體裡」的錯誤。)

**Step 4** → 後端建造者實作 service、API 路由、BullMQ 任務與單元測試。回傳:檔案變動、重用模式、全部測試綠燈。

**Step 5** → 前端建造者讀後端的 API 摘要,做出 admin UI 區塊與提醒按鈕,寫元件測試。全綠。

**Step 6** → 測試驗證者為六條驗收標準寫驗收測試。回報:7 條透過,1 條失敗——手動觸發沒有檢查租戶擁有權。

**Step 7** → 驗證員抓到了。以 Critical 等級回報,附檔案路徑與行號。

→ 回到後端建造者。修好。8 條驗收測試全綠。驗證員再跑一次。乾淨。

⏸ **暫停:你審查並開 PR。**

**三個人類審核點。其他都自己跑。**

![](https://pbs.twimg.com/media/HJJcDPQbsAA61t6?format=jpg&name=large)

### 基礎:代理人能運作之前,你需要這個

#### CLAUDE.md ── 存活於每個對話的記憶

每一次你開啟 Claude Code,它都是從「零記憶」開始。

CLAUDE.md 修正這件事。

它是 repo 根目錄的一個 Markdown 檔,每個對話開始時都會自動載入。

它是「永久專案事實」的家:

- 你的技術棧(Next.js App Router、Node.js、Prisma、BullMQ、Resend)
- 你的指令(`npm run dev` 、 `npm test` 、 `npx prisma migrate dev`)
- 架構規則(「商業邏輯放在 services。API 路由保持薄。」)
- 不要做的事(「不要加 cron——用 BullMQ。不要把原始付款 payload 寫進 log。」)
- 深入檔案的指標(`docs/billing.md` 、 `docs/architecture.md`)

**保持在 100–300 行。**

每次 AI 犯了一個讓你驚訝的錯誤,問自己:「如果 CLAUDE.md 裡有一條規則,這次能避免嗎?」

把規則加進去。

幾週後,你的 CLAUDE.md 就會變成「AI 曾經弄錯的所有假設」的記錄——你的對話會明顯變得更好。

![](https://pbs.twimg.com/media/HJJcHnxaMAEPefe?format=jpg&name=large)

#### 上下文漂移 ── 那個無聲的殺手

大部分 Claude Code 對話不會戲劇性地失敗。

**它們會漂移。**

一個錯的假設進入上下文。模型繼續往上面疊。

你要 Claude 做「訂閱管理」。它設計:User → Subscription。

你後來才想起:訂閱屬於「公司」,不是「使用者」。

如果你只說「不對,訂閱屬於公司」——Claude 會打補丁。

現在你就有了同時飄在四處的 `user.subscriptionId` 與 `company.subscriptionId` 。

**規則:**

- 小錯字?直接 inline 修正。
- 架構假設錯了?**把整個對話丟掉**,從頭開始,把對的假設烙進第一個 prompt。

**一個有正確心智模型的乾淨對話,永遠勝過一個打了補丁的對話。**

### 結果:真正改變的是什麼

#### 工廠之前:

- Vibe coding 迴圈:prompt → 生成 → 錯誤 → 補丁 → 重複
- 對話的上下文塞滿雜訊
- 錯的假設累積成壞掉的功能
- 一個工程師一次只能做一件事
- 每個功能都在等對的人有空

#### 工廠之後:

- 結構化的鏈:研究 → 故事 → 簡報 → 建造 → 驗證 → 確認
- 每個代理人擁有乾淨的上下文,只裝它需要的東西
- 錯的假設在「簡報核准」就被抓出——不是 10 個檔案之後
- 一個工程師可以出一個完整的垂直切片:後端、前端、測試、驗證
- 團隊最好的知識住在代理人裡——不被困在「某個人」身上

#### 真正的轉變:

付款專家做一個 payments-integration 代理人。從那一刻起,團隊每一個工程師都能出帶帳務的功能。 **不必等待,不必交接。**

前端 lead 的元件模式,住在 frontend-builder 代理人裡。DevOps 工程師的 CI 檢查,住在 hook 裡。QA lead 的邊界情況,住在 test-verifier 規則裡。

**專家知識,以代理人的形式被共享。不被困在「誰有空」這件事裡。**

### 這個週末就做出你自己的版本

#### 8 步設定清單:

1. 安裝 Claude Code → `code.claude.com`
2. 建立資料夾結構:
	- `.claude/agents/`
		- `.claude/skills/feature-factory/`
		- `.claude/skills/build-with-tests/`
		- `.claude/hooks/`
3. 寫你的 CLAUDE.md(100–300 行:技術棧、指令、架構規則、不要做的清單)
4. 用 Claude Code 的 `/agents` 指令建立 7 個代理人。描述每個代理人的角色。Claude 寫檔案。你審查並 commit。
5. 建立 feature-factory orchestrator skill。叫 Claude 幫你寫——它會讀你 7 個 agent 檔並接好整條鏈。
6. 建立 build-with-tests skill。描述你的團隊怎麼建造:對齊既有模式、邊寫程式邊寫測試、最後跑 typecheck。
7. 加一個 pre-commit hook。擋住把 `.env` 、`.key` 、`.pem` 或 `secrets.json` 提交進去。5 分鐘搞定,可以避免大型災難。
8. 跑一個真實的功能走完整條鏈。挑一個小的。觀察它在哪裡卡住。加規則。 **工廠會自己調整。**

**總時間:2–3 小時。**

跑個幾個功能。3–4 個之後,工廠就認識你的程式碼庫了。

你會花更少時間監督,更多時間決定「接下來要做什麼」。

![](https://pbs.twimg.com/media/HJJcLzybIAAGyJo?format=jpg&name=large)

### 七個代理人 ── 快速參考

- **研究員(Researcher)** — 在任何東西被造之前先掃過程式碼(只讀)
- **故事撰寫者(Story Writer)** — 把構想變成使用者故事與驗收標準(只讀)
- **規格撰寫者(Spec Writer)** — 把故事變成技術簡報(只讀)
- **後端建造者(Backend Builder)** — 建造 API、services、jobs、單元測試(只限後端資料夾)
- **前端建造者(Frontend Builder)** — 建造元件、頁面、hooks、UI 測試(只限前端資料夾)
- **測試驗證者(Test Verifier)** — 對使用者故事寫驗收測試(只限測試檔)
- **驗證員(Validator)** — 拿實作對照故事與簡報、回報落差(只讀)

**3 個人類審核點:**

→ 核准故事 → 核准簡報 → 核准 PR

**其他都自己跑。**

---

大部分用 Claude Code 的開發者還在 vibe coding。Prompt → 生成 → 補丁 → 祈禱。

那不算錯。 **但那有天花板。**

工廠不是把你從流程裡踢出去。 **它是把你從「不需要你判斷」的部分裡踢出去。**

你會留在那些「你的判斷真正重要」的環節裡:

> 這是對的問題嗎?這是對的設計嗎?這個可以安全上線嗎?

中間的所有事情,代理人負責。

**這就是「把 AI 當成更快的鍵盤」和「把 AI 當成一支協調團隊」的差別。**

原文作者： [@sairahul1](https://x.com/sairahul1/status/2058832033628241931)

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png)](https://t.me/blocktemponews/)

#### 📍相關報導📍

**[OpenAI 重磅宣布：開放 Agents SDK 支援 MCP，串聯萬物再跨關鍵一步](https://www.blocktempo.com/openai-adopts-mcp-standard-opens-agents-to-a-wide-range-of-open-source-tools/)**

**[MCP（Model Context Protocol）模型上下文協議：加密AI的數位連結器](https://www.blocktempo.com/introduction-of-mcp/)**

**[串聯萬物的 MCP 加上 Web3，能成下一波 AI 百倍敘事嗎？](https://www.blocktempo.com/web3-plus-mcp-the-next-wave-of-ai-100x-narrative/)**

**[Google、Meta 研究員聯手喊話：AI Agent 安全不是模型問題，是系統問題](https://www.blocktempo.com/google-meta-researchers-ai-agent-security-systems-problem/)**

**[交易量翻倍、多鏈布局加速，為什麼 x402 協議沒有曇花一現？](https://www.blocktempo.com/x402-protocol-sustainable-growth-analysis/)**

### 最新文章

---

[![丹麥退休基金把 SpaceX 列黑名單，批馬斯克「災難級治理」1.8 兆美元估值虛高](https://image.blocktempo.com/2026/05/spacex-ipo-1-1140x815.png "spacex ipo | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://image.blocktempo.com/2026/05/spacex-ipo-1.png)

擁有 250 億美元資產的丹麥退休基金 AkademikerPension 宣布將 SpaceX 列入黑名單，不僅批評其 1.8 兆美元目標估值「嚴重虛高」，更直指馬斯克同時身兼執行長、技術長與董事長、握有 80％ 投票權是「災難級治理結構」。  
（前情提要： [月付 29 美元到 750 美元：GitHub Copilot 改按 Token 用量收費，開發者社群炸](https://www.blocktempo.com/github-copilot-usage-based-token-billing-developer-backlash-microsoft-pricing/) ）  
（背景補充： [AI 股神 Serenity 力推的瑞典光子廠 SIVE，預期還能上修？](https://www.blocktempo.com/serenity-sive-sivers-photonics-chokepoint-bears-wrong-win-semi-scale/) ）

彭博報導 [指出](https://www.bloomberg.com/news/articles/2026-05-29/danish-pension-fund-blacklists-spacex-citing-governance-issues) ，丹麥退休基金 AkademikerPension 投資長 Anders Schelde 在一封電子郵件中明確表示，該基金不會碰 SpaceX，理由是其估值「嚴重虛高」，且治理架構「災難至極」。

### 基金自行推算上限僅 1 兆美元

我們知道，SpaceX 於 5 月 20 日提交 IPO 申請檔案，預計下個月啟動正式路演行銷。然而 AkademikerPension 的內部測算結論指出，SpaceX 的合理估值「不可能合理地超過 1 兆美元」，只有市場喊價的一半出頭，因此從投資回報角度來說，沒有理由去參與此次 IPO。

Schelde 直言，投資人在這檔 IPO 裡，等於被要求為一家「高度不確定的公司」接受「前所未有的低風險溢價」，而定價的驅動力與其說來自基本面，不如說更多仰賴馬斯克的「個人敘事」，而非「經濟現實」。

### 一人握約 80％ 投票權，美國三大退休基金同聲質疑

同樣讓 AkademikerPension 卻步的：是治理。Schelde 強調，就算估值合理，基金仍會把 SpaceX 列入黑名單。

彭博產業研究對 IPO 申請檔案的解讀：馬斯克對 SpaceX 有著「近乎絕對的控制權」，他預計掌握約 80％ 的投票權，同時一肩扛起執行長、技術長與董事長三個身分，「暴露出重大的治理疑慮」

今年 5 月 14 日，紐約市主計長 Mark Levine、加州公務員退休基金（CalPERS）執行長 Marcie Frost、以及紐約州主計長 Thomas DiNapoli，也 [聯名致信](https://comptroller.nyc.gov/reports/letter-to-spacex-re-ipo-from-nyc-comptroller-levine-nys-comptroller-dinapoli-and-calpers-ceo-frost/) 馬斯克，對他們口中 SpaceX 的「極端治理結構」表達嚴重關切。

這三支基金管理的資產規模以兆美元計，集體發聲的分量遠非一般股東信可比。

### 反差：轉頭擁抱 OpenAI

有趣的是，對於同樣備受矚目的 OpenAI，AkademikerPension 的態度卻截然不同。Schelde 透露，這家 ChatGPT 開發商本月稍早才在馬斯克提起的訴訟中勝出，馬斯克當時指控 OpenAI 轉向營利結構、背叛了最初的公益使命。

Schelde 說，「OpenAI 很可能在 IPO 後進入我們的被動股票投資組合，前提是它被納入相關市場指數。」至於是否納入主動管理的投資組合，則要看其 IPO 估值與風險評估而定。

### 超大型 IPO 繞不開的治理考題

當然對 SpaceX 而言，這筆拉黑的金額或許微不足道。但它點出的問題，卻是這場史詩級 IPO 繞不開的考題：當一家公司的估值高度依賴創辦人的個人敘事、權力又高度集中於一人之手，機構投資人該如何為風險定價？

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

#### 📍相關報導📍

**[xAI急推Grok Build衝到v0.2.11！挖角Cursor兩核心，企圖追上Claude Code](https://www.blocktempo.com/xai-grok-build-coding-agent-cursor-hires-claude-code-benchmark/)**

**[DAT加密儲備企業盈虧大盤點！Strategy虧19億鎂，買$HYPE兩企業浮盈](https://www.blocktempo.com/dat-treasury-companies-profit-loss-strategy-metaplanet-below-nav-scorecard/)**

**[MiniMax 準備 A 股 IPO 科創板！與智譜同步上市吸 AI 資本](https://www.blocktempo.com/minimax-citic-ipo-zhipu-ai-star-board-dual-listing-race/)**

**[Quantinuum量子IPO認購破錶：上調發行價10%，估值衝140億鎂，6/4掛牌](https://www.blocktempo.com/quantinuum-quantum-ipo-double-digit-oversubscribed-price-hike-nasdaq-listing/)**

**[比特幣73,600橫盤、以太坊守住2,000鎂！恐懼指數23，HYPE獨破67美元新高](https://www.blocktempo.com/bitcoin-73600-sideways-ethereum-holds-2000-hype-breaks-67-ath/)**