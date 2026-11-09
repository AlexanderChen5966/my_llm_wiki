---
title: "WebMCP Skill 與 Types 是什麼？Chrome 團隊推廣的開源工具怎麼用、風險在哪"
source: "https://www.aiposthub.com/webmcp-skill-types-chrome-agent-ready-website/"
author:
  - "[[Philo]]"
published: 2026-10-21
created: 2026-11-04
description: "Chrome 團隊近期推廣三項 WebMCP 開源工具，其中 WebMCP Skill 教 AI 編碼代理設計、審查與實作網頁工具，WebMCP Types 補上 TypeScript 型別。本文整理安裝方式、安全規則、官方評測的讀法與導入前檢查。"
tags:
  - "clippings"
status: "inbox"
---
WebMCP Skill 想解決的問題很具體：你的網站有「加入購物車」和「預約訂位」，AI 代理想替使用者操作時，今天多半是看畫面、猜按鈕、一格一格點。WebMCP 的想法是讓網頁直接告訴代理，我這裡有哪些工具、每個工具要填什麼。

聽起來只要把功能包成工具就好，但寫起來有不少講究。工具名稱太長、描述太囉嗦、沒標示「這一步會扣款」，代理就可能選錯工具，甚至在使用者沒確認時送出訂單。

Chrome 團隊的 Sarah Drasner 在台灣時間 10 月 19 日晚上 9 點多於 X 發文，文字提到兩項開源工具，附的資訊圖則多列了第三項，我在 10 月 21 日早上查詢時，貼文約有 860 個讚。這只是社群熱度的線索，細節以官方倉庫為準。

貼文文字提到的是 WebMCP Skill 與 WebMCP Types。前者是一組給 AI 編碼代理讀的說明檔，涵蓋安全、開發與高風險流程的最佳做法。後者是 TypeScript 型別定義，能從工具的輸入規格推出參數型別。

資訊圖上的第三項是瀏覽器擴充功能，後面會簡單帶到。

Skill 在這裡指的是一個資料夾，裡面有主說明檔與幾份參考文件，代理遇到相關任務時才去讀，所以不用每次都塞進對話。

我的判斷是，正在試做 WebMCP 的前端團隊值得先讀一遍。想一鍵讓網站變成代理可用的人要放慢，因為 WebMCP 本身還在提案階段，Google 也在倉庫聲明這不是正式支援的產品。

要看懂這兩個工具補了什麼洞，得先弄清楚 WebMCP 讓網頁多出哪些能力。

## WebMCP 是什麼？網頁自己宣告工具，代理不必再猜按鈕

WebMCP 讓網頁用標準方式列出自己能做的事，AI 代理就能直接呼叫，而不用模擬滑鼠點擊。

依 Chrome 官方文件，它是一項「提案中的網頁標準」，提供兩種寫法。

命令式寫法用 JavaScript 在 document.modelContext 上註冊工具。宣告式寫法則是替 HTML 表單加上 toolname 與 tooldescription 屬性。

官方的說法是，這比讓代理逐步操作畫面更可靠，因為每個步驟都不必由代理自己詮釋。工具會在頁面上看得見地執行，使用者也能確認任務真的完成。

目前的取得方式要留意。

官方文件寫的是 Chrome 149 起可以加入 WebMCP 的來源試用，也就是網站先登記，就能提前在自己的頁面上試用實驗功能。本機開發則在 chrome://flags 裡開啟 enable-webmcp-testing 旗標。

規格本身由 W3C 的 Web Machine Learning 社群小組維護。我在 10 月 21 日查詢時，頁面標示為 10 月 20 日的社群草案，並註明它不是 W3C 標準，也不在標準軌道上。

所以這套工具現在服務的是「提前準備」的團隊，而不是已經能面向所有瀏覽器使用者的功能。

Chrome 文件也承認限制。它主要為本機瀏覽器、有人在旁邊的流程設計，複雜的介面可能要先整理狀態管理，瀏覽器也得先造訪網站才知道有哪些工具。

