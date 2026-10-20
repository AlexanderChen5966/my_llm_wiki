---
title: "AI家家酒【Ollama】×【Continue】一起用Vibe Coding毀滅公司吧"
source: "https://www.cc.ntu.edu.tw/chinese/epaper/home/20260320_007606.html"
author:
published:
created: 2026-05-15
description:
tags:
  - "clippings"
status: processed
---
### 專題報導

AI家家酒【Ollama】×【Continue】一起用Vibe Coding毀滅公司吧

- 卷期：v0076
-  出版日期：2026-03-20

作者：陳俊佑 / 臺灣大學計算機及資訊網路中心程式設計組行政專員

---

### 用Ollama玩Vibe Coding。

### 前言

大家好，我是富田町47番地的社畜工程師柚子哥，很高興可以跟大家分享自己玩AI的心得，近期Vibe Coding一詞非常火紅，用AI輔助寫程式其實最早在2021年時GitHub就推出過Copilot給大家嚐鮮，當時評價可說是好壞參半，但AI經過這幾年的淬鍊，各種花式應用都出來了，當然輔助寫程式也是越做越好，只要規格寫的完整，AI就會自動把整個架構寫出來，還可以用運作(但結果正不正確是另外一回事)，萌發很多不懂程式的人也開始用AI去接案，用低廉的價格去搶開發案，藉此賺一波熱錢，結果就是在社群平台上開始看到各式大型翻車現場，諸如接案後棄坑、違約、後續維護開發不出新功能，甚至爆出大量系統資安危機(目前AI會寫程式但是對於哪接環節會有資安問題並不會知道)。

Vibe Coding的出現，讓更多小公司只願意聘請低薪工程師(甚至工讀生)用AI寫程式碼，產出的程式碼做什麼用途連自己都不懂，只要能動就好，但隨者系統越龐大、結構越龐大，AI自己產出的程式碼會開始出現大量的不正確性，最終結果就是產生更多技術債，反而延伸出Vibe Coding Cleanup Specialis(Vibe Coding清除專家)的職位來替這些公司擦屁股。

但Vibe Coding不是萬惡之源，好好利用可以讓初級工程師學習進階的coding技術，快速提升技能，彌補不足的知識，讓初階工程師更快速的成長為進階工程師，用AI幫公司傳承核心技術，或是用AI當學長姐帶領初階工程師，資深工程師有更多的時間，才可以接更多大案子，增加公司營收，發出更多分紅給工程師以此留才，形成正循環，這才是公司可以好好利用的地方。

### 二、用ollama輔助開發

            前幾篇教學有提到在mac上架設自己的ai服務，也用postman去測試ollama的API是可使用的，這時就可以好好的利用本地端的AI來幫忙寫程式，也就是貧民版的Vibe Coding，要達成AI協助開發程式還需要另外一個工具來幫忙，就是開源的【continue】。

### 三、continue在vscode的延伸模組

