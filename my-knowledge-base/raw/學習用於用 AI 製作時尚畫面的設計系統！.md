---
title: "學習用於用 AI 製作時尚畫面的設計系統！"
source: "https://codelove.tw/@tony/post/qJRXWq"
author:
published:
created: 2026-06-24
description: "Web網站的設計如果只跟 AI 或設計師說「請做得時尚一點」「請做得有質感一點」，最後完成的樣子往往會和想像差很多。因為 Web 設計其實包含了「要傳達哪些資訊、用什麼順序、什麼版面配置、什麼氛圍、什麼顏色、什麼字體、什麼元件、手機上怎麼呈現」等**多個面向**。這篇文章會把想用 AI 做出..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月23日](https://codelove.tw/@tony/post/qJRXWq "2026-06-23 17:05:49")

## 學習用於用 AI 製作時尚畫面的設計系統！

Web網站的設計如果只跟 AI 或設計師說「請做得時尚一點」「請做得有質感一點」，最後完成的樣子往往會和想像差很多。

因為 Web 設計其實包含了「要傳達哪些資訊、用什麼順序、什麼版面配置、什麼氛圍、什麼顏色、什麼字體、什麼元件、手機上怎麼呈現」等 **多個面向** 。

這篇文章會把想用 AI 做出有質感畫面的時候，應該先了解的 **設計系統** ，以及對 AI／設計師下指令的思考方式，整理成連設計新手也看得懂的內容。每一章也都附上可以直接拿去用的「指定範例（Prompt）」。

> 本文以公司內部的快速掌握會（AI 設計工具「Claude Design」的驗證）所製作的資料為基礎。對於從零開發的專案來說，因為沒有現成的設計系統，事先知道該怎麼對 AI 指示、該定義哪些內容，會直接影響品質。

[![ChatGPT Image 2026年4月26日 13_28_43.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2Fe4ae3058-119c-4072-8aa2-92827f641a04.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4798273107e1416d95a53dc5181a0055)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2Fe4ae3058-119c-4072-8aa2-92827f641a04.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=4798273107e1416d95a53dc5181a0055)

---

## 1\. Web 設計需求中該考慮的整體面向

在提出網站設計需求時，建議先把以下分類整理清楚再傳達：

1. 目的與目標族群
2. **資訊架構**
3. **頁面結構**
4. **版面配置**
5. **視覺風格**
6. **語氣與風格**
7. 色彩設計
8. **字體排版**
9. 元件設計
10. UI 狀態
11. 留白・圓角・陰影
12. 響應式設計
13. **無障礙設計**
14. **設計系統化**

以下會依序說明。

---

## 2\. 目的・目標族群

## 2-1. 目的是什麼

就是做這個網站的理由。即使都是「服務介紹網站」，只要目的不同，設計就會有很大的差異。

目的｜設計方向  
想增加服務註冊數｜讓 CTA 更醒目  
想讓人購買商品｜重視商品照片與評價  
想傳達企業的信賴感｜採用沉穩配色與整齊版面  
想增加徵才應徵｜傳達員工氛圍與工作方式  
想傳達品牌世界觀｜重視照片、留白、文字語氣

## 2-2. 目標族群是什麼

也就是這個網站是做給誰看的。

- 20 多歲後半的女性
- 中小企業經營者
- 不太懂 IT 的自由工作者
- 正在考慮應徵工作的學生
- 育有小孩的 30 多歲夫妻
- 正在評估導入 SaaS 的企業承辦人員

## 2-3. 為什麼目標族群很重要

因為不同族群喜歡的設計不一樣。像同樣是「健康管理 App」：

- 給高齡者：字要大、操作要簡單
- 給年輕女性：重視柔和色彩與親切感
- 給企業用戶：重視信賴感與資料的可讀性

因此設計方向會完全不同。

## 2-4. 給 AI／設計師的指定範例

```
這個網站的目的，是清楚傳達服務內容，並引導使用者完成免費註冊。

目標族群是 20 多歲後半到 30 多歲前半的女性，平常在工作或日常生活中有些疲憊，
正在尋找新的興趣或想轉換心情的人。

因此，請避免帶有強烈推銷感的設計，改成溫柔貼心、像是在陪伴使用者的印象。
```

---

## 3\. 資訊架構（Information Architecture / IA）

## 3-1. 什麼是資訊架構

就是決定「要放什麼、先放什麼、怎麼呈現」。網站不是把資訊單純排上去就好，而是要依照使用者能自然理解的順序來呈現。

