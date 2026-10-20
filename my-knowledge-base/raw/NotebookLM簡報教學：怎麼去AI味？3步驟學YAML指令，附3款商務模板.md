---
title: "NotebookLM簡報教學：怎麼去AI味？3步驟學YAML指令，附3款商務模板"
source: "https://www.bnext.com.tw/article/91085/ai-presentation-yaml-style-guide"
author:
  - "[[羅秀如]]"
published: 2026-05-29
created: 2026-06-01
description: "做簡報最浪費時間的是一遍遍調整 AI 產出的版面，YAML 語法能把視覺需求寫成 AI 看得懂的參數，一次設定配色、字體、排版，省掉每次重調的麻煩。"
tags:
  - "clippings"
status: processed
---
2026.05.29 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

## NotebookLM簡報怎麼去AI味？3步驟學會YAML指令，附3款商務模板「複製就能用」

做簡報最浪費時間的是一遍遍調整 AI 產出的版面，YAML 語法能把視覺需求寫成 AI 看得懂的參數，一次設定配色、字體、排版，省掉每次重調的麻煩。

[＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃NotebookLM](https://www.bnext.com.tw/tags/NotebookLM) [＃提示詞（prompt）](https://www.bnext.com.tw/tags/%E6%8F%90%E7%A4%BA%E8%A9%9E%EF%BC%88prompt%EF%BC%89)

---

使用 AI 生成簡報雖然快速，但在調整視覺風格時，往往很難精準控制細節。當你輸入「現代感」、「專業風」這類主觀的形容詞時，AI 因為無法理解抽象的概念，通常只能從內建的主題中，挑選出最接近大眾刻板印象的元素來填滿版面，導致產出的版面可能與你的預期有落差，或者無法完美對齊公司的品牌標準色（CI）。

要解決這種溝通落差，解法是換一種語法跟 AI 對話。透過結構化的 YAML 語法，直接將字體、配色與排版底線寫成具體的參數，讓 AI 有一套明確的視覺規範可以依循。

## 什麼是 YAML？先把它想成「給 AI 的視覺點菜單」

[YAML（YAML Ain't Markup Language）](https://www.bnext.com.tw/article/85609/ppt-yaml-notebooklm-style) 是一種純文字語法，不需要工程師背景，只要記住兩件事：  
1\. 項目與內容之間，用冒號加一個空格隔開。  
2\. 留意前方的空格（縮排）：新手建議直接複製下方的完整模板，不要隨意刪動每行前面的空格，確保 AI 能正確讀取階層。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

\`\`  
  
複製器

```
color: "#002060" # 冒號後面必須空一格，系統才能讀取
font: "Arial, bold"
```

\`\`

左邊是參數名稱，右邊是設定值，# 之後可以加中文備註作為給 AI 的提示。把顏色、字體、排版限制都寫進這份點菜單，AI 就會照單執行。

這套方法不只適用於文字工具，只要能輸入文字指令（Prompt）的 AI 設計工具都能通用：

- NotebookLM (做為企劃大腦)：在對話框中與簡報大綱一起輸入，強制 AI 在規劃每頁內容時，同步幫你寫好這頁簡報要配什麼顏色、用什麼字體與版面規範。
- Gamma 等 AI 簡報工具 (做為視覺輸出)：在工具的「自訂主題 (Custom Themes)」或進階指令欄中貼上這些參數，就能一鍵幫所有生成的簡報直接「換裝」成你的企業標準色。
- ChatGPT Plus / Claude：用來設定數據圖表的配色與視覺邊界。

我們拿 NotebookLM 來示範，看看這份視覺點菜單到底怎麼寫：

### 步驟 1. 先認識菜單結構，這 4 個欄位控制了一切

一份完整的 YAML 點菜單由 4 個參數組成，各自控制不同的視覺面向：

**color\_palette: # 控制配色  
typography: # 控制字體  
design\_system: # 控制視覺風格與插圖類型  
output\_constraints: # 設下禁止項目，防止 AI 過度設計**

### 步驟 2. 逐一設定 4 個參數

**1\. 配色（color\_palette）：用色碼取代顏色名稱**

你跟 AI 說「深藍色」，但它產出的結果往往有色差。要讓 AI 顯出正確的顏色，必須提供十六進位色碼（Hex Code）。可以用瀏覽器外掛吸取公司官網的標準色，得到如 #003366 的字串。

配色

**2\. 字體（typography）：分別指定標題與內文**

字體直接影響簡報的語氣，像 Sans-serif（無襯線體）筆畫俐落，適合數字與商務報告；Serif（襯線體）筆畫末端有裝飾，適合傳遞故事與品牌溫度。

字體

**3\. 風格（design\_system）：給 AI 一個具體場景描述**

要求 AI 做出「科技感」，它會無從判斷，最好直接給一個具體的場景描述，它才有依據可循。

風格

**4\. 禁止清單（output\_constraints）：把不要的元素明文寫出來**

AI 有往版面塞東西的傾向，像是加霓虹光、堆滿元素、套賽博龐克風格，你可以用「禁止清單」提前畫出紅線。

### 步驟 3. 把點菜單跟大綱一起貼給 AI

將你事先準備好（或讓 AI 幫你梳理好）的簡報大綱，連同 YAML 語法一起貼給 AI，並加上這段提示詞：

**「你現在是一位專業的簡報設計師。請嚴格按照我提供的簡報大綱內容，並全程套用上方的 YAML 風格規範來生成視覺化投影片草稿。請保持結構一致性，禁止自行添加外部資料與多餘的裝飾。」**

送出指令後，像 NotebookLM 這類文字 AI 並不會直接吐出一個「.pptx 檔案」給你下載。它收到你的 YAML 點菜單後，會幫你把每一頁要用的顏色、字體、排版規則都列得清清楚楚。

接下來，你只要把這些規則複製下來，貼給真正負責圖像生成的工具（例如 Gamma、Canva，或是自己打開 PowerPoint 照著做），AI 就會乖乖照著規定，精準產出你要的排版風格。

## 3 款商務風格 prompt 指令，複製貼上就能用

不想從零填參數？以下示範的 3 種 YAML 模板，取自 [《AI 簡報極速工作流》](https://portaly.cc/managertoday/product/3uIHatNl8kWOHw1wkFEl?utm_source=web_bn&utm_medium=article&utm_campaign=ai_presentation&utm_content=article_91085) 線上課程指令集，對應 3 種不同的簡報情境，找到符合需求的版本，複製貼上就能用。

**2.5D 立體風**

![#0 教學｜NotebookLM 簡報怎麼去除 AI 味？3 步驟寫好 YAML 指令，配色、字體、](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/42w7-1779941352.jpg?w=1200&output=webp)

圖／ 經理人

![#1 教學｜NotebookLM 簡報怎麼去除 AI 味？3 步驟寫好 YAML 指令，配色、字體、](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/9tha-1779941352.jpg?w=1200&output=webp)

圖／ 經理人

適用情境：系統流程、物流網格、智慧城市架構  
整張簡報從固定的斜角俯視，所有元素像積木一樣整齊排列。適合用來呈現有層次、有流向的系統架構，讓觀眾一眼看出各部件之間的關係。

**高階顧問風**

![#3 教學｜NotebookLM 簡報怎麼去除 AI 味？3 步驟寫好 YAML 指令，配色、字體、](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/z4f3-1779941353.jpg?w=1200&output=webp)

圖／ 經理人

![#4 教學｜NotebookLM 簡報怎麼去除 AI 味？3 步驟寫好 YAML 指令，配色、字體、](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/5zce-1779941354.jpg?w=1200&output=webp)

圖／ 經理人

適用情境：商業決策、高階主管報告  
版面乾淨，沒有多餘的裝飾，每一頁只說一件事。結構遵循金字塔原理，先結論後細節，適合需要快速傳遞決策依據的場合。

**軟體服務風**

![#5 教學｜NotebookLM 簡報怎麼去除 AI 味？3 步驟寫好 YAML 指令，配色、字體、](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/me7j-1779941354.jpg?w=1200&output=webp)

圖／ 經理人

![#6 教學｜NotebookLM 簡報怎麼去除 AI 味？3 步驟寫好 YAML 指令，配色、字體、](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/jv9p-1779941354.jpg?w=1200&output=webp)

圖／ 經理人

適用情境：產品介紹、功能展示、系統流程  
版面像一個整理好的 App 介面，每個區塊獨立放在白色卡片裡，層次清楚、不擁擠。適合展示產品功能或操作流程，讓複雜的內容看起來簡單好懂。

## 學會完整 AI 工作流，做簡報才能真正省時

不過，YAML 語法只幫你解決了最後的「視覺排版」環節。許多人在動手設計前，其實卡在更上游的階段：面對空白簡報不知道怎麼開場、手上的大堆數據理不出邏輯，或是每次都要從頭摸索架構。

真正有效率的工作流，是讓 AI 負責 80% 收集資料、梳理大綱與建立排版架構等工作；而你設定好專屬的 YAML 風格參數後，就能把寶貴的精力，留給剩下 20% 的核心觀點修改與決策。

[![拒絕一鍵生成的廉價感！用 AI 省下 70% 製作時間，打造有說服力的商用簡報](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-02/idy9-1770703973.png?w=1200&output=webp)](https://portaly.cc/managertoday/product/3uIHatNl8kWOHw1wkFEl?utm_source=web_mt&utm_medium=article&utm_campaign=ai_presentation&utm_content=article_72247)

圖／ 經理人

[《AI 簡報極速工作流》](https://portaly.cc/managertoday/product/3uIHatNl8kWOHw1wkFEl?utm_source=web_bn&utm_medium=article&utm_campaign=ai_presentation&utm_content=article_91085) 線上課程給你一套可以重複使用的指揮邏輯，涵蓋發想、架構、視覺到優化的完整流程，每個環節都有對應的 Prompt 方法。課程附贈「顧問級 Prompt 型錄」，收錄近 50 組商用模板，色碼、字體、禁止項目全部設定好，直接複製貼上就能立即使用，讓你做簡報時一次就能產出符合預期的專業質感。

> 本文授權轉載自 [經理人月刊](https://www.managertoday.com.tw/articles/view/72247)

[![](https://cdn.bnextmedia.com.tw/img/16x9.png "《AI TAIWAN 未來商務展》直播論壇 ——《AI 轉型實戰-從工具導入到商業變現： 用工具提效，還是打造產品變現？》")](https://eventgo.bnextmedia.com.tw/event/detail/2111bd9dfe8302d08cb2ac17d52313cf?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "《AI TAIWAN 未來商務展》直播論壇 ——《AI 轉型實戰-從工具導入到商業變現： 用工具提效，還是打造產品變現？》")

[《AI TAIWAN 未來商務展》直播論壇 ——《AI 轉型實戰-從工具導入到商業變現： 用工具提效，還是打造產品變現？》](https://eventgo.bnextmedia.com.tw/event/detail/2111bd9dfe8302d08cb2ac17d52313cf?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "《AI TAIWAN 未來商務展》直播論壇 ——《AI 轉型實戰-從工具導入到商業變現： 用工具提效，還是打造產品變現？》")

[

活動詳情

](https://eventgo.bnextmedia.com.tw/event/detail/2111bd9dfe8302d08cb2ac17d52313cf?utm_source=web_bn&utm_medium=aa_event_er&utm_campaign=stage_1&utm_term=model_5 "《AI TAIWAN 未來商務展》直播論壇 ——《AI 轉型實戰-從工具導入到商業變現： 用工具提效，還是打造產品變現？》")

關鍵字： [＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃NotebookLM](https://www.bnext.com.tw/tags/NotebookLM) [＃提示詞（prompt）](https://www.bnext.com.tw/tags/%E6%8F%90%E7%A4%BA%E8%A9%9E%EF%BC%88prompt%EF%BC%89)

tw\_bnext \[Dynamic Article\]-20260529-08:03

arrow\_forward\_ios

閱讀文章

<video title="Advertisement" src="https://rr4---sn-ipoxu-un5ed.googlevideo.com/videoplayback?expire=1780319290&amp;ei=uhMdapKVHYubvcAPl5eVqAc&amp;ip=60.250.84.232&amp;id=8a803431b699a646&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=140&amp;met=1780290490,&amp;mh=MC&amp;mm=31&amp;mn=sn-ipoxu-un5ed&amp;ms=au&amp;mv=m&amp;mvi=4&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=15.162&amp;lmt=1724558211293492&amp;mt=1780290098&amp;cpn=XhCiSdD3nE79ufpn&amp;txp=5432434&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AHEqNM4wRQIhAPAiqz8J8okpHwjoeUiYvPf0bk3XKhFHu2o1KqAdlqQkAiAd8KVXQ5PT0bfAti2GHMmx2qnwzK5w7RV87beV4Wih2Q==&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRAIgTRWO_pZL37L56VmvZiyeGiecQfjssfaLnoSfLieOAZQCIGNrlEk7xhAzhs5gJNqrimqcmGPE0TFaC88c_x3r1__m" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.766.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1944062303%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C745150931%2Cnull%2C%5Bnull%2C1%5D%5D%2C%5Bnull%2C749060184%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5B841585769%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45774999%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45765927%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45661356%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C500%5D%5D%2C%5Bnull%2C45767902%2Cnull%2C%5Bnull%2C500%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761216%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B31097690%5D%2C%5B31097691%2C%5B%5B846355750%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95345206%5D%2C%5B95345207%2C%5B%5B45661356%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95356068%5D%2C%5B95356069%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5B%5D%5D%5D%5D%2C%5B95356070%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5B%5D%5D%5D%5D%2C%5B95356071%2C%5B%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95382403%2C%5B%5B45794243%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95386528%2C%5B%5Bnull%2C45767902%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B100%2C%5B%5B95387816%5D%2C%5B95387817%2C%5B%5B45774999%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95390088%5D%2C%5B95390089%2C%5B%5B45784857%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391694%5D%2C%5B95391695%2C%5B%5B45761216%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5Bnull%2C%5B%5B95391980%5D%2C%5B95391981%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fpagead%2Fmanaged%2Fjs%2Factiveview%2Fcurrent%2Frx_omid_video.js%5C%22%5D%5D%5D%5D%2C%5B95391982%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fomsdk%2Freleases%2Flive%2Fomid_monitoring_bin.js%5C%22%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=1128144102542434&amp;pvsid=829092487198225&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F91085%2Fai-presentation-yaml-style-guide%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="168.75"></iframe>

00:00

00:03

00:44

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/bnext.com.tw/bnext.com.tw-1780041830.350766.mp4?verify=1780289820-Zjjo4t12TGye%2FG9S7wOaM5sd0MUkywlSpXVNViZyJKw%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/bnext.com.tw/bnext.com.tw-1780041830.350766.mp4?token=zAOnocy8CwIZ6A5G-jYKzl9vvWoemz6fg0YIqp9vEUg&amp;expires=1780376220"></video>

即時熱門文章

[1 NotebookLM簡報怎麼去AI味？3步驟學會YAML指令，附3款商務模板「複製就能用」](https://www.bnext.com.tw/article/91085/ai-presentation-yaml-style-guide) [2 「5年後還在不在」才是客戶在乎的！天能綠電手握341億度電，日月光、美光都是它的客戶](https://www.bnext.com.tw/article/91077/greenet-on-time-delivery-winner) [3 17組萬用提示詞快收藏！從寫長文到管時間，丟給Claude、ChatGPT、Gemini都能用](https://www.bnext.com.tw/article/91079/17-essential-ai-prompts) [4 Claude入門學習地圖！38篇教學、8大主題一次收，Cowork、Skills、省Token秘訣全打包](https://www.bnext.com.tw/article/91080/claude-cowork-skills) [5 「若航空這麼好賺，黃仁勳也能來開」星宇還在燒錢長大，張國煒：想舒服過早就當公子哥，何必來扛](https://www.bnext.com.tw/article/91082/2026-starlux-airlines-shareholders-meeting) [6 黃仁勳兆元宴「行走版0050」齊聚！出席企業市值飆上91.17兆元，AI一年灌進54兆元](https://www.bnext.com.tw/article/91093/nvidia-jensen-huang-taiwan-ai-supply-chain)