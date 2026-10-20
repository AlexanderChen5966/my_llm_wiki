---
title: "Day 10｜引用查證器：每一筆參考文獻都要能被打開 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10409750"
author:
  - "[[iThome]]"
published:
created: 2026-09-24
description: "Day 6 那筆捏造的引用（需作者補充：那筆引用長什麼樣、怎麼被發現的），我後來把它印出來貼在螢幕旁邊。作者是真的人，期刊是真的期刊，年份、卷期、頁碼都有，格式完美。唯一的問題是那篇論文不存在。 從那..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 10

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 10 篇

## Day 10｜引用查證器：每一筆參考文獻都要能被打開

- 分享至

Day 6 那筆捏造的引用（需作者補充：那筆引用長什麼樣、怎麼被發現的），我後來把它印出來貼在螢幕旁邊。作者是真的人，期刊是真的期刊，年份、卷期、頁碼都有，格式完美。唯一的問題是那篇論文不存在。

從那天起，我的參考文獻清單上多了一種標記：〔未查證〕。Day 7 接上 PubMed 之後，有些已經改成〔已查證〕，但那是手動一筆一筆查的。清單上還有（需作者補充：幾筆）筆掛著〔未查證〕。今天做的工具就是把它們清掉。

查證不是「看起來像不像」  
輸血前的核對是我在臨床學到最嚴格的一套流程。兩個人，血袋上的每一欄對病人手圈上的每一欄：姓名、病歷號、血型、血袋號碼。逐項對，不是掃一眼「大概是這個人」。一項不符，整袋退回，不是誰去把手圈改一改。

引用查證要用同樣的標準。一筆引用「看起來對」沒有意義，因為 AI 捏造的引用就是為了看起來對而生的。要對的是欄位：這個 DOI 解析出來的論文，標題是不是這個、第一作者是不是這個、年份是不是這個、期刊卷期頁碼是不是這個。

跟誰對  
兩個來源，依序用。

有 DOI 的，先對 Crossref。 Crossref 是 DOI 的登記機構，幾乎所有期刊論文的 DOI 都登記在那裡。它有公開的查詢介面，把 DOI 丟過去，回來的是這篇論文登記時的標題、作者、年份、期刊、卷期頁碼。Claude Code 可以直接抓網頁，不用另外裝東西。

沒 DOI 的，或 Crossref 查不到的，對 PubMed。 用 Day 7 接好的 MCP，拿標題去查，查到的話拿回 PMID 和欄位。

兩邊都查不到的，就是查不到。 不猜、不補、不假設「大概是資料庫漏收」。

三種結果  
對完之後每一筆引用得到一個標記，三種顏色，跟血品核對一樣沒有「大概」：

🟢 找到了，而且標題、第一作者、年份、期刊、卷期頁碼全部一致。  
🟡 找到了，但有欄位不一致。可能是年份差一年（線上先發表跟正式刊出不同）、頁碼是文章編號不是頁數、作者順序不同、或期刊改名。  
🔴 找不到。DOI 無法解析，PubMed 也沒有這篇，或者標題查到了但作者完全不同——後者最危險，那通常就是捏造的引用借了真論文的標題。  
🟡 不代表我錯，也不代表資料庫錯，只代表「有差異，要人來看」。🔴 也不直接等於捏造，但在我確認之前，它不能出現在論文裡。

SKILL.md 全文  
存在.claude/skills/cite-check/SKILL.md：

---

## name: cite-checkdescription: 逐筆查證參考文獻是否真實存在、欄位是否一致，對 Crossref 與 PubMed 比對後輸出 🟢🟡🔴 報告到 literature/cite-check.md。使用者說「查證引用」「查參考文獻」「這些引用是真的嗎」時使用。

## 引用查證

## 輸入

使用者用 @ 指定參考文獻清單檔（draft/references.md）。每筆引用一行。

## 查證順序