## 3-2. 資訊架構要思考的事

- 一開始先傳達什麼
- 在哪裡消除使用者的不安
- 以什麼順序傳達服務的魅力
- 在哪個位置引導註冊、購買、詢問等行動
- 詳細資訊要放到什麼程度／FAQ 要放在哪裡

## 3-3. 良好的資訊架構範例

[![ChatGPT Image 2026年4月26日 14_01_02.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2Fd6389d76-92a5-47ab-8f7c-66c548d2f2d0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f5df73e77e1ceab1fb02d9d8bfb8546d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2Fd6389d76-92a5-47ab-8f7c-66c548d2f2d0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f5df73e77e1ceab1fb02d9d8bfb8546d)

如果是服務介紹網站，通常會使用以下流程：

1. **首屏（Hero Section）** … 傳達服務最重要的價值
2. **問題提出** … 共感使用者正在面對的困擾
3. **解決方案** … 說明這個服務能做什麼
4. **特色** … 解釋和其他服務的差異與強項
5. **使用步驟** … 簡單說明使用方式
6. **實績・評價** … 提高信賴感
7. **價格** … 提供判斷依據
8. **FAQ** … 消除疑慮
9. **CTA** … 引導註冊、詢問等行動

## 3-4. 不良資訊架構範例

- 一開始就塞太多細節功能說明
- 還不知道是什麼服務就先出現價格
- 看不出 CTA 在哪裡
- 類似資訊重複出現很多次
- 沒有碰到使用者的痛點，只放提供方想講的內容

## 3-5. 給 AI／設計師的指定範例

```
請將資訊架構設計為以下流程。

1. 在首屏用一句話傳達服務價值
2. 共感使用者正在面臨的問題
3. 說明服務如何解決這些問題
4. 介紹 3 個主要特色
5. 用 3 個步驟說明使用流程
6. 放上使用者心得
7. 用 FAQ 消除疑慮
8. 最後配置免費註冊的 CTA
```

---

## 4\. 頁面結構

## 4-1. 什麼是頁面結構

就是把 Web 頁面分成哪些區塊來組成。若資訊架構是「資訊的流向」，那頁面結構就是把那個流向實際切成畫面區塊。

## 4-2. 常用區塊

區塊名稱｜角色  
Header｜顯示 Logo 與選單  
Hero Section｜在頁面最上方傳達最重要的訴求  
Problem Section｜提出使用者的困擾  
Solution Section｜提出解決方案  
Feature Section｜介紹服務特色  
Step Section｜說明使用方式與流程  
Testimonial Section｜顯示評價或使用者心得  
Pricing Section｜顯示價格方案  
FAQ Section｜顯示常見問題  
CTA Section｜引導註冊、詢問等行動  
Footer｜顯示公司資訊與連結

## 4-3. 什麼是 Hero Section

就是打開網站後第一眼看到的大區塊。如果在這裡看不出「這是什麼網站」「可以做什麼」，使用者很容易離開。

常見內容：主標語 / 副標語 / 主視覺 / CTA 按鈕 / 補充說明文字 / 實績或信賴資訊

```
主標語：在每天生活中，找到一個有點開心的興趣吧。
副標語：依照你的心情、個性與現在所在的地方，推薦最適合你的興趣與出遊提案。
CTA：免費開始
```

## 4-4. 什麼是 CTA

CTA 是 **Call To Action** 的縮寫，指的是引導使用者採取行動的按鈕或連結。

例如：免費開始／索取資料／聯絡我們／下載 App／預約諮詢／查看詳情

## 4-5. 給 AI／設計師的指定範例

```
請依以下頁面結構設計。

- Header / Hero / Problem / Solution / Feature / Step / Testimonial / FAQ / CTA / Footer

在 Hero Section 中，請配置主標語、副標語、CTA 按鈕，
以及能傳達服務氛圍的視覺元素。
```

---

## 5\. 版面配置

## 5-1. 什麼是版面配置

就是畫面中的元素要怎麼擺放。即使內容一樣，因為版面不同，可讀性與印象就會有很大差異。

## 5-2. 版面配置常用術語

術語｜意思  
欄（Column）｜垂直方向的一列  
1 欄｜以滿版寬度垂直堆疊顯示  
2 欄｜左右兩列顯示  
3 欄｜三列排列卡片等內容  
Grid｜規則排列元素的基準  
12 欄 Grid｜Web 常用的 12 欄設計  
Container 寬度｜內容的最大寬度  
Section｜頁面中的大區塊  
首屏｜打開頁面後最先看到的範圍

## 5-3. 代表性的版面配置

- **1 欄** … 以縱向排列資訊的簡潔形式。很適合手機，常用於 LP、部落格、服務介紹、徵才頁面。
- **2 欄** … 把元素左右分開的形式（例如左文字＋右圖片）。容易把資訊與視覺一起呈現，適合比較與說明。
- **卡片版面** … 把資訊放進一個個方塊中排列。適合功能介紹、價格、評價、列表顯示。
- **Bento UI** … 以大小不一的卡片在格狀中排列，像便當盒一樣整理多個資訊。是現代感很強的版面，常見於 SaaS、AI 服務，也容易做出資訊輕重層次。

## 5-4. 給 AI／設計師的指定範例

```
為了讓手機上也容易閱讀，基本請採用 1 欄配置。
在 PC 上，只有部分區塊改成 2 欄，讓版面更有節奏感。

特色介紹請使用卡片版面（圖示・標題・說明文字／PC 3 欄・手機 1 欄），
功能介紹請用 Bento UI，以大卡片整理重要功能、小卡片整理補充功能。
```

---

## 6\. 視覺風格

## 6-1. 什麼是視覺風格

指的是 UI 外觀的流派與表現方法。相較於語氣與風格是「想呈現什麼印象」，視覺風格則是「用什麼樣的外觀方式來做」。

同樣是「親切的網站」，使用 Flat、Material、Glassmorphism、Bento UI、Editorial 等不同風格，方向就會改變。

## 6-2. 視覺風格一覽

[![ChatGPT Image 2026年4月26日 13_35_12.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2F4bc5ce45-8684-483a-b08b-8df55c0acb84.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=83698bf4c749953d631e74d82824397b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2F4bc5ce45-8684-483a-b08b-8df55c0acb84.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=83698bf4c749953d631e74d82824397b)

[![ChatGPT Image 2026年4月26日 13_46_29.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2Fb87e1340-c5ea-42f5-90d7-929979d6b43c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ca0866c506ea077413e5f1ff989a5ed6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2Fb87e1340-c5ea-42f5-90d7-929979d6b43c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ca0866c506ea077413e5f1ff989a5ed6)

風格名稱｜概要｜主要特徵｜適合用途｜注意事項  
Flat Design｜降低陰影與立體感的平面簡約設計｜裝飾少、純色塊、資訊看起來整齊｜LP、SaaS、App 介紹、管理介面｜太簡單時容易不留下印象，CTA 要用顏色與留白明確表達  
Material Design｜Google 提倡，以紙張層疊與陰影表現階層｜卡片 UI、陰影、層次感、狀態變化清楚｜Android App、Web App、管理介面｜過多會顯得厚重，也可能失去獨特性  
Skeuomorphism｜模仿現實物體的質感｜皮革、紙、金屬等真實質感、明顯陰影｜復古風、遊戲 UI、體驗型內容｜容易顯得老派，使用過度要注意  
Neumorphism｜以相同色系背景與柔和陰影做出浮起感｜淡色背景、相近色、柔和陰影、蓬鬆感｜健康、日記 App、實驗性 UI｜對比低，無障礙較弱，是否可點擊不易辨認  
Glassmorphism｜像磨砂玻璃的半透明面板｜半透明、背景模糊、細邊框、透明感｜AI 服務、創意型 LP、儀表板｜視背景而定可能降低可讀性，要注意文字清晰度  
Minimal Design｜極簡化裝飾，用留白、文字、照片來呈現｜留白大、色數少、洗練、優雅｜品牌網站、美容、顧問、作品集｜資訊量多時整理會變難  
Brutalism｜刻意粗獷、原始感很強的個性化表現｜粗線條、大字、強烈對比、實驗性｜藝術、音樂、時尚、創作者｜不適合一般大眾市場，風格很強烈  
Neo Brutalism｜把 Brutalism 現代化、帶點流行感｜粗黑框、明亮色、實體陰影、趣味感｜年輕族群、個人開發、教育、新創 LP｜太過活潑可能顯得便宜，不適合強調高級感  
Claymorphism｜像黏土般圓潤柔軟的立體感｜大圓角、柔和陰影、粉彩色｜女性向、兒童向、興趣 App、健康類｜過度使用會顯得幼齡化  
Editorial Design｜像雜誌一樣用照片、文字、留白美化呈現｜大標題、大圖、留白、閱讀感｜媒體、旅遊、美容、時尚、品牌｜資訊太多時會不好讀，CTA 不可太弱  
Corporate Design｜重視企業網站式的信賴感與整潔感｜白底、整齊 Grid、海軍藍／藍色、低調｜BtoB、企業官網、專業服務、顧問、徵才｜容易流於普通，要用照片、文案、強調色做差異化  
Luxury Design｜重視高級感與質感｜大留白、少色彩、黑／白／金／米色、細線條｜美容、飯店、珠寶、高單價服務｜文字容易太小、資訊過少  
Pop Design｜明亮、愉快、親切｜高彩度、圓角、插圖、大按鈕｜活動、教育、娛樂、年輕族群與家庭｜太花會顯得廉價，重點層級要清楚  
Kawaii UI｜呈現日式「可愛感」｜粉彩、圓潤、小圖示、手寫風｜女性向、興趣、日記、情緒照護｜容易太幼稚，大人向要降低彩度  
Retro Design｜加入懷舊感與舊時代氛圍｜霧面色、奶油色、粗糙紋理｜咖啡廳、雜貨、音樂、活動、興趣類｜操作方式還是要維持現代感  
Y2K Design｜2000 年代風格的數位感與光澤感｜金屬感、亮片、強烈漸層、粉紅／銀色｜時尚、音樂、SNS、年輕族群｜潮流感強但容易退燒  
Cyberpunk｜近未來、夜色、霓虹、科技感｜黑底、霓虹色、故障風、紫／藍／粉｜遊戲、AI、Web3、科技活動｜可讀性容易下降，不適合需要安心感的服務  
Dark UI｜以深色背景為主的設計｜黑／深藍／炭灰、白字、重點色突出｜AI、開發者工具、音樂、影像、儀表板｜對比要調整好，長文閱讀容易疲勞  
Wamodern｜把和風元素現代化整理｜米白、墨黑、藍色、留白、細線、和紙質感｜旅館、和食、傳統工藝、地方文化｜太偏和風會顯得老派  
Organic Design｜自然、手感、柔和｜大地色、曲線、不規則形、自然照片｜健康、食品、自然體驗、女性向｜太樸素時會失去洗練感  
Isometric｜使用斜上方立體插圖｜3D 風插圖、容易說明機制、很有 SaaS 感｜SaaS、BtoB、FinTech、效率工具｜品質會很吃插圖水準  
3D UI｜立體物件與圖示｜立體感、深度、華麗、先進｜AI、SaaS、App LP、新穎服務｜載入成本與實作成本都較高  
Illustration-focused｜以插圖為主來增加親切感｜柔和、情緒表現容易、品牌感強｜教育、初學者向、健康照護、家庭｜風格不一致會失去統一感  
Photo-focused｜大幅使用照片來傳達世界觀與情感｜大圖、留白、短文案、情感訴求｜旅遊、美容、活動、餐飲、時尚｜很吃照片品質，也要注意文字可讀性  
Dashboard UI｜專注於資料與管理介面｜側欄、卡片、表格、圖表、篩選器｜管理介面、分析工具、SaaS、CRM｜資訊密度高，需要清楚的優先順序與視線引導  
Bento UI｜以大小不同的卡片在格狀中整理資訊｜卡片格局、大小強弱、現代、Apple／SaaS 感｜AI 服務、SaaS、App LP、功能介紹｜卡片太多會單調，要用大小差異凸顯重點

## 6-3. 風格可以組合指定

視覺風格不一定只能選一種。實際上更常見的是把幾種風格組合起來指定。

組合｜印象｜適合用途  
Minimal × Bento UI｜洗練且有整理感｜AI 服務、SaaS、App LP  
Flat × Corporate｜清爽且有信賴感｜BtoB、企業官網、專業服務  
Glassmorphism × Dark UI｜有未來感且先進｜AI、Web3、創意工具  
Editorial × Photo-focused｜像雜誌般傳達世界觀｜旅遊、美容、生活風格  
Claymorphism × Kawaii UI｜柔和親切｜女性向、興趣、日記、健康照護  
Organic × Minimal｜自然且平靜｜健康、食品、生活風格  
Neo Brutalism × Pop｜有趣且容易留下印象｜年輕族群、教育、個人開發服務  
Luxury × Minimal｜有質感且高級｜美容、飯店、品牌網站

## 6-4. 給 AI／設計師的指定範例

```
視覺風格請以 Minimal × Bento UI × 卡片式 UI 為基礎。

請保留寬鬆留白，並用大小不同的卡片整理資訊。
裝飾要克制，透過色彩、字體與留白營造質感。
整體請兼具現代、整齊與親切感。
```

---

## 7\. 語氣與風格

## 7-1. 什麼是語氣與風格

就是網站整體的氛圍與印象規則，簡稱「Tonmana」。即使都是粉色系網站，只要是「可愛／優雅／活潑／成熟／溫柔／高級感」，印象就會完全不同。

## 7-2. 常用的語氣詞彙

[![ChatGPT Image 2026年4月26日 13_46_29.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2Fdb2cebf0-d63d-4cc5-ae6f-7f73089e3964.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5aeda3450675e5233937fba9d70bf92f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4261508%2Fdb2cebf0-d63d-4cc5-ae6f-7f73089e3964.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5aeda3450675e5233937fba9d70bf92f)

語氣｜印象  
Modern｜有現代感、洗練  
Natural｜自然、柔和  
Feminine｜女性化、柔軟  
Clean｜清爽、有潔淨感  
Premium｜上質、有高級感  
Friendly｜親切、易接近  
Pop｜明亮、快樂  
Chic｜沉穩、大人感  
Casual｜輕鬆、親近  
Corporate｜有信賴感、穩健  
Emotional｜能打動情感  
Stylish｜帥氣、洗練

## 7-3. 語氣可以組合，也要傳達想避免的印象

語氣多組合會更具體，例如： `Natural × Premium × Friendly` 、 `Modern × Clean × Trustworthy` 。

不只要說想要的印象， **也要說不要的印象** ，可以大幅減少落差。

## 7-4. 給 AI／設計師的指定範例

```
請將語氣與風格設定為自然、優雅，並帶有親切感。

雖然是女性向，但請避免過甜或太像小朋友的設計。
請保留寬鬆留白，採用沉穩配色，做出讓人安心的網站。
```

---

## 8\. 色彩設計

## 8-1. 什麼是色彩設計

就是決定網站內每種顏色的用途。顏色不是「隨便選自己喜歡的色」，而是依照用途來決定，設計才會穩定。

## 8-2. 主要顏色的功能

顏色種類｜用途  
Primary Color｜網站的主色  
Secondary Color｜輔助主色  
Accent Color｜用在 CTA 或重要區塊的強調色  
Background Color｜背景色  
Surface Color｜卡片或面板的背景色  
Text Color｜內文文字色  
Muted Text Color｜補充說明等較淡的文字色  
Border Color｜邊框顏色  
Error / Success / Warning Color｜錯誤／成功／注意狀態顏色

## 8-3. 色彩指定的具體範例（女性向生活風格）

```
Primary Color：#E8856A  鮭魚粉。帶有溫度與親切感。
Secondary Color：#FAF5EE  象牙白。柔和且沉穩的背景色。
Accent Color：#A8C5A0  霧感綠。帶來自然感與安心感的點綴。
Text Color：#2E2420  深棕色。比黑色更柔和。
Muted Text Color：#7A6A62  用於補充說明的柔和棕色。
Border Color：rgba(232, 133, 106, 0.2)  淡淡的鮭魚色邊框。
```

## 8-4. 給 AI／設計師的指定範例

```
請以帶有溫度的鮭魚粉、象牙白、霧感綠作為主要配色。

請避免黑白對比過強的配色，內文請使用深棕色系。
CTA 按鈕請使用 Primary Color，背景請以象牙白系營造柔和印象。
```

---

## 9\. 字體排版

## 9-1. 什麼是字體排版

就是字體外觀與閱讀性的設計。再漂亮的設計，如果字不好讀，網站就會變得不好用。

要考慮的項目：Font Family（字型）／Font Size／Font Weight（粗細）／Line Height（行高）／Letter Spacing（字距）／Heading（標題）／Body（內文）／Caption（補充文字）／Display（大標題）

## 9-2. 日文網站的注意事項

日文比英文文字量多，容易看起來比較擁擠，因此：

- 內文字級不要太小
- 行高要拉大／字重要做區分
- 長文不要放得太寬／標題與內文的差異要明確

## 9-3. 文字大小・字型範例

```
H1：40px / Bold / line-height 1.3
H2：28px / Bold / line-height 1.4
H3：22px / Bold / line-height 1.5
Body：16px / Regular / line-height 1.8
Caption：13px / Regular / line-height 1.6
Button：15px / Bold
```

日文常用字型：Noto Sans JP / Hiragino Sans / Yu Gothic / Zen Kaku Gothic New / M PLUS Rounded 1c / Noto Serif JP

字體分類｜印象  
哥德體｜好讀、現代、通用  
明體｜優雅、知性、和風、高級感  
圓體｜溫柔、可愛、親切  
粗哥德體｜有力、活潑、醒目

## 9-4. 給 AI／設計師的指定範例

```
請重視日文的可讀性來設計字體排版。

標題請稍微加粗，營造印象；內文請使用 16px 以上，行高設為 1.7～1.8 左右。
字型以 Noto Sans JP 或 Hiragino Sans 為前提，營造柔和親切的印象。
```

---

## 10\. 元件設計

## 10-1. 什麼是元件

就是組成網站的零件，例如按鈕、卡片、輸入表單、Header、Footer、分頁、Accordion 等。網站就是由這些元件組合而成。

如果先定義好，整個網站會更有一致性。若每個頁面的按鈕樣式都不一樣，使用者就會不知道哪個按鈕最重要。

## 10-2. 常用元件

元件｜用途  
Button｜引導使用者行動  
Card｜把資訊作為一個單位呈現  
Badge｜小型標籤或狀態顯示  
Form｜接收使用者輸入  
Header / Footer｜導覽／補充連結與公司資訊  
Modal｜彈出式視窗  
Accordion｜可展開／收合，例如 FAQ  
Tab｜切換不同資訊  
Alert / Toast｜注意、成功、錯誤／暫時通知  
Navigation｜頁面移動導引  
Pricing Card / Testimonial Card｜價格方案／使用者心得

## 10-3. Button・Card 的設計範例

```
Primary Button：最重要的行動（免費開始、購買、聯絡我們）
Secondary Button：次要行動（查看詳情、查看資料）
Text Button：較輕的導引（查看更多、跳過）

Feature Card：圖示 / 標題 / 說明文字
Event Card：圖片 / 標題 / 日期時間 / 地點 / 標籤 / 詳細按鈕
Testimonial Card：使用者名稱 / 評論 / 星等 / 屬性
```

## 10-4. 給 AI／設計師的指定範例

```
請設計以下元件。

- Primary Button / Secondary Button / Feature Card / Testimonial Card
- FAQ Accordion / Header Navigation / Footer / Form Input / Badge

請為每個元件定義外觀、間距、圓角、顏色與狀態。
```

---

## 11\. UI 狀態

## 11-1. 什麼是 UI 狀態

指的是按鈕、輸入欄等元件，會因為使用者操作或狀況不同而改變的外觀。如果沒有定義 UI 狀態，使用者會不知道怎麼操作。

狀態｜意思  
Default｜一般狀態  
Hover｜滑鼠移上去  
Active｜按下中  
Focus｜被鍵盤操作選取中  
Disabled｜不可點擊  
Loading｜載入中  
Error / Success｜錯誤／成功  
Empty State｜沒有資料時的狀態

## 11-2. 按鈕・表單狀態範例

```
[Button]
Default：一般顯示 ／ Hover：顏色加深、陰影變強 ／ Active：看起來像被按下
Disabled：顏色變淡，表示無法點擊 ／ Loading：顯示轉圈圖示，避免重複送出

[Form]
Focus：邊框改成 Primary Color，表示正在輸入
Error：邊框改成紅色系並顯示錯誤訊息
Success：顯示勾選圖示等 ／ Disabled：以淡背景色表示不可輸入
```

## 11-3. 給 AI／設計師的指定範例

```
請為按鈕、表單與卡片定義 Default、Hover、Active、Focus、Disabled、Loading、Error 等狀態。
能讓使用者操作的元素，請清楚表現出可以被操作。
```

---

## 12\. 留白・圓角・陰影

## 12-1. 留白

就是元素彼此之間的間距。適當的話資訊會更清楚，太少就會顯得雜亂。

術語｜意思  
Padding｜元素內側的留白  
Margin｜元素外側的留白  
Section Spacing｜區塊與區塊之間的間距  
Gap｜Grid 或卡片之間的間距

## 12-2. 圓角（border-radius）

圓角｜印象  
0px｜硬、俐落、粗獷  
4px｜克制、偏商務  
8px｜標準、通用  
16px｜柔和、親切  
24px 以上｜可愛、App 感、溫和

## 12-3. 陰影（box-shadow）

陰影強度｜印象  
無陰影｜扁平、簡潔  
淡陰影｜優雅、自然、輕盈  
強陰影｜立體、強調、稍微搶眼  
實心陰影｜Neo Brutalism 風、活潑感

## 12-4. 給 AI／設計師的指定範例

```
請整體保留較大的留白，讓版面沒有壓迫感。

卡片請設定 24px padding、16px 圓角、淡陰影。
區塊之間的間距在 PC 為 80px、手機為 48px，卡片間 gap 為 24px。
按鈕請做成圓潤的膠囊型，營造親切感。
```

---

## 13\. 響應式設計

## 13-1. 什麼是響應式設計

就是依照 PC、平板、手機等不同螢幕大小，調整呈現方式的設計。因為現在大多數情況都是在手機上觀看，所以響應式對應是必要的。

要思考的事：PC 橫向排列、手機縱向堆疊／手機字級稍微縮小／按鈕要夠大方便手指點擊／選單改成漢堡選單／圖片不要溢出／不要出現橫向捲動

## 13-2. 螢幕寬度的思考方式

```
Mobile：〜767px
Tablet：768px〜1023px
Desktop：1024px 以上
Large Desktop：1280px 以上
```

## 13-3. 給 AI／設計師的指定範例

```
請以響應式設計為前提。

PC 請採用最大寬度 1200px 的置中版面，並用 2 欄或 3 欄整理資訊。
手機請收斂成 1 欄，按鈕請做成方便手指點擊的高度。
卡片請縱向堆疊，避免出現橫向捲動。
```

---

## 14\. 無障礙設計

## 14-1. 什麼是無障礙設計

就是讓所有人都容易使用的網站設計概念（特別是 BtoC 網站很重要）。也包含視力較弱的人、分不清顏色的人、只用鍵盤操作的人、高齡者、使用朗讀功能的人等。

要思考的事：文字與背景要有足夠對比／不要把字做得太小／按鈕要夠好按／ **不要只靠顏色傳達資訊** ／表單錯誤要容易理解／可用鍵盤操作／圖片要加替代文字

## 14-2. 不良範例與改善範例

```
[不良範例] 只用紅色框線表示錯誤
→ 對色覺辨識不佳的人傳達不了，也看不出哪裡有錯

[改善範例] 除了紅色框線外，另外顯示具體錯誤訊息
例如：電子郵件地址格式不正確。
```

## 14-3. 給 AI／設計師的指定範例

```
請考量無障礙設計。

請確保文字與背景有足夠對比，內文字級請使用 16px 以上。
錯誤顯示不要只靠顏色，請同時顯示具體訊息。
按鈕請設計成手機上也方便點擊的大小。
```

---

## 15\. 設計系統

## 15-1. 什麼是設計系統

就是把整個網站或 App 會用到的設計規則整理起來（顏色、字體、留白、圓角、陰影、按鈕、卡片、表單、狀態、版面規則等）。

優點：

- 設計不容易跑掉
- 比較容易實作／修改
- 團隊共享方便
- **讓 AI 之後再生其他畫面時也更有一致性**

## 15-2. 什麼是 Design Token

把顏色、留白、字級等設計值 **定義成變數** 。

```
--color-primary: #E8856A;
--color-background: #FAF5EE;
--color-text: #2E2420;
--radius-card: 16px;
--space-md: 16px;
--font-body: 16px;
```

## 15-3. 給 AI／設計師的指定範例

```
請將顏色、留白、圓角、陰影、字級定義為 Design Token。
請把按鈕、卡片、表單等主要元件整理成可重複使用的設計規則。
```

---

## 16\. 可直接使用的「AI 設計需求模板」

```
請提案 Web 網站的設計。

## 對象服務
服務名稱：
服務概要：

## 目標族群
年齡層／性別／使用情境／正在困擾的問題：

## 網站目的
例如：讓人理解服務內容／完成免費註冊／增加詢問／傳達品牌世界觀

## 希望的印象
例如：自然／優雅／親切／現代／有信賴感

## 不希望的印象
例如：太像小朋友／太花／太廉價／資訊雜亂

## 希望的視覺風格
例如：Minimal／Bento UI／Flat Design／Editorial／卡片式 UI

## 希望提案的內容（請依以下觀點整理）
1. 資訊架構  2. 頁面結構  3. 版面配置  4. 視覺風格
5. 語氣與風格  6. 色彩配置  7. 字體排版  8. 元件
9. UI 狀態  10. 留白・圓角・陰影  11. 響應式方針  12. 無障礙設計  13. Design Token
```

---

## 17\. 依目的別的推薦指定範例

**SaaS・業務系系統**

```
請以 BtoB SaaS 的網站為前提，重視信賴感、效率與易懂性。
視覺風格以 Corporate × Flat Design × Bento UI 為基礎。
以白底、海軍藍、藍色系強調色營造資訊整理良好的印象。
功能介紹請用卡片式 UI 整理，導入效益請用數字與圖示清楚表達。
```

**美容・高級服務系**

```
請做成適合美容服務的網站，呈現優雅且有高級感的印象。
視覺風格以 Minimal × Luxury × Photo-focused 為主。
請保留寬鬆留白、減少色數，透過細線條與優雅字體營造質感。
請大量使用照片，文字則簡短而有印象。
```

**徵才網站系**

```
請以徵才網站為前提，讓公司氛圍、員工魅力與安心感能被傳達。
視覺風格以 Corporate × Editorial × Photo-focused 為基礎。
請放大員工照片與訪談內容，並整理工作內容、企業文化、福利、招募職缺的導引。
```

**活動・活動促銷頁系**

```
請以活動 LP 為前提，重視明亮且有趣的印象。
視覺風格以 Pop × 卡片式 UI × Illustration-focused 為主。
請讓活動日期、地點、參加好處與報名 CTA 一眼就看懂。
色彩請明亮，標題請放大，讓手機上也能快速傳達資訊。
```

---

## 18\. 需求時應避免的模糊表達

❌ 模糊的需求

```
請做得有感覺一點。 / 請做得時尚一點。 / 請做得有現代感一點。
請做成女性向的感覺。 / 請做得容易看。
```

✅ 改善後的需求

```
請以 20 多歲後半女性為對象，做成自然、優雅、親切的印象。
請避免太可愛或太像小朋友的表現，保留寬鬆留白，
以鮭魚粉和象牙白為主的柔和配色來呈現。
UI 請以卡片式為主，圓角做大一點，陰影做淡一點。
```

---

## 19\. 總結

在請人設計網站時，不要只說「請做得好看一點」，而是依照以下分類整理，精準度會提高很多。即使是請 AI 設計也是一樣， **知道「該定義什麼」本身就是設計系統的一部分** 。

1. **目的・目標族群** … 要對誰傳達什麼、希望對方做什麼
2. **資訊架構** … 要用什麼順序呈現資訊
3. **頁面結構** … 要用哪些區塊組成頁面
4. **版面配置** … 元素要怎麼擺
5. **視覺風格** … Flat、Material、Minimal、Bento UI 等外觀流派
6. **語氣與風格** … Natural、優雅、親切等整體印象
7. **色彩設計** … 主色、背景色、文字色、強調色
8. **字體排版** … 字型、字級、粗細、行高
9. **元件設計** … 按鈕、卡片、表單、FAQ 等 UI 零件
10. **UI 狀態** … Hover、Disabled、Loading、Error 等狀態
11. **留白・圓角・陰影** … 間距、柔和感、立體感的調整
12. **響應式設計** … PC 與手機的呈現方式
13. **無障礙設計** … 讓所有人都容易使用
14. **設計系統** … 把顏色與元件規則化，方便重複使用

如果把這些當成模板提供給 AI，就能更高再現性地做出「有質感的畫面」。

---

原文出處： [https://qiita.com/yusuke\_ando\_vj/items/dd17a285217a15841a3a](https://qiita.com/yusuke_ando_vj/items/dd17a285217a15841a3a)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qJRXWq) [19瀏覽](https://codelove.tw/@tony/post/qJRXWq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qJRXWq)

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

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝16 💬1 ❤️1

530

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

38

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782264318&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRXWq&amp;pra=5&amp;asro=0&amp;itsi=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782264318127&amp;bpp=1&amp;bdt=460&amp;idt=85&amp;shv=r20260623&amp;mjsv=m202606230101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782264292%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782264292%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782264292%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=7569525097890&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099344%2C31099321%2C95394755&amp;oid=2&amp;pvsid=1660024353057853&amp;tmod=207890853&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=106" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="320" height="50" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260623/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=2&amp;uci=a!2&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>