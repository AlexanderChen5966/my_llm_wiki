---
title: "Day 6｜我踩的第一個坑：AI 捏造引用，以及我怎麼建驗證機制 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10408556"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "系列：研究生自救指南：30 天用 Claude Code 打造我的論文工具箱進度：第一週「地基」（Day 1–6）第 6 篇 老師在我文獻回顧初稿的第（需作者補充：頁碼）頁圈了一筆引用，旁邊寫了（需作..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 6

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 6 篇

## Day 6｜我踩的第一個坑：AI 捏造引用，以及我怎麼建驗證機制

- 分享至

> 系列：研究生自救指南：30 天用 Claude Code 打造我的論文工具箱  
> 進度：第一週「地基」（Day 1–6）第 6 篇

老師在我文獻回顧初稿的第（需作者補充：頁碼）頁圈了一筆引用，旁邊寫了（需作者補充：老師的原話）。我回去查。PubMed 沒有。Google Scholar 沒有。那本期刊的網站上，那一卷那一期的目錄裡沒有這篇文章。DOI 打進去，連到一篇完全不相干的論文。

那筆引用長這樣，我把作者、篇名、期刊都改掉了，格式跟原本的一模一樣：

> 〔範例〕王大明、李小華（2019）。輪班護理人員睡眠品質與工作壓力之相關研究。某某護理學刊，12（3），45–60。https://doi.org/10.xxxx/xxxxx

作者像真的，篇名像真的，期刊卷期頁碼一應俱全，APA 格式一個標點都沒錯。它不存在。而它是我在聊天視窗時期請 AI「幫我找幾篇相關文獻」的產物，我當時看它格式完整就收進清單，沒有一筆一筆點開。

## 為什麼會這樣

我後來想通的說法是：我請它「找」文獻，它做的其實是「寫一段看起來像文獻的文字」。模型擅長的是產生像樣的文字，一筆引用該長什麼樣它看過幾百萬次，所以寫出來的東西格式完美。但格式完美和存在與否，是兩件不相干的事。

這跟它有沒有查到資料無關，跟它「誠不誠實」也無關。它就是這樣運作的。所以我不能靠「這次它應該不會亂講」，我需要一個機制，讓每一筆引用都必須帶著可以被查的東西出現，查不到就自動被標記，而且標記不能被省略。

## 寫進 CLAUDE.md 的規則

Day 3 的範本裡有一行「不得捏造參考文獻」，那一行就是這件事之後補的，今天把它擴成完整版。放在 CLAUDE.md 的「規則」段，位置往前挪，越重要的規則越要在前面。

```markdown
## 引用規則（優先於其他所有規則）
1. 每一筆引用都必須附 DOI 或 PMID，擇一，寫在該筆最後。
2. 無法確認 DOI 或 PMID 存在時，在該筆最前面標〔未查證〕。不省略、不猜一個、不用「應該是」。
3. 不得產生我沒有提供、或你沒有實際查到的文獻。「我記得有一篇」不算查到。
4. 摘要或引述一篇文獻的內容時，附頁碼或段落位置；沒有全文就標〔無全文〕，不憑摘要推測內文。
5. 輸出引用清單時固定用這個格式，狀態欄不得空白：
   〔已查證／未查證／無全文〕作者（年）。篇名。期刊，卷（期），頁碼。DOI 或 PMID
6. 我沒有要求時，不要主動「補充幾篇相關文獻」。
```

第五條是關鍵。狀態欄是必填的，它就沒有辦法用「格式完整」矇混過去，每一筆都得表態。第六條是因為聊天視窗時期的假文獻，大多來自它「順便」補充的那幾筆。

規則寫進去之後我做了一個測試：故意問它「幫我找三篇台灣護理人員睡眠品質的研究」。它回了三筆，前面全部標〔未查證〕，並說明它無法連上資料庫確認。這是我要的答案。之後 Day 7 接上 PubMed MCP，它才有辦法真的去查，〔已查證〕這個狀態才會出現。

## 引用三讀：人工核對流程

規則管的是 AI 那一端，我這一端也要有流程。給藥有三讀五對，我把引用核對也拆成三讀，每一筆都過。

| 讀 | 看什麼 | 過不了怎麼辦 |
| --- | --- | --- |
| 第一讀：看欄位 | 這筆有沒有 DOI 或 PMID？狀態欄是什麼？ | 缺欄位或〔未查證〕的，先退回，不進 references.md |
| 第二讀：開連結 | 打開 DOI 或 PubMed 頁面，作者、年份、篇名、期刊是否與清單一致 | 有 DOI 但指到別篇，視同不存在；找得到但欄位不符，用官方頁面的資訊改清單 |
| 第三讀：讀內容 | 讀摘要或全文，它真的說了我引用它說的那件事嗎？ | 沒說，整筆刪掉，不管格式多漂亮 |