1. 有 DOI：查 [https://api.crossref.org/works/](https://api.crossref.org/works/) ，取回標題、第一作者姓氏、年份、期刊、卷、期、頁碼。
2. 無 DOI 或 Crossref 查無：用 PubMed MCP 以標題查詢，取回 PMID 與相同欄位。
3. 兩者皆查無：標 🔴。

## 比對規則

1. 必須同時比對五項：標題、第一作者姓氏、年份、期刊名、卷期頁碼。
2. 五項全部一致：🟢。
3. 標題與第一作者一致，其餘任一項不一致：🟡，並在「差異」欄逐項寫出「清單寫的／來源寫的」。
4. 標題一致但第一作者不一致：🔴，備註「標題相符但作者不符」。
5. 查無：🔴。
6. 標題比對忽略大小寫與標點，但不得忽略字詞。

## 絕對不要

- 不得修改 references.md 裡的任何一筆引用。
- 不得因為「看起來很像」就標 🟢。
- 不得在查無時自行推測可能的正確引用。

## 輸出

literature/cite-check.md，表格一筆一列：  
| 編號 | 引用（原文前 40 字） | 結果 | 來源 | DOI／PMID | 差異 | 建議 |  
表格下方三行：🟢 幾筆、🟡 幾筆、🔴 幾筆。  
在專案資料夾啟動 claude，打「@draft/references.md 查證引用」。三十幾筆大約（需作者補充：幾分鐘）跑完。

報告長什麼樣  
三筆虛構的示範，只看格式：

| 編號 | 引用（原文前 40 字） | 結果 | 來源 | DOI／PMID | 差異 | 建議 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 林某某、王某某（2023）。護理人員睡眠品質與… | 🟢 | Crossref | 10.xxxx/xxxx | — | — |
| 2 | Smith, J., & Lee, K. (2021). Sleep quality and… | 🟡 | Crossref | 10.xxxx/yyyy | 年份：清單 2021／來源 2022；頁碼：清單 45–58／來源 e12345 | 線上先發表與正式刊出年份不同，確認你引的是哪一版 |
| 3 | Chen, M. (2020). Burnout among ICU nurses… | 🔴 | — | — | Crossref 查無此 DOI；PubMed 以標題查得一篇，第一作者為 Wang | 標題相符但作者不符，疑為捏造 |
| 🟢 1 筆　🟡 1 筆　🔴 1 筆 |  |  |  |  |  |  |
| 第三筆就是 Day 6 那種東西。標題是真的，DOI 是假的，作者是拼湊的。沒有工具的時候，這種引用可以在論文裡活到口試。 |  |  |  |  |  |  |

哪裡會出錯  
標題查到了，但不是那一篇。 同一個領域裡標題相近的論文很多，「Sleep quality among nurses」這種標題可能有五篇。第一版只比對標題，它拿到第一個結果就標 🟢。所以規則一寫「必須同時比對五項」，標題對了作者不對就是 🔴。這條規則加上去之後，🟢 的數量少了（需作者補充：幾筆），少掉的那些每一筆都值得少。

它會「順手」把引用改好。 看到 🟡 的年份差一年，它直接把 references.md 裡的年份改了，然後告訴我「已修正」。我沒有要它修正。查證工具的工作是報告，不是修改——血袋不符是退回，不是護理師自己去改手圈。「絕對不要」的第一條就是這樣來的。改不改、怎麼改，是我看過差異之後的決定。

中文文獻查不到，不等於捏造。 台灣的護理期刊很多不在 Crossref，也不在 PubMed。它們在華藝、在國圖的期刊文獻資訊網。這些工具碰不到，所以中文文獻幾乎全部 🔴。第一次看到報告我嚇一跳，以為清單裡一半是假的。後來在報告的「建議」欄加了一條判斷：來源是中文期刊的 🔴，備註「來源外，需人工查華藝」。這不是工具的問題，是工具的邊界，要知道邊界在哪。

Crossref 本身會錯。 出版社上傳的登記資料偶爾有錯，年份打錯、作者名拼錯。所以 🟡 不是「我錯了」的意思，是「兩邊不一樣」的意思。誰對誰錯，要開原文看。

為什麼不讓它自動修  
一筆 🟡，工具知道差異在哪，也知道來源寫的是什麼，讓它直接改成來源的版本不是很省事嗎？

我試過，然後停掉了。原因是 🟡 的差異有時候是我對、來源錯；有時候是我引的是線上先發表版本，年份本來就該不同；有時候是我故意引舊版。這些判斷工具做不了，它只看得到「不一樣」，看不到「為什麼不一樣」。

讓它自動修，等於讓它替我決定我引的是哪一篇。這條線跟「我的用途」那一欄一樣，不讓它跨。

我還不確定的地方  
書籍章節、政府報告、學位論文這三類，Crossref 和 PubMed 都查不太到，目前全靠人工。另外，查證過的引用會不會過期？DOI 理論上永久，但期刊改名、出版社合併之後，Crossref 的登記資料會變。我打算口試前重跑一次全部，看 🟢 有沒有變 🟡。要不要定期重跑，Day 12 再想。

明天  
Day 11：查證確認了引用「存在」，明天確認它「寫對」——APA 第七版校對，中英文混排的地獄。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10409494)

