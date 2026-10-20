---
title: "Day 5｜Skill 是什麼：把「每次都要重講一遍的事」變成一次設定 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10408287"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "CLAUDE.md 寫好之後，我以為重講的問題解決了。結果這幾天我還是一直在打同一段話：「中文文獻排前面，兩位中文作者中間用頓號，英文用 &，年份括號全形，期刊卷期不要斜體，DOI 用 htt..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 5

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 5 篇

## Day 5｜Skill 是什麼：把「每次都要重講一遍的事」變成一次設定

- 分享至

CLAUDE.md 寫好之後，我以為重講的問題解決了。結果這幾天我還是一直在打同一段話：「中文文獻排前面，兩位中文作者中間用頓號，英文用 &，年份括號全形，期刊卷期不要斜體，DOI 用 [https://doi.org/](https://doi.org/) 開頭，缺欄位的標出來不要補。」每次要它整理一批參考文獻，就再打一次，打到（需作者補充：第幾次）的時候我把這段話存成了記事本，複製貼上。

複製貼上就是 skill 的前身，只是 Claude Code 有一個正式的位置放它。

## 背景和流程是兩件事

CLAUDE.md 解決的是「這個專案是什麼」：題目、對象、量表、規則。它每次啟動都讀，所以裡面只該放每次都用得到的事。

APA 格式怎麼排，不是每次都用得到。它是一件特定的事的做法：輸入是什麼、要照什麼順序處理、輸出長什麼樣、哪些情況要停下來問。這種東西塞進 CLAUDE.md，會讓那份檔案越來越長，而且大多數時候在占位子。

用病房的話講：CLAUDE.md 是病人的基本資料和過敏史，每個接手的人都要看；skill 是某一項處置的標準作業流程，做那件事的時候才翻出來。兩個都重要，但不該寫在同一張紙上。

Claude Code 的 skill 就是這張 SOP。它是一個資料夾，裡面一份 `SKILL.md` ，用 Markdown 寫，開頭有幾行說明它叫什麼、什麼時候該用。之後每一件「我發現自己又在重講」的事，都會走同一條路：Day 8 的摘要卡、Day 11 的校對、Day 22 的口試委員，全部是 skill。

## 放哪裡、長什麼樣

專案級的 skill 放在 `.claude/skills/<skill 名稱>/SKILL.md` ，Day 4 的資料夾樹已經留了位子。只跟這個論文專案有關的，放這裡。

使用者級的放在 `C:\Users\你的名字\.claude\skills\<skill 名稱>\SKILL.md` ，所有專案共用。「APA 第七版中文格式」這種下一篇論文還會用的，之後可能搬過去；現在先放專案級，方便 Day 26 整包給同學。

SKILL.md 開頭要有一段 frontmatter，就是兩條橫線夾著的幾行：

- `name` ：skill 的名字，跟資料夾同名。
- `description` ：一句話說明它做什麼、什麼時候該用。這一句是觸發依據，Claude 會拿使用者說的話跟它比對，決定要不要用這個 skill。

frontmatter 底下就是正文，寫法自由。同一個資料夾可以再放 `references/` 、 `scripts/` 之類的輔助檔，在 SKILL.md 裡用相對路徑指過去。今天的範例還用不到。

## 第一個 skill：apa7-zh

這是 `.claude/skills/apa7-zh/SKILL.md` 的全文。規則的部分是我系上的版本，你的系所或投稿期刊如果有不同規定，改那幾行就好。

```markdown
---
name: apa7-zh
description: 產生或檢查 APA 第七版格式的參考文獻，特別處理中文文獻與中英文並列。當使用者提到「參考文獻格式」「APA」「文獻排序」「這筆引用格式對不對」時使用。
---

# APA 第七版中文文獻格式

## 這個 skill 做什麼
把使用者提供的文獻資訊（作者、年份、篇名、期刊、卷、期、頁碼、DOI）排成 APA 第七版格式；
或檢查使用者提供的參考文獻清單是否符合格式。

## 界線
- 只處理使用者提供的文獻資訊。缺少的欄位標〔缺：欄位名〕，不猜、不補。
- 不新增使用者沒有提供的文獻。
- 檢查模式下不直接修改檔案，輸出「原句／問題／建議」三欄清單，由使用者決定要不要改。
- 每一筆最後附〔DOI：有／無〕。無 DOI 的中文文獻標〔無 DOI，需人工確認來源〕。

## 排序
1. 中文文獻在前，英文文獻在後。
2. 中文依第一作者姓氏筆畫排序；英文依第一作者姓氏字母排序。
3. 同一作者多篇，依年份由舊到新。

## 中文期刊文獻格式
作者一、作者二（年）。篇名。期刊名，卷（期），起頁–迄頁。https://doi.org/xxx
- 作者之間用全形頓號「、」。
- 年份、卷期的括號用全形。
- 中文標點全部用全形。
- 篇名與期刊名不加書名號、不加引號。
（需作者補充：系所對中文格式的特殊規定，例如期刊名是否加粗）

## 英文期刊文獻格式
Author, A. A., & Author, B. B. (Year). Title of article. *Journal Name, volume*(issue), pages. https://doi.org/xxx
- 最後兩位作者之間用「&」，前面用逗號。
- 篇名只有第一個字、冒號後第一個字與專有名詞大寫。
- 期刊名與卷數斜體，期數與頁碼不斜體。

## 內文引用
- 中文兩位作者：（王大明、李小華，2020）；敘述式寫「王大明與李小華（2020）指出」。
- 英文兩位作者：(Wang & Lee, 2020)；敘述式寫 Wang and Lee (2020)。
- 三位以上作者：第一作者加「等」或「et al.」。

## 輸出格式
產生模式：每筆一行，依上述排序，最後附〔DOI：有／無〕。
檢查模式：三欄表格，欄位為「原句」「問題」「建議」，一個問題一列。
```

## 怎麼觸發、怎麼核對

兩種觸發方式。自動：我說「幫我把 @references.md 裡的文獻排成 APA 格式」，它比對 description，決定用這個 skill。手動：直接打 `/apa7-zh` ，後面接要處理的內容或檔案。我一開始都用手動，確定它會照我的流程走之後才放給它自己判斷。

核對方法我用三筆測試文獻。一筆完整的中文期刊文獻，看它排出來跟系上範例是否一字不差；一筆故意缺頁碼的，看它有沒有標〔缺：頁碼〕，還是自己補了一個；一筆英文的，看 & 和斜體。第二筆最重要。它如果補了頁碼，表示「不猜、不補」這一行沒有生效，要回去改寫法，把那句放到更前面、寫得更硬。

做好之後，整理文獻變成打一句話的事。原本那段一百多字的規則，從此不用再打，也不會因為哪天漏打一句而排錯。第一次省下這段話的時候，我才真的懂 Day 2 說的「能累積」：CLAUDE.md 累積的是背景，skill 累積的是做法，兩個加起來才是一份完整的交班。

我這裡先說一件我到 Day 6 才學到的事：這個 skill 只管格式，不管那篇文獻是否存在。格式排得再漂亮的假文獻還是假的。

## skill 和 CLAUDE.md 的分工，用一句話

CLAUDE.md 回答「你在做什麼研究、有什麼規矩」，skill 回答「這件事怎麼做」。分不清楚的時候問自己：這段話每次啟動都需要嗎？需要，進 CLAUDE.md；不需要，做成 skill。

Day 11 會把今天這個 skill 擴充成完整的校對工具，處理中英文混排最常出錯的那幾種。Day 26 會講 skill 資料夾整包複製給同學的時候，哪些要改、哪些不用。

## 我還不確定的地方

description 要怎麼寫才會準確觸發，我還在試。寫太窄，它只有我說「APA」時才動；寫太寬，我在講別的事它也跳出來。我目前的版本列了四種說法，不確定夠不夠，之後每被漏觸發一次就補一種。

明天 Day 6：我踩的第一個坑，AI 給了我一筆格式完美、但根本不存在的文獻。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10408143)

