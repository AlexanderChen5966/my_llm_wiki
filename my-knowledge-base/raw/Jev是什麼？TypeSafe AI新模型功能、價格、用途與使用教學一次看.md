---
title: "Jev是什麼？TypeSafe AI新模型功能、價格、用途與使用教學一次看"
source: "https://www.bnext.com.tw/article/92319/typesafe-jev-decision-model-explainer"
author:
  - "[[李先泰]]"
published: 2026-09-21
created: 2026-09-22
description: "TypeSafe AI推出AI決策模型Jev，不生成文字，專門負責分類、評分與機率判斷。速度最快提升200倍、輸出免費，本文整理功能、用途與上手教學。"
tags:
  - "clippings"
status: processed
---
[![](https://pk-img.learnin.tw/prod/photos/2026-09/img-1788712566-41018.gif)](https://meettaipei.tw/exhibitor/?utm_source=web_bn&utm_medium=logo_banner&utm_campaign=2026mtpe&utm_content=173552&utm_term=channel_all)

2026.09.21 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

## 不能生成文字卻暴紅！ChatGPT技術推手打造AI決策模型Jev，為何讓開發者搶到當機？

TypeSafe AI推出AI決策模型Jev，不生成文字，專門負責分類、評分與機率判斷。速度最快提升200倍、輸出免費，本文整理功能、用途與上手教學。

---

9月15日，結束兩年隱身的舊金山新創TypeSafe AI，正式發表首款AI模型Jev並開放早期試用。沒想到，湧入的開發者瞬間塞爆系統，甚至讓官方API一度撐不住而斷線。

有趣的是，這款讓開發者搶到當機的AI，竟然「不會陪你聊天」。不同於ChatGPT、Claude等大型語言模型（LLM），Jev完全捨棄文字生成功能，當你輸入一段內容與問題，它只會給出三種明確答案：選哪一個、打幾分、是或不是，並附上每個決策的機率。

TypeSafe AI的來頭並不簡單，創辦人Diogo Almeida不僅是OpenAI前研究員，也是InstructGPT論文共同作者之一，參與開發後來催生ChatGPT的關鍵技術RLHF（人類回饋強化學習）。這不禁令人好奇，身為讓AI學會「說人話」的關鍵推手，Almeida為何不加入當今殺紅眼的大型語言模型戰局，反而回頭打造一款「不寫字」的AI？

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## Jev是什麼？TypeSafe AI打造「不生成文字」的System One模型

簡單來說，Jev不是拿來聊天或寫文章的生成式AI，而是一款專門替軟體「做判斷」的AI模型。TypeSafe AI將這類模型稱為「System One Model」，概念取自心理學家Daniel Kahneman提出的System 1：快速、直覺、不需要長時間推理的思考模式。

Jev的運作方式也因此和ChatGPT不同。使用者輸入一段資料後，不會得到一段文字回答，而是直接取得程式可以使用的結構化判斷。目前Jev主要提供三種回答方式：

> **Choice（選擇）：** 從預先設定的選項中挑出最適合的一個。  
> **Score（評分）：** 按照指定標準替內容評分。  
> **Noul（機率判斷）：** 判斷一項敘述成立的機率。

例如收到一封客訴信，ChatGPT可能會分析問題並寫出一段建議；Jev則可以直接判斷「該交給哪個部門」、「退款需求有多強」、「事情有多緊急」，讓程式直接根據結果執行下一步。

## Jev和ChatGPT有何不同？軟體要的是「數值」，不是一段話

Almeida的想法是，業界在人類語言上已經強了四年，自動化卻沒跟上，因為「電腦講的是另一種語言」。程式需要的是能直接拿來分支、排序、路由的值；LLM給的卻是一段還要再解析的文字，中間還可能夾著它自己編出來的東西。

因此Jev跳過寫字這一步。依TypeSafe開發文件，你送進去的是「狀態」加「問題」：狀態可以是客服信、程式碼、AI Agent操作紀錄等非結構化內容，再交由Jev進行選擇（Choice）、評分（Score）或機率判斷（Noul）。

而且一次可以交給Jev判斷多個問題，各自平行評估，多問幾題幾乎不增加等待時間。

AI開發者Sam Witteveen在實測影片中，將Jev貼切地比喻為「一個聽得懂人話的超強if判斷式」。他以自動化客服為例，輸入一句「我被重複扣款了，請退款」，要求Jev同時判斷該轉交哪個部門、是否需退款，以及事情急不急。

實測發現Jev對語氣極為敏銳：原本那句話的「緊急度」僅7%，但只要加重語氣改成「現在就退！」，緊急度立刻飆升至6、7成。更重要的是，一般語言模型（LLM）回答出的機率數字，本質上只是靠「文字接龍」拼湊出來的一串字；而Jev給出的數值，是模型在數學層面真實算出的機率，這讓軟體系統在進行自動化派單時，有了更穩定可靠的判斷依據。

## Jev有多快？速度快40～200倍、輸出成本免費

Jev的核心賣點，可以具體濃縮為三個數字：

> 1. **反應快（70～500毫秒）** ：根據TypeSafe官方數據，Jev的端到端反應時間僅需70到500毫秒，遠勝目前頂尖LLM的3到329秒。
> 2. **效率高（快40～200 倍）** ：在處理「System One」（一眼就能判斷、不需複雜推理）的任務上，Jev比同等智力的模型快上40到200倍。
> 3. **成本低（輸出免費）** ： 每百萬輸入Token僅收0.042美元（約新台幣1.3元），輸出更是完全免費。以Sam Witteveen的實測為例，連續執行20個分類任務，總成本只需0.05美分。

## TypeSafe AI是誰創辦的？ChatGPT關鍵技術RLHF共同發明人創業

Almeida的履歷相當亮眼，曾先後效力於Google Brain與OpenAI，更是2022年InstructGPT論文的共同作者。正是這篇論文，成功將原本「只會接文字」的基礎模型，訓練成能聽懂指令做事的AI，成為後來ChatGPT誕生的基石。

諷刺的是，這位把AI教會「跟人對話」的關鍵推手，卻花了四年深陷「AI聊天很強，但到底怎麼用於自動化？」的困境。最終，他於2024年離開OpenAI，在舊金山創立了TypeSafe。

這家新創的陣容同樣堅強。技術長Erik Gafni是連續創業家，曾創辦DNA定序AI公司Ravel，也是兩家生技獨角獸的早期員工；營運長Sasha Sheng則是前Meta FAIR研究工程師，擁有深厚的AI產品與研究經驗。其餘團隊成員也多來自Stripe、Airbnb、Docker等頂尖企業。特別的是， **在遠距工作盛行的矽谷，這支菁英團隊至今堅持「每週五天進辦公室」的硬派作風。**

![TypeSafe AI聯合創始人](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/n57w-1789965797.jpg?w=2000&output=webp)

Diogo Almeida為TypeSafe AI聯合創始人（右）、Sasha Sheng為營運長（中）、Erik Spock Gafni為技術長（左）。

圖／ Diogo Almeida

## TypeSafe AI為何押注合成資料？Jev背後的技術路線

談到技術護城河，Almeida向《TechCrunch》透露，創業至今最關鍵的決策就是「很早就決定押注，所有訓練資料都由我們自己生成。」他自豪地表示，這是他人生中最棒的賭注，甚至認為這比發明RLHF還要重要。 **「我們公司有一半的編制是實驗室，基本上已經掌握了『統計上可理解的合成資料』這整個子領域。」**

至於模型為何命名為「Jev」？這源自19世紀經濟學家提出的「傑文斯悖論（Jevons Paradox）」——當商品變便宜，用量反而會暴增。Almeida賭的是，當AI的成本大幅下降，未來的世界不會是由單一「超級 App」壟斷，而是像早期網際網路一樣，到處都散布著聰明、高效的微型軟體。

當被問及 TypeSafe 是否自視為「前沿實驗室（Frontier Lab）」時，Almeida 犀利地回應：「前沿實驗室產出的主要商品是『恐懼』與『炒作』，但我希望我們的主要產品是『智慧』。」而這份務實的野心，在公司的簡短宣言中表露無遺：「我們打造的是生產系統，而不是神。」

> 延伸閱讀： [ChatGPT指令大全！復古海報風、風景照變紀念徽章、模擬80年代的你⋯130組提示詞一次整理](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete)

## Jev可以做什麼？3大應用：AI分類、Agent監控、模型路由

目前開發者們目前將Jev投入實際運用的場景，主要可歸納為三大類：

> 1. **取代LLM分類器** ：Vercel工程師Pranit Sharma原本用OpenAI的GPT-5.6 Luna審查指令安全性，改用Jev後速度暴增5到18倍，且更為精準；Bryo AI技術長Nikhil Mudholkar將Jev與Gemini進行商業郵件分類對比，發現Gemini雖準確度微幅領先，但成本卻貴了10到20倍。他強調：「Jev是唯一能回傳真實機率的模型，這讓它極度適合用於自動化工作流程。」
> 2. **監控其他AI代理（Agent）** ：創辦人Almeida認為，若用Agent來監督另一個Agent，成本很快就會失控。但換成用Jev來追蹤LLM Agent的操作軌跡並攔截「越獄（Jailbreak）」嘗試，能將防護成本控制在合理範圍。
> 3. **模型路由（Model Routing）與即時分流** ：開發者Ronacher提出，系統在處理任務前，可先透過 Jev判斷是否需要動用昂貴的旗艦模型。Jev的速度與極低價格，讓這種「即時分流」具備了高度的經濟效益，取代了過去用LLM做初步判斷的不划算做法。

## Jev怎麼用？3步驟串接Claude Code、Codex

要先說的是，Jev沒有聊天介面，一般人無法直接「跟它對話」。最順手的用法，是把Claude或Codex這些「AI程式助理」當成中間人，讓助理替你向Jev派發任務，其實只要三步驟就能開始用：

**第一步：開帳號、領免費額度**  
到 [TypeSafe](https://typesafe.ai/) 官網註冊免費帳號，不需綁定信用卡，就會直接送你5美元（約新台幣159元）的測試額度。要注意的是，Jev是依照你「輸入的字數（Token）」來計費，題目越長、扣款就越多。

**第二步：讓AI助理幫你裝好「TypeSafe外掛」**  
不用被程式碼嚇到，你只需要在你的AI助理（例如Claude Code或Codex）輸入對應的安裝指令。  
如果是用通用的AI助理，直接貼上這段讓它跑：

```bash
npx skills add typesafe-ai/skills --skill typesafe-ai
```

裝好之後，只要在對話框對著助理說：「 **使用TypeSafe skill** 」，就能喚醒這個功能。接著，按照官方指示到TypeSafe後台申請一組API金鑰（就像通行密碼），存入系統後，助理就能順利連線了。

**第三步：用大白話交代任務，讓助理幫你寫程式**  
你完全不需要懂複雜的 API 串接，只要用白話文把「你要分類什麼」以及「分類標準」跟 AI 助理講清楚即可。你可以直接複製以下這段話當作指令模板：

> 「請用 TypeSafe skill 幫我設計 Jev 的任務。我要整理 **\[貼上你的資料，例如聯絡人名單\]** ，請把每一筆資料分成 **\[類別A／類別B／類別C／不確定\]** 。  
> 規則：Jev 只負責判斷『語意』；如果有算數、日期或字串比對，請（AI 助理）你自己先算好再傳給 Jev。  
> 另外，請設定一個『信心門檻（Confidence）』，只要Jev信心低於0.6的，一律歸類為『待人工檢查』。動工前先給我看完整清單，我確認後再跑。」

這裡提到的 **信心指數（Confidence）** ，指的是Jev對自己的判斷「有多篤定」，而不是「答對的機率」。如果選項很明確，信心值就會接近1。官方建議，這個門檻可以先設定保守一點，用自己的資料測試後再微調。

> 延伸閱讀： [免費下載「會議紀錄整理師」！逐字稿丟給AI，自動抓決議跟待辦，支援ChatGPT與Claude](https://www.bnext.com.tw/article/92317/ai-meeting-notes-workflow)

## Jev適合哪些任務？3個自動化應用範例

具體來說，可以完成的自動化任務如下範例：

1. 將LinkedIn人脈自動分成不同優先等級。
2. 將Gmail聯絡人，自動分成「保留、待審、刪除」三種。
3. 幫多篇艱澀的學術論文，篩選出「對一般人有沒有用」。

## Jev使用前注意什麼？問題拆小、設定Confidence門檻

最後，新手實作前有兩個小提醒：

1. **問題要拆小** ：不要直接問Jev「這份創業簡報好不好？」它會不知所措；你要拆成「市場大不大？」、「技術可不可行？」分開問它，最後再請AI助理幫你綜合評分。
2. **AI仍不懂通靈/魔法** ：上述三個成功案例，其實都是靠著AI助理在背後幫你寫出一個迷你的網頁應用程式或Python腳本來執行。 **雖然你不用親自寫Code，但仍需要一些引導AI寫程式的基礎耐心。** 此外，如果給 Jev的題目太短，它的信心指數會下降，同一題重跑幾次可能會有微小的答案浮動。

> 延伸閱讀： [跟AI的對話被看光光？一次搞懂ChatGPT、Claude的9大隱私防線，4步驟保護機密不外洩](https://www.bnext.com.tw/article/92235/chatgpt-claude-privacy-9-stops-opt-out-training)

資料來源： [TypeSafe官方部落格](https://typesafe.ai/blog/introducing-system-one-models-and-jev) 、 [TypeSafe開發文件](https://docs.typesafe.ai/introduction) 、 [TypeSafe技術入門文件](https://docs.typesafe.ai/introduction/machine-learning-primer) 、 [TypeSafe Confidence文件](https://docs.typesafe.ai/confidence) 、 [TypeSafe安裝文件](https://docs.typesafe.ai/agent-skill) 、 [TypeSafe團隊頁](https://typesafe.ai/team) 、 [TypeSafe宣言](https://typesafe.ai/manifesto) 、 [TypeSafe融資新聞稿](https://finance.yahoo.com/technology/ai/articles/typesafe-ai-emerges-stealth-40m-190000776.html) 、 [《TechCrunch》](https://techcrunch.com/2026/09/18/a-new-kind-of-ai-model-from-a-chatgpt-inventor-is-thrilling-developers/) 、 [Sam Witteveen YouTube實測](https://www.youtube.com/watch?v=X117w2Rark8) 、 [Ruben Hassid電子報](https://ruben.substack.com/p/jev) 、 [OpenAI GPT-5.6 Luna模型文件](https://developers.openai.com/api/docs/models/gpt-5.6-luna) 、 [InstructGPT論文](https://arxiv.org/abs/2203.02155)

本文初稿為AI編撰，整理．編輯/ 李先泰

關鍵字： [＃ChatGPT](https://www.bnext.com.tw/tags/ChatGPT) [＃TypeSafe AI](https://www.bnext.com.tw/tags/TypeSafe%20AI)

往下滑看下一篇文章

<video title="Advertisement" src="https://rr4---sn-ipoxu-un5ed.googlevideo.com/videoplayback?expire=1790073564&amp;ei=XOqxav39FOG12roPxMqquQY&amp;ip=60.250.84.232&amp;id=7d2fcc211b21dda5&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=247&amp;met=1790044764,&amp;mh=8I&amp;mm=31&amp;mn=sn-ipoxu-un5ed&amp;ms=au&amp;mv=m&amp;mvi=4&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=26.192&amp;lmt=1753988300481906&amp;mt=1790044372&amp;cpn=QsBJnrz_dMlzns1a&amp;txp=5432534&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AE0s2JYwRgIhAMBocvnveWqm8Okv8z5GryXP_fcDCkqFZDVdzht43U2IAiEA87TbOSWKuwJGA99Tnd3hkJjyEDEOyeV1jIHRCKjz43w=&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRgIhAJ48iLuRleGzIcLeXd7ptYFvdnxeTwAn4fJ_wNT8aemCAiEAkCSJbJNcNuzBatLpUM3OJANBvY0lnUil8nr7hJpqAPk=" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.790.0_en.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_969276387%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95401453%5D%2C%5B95401454%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95402052%5D%2C%5B95402053%2C%5B%5B957164676%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=4001076911121770&amp;pvsid=2643258990660933&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F92319%2Ftypesafe-jev-decision-model-explainer%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="169"></iframe>

即時熱門文章

[1 OK超商大變身！紅白招牌換藍白、新Logo像一尾魚，美廉社母公司打什麼算盤？](https://www.bnext.com.tw/article/92318/ok-mart-acquisition-new-store-logo) [2 皮拉提斯、瑜珈免費上！運動部「國民體適能計畫」誰能參加？資格、申請方式一次看](https://www.bnext.com.tw/article/92320/free-fitness-course-2026) [3 Gemini Notebook教學地圖！AI筆記本、簡報製作、財報解讀28篇精華用法一次看](https://www.bnext.com.tw/article/91193/notebooklm-complete-guide-and-use-cases) [4 開發者免盯場！Claude Code推出新版「專案」功能：只要設定目標，AI就能自動完成多線開發](https://www.bnext.com.tw/article/92300/claude-code-projects-redesign-parallel-threads) [5 經濟學家警告美股恐暴跌72%！預測4階段走勢：先衝3.9萬點、再跌回2022年低點](https://www.bnext.com.tw/article/92296/us-stock-crash-72-percent-forecast) [6 2026行動支付怎麼選？LINE Pay、街口、Apple Pay差在哪？TWQR是什麼？一張表看懂](https://www.bnext.com.tw/article/92274/mobile-payment-taiwan-how-to-choose)

![把老師傅經驗變成 AI！智穎智能讓射出機從「導航」走向「自駕」，桃園如何助攻技術落地？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/zq99-1789617822.jpg?w=900&output=webp)

2026.09.17 |

把老師傅經驗變成 AI！智穎智能讓射出機從「導航」走向「自駕」，桃園如何助攻技術落地？

塑膠射出產業面臨老師傅退休與經驗斷層，智穎智能從桃園出發，以 AI 破解製程難題，並在桃園市政府串聯場域、客戶與海外資源下，加速技術落地與海外布局。

---

分享

曾是台灣製造業重要根基的塑膠射出產業，如今正面臨人才斷層危機。尤其桃園聚集大量工業區與製造業者，老師傅退休、新人才接不上的現況，更是許多工廠正在面對的現實難題。

從汽車零組件、網通設備、醫療器材到半導體載具，許多產品都離不開射出成型。智穎智能創辦人暨執行長張詠翔指出，全球塑膠射出市場規模超過 3000 億美元，每年仍以約 3% 至 4% 的幅度成長。需求並未消失，只是製造基地從台灣、中國逐步往東南亞等地移動。但機器可以搬、訂單可以轉，老師傅數十年累積的經驗卻很難複製。

這道人才缺口，正是 2020 年成立的智穎智能切入市場的機會。團隊從桃園中原大學的研究能量出發，歷經近十年研究與工廠驗證，嘗試把老師傅腦中的射出經驗，轉化成可以計算、保存與持續學習的 AI 模型，解決產業第一線面對到的挑戰。

## 把老師傅經驗交給 AI，射出成型從「導航」走向「自駕」

「餐廳沒有廚師是不行的。有再好的設備，不會做的人還是會把它做失敗。」張詠翔如此比喻射出成型的技術門檻。當模具完成後，生產現場仍得依產品、材料與機台特性，調整溫度、速度、壓力、位置等條件；即使使用同一種塑料，不同產品的調整方式也可能完全不同。

而老師傅的珍貴之處，不只是記住參數，而是知道面對材料、機台與環境變化時如何修正。智穎智能要做的，就是把這些難以傳承的「火候」拆解成可以運算的製程邏輯。目前其 AI 解決方案已應用於超過 200 種材料、50 多種產品類型。

把老師傅的經驗轉化為製程邏輯後，第一步就是讓 AI 協助操作人員找到合適的成型條件。智穎智能為此開發智能成型導引系統，工廠只要輸入材料、機台規格、產品尺寸等資訊，sMolding 便能計算第一組成型參數；若試模後出現產品缺陷，再由 sTroubleshooting 提供修正建議。

![](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/kxx2-1789617824.png?w=2000&output=webp)

用手機即可查看系統狀況

圖／ 智穎智能提供

「它就像導航。我今天告訴你要從 A 點到 B 點，但不知道路怎麼走，導航會把路徑算給你。」張詠翔說。對射出成型而言，這條「路徑」就是溫度、速度、壓力與位置等參數如何組合。以半導體載具試模為例，傳統方式需 7.5 小時，導入系統後縮短至 2 小時，材料使用量從 23 公斤降至 8.7 公斤，整體試模成本降低 64%。

但找到正確參數只是第一步，真正進入量產後，工廠面對的「路況」還會持續改變。溫溼度、材料批次與機台耗損，都可能讓原本設定好的製程條件產生偏移。智穎智能因此進一步開發 StabMolding 成型穩定模組，透過射出壓力數據持續監控品質，並依生產狀態自動補償關鍵參數。換言之，AI 不只在試模階段提供一條正確的「導航路徑」，進入量產後還能因應製程變化持續修正，進一步朝「自駕」邁進。

## 從產線驗證到海外布局，桃園如何助攻智穎智能跨過0到1？

但要讓 AI 真正走進工廠，智穎智能採取 SaaS 訂閱模式，將核心運算留在雲端持續學習與更新，降低中小型製造商一次投入大量資金的門檻。另一項差異則是「跨廠牌」。現實中的射出工廠往往同時擁有台灣、日本、歐洲不同品牌與年份的設備，因此智穎智能的方案既能導入既有設備，也能與射出機、控制器業者合作，在新機出廠時直接整合。「一個工廠不會只有一個廠牌，就像你家的車庫不會永遠只放同一品牌的車。」張詠翔說。

對智穎智能而言，AI 真正走進製造現場的關鍵，在於不同世代、不同品牌的設備都能導入。只是對工業 AI 而言，降低導入門檻還不夠，更關鍵的是能否取得真實產線反覆驗證。對從中原大學研究成果衍生創立的智穎智能而言，桃園不只是公司所在地，也提供了技術走向市場的第一塊試驗場。「對新創來說，第一個就是要經費、要人才、要場域，再來就是客戶與合作夥伴資源。」張詠翔說。

成立初期，團隊透過桃園市政府研發補助、新創競賽與基地進駐等資源，降低早期研發與營運負擔。更重要的是，桃園密集的製造業聚落，本身就是智穎智能最直接的潛在市場。透過市府串聯工業區與在地企業，智穎智能在創業初期接觸到位於桃園的「良器」，成為公司第一個客戶。從概念驗證、產品調整到實際導入，合作一路延續至今。對必須進入產線反覆驗證才能成熟的工業 AI 而言，第一個願意開放場域的客戶，意義遠超過第一筆營收。

如今，從桃園製造現場磨出的解決方案也開始往海外走。「日本這一次，完完全全都是桃園市政府帶著我們去做這件事情。」張詠翔表示，透過市府帶領團隊前往日本神戶，智穎智能接觸當地官方與企業資源。在市府引薦下，團隊成功對接了神戶知名加速器，更取得日方針對外資新創設立公司與拓展市場的兩階段實質補助資訊。目前團隊正籌備日本布局，為下一階段海外市場拓展做準備。

![](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/y966-1789617822.jpg?w=2000&output=webp)

智穎智能在 TaipeiPLAS 2026 台北國際塑橡膠工業展，展示 AI 智慧製造成果，現場商洽氣氛熱烈，成功吸引產業關注。

圖／ 智穎智能提供

## AI 解決「怎麼生產」後，下一步是「替誰生產」

當 AI 開始解決「怎麼生產」，張詠翔看到的下一個問題，是「替誰生產」。部分製造商提高生產效率後反過來問他，如果我的產能多了 20%，但沒有多 20% 的訂單，我何必要做那麼快？

這個問題催生了智穎智能下一階段的製造媒合平台構想。隨著供應鏈分散至不同國家，品牌要在海外重新尋找符合特定規範與製程能力的供應商，也愈來愈困難。團隊因此希望串聯既有使用 AI 方案的製造商，讓品牌端能依條件尋找供應商，也讓製造端的閒置產能有機會被看見。團隊預計 2027 年上半年推進平台服務，初期規劃串聯超過 200 家製造商與破千台機器。

從中原大學的研究，到桃園工廠提供第一個實際驗證場域；從把老師傅經驗變成 AI「導航」，再走向能自行修正製程的「自駕」，智穎智能試圖數位化的，是製造業最難被保存的資產：人的經驗。而當全球供應鏈持續移動、製造基地愈來愈分散，這套從桃園工廠磨出的 AI，下一步不只要替老師傅找到數位接班人，也要重新連結散落全球的製造能力。  
而「Meet 桃園+」也期盼持續透過計畫的媒體報導與宣傳資源，將更多像智穎智能這樣扎根在地、走出海外的創業故事傳遞出去，讓外界看見桃園豐沛且具實戰力的創新動能。

即時熱門文章

[1 OK超商大變身！紅白招牌換藍白、新Logo像一尾魚，美廉社母公司打什麼算盤？](https://www.bnext.com.tw/article/92318/ok-mart-acquisition-new-store-logo) [2 皮拉提斯、瑜珈免費上！運動部「國民體適能計畫」誰能參加？資格、申請方式一次看](https://www.bnext.com.tw/article/92320/free-fitness-course-2026) [3 Gemini Notebook教學地圖！AI筆記本、簡報製作、財報解讀28篇精華用法一次看](https://www.bnext.com.tw/article/91193/notebooklm-complete-guide-and-use-cases) [4 開發者免盯場！Claude Code推出新版「專案」功能：只要設定目標，AI就能自動完成多線開發](https://www.bnext.com.tw/article/92300/claude-code-projects-redesign-parallel-threads) [5 經濟學家警告美股恐暴跌72%！預測4階段走勢：先衝3.9萬點、再跌回2022年低點](https://www.bnext.com.tw/article/92296/us-stock-crash-72-percent-forecast) [6 2026行動支付怎麼選？LINE Pay、街口、Apple Pay差在哪？TWQR是什麼？一張表看懂](https://www.bnext.com.tw/article/92274/mobile-payment-taiwan-how-to-choose)

[![台達電全解讀](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/e5i9-1785123864.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130221)