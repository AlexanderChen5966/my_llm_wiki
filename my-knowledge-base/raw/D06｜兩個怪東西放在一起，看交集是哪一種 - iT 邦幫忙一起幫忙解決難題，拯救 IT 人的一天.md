---
title: "D06｜兩個怪東西放在一起，看交集是哪一種 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407435"
author:
  - "[[iThome]]"
published:
created: 2026-09-11
description: "Koestler 1964 年給創造下了一個很窄的定義：創造不是想得比別人深，是把兩套本來各走各的思考框架接在一起。他管這個叫 bisociation，跟一般的聯想分開講——聯想是在同一個框架裡往前滑..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 6

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 怎麼找到新東西？把 200 年的「發現」老方法教給 AI——方法圖鑑 × Claude Skills系列 第 6 篇

## D06｜兩個怪東西放在一起，看交集是哪一種

- 分享至

Koestler 1964 年給創造下了一個很窄的定義：創造不是想得比別人深，是把兩套本來各走各的思考框架接在一起。他管這個叫 bisociation，跟一般的聯想分開講——聯想是在同一個框架裡往前滑，bisociation 是兩個框架第一次碰面。

他最愛舉的例子是古騰堡。印章能把字壓在蠟上，一次一小塊；酒榨能均勻壓過一大片，但幾百年來只拿來榨葡萄。兩樣東西都在那裡，沒人把它們放在一起看過，直到有人把它們接起來。

這個故事要打個折。古騰堡幾乎沒有留下任何私人文字，我們手上跟他有關的文件大多是法庭與公證紀錄那一類；Koestler 轉述的那封「看著葡萄酒榨、一道光射進來」的信，在史料上站不住。所以它是一個講法，不是證據——螺旋壓榨機確實是印刷機的機械祖先，但那個靈光一閃的瞬間沒有人記錄過。

真正撐得住的是後來的資料。2013 年 Uzzi 這組人把 Web of Science 上 1790 萬篇論文的參考文獻兩兩拆開，算每一組期刊配對在真實文獻裡有多罕見，再回頭看哪些論文最後進了被引用數的前 5%。結果不是「越怪越好」：命中的論文長得很一致——大量常規組合當底，外加一小撮極不尋常的組合，兩個條件同時滿足的論文，成為高引用論文的機率大約是兩倍。只有常規的不會爆，通篇都是罕見組合的也不會。

所以配是要配，但不是隨便抓兩個東西來配。要在一個已經有主軸的地方配。昨天那張清單就是主軸——今天這支 skill 只能吃它，不准再跑去外面撿題目回來湊對。

