---
title: "iPhone Duo 為什麼把 tab bar 搬到側邊？折疊 iPhone 設計指南：size class、保留區與 Figma 畫板尺寸"
source: "https://rar.design/posts/iphone-duo-design-guide-hig-tab-bar"
author:
  - "[[設計師 Riven]]"
published: 2026-09-10
created: 2026-09-15
description: "iPhone Duo 內螢幕 7.6 吋、外螢幕 5.4 吋，10 月 23 日開賣。蘋果同日在 HIG 新增〈Designing for iPhone Duo〉，要求用兩個 size class 撐所有姿勢、避開三個保留區、把工具列與 tab bar 移到側邊。本文整理尺寸換算、版面規則、官方邊框..."
tags:
  - "clippings"
status: processed
---
蘋果在發表會當天交出 HIG 新頁與六支 Tech Talk，設計師開賣前要改的畫面、要開的畫板、要準備的兩套截圖

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80,format=webp/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年9月10日 下午 8:32

UI/UX 設計

UI/UX · APPLE · 2026

![iPhone Duo 展開後的內螢幕，主畫面的 Dock 與狀態列直排在右側邊緣](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/bc3dd657-0744-4643-b34c-541626ac8e5e/d4c83323-4ce1-4c46-8968-5395876dc6f0.jpg)

iPhone Duo 展開後的內螢幕，主畫面的 Dock 與狀態列直排在右側邊緣

▲ iPhone Duo 展開後的內螢幕：Dock 與狀態列都站在右側邊緣，垂直空間整片留給內容（圖：Apple）

QUICK ANSWER

iPhone Duo 是蘋果第一台折疊 iPhone，內螢幕 7.6 吋、外螢幕 5.4 吋，10 月 23 日開賣。蘋果同一天在 Human Interface Guidelines 新增〈Designing for iPhone Duo〉，核心只有三件事：用 compact 與 regular 兩個 size class 撐住所有姿勢、避開動態島、相機與摺線三個保留區、把工具列與 tab bar 移到側邊。本文整理尺寸換算、版面規則、mockup 素材與開賣前時間表。

iPhone Duo 闔起來的時候，tab bar 站在螢幕右側。

iPhone 的 tab bar 從 2007 年起就待在底部。9 月 9 日發表會當天下午，蘋果在 Human Interface Guidelines 新增〈Designing for iPhone Duo〉這一頁，把它搬走了：外螢幕又寬又矮，狀態列、動態島、工具列、tab bar 全部靠到側邊排成一直行，把垂直空間留給內容。同一天上線的還有六支 Tech Talk、開發者專頁、規格頁，以及 Design Resources 裡展開與闔起兩種狀態的官方邊框。

這篇文章把這些一手資料讀完，整理成設計師現在就用得上的東西：兩塊螢幕的真實尺寸和換算後的 Figma 畫板 pt 數、蘋果要求的兩個 size class、三種保留區、側邊控制列的排序規則、mockup 與 App Store 截圖的準備方式，還有從現在到 10 月 23 日開賣前的時間表。

我做了近十年 UI/UX，這是第一次看到蘋果在發表會當天就把硬體、設計指南和素材一起交齊。接下來六週，這頁指南會決定你的 App 在一台 NT$74,900 的手機上，是撐滿整片內螢幕，還是縮在中間像一支普通 iPhone。

CONTENTS

01

