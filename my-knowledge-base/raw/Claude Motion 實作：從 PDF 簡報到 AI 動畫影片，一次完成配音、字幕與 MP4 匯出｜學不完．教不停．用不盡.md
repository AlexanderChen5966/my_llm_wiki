---
title: "Claude Motion 實作：從 PDF 簡報到 AI 動畫影片，一次完成配音、字幕與 MP4 匯出｜學不完．教不停．用不盡"
source: "https://vocus.cc/article/6ac8a933fd897800013ad182"
author:
  - "[[vincent]]"
published: 2026-10-09
created: 2026-10-13
description: "Anthropic 於 2026 年 10 月 8 日正式發表 Claude Motion，這是一項全新的 AI 動畫製作功能，讓使用者只需要透過自然語言描述需求，就能由 Claude 自動撰寫程式碼，將文字、圖表、圖片與各種視覺元素轉換成具有動態效果的短影片。 目前 Claude Motion"
tags:
  - "clippings"
status: processed
---
Anthropic 於 2026 年 10 月 8 日正式發表 ****Claude Motion**** ，這是一項全新的 AI 動畫製作功能，讓使用者只需要透過自然語言描述需求，就能由 Claude 自動撰寫程式碼，將文字、圖表、圖片與各種視覺元素轉換成具有動態效果的短影片。

****Claude Motion 的核心技術：不是 AI 生成影片，而是 AI 編寫動畫程式****

為什麼會看到廣告

Claude Motion 與一般 AI 影片生成工具最大的不同，在於它並不是透過影片生成模型，直接產生一連串影像畫面。它採用的是 ****程式碼驅動的動畫製作方式（Code-driven Animation）**** 。

使用者只需要輸入提示詞（Prompt），Claude 就會分析內容與設計需求，再透過程式碼控制文字、圖形、圖表與圖片的版面配置、動畫效果及播放時序。

這類技術概念可以透過 Remotion、CSS Animation、JavaScript 等工具與技術來理解。不過，官方目前主要將 Motion 描述為由 Claude 自動編寫動畫程式碼，並未明確指出其內部固定使用哪些動畫框架。

相較於以擬真畫面為主的 AI 影片生成工具，Claude Motion 更適合需要精準呈現資訊、數據、流程與概念的動畫內容。

