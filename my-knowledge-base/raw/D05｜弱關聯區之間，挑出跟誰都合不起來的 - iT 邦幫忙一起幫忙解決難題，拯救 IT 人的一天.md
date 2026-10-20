---
title: "D05｜弱關聯區之間，挑出跟誰都合不起來的 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10406929"
author:
  - "[[iThome]]"
published:
created: 2026-09-11
description: "Petrič 這群人手上有一個現成的戰績。2009 年他們掃自閉症的文獻，撈出一個當時完全沒人正式研究過的東西：鈣調磷酸酶（calcineurin）。自閉症的論文和鈣調磷酸酶的論文各自堆成一座山、互不..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 5

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 怎麼找到新東西？把 200 年的「發現」老方法教給 AI——方法圖鑑 × Claude Skills系列 第 5 篇

## D05｜弱關聯區之間，挑出跟誰都合不起來的

- 分享至

Petrič 這群人手上有一個現成的戰績。2009 年他們掃自閉症的文獻，撈出一個當時完全沒人正式研究過的東西：鈣調磷酸酶（calcineurin）。自閉症的論文和鈣調磷酸酶的論文各自堆成一座山、互不引用，接點就在兩座山中間沒人站的地方。

2012 年他們回頭問：那個接點當初到底躲在哪一種文件裡？把自閉症×鈣調磷酸酶、加上 Swanson 那個經典的偏頭痛×鎂，兩組資料各跑一次，答案一致——橋接概念集中在 outlier 文件裡：主題邊緣、擺在自己領域裡格格不入、丟給分類器還會被判去隔壁領域的那種。這批文件只佔整個資料集的一小撮，卻是橋接概念濃度最高的地方。結論很實際：專家不必讀完整個領域，只要讀那一小撮跟自己領域合不起來的，就能撈到大部分的跨域線索。熱門論文人人都讀過，裡面能接的線早被接光了。

那條線後來呢？沒有藥。這個猜測發表之後就停在假說階段，沒有人接著做實驗去驗。真正的回音是八年後從完全不相干的方向來的——2017、2018 年幾組人做外顯子定序，發現 PPP3CA（編碼鈣調磷酸酶催化次單元的那個基因）的新生突變會造成嚴重的神經發育疾病，其中一型病人身上，自閉症特徵是常見表現。沒有人是因為讀了那篇文獻探勘論文才去定序的，兩邊各走各的路，最後撞在同一個分子上。所以這套方法交出來的是線索，不是結論，更不是藥——它的價值是讓你比別人早八年開始看那個方向。

昨天的地圖上，有些衍伸主題抱團成群，有些孤零零站在外面。如果平均用力去查，力氣一定花在被講到爛的地方。先篩一次，把最怪、最合不起來的挑出來，才划算。

