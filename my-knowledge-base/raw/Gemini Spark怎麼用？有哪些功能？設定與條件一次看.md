---
title: "Gemini Spark怎麼用？有哪些功能？設定與條件一次看"
source: "https://www.bnext.com.tw/article/91668/gemini-spark-taiwan-launch-guide--google-gemini-spark-ai-agent-tutorial"
author:
  - "[[李先泰]]"
published: 2026-08-05
created: 2026-08-05
description: "Google個人AI代理 Gemini Spark 登台！本文整理 4 大使用門檻、背景自動執行機制、設定步驟與提示詞注入等安全風險，帶你一次掌握。"
tags:
  - "clippings"
status: processed
---
Google 台灣在 7 月 29 日的官方部落格公告，推出個人 AI 代理 [Gemini Spark](https://support.google.com/gemini/answer/17094507?hl=zh-Hant&co=GENIE.Platform=Android&sjid=15893578920318965848-NC#zippy=,%E6%9F%A5%E7%9C%8B%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E7%9A%84%E9%80%B2%E5%BA%A6%E5%92%8C%E5%B7%B2%E5%AE%8C%E6%88%90%E7%9A%84%E6%AD%A5%E9%A9%9F,%E6%9F%A5%E7%9C%8B-gemini-%E8%AE%80%E5%8F%96%E6%88%96%E6%9B%B4%E6%96%B0%E7%9A%84%E6%AA%94%E6%A1%88,%E6%9A%AB%E5%81%9C%E5%8F%8A%E7%B9%BC%E7%BA%8C%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%8E%92%E7%A8%8B,%E7%9B%A3%E6%8E%A7%E5%8F%8A%E6%8E%A5%E6%89%8B%E4%BB%BB%E5%8B%99,%E5%88%AA%E9%99%A4%E9%81%A0%E7%AB%AF%E7%80%8F%E8%A6%BD%E5%99%A8%E8%B3%87%E6%96%99,%E5%88%AA%E9%99%A4%E9%81%A0%E7%AB%AF%E7%A8%8B%E5%BC%8F%E7%A2%BC%E5%9F%B7%E8%A1%8C%E8%B3%87%E6%96%99,%E6%8A%80%E8%83%BD%E5%92%8C%E6%8E%92%E7%A8%8B,%E9%80%A3%E7%B5%90%E7%9A%84%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E5%92%8C%E5%85%B6%E4%BB%96-google-%E6%9C%8D%E5%8B%99,%E5%80%8B%E4%BA%BA%E5%8C%96%E6%99%BA%E6%85%A7%E6%9C%8D%E5%8B%99,%E9%81%A0%E7%AB%AF%E7%80%8F%E8%A6%BD%E5%99%A8%E5%92%8C%E9%9B%BB%E8%85%A6,%E5%85%B6%E4%BB%96-gemini-%E5%8A%9F%E8%83%BD,%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86%E5%BC%8F-ai-%E7%9A%84%E6%BD%9B%E5%9C%A8%E9%A2%A8%E9%9A%AA,spark-%E5%A6%82%E4%BD%95%E5%8D%94%E5%8A%A9%E7%A2%BA%E4%BF%9D%E5%80%8B%E4%BA%BA%E8%B3%87%E8%A8%8A%E5%AE%89%E5%85%A8%E7%84%A1%E8%99%9E,%E7%9B%A3%E7%9D%A3%E7%9A%84%E9%87%8D%E8%A6%81%E6%80%A7%E4%BB%A5%E5%8F%8A%E5%81%9C%E6%AD%A2-gemini-%E6%93%8D%E4%BD%9C%E7%9A%84%E6%96%B9%E5%BC%8F,%E4%BB%BB%E5%8B%99%E6%8E%92%E7%A8%8B%E8%88%87%E6%8A%80%E8%83%BD%E6%9C%89%E4%BD%95%E4%B8%8D%E5%90%8C,%E5%A6%82%E6%9E%9C%E9%99%8D%E7%B4%9A%E6%88%96%E5%8F%96%E6%B6%88%E8%A8%82%E9%96%B1-google-ai-ultra%E6%9C%83%E7%99%BC%E7%94%9F%E4%BB%80%E9%BA%BC%E6%83%85%E6%B3%81,gemini-spark-%E5%8F%AF%E4%BB%A5%E5%9F%B7%E8%A1%8C%E5%93%AA%E4%BA%9B-workspace-%E5%8B%95%E4%BD%9C,%E7%9B%B4%E6%8E%A5%E5%9C%A8%E4%BB%BB%E5%8B%99%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E8%BC%B8%E5%85%A5%E8%B3%87%E8%A8%8A%E6%99%82%E8%AB%8B%E5%8B%99%E5%BF%85%E5%B0%8F%E5%BF%83%E8%AC%B9%E6%85%8E,%E9%81%BF%E5%85%8D%E5%AE%89%E6%8E%92%E6%B6%89%E5%8F%8A%E6%95%8F%E6%84%9F%E8%B3%87%E8%A8%8A%E7%9A%84%E4%BB%BB%E5%8B%99) ，並表示將在該公告後的未來幾週內，陸續開放給台灣的 Google AI Pro 訂閱用戶，繁體中文與英文都能使用，背後由 Gemini 3.6 Flash 驅動。

它跟你現在用的 Gemini 最大的差別，是它會留在背景繼續工作。官方的說法是， **就算你闔上筆記型電腦、鎖定手機螢幕，Spark 也照樣運作。**

## 功能是什麼？從「回答問題」變成「替你把事做完」

過去你問 Gemini 一件事，它回答一次，對話就結束。Spark 改變的是這個模式： **你給它一個目標和一個時間，它自己去執行，中途回報進度，需要你出手時才叫你。** 官方舉的入門用途包括整理收件匣（總結或封存電子報、取消訂閱郵件）、產出自訂新聞摘要，以及彙整研究資料並附上完整引用來源。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

操作邏輯由三個元件組成，官方把它們對應成「要做什麼」、「何時執行」、「如何達成」。

> - **任務** 是核心目標，例如「規劃及管理我的倫敦出差行程」。
> - **排程** 決定何時觸發，可以是固定時間，也可以是事件發生的當下，例如「每天上午 8 點，提供最新的 AI 新聞」或「如果我的航班延誤，請通知我並規劃新的行程」。
> - **技能** 是一組可重複使用的指令，教它用什麼工具、按什麼方式做事，例如同時使用「旅遊預訂」與「Gmail 撰寫」兩項技能，重新訂房並寄出確認信。

如果你想知道 Spark 可以做什麼、怎麼做？我們有教學文章 [〈Gemini Spark是什麼？幫你讀Gmail、改文件、自動跑完工作流程，3個實測情境一次看〉](https://support.google.com/gemini/answer/17094507?hl=zh-Hant&co=GENIE.Platform=Android&sjid=15893578920318965848-NC#zippy=,%E6%9F%A5%E7%9C%8B%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E7%9A%84%E9%80%B2%E5%BA%A6%E5%92%8C%E5%B7%B2%E5%AE%8C%E6%88%90%E7%9A%84%E6%AD%A5%E9%A9%9F,%E6%9F%A5%E7%9C%8B-gemini-%E8%AE%80%E5%8F%96%E6%88%96%E6%9B%B4%E6%96%B0%E7%9A%84%E6%AA%94%E6%A1%88,%E6%9A%AB%E5%81%9C%E5%8F%8A%E7%B9%BC%E7%BA%8C%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%8E%92%E7%A8%8B,%E7%9B%A3%E6%8E%A7%E5%8F%8A%E6%8E%A5%E6%89%8B%E4%BB%BB%E5%8B%99,%E5%88%AA%E9%99%A4%E9%81%A0%E7%AB%AF%E7%80%8F%E8%A6%BD%E5%99%A8%E8%B3%87%E6%96%99,%E5%88%AA%E9%99%A4%E9%81%A0%E7%AB%AF%E7%A8%8B%E5%BC%8F%E7%A2%BC%E5%9F%B7%E8%A1%8C%E8%B3%87%E6%96%99,%E6%8A%80%E8%83%BD%E5%92%8C%E6%8E%92%E7%A8%8B,%E9%80%A3%E7%B5%90%E7%9A%84%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E5%92%8C%E5%85%B6%E4%BB%96-google-%E6%9C%8D%E5%8B%99,%E5%80%8B%E4%BA%BA%E5%8C%96%E6%99%BA%E6%85%A7%E6%9C%8D%E5%8B%99,%E9%81%A0%E7%AB%AF%E7%80%8F%E8%A6%BD%E5%99%A8%E5%92%8C%E9%9B%BB%E8%85%A6,%E5%85%B6%E4%BB%96-gemini-%E5%8A%9F%E8%83%BD,%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86%E5%BC%8F-ai-%E7%9A%84%E6%BD%9B%E5%9C%A8%E9%A2%A8%E9%9A%AA,spark-%E5%A6%82%E4%BD%95%E5%8D%94%E5%8A%A9%E7%A2%BA%E4%BF%9D%E5%80%8B%E4%BA%BA%E8%B3%87%E8%A8%8A%E5%AE%89%E5%85%A8%E7%84%A1%E8%99%9E,%E7%9B%A3%E7%9D%A3%E7%9A%84%E9%87%8D%E8%A6%81%E6%80%A7%E4%BB%A5%E5%8F%8A%E5%81%9C%E6%AD%A2-gemini-%E6%93%8D%E4%BD%9C%E7%9A%84%E6%96%B9%E5%BC%8F,%E4%BB%BB%E5%8B%99%E6%8E%92%E7%A8%8B%E8%88%87%E6%8A%80%E8%83%BD%E6%9C%89%E4%BD%95%E4%B8%8D%E5%90%8C,%E5%A6%82%E6%9E%9C%E9%99%8D%E7%B4%9A%E6%88%96%E5%8F%96%E6%B6%88%E8%A8%82%E9%96%B1-google-ai-ultra%E6%9C%83%E7%99%BC%E7%94%9F%E4%BB%80%E9%BA%BC%E6%83%85%E6%B3%81,gemini-spark-%E5%8F%AF%E4%BB%A5%E5%9F%B7%E8%A1%8C%E5%93%AA%E4%BA%9B-workspace-%E5%8B%95%E4%BD%9C,%E7%9B%B4%E6%8E%A5%E5%9C%A8%E4%BB%BB%E5%8B%99%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E8%BC%B8%E5%85%A5%E8%B3%87%E8%A8%8A%E6%99%82%E8%AB%8B%E5%8B%99%E5%BF%85%E5%B0%8F%E5%BF%83%E8%AC%B9%E6%85%8E,%E9%81%BF%E5%85%8D%E5%AE%89%E6%8E%92%E6%B6%89%E5%8F%8A%E6%95%8F%E6%84%9F%E8%B3%87%E8%A8%8A%E7%9A%84%E4%BB%BB%E5%8B%99)

官方的總結是，定義任務、設定排程、提供技能，三件事湊齊就能運作。

這裡有個容易搞混的地方：Spark 的「排程」跟你可能已經用過的 Gemini 對話「排定動作」是兩套功能，官方特別出來提醒。你之前設過每天早上的摘要通知，不代表你已經在用 Spark。

## 它能碰到你哪些資料？

依官方說明，Spark 可動用的來源包括 Google Workspace 的八項服務（日曆、文件、雲端硬碟、Gmail、Keep、試算表、簡報、Tasks）、Google 搜尋與財經、航班／機票、飯店、地圖等搜尋服務、YouTube，以及 Canva、Dropbox、Instacart、OpenTable、Zillow 五個第三方應用程式。

它也會讀取你與 Gemini 的對話記錄和你給過的指令，並在需要時使用遠端瀏覽器實際操作網頁、用遠端電腦執行程式碼。

換句話說，它的權限幾乎覆蓋你日常會用到的 Google 服務。官方在清單裡特別標明，如果要編輯 **共用的** 文件、試算表和簡報，你必須先查看並確認它預計進行的編輯。

## 誰可以用？四個條件缺一不可

依 Gemini 說明中心的「需求條件」，要用 Spark 必須同時滿足四項： **年滿 18 歲** ； **以個人 Google 帳戶登入** ，官方明寫現階段不支援公司或學校專用帳戶； **訂閱 Google AI 付費方案** ； **開啟「保留活動記錄」設定** 。最後一條是個要自己權衡的取捨，想用 Spark，就得讓 Google 保留你的 Gemini 活動紀錄。

至於台灣什麼時候真的能用，Google台灣部落格指出 Google AI Pro 訂閱用戶要等到公告後的未來幾週內才會陸續取得，實務上只能以你的帳號裡是否已經出現 Spark 入口為準。

## 怎麼用？三步建立任務，進度與接手都在「工作面板」

按照 [Google官方文件](https://support.google.com/gemini/answer/17094507?hl=zh-Hant&co=GENIE.Platform=Android&sjid=15893578920318965848-NC#zippy=,%E6%9F%A5%E7%9C%8B%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E7%9A%84%E9%80%B2%E5%BA%A6%E5%92%8C%E5%B7%B2%E5%AE%8C%E6%88%90%E7%9A%84%E6%AD%A5%E9%A9%9F,%E6%9F%A5%E7%9C%8B-gemini-%E8%AE%80%E5%8F%96%E6%88%96%E6%9B%B4%E6%96%B0%E7%9A%84%E6%AA%94%E6%A1%88,%E6%9A%AB%E5%81%9C%E5%8F%8A%E7%B9%BC%E7%BA%8C%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%8E%92%E7%A8%8B,%E7%9B%A3%E6%8E%A7%E5%8F%8A%E6%8E%A5%E6%89%8B%E4%BB%BB%E5%8B%99,%E5%88%AA%E9%99%A4%E9%81%A0%E7%AB%AF%E7%80%8F%E8%A6%BD%E5%99%A8%E8%B3%87%E6%96%99,%E5%88%AA%E9%99%A4%E9%81%A0%E7%AB%AF%E7%A8%8B%E5%BC%8F%E7%A2%BC%E5%9F%B7%E8%A1%8C%E8%B3%87%E6%96%99,%E6%8A%80%E8%83%BD%E5%92%8C%E6%8E%92%E7%A8%8B,%E9%80%A3%E7%B5%90%E7%9A%84%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E5%92%8C%E5%85%B6%E4%BB%96-google-%E6%9C%8D%E5%8B%99,%E5%80%8B%E4%BA%BA%E5%8C%96%E6%99%BA%E6%85%A7%E6%9C%8D%E5%8B%99,%E9%81%A0%E7%AB%AF%E7%80%8F%E8%A6%BD%E5%99%A8%E5%92%8C%E9%9B%BB%E8%85%A6,%E5%85%B6%E4%BB%96-gemini-%E5%8A%9F%E8%83%BD,%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86%E5%BC%8F-ai-%E7%9A%84%E6%BD%9B%E5%9C%A8%E9%A2%A8%E9%9A%AA,spark-%E5%A6%82%E4%BD%95%E5%8D%94%E5%8A%A9%E7%A2%BA%E4%BF%9D%E5%80%8B%E4%BA%BA%E8%B3%87%E8%A8%8A%E5%AE%89%E5%85%A8%E7%84%A1%E8%99%9E,%E7%9B%A3%E7%9D%A3%E7%9A%84%E9%87%8D%E8%A6%81%E6%80%A7%E4%BB%A5%E5%8F%8A%E5%81%9C%E6%AD%A2-gemini-%E6%93%8D%E4%BD%9C%E7%9A%84%E6%96%B9%E5%BC%8F,%E4%BB%BB%E5%8B%99%E6%8E%92%E7%A8%8B%E8%88%87%E6%8A%80%E8%83%BD%E6%9C%89%E4%BD%95%E4%B8%8D%E5%90%8C,%E5%A6%82%E6%9E%9C%E9%99%8D%E7%B4%9A%E6%88%96%E5%8F%96%E6%B6%88%E8%A8%82%E9%96%B1-google-ai-ultra%E6%9C%83%E7%99%BC%E7%94%9F%E4%BB%80%E9%BA%BC%E6%83%85%E6%B3%81,gemini-spark-%E5%8F%AF%E4%BB%A5%E5%9F%B7%E8%A1%8C%E5%93%AA%E4%BA%9B-workspace-%E5%8B%95%E4%BD%9C,%E7%9B%B4%E6%8E%A5%E5%9C%A8%E4%BB%BB%E5%8B%99%E5%B0%8D%E8%A9%B1%E4%B8%B2%E4%B8%AD%E8%BC%B8%E5%85%A5%E8%B3%87%E8%A8%8A%E6%99%82%E8%AB%8B%E5%8B%99%E5%BF%85%E5%B0%8F%E5%BF%83%E8%AC%B9%E6%85%8E,%E9%81%BF%E5%85%8D%E5%AE%89%E6%8E%92%E6%B6%89%E5%8F%8A%E6%95%8F%E6%84%9F%E8%B3%87%E8%A8%8A%E7%9A%84%E4%BB%BB%E5%8B%99) ，建立任務的路徑是：開啟 Gemini 行動應用程式或前往 [gemini.google.com](https://gemini.google.com/app) ，點頂端「選單」再點「Spark」，然後在輸入欄位描述你要它管理的任務。

這一步可以疊加三件事：

> 1. 指定執行時間、輸入斜線 `/` 挑選既有技能、要求它使用你自訂的連結應用程式
> 2. 需要附檔就點「上傳和工具」，可上傳檔案、加入雲端硬碟檔案或新增筆記本，最後按「提交」。
> 3. 完全不知道從哪開始的人，官方給的辦法是直接在 Spark 任務裡輸入「設定」、「開始」或「問我問題」，讓它反過來問你。

送出後要定期回來看進度。查看入口是「工作面板」，點「選單」、「Spark」、「任務」，選擇任務對話串，再點頂端的進度方塊即可展開，行動版分為「進度」、「排程」、「檔案」、「技能和應用程式」四個專區，可以看已完成的步驟、它讀取或更新過哪些檔案，也能暫停或繼續排程。

部分任務是在遠端瀏覽器裡完成的，例如前往網站、把商品加入購物車。你隨時可以從工作面板打開遠端瀏覽器畫面、點「接手任務」自己操作，做完再點「返回 Gemini」交還控制權；它遇到需要你出手的動作時，也會主動請你接手。

用量有兩道限制： **一次最多同時執行 15 項工作** ，超過要等，而且已有 15 項在跑時排程也不會啟動；整體用量以運算量計算，方案等級越高上限越高，官方未公布具體次數，只說接近上限會通知你。

不想用了可以到「設定與說明」的「Gemini Spark 設定」關閉，關閉後遠端瀏覽器與程式碼執行資料會被刪除，排程與任務會暫停但不會刪除，重新開啟會自動接續。

> 延伸閱讀： [台達電憑什麼暴衝市值三雄？5張圖拆解：最被低估的AI黑馬，如何吃下「黃仁勳蛋糕」最難2層？](https://www.bnext.com.tw/article/91638/delta-tw-ai)

資料來源： [Google 台灣官方部落格](https://blog.google/intl/zh-tw/products/devices-services/gemini-spark-ai/) 、 [Gemini 說明中心](https://support.google.com/gemini/answer/17094507?hl=zh-Hant&co=GENIE.Platform%3DAndroid) 、 [Gemini Spark 排程說明](https://support.google.com/gemini/answer/17094710?hl=en) 、 [《Android Police》](https://www.androidpolice.com/googles-always-on-ai-agent-expanding-but-wont-come-cheap/) 、 [《ChromeUnboxed》](https://chromeunboxed.com/googles-agentic-ai-gemini-spark-is-now-available-on-the-20-pro-tier-in-the-us/)

本文初稿為AI編撰，整理．編輯/ 李先泰