> 出處：Koestler, A. (1964). *The Act of Creation*. London: Hutchinson.  
> 　　　Uzzi, B., Mukherjee, S., Stringer, M., & Jones, B. (2013). Atypical Combinations and Scientific Impact. *Science*, 342(6157), 468–472. [https://doi.org/10.1126/science.1240474](https://doi.org/10.1126/science.1240474)

## 把清單上的主題兩兩配對，看交集有多深

D05 的輸出是一張怪度排序清單——地圖上的節點照「合不起來的程度」排好，附上每一條為什麼合不起來。D06 吃這份清單，從裡面挑最不相干的兩個放在一起，問一個問題：這兩個東西之間的交集到底是什麼？只共用一個詞、有一小片關係結構長得像、還是整個形狀都一樣。三選一，不能含混。

要比對關係結構，兩邊都得先寫得出常用詞和骨架。不熟的主題不能憑印象寫，也不能直接留白——先查，查了還寫不出來才留白。

這個skill的prompt這樣下：

> 建立 D06 的 skill。這支 skill 相信新點子是兩個舊東西第一次見面，同一個領域裡想得再深都只是聯想。當我說「幫我配對去探」，就去讀 `作業區/D05/` 的怪度清單，從清單裡挑最不相干的主題兩兩配對，各自寫下常用詞和常見的關係結構——不熟的主題先上網查再寫，查了還寫不出來才留白並註明查了什麼——比對出交集是只共用一個詞、一小片相似的關係、還是整個形狀都一樣，三選一標型，表面相似不能算整個形狀，不要填一個看起來合理的值，只寫辨識說明不寫操作教學，輸出配對表存到 `作業區/D06/` 。

最後那句「只寫辨識說明不寫操作教學」是有原因的。這條線遲早會探到一些機制——例如忽冷忽熱的對待跟間歇性增強是同一個東西——把機制的名字寫出來，是想讓正在經歷的人認出自己身處的處境，不是要教誰怎麼設計，這條線只做辨識，不產手法。

## 試試看

把這句話直接丟進 Claude Code：

> 幫我配對去探

它先講挑選標準：讓 D05 怪度前六名互配，但 **同群的不配** ——投射機制跟榮格陰影同屬群 4，詞源學跟 1947 期刊同屬群 5，互配等於在同一個領域裡繞圈。所以改成跨群配兩組，另外刻意留一組當對照：挑 D05 清單裡 **唯一一組已經被上游宣稱有共用結構** 的組合，拿來檢驗「表面相似不能算整個形狀」這句話。

六個主題它全部獨立查過，沒有一個憑印象寫。用了 15 組關鍵字、抓了 3 個網頁全文。

| 組 | A 主題 | B 主題 | 判型 | 對得上／共幾條 |
| --- | --- | --- | --- | --- |
| 1 | 女性教務長協會與高教學生事務史（95） | 投射機制（88） | 一小片相似的關係 | 3／6 |
| 2 | 詞源學與歷史語料（76） | 論壇匿名第一人稱自白（66） | **整個形狀都一樣** | 6／7 |
| 3 | 1947 年 Redl 那篇（70） | 盡責性與工作後果的量化研究（50）——對照組 | 只共用一個詞 | 0／6 |

第 1 組連一個共用詞都沒有，交集完全在關係結構層。女性教務長那條它查出來的骨架是：一群被排除在正式職涯階梯外的從業者，自己辦會議、創協會、寫教科書、辦期刊，把現場實務封裝成知識；知識活下來成了今天學生事務的地基， **但生產者的身分從那套知識的自我敘事裡消失了** ——1946 年已有降級的觀察，1962 年只剩三成直屬校長，1970 年代職位罕見，職稱被 dean of students 吸收。對上的是「內容脫離來源被別處承載、來源不再被標記、要認回來得另外做一次專門的工作」；對不上的是承擔者方向相反——教務長那邊承接者受益，投射那邊承接者受害。

第 2 組是這輪唯一判「整個形狀都一樣」的。兩邊術語零重疊——歷史辭書學的 antedating、reading programme，跟匿名社群研究的 disinhibition、throwaway——但七個環節對上六個，關鍵環節四個全對。它把共同骨架寫成一句話：

> 在一個只能靠留存文本的證據面上，宣告一個只對當下檢索範圍成立的存在／不存在判斷；該判斷只能被單一筆正面證據推翻，不能被更多的「沒找到」加強；而下游會把它當成關於世界的事實接手。

第 3 組零分，而且順手修正了上游一個宣稱。D05 寫 1947 那篇「跟群 3 共用『制度中有權力差的一方拿情緒管理下位者』這個結構」，登記為 D04 的洞 2。逐條核對打開：那個結構存在於 Redl 與 **職場情緒勒索這個現象** 之間， **不存在於 Redl 與那條量化研究線** 之間——因為量化研究的模型裡 **沒有施為者** ，自變項是被勒索者在量表上勾出來的知覺分數，勒索者做了什麼從來沒被測量。上游看到的相似，是「情緒勒索」這串字在兩邊都出現造成的。

---

**下一篇** ：這輪換了一種撞牆法——不是量不到，是量到的東西太抽象。下一篇試試看改挑最怪的那個，去對一個內容夠厚、夠具體的主流概念，看比對會不會落回地面。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10406929)

[

D05｜弱關聯區之間，挑出跟誰都合不起來的

](https://ithelp.ithome.com.tw/articles/10406929)[下一篇](https://ithelp.ithome.com.tw/articles/10407436)

[

D06b｜最怪的那個不動，換七個常見說法來配

](https://ithelp.ithome.com.tw/articles/10407436)

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

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/10/2/ed51f871-350a-4e1e-8197-667096b8b090_small.png)

[雲端備份的最佳助力，Microsoft 365 維運優化大解密！【宏碁資訊網路學堂】](https://itplus.ithome.com.tw/vod-page/706?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

ＡＥＢ大補帖 |

41 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/19/6aa7b126-713e-4982-a22a-14e04008b6e7_small.png)

[透過多重體驗開發平台 (MXDP) 加快開發速度、強化使用者體驗](https://itplus.ithome.com.tw/vod-page/751?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/876ffb26-1117-433b-a208-163f6df0dcca_small.png)

[Centralized CI/CD 策略的探索與實踐](https://itplus.ithome.com.tw/vod-page/1094?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/2/5/6057bb7c-8b1a-4107-8907-c7398becb179_small.png)

[自建 K8s 環境中的高可用性規劃和災難復原方式](https://itplus.ithome.com.tw/vod-page/1115?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

37 分