了解這個背景，就能看 WebMCP Skill 具體在教代理什麼。

延伸閱讀： [WebMCP Challenge 開跑：OpenAI 用 10 天挑戰「讓網站直接給 AI Agent 使用」](https://www.aiposthub.com/openai-webmcp-challenge-ai-agent-web/)

![Google 資訊圖：WebMCP 三項新工具 webmcp-extension、webmcp-types 與 webmcp-skills](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/webmcp-skills-types-cover.jpg)

Chrome 團隊成員 Sarah Drasner 在 X 貼出的 Google 資訊圖，介紹 WebMCP 的三項開源工具，非本站製作。 圖片來源： 官方原文 。

![](https://www.youtube.com/watch?v=bG5Dwkz65Es)

Chrome for Developers 官方頻道影片《Answering your questions about WebMCP》（2026 年 10 月 19 日上傳，約 22 分鐘），非本站實測。 來源： [官方 YouTube](https://www.youtube.com/watch?v=bG5Dwkz65Es&ref=aiposthub.com) 。

## WebMCP Skill 在做什麼？從設計到審查的六個階段

WebMCP Skill 的任務是讓編碼代理在寫工具之前先設計對話，而不是一上來就包函式。

它放在 GoogleChromeLabs 的 webmcp-tools 倉庫裡，目錄叫 webmcp-skills，授權是 Apache-2.0。對外的技能只有一個，名字是 build-webmcp-tools。

GitHub 紀錄顯示這個目錄在 9 月 29 日首次提交，10 月 20 日仍有更新，所以這組工具並不是 10 月 19 日才出現。

技能會依你的需求把對話導向不同階段。依 README，六個階段依序是盤點使用者目標、列出起始狀態、逐回合角色扮演、補上邊界情境、產出工具規格與測試案例，最後才是依框架寫程式。

這個順序的用意，是避免「函式很簡單，所以工具也很簡單」的誤會。

技能文件直接寫道，程式碼簡單不代表對話簡單。例如「儲存我剛打開的那一筆」遇到重複網址時，代理就容易出錯。

它還把 Chrome 的寫作限制寫成必守規則：工具名稱與參數名稱不超過 30 個字元，工具描述不超過 500 字元，參數描述不超過 150 字元，單次輸出不超過 1,500 字元。

這些數字出自 Chrome 的安全文件，官方也註明可能隨生態回饋調整，所以它們是建議值，不是規格書的硬性條款。

註解（annotations）的規則解決另一類問題。readOnlyHint 標示只讀，consequentialHint 標示不可逆的重大動作，untrustedContentHint 標示內容來自使用者或第三方。

技能的核心規則是這些提示預設都是 false，只在需要時設為 true。例如切換分頁不該標成只讀。付款、訂位這類動作則建議先交給一個讓使用者按確認的畫面，只有真正送出動作的那個工具才標成重大動作，讓代理或瀏覽器在執行前先問使用者。

錯誤處理也有差別。技能文件說明，React 的寫法要丟出錯誤，原生 registerTool 則要回傳含 error、code、retryable 的結果。

否則拒絕會被轉成一個沒有資訊的通用錯誤，代理就無從修正。

實際使用時，你不需要記這些階段。

README 給了幾種提示範例，例如「Add WebMCP tools to this project」會讓代理先檢查專案再走完整流程，「Audit the WebMCP tools in this repo」則直接進入審查。

審查報告的結尾，技能會列出驗證步驟：在 Chrome DevTools 的 Application 面板手動執行 WebMCP 工具，再跑 Lighthouse 的 Agentic browsing 稽核。

寫好工具與測試案例後，也可以用倉庫另一個工具跑評測，指令是 npx webmcp-evals local，並指定 schema.json 與 evals.json。

了解它做什麼之後，還要看另一個工具 Types 補了什麼。

## WebMCP Types 補了什麼？讓 TypeScript 認得 document.modelContext

Types 解決的是一個很實際的痛點：TypeScript 預設不認識 document.modelContext，寫起來不是滿滿的 any，就是被編譯器擋下。

它放在 webmachinelearning 組織下，也就是發布 WebMCP 規格草案的 W3C 社群小組帳號，不是 GoogleChromeLabs。內容是一份.d.ts 宣告檔，npm 套件名叫 webmcp-types，授權為 MIT。

npm 顯示最新版是 0.1.10，倉庫最近一次發版在 9 月 28 日，新增了 toolactivated 與 toolcancel 兩個事件的型別。

從更新紀錄看，這個套件 7 月初才在 npm 建立，到 9 月底已經走到 0.1.10，介面變動不小，所以專案裡最好鎖定版本。

使用方式是把它當開發依賴安裝，再到 tsconfig 的 types 欄位加上 webmcp-types。

README 寫明需要 TypeScript 5.0 以上。從宣告檔看，推論用到了 5.0 才有的 const 型別參數，這大概就是原因。

Sarah 貼文提到的「從 InputSchema 推出型別」，我對照了宣告檔，確實有這個設計。

registerTool 在你用字面值寫出輸入規格時，會讓 execute 的參數型別自動符合規格。例如規格說 quantity 是數字，函式裡就會被當成 number。

這對代理寫的程式特別有用。代理偶爾會把欄位名稱拼錯，例如把 itemId 寫成 `item_id` ，加上型別後編譯器會立刻抱怨，不用等到瀏覽器執行才發現。

但型別只管編譯期，執行時仍然需要瀏覽器真的支援 WebMCP，或使用 webmcp-tools 倉庫 demo 目錄附的相容層（polyfill）來測試。

型別推論的範圍也有限。我看宣告檔，處理的是固定值、列舉、基本型別、陣列與物件，沒有看到 anyOf 這類組合寫法，複雜規格就得自己補型別。

除了這兩個，貼文附的資訊圖還列了第三項，一個可以在瀏覽器裡檢查與測試 WebMCP 工具的 Chrome 擴充功能。

倉庫是 GoogleChromeLabs 的 webmcp-extension，同樣標示為不受官方支援的範例。它預設把你的提示送到本機 Node 伺服器，再轉給 Gemini 等模型，金鑰留在本機伺服器上。

## WebMCP Skill 的官方評測怎麼讀？99.53% 不等於網站會更好用

倉庫附了一份評測報告，但它量的是「代理有沒有照技能的規則寫」，不是「網站被代理使用後成功率提高多少」。

報告的設定是 74 個測試案例，每個案例分別在有技能與沒有技能的情況下各跑 3 次，共 444 次。

使用的模型是 gemini-3.5-flash-lite，平均通過率有技能時為 99.53%，沒有技能時為 42.37%。

改用「三次都全過才算過」的嚴格算法，有技能時 74 個案例中有 71 個通過，沒有技能時只有 15 個。

這份成績的比較口徑要看清楚。測的人是倉庫維護者自己，題目與評分規則寫在同一個倉庫，內容包括字元上限、註解預設值、錯誤處理方式，這些剛好是技能教的東西。

沒有技能的基準組看不到這些規則，所以在字元預算這一項拿 0 分並不意外。

因此較合理的讀法是：在這組題目上，技能讓這個模型更常照 Chrome 的建議寫。至於這些建議能不能讓真實網站被代理操作得更準，這份報告沒有量到。

成本也要一起看。報告列的平均總詞元是有技能 13,614、沒有技能 1,538，我依官方表格相除，大約是 8.9 倍，平均延遲則是 4.11 秒對 4.50 秒。

詞元是模型計費與處理文字的單位，所以技能帶來的是更多讀取的內容與更高的用量。

如果你用的是按量計費的模型，每次啟動技能前最好先估算這筆開銷。

## 適合誰、不適合誰與導入前檢查

適合已經在評估 WebMCP、並且用 Claude Code、Cursor、Gemini CLI 這類編碼代理做前端的團隊，不適合期待「裝了就自動讓全站變成代理可用」的人。

技能文件把使用情境分得很清楚。新專案可以走完整六階段，已有工具的專案可以直接進入審查。

只想補一個小工具時也有快速路徑，但技能會先提醒你，跳過設計會讓模糊需求與確認時機沒被測試。

如果你的網站沒有需要代理代勞的重複流程，例如只有文章閱讀，現在投入的效益很有限。

反過來，預約、下單、客服表單這類欄位明確又常出錯的流程，最能看出差別。

決定試用前，我會做三件小事。先用 Chrome 的旗標在本機把頁面跑起來，確認 document.modelContext 存在。

再挑一個最小的唯讀工具，例如查詢商品，檢查名稱與描述有沒有超過字元上限。

最後才處理會改動資料的工具，把需要使用者確認的動作標成 consequentialHint，並在 DevTools 手動執行一次。這樣的順序可以把風險留到最後，前面的錯誤也比較好抓。

至於安裝，倉庫提供兩種方式。一種是執行 npx skills add 指令，另一種是手動把 skills 目錄裡的 build-webmcp-tools 資料夾複製到專案的.claude/skills 或.agents/skills。

我會選手動複製。因為技能本質上是一堆文字說明檔，先下載、讀過內容再放進專案，比直接執行不熟悉的套件更有把握。

延伸閱讀： [Cloudflare WebMCP 是什麼？一鍵讓網站支援 AI Agent，真正能做什麼？](https://www.aiposthub.com/cloudflare-webmcp-ai-agent-website/)

## 限制與風險：提案階段、提示詞注入與評測範圍

最大的限制是標準還沒定案，其次是安全問題沒有被技能自動解決。

官方安全文件直白地說，沒辦法保證大型語言模型內部的安全，因為模型有機率性，而針對代理的提示詞注入攻擊已有可重現案例。

提示詞注入是指攻擊者把惡意指令藏在網頁或工具回傳的文字裡，誘使代理照做。

所以如果你的工具會回傳使用者留言，就要標上 untrustedContentHint，讓代理知道這些字只是資料。

跨來源也有規則。工具預設只在同一來源開放，要讓嵌入的 iframe 使用，得加上 allow="tools"，並用 exposedTo 指定信任的來源。

官方還提醒，Chrome 擴充功能可以用內容腳本查詢並執行 WebMCP 工具，所以別以為只有網站自己的程式碼能呼叫你的工具。

技能所在的 webmcp-tools 倉庫，以及那個擴充功能倉庫，都寫明「不是 Google 正式支援的產品」，也不在 Google 的開源漏洞獎勵計畫範圍內，出問題時你得自己承擔。

Types 屬於 W3C 社群小組的帳號，README 沒有這句聲明，但它對應的規格本身仍是草案，介面還會變。

另外，倉庫附的評測需要自己準備模型金鑰，README 的範例使用 Gemini 金鑰。在團隊環境裡跑評測時，金鑰要放在安全位置，不要寫進程式碼。

## WebMCP Skill 常見問題

### WebMCP Skill 只能在 Chrome 用嗎？

技能本身只是說明檔，倉庫說明可以放進 Antigravity、Gemini CLI、Claude Code、Cursor 與 VS Code 的技能資料夾。

但被它引導寫出的 WebMCP 工具，要靠支援 WebMCP 的瀏覽器才跑得起來，目前官方文件談的是 Chrome。

### WebMCP 跟一般 MCP 伺服器一樣嗎？

不一樣。WebMCP 在瀏覽器分頁裡執行，使用的是使用者已登入的頁面狀態，而一般 MCP 伺服器多半跑在後端。

技能文件明確把後端 MCP 伺服器列為範圍之外，並說 WebMCP 只支援工具，不支援資源與提示。

### Skill 與 Types 一定要一起用嗎？

不用。Skill 負責設計與審查規則，Types 負責讓 TypeScript 認得介面，兩者可以單獨使用。

一起用時，代理寫出的程式比較容易通過型別檢查，也比較符合 Chrome 建議的欄位長度與註解寫法。

### 官方評測的 99.53% 可以直接引用嗎？

最好寫清楚口徑再引用：這是倉庫維護者用 gemini-3.5-flash-lite 跑的 74 個測試案例，量的是照規則寫的程度。

換成別的模型或真實網站，結果可能不同，官方也沒有給出這種條件下的數字。

## 先挑一個小工具，驗證代理真的選得對

WebMCP Skill 與 Types 的價值，在於把 Chrome 團隊累積的設計經驗交給你的編碼代理，並讓型別系統幫忙攔下低級錯誤。

如果你想試，先挑網站裡一個欄位明確的流程，用技能設計一個唯讀工具和一個需要確認的工具，再用 DevTools 與 Lighthouse 檢查。

這樣能很快看出，代理選工具的行為有沒有變得更穩定。

想持續追蹤 WebMCP 與瀏覽器代理的進展，可以免費訂閱 AI 郵報，也建議定期回頭看 Chrome 官方文件的更新日期。

## 資料來源

- Chrome for Developers，WebMCP： [https://developer.chrome.com/docs/ai/webmcp](https://developer.chrome.com/docs/ai/webmcp?ref=aiposthub.com)
- Chrome for Developers，WebMCP 工具安全： [https://developer.chrome.com/docs/ai/webmcp/secure-tools](https://developer.chrome.com/docs/ai/webmcp/secure-tools?ref=aiposthub.com)
- Chrome for Developers，WebMCP 最佳實務： [https://developer.chrome.com/docs/ai/webmcp/best-practices](https://developer.chrome.com/docs/ai/webmcp/best-practices?ref=aiposthub.com)
- Chrome for Developers 官方 YouTube，Answering your questions about WebMCP： [https://www.youtube.com/watch?v=bG5Dwkz65Es](https://www.youtube.com/watch?v=bG5Dwkz65Es&ref=aiposthub.com)
- GoogleChromeLabs/webmcp-tools（Apache-2.0）： [https://github.com/GoogleChromeLabs/webmcp-tools](https://github.com/GoogleChromeLabs/webmcp-tools?ref=aiposthub.com)
- WebMCP Skills README： [https://github.com/GoogleChromeLabs/webmcp-tools/tree/main/webmcp-skills](https://github.com/GoogleChromeLabs/webmcp-tools/tree/main/webmcp-skills?ref=aiposthub.com)
- WebMCP Skills 評測報告 BENCHMARK.md： [https://github.com/GoogleChromeLabs/webmcp-tools/blob/main/webmcp-skills/BENCHMARK.md](https://github.com/GoogleChromeLabs/webmcp-tools/blob/main/webmcp-skills/BENCHMARK.md?ref=aiposthub.com)
- webmachinelearning/webmcp-types（MIT）： [https://github.com/webmachinelearning/webmcp-types](https://github.com/webmachinelearning/webmcp-types?ref=aiposthub.com)
- webmcp-types npm 套件頁： [https://www.npmjs.com/package/webmcp-types](https://www.npmjs.com/package/webmcp-types?ref=aiposthub.com)
- GoogleChromeLabs/webmcp-extension： [https://github.com/GoogleChromeLabs/webmcp-extension](https://github.com/GoogleChromeLabs/webmcp-extension?ref=aiposthub.com)
- WebMCP 規格草案（W3C Web Machine Learning 社群小組）： [https://webmachinelearning.github.io/webmcp/](https://webmachinelearning.github.io/webmcp/?ref=aiposthub.com)
- Sarah Drasner 原貼文（熱度線索，含資訊圖）： [https://x.com/sarah\_edo/status/2112167393044672954](https://x.com/sarah_edo/status/2112167393044672954?ref=aiposthub.com)