[

Day 9｜十張卡攤在桌上還是看不出來：文獻比較矩陣

](https://ithelp.ithome.com.tw/articles/10409494)[下一篇](https://ithelp.ithome.com.tw/articles/10409963)

[

Day 11｜APA 第七版校對：中英文混排的地獄，和一份學校規定優先的規則檔

](https://ithelp.ithome.com.tw/articles/10409963)

系列文

[研究生自救指南:30 天用 Claude Code 打造我的論文工具箱](https://ithelp.ithome.com.tw/users/20183988/ironman/9487) 共 21 篇

目錄

1. 17
	[Day 17｜圖表工具：色盲安全色票，印成黑白還是可能失敗](https://ithelp.ithome.com.tw/articles/10413818)
2. 18
	[Day 18｜沒有「回頭」這個選項：我在原始資料上直接動手的那三個月](https://ithelp.ithome.com.tw/articles/10414351)
3. 19
	[Day 19｜第三週回顧：資料這週，錯的代價比文獻週高很多](https://ithelp.ithome.com.tw/articles/10414697)
4. 20
	[Day 20｜文獻回顧段落草稿：它寫的每一句都對，合起來卻什麼都沒說](https://ithelp.ithome.com.tw/articles/10415367)
5. 21
	[Day 21｜把生成語感改回人話：AI 痕跡從哪裡來，怎麼抓](https://ithelp.ithome.com.tw/articles/10416067)
[完整目錄](https://ithelp.ithome.com.tw/users/20183988/ironman/9487)

[![.](https://itadstatic.ithome.com.tw/B9/1789958815_6ab09a9f3bf4f.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/11/690f3279-6842-44b6-8c53-eda67423af55_small.jpg)

[進入無密碼時代：探索新一代的身分驗證](https://itplus.ithome.com.tw/vod-page/506?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

34 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/2/5/d41fd572-2c80-49ed-8f21-ec4562baf98c_small.png)

[用 Kustomization 搭配 yq 部署 K8s 容器](https://itplus.ithome.com.tw/vod-page/1108?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/c0043af7-8ad0-4a2e-8bce-5cc74cf7e1c1_small.png)

[【推動 AI 提升政府機關工作效率】](https://itplus.ithome.com.tw/vod-page/927?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

20 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/e7adb7fd-6bb8-4533-b703-eaecb45ea74f_small.png)

[Prometheus + AutoML 驅動的 AIOps 革命](https://itplus.ithome.com.tw/vod-page/1363?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

44 分