![vocus｜新世代的創作平台](https://images.vocus.cc/f9151d24-81d0-4276-a3f2-4a640de9f7e2.jpg) ![zoomable](https://images.vocus.cc/f9151d24-81d0-4276-a3f2-4a640de9f7e2.jpg)

### 實例演練

我先準備了幾張如下的投影片簡報檔，並且轉換格式為 PDF 檔。（簡報內容是大多由圖片組成的投影片）

![vocus｜新世代的創作平台](https://images.vocus.cc/1c95b50b-2b45-43dc-b49a-fca880a6614e.png) ![zoomable](https://images.vocus.cc/1c95b50b-2b45-43dc-b49a-fca880a6614e.png)

接著，透過 Claude Code 來執行。

先在側邊欄位選取：成品（原來是：Artifact）。然後，在此選取：Motion。

![vocus｜新世代的創作平台](https://images.vocus.cc/0d22d5c9-b67f-486c-858d-30e418adf6d9.png) ![zoomable](https://images.vocus.cc/0d22d5c9-b67f-486c-858d-30e418adf6d9.png)

回到主頁面，即可輸入提示詞： ****將檔案內容製作為教學說明動畫網頁，單一 HTML 檔案製作成 Artifact，需要有自動播放與動態流暢的視覺效果。****

![vocus｜新世代的創作平台](https://images.vocus.cc/ea0e606e-f414-4b1c-8aa6-9b0f50347db6.png) ![zoomable](https://images.vocus.cc/ea0e606e-f414-4b1c-8aa6-9b0f50347db6.png)

Claude Codex 先以對話方式詢問：

- 想要哪一種風格？我選了：插槽模組
- 想用哪組配色？我選了：晨光教室
- 動畫要多長？我選了：Other，並填入：300秒

![vocus｜新世代的創作平台](https://images.vocus.cc/5b47b54e-0e34-43d2-ba3c-96a23c31d3f8.png) ![zoomable](https://images.vocus.cc/5b47b54e-0e34-43d2-ba3c-96a23c31d3f8.png)

接著，開始執行。

![vocus｜新世代的創作平台](https://images.vocus.cc/80154918-f60c-40f4-9d9c-acd2d82fa750.png) ![zoomable](https://images.vocus.cc/80154918-f60c-40f4-9d9c-acd2d82fa750.png)

過程中你可以看到整個 HTML 檔建立的過程，也可以使用視窗下的工具來檢視目前進度的內容。

![vocus｜新世代的創作平台](https://images.vocus.cc/7c987f5f-e45b-471d-89c4-31b834bd1c50.png) ![zoomable](https://images.vocus.cc/7c987f5f-e45b-471d-89c4-31b834bd1c50.png)

結果完成後，檢視這個 HTML 檔，真的是由動畫組成的網頁。播放網頁時，可以聽到有背景音樂，每個動畫也會有音效。到此，已經是一個可以播放的教學影片。

在此，你可以切換 Style 為不同的色彩配置。

![vocus｜新世代的創作平台](https://images.vocus.cc/e8fa011c-57d2-443b-bf5d-044f12b3887c.png) ![zoomable](https://images.vocus.cc/e8fa011c-57d2-443b-bf5d-044f12b3887c.png)

選選：分享，即可「匯出」中選取要下載影片。（也能下載為 GIF 檔）

![vocus｜新世代的創作平台](https://images.vocus.cc/ab6255f6-af71-476f-b3b3-199bcb9a2c9b.png) ![zoomable](https://images.vocus.cc/ab6255f6-af71-476f-b3b3-199bcb9a2c9b.png)

在此可以選取匯出的影片格式，便可進行下載。

![vocus｜新世代的創作平台](https://images.vocus.cc/4902b03a-0aea-42da-8a76-ae44bb59a87b.png) ![zoomable](https://images.vocus.cc/4902b03a-0aea-42da-8a76-ae44bb59a87b.png)

本例下載的 GIF 檔：

![vocus｜新世代的創作平台](https://img.vocus.cc/TjgN65oDNkU_XFkqTkkO0JCg23OMLE8AYB5kPzgkdIM/f:gif/plain/https://images.vocus.cc/b727fbe5-a11c-4674-9670-f374c7fb55b9.gif) ![zoomable](https://img.vocus.cc/TjgN65oDNkU_XFkqTkkO0JCg23OMLE8AYB5kPzgkdIM/f:gif/plain/https://images.vocus.cc/b727fbe5-a11c-4674-9670-f374c7fb55b9.gif)

### 下載 HTML 檔

接著，我請 Claude Code ****提供這個已完成的單一 HTML 檔**** 。

![vocus｜新世代的創作平台](https://images.vocus.cc/0573afea-0266-417f-9a28-bddc85992a39.png) ![zoomable](https://images.vocus.cc/0573afea-0266-417f-9a28-bddc85992a39.png)

開啟這個 HTML 檔執行，看起就像影片一樣。

![vocus｜新世代的創作平台](https://images.vocus.cc/9c5a978f-b102-45cd-a7d3-a5b36aaad443.png) ![zoomable](https://images.vocus.cc/9c5a978f-b102-45cd-a7d3-a5b36aaad443.png)

Claude Code 回應：

單一 HTML 檔已完成：AI三大核心教學動畫.html（約 97 KB，存在桌面的 Claude\_Code 資料夾）。

- ****播放方式：**** 檔案裡已包含全部 23 個場景、配樂和播放器，不需要網路，雙擊就會在瀏覽器裡自動開始播放，長度約 5 分鐘。
- ****聲音：**** 瀏覽器不允許網頁自動出聲，要先按「開啟聲音」（或按 M 鍵）才會有配樂。
- ****操作方式：****
- - 進度條可以拖曳，下方的章節按鈕可以直接跳到某一段。
		- 有「循環播放」和「全螢幕」按鈕（或按 F 鍵）。
		- 空白鍵是播放／暫停，左右方向鍵是倒退或前進 5 秒。
- ****配樂：**** 這個檔案用的是我另外寫的簡易合成器，音色和網頁版比較起來會稍微簡單一些。
- ****有修正一處：**** 「三大核心」那一段的「Project＝工作空間」原本會斷成兩行、和下方文字重疊，已經縮小字級改好，網頁版也一起改了。
- ****檢查範圍：**** 我在瀏覽器實際播放抽查了開場、三大核心、何時用技能、三平台技能、Project 是什麼、三者協作這幾段，畫面正常，也沒有程式錯誤。

### 製作影片

接著，我想要讓 Claude Code 製作具有語音播放的影片。

使用以下提示詞：

```javascript
根據這個 HTML 檔的內容產生：
1.解說內容的旁白配音，語速中等，語音採用預設的語音。
2.解說內容的旁白文字逐字稿，依語氣適當的切割文字，讓每次出現的文字
不超過一個畫面能容納的字數。
3.影片中不要燒錄旁白文字。
4.要產生含有語音簡報内容的 MP4 影片檔(影片中含有語音配音)，該影片
必須能在 Windows 中以預設的媒體播放器播放。 
5.產生將簡報影片可以放到 YouTube 上使用的字幕檔。
```

看起來，Claude Code 很簡單就完成了這個工作。

![vocus｜新世代的創作平台](https://images.vocus.cc/a136e798-d4d4-4bbf-a8f4-a846b0409d07.png) ![zoomable](https://images.vocus.cc/a136e798-d4d4-4bbf-a8f4-a846b0409d07.png)

順利取得：

- AI三大核心教學影片.mp4：6 分 31 秒，1920×1080，30fps，約 45 MB
- AI三大核心教學影片.srt：YouTube 字幕檔，共 135 段
- AI三大核心教學影片\_旁白逐字稿.md／.txt：依 23 個場景分段的逐字稿，每段標了開始時間
1. ****旁白配音：**** 用 edge-tts 的台灣華語預設女聲 HsiaoChen，語速是預設的中等速度，內容依網頁的 23 個場景撰寫。
2. ****字幕切段：**** 依標點和語氣切開，每段最多約 18 個中文字（含英文的段落最長 31 個字元），一行就放得下。
3. ****影片中沒有燒錄旁白文字：**** 畫面只有動畫原本的內容。
4. ****播放相容性：**** 影片是 H.264（yuv420p）加 AAC 立體聲，這是 Windows 預設播放器支援的格式。
5. ****上傳字幕到 YouTube：**** 在 YouTube Studio 的「字幕」裡選「上傳檔案」，再選「有時間碼」，然後選這個 SRT 檔即可。

順利上架 YouTube ：

![vocus｜新世代的創作平台](https://images.vocus.cc/a5eca1d7-9899-4115-a001-fadd5692ce8b.png) ![zoomable](https://images.vocus.cc/a5eca1d7-9899-4115-a001-fadd5692ce8b.png)