---
title: "Images 2.6 拼貼動畫教學：Codex 五秒 B-roll 實作"
source: "https://www.aiposthub.com/images-2-6-codex-collage-broll-tutorial/"
author:
  - "[[Philo]]"
published: 2026-11-06
created: 2026-11-10
description: "Images 2.6 拼貼動畫教學，從一句口播拆成視覺隱喻，到透明紙片、定格組裝與 5 秒無聲 MP4。附實作成片、可複製提示詞、原版 Codex Skill 安裝方式，以及官方 GIF 圖片與影片示範。"
tags:
  - "clippings"
status: "inbox"
---
Images 2.6 拼貼動畫教學，可以先從一個很小的任務開始：把「一句想法，組裝成一支短片」做成五秒畫面，墊在口播下面。這類補充畫面就叫 B-roll，適合在說話時呈現一個動作或比喻。

你可能看過那種黑白人物配彩色卡紙的影片。燈泡、膠片、剪刀從畫面外一件件滑入，最後組成海報般的構圖。因為每件物品都有自己的出場順序，所以看起來像有人逐格搬動紙片。

這次我們先完成一支直式無聲影片，再回頭拆解做法。素材由 Codex 內建生圖工具產生，動畫則用可逐層修改的時間線完成。Images 2.6 的新功能介紹，依據 OpenAI 官方公告與文件。

**這支成片是生圖素材加上分層動畫的實作，並非指定 Images 2.6 API 的測速或畫質測試。** API 是讓程式呼叫模型的介面，而本次內建生圖工具沒有提供可核對的模型識別資訊。

如果想照著做，先把一句話變成一個看得懂的畫面，再處理動畫，會比同時要求模型理解意思、設計構圖和安排動作更容易修改。這也是原作者把流程拆成幾個確認階段的原因。

## Images 2.6 更新了什麼，和拼貼動畫有什麼關係？

Images 2.6 提供產圖、編圖與 GIF 動畫能力，因此可以用來準備拼貼素材，也能探索短循環動作。OpenAI 在 2026 年 11 月 4 日發布這次更新，產品名稱是 Images 2.6。API 的模型名稱則分成 Flare 與 Sunburst。

