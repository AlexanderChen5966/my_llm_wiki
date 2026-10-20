---
title: "Day 11 - 用 Canvas 視覺化圖譜 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10414163"
author:
  - "[[iThome]]"
published:
created: 2026-09-23
description: "前言 編輯器與知識圖譜是使用者看得最久的兩個元件，而它們踩的是同一個坑：狀態該歸誰管。 編輯器這邊的答案是「盡量不要有狀態」，圖譜那邊則是「狀態要自己管，不要交給 React 的渲染週期」。 兩個模式..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 11

0

[佛心分享-SideProject30](https://ithelp.ithome.com.tw/2026ironman/side-project-30)

### 為你自己蓋一座會複利的知識庫——WikiBrain系列 第 11 篇

## Day 11 - 用 Canvas 視覺化圖譜

- 分享至

## 前言

編輯器與知識圖譜是使用者看得最久的兩個元件，而它們踩的是同一個坑： **狀態該歸誰管** 。

編輯器這邊的答案是「盡量不要有狀態」，圖譜那邊則是「狀態要自己管，不要交給 React 的渲染週期」。

## 兩個模式，不要做花俏的即時預覽

我沒有做左右分割的即時預覽，只有兩個模式：閱讀跟編輯，一顆按鈕切換。

理由是這座 wiki 大部分時間是拿來 **讀** 的。頁面是 agent 寫的，你多半只是查東西，偶爾才手動改一段。分割畫面會讓兩邊都變窄，而且大多數時候右邊那半是浪費的。

閱讀模式做了幾件讓內容更好讀的事：把內文開頭跟頁面標題重複的那個 H1 藏起來、 `[[連結]]` 渲染成可以點的連結、 `[@引用鍵]` 渲染成（作者, 年份）並連到來源頁、頁尾自動長出參考文獻、數學公式用 KaTeX、Mermaid 圖直接畫出來。

那個「藏掉重複 H1」是小事，但沒做的時候每一頁都會看到標題出現兩次，很煩。

*閱讀模式。側欄是三層資料夾的檔案總管，右邊是反向連結。*

## 版本比較

每一次儲存都有快照，所以版本歷史是現成的。比較差異則是自己寫的，一段 LCS 行比對：

- 找出兩個版本的最長共同子序列，剩下的就是新增與刪除
- 前後綴先修剪掉，大部分修改只動幾行，這樣可以省掉絕大部分的計算
- 相同的段落預設折疊，只展開有變動的地方
- 超大的頁面直接退化成整段替換，不做精細比對

沒有用現成套件，因為需求很單純，而且我不想為了一個對話框拉進一個相依套件。

比較視窗裡可以直接復原到那一版。這裡有個容易做錯的地方： **復原不是把版本號改回去，而是拿舊版的內容建立一個新版本** 。歷史永遠只往前長，沒有任何一筆會消失。這樣「誰在什麼時候把它改回舊版」也留在紀錄裡。

## 草稿：二十行擋掉最生氣的那種遺失

[之前講過衝突時不能丟掉使用者的字](https://ithelp.ithome.com.tw/articles/10412617) 。同樣的原則還有另一個更常見的情境： **使用者根本還沒按儲存** 。

分頁被關掉、瀏覽器崩潰、手機切到別的 app 回來被回收、不小心點到返回。這些都會讓正在打的字消失，而且完全不是使用者的錯。

現在的做法是編輯器打字的時候持續把內容存進 localStorage，用筆記路徑當鍵，五百毫秒的防抖：

```ts
const draftKey = \`wb-draft:${note.path}\`;
// …打字時防抖寫入
localStorage.setItem(draftKey, draft);
```

*出處： [`web/src/components/Editor.tsx:10-18`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/web/src/components/Editor.tsx#L10-L18)*

用路徑當鍵是有意的：同一個工作區開了三個分頁分別編三頁，三份草稿不會互相蓋掉。

儲存或取消的時候清掉。下次再打開這一頁，如果 localStorage 裡有草稿而且跟伺服器上的內容不一樣，就在編輯器上方跳出一列：「有未儲存的草稿」，兩個按鈕，還原或捨棄。

實作大概二十行，但它處理的是使用者最生氣的那種資料遺失：系統沒有壞，字也確實打了，但它就不見了。

## 圖譜第一版：每次都重建

編輯器的答案是把狀態盡量推掉：沒有即時預覽、比對用最簡單的演算法、草稿丟給瀏覽器保管。圖譜剛好相反：它有一個持續在跑的物理模擬，狀態推不掉，只能想清楚由誰保管。

最直覺的寫法是把圖譜當成一般的 React 元件：資料變了就重新渲染，重新渲染就重新建立模擬、重新掛事件、重新開動畫迴圈。

單純顯示的時候看不出問題。但我加了兩個功能之後就爆了：

- **時間軸** ：拉一條軸，看知識庫從第一天到今天怎麼長出來，還能按播放自動前進
- **篩選** ：只看某一層、只看某個資料夾、搜尋標題、隱藏孤立的點

這兩個功能都會頻繁改變「現在該顯示哪些節點」。每改一次就重建一次模擬，結果是所有節點的位置歸零重算，畫面在肉眼看來就是 **一直閃** 。按播放的時候尤其明顯，像壞掉的燈管。

## 第二版：持久模擬

改法的核心觀念是： **canvas、事件監聽、動畫迴圈只建立一次，之後永遠不重建** 。

模擬的狀態放在一個 ref 裡，資料變的時候不是重來，而是同步差異：

```ts
// Simulation state lives in a ref: graph changes (timeline playback, filters) only sync nodes and edges
// without rebuilding the canvas or animation loop -> no flicker
// Sync: new nodes are placed near their neighbours (or at a remembered position), vanished nodes are removed
const neighbour = graph.edges.map(e => (e.from === g.path ? e.to : e.to === g.path ? e.from : null))
  .map(p => (p ? byPath.get(p) : undefined)).find(Boolean);
const bx = prev?.x ?? (neighbour ? neighbour.x + (Math.random() - .5) * 60 : W / 2 + (Math.random() - .5) * 220);
const by = prev?.y ?? (neighbour ? neighbour.y + (Math.random() - .5) * 60 : H / 2 + (Math.random() - .5) * 180);
```

*出處： [`web/src/components/GraphView.tsx:76-94`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/web/src/components/GraphView.tsx#L76-L94)*

`prev?.x ?? (neighbour ? … : 隨機)` 這一串就是「不閃」的全部： **先用這個節點上次的位置，沒有的話放在它鄰居旁邊，都沒有才隨機擺** 。還在的節點連速度都留著，所以拉時間軸的時候既有的點穩穩待著，新的點從鄰近處長出來，整張圖是「生長」而不是「重擲」。

時間軸還有一個同類的坑：播放按鈕一開始完全不動。原因是軸的上界我寫成「現在」，而「現在」是每次渲染時用 `Date.now()` 算的。播放時每一幀都在渲染，所以上界每一幀都往前跳一點，進度的比例被重新計算，看起來就是原地踏步。修法是載入時把上界固定下來，程式裡留了一行註解：

```ts
// max is fixed at graph load time (must not call Date.now() on every render,
// or the play loop and the "now" check keep resetting)
```

*出處： [`web/src/components/GraphView.tsx:39-40`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/web/src/components/GraphView.tsx#L39-L40)*

**你以為是常數的東西其實在動** ，這類 bug 不會報錯，只會讓畫面行為怪怪的。同型的還有把 `new Date()` 放進依賴陣列、用 `Math.random()` 當 key。

這個模式其實不限於圖譜。任何有連續狀態的視覺化（動畫、模擬、遊戲）都不該讓框架的渲染週期決定它的生命週期。React 管 DOM，模擬自己管自己，兩邊透過一個同步函式溝通。

![知識圖譜：節點依連入數決定大小，左側有篩選面板，下方是時間軸](https://ithelp.ithome.com.tw/upload/images/20260920/20184129adecadcuv1.png)

*篩選面板與時間軸都會頻繁改變顯示的節點，這正是第一版會閃爍的原因。*

## 圖大了怎麼看

節點超過幾十個之後，全部標上文字就是一團糊。處理方式有幾層：

- **只標樞紐** ：節點多於六十個時，只標連結數前 15% 的節點，加上目前這一頁跟滑鼠指到的那一個
- **放大就全標** ：縮放超過 1.6 倍代表使用者在細看，這時候全部標出來
- **半徑用平方根成長** ：連結多的點大一些，但不能大到吃掉整張圖，所以取平方根再設上限
- **焦點模式** ：以目前這一頁為中心，只看一層或兩層鄰居。這是實際上最有用的一個，因為大部分時候你想問的是「這一頁跟什麼有關」，不是「整座知識庫長怎樣」

篩選狀態記在 localStorage，按鈕上顯示「顯示數／總數」。不顯示的話，下次打開會以為圖譜少了東西，其實是上次的篩選還在。

## 效能

節點超過一百五十個之後，斥力計算只算近距離的鄰居，遠的直接忽略。標準做法是用四叉樹做 Barnes-Hut 近似，我用了更笨但夠用的版本：距離超過門檻就跳過。

判斷依據是這個產品的實際情況：個人知識庫幾百頁就算多了，寫一個四叉樹的複雜度換來的收益，不如把時間花在焦點模式上。

## 順便：資料表檢視

同一批工作裡我還做了一個資料表檢視，把 front matter 當成欄位：所有 `raw/sources` 底下的頁面，作者、年份、期刊、DOI 各成一欄，可以篩選、排序、分組、複製成 CSV。

寫了一個小剖析器處理 front matter 的幾種寫法：純量、行內清單、區塊清單、布林、數字，還有值裡面帶引號跟逗號的情況。研究者模版套用之後，這個表預設就是文獻表。

![資料表檢視：把 front matter 的欄位變成表格的欄，可以篩選與排序](https://ithelp.ithome.com.tw/upload/images/20260920/20184129TY730bUMIp.png)

同樣的資料，圖譜看關係，表格看屬性。兩種檢視回答的是不同的問題。

## 順手做的小事

**確認對話框全部換掉。** 原生的 `confirm()` 和 `prompt()` 又醜又不能自訂，而且在某些行動瀏覽器上表現很怪。換成自己的元件之後，危險操作可以用紅色、可以帶輸入欄（刪帳號要打密碼）、可以用 Esc 關掉。

**Toast 移到畫面底部。** 原本在頂欄下方，結果它會蓋住「新增」按鈕跟編輯器工具列，端對端測試因為點不到按鈕而失敗，這才發現真的使用者也會被擋到。

**指令面板。** ⌘K 打開，可以直接跳到任何一頁或執行動作。完全是前端在已經載入的檔案樹上做篩選，所以是瞬間的。

## 小結

編輯器能不留的狀態就不留：沒存的字交給 localStorage，分頁關了也拿得回來。圖譜反過來，模擬收進 ref、畫布只建一次，拉時間軸、切篩選都只是同步差異——還在的節點連速度都留著，新節點從鄰居旁邊長出來，畫面不再閃。真正的坑是看起來像常數的東西：「現在」其實每幀都在動，播放鍵才會原地踏步。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10413861)

[

Day 10 - 用 Vite 渲染 SEO

](https://ithelp.ithome.com.tw/articles/10413861)[下一篇](https://ithelp.ithome.com.tw/articles/10414909)

[

Day 12 - i18n 與深色模式

](https://ithelp.ithome.com.tw/articles/10414909)

系列文

[為你自己蓋一座會複利的知識庫——WikiBrain](https://ithelp.ithome.com.tw/users/20184129/ironman/9590) 共 13 篇

目錄

1. 9
	[Day 09 - 用資料庫存 wiki 連結的邊](https://ithelp.ithome.com.tw/articles/10413279)
2. 10
	[Day 10 - 用 Vite 渲染 SEO](https://ithelp.ithome.com.tw/articles/10413861)
3. 11
	[Day 11 - 用 Canvas 視覺化圖譜](https://ithelp.ithome.com.tw/articles/10414163)
4. 12
	[Day 12 - i18n 與深色模式](https://ithelp.ithome.com.tw/articles/10414909)
5. 13
	[Day 13 - 抓取網頁來源，匯入知識庫](https://ithelp.ithome.com.tw/articles/10415534)
[完整目錄](https://ithelp.ithome.com.tw/users/20184129/ironman/9590)

[![.](https://itadstatic.ithome.com.tw/B9/1789958815_6ab09a9f3bf4f.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/11/12/8ddb4075-cb3c-4b80-be90-78a42e0bc603_small.png)

[Citrix 多工虛擬化解決方案協助企業增進生產力](https://itplus.ithome.com.tw/vod-page/882?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/8/8/acad899b-f06a-4f28-9c8e-4ba4f62eb2c7_small.jpg)

[從遠古到現代：Linux 系統管理的大躍進之路](https://itplus.ithome.com.tw/webinar-page/269?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/4/9/696ceba6-85e0-4c32-890d-083cdd48cc48_small.png)

[低碳關鍵：建構線上碳流資訊平台的重要性](https://itplus.ithome.com.tw/vod-page/1136?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位永續高峰會 |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/aa6d8015-e4f4-4b47-854e-7edd38eac52d_small.png)

[高效治理新解方：電子簽章、AI 智慧文件與數據驅動決策](https://itplus.ithome.com.tw/vod-page/1250?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

27 分