第一讀很快，看一眼欄位就好。第二讀每筆要（需作者補充：平均幾分鐘），我目前（需作者補充：筆數）筆文獻做下來，第二讀花的時間最多。第三讀最慢，但它抓的錯最嚴重：文獻存在，但根本沒講那件事，這種錯口試委員一問就露餡。

前兩讀是機械式的，Day 10 會讓 Claude Code 寫一支腳本自動跑，輸出一張「有／無」報表，我只看「無」的那幾筆。第三讀永遠是人做。

## 查不到，不等於不存在

規則跑了幾天，我碰到一個問題：很多中文文獻沒有 DOI，也不在 PubMed，尤其是幾年前的。照規則它們全部會被標〔未查證〕，然後在第一讀被我退回。這不對，它們是真的存在的文獻。

我加了一條補充：無 DOI 且無 PMID 的文獻，可以改附一個能打開的來源連結（期刊官網或圖書館資料庫的該篇頁面），或紙本的館藏資訊。狀態改標〔人工查證〕，由我在第二讀親自確認。這條規則讓查證流程對中文文獻不會太嚴，也維持了「每一筆都要能被打開」的底線。

「查不到不代表不存在，但一定要人看」，這句話我寫在 references.md 的第一行。

## 我還不確定的地方

這套機制現在攔得住「憑空捏造」的文獻，但攔不攔得住「文獻存在、數字被改」的那種，我不知道。例如它摘要一篇文獻時把樣本數寫錯，或把相關係數的方向寫反，三讀裡只有第三讀有機會抓到，而第三讀是最花時間的一讀。Day 18 會講一次它在資料上而非引用上出錯的事，那次讓我把這套三讀又加了一層。

明天 Day 7：把 PubMed 檢索式存成檔案，接上 MCP，讓 Claude Code 真的去查，而且每次查了什麼都留紀錄。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10408287)

[

Day 5｜Skill 是什麼：把「每次都要重講一遍的事」變成一次設定

](https://ithelp.ithome.com.tw/articles/10408287)[下一篇](https://ithelp.ithome.com.tw/articles/10408927)

[

Day 7｜文獻檢索：把搜尋策略寫成可重現的腳本（PubMed MCP）

](https://ithelp.ithome.com.tw/articles/10408927)

系列文

[研究生自救指南:30 天用 Claude Code 打造我的論文工具箱](https://ithelp.ithome.com.tw/users/20183988/ironman/9487) 共 15 篇

目錄

1. 11
	[Day 11｜APA 第七版校對：中英文混排的地獄，和一份學校規定優先的規則檔](https://ithelp.ithome.com.tw/articles/10409963)
2. 12
	[Day 12｜第二週回顧：五個文獻工具用了一週，老實算帳](https://ithelp.ithome.com.tw/articles/10410477)
3. 13
	[Day 13｜問卷資料清理：從 Google 表單匯出的那份原始檔開始](https://ithelp.ithome.com.tw/articles/10411094)
4. 14
	[Day 14｜反向題、遺漏值、離群值：清理規則不是 prompt，是程式碼](https://ithelp.ithome.com.tw/articles/10412049)
5. 15
	[Day 15｜匯出給 SPSS：變數標籤與值標籤的自動化，以及一個會讓平均數爆掉的數字](https://ithelp.ithome.com.tw/articles/10412671)
[完整目錄](https://ithelp.ithome.com.tw/users/20183988/ironman/9487)

[![.](https://itadstatic.ithome.com.tw/B9/1789119962_6aa3cdda3af6d.png)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/1b608b03-3905-4e58-afef-17bacd6ef599_small.jpg)

[企業安全開發流程升級之路](https://itplus.ithome.com.tw/vod-page/392?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/13/b6b2aa22-a1bf-4c00-9838-25f14862a151_small.png)

[那些年，我們一起踩過的坑 - EKS 五年企業級實戰分享](https://itplus.ithome.com.tw/vod-page/913?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/ecd8366b-3d2b-4573-accd-ae88ef483be7_small.png)

[AI 及自動化實戰演練：系統化合規超簡單！](https://itplus.ithome.com.tw/vod-page/873?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/2a49150b-cba3-4651-b324-45c8052f7a2a_small.png)

[Connectivity Cloud－安全無處不在](https://itplus.ithome.com.tw/vod-page/869?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

32 分