> 出處：Petrič, I., Cestnik, B., Lavrač, N., & Urbančič, T. (2012). Outlier Detection in Cross-Context Link Discovery for Creative Literature Mining. *The Computer Journal*, 55(1), 47–61. [https://doi.org/10.1093/comjnl/bxq074](https://doi.org/10.1093/comjnl/bxq074)  
> 　　　Petrič, I., Urbančič, T., Cestnik, B., & Macedoni-Lukšič, M. (2009). Literature mining method RaJoLink for uncovering relations between biomedical concepts. *Journal of Biomedical Informatics*, 42(2), 219–227. [https://doi.org/10.1016/j.jbi.2008.08.004](https://doi.org/10.1016/j.jbi.2008.08.004)  
> 　　　Myers, C. T., et al. (2017). De Novo Mutations in PPP3CA Cause Severe Neurodevelopmental Disease with Seizures. *The American Journal of Human Genetics*, 101(4), 516–524. [https://doi.org/10.1016/j.ajhg.2017.08.013](https://doi.org/10.1016/j.ajhg.2017.08.013)  
> 　　　Mizuguchi, T., et al. (2018). Loss-of-function and gain-of-function mutations in PPP3CA cause two distinct disorders. *Human Molecular Genetics*, 27(8), 1421–1433. [https://doi.org/10.1093/hmg/ddy052](https://doi.org/10.1093/hmg/ddy052)

## 從地圖挑出最怪、最合不起來的

D04 的輸出是一張衍伸地圖——每個弱連結往外長出的相關主題，以及群與群之間的洞。D05 吃這份地圖，問：哪個主題跟所有人都合不起來？給每條打怪度分，越合不起來越高，排序出清單。

這一步最容易犯的錯是把「我不熟」當成「它很怪」。所以覺得一個節點怪之前，得先查它實際上是什麼——查完還是合不起來，才算。

這個skill的prompt這樣下：

> 建立 D05 的 skill。這支 skill 相信出現頻率量的是共識，不是價值——發現的機會集中在資料邊緣，不在大家都在講的地方。當我說「幫我挑最怪的」，就去讀 `作業區/D04/` 的衍伸地圖，找出不屬於任何一群的孤島，對每個候選打怪度分數（越合不起來越高），寫清楚合不起來的地方具體是什麼，覺得怪之前先上網查那個節點實際上是什麼、不能因為自己不熟就打高分，收成排序清單存到 `作業區/D05/` 。

## 試試看

把這句話直接丟進 Claude Code：

> 幫我挑最怪的

它先講了一件我沒指定的事：地圖上 24 個節點全部都要當候選重看一遍，不能只看沒被畫到線的——有沒有連線跟內容上合不合群是兩個問題。然後才開始查。這一輪它用了 17 組關鍵字，覺得可疑的節點一個一個查過才打分。查完，6 個排除、18 個進排序表。

| 排名 | 節點 | 原本的群 | 怪度 | 合不起來在哪裡 |
| --- | --- | --- | --- | --- |
| 1 | 女性教務長協會與美國高教學生事務史 | 群5 教育史 | 95 | 查了是真的：NADW 1916 年創立、1938 年起發期刊、2000 年併入 NASPA，有完整的組織史研究。 **但它上地圖的唯一理由是 1947 那篇文章剛好登在它的會刊上** ——出版載體的巧合，不是內容關係 |
| 2 | 投射機制 | 群4 榮格 | 88 | 概念本身文獻充足；但中英文各查一輪，找不到任何人用榮格投射解釋情緒勒索。它掛的那條線又接在一句被標「原創研究」的維基句子上 |
| 3 | 榮格心理陰影詮釋 | 群4 榮格 | 84 | 整個群 4 的根是中文維基導言那句無出處的話。它另外抓了法文維基全文比對： **三個語版只有中文版有這句** |
| 4 | 詞源學與歷史語料 | 群5 教育史 | 76 | 真實且嚴謹，OED 1936 年書證對得上。但它是全圖唯一以「這個詞」而不是「這個現象」為研究對象的節點 |
| 5 | 1947 年期刊源流 | 群5 教育史 | 70 | 它把 Redl 那條查到底了：完整書目是 *Journal of the National Association of Deans of Women*, vol. 11, no. 1, Oct 1947, pp. 3–15，來源是法文維基。分數沒更高，是因為它跟職場量化那群共用「權力方拿情緒管下位者」的結構——那部分是 D04 洞 2 的材料，不算它自己怪 |
| 6 | 論壇匿名第一人稱自白 | 群1 中文科普 | 66 | D04 標「推斷，樣本只有 1」；它再搜一輪 PTT／Dcard，撈回來全是受害端敘事，第二筆自白仍然找不到 |

第 7 名以下分數掉到 52 以下，理由都是「查了之後發現有紮實的脈絡」——例如 Redl & Wattenberg 的班級經營模式是教師檢定的標準考點，國教院雙語詞彙裡有正式譯名。

排除掉的 6 個都各自附了查證。兩個值得記：五大人格特質那條，它查到 104 人力銀行的人資內容站直接以五大特質談職場情勒，學術端與實務端都活著；陰影工作出版那條，它查到那些書跟中文情勒科普是 **同一個出版產業、同一個通路、同一批讀者** ——地圖卻把兩者畫在互不相連的群裡。

它另外把 18 個排序節點裡 4 個標成「上游延伸過頭的產物」：投射機制、榮格陰影、第一人稱自白、盡責性與工作表現。前五名裡佔了兩個。

---

**下一篇** ：清單上的這幾條，要兩兩配對去探了——看兩個合不起來的東西之間到底有沒有交集，是只共用一個詞、一小片關係，還是整個形狀都一樣。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10406926)

[

D04｜弱關聯區長成一張地圖

](https://ithelp.ithome.com.tw/articles/10406926)[下一篇](https://ithelp.ithome.com.tw/articles/10407435)

[

D06｜兩個怪東西放在一起，看交集是哪一種

](https://ithelp.ithome.com.tw/articles/10407435)

系列文

[怎麼找到新東西？把 200 年的「發現」老方法教給 AI——方法圖鑑 × Claude Skills](https://ithelp.ithome.com.tw/users/20183944/ironman/9462) 共 12 篇

目錄

1. 8
	[D07｜把五天的檔案寫成一篇給外人看的文章](https://ithelp.ithome.com.tw/articles/10407437)
2. 9
	[D08｜兩個沒人接過的題目，找一個兩邊都寫過的東西](https://ithelp.ithome.com.tw/articles/10407443)
3. 10
	[D09｜比喻只准比關係，不准比長相](https://ithelp.ithome.com.tw/articles/10407706)
4. 11
	[D10｜把問題的行話放掉，丟給遠方](https://ithelp.ithome.com.tw/articles/10407723)
5. 12
	[D11｜九個skill串成一個，順便說 skill 為什麼長成兩層](https://ithelp.ithome.com.tw/articles/10407727)
[完整目錄](https://ithelp.ithome.com.tw/users/20183944/ironman/9462)

[![.](https://itadstatic.ithome.com.tw/B9/1787886940_6a90fd5c11c52.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/26/0dc4fb1e-5100-426d-bca5-b3a670933ebd_small.jpg)

[SUSE 全球最大獨立開源方案加速創新應用開發](https://itplus.ithome.com.tw/vod-page/442?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Agile Summit 敏捷高峰會 |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/27/1cbf6fe7-1530-44f1-a70f-2733169118c5.png)

[從 VSM 到 DevOps 指標了解交付能力](https://itplus.ithome.com.tw/vod-page/790?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

46 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/807b8a23-24a0-4801-8214-d6177553983e_small.png)

[企業之生成 AI 檢索查詢 - RAG 解決方案之規劃實作](https://itplus.ithome.com.tw/vod-page/897?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/493b1af4-7706-40f1-ae64-307df7b0adfc_small.png)

[API 定義到測試自動化：以 AI 驅動 Postman 測試產生器](https://itplus.ithome.com.tw/vod-page/1364?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

28 分