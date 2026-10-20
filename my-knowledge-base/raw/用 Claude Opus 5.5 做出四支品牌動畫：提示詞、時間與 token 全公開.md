---
title: "用 Claude Opus 5.5 做出四支品牌動畫：提示詞、時間與 token 全公開"
source: "https://des13.com/news/ai/1734-claude-opus.html"
author:
  - "[[林小鈴]]"
published: 2026-09-29
created: 2026-09-30
description: "四支品牌動畫影片，全由 Claude Opus 5.5 在 Claude Code 裡寫程式完成，不用剪輯軟體。本文公開每支影片的提示詞、製作時間與 token 用量，包含 iChurch 與益盛科技的 Showreel。一段提示詞加上網址，就能做出 40 秒動態影片，附準備工具，照著做就能開始。"
tags:
  - "clippings"
status: processed
---
益盛科技｜AI 動態影片實作紀錄

這個影片是怎麼做的？

這支影片是我在 Claude Code 裡用程式把動畫寫出來，再輸出成 MP4。

這兩天我們用 **Claude Opus 5.5** 做了四支品牌動畫影片，兩支給 iChurch（ [ichurch.tw](https://ichurch.tw/) ），兩支給益盛科技自己，全程沒有使用剪輯軟體。我們把做法整理成 Threads 串文，發布後 15 小時的數據如下：

**19,840** 瀏覽次數

**14,928** 瀏覽人數

**342** 分享

**40** 引用

**322** 讚

![](https://des13.com/images/2026/claude/ichurch2.webp)

下面把四支影片的提示詞、製作時間和 token 用量完整整理出來，你可以照著做。

## 需要準備哪些工具？

你需要 Claude Code，模型選 Claude Opus 5.5，電腦裡要有 Node.js 和 FFmpeg，並在終端機執行一次 `npx hyperframes skills update` 。HyperFrames 是 HeyGen 開源的工具，會把網頁動畫一格一格截圖輸出成 MP4；Claude 負責研究網站、寫動畫程式和處理配樂，HyperFrames 負責把成果變成影片。

## 四支影片分別怎麼做？

### 1\. iChurch 40 秒橫式 Showreel

![Play: YouTube video player](https://i.ytimg.com/vi_webp/o7KeJ9dI-Es/hqdefault.webp)
![](https://www.youtube.com/watch?v=o7KeJ9dI-Es)

1920×1080一段提示詞25 分鐘

這支只給一段英文，沒有分鏡也沒有素材：

```
Research https://ichurch.tw/
Make a dynamic 40-second motion graphics video on https://ichurch.tw/ that shows what an incredible motion designer you are. Like it's your showreel. Go all out.
```

Opus 5.5 自行打開網站截了 8 個頁面，再用 4,957 間教會的真實座標拼出台灣地圖；縣市長條圖、禱告主題和同工介紹都取自網站內容。

### 2\. iChurch 15 秒直式短片

![Play: YouTube video player](https://i.ytimg.com/vi_webp/AOIeqk_b8-U/hqdefault.webp)
![](https://www.youtube.com/watch?v=AOIeqk_b8-U)

1080×1920詳細分鏡23 分鐘＋修改 4 分鐘

我們寫了詳細分鏡，附上四張 8-bit 角色圖和網站截圖。Opus 5.5 把截圖拆成可以單獨移動的元件，用 HTML 和 GSAP 寫動畫，配樂用 Python 合成原創曲。精簡後的提示詞如下：

```
請用 HyperFrames 做一支 15 秒直式品牌短片（1080×1920、30fps），輸出 MP4。
素材：資料夾裡的網站截圖、Logo、角色圖
0–2 秒：大字「找教會？」，搜尋框自動打字
2–5 秒：網站畫面放大，帶到三個功能
5–9 秒：四個角色輪流登場
9–15 秒：Logo、標語、網址
角色維持原圖不要重畫，網站截圖要拆開做動畫，加上音樂和音效，不要旁白。
```

第一版結尾有個角色擋到網址，修改指令是「小鴿擋到文字，讓他和其他三個角色排成一列」，4 分鐘後完成新版本。

### 3\. 益盛科技 40 秒 Showreel

![Play: YouTube video player](https://i.ytimg.com/vi_webp/kWIr3dUri-U/hqdefault.webp)

1920×1080・60fps同一段提示詞39 分鐘

使用第二支的同一段英文提示詞，只把網址換成 [des13.com](https://des13.com/) 。Opus 5.5 讀完官網，挑出 13 年經驗、1,000+ 完成案件、四項服務、網站保固與永久服務，做成深藍底加網格的科技風，背景音樂取自 Mixkit 免費授權曲庫，並附上授權紀錄。

### 4\. 益盛科技作品集 Showreel

![Play: YouTube video player](https://i.ytimg.com/vi_webp/4mEzLJAlIvw/hqdefault.webp)

1920×1080・60fps中文提示詞41 分鐘＋修改 12 分鐘

```
請先研究並瀏覽 https://des13.com/，重點查看益盛科技過去的網站設計作品、企業官網、電商網站、客製化系統、UI／UX、SEO／GEO 等實際案例。請以這些作品案例為主角，製作一支約 40 秒、高動態感的 Motion Graphics 作品集影片。整體感覺要像一位很強的 Motion Designer 在製作自己的 Showreel。
```

Opus 5.5 打開 8 個客戶的真實網站放進瀏覽器框裡播放，再排出 24 件作品組成的作品牆。第一版網頁捲動較快，修改指令是「不要往下捲動」，12 分鐘後完成新版本。

## 每支影片用了多少 token？

數字是從對話紀錄中把每一次模型回應的用量加總而來，四支都使用 Claude Opus 5.5：

| 影片 | 製作時間 | 輸出 token | 總計 token |
| --- | --- | --- | --- |
| iChurch 15 秒 | 23 分（修改 4 分） | 8.3 萬 | 1,068 萬（修改 176 萬） |
| iChurch 40 秒 | 25 分 | 7.5 萬 | 1,296 萬 |
| 益盛 40 秒 | 39 分 | 6 萬 | 943 萬 |
| 益盛作品集 | 41 分（修改 12 分） | 4.5 萬 | 1,142 萬（修改 394 萬） |

總計中大部分是快取讀取：模型每做一步，都會把前面的對話再讀一次。實際寫出的程式和回覆，是「輸出 token」那一欄。

## 想做自己的影片，要從哪裡開始？

挑一個網站，把第二支那段英文提示詞換上你的網址，貼給 Claude Code 就可以開始。想讓成果照你的規劃走，就像第一支一樣把分鏡寫細；想看 Opus 5.5 自己怎麼拍，就像第二支一樣只給一句話。

製作時間從貼上提示詞算到最後一次模型回應，包含過程中的來回對話。作品集影片中的客戶網站畫面為益盛科技過往作品。

## 今天就可以丟給 Opus 5.5 做的 12 件事：依真正爆紅的內容排名

「這個 Prompt 真的有 1,370 萬觀看？」同事上週看到我整理的數據時這樣問我。

是真的。而且那個 Prompt 只有一句話。

我們這週追了 9/22～9/28 最熱門的 Opus 5.5 使用案例，按照觀看數排名，整理成這份清單。不是理論，是實際爆紅的內容，連第一句 Prompt 都附上去，可以直接拿走用。

---

## 完整排名：觀看數由高到低

1. **動畫歷史影片** （1,370 萬）  
	2 分鐘的動畫，介紹西方文明等主題  
	Prompt：「製作一支 2 分鐘的 \[主題\] 動畫影片，每一幀都用程式碼繪製。」
2. **音樂 MV** （340 萬）  
	5 分鐘的歌曲，耗時 12 小時完成  
	Prompt：「為 \[歌曲\] 製作 MV，每一幕都配合節拍切換。」
3. **UI Morph 動畫** （96.3 萬）  
	一個形狀變成 8 個畫面，全程式碼  
	Prompt：「讓一個形狀依序變成 8 個不同畫面，不要切場、不要淡入淡出。」
4. **串流平台風格紀錄片** （82 萬）  
	高規格紀錄片風格，由模型規劃 MCP  
	Prompt：「製作一支高規格的 \[主題\] 紀錄片，先規劃好每一個場景。」
5. **產品宣傳影片** （70.1 萬）  
	完整產品介紹，15 分鐘完成  
	Prompt：「為 \[產品\] 製作 30 秒宣傳影片，使用真實畫面。」
6. **產品發表影片** （34.1 萬）  
	約 1 分鐘，包含真實 UI  
	Prompt：「為 \[產品\] 製作約一分鐘的發布影片，使用真實 UI。」
7. **Motion Showreel 動態作品集** （27.5 萬）  
	一句 Prompt 就能做出驚豔作品  
	Prompt：「製作一支 15 秒 Showreel，證明你是一位很厲害的 Motion Designer。」
8. **主題解說影片** （19.4 萬）  
	從歷史到現在，全程式碼繪製  
	Prompt：「從起源到現在解釋 \[主題\]，不要素材影片、不要圖片。」
9. **重製廣告影片** （18.7 萬）  
	花費 $1,000+，30 分鐘完成  
	Prompt：「依照參考影片重製這支宣傳片，相同節奏、加入新的程式碼。」
10. **食譜 Motion Graphic** （7.4 萬）  
	單張照片轉成精美食譜動畫  
	Prompt：「把這張照片製成食譜 Motion Graphic。」
11. **商業解說影片** （4 萬）  
	30 秒商業主題，5 個場景、快節奏  
	Prompt：「製作 30 秒解說影片：5 個場景、快節奏，最後放 CTA。」
12. **可玩的 3D 世界** （14 分鐘完成，花費約 $2.54 美元）  
	在單一 HTML 檔中完成  
	Prompt：「在單一 HTML 檔案中建立一個可以玩的 3D \[世界\]。」

---

## 為什麼 Top 3 能爆紅？

看完這份清單，有三件事讓我覺得值得記下來。

第一，Opus 5.5 做的這些影片 **不需要影片生成模型** 。它寫程式碼，程式碼直接繪製每一幀。這跟一般 AI 影片工具的邏輯完全不同。

第二， **最熱門的兩個案例都只從一個 Prompt 開始** 。沒有複雜的提示工程，沒有 n 個步驟。一句話，然後等它跑。

第三， **指令要直接** 。直接告訴它有幾個場景、影片多長、節奏怎麼跑。不要寫「一支充滿創意、讓人感動的影片」，那種描述它不知道要怎麼處理。

---

這份排名每週更新。如果你也在想怎麼把 AI 工具用在實際的網站或行銷內容上，歡迎來聊。

想把 AI 工具實際用在你的品牌或網站上？

益盛科技提供 AI SEO 技術稽核、網頁設計與品牌電商規劃服務，歡迎直接來聊。

[LINE 諮詢](https://line.me/ti/p/@igodos) [查看 AI SEO 服務](https://des13.com/service/seo.html) [查看 網頁設計 服務](https://des13.com/service/rwd.html) [查看 品牌電商 服務](https://des13.com/service/e-commerce.html)