依照 [官方模型選擇指南](https://developers.openai.com/api/docs/guides/image-prompting?ref=aiposthub.com) ，Flare 偏向快速嘗試，Sunburst 適合對細節、構圖或修改精準度要求較高的工作。兩者都能處理透明背景，所以做獨立紙片時，可以把「透明底」直接列入要求。

對創作者來說，這代表可以先快速比較幾種視覺方向，再把時間花在想留下的構圖。因為同一支片通常還會改顏色、姿勢或物件，第一次生圖後仍要逐項檢查，尤其是紙片之間的關係。

![Images 2.6 官方 GIF 動畫示範集錦](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/release-montage.webp)

Images 2.6 官方發布頁的動畫案例集錦。 來源： OpenAI 。

GIF 是把幾張圖片依序播放、常用來循環展示的動畫格式。官方發布頁展示了角色反應、手繪貓頭鷹變成動畫等案例，適合用來理解這次更新想服務的創作情境。

<iframe src="https://player.vimeo.com/video/1242918503?h=5974be9997&amp;badge=0&amp;autopause=0&amp;controls=1&amp;autoplay=0&amp;loop=0&amp;muted=0" title="OpenAI Images 2.6 官方 GIF 動畫集錦" width="960" height="480" allow="fullscreen;picture-in-picture" allowfullscreen=""></iframe>

OpenAI 官方發布頁嵌入的 Images 2.6 動畫集錦，保留原始播放器。來源： [OpenAI](https://openai.com/index/introducing-images-2-6/?ref=aiposthub.com) 。

但「能生成 GIF」和「已經完成一支能剪進短影音的 MP4」之間，還有輸出格式、時間長度與動作控制等選擇。下面這支示範採用分層動畫，目的是讓每張紙片的位置和出場時間都能重新調整。

## 先看成片：一句想法如何組成五秒紙拼貼？

這支片把「想法變成短片」拆成六件物品：思考中的人、燈泡、膠片、剪刀、箭頭和完成的影片卡。每件物品只負責一個意思，所以觀眾不需要讀文字，也能沿著組裝順序看懂畫面。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/11/collage-broll-5s.mp4" width="720" height="1280" title="原創拼貼 B-roll：一句想法，組裝成一支短片" controls=""></video>

AI 郵報本次原創示範：AI 生成紙片素材，搭配分層時間線輸出的五秒無聲影片。此片非 OpenAI 官方示範。

[下載本次可編輯專案與素材包（ZIP，約 6.4 MB）](https://www.aiposthub.com/content/files/2026/11/collage-broll-editable-pack.zip) ，包含六張透明紙片、動畫原始碼、MP4 與重製說明。

影片從青綠空場開始，先放入膠片，再讓人物和燈泡進場。接著剪刀代表製作，箭頭把注意力帶到成品卡，最後停住，讓口播有時間把句子說完。

以下是實際產出的檔案規格，方便你判斷能否放進自己的剪輯專案。

| 項目 | 本次成品 |
| --- | --- |
| 畫面 | 直式 9:16，720 × 1280 |
| 長度 | 5 秒 |
| 播放格數 | 每秒 24 格，共 120 格 |
| 聲音 | 零音軌，可直接墊口播 |
| 素材 | 一張六格透明素材圖，切成六張獨立紙片 |
| 動畫 | HyperFrames 時間線，逐件滑入並落位 |

因為這些物件各自保留成透明圖片，改動時就能只換其中一層。例如把燈泡換成指南針，就能把「有了想法」改成「找到方向」，其餘紙片不必全部重生。

本次沒有執行原專案的 Gemini Omni 影片生成。環境自檢缺少對應的 API 金鑰和指定執行環境，所以我們改用分層動畫完成這支片。

## 拼貼動畫教學第一步：把口播縮成一個畫面

先決定畫面要讓人看懂什麼，再挑物件。因為五秒內能辨識的資訊有限，一句話只保留一個主要比喻，通常比把每個名詞都畫出來清楚。

以這次的口播為例，核心意思是「把腦中的想法做成看得見的成品」。所以人物負責起點，燈泡代表想法，膠片和剪刀交代製作，箭頭把視線送到完成卡。

你可以先把下面這段貼給 Codex，只做文字規劃，暫時不花生圖和影片生成的用量。

```
我要把這句口播做成 5 秒紙拼貼 B-roll：
「一句想法，組裝成一支短片。」
先只提出一個視覺隱喻，不要生成圖片或影片。
請寫出核心意思、情緒、3 到 6 個關鍵物件、
底色、物件的相對位置，以及逐件進場順序。
不要把口播文字放進畫面，等我確認再繼續。
```

確認時可以把所有物件念一遍，再問自己：「把字幕拿掉，這幾件東西的關係還清楚嗎？」如果需要長篇解釋，先刪掉不必要的紙片或換一個比喻，後面會比較好做。

底色也可以跟意思一起選。本次是創作與組裝，所以使用青綠。若要講反覆消耗時間，可以試焦橙配時鐘。這是視覺設計上的選擇，沒有任何模型要求特定情緒一定要配某種顏色。

## 第二步：生成能分開移動的透明紙片

要逐件滑入，就先讓物件能夠分開。這次把六件素材放在同一張規則排列的圖上，因為風格可以一起生成，之後也容易切成獨立圖片。

我們使用的素材圖是三欄兩列，每格放一件物品。人物、膠片和剪刀以黑白網點印刷呈現，燈泡、箭頭和影片卡用彩色紙張，外緣留奶油白裁邊。

![拼貼動畫使用的六件透明紙片素材：人物、燈泡、膠片、剪刀、影片卡、箭頭](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/collage-sheet.png)

本次實際產生的六格素材圖。透明通道保留在原始 PNG 中。 AI 郵報／Codex 內建生圖工具，AI 生成示意圖。

下面的提示詞可以直接複製。英文中的 halftone 指印刷網點，也就是用一顆顆點組成明暗的效果。cut-out 則是剪下來、能獨立移動的紙片。

```
Create exactly six isolated paper-collage cut-outs on a
transparent background, arranged in a precise 3-column
by 2-row grid. Use a 1536 × 1024 canvas. Each object must
stay entirely inside its own 512 × 512 cell, with clear
transparent gaps and no overlap between cells.
Reading order:
1. A thoughtful black-and-white halftone editor, head and torso.
2. A mustard-yellow paper lightbulb.
3. A curling black-and-white 35mm film strip.
4. Black-and-white halftone scissors.
5. A cream video card with a teal landscape and play symbol.
6. A vermilion paper arrow pointing right.
Printed paper texture, warm cream cut edges, restrained
soft shadows, consistent flat editorial collage style.
No text, letters, numbers, logos, watermark or background panel.
```

生圖後先放大檢查每格邊緣。物件如果跨進鄰格，切圖時就可能把它剪斷。透明底若被畫成灰白棋盤格，放進彩色背景後也會露出方框。

因此，除了看預覽，還要確認檔案含有透明通道。這個通道記錄每個像素的透明程度，本次原圖確實包含透明資料，才進行後續切片。

若工具沒有照指定尺寸輸出，先查實際寬高再切圖。下面指令適用於本次的 1536 × 1024 素材，會取出左上角的人物。

```bash
ffmpeg -i collage-sheet.png \
  -vf "crop=512:512:0:0" -frames:v 1 person.png
```

同一列的起點依序是 x=0、512、1024，第二列的 y=512。因為每件物品都完整留在自己的格子裡，切完後就能在時間線上各自移動。

如果你只想沿用原作者的 Gemini 路線，則應生成一張完成構圖，交給影片模型當尾幀。本節的透明拆層，是為了製作可精確調整的動畫。

## 第三步：把紙片排進可編輯時間線

在分層路線中，AI 負責素材，時間線負責動作。HyperFrames 是把網頁畫面輸出成影片的工具，因此 Codex 可以修改紙片的位置和播放時間，再產生 MP4。

不熟程式時，可以先用自然語言要求 Codex 建立專案，讓它把規則寫進檔案。

```
請用這六張透明紙片製作 HyperFrames 動畫。
畫布 720×1280，長度5秒，每秒24格，無音軌。
青綠底色 #126D69，0到0.25秒保持完全空場。
紙片依序滑入：膠片、人物、燈泡、剪刀、箭頭、成品卡。
每件用0.55秒進場，以8個離散姿態呈現定格紙片質感。
3.8秒前完成組裝，剩下時間保持最終構圖。
固定鏡位，不縮放、不變形、不增加文字。
保留每件紙片的獨立圖層，輸出前先檢查並提供抽幀圖。
```

本次的進場節奏如下。因為每件紙片的移動大約半秒就結束，觀眾能先辨識上一件，再接著看到下一件。

| 紙片 | 開始時間 | 動作 |
| --- | --- | --- |
| 膠片 | 0.25 秒 | 從右側滑入 |
| 人物 | 0.85 秒 | 從左側滑入 |
| 燈泡 | 1.45 秒 | 從上方落位 |
| 剪刀 | 2.05 秒 | 從左下進場 |
| 箭頭 | 2.70 秒 | 往右推進 |
| 成品卡 | 3.25 秒 | 從右下進場，接住視線 |

動畫曲線使用 `steps(8)` ，意思是把一次移動拆成八段停格。因為每個姿態會短暫停住，所以效果比較接近手工搬動紙片，和一般平滑滑入的感覺不同。

如果自己建立專案，本次使用的工具版本是 HyperFrames 0.9.16。完成檔案後，在專案資料夾的終端機執行檢查，再輸出影片。

```bash
npx --yes hyperframes@0.9.16 check --json
npx --yes hyperframes@0.9.16 preview --background
npx --yes hyperframes@0.9.16 render \
  --quality delivery --fps 24 --output collage-broll-5s.mp4
```

終端機是電腦執行文字指令的地方，以上指令要在專案資料夾執行。若還沒裝過工具，可先請 Codex 檢查 Node.js 22 以上版本和 FFmpeg，分別負責執行工具與處理影音檔案。

延伸閱讀： [HyperFrames x Codex 快速上手：安裝、工作流、案例分享](https://www.aiposthub.com/hyperframes-codex-quickstart-guide/)

### 輸出後，要檢查哪幾個畫面？

檢查時要看完整組裝過程，因為尾幀漂亮並不代表前面動作正確。我們另外解碼輸出的 MP4，檢查首幀、逐秒畫面，以及最後一格與原構圖的對照。

![五秒拼貼動畫在第0、1、2、3、4秒的實際解碼畫面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/decoded-contact-sheet.jpg)

由輸出 MP4 擷取第 0、1、2、3、4 秒畫面，檢查紙片是否依序入場。 AI 郵報本次實作與抽幀。

首幀要確認沒有紙片提早露出，中段要能看到個別物件進場，尾幀則檢查人物、剪刀、箭頭和影片卡的位置。本次成片也以檔案資訊確認為五秒、120 格、零音軌。

若影片模型產出聲音，可以再用下面指令移除。 `-an` 代表不保留音訊，因此剪進口播時不會混入額外聲音。

```bash
ffmpeg -i input.mp4 -map 0:v:0 -c:v copy -an output-noaudio.mp4
```

## 想沿用原版 Skill，Codex 要怎麼安裝？

原版 [gbro-collage-broll](https://github.com/pyang5166/gbro-collage-broll?ref=aiposthub.com) 把隱喻規劃、靜幀、影片生成與檢查串成固定流程。Skill 可以理解為交給 Codex 反覆執行的工作說明，裡面也附上呼叫影片模型的程式。

它採 MIT 開源授權。若要安裝原版，可把整個專案放進 Codex 使用的 skills 資料夾，因為只複製主說明檔會漏掉附帶腳本。

```bash
git clone https://github.com/pyang5166/gbro-collage-broll.git \
  ~/.agents/skills/gbro-collage-broll
bash ~/.agents/skills/gbro-collage-broll/scripts/check_setup.sh
```

原專案需要 Codex 內建生圖工具、Gemini API 金鑰、Python 3.10 以上、 `google-genai` 2.10.0 以上，以及 FFmpeg／ffprobe。最後一個工具負責讀取影片規格，用來確認長度和音軌。

金鑰請在 [Google AI Studio](https://aistudio.google.com/apikey?ref=aiposthub.com) 建立，依自己的電腦設定成 `GEMINI_API_KEY` 。它會連到對應帳號的計費用量，所以不要貼進對話、文章或公開程式庫。

啟用後，可以給 Codex 一句口播，讓它先提出視覺方案。

```
collage b-roll：
一句想法，組裝成一支短片。
請依原 Skill 先提出隱喻，等我確認後才生成靜幀。
```

原版流程先讓你確認隱喻，再確認靜幀，通過後才生成影片。因此，你可以在還沒花影片用量之前，決定要換物件、改顏色，或放棄某個方向。

影片階段使用空場首幀與完成構圖尾幀，交給 Gemini Omni Flash 安排組裝動作。這條路適合想嘗試生成式動態的創作者。若要求每張紙片在確切時間停到指定位置，分層時間線會更方便修改。

## Images 2.6 的 GIF 功能適合用在哪裡？

如果目的是角色眨眼、燈泡點亮或一小段循環反應，可以先試 Images 2.6 的 GIF 輸出。因為這類任務只有一個短動作，未必需要另外建立多層影片專案。

OpenAI 的 [圖像生成文件](https://developers.openai.com/api/docs/guides/image-generation?ref=aiposthub.com#generate-animated-gif-output) 說明，產圖請求可設定 `output_format` 為 `gif` ，並用格數與每格時間控制動畫。這項輸出適用於產圖端點，圖片編輯端點不支援 GIF。

以下是依文件整理的請求範例， **本次沒有呼叫這個 API** 。如果要測試，需使用自己的 OpenAI API 金鑰與可用額度，並讓程式把回傳的 Base64 資料解碼存成 GIF。Base64 是把檔案內容轉成文字的編碼方式。

請把這份 JSON 送到 `POST /v1/images/generations` 。若不熟悉串接，可請 Codex 依官方文件建立呼叫程式，因為單獨儲存 JSON 檔不會啟動生成。

```json
{
  "model": "gpt-image-2.6-flare",
  "prompt": "A yellow paper lightbulb gently lights up and returns to its starting state. Flat teal paper background, crisp cream cut edges, fixed camera, seamless loop, no text.",
  "output_format": "gif",
  "frame_count": 9,
  "frame_duration_ms": 200,
  "size": "auto",
  "n": 1
}
```

九格、每格 200 毫秒，合起來就是 1.8 秒。若要接進影片剪輯，可以另外轉成 MP4，但單純增加播放時間只會延長或重複動作，不會自動補出新的組裝細節。

![OpenAI 官方示範用的手繪貓頭鷹草圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/owl-sketch.webp)

官方示範的起點是一張手繪貓頭鷹草圖。 來源： OpenAI 。

![OpenAI Images 2.6 貓頭鷹動畫示範封面](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/owl-animation.webp)

同一官方示範的動畫封面。這張圖片用於對照角色設計，動態請見官方發布頁。 來源： OpenAI 。

因此，可以依交付物選工具。短循環貼圖先試 GIF，想讓模型安排紙片動作可用原 Skill 的影片路線，需要逐層修改位置和節奏則用時間線。三種方法都能從同一句視覺構想開始。

## Images 2.6 拼貼動畫常見問題

### 完全不會寫程式，也能照著做嗎？

可以先用文中的自然語言指令請 Codex 規劃和建立專案，但第一次仍要完成工具環境設定。因為電腦可能缺少影音工具，先讓它自檢，會比直接貼一大段動畫要求更容易找出問題。

### 做拼貼動畫一定要 Gemini Omni 嗎？

使用原版 Skill 的預設影片流程時需要。本篇已完成的示範採用分層動畫，因此沒有呼叫 Gemini。你可以依想要的動作控制程度選擇路線。

### 這支示範能證明 Images 2.6 的畫質嗎？

不能，因為本次 Codex 內建生圖工具未回報可核對的精確模型識別。成片能展示的是透明素材與分層組裝的實際結果，模型能力則以官方文件與官方示範為依據。

### 這套 Skill 能直接搬去 Claude Code 嗎？

原作者的靜幀生成依賴 Codex 內建 `image_gen` ，因此整條流程不能只換資料夾就視為相容。若要換代理工具，必須先準備等效的生圖方式，再檢查後續腳本是否能執行。

### MIT 開源代表生成影片也免費嗎？

MIT 是原專案的程式授權，模型服務仍依各自的方案與用量計費。因為生圖、影片生成和本機輸出分屬不同步驟，所以應分別看使用條件，不能只看 Skill 是否開源。

### 為什麼成片刻意不加字幕和聲音？

這支片的用途是墊在口播下面，所以保留無聲、無字版本，讓原影片的旁白、字幕和配樂繼續主導。若需要獨立發布，則可以在剪輯時補上適合的說明。

## 先完成一支五秒片，再擴成自己的素材庫

第一次練習就用一個意思、少量紙片和固定鏡位。因為你能看清每件物品如何服務口播，下一次換主題時，才知道該改隱喻、素材，還是進場順序。

建議先沿用「一句想法，組裝成一支短片」做出自己的版本，再換成工作中真的會用的一句話。等這條流程跑通，保留提示詞、透明圖層與時間線，就能逐步累積可重用的口播畫面。

延伸閱讀： [HyperFrames Studio 是什麼？用 Codex、Claude Code 做影片，圈選畫面就能改](https://www.aiposthub.com/hyperframes-studio-app-codex-claude-code-video-editor/)

想繼續練習實際能做出成品的 AI 工作流程，歡迎免費訂閱 AI 郵報。