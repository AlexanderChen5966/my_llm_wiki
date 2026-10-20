---
title: "Claude Motion是什麼？AI動畫生成教學、3步驟操作與方案限制"
source: "https://www.bnext.com.tw/article/92639/claude-motion-three-step-tutorial"
author:
  - "[[李先泰]]"
published: 2026-10-15
created: 2026-10-16
description: "Anthropic推出Claude Motion，可將試算表、文件轉成可編輯的動畫，支援MP4匯出。本文整理3步驟操作教學、應用場景與方案限制。"
tags:
  - "clippings"
status: processed
---
Anthropic於2026年10月8日推出 [Claude Motion測試版](https://claude.com/resources/articles/dashboards-and-motion) ，這是一款AI動畫製作工具，能將試算表、文件及圖片轉換成可編輯的動態影片，並支援MP4匯出。使用者不需要學習動畫軟體，只要上傳資料、輸入指令，就能讓Claude自動生成帶有圖表、文字與轉場效果的動畫。

過去想把試算表數據做成動畫影片，通常得使用 [Adobe After Effects](https://www.adobe.com/products/aftereffects.html) 等專業軟體，或委託外包。 **Claude Motion讓使用者只要上傳試算表、輸入文字指令，就能透過AI生成動畫影片** ，還能直接圈選畫面修改文字、圖表與動畫效果，不必整支影片重新製作。

## Claude Motion是什麼？與一般AI影片生成工具有何不同？

Claude Motion產出的是一段程式碼，Claude依你的提示寫出程式，由程式讓文字、圖表、形狀和圖片動起來，像短影片一樣播放。畫面裡的文字、數字與播放時序，都能單獨修改。例如把Q3的數字改成18%，其餘畫面不必重做。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

也因為它本身不用影片生成模型，不會直接生出擬真人物或實拍風格的影片素材，官方文件把這點講得很直白。它能做的是用程式畫出示意人物與圖表、文字、形狀的動態，再搭配你提供的圖片或外部生成的影片素材，這也正是簡報裡最常需要動起來的東西。

## Claude Motion怎麼用？3步驟生成AI動畫並匯出MP4

入口在Claude介面左側「成品」（Artifacts）選Motion，或在訊息框輸入/motion。

![Claude Motion](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/zzwa-1791971265.png?w=2000&output=webp)

左側邊欄點擊「成品」，就可看見Motion的選項。

圖／ Claude

使用前注意：\*\*截至2026年10月14日，Claude Motion僅開放Team與Enterprise方案測試，Pro與Max個人訂閱用戶尚無法使用。

### 步驟1：上傳試算表或文件，設定動畫內容與長度

官方文件的建議很具體： **說明誰會看、在哪裡播、大約多長，並附上你自己的報告、資料或圖片，動畫才會用你自己的內容做。** 建議第一次下指令就把這三件事寫進去，範本如下（編輯依官方建議自擬，非官方提供的指令）：

```
附件是我們第三季的營收試算表。請做一支30秒的動畫，在全員大會上播放，觀眾是非財務部門的同事。重點放在三個區域的成長差異，最後一幕用一句話講結論。
```

只丟檔案、只說「解釋這些結果」也跑得出一版，圖表型態與結論全由Claude決定。多交代場合與觀眾，是官方文件明列的「獲得更好結果的提示」。

### 步驟2：提供參考圖片，調整動畫風格

第一版的風格不合意時，可以直接提供參考圖片。找一支你喜歡的動畫，例如Anthropic官方做的示範，截幾張畫面，開新對話把截圖連同原本的指令一起丟進去重跑。示範影片裡用這招做出的第二版，明顯比第一版更接近專業成品。 **形容風格很難，給參考最快。**

範本如下（編輯依示範流程自擬，非官方提供的指令）：

```
這邊附上5張截圖，是我喜歡的動畫風格。請用這個配色、字體與轉場節奏，重做上一支動畫，內容不變。
```

### 步驟3：圈選修改動畫，匯出MP4影片

看第一版時，不用重新描述整支影片。右側預覽可以直接改文字；要改動畫本身，就用留言工具圈選那個元素，寫一句話送出，Claude針對被圈的那一處修改。例如官方文件列出的「減慢第二個場景」、「將Q3數字變更為18%」。

改到滿意，按「匯出」選MP4下載。官方公告另列出Adobe、Descript、HeyGen、Higgsfield、invideo、Luma AI、Runway七個可以接續編輯的工具，Canva與Captions待推出。

## Claude Motion可以做什麼？3大實用應用場景

除了將試算表轉成數據動畫，Claude Motion也能應用在業務簡報、影片製作及個人紀念影片等場景。以下整理3種實用做法：

- **業務與客戶場景** ：官方範例還有定價方案的比較動畫，給業務啟動會用，以及新客戶上線的設定步驟演示。
- **對齊你自己的影片** ：先把影片片段丟給 [Gemini](https://gemini.google.com/) 這類工具，請它產生帶時間碼的SRT字幕檔，再連同風格截圖一起貼給Motion，讓動畫對上你說話的時間點。畫面比例記得講清楚是16:9。
- **紀念影片開頭** ：上傳20年來去過的地點清單，做成呈現世界各地足跡的動態圖像，後面再接實拍片段。

## Claude Motion免費嗎？支援方案與使用限制一次看

截至2026年10月14日，Motion只開放Team與Enterprise方案測試，Pro與Max個人方案尚未開放。Team預設開啟，Enterprise預設關閉，要由擁有者在組織設定中打開。

| Claude方案 | Claude Motion使用權限 |
| --- | --- |
| Free（免費版） | ❌ 不支援 |
| Pro | ❌ 不支援 |
| Max | ❌ 不支援 |
| Team | ✅ 支援，預設開啟 |
| Enterprise | ✅ 支援，需由組織擁有者開啟 |

要注意的是，不是什麼素材都能無中生有。例如AI影片平台 [Higgsfield](https://higgsfield.ai/) 的示範影片裡那些實拍素材、卡通角色與配音，是另外用Higgsfield生成，而且是在Claude Code接上它的MCP連接器、再加自訂skill才串起來的流程；Motion負責的只有可編輯的動態圖像。

可以說，Motion問世之後會動的簡報不難做，更難的是一開始就說清楚要給誰看。

> 延伸閱讀： [7個月造出Muse！Meta首席AI長揭密：如何改押個人AI代理，再用100項評測實現「魔法時刻」？](https://www.bnext.com.tw/article/92613/meta-muse-alexandr-wang-personal-agent-7-months)

資料來源： [Anthropic 官方公告](https://claude.com/resources/articles/dashboards-and-motion) 、 [Claude 支援文件](https://support.claude.com/zh-TW/articles/17454997-%E9%96%8B%E5%A7%8B%E4%BD%BF%E7%94%A8-claude-motion) 、 [Claude Dashboards 支援文件](https://support.claude.com/en/articles/17454700-get-started-with-claude-dashboards) 、 [Paul J Lipsky 影片](https://www.youtube.com/watch?v=hbPvZvpu9KU) 、 [Higgsfield AI 影片](https://www.youtube.com/watch?v=5HYYXYYCrL4)

本文初稿為AI編撰，整理．編輯/ 李先泰