[發表會當天下午，蘋果交出的四樣東西](#chapter-1)

02

[兩塊螢幕到底多大：px、pt 與比例](#chapter-2)

03

[兩個 size class，撐住所有姿勢](#chapter-3)

04

[三個保留區：動態島、相機、摺線](#chapter-4)

05

[側邊控制列：iPhone 介面文法的新一頁](#chapter-5)

06

[1:1.42 的代價：影片、遊戲與滿版畫面](#chapter-6)

07

[現在就能動手的三件事](#chapter-7)

08

[開賣前的時間表與重點整理](#chapter-8)

CHAPTER 01 · WHAT CHANGED

## 發表會當天下午，蘋果交出的四樣東西

台灣時間 9 月 10 日凌晨一點 keynote 開場，iPhone 18 Pro 先上，接著是 iPhone Duo。 [WWDC 2026](https://rar.design/posts/wwdc-2026-liquid-glass-siri-ai-recap) 時蘋果把 Liquid Glass 底層重修了一遍，很多人以為今年設計面的大事到此為止，結果硬體發表會又補了一刀。

![](https://i.ytimg.com/vi/FUfGcZ092b0/hqdefault.jpg)

▲ Apple 官方影片〈Introducing the new iPhone Duo〉，發表會當天上線

Keynote 結束後幾小時內，四樣東西同時上線。第一是 Human Interface Guidelines 的新頁面〈Designing for iPhone Duo〉，更新紀錄寫著 September 9, 2026、New page，內容涵蓋裝置姿勢、雙螢幕的動態版面、還有工具列與 tab bar 改站垂直軸的規則。第二是 developer.apple.com/iphone-duo 這個專頁，掛了六支 Tech Talk：Design for iPhone Duo、Prepare your app for iPhone Duo、Strike a pose with adaptive layouts on iPhone Duo、Raise the bar with iPhone Duo，另外兩支講多螢幕與相機。第三是規格頁，兩塊螢幕的解析度與 ppi 全部公開。第四是 Design Resources 的 Product Bezels 區新增了 iPhone Duo 與 iPhone 18 的邊框素材。

給設計師看的那支叫 Design for iPhone Duo，由蘋果設計團隊主講，建議整篇文章看完之後回頭看一次，你會發現指南裡每條規則背後都有一個具體的 App 案例。

![](https://i.ytimg.com/vi/do3UqxfYc3I/hqdefault.jpg)

▲ Apple Developer 頻道的 Tech Talk〈Design for iPhone Duo〉，發表會當天上線

CHAPTER 02 · TWO DISPLAYS

## 兩塊螢幕到底多大：px、pt 與比例

先把數字擺出來。內螢幕 7.6 吋，1878×2670 px，430 ppi，nano-texture 霧面處理；外螢幕 5.4 吋，1398×2034 px，460 ppi。兩塊都是 Super Retina XDR、ProMotion 最高 120Hz、常駐顯示，而且兩塊都有動態島。內螢幕的前鏡頭藏在螢幕下方，只在相機啟動時才現身；外螢幕的前鏡頭在角落，永遠看得見。

![闔起與展開的 iPhone Duo 並排，外螢幕 5.4 吋、內螢幕 7.6 吋](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/cecfcee3-d74e-40bc-b7dd-ea5ea46126ed/c2e9c248-da64-4c60-b43d-5cb8924abc83.jpg)

闔起與展開的 iPhone Duo 並排，外螢幕 5.4 吋、內螢幕 7.6 吋

▲ 闔起與展開的 iPhone Duo 並排：外螢幕 5.4 吋、內螢幕 7.6 吋，兩塊比例幾乎相同（圖：Apple Newsroom）

626×890

內螢幕 · PT（@3x 換算）

466×678

外螢幕 · PT（@3x 換算）

1:1.42

內螢幕長寬比 · 接近 A4

蘋果的規格頁只給像素，沒有給 pt。現役 iPhone 全部是 @3x，把像素除以三就是 626×890 與 466×678，這也是幾個開發者部落格在發表會當晚算出來的數字。模擬器要等本月稍晚的 Xcode 27.1 beta，在那之前請把這兩組數字當成「換算值」，畫板先開，量測等模擬器。

DISPLAY

Duo 內螢幕

Duo 外螢幕

iPhone 18 Pro

尺寸

7.6 吋

5.4 吋

6.3 吋

像素

1878×2670

1398×2034

1206×2622

ppi

430 ppi

460 ppi

460 ppi

pt（@3x）

626×890

466×678

402×874

長寬比

1:1.42

1:1.45

1:2.17

size class

regular

compact

compact

表格裡最該盯住的一列是外螢幕。466×678 pt 對上 iPhone 18 Pro 的 402×874 pt，寬了 64 pt，矮了 196 pt。矮掉的兩百個點差不多是一整組 navigation bar 加 tab bar 再加一個 section 的高度，這就是蘋果把控制列全部趕去側邊的原因。內螢幕那邊，2670 除以 1878 是 1.42，跟 A4 紙的 √2（1.414）幾乎重疊，蘋果自己的說法是內螢幕面積比 iPhone 18 Pro Max 大五成，外螢幕面積則有 iPhone 18 Pro 的九成以上。你可以把展開後的 iPhone Duo 想成一張直放的 A5 紙，闔起來則是一支矮胖版的 iPhone。

![iPhone Duo 外螢幕示意圖，鉸鏈在左側、前鏡頭在右上角](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/7d6612cf-332b-431a-920e-edc5cf06eda7/539a34d4-3d98-48f9-9152-fe95ba0e9c1e.png)

iPhone Duo 外螢幕示意圖，鉸鏈在左側、前鏡頭在右上角

▲ 外螢幕：鉸鏈在左側，前鏡頭固定在右上角（圖：Apple HIG）

![iPhone Duo 內螢幕示意圖，鉸鏈在正中央、前鏡頭在右上角](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/c23eba36-ef16-4946-b10e-f14eb7847a2d/7be8126f-108d-4211-bfa9-cc3bd57bf25e.png)

iPhone Duo 內螢幕示意圖，鉸鏈在正中央、前鏡頭在右上角

▲ 內螢幕：鉸鏈在正中央，前鏡頭藏在右上角的螢幕下（圖：Apple HIG）

CHAPTER 03 · SIZE CLASSES

## 兩個 size class，撐住所有姿勢

HIG 對 iPhone Duo 的第一條規則，跟很多人的直覺相反。這台機器可以半開像一本書、攤平放在桌上、立在桌緣當小相框，看起來像要為每一種姿勢各畫一套版面，蘋果的指南明講：支援各種姿勢，不代表為每種姿勢設計一套自訂版面。外螢幕用 compact width 的版面，內螢幕用 regular width 的版面，這兩套就是所有姿勢的基本盤。App 改變尺寸的時候不要重新發明介面，讓既有的版面依可用空間展開就好。

![HIG 列出的六種 iPhone Duo 裝置姿勢線稿](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/91071d76-02c7-448c-8dfa-ff53b46cd7f1/a4540f20-1adf-4924-a95d-f9f92aa6ae1a.png)

HIG 列出的六種 iPhone Duo 裝置姿勢線稿

▲ HIG 列出的六種裝置姿勢，版面只需要 compact 與 regular 兩套（圖：Apple HIG）

Mail 是指南裡的示範。闔起時外螢幕只顯示一層，信件列表或單封信件擇一；展開後兩者並排。資訊架構沒有動，只是在大螢幕上多露出一層。這跟 iPhone 轉去 iPad 的邏輯完全一樣，split view 在內螢幕展開成雙欄、在外螢幕收成單欄，用標準元件蓋的 App 幾乎不用改。

![Mail 在 iPhone Duo 外螢幕，單封信件，控制列直排右側](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/c5cad692-5f3f-4214-9846-fe19b3523ce9/b0f928c0-7514-46f4-a6cd-ab3a0d700085.png)

Mail 在 iPhone Duo 外螢幕，單封信件，控制列直排右側

▲ 外螢幕的 Mail：一次只顯示一層，控制列直排在右側（圖：Apple HIG）

![Mail 在 iPhone Duo 內螢幕，信件列表與信件內容並排](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/bfdaaf1d-6d29-43fa-bbc3-dea9cdcfe23e/6b9c9aca-6bdc-4104-bf7b-824cbcbc6a2a.png)

Mail 在 iPhone Duo 內螢幕，信件列表與信件內容並排

▲ 內螢幕的 Mail：列表與信件並排，多露出一層（圖：Apple HIG）

外螢幕 · 闔起

compact width

單欄版面，一次一層資訊。狀態列、動態島、工具列、tab bar 全部直排在側邊，垂直空間留給內容。split view 收成單一 pane。

內螢幕 · 展開

regular width

雙欄版面，多露出一層資訊。橫向時控制列仍在側邊，直向時垂直空間夠，保留標準的橫向工具列。split view 展開成兩欄，可以跑 Split View 多工。

指南裡真正的新東西叫 arrangement view，一種裝著主視圖與次視圖的版面容器，依螢幕大小、方向和保留區自動決定怎麼排。它有兩種型態：split 會把空間分給兩個視圖，比較寬時左右切、比較高時上下切；overlay 則把兩個視圖疊在一起，裝置半開時各佔一邊，其他時候主視圖蓋在次視圖上。指南給的對應關係很好記：原本用 HStack 或 VStack 並排的兩個視圖，直接對應 split arrangement；原本用 ZStack 疊起來的，對應 overlay arrangement。它只管版面不管導覽，navigation split view 和 tab view 要包在它外面。

![split arrangement 示意圖，次視圖與主視圖左右分割](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/20463036-c54b-4d58-9118-52acbfedf3d8/aa02bfc9-7554-4e3d-88f2-2de999646e29.png)

split arrangement 示意圖，次視圖與主視圖左右分割

▲ split arrangement：次視圖與主視圖左右分割（圖：Apple HIG）

![overlay arrangement 示意圖，主視圖疊在次視圖上](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/7fb748bd-7a7b-4907-a6c7-68e9ad86208c/f836e29f-42f3-48ed-8417-155b99a70ec0.png)

overlay arrangement 示意圖，主視圖疊在次視圖上

▲ overlay arrangement：主視圖疊在填滿畫面的次視圖上（圖：Apple HIG）

對 Figma 端的意義是：你的元件必須真的能 resize。 [Auto Layout 的 fill、hug、fixed 三種 resizing](https://rar.design/posts/figma-auto-layout-guide) 在這台機器上會被實際考驗，一個寫死寬度的卡片在 466 pt 與 626 pt 之間會露餡。用 [Slots](https://rar.design/posts/figma-slots-design-system) 管理內容替換的設計系統，這次會比 detach 派輕鬆很多。

![Slack 在 iPhone Duo 內螢幕，頻道列表與對話並排，控制列在右側邊緣](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/446872a9-cdb4-43f8-b748-48bbffdaea67/a235750d-1431-4374-bc12-042321b7177b.jpg)

Slack 在 iPhone Duo 內螢幕，頻道列表與對話並排，控制列在右側邊緣

▲ Slack 在內螢幕：頻道列表與對話並排，控制列在右側邊緣（圖：Apple Newsroom）

![](https://i.ytimg.com/vi/d1xi9GAfSRI/hqdefault.jpg)

▲ Tech Talk〈Strike a pose with adaptive layouts on iPhone Duo〉，講姿勢與自適應版面

CHAPTER 04 · RESERVED REGIONS

## 三個保留區：動態島、相機、摺線

safe area 大家都會處理，iPhone Duo 在 safe area 之上多了一層叫 reserved regions 的概念：螢幕上有些區域內容要避開，元件要順著它調整。指南說如果你做過 iPad 上的視窗控制項，會覺得眼熟。

![iPhone Duo 外螢幕的相機區示意圖，位在右上角](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/1d7e72d1-9ccd-4d3a-87a6-783de859cc28/5b9b490d-78f6-4f28-8a01-50352688961d.png)

iPhone Duo 外螢幕的相機區示意圖，位在右上角

▲ 外螢幕：前鏡頭在右上角，永遠看得見（圖：Apple HIG）

![iPhone Duo 內螢幕的摺疊區與相機區示意圖](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/a870d79a-be61-4d88-99e2-60a42f214b9c/fe4c2274-0262-41cd-8e5e-003833d413b9.png)

iPhone Duo 內螢幕的摺疊區與相機區示意圖

▲ 內螢幕：摺疊區在正中央，相機區只在拍照時出現（圖：Apple HIG）

01

動態島區

永遠存在。Live Activities 出現時會擴大。控制列在側邊時，系統會自動把元件排開避讓。

02

相機區

只在相機啟動時出現。內螢幕的鏡頭平常藏在螢幕下，一開相機，UI 會讓開一塊，提示鏡頭在那裡。

03

摺疊區

裝置半開時才出現。把內螢幕分成兩塊可用區域，中間那條摺線被扣掉。

大部分系統元件會自動避讓：alert、context menu、sheet 遇到摺線會自動移位，navigation split view 會調整欄寬與邊距去對齊內螢幕的對稱結構。自訂元件則要靠 reserved region API 把內容推離保留區。指南給了兩條實務建議，第一條是版面容器選會自己適應的那種，例如備忘錄的 split view 會在裝置摺起時調整兩個 pane 的寬度，讓兩邊都看得清楚；第二條是格線版面用偶數欄，內容才能在摺線兩邊分得乾淨。三欄的產品格線在半開的 iPhone Duo 上，中間那欄會剛好卡在摺線上。

還有一句話很值得抄進設計系統文件：只移動必要的東西。元件消失或大幅位移都會讓人找不到，小幅調整永遠好過重新排列。

CHAPTER 05 · VERTICAL CONTROLS

## 側邊控制列：iPhone 介面文法的新一頁

這章是整份指南的核心，也是設計師最需要重新練習的部分。外螢幕比其他 iPhone 都寬、都矮，所以系統把原本在螢幕頂端與底端的東西全部移到側邊：動態島、狀態列、含導覽按鈕的工具列、tab bar，站成一直行。內螢幕橫向時控制列也留在側邊，好讓人在兩塊螢幕之間切換時，控制列一直在同一個高度。唯一的例外是內螢幕直向，垂直空間夠，保留標準的橫向工具列。

![HIG 示意圖：iPhone Duo 外螢幕的動態島、狀態列、工具列與 tab bar 直排在右側邊緣](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/ededcddc-bf8f-4b37-ab74-e11873703866/5e388f2e-ec16-402d-9e73-087cff47c4fe.png)

HIG 示意圖：iPhone Duo 外螢幕的動態島、狀態列、工具列與 tab bar 直排在右側邊緣

▲ HIG 的示意圖：外螢幕的動態島、狀態列、工具列、tab bar 全部站在右側邊緣（圖：Apple HIG）

![iPhone Duo 內螢幕橫向顯示行事曆，月檢視與日檢視並排，工具列與 tab bar 沿右側邊緣直排](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/1ff1ddc8-4380-4ff5-a398-a9afc04c257b/75785eea-3cc3-43bd-bb9a-8e7865e2d35c.jpg)

iPhone Duo 內螢幕橫向顯示行事曆，月檢視與日檢視並排，工具列與 tab bar 沿右側邊緣直排

▲ 行事曆在內螢幕橫向：月檢視與日檢視並排，狀態列、tab bar 與新增按鈕沿右側邊緣直排（圖：Apple）

兩個 App 用 Split View 共用內螢幕時，各自把控制列放在自己的外側邊緣，左邊的 App 控制列在左、右邊的在右。控制列對齊的是硬體，它在外螢幕上永遠跟前鏡頭保持同一個相對位置，右到左的語言也不換邊。這代表版面的可用空間是不對稱的，safe area 要確實用上，包括對面那一側，因為 Split View 時對面可能是另一個 App 的控制列。

![Split View 示意圖：兩個 App 各自把控制列放在自己的外側邊緣](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/cee4b3b6-c05d-48dc-ae56-2e88bcc81201/92006e01-6236-4704-aba5-ae1b7f92af7d.png)

Split View 示意圖：兩個 App 各自把控制列放在自己的外側邊緣

▲ Split View 時兩個 App 各自把控制列放在自己的外側邊緣（圖：Apple HIG）

垂直軸上的排序，指南寫得很清楚。

1

最上面：主要導覽，Back 或 Close

保留垂直軸的頂端給導覽控制項，維持大家熟悉的導覽習慣。

2

接著：顯眼的主要動作，例如 Done

重要動作緊跟在導覽後面，手指不用跑遠。

3

其餘工具列項目，保持原本的分組

系統會在原本頂端與底端的項目之間留一段垂直空隙，讓兩組東西分得開。

4

放不下就溢出，預設由下往上收

用 visibility priority 改順序，先整組排、再排組內的個別項目。常用動作先保、帶 badge 的狀態項目留久一點。

幾條細規則會直接影響你怎麼畫元件。第一，帶文字的標籤會留在橫向列裡，所以按鈕能用 symbol 就用 symbol，但 title 還是要給，系統在溢出選單與展開形式裡會用它。第二，導覽為主的 App 把工具列項目收進溢出選單，讓 tab bar 與主要目的地留著；任務為主的 App 則反過來縮小 tab bar，保住完成任務要用的工具列動作，這跟其他 iPhone 上的 tab bar 縮小行為是同一套。第三，如果你的 App 本來就有自己的「更多」選單，把那些動作併進系統的溢出選單，省略號留給溢出，其他選單換別的 symbol。

滿版版面沒有被禁止。計算機就是整個寬度撐滿，只要別跟動態島與狀態列打架，沉浸式、不捲動的視覺介面都可以這樣做，也可以混用：背景圖或 header 滿版，可捲動的內容內縮。

![計算機在 iPhone 16 與 iPhone Duo 外螢幕上的畫面對照，滿版撐滿整個寬度](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/6d15e374-66aa-419a-a116-8e54b49fbcba/fc16e8e1-2d6c-41e4-b472-5037ffc8144c.png)

計算機在 iPhone 16 與 iPhone Duo 外螢幕上的畫面對照，滿版撐滿整個寬度

▲ 計算機在 iPhone 16（左）與 iPhone Duo 外螢幕（右）：滿版版面撐滿整個寬度（圖：Apple HIG）

“

控制列跟著硬體站，內容跟著空間走。

這句話是我讀完整份指南之後的濃縮。iPhone 十九年來的介面文法，第一次把「控制列在哪裡」交給硬體決定，把「內容長什麼樣」交給空間決定。指南自己的用詞是：控制列在垂直軸上的位置，是 iPhone Duo 的核心模式之一，把控制列放在熟悉的位置，人才能一拿到手就知道你的 App 怎麼用。

![](https://i.ytimg.com/vi/2y6xvya0b8M/hqdefault.jpg)

▲ Tech Talk〈Raise the bar with iPhone Duo〉，講側邊工具列與 tab bar

CHAPTER 06 · THE 1.42 PROBLEM

## 1:1.42 的代價：影片、遊戲與滿版畫面

接近 A4 的比例對文件、信件、閱讀、雙欄版面都是禮物，因為那就是一張紙對摺後的形狀。對 16:9 的內容則是麻煩。內螢幕橫放時長邊 890 pt，一支 16:9 的影片會被畫成 890×501 pt，上下總共剩 125 pt 的黑邊，差不多是螢幕的五分之一。影片 App 在這台機器上同時拿到更大的畫面和更大的黑邊，比例就是比例，沒有魔法可以消掉。

遊戲的規則寫得更直接。你可以鎖定直向或橫向，但姿勢改變時畫面要填滿螢幕；resize 的時候文字與控制項的大小盡量維持一致；能改比例就改比例，別用 letterbox 或 pillarbox；真的避不掉，就在留白區補上美術，讓體驗看起來仍然是滿版。這條規則對做遊戲 UI 的設計師而言，等於多了一張「邊框美術」的工作單。

CHAPTER 07 · IN PRACTICE

## 現在就能動手的三件事

模擬器要等 Xcode 27.1 beta，但有三件事今天就能做，而且都會在 10 月 23 日之前派上用場。

1

在 Figma 開兩個畫板：626×890 與 466×678

把你 App 最重要的五個畫面各放進去一次。466 寬會逼你面對「矮 196 pt」的問題，626 寬會逼你決定第二欄要露出什麼。tab bar 元件加一個 vertical 的 variant，按 Back／Close、Done、其餘項目的順序排。

2

下載官方邊框，照規範做 mockup

Design Resources 的 Product Bezels 區新增了 iPhone Duo，Photoshop 與 PNG 兩種格式，展開與闔起各兩色。PSD 用 Smart Object 換畫面，狀態列的訊號、Wi-Fi、電量選項就在圖層裡。蘋果的行銷規範要求產品圖原樣使用：不加倒影、陰影、高光，不裁切、不傾斜、不旋轉，文案放旁邊不壓在機身上，螢幕上最小 200 px，多台裝置同框維持真實比例，而且永遠用你的 App 支援的最新一代機型。

3

App Store 截圖從此準備兩套

闔起一套、展開一套。App Store Connect 的截圖規格還沒公布，先把兩種狀態的關鍵畫面做出來，規格出來的時候只剩套框。展開態的第一張截圖，請用雙欄版面證明你有為這塊螢幕做設計。

![Apple Design Resources 的 iPhone Duo 產品邊框素材，闔起狀態的機背與外螢幕](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/5bd98a7e-96cb-46ce-85f7-46ea44f894e2/084c6283-9d7f-4014-9aee-8520de3e5c36.png)

Apple Design Resources 的 iPhone Duo 產品邊框素材，闔起狀態的機背與外螢幕

▲ Apple Design Resources 的 iPhone Duo 邊框素材：闔起狀態的機背與外螢幕，外螢幕右上角是常駐的前鏡頭（圖：Apple）

官方邊框沒有 Figma 版，Figma 使用者的作法是把 PNG 拖進去當框，或者在 Photoshop 裡用 Smart Object 換畫面，跟 [包裝 mockup 的置入邏輯](https://rar.design/posts/photoshop-packaging-mockup-tutorial) 相同。另外提醒一個小陷阱：Design Resources 上的資料夾叫 iPhone 18，裡面裝的是 18 Pro 系列的邊框，標準版 iPhone 18 要到 2027 年初才會出。

![iPhone Duo 星光白色與夜空色，正面與背面各一](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/282990d4-733b-4481-8fcd-514f05688b62/a81c3056-115b-4baa-8236-7d67260a4700.jpg)

iPhone Duo 星光白色與夜空色，正面與背面各一

▲ 星光白色與夜空色，正反面各一，官方邊框素材也是這兩色（圖：Apple Newsroom）

CHAPTER 08 · TAKEAWAYS

## 開賣前的時間表與重點整理

TIMELINE · SEP—OCT 2026

9/9

發表會

HIG 新頁、六支 Tech Talk、規格頁、官方邊框同日上線

9/16–17

Group Labs

線上直接問蘋果的工程師與設計師

9/23

Forums Q&A

SwiftUI、UIKit、相機三場

9 月下旬

Xcode 27.1 beta

模擬器到手，驗證 pt 數與側邊控制列

10/16

台灣開放預訂

晚上 8 點，夜空色與星光白色

10/23

開賣

你的 App 出現在真機上的第一天

NOTE

依 Tech Talk 的說法，沒有用 iOS 27 SDK 編譯的 App，在展開的內螢幕上只會以原本的大小顯示；用 iOS 27 SDK 會延伸到狀態列旁邊；要撐到螢幕邊緣得用 iOS 27.1 SDK。arrangement view 這類新 API 也要等 iOS 27.1。設計稿可以先走，工程端的排程要對齊這個版本。

TAKEAWAYS · 一分鐘懶人包

→

兩組畫板：內螢幕 626×890 pt、外螢幕 466×678 pt（@3x 換算，等 Xcode 27.1 模擬器驗證）。外螢幕比 iPhone 18 Pro 寬 64 pt、矮 196 pt。

→

兩個 size class 撐所有姿勢：外螢幕 compact width、內螢幕 regular width，別為每種姿勢各畫一套。

→

三個保留區：動態島永遠在、相機區只在拍照時出現、摺疊區在半開時把內螢幕切成兩塊。格線用偶數欄。

→

側邊控制列的排序：Back／Close 最上、Done 其次、其餘保持原分組、溢出由下往上收；能用 symbol 就別用文字標籤。

→

內螢幕 1:1.42 接近 A4：文件與雙欄舒服，16:9 影片橫放會留五分之一的黑邊，遊戲寧可改比例也別 letterbox。

→

官方邊框只有 PSD 與 PNG，展開、闔起各兩色；行銷素材原樣使用、最小 200 px、永遠用最新機型。App Store 截圖準備兩套。

→

兩個日期：10 月 16 日晚上 8 點台灣開放預訂，10 月 23 日開賣；官網售價 NT$74,900 起。

回頭看這一整頁指南，蘋果做的事情很節制。它沒有替折疊機發明一套新的 iPhone 介面，它把既有的 size class、safe area、split view 和 tab bar 縮小行為全部搬過來，只在兩個地方動刀：多了保留區，控制列改站側邊。剩下的工作留給設計師，那就是把每一個畫面都放進 466 寬與 626 寬的框裡，看它會不會露餡。

從今天起，每個 iPhone 畫面都要能被摺成兩半。

RELATED READS · 延伸閱讀[WWDC 2026 發表了什麼？Liquid Glass 大修、Siri AI 登場與設計師重點解析](https://rar.design/posts/wwdc-2026-liquid-glass-siri-ai-recap)

[

iOS 27 的設計底層在六月就改過一輪，iPhone Duo 的指南是接在它後面的。

→](https://rar.design/posts/wwdc-2026-liquid-glass-siri-ai-recap)[

Figma Auto Layout 完整教學：從基礎到 Grid 新功能

兩個 size class 的畫面，最後都靠 resizing 設定撐起來。

→](https://rar.design/posts/figma-auto-layout-guide)[

Figma Slots 教學：設計系統終於可以靈活又不失控了

元件要能在 466 與 626 之間替換內容而不 detach，Slots 是現成的答案。

→](https://rar.design/posts/figma-slots-design-system)[

2026 UI/UX 設計趨勢全解析：10 個正在改寫介面的方向

折疊機的側邊控制列，會是今年趨勢清單裡最實體的一條。

→](https://rar.design/posts/ui-ux-design-trends-2026)[

7 個 UI/UX 設計原則完整解析：從格式塔到 80/20 法則

「只移動必要的東西」背後是格式塔的連續性原則。

→](https://rar.design/posts/7-ui-ux-design-principles)[

Figma Config 2026 發表了什麼？Code Layers、Motion 與六種材料上 canvas

今年設計工具端的大更新，跟蘋果這頁指南對照著看。

→](https://rar.design/posts/figma-config-2026-recap)[

《蘋果之道》讀後心得：蘋果 50 年給創作者的 5 個啟發

蘋果為什麼總是把規則寫得這麼細，書裡有答案。

→](https://rar.design/posts/apple-50-years-book-review)

EXTRA · LINKS 官方資源

[Human Interface Guidelines：Designing for iPhone Duo](https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo) 　本文所有版面規則的原始出處

[Get Ready for iPhone Duo](https://developer.apple.com/iphone-duo/) 　六支 Tech Talk、Group Labs 與 Q&A 時程

[Apple Design Resources：Product Bezels](https://developer.apple.com/design/resources/#product-bezels) 　iPhone Duo 與 iPhone 18 系列的官方邊框

[Marketing Resources and Identity Guidelines](https://developer.apple.com/app-store/marketing/guidelines/#section-products) 　產品圖原樣使用、最小尺寸與禁止事項

[iPhone Duo 技術規格（台灣官網）](https://www.apple.com/tw/iphone-duo/specs/) 　兩塊螢幕的解析度、ppi 與機身尺寸![100 種 Figma 設計的方法](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/bc8c4056-d9e6-443e-b7fd-6c5f75178b89/66d668a8-e692-4241-bfb0-e513f1319241.png)

100 種 Figma 設計的方法

[

COURSE

100 種 Figma 設計的方法

兩個 size class 的畫面，最後都靠 Auto Layout 的 resizing 與 variant 撐起來，這門課從零教起。

了解課程 →](https://rar.design/courses/figma)[![AI 覺醒設計應用攻略](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/63e96ac9-97ce-4dfc-be50-154f731f1cc9/e5ca45f0-206a-478f-af1a-96960be47d6a.jpg)

AI 覺醒設計應用攻略

SUBSCRIPTION

AI 覺醒設計應用攻略

AI 工具每週都在更新，我幫你過濾雜訊，只留對設計師真正有用的應用。

了解訂閱 →](https://rar.design/courses/aiwaken)

FAQ

## 常見問題

### Q：iPhone Duo 的螢幕尺寸與解析度是多少？

A：內螢幕 7.6 吋、1878×2670 px、430 ppi；外螢幕 5.4 吋、1398×2034 px、460 ppi。兩塊都是 Super Retina XDR OLED、ProMotion 最高 120Hz、常駐顯示，都有動態島。內螢幕長寬比約 1:1.42，接近 A4 紙的比例。

### Q：iPhone Duo 的 Figma 畫板要開多大？

A：以 @3x 換算，內螢幕 626×890 pt、外螢幕 466×678 pt。蘋果的規格頁只公布像素，pt 是把像素除以三的換算值，正式數字請等 Xcode 27.1 beta 的模擬器驗證。對照組：iPhone 18 Pro 是 402×874 pt。

### Q：iPhone Duo 的 tab bar 為什麼在側邊？

A：外螢幕比其他 iPhone 寬又矮，為了把垂直空間留給內容，系統把狀態列、動態島、工具列與 tab bar 全部移到側邊直排；內螢幕橫向時也維持在側邊，讓兩塊螢幕之間的體驗連續。只有內螢幕直向保留標準的橫向工具列。

### Q：什麼是 iPhone Duo 的保留區（reserved regions）？

A：內容要避開、元件要順應的螢幕區域，共三種：永遠存在的動態島區、只在相機啟動時出現的相機區、裝置半開時把內螢幕分成兩塊的摺疊區。大部分系統元件會自動避讓，自訂元件要用 reserved region API 處理。

### Q：現有的 iPhone App 在 iPhone Duo 上會壞掉嗎？

A：不會壞，但會浪費螢幕。依 Tech Talk 的說法，沒有用 iOS 27 SDK 編譯的 App 在展開的內螢幕上只會以原本的大小顯示，要撐到螢幕邊緣得用 iOS 27.1 SDK。用標準系統元件且支援 resize 的 App，只需少量調整就能適應各種姿勢。

### Q：iPhone Duo 什麼時候開賣、台灣售價多少？

A：台灣 10 月 16 日晚上 8 點開放預訂，10 月 23 日開賣，兩色：夜空色、星光白色。官網售價 NT$74,900 起，容量 256GB 到 2TB。

### Q：哪裡下載 iPhone Duo 的官方邊框（mockup）素材？

A：Apple Design Resources 頁面的 Product Bezels 區，提供 Photoshop 與 PNG 兩種格式，包含展開與闔起兩種狀態、兩種顏色。使用時要遵守蘋果的 Marketing Resources and Identity Guidelines：原樣使用、不加倒影陰影、不傾斜裁切、螢幕上最小 200 px。