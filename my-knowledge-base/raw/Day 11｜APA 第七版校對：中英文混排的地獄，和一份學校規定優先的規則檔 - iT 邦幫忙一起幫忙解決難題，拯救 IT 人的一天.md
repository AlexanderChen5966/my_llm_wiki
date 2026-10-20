---
title: "Day 11｜APA 第七版校對：中英文混排的地獄，和一份學校規定優先的規則檔 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10409963"
author:
  - "[[iThome]]"
published:
created: 2026-09-24
description: "Day 1 那四個小時，就是在做今天這件事。 三十幾筆參考文獻，改到第二十筆發現前十筆用的是另一套規則。不是我不會 APA，是 APA 第七版手冊是英文寫的，中文文獻該怎麼寫，手冊沒說。台灣各校各自訂..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 11

0

[Claude AI](https://ithelp.ithome.com.tw/2026ironman/claude-ai)

### 研究生自救指南:30 天用 Claude Code 打造我的論文工具箱系列 第 11 篇

## Day 11｜APA 第七版校對：中英文混排的地獄，和一份學校規定優先的規則檔

- 分享至

Day 1 那四個小時，就是在做今天這件事。

三十幾筆參考文獻，改到第二十筆發現前十筆用的是另一套規則。不是我不會 APA，是 APA 第七版手冊是英文寫的，中文文獻該怎麼寫，手冊沒說。台灣各校各自訂了一套中文版，彼此不完全一樣，跟老師的習慣又不完全一樣。我改到一半才發現自己混了兩套。

今天要把那四個小時變成四分鐘，但前提是先把「哪一套」講清楚。

中文 APA 沒有標準答案，只有學校規定  
英文文獻的 APA 7 很明確：作者姓在前、年份括號、篇名 sentence case、期刊名和卷數斜體、期數括號不斜體、DOI 用 [https://doi.org/](https://doi.org/) 開頭。有爭議的地方少。

中文文獻就是另一回事。作者之間用頓號還是逗號？年份括號用全形還是半形？期刊名要斜體、粗體、還是不動？中文文獻排在英文前面還是後面，依筆畫還是依拼音？三人以上的內文引用寫「等」還是「等人」？每一題都有不只一種答案，而且我學校的答案跟隔壁學校不一樣（需作者補充：你學校規定裡跟一般 APA 中文版不同的一兩條）。

這件事我在臨床早就懂：護理紀錄的格式每家醫院都有自己的規範，你不會拿別家醫院的格式來寫，也不會拿教科書的範例來跟護理長吵。規範在哪裡，就照哪裡寫。

所以今天的工具，第一件事不是校對，是把「我學校的規定」寫成一份規則檔。

規則檔，不是寫進 skill  
規則放在 literature/apa-rules.md，跟 skill 分開。原因跟 Day 3 的交班單一樣：規則會變，換學校、換老師、換期刊都要改；skill 是流程，不該每次都動。

## APA 7 校對規則（本校版）

## 優先順序

1. 本檔規則
2. 本檔沒寫的，依 APA 第七版手冊
3. 兩者都沒寫的，不改，列入「未涵蓋」

## 中文文獻

- 作者之間用頓號「、」，最後一位前不加「與」
- 年份用全形括號（2023），後接全形句號
- 期刊名與卷數：（需作者補充：斜體／粗體／不動）
- 期數用全形括號，頁碼用半形短橫「45–58」
- 排序：中文在前依第一作者姓氏筆畫，英文在後依字母
- DOI 一律 [https://doi.org/](https://doi.org/) 開頭

## 英文文獻

- 依 APA 7 手冊
- 篇名 sentence case，期刊名 title case
- 期刊名與卷數斜體，期數括號不斜體
- 20 位作者以內全列，超過依手冊處理

## 內文引用

- 中文兩位作者：林某某與王某某（2023）／（林某某、王某某，2023）
- 中文三位以上：林某某等（2023）
- 英文依 APA 7：(Smith & Lee, 2021)、Smith et al. (2021)
- 中文句子裡引英文文獻：括號用全形（Smith & Lee, 2021）

## 本校特別規定

- （需作者補充）  
	這份檔的每一條都要能指到一個出處：學校的論文格式手冊第幾頁、或 APA 手冊第幾節。指不出出處的規則不寫，因為指不出出處的規則，口試委員問起來我也答不出來。

這一次，讓它改——但改在副本  
Day 10 我堅持查證工具只報告不修改，因為「差異為什麼存在」工具判斷不了。

APA 校對不一樣。「作者之間該用頓號」這種規則沒有為什麼，只有對不對。規則明確、結果可驗證、而且可逆——這三個條件都成立的時候，我讓它動手。

但動手的地方是副本。它讀 draft/references.md，寫出 draft/references.apa.md，原檔一個字不碰。同時輸出一份逐筆對照報告，我看完報告、確認過，才自己把副本換成正本。這回到 Day 4 的原則二：副本是 output，做壞了刪掉重跑。

SKILL.md 全文  
存在.claude/skills/apa-check/SKILL.md：

---

## name: apa-checkdescription: 依 literature/apa-rules.md 校對參考文獻清單的 APA 7 格式，輸出修正後副本與逐筆對照報告。使用者說「校對 APA」「檢查參考文獻格式」「APA 校對」時使用。

## APA 7 校對

## 輸入

使用者用 @ 指定參考文獻清單（draft/references.md）。

## 前置檢查

1. 先讀 literature/cite-check.md。結果為 🔴 的引用不校對，原樣保留並標「未通過查證，略過」。
2. 讀 literature/apa-rules.md。找不到這份檔就停下來，不得用自己記得的 APA 規則代替。

## 校對規則

1. 只依 apa-rules.md 校對。規則檔沒寫的項目一律不改，列入「未涵蓋」。
2. 每一筆逐項檢查：作者分隔、年份括號、篇名、期刊名格式、卷期頁碼、DOI 格式。
3. 全形／半形括號、頓號／逗號、短橫「–」／連字號「-」視為不同字元，逐一比對。
4. 不得改動任何實質內容：作者名、年份數字、篇名文字、頁碼數字。只改格式。
5. 中英文混排的一筆（例如中文作者的英文論文），依該筆的語言判定，並在報告備註。

## 絕對不要

- 不得修改 references.md 原檔。
- 不得在規則檔沒寫的情況下憑記憶套用 APA 6 或 APA 7 規則。
- 不得補上任何原文沒有的欄位（例如自行查 DOI 補上）。

## 輸出

- draft/references.apa.md：修正後的完整清單
- literature/apa-report.md：表格一筆一列  
	| 編號 | 原文 | 修正後 | 改了什麼 | 依據（規則檔第幾條） |  
	表格下方：改了幾筆、未改幾筆、未涵蓋幾筆及其項目  
	在專案資料夾啟動 claude，打「@draft/references.md 校對 APA」。

報告長什麼樣  
三筆虛構示範：

| 編號 | 原文 | 修正後 | 改了什麼 | 依據 |
| --- | --- | --- | --- | --- |
| 1 | 林某某，王某某 (2023)。護理人員睡眠品質與職業倦怠之相關研究。虛構護理期刊，10(2)，45-58。 | 林某某、王某某（2023）。護理人員睡眠品質與職業倦怠之相關研究。虛構護理期刊，10（2），45–58。 | 作者分隔逗號→頓號；年份括號半形→全形；期數括號半形→全形；頁碼連字號→短橫 | 中文文獻第 1、2、4 條 |
| 2 | Smith, J., & Lee, K. (2021). Sleep Quality and Burnout Among Nurses. Journal of Nursing Studies, 12(3), 45-58. doi:10.xxxx/yyyy | Smith, J., & Lee, K. (2021). Sleep quality and burnout among nurses. *Journal of Nursing Studies*, *12* (3), 45–58. [https://doi.org/10.xxxx/yyyy](https://doi.org/10.xxxx/yyyy) | 篇名→sentence case；期刊名與卷數加斜體；頁碼→短橫；DOI 舊格式→新格式 | 英文文獻第 2、3 條；DOI 規則 |
| 3 | Chen, M. (2020). Burnout among ICU nurses… | （未通過查證，略過） | — | 前置檢查第 1 條 |
| 改了 2 筆　未改 0 筆　略過 1 筆 |  |  |  |  |
| 未涵蓋：第 2 筆的期刊名縮寫問題（規則檔未定義） |  |  |  |  |
| 第一筆改了四個地方，肉眼看原文和修正後幾乎一樣——這正是四小時的來源。全形半形括號、頓號逗號、短橫連字號，這些差異在 Word 裡放大到 200% 才看得出來，工具看的是字元，不會看漏。 |  |  |  |  |

第三筆是 Day 10 的 🔴。格式再對，一筆不存在的引用還是不存在。查證在校對之前，順序不能反。

哪裡會出錯  
它會用「它記得的 APA」蓋掉我的規則檔。 第一版沒有「找不到規則檔就停下來」那條，某次規則檔路徑打錯，它沒找到，就默默用自己的版本校了一遍。改出來的東西看起來也很 APA，只是不是我學校的 APA。訓練資料裡 APA 6 和 7 混在一起，它記得的版本你永遠不確定是哪一版。所以前置檢查第 2 條是硬的：沒規則檔就不做。

看不見的字元。 全形括號和半形括號、短橫「–」和連字號「-」，在報告裡我常常看不出差別。後來要求它在「改了什麼」欄用文字描述（「半形→全形」），不要只給我兩個看起來一樣的字串。

中英夾雜的一筆。 台灣作者用中文名發表英文論文，或英文論文有中文譯名，這種一筆裡兩種語言的引用，它會兩套規則都套一點。規則第 5 條要它先判定語言、再套規則、並在備註寫明判定依據。判定錯了我看備註就知道。

斜體會不會活到 Word 裡。 markdown 的斜體是 *…* ，直接貼進 Word 變成星號。這不是校對的錯，是輸出格式的問題（需作者補充：你目前怎麼從 markdown 進 Word，是貼、是轉檔、還是用 Zotero）。目前我的做法是（需作者補充）。

什麼時候讓它改，什麼時候不讓  
第二週做到現在，三個工具各自畫了一條線：

Day 8 摘要卡：它寫，我核對。  
Day 10 查證：它報告，我決定。  
Day 11 校對：它改副本，我換正本。  
三條線不一樣，但判準是同一個：規則越明確、結果越可驗證、動作越可逆，我就讓它做越多。 反過來，只要有一項不成立，它就只能報告。這個判準會一路用到第三週的資料處理，那裡的動作更危險，線也會畫得更緊。

我還不確定的地方  
規則檔會越寫越長。我學校的手冊本身就有二十幾頁，逐條搬進去，規則檔會變得跟手冊一樣難讀。另一個問題是規則檔跟老師的習慣衝突時聽誰的——目前我的答案是聽老師的，然後把老師的版本寫進「本校特別規定」那一節，並註明「指導教授要求」。這樣至少口試時我說得出這條規則是誰定的。

明天  
Day 12：第二週週回顧。五個文獻工具實測一週，省下幾小時、哪些還是得人做，以及三個我到現在還沒答案的問題。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10409750)

[

Day 10｜引用查證器：每一筆參考文獻都要能被打開

](https://ithelp.ithome.com.tw/articles/10409750)[下一篇](https://ithelp.ithome.com.tw/articles/10410477)

[

Day 12｜第二週回顧：五個文獻工具用了一週，老實算帳

](https://ithelp.ithome.com.tw/articles/10410477)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/3/11/c5fb9928-9e67-482d-b3d0-5808698bf688_small.jpg)

[30分鐘用網頁學 Shader？你在開玩笑吧？](https://itplus.ithome.com.tw/webinar-page/198?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

35 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/17/529cbeb4-2673-4483-9026-88ececc25a72_small.png)

[產業 IT 新戰略—完善數據資料免遭勒索加密](https://itplus.ithome.com.tw/vod-page/591?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

34 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/18/8625ae63-9f93-4f47-9a61-ab9c105d341e_small.png)

[「智慧跨域 共創永續」一卡通與政府、企業永續創新思維合作](https://itplus.ithome.com.tw/vod-page/952?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位永續高峰會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/46f5257d-797c-4fdb-82a2-4b8c236682d8_small.png)

[如何用JS建構POS系統 - 發票機跟刷卡機](https://itplus.ithome.com.tw/vod-page/1071?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

21 分