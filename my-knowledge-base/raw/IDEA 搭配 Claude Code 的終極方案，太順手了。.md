---
title: "IDEA 搭配 Claude Code 的終極方案，太順手了。"
source: "https://codelove.tw/@tony/post/3DREjq"
author:
published:
created: 2026-07-07
description: "IDEA 爽用 Claude Code 的終極方案，太絲滑。  =================================    大家好，我是二哥呀。之前有讀者在留言區安利了一款叫 CC GUI 的 IntelliJ IDEA 外掛。可以在 idea 中爽用 Claude Co..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月06日](https://codelove.tw/@tony/post/3DREjq "2026-07-06 08:17:38")


## IDEA 爽用 Claude Code 的終極方案。

```
大家好，我是二哥呀。
```

之前有讀者在留言區安利了一款叫 CC GUI 的 IntelliJ IDEA 外掛。

可以在 idea 中爽用 Claude Code。

剛好 DeepSeek V4 pro 大降價，我就想著，能不能在 CC GUI 中配置上 DeepSeek V4 的 API key，然後爽用起來。

[![](https://i.imgur.com/rMKIWkV.jpeg)](https://i.imgur.com/rMKIWkV.jpeg)

2.5 折，比打 10 折確實優惠得多😄

從我之前發的內容來看，用 idea 的小夥伴還是非常多的，尤其是後端開發，離不開 IntelliJ IDEA 啊，查看程式碼、除錯還是方便。

[![](https://i.imgur.com/dPhwHf7.jpeg)](https://i.imgur.com/dPhwHf7.jpeg)

索性今天我們就來實踐一把，手把手帶大家體驗。

> 外掛地址： [plugins.jetbrains.com/plugin/2934…](https://link.juejin.cn/?target=https%3A%2F%2Fplugins.jetbrains.com%2Fplugin%2F29342-cc-gui-claude-or-codex-)

[![](https://i.imgur.com/2GByqJj.jpeg)](https://i.imgur.com/2GByqJj.jpeg)

支援的功能也非常多，比如說圖片解析、Skill 命令、MCP 伺服器等等。

[![](https://i.imgur.com/mnuM8Cy.jpeg)](https://i.imgur.com/mnuM8Cy.jpeg)

## 01、怎麼裝和怎麼配？

裝起來沒什麼門檻。

IDEA 裡按 Cmd+Shift+A 叫出搜尋，輸入 Plugins 進到外掛市集，搜 “CC GUI” 就能看到結果。我這裡已經安裝過了。

[![](https://i.imgur.com/fwrsjaQ.jpeg)](https://i.imgur.com/fwrsjaQ.jpeg)

裝好以後 IDE 都不用重新啟動。右側邊欄多出一個 “CC GUI” 圖示，點一下就能打開操作面板。

[![](https://i.imgur.com/OnQMr2k.jpeg)](https://i.imgur.com/OnQMr2k.jpeg)

第一次打開的時候會跳出提示，讓你先把 Claude Code SDK 裝上，這是 Agent 跑起來的底層依賴，跟著引導，大概半分鐘搞定。

[![](https://i.imgur.com/3lYVwej.jpeg)](https://i.imgur.com/3lYVwej.jpeg)

SDK 到位之後，下一步是配模型。點開供應商設定介面：

[![](https://i.imgur.com/3z0mX0u.jpeg)](https://i.imgur.com/3z0mX0u.jpeg)

支援直接讀取本地已有的 settings.json 來完成驗證。

換句話說，如果你的 Claude Code 本來就已經跑通了，把設定路徑指過來就好。用國內的 Coding Plan（GLM-5.1、Kimi 2.6 之類的）都沒問題，不用非得登入 Anthropic 帳號。

除了透過 settings.json，還支援新增供應商。

點擊【+新增】，選擇 DeepSeek。

[![](https://i.imgur.com/toikMvC.jpeg)](https://i.imgur.com/toikMvC.jpeg)

往下翻，把 deepseek-v4-pro 填入到 Sonnet 和 Opus 中，把 deepseek-v4-flash 填入到 haiku 中。

[![](https://i.imgur.com/aOeqx4j.jpeg)](https://i.imgur.com/aOeqx4j.jpeg)

然後點擊【確認新增】就 OK 了。

本質上，是幫你重寫一下 setting.json 檔案。

配完之後在統計面板能看到 token 消耗明細，每輪對話花了多少、累計燒了多少，一目了然。

[![](https://i.imgur.com/xwEbWCP.jpeg)](https://i.imgur.com/xwEbWCP.jpeg)

MCP 可以給 Agent 掛載各種外部工具。

首先推薦 Chrome Devtools 這個 MCP，掛上之後 Agent 能直接操控瀏覽器，做前端頁面的自動化測試和除錯。特別是那些必須登入才能復現的 bug，或者需要特定瀏覽器環境才能觸發的問題，有了它方便太多。

[![](https://i.imgur.com/GoaHibx.jpeg)](https://i.imgur.com/GoaHibx.jpeg)

還有一個 idea MCP，可以讓 Claude Code 跟 IntelliJ IDEA 本身建立連線。

另外 git 提交相關的 MCP 也值得裝，自動幫你寫 commit 訊息，省心。

[![](https://i.imgur.com/W7XSlpA.jpeg)](https://i.imgur.com/W7XSlpA.jpeg)

這個功能我之前是靠 GitHub Desktop 裡內建的 Copilot 來做的。

[![](https://i.imgur.com/pRKdorr.jpeg)](https://i.imgur.com/pRKdorr.jpeg)

然後要說的是 Skill，這塊可以理解為 Claude Code 的能力擴充包，給 Agent 裝上不同的 Skill 就能解鎖對應領域的專項能力。

我個人認為必裝的兩個：web-access 和 frontend-design。

[![](https://i.imgur.com/8mDKk9Y.jpeg)](https://i.imgur.com/8mDKk9Y.jpeg)

web-access 的作用是打通瀏覽器通道，碰到需要登入憑證的頁面存取、或者要模擬真實使用者行為的場景，用它特別順手。

frontend-design 則是前端頁面生成器，丟一段需求描述過去就能出 HTML/CSS，整體設計感比我預想的要好不少，至少比自己從零寫快得多。

CC GUI 在 GitHub 上已經累積了 3.1k 的星標，社群活躍度很高。作者迭代節奏也快，差不多每個星期都推新版，修 bug 和加功能都很及時。

> [github.com/zhukunpengl…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fzhukunpenglinyutong%2Fidea-claude-code-gui%2Fblob%2Fmain%2FREADME.zh-CN.md)

## 02、實戰1：文章閱讀統計功能優化

東西裝好了，直接上手幹活。第一個場景拿技術派的真實業務來試。

技術派是我們自己做的一個開源社群。

PaiAgent 這個開源專案的教學也會在上面同步。

[![](https://i.imgur.com/nhCpoAB.jpeg)](https://i.imgur.com/nhCpoAB.jpeg)

目前閱讀數的更新邏輯寫在 `ArticleReadService.java` 裡面，每次有讀者看文章就直接打一條 SQL 到資料庫。我打算把它換成 Redis 先扛著、再定時刷回資料庫的方案，改動會橫跨 Service、Controller 和定時任務好幾個檔案。

在 CC GUI 的對話框裡我輸入：「幫我優化技術派專案的文章閱讀統計功能，改成 Redis 快取方案，先累加 Redis 計數，再定時同步到資料庫」。

[![](https://i.imgur.com/2FAmHfA.jpeg)](https://i.imgur.com/2FAmHfA.jpeg)

Agent 第一件事就是把現有程式碼結構看了一遍，順便檢查了專案依賴，確認 Redis 相關的配置早就到位了。

[![](https://i.imgur.com/RN3pVXY.jpeg)](https://i.imgur.com/RN3pVXY.jpeg)

定位到了瓶頸所在，隨即給出了整套改造思路。

[![](https://i.imgur.com/Sn8YC5s.jpeg)](https://i.imgur.com/Sn8YC5s.jpeg)

首先動刀 CountServiceImpl.incrArticleReadCount()，把直連資料庫的程式碼幹掉，只留 Redis 的計數累加邏輯：

[![](https://i.imgur.com/Weh3Hnq.jpeg)](https://i.imgur.com/Weh3Hnq.jpeg)

緊接著補一個定時同步任務，每隔 5 分鐘把 Redis 裡的閱讀量刷到資料庫。

第三步是批量更新的邏輯。

[![](https://i.imgur.com/BjK31Ux.jpeg)](https://i.imgur.com/BjK31Ux.jpeg)

最後一步，補上單元測試。

[![](https://i.imgur.com/EoqGuGR.jpeg)](https://i.imgur.com/EoqGuGR.jpeg)

方案沒問題，我點了確認讓 Agent 動手。

全程下來我只需要在關鍵節點上做好決策，至於具體怎麼寫程式碼、怎麼建立檔案、方法之間怎麼串起來，Agent 全包了。

[![](https://i.imgur.com/MhPUTFY.jpeg)](https://i.imgur.com/MhPUTFY.jpeg)

仔細看了下生成的 Redis 快取方案，有幾處細節我覺得處理得確實不錯：

首先，用了 Redis 的 `incr` 做原子遞增，高併發下不會出現計數混亂的問題。

```
java 代碼解讀複製代碼@Override
public void incrArticleReadCount(Long authorUserId, Long articleId) {
    // 移除: articleDao.incrReadCount(articleId);
    // 只更新 Redis 計數器
    RedisClient.pipelineAction()
            .add(CountConstants.ARTICLE_STATISTIC_INFO + articleId, CountConstants.READ_COUNT,
                    (connection, key, value) -> connection.hIncrBy(key, value, 1))
            .add(CountConstants.USER_STATISTIC_INFO + authorUserId, CountConstants.READ_COUNT,
                    (connection, key, value) -> connection.hIncrBy(key, value, 1))
            .execute();
}
```

其次，給 key 加了過期時間，冷門文章的計數不會一直賴在記憶體裡。再者，定時同步走的是批量模式，一次性刷多筆紀錄，減少了資料庫連線的開銷。

```
java 代碼解讀複製代碼/**
 * 每5分鐘執行一次，將 Redis 中的文章閱讀計數同步到資料庫
 */
@Scheduled(cron = "0 */5 * * * ?")
public void syncArticleReadCountToDb() {
    Long start = System.currentTimeMillis();
    log.info("開始同步文章閱讀計數到資料庫");

    // 掃描所有文章統計 key
    Set<String> keys = scanKeys(CountConstants.ARTICLE_STATISTIC_INFO + "*");

    int batchSize = 100;
    int synced = 0;

    for (String key : keys) {
        try {
            // 提取 articleId
            Long articleId = Long.parseLong(key.replace(CountConstants.ARTICLE_STATISTIC_INFO, ""));

            // 取得 Redis 中的閱讀計數
            Integer readCount = RedisClient.hGet(key, CountConstants.READ_COUNT, Integer.class);
            if (readCount != null && readCount > 0) {
                // 批量更新資料庫
                batchUpdateReadCount(articleId, readCount);
                synced++;

                if (synced % batchSize == 0) {
                    log.info("已同步 {} 篇文章的閱讀計數", synced);
                }
            }
        } catch (Exception e) {
            log.error("同步閱讀計數失敗, key: {}", key, e);
        }
    }

    log.info("同步文章閱讀計數完成，共同步 {} 篇文章，耗時: {}ms", synced, System.currentTimeMillis() - start);
}
```

Agent 替我想得很周全。

改完直接讓它跑一下驗證，會透過 Chrome Devtools MCP 在瀏覽器端走一遍測試流程。

[![](https://i.imgur.com/wPvrxgR.jpeg)](https://i.imgur.com/wPvrxgR.jpeg)

[![](https://i.imgur.com/iWWikfH.jpeg)](https://i.imgur.com/iWWikfH.jpeg)

## 03、實戰2：留言系統分頁查詢重構

第二個案例同樣來自技術派，這回是拿留言系統的分頁查詢開刀。

改完的效果類似微信公眾號的留言區，我認為效果還是非常不錯的。

超過 1 條回覆的話，就有一個展開的功能。

[![](https://i.imgur.com/KvkEWz2.jpeg)](https://i.imgur.com/KvkEWz2.jpeg)

展開後也支援 markdown，使用者體驗我自認為很不錯，😄

[![](https://i.imgur.com/SFH1EHW.jpeg)](https://i.imgur.com/SFH1EHW.jpeg)

`CommentReadService.java` 裡之前的做法是一把梭——查文章留言的時候把一級留言和下面的回覆一股腦全撈出來。資料量小還好，一旦留言多起來就很浪費網路流量，畢竟我加了劃線留言，llm 回覆的內容很多。

如果一篇熱門文章掛著 100 條留言，每條底下再帶 10 條回覆，一次性查 1000 條紀錄，前端渲染會非常卡。

我的想法是做成懶加載：頁面打開先只展示一級留言，使用者手動點「查看回覆」的時候再非同步去拉二級內容。這樣首屏速度上來了，體驗也更流暢。

這個改動前後端都得動。

後端要拆查詢邏輯、新開一個專門拿回覆列表的介面；前端要加互動事件和載入過渡動畫。擱以前我得兩邊來回協調介面格式，折騰半天。

現在直接在 CC GUI 裡說：「幫我重構技術派專案的留言系統，改成一級留言和二級回覆分開載入。先查一級留言分頁，點擊展開時再非同步載入回覆」。

Agent 先把相關程式碼掃了一圈。

[![](https://i.imgur.com/AJNTpH5.jpeg)](https://i.imgur.com/AJNTpH5.jpeg)

[![](https://i.imgur.com/Giw7ryV.jpeg)](https://i.imgur.com/Giw7ryV.jpeg)

掃完之後，給出了完整的改造藍圖：

[![](https://i.imgur.com/Pnafpot.jpeg)](https://i.imgur.com/Pnafpot.jpeg)

[![](https://i.imgur.com/lZmcJ3H.jpeg)](https://i.imgur.com/lZmcJ3H.jpeg)

在 IDEA 裡面看 Agent 改後端程式碼有一個天然優勢：改到哪你就能跟到哪，及時審查。

[![](https://i.imgur.com/6pfVNL8.jpeg)](https://i.imgur.com/6pfVNL8.jpeg)

這一點是終端或者 Web 版 Agent 做不到的。

具體的執行步驟如下：

1. 改 TopCommentDTO，補上 childCommentCount（子留言總數）和 hasMoreChild（是否還有更多回覆）兩個欄位
2. 在 CommentReadService 裡新增 getSubComments(Long topCommentId, PageParam page) 方法
3. CommentRestController 新開 /comment/api/subcomments 介面
4. 前端模板裡加點擊展開和非同步載入的邏輯

[![](https://i.imgur.com/7YJWqBo.jpeg)](https://i.imgur.com/7YJWqBo.jpeg)

這次的工程量明顯大了，一共拆成了 9 個子任務。需要碰的檔案清單也列得清清楚楚：

模組檔案操作 [paicoding-apiTopCommentDTO.java](http://paicoding-apitopcommentdto.java/) 新增欄位 [paicoding-apiSubCommentListVO.java](http://paicoding-apisubcommentlistvo.java/) 新建 [paicoding-serviceCommentReadService.java](http://paicoding-servicecommentreadservice.java/) 新增介面方法 [paicoding-serviceCommentReadServiceImpl.java](http://paicoding-servicecommentreadserviceimpl.java/) 修改+新增方法 [paicoding-serviceCommentDao.java](http://paicoding-servicecommentdao.java/) 新增方法 [paicoding-webCommentRestController.java](http://paicoding-webcommentrestcontroller.java/) 新增介面paicoding-uicomment-item.html修改模板跑完之後看一下實際效果。

[![](https://i.imgur.com/rfmD3he.jpeg)](https://i.imgur.com/rfmD3he.jpeg)

還挺像回事的 😄

[![](https://i.imgur.com/HMo5h9U.jpeg)](https://i.imgur.com/HMo5h9U.jpeg)

## 05、CC GUI 的工作原理

用完了再來聊聊原理，搞清楚 CC GUI 到底是怎麼把 Claude Code 搬進 IDEA 的。

可能有小夥伴的第一反應是：它是不是就是在 IDEA 裡面套了個終端，然後敲 `claude` 命令？

肯定不是的。

CC GUI 根本沒有呼叫 Claude Code CLI（也就是你在終端裡敲的那個 `claude` 命令），而是直接載入 Anthropic 官方發布的 **Claude Agent SDK** （ `@anthropic-ai/claude-agent-sdk` ）。

[![](https://i.imgur.com/Qtpg0a9.jpeg)](https://i.imgur.com/Qtpg0a9.jpeg)

這也是為什麼第一次打開 CC GUI 的時候，它會讓你安裝「Claude Code SDK」。這個 SDK 會被安裝到 `~/.codemoss/dependencies/claude-sdk/node_modules/` 目錄下。

[![](https://i.imgur.com/oAtfyFK.jpeg)](https://i.imgur.com/oAtfyFK.jpeg)

整個通訊架構分三層，我畫個圖大家就明白了：

[![](https://i.imgur.com/aJBGGAL.jpeg)](https://i.imgur.com/aJBGGAL.jpeg)

核心是中間那層 **daemon.js** 。

CC GUI 啟動後會在背景 spawn 一個 Node.js 子程序，跑一個叫 `daemon.js` 的腳本。這個程序會常駐在背景，不是每次對話都起一個新程序。

daemon.js 啟動的時候就會把 Claude Agent SDK 預先載入到記憶體裡。這樣做的好處是：SDK 的初始化只發生一次，後續每次請求直接重用，省掉了 2~5 秒的冷啟動開銷。在 CC GUI 裡發訊息感覺很快，原因就在這。

Java 端（IDEA 外掛）和 daemon.js 之間的通訊走的是 **NDJSON 協定** （Newline-Delimited JSON），說白了就是透過子程序的 stdin/stdout 管道互發 JSON 訊息，每條訊息占一行。

Java 端發一條請求長這樣：

```
json 代碼解讀複製代碼{"id":"req-001","method":"claude.send","params":{"prompt":"幫我優化閱讀統計..."}}
```

daemon.js 處理完後，流式地往 stdout 寫回應：

```
json 代碼解讀複製代碼{"id":"req-001","line":"[CONTENT_DELTA] \"首先我來分析現有程式碼...\""}
{"id":"req-001","line":"[CONTENT_DELTA] \"建議改成 Redis 快取方案...\""}
```

每條回應都帶著請求 ID，這樣 Java 端就能準確地把回應路由到對應的對話視窗。CC GUI 支援平行發多個請求，靠的就是這個 ID 標記機制。

那 settings.json 又是怎麼回事？

CC GUI 會直接讀取 `~/.claude/settings.json` ，這個檔案就是 Claude Code CLI 的設定檔。也就是說，如果你之前在終端裡配好了 Claude Code（不管是官方帳號還是第三方 API），CC GUI 拿來就能用，不用重新配一遍。

這也解釋了為什麼在 CC GUI 的供應商設定裡點「新增」之後，它本質上就是幫你改寫 `settings.json` 。

設定是共享的，CLI 和外掛用的是同一份檔案。

MCP 和 Skills 也是同樣的道理。daemon.js 會從 settings.json 裡讀取 MCP 伺服器的配置，在執行請求的時候把可用的 MCP 工具列表傳給 SDK。所以在 CC GUI 裡配的 Chrome Devtools MCP，和在終端 Claude Code 裡用的是同一套。

[![](https://i.imgur.com/deAUOYy.jpeg)](https://i.imgur.com/deAUOYy.jpeg)

最後還有個小細節：daemon.js 每 15 秒會發一次心跳檢測，同時每 10 秒檢查一次父程序（也就是 IDEA）。如果 IDEA 被關掉了，daemon 會自動退出，不會變成殭屍程序占用資源。反過來，如果 daemon 意外崩了，Java 端會自動重啟它，最多重試 3 次。

## 04、ending

好工具的核心價值，就是把麻煩事變成順手的事。

Claude Code 和 Codex 本身都很能打，但它們原生跑在終端裡。

CC GUI 幹的事情，

就是把這種能力從命令列搬進了 IDE，

變成日常編碼流程裡自然而然的一環。

[![](https://i.imgur.com/bSpXsd3.jpeg)](https://i.imgur.com/bSpXsd3.jpeg)

【 **好工具是手的延伸，不是手的替代。** 】

CC GUI + Claude Code 的組合，是我目前用著最絲滑的 IntelliJ IDEA 開發後端的方式。

這裡必須補一句，Claude Code 做得很 open，支援官方授權方式，也支援 API key 的方式，這樣我們就可以配置各種模型，包括 GLM-5.1、Kimi 2.6、DeepSeek V4。

我們下期見～

---

原文出處： [https://juejin.cn/post/7657794899358973986](https://juejin.cn/post/7657794899358973986)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3DREjq) [49瀏覽](https://codelove.tw/@tony/post/3DREjq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3DREjq)

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

📝17 💬3 ❤️4

689

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

137

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783385960&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3DREjq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783385958072&amp;bpp=1&amp;bdt=260&amp;idt=2053&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783385939%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2180498155588&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395662%2C95390278%2C95393280%2C95393485&amp;oid=2&amp;pvsid=6891320560182192&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=2073" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>