Continue是AI 程式碼助理的vscode延伸模組，讓開發者在編輯器中可以聊天 (Chat)、即時補完(Autocomplete／Tab 補完)、編輯建議、多檔案操作(Agent)等功能。詳細的可以看他們官網介紹[https://www.continue.dev/](https://www.continue.dev/ "Js")。

更重要的是他可以接本地端的AI，因為很多大公司都害怕自己的商業機密外流，不允許使用雲端AI，或是像是公家單位AI使用政策大是要安全、可控與合規保障，因此地端自架AI反而是第一首選。

而我今天使用的coding IDE是vscode，因此我就在vscode的延伸模組上搜尋continue。

![20260320_007606_01](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/7c594722-86e0-40aa-92ca-477babfe85eb.png)

圖一、continue延伸模組

當安裝好延伸模組，側欄就會多一個continue的功能。只要點開就會出現對話模組。

![20260320_007606_02](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/2cfa317a-7fff-48b7-a8cc-ce1d8752639a.png)

圖二、continue對話功能

但這時還不能用，我們要把本地端架好的ollama api新增到continue裡面。這時要點下面的選項【configure own models】。就可以選擇本地端的ollama，這時裡面已經有預設好ollama預設的api 是localhost :11434，以及基本的ai 模型，可以自己選更大的語言模型(有更好的回答模式)，好了就按下【connect】。

![20260320_007606_03](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/6e937547-1c81-411c-a1fe-c42939ab8e24.png)

圖三、連接本地端ollama

按下【connect】就會開始自己下載設定好的AI模型，下載完成並測試完成就會亮綠燈，這時就可以使用了。就是這麼簡單。

![20260320_007606_04](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/c674fd58-162b-4999-aec5-16b0c32f5520.png)

圖四、下載及測試

如果要新增不同的語言模型到connect，並設定不同功用時，可以到設定裡面去新增及設定。

![20260320_007606_05](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/f268242e-bd96-4643-802c-30436556f338.png)

圖五、設定

如果有整間實驗室架設的AI模型，也可以從設定新增。只是API要改成實驗室AI的IP位置。而實驗室也記得要做好安全措施，只能限定試驗室內的網段可以連進去，不要開放給外部亂連。

![20260320_007606_06](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/da94545d-5027-474c-bbf7-41524ff046ca.png)

圖六、新增實驗室架設的AI

四、continue基礎用法

continue用法很多這邊只簡單示範，例如用滑鼠框住程式碼的範圍，按下CTRL + L，程式碼現在出現在對話框裡面。這時只要提出對話，AI就會回答。

![20260320_007606_07](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/c64b2e53-1a3f-4063-bf46-3f54c12e0084.png)

圖七、滑鼠框住程式碼的範圍，按下CTRL + L

這時我用gpt-oss-20b的模型幫我解釋這一段tailwind css 的效果，他有很正確的解析出來。

![20260320_007606_08](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/40856ff4-e6a5-4ce0-9d46-078849543093.png)

圖八、請AI翻譯code

這時我假裝自己不會寫程式，請AI開發九宮格OOXX的遊戲，看看AI的能力。AI就在對話框寫了很多程式碼。

![20260320_007606_09](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/499b7c0c-a426-4b51-af6d-d31901dc048f.png)

圖九、請AI開發遊戲

![20260320_007606_10](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/7655fe3d-68d0-4068-9000-e9205cc52446.png)

圖十、AI回答結果

![20260320_007606_11](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/41ec1881-6d56-4a45-b272-f0ef008572f5.png)

圖十一、AI回答結果

![20260320_007606_12](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/d8494b30-0f71-46a1-a831-d521703293b0.png)

圖十二、AI回答結果

![20260320_007606_13](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/8ff46dab-5a7a-40ff-816d-fcce5df46cb1.png)

圖十三、AI回答結果

![20260320_007606_14](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/49d158ea-4a09-4da3-9c6a-2c778458bed1.png)

圖十四、AI回答結果

AI在對畫框產生的程式碼只要按下Apply就會自動建置起來，並生成相關檔案。

![20260320_007606_15](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/6b214347-e9f9-4390-93be-a8e0658ecb80.png)

圖十五、Apply

這時就可以啟動小遊戲，但會發現有些不完美，和局的時候不會出現提示。

![20260320_007606_16](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/b07e2c84-b936-4a0c-9d69-ce0727f470cc.png)

圖十六、啟動網頁小遊戲

![20260320_007606_17](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/69c67a5a-e37a-4e82-aa01-8266493c0c68.png)

圖十七、有BUG的小遊戲

所以這時候要再跟AI講我要需求，希望和局可以有提示並且重置遊戲，AI知道我們的需求後就會開始修正，因為我們是慣老闆提出的需求很模糊，所以要一步一步跟AI對馬，以此類推把功能完善，最終達到我們要的需求。

![20260320_007606_18](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/87ffb15c-5521-469c-87b7-9a778f75dfbd.png)

圖十八、提出修正需求

![20260320_007606_19](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/7a37ed60-fc26-4126-82c0-44b06e2875b0.png)

圖十九、正確的顯示和局

我們可以盡情的PUA大語言模型，提出一些慣老闆最愛的無腦需求，例如我要求他把遊戲改成歐風配色。就考驗AI的美感。只要在對畫框打出@再接上我們要修改的檔案，並提出需求，就只會針對這個檔案去修改。最後AI把自己認知的歐洲風格呈現出來。

![20260320_007606_20](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/9ae79321-68bd-461c-a913-0608c3954e47.png)

圖二十、練習當慣老闆。

![20260320_007606_21](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/6d936b7a-cc29-4db0-b63b-de4523ccc17a.png)

圖二十一、開始分析什麼是歐洲風格

![20260320_007606_22](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/95f6acd4-4c59-4a09-b020-8d535b204bbb.png)

圖二十二、AI認知中的歐洲風格

但如果接手別人用AI產出的code，看不懂在寫什麼，那就用它的魔法來對付它吧。通常我會用AI幫我寫註解。一樣用@去代出要改的檔案。請AI幫我寫註解，最後就一鍵apply加上註解。當然我是連看都不看，這主要是讓AI寫註解給AI看，程式碼有註解之後，請AI改程式時會比較容易從註解去找對應的功能並帶入副程式或是修改需求。算是提升一點AI正確率的方法之一。

![20260320_007606_23](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/6b66d6c2-2fe4-43c2-8df4-088b50320163.png)

圖二十三、用@標註檔案

![20260320_007606_24](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/6ab52300-0aa9-4e64-bf4b-3313f32b7be0.png)

圖二十四、提出註解的需求

![20260320_007606_25](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/6e3d2f55-7b55-4119-b5ec-c2afcc015f73.png)

圖二十五、AI產出註解

範例就先代到這邊，之後有玩出進階功能再跟大家分享。

### 八、結語

如果你任職的公司老闆是慣老闆，低薪高工時，毫無疑問的請在VS Code上裝Continue，保證讓你的程式人生開始走向兩個極端——效率巔峰與災難深淵，不斷按下Tab跟Enter產出大量的程碼，大量使用不用跟他客氣，靜靜看著公司在燃燒，不斷出現大型翻車事故，讓這間公司被自然淘汰。但毀滅公司的同時請記得也要不斷提升自己的能力，精進知識與技能，努力考取證照，畢竟工程師可是有價值的訂閱服務，努力提升自己能力才會有其他公司用更高的價格訂閱你的時間為他服務。

### 參考資料

Continue官網：[https://www.continue.dev/](https://www.continue.dev/ "Js")