[

Day 4|病歷不能塗改,原始資料也是:論文專案的資料夾結構

](https://ithelp.ithome.com.tw/articles/10408143)[下一篇](https://ithelp.ithome.com.tw/articles/10408556)

[

Day 6｜我踩的第一個坑：AI 捏造引用，以及我怎麼建驗證機制

](https://ithelp.ithome.com.tw/articles/10408556)

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

[![.](https://itadstatic.ithome.com.tw/B9/1789123100_6aa3da1cbf61d.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/8c3e3140-1e4a-44ed-93ac-6e3bb048b760_small.png)

[運用雲端技術，縮短 APP 研發週期與千台實機測試](https://itplus.ithome.com.tw/vod-page/528?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/25/053391d5-7c2d-4e90-a51d-a056c1f2f158_small.jpg)

[從另外一個角度看 ChatGPT 的社交工程潛力—如何建構最高性價比的資訊安全防線](https://itplus.ithome.com.tw/vod-page/812?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/a1eba963-ad43-4912-baa3-e6eb8f86f107_small.png)

[DevOps 困局與平台工程](https://itplus.ithome.com.tw/vod-page/985?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/8/11/3462fd71-7289-46d6-9fc9-5785fb063942_small.png)

[讓 AI 不只回答，更能完成工作：企業 AI 工作流程的落地實務](https://itplus.ithome.com.tw/vod-page/1509?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Agentic Automation Day 企業自動化 x Agent |

29 分