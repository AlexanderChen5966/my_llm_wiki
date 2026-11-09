---
title: "Video 2 GLSL 是什麼？Opus 5.5 把影片改寫成 156KB shader，流程與限制"
source: "https://www.aiposthub.com/video-2-glsl-claude-opus-5-5-shader-skills-guide/"
author:
  - "[[Philo]]"
published: 2026-10-20
created: 2026-11-04
description: "Video 2 GLSL 是 gokaygokay 在 X 展示的實驗，讓 Claude Opus 5.5 把 20 秒影片重建成單一 GLSL shader，156KB、無貼圖。本文整理開源 shader-skills 的做法、驗收流程、第三方實驗結果與版權限制。"
tags:
  - "clippings"
status: "inbox"
---
Video 2 GLSL 是作者自述的實驗：一段 19.3 MB 的影片，被重建成 156 KB 的程式碼，沒有貼圖，也沒有任何影片畫格，只靠數學在螢幕上重畫同樣的畫面。

10 月 19 日凌晨（台灣時間），開發者 gokaygokay（帳號 gokayfem）在 X 發布這個實驗。

他說，自己給 Claude Opus 5.5 一段 20 秒的影片，模型把它重建成單一的 GLSL fragment shader，而且可以無縫循環。

fragment shader 是一種在顯示卡上執行的小程式，會對畫面的每一個像素算出顏色。GLSL 是寫這種程式的語言，Shadertoy 則是讓人在網頁上貼上並預覽這類程式的網站。

不過，示範中的 156KB GLSL 程式與 20 秒原片都沒有公開。能查證的，是作者同一串貼文附上的開源倉庫 shader-skills，以及倉庫裡一份專門用來把影片轉成 shader 的提示詞。

另外要揭露，作者在 X 簡介自稱任職於 fal，擔任創意工程負責人。這不影響技術內容，但讀者可以把它視為當事人自己的示範，而不是第三方評測。

我的判斷是，這套做法適合做 GLSL 藝術創作、網頁動態背景，或想了解 AI 代理如何驗收長任務的開發者，不適合期待「丟影片就拿到可商用成品」的人。

要理解這個判斷，先看示範到底證明了什麼，又沒有證明什麼。

## Video 2 GLSL 是什麼？把影片翻譯成一段純數學程式

示範影片分成兩半。前半是左右對照：左邊標示「Video in」，檔名 village-loop.mp4，右邊標示「Shader out」，檔名 village-loop.out.glsl，並註明在 WebGL2 即時執行。

畫面是一條童話風格的石板街，主角是戴著藍綠色大帽子的小角色向前跑。

後半是數字與流程。畫面寫著 19.3 MB 的影片變成 156 KB 的 GLSL，沒有貼圖、沒有影片，只有數學。流程寫成三步：描述影片，用技能建造，逐格檢查，其中第二步標註了五個代理平行工作。

這些內容全部來自作者自己的影片，是原作者的自述。本文能確認的只有一件事：倉庫公開的，是技能與提示詞，而不是這支 GLSL shader。我查了倉庫的檔案清單，裡面沒有 village-loop 的 shader，也沒有參考影片。

所以「156 KB」這個數字，目前只能當作者自述。它可以說明程式碼體積很小，但無法證明畫面與原片有多接近，也看不出在一般筆電上跑得多順。

另一個要小心的是「從 19.3 MB 變成 156 KB」這種說法。影片檔存的是每一格畫面的像素資料，GLSL 存的是畫圖的公式，每次播放都要靠顯示卡重新運算。兩者不是壓縮前後的關係，所以不能說壓縮了一百多倍。

接下來的問題是，為什麼一段數學程式可以畫出一整條街。

![Video 2 GLSL 示範影片畫面，左側標示 Video in 的童話石板街參考影片，右側標示 Shader out 的 WebGL2 即時輸出](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/video-to-glsl-shader-skills-cover.jpg)

原作者 gokaygokay 示範影片的單格畫面（左為參考影片、右為 shader 輸出），為作者自述成果，非本站製作或實測。 圖片來源： 官方原文 。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/10/video-to-glsl-shader-skills-demo.mp4" width="1280" height="720" controls=""></video>

原作者 gokaygokay 發布的 Video 2 GLSL 示範影片，數字與成果為作者自述，非本站實測。 來源： 官方發布 。

## 不用貼圖怎麼畫出一條街？SDF 與光線步進的白話解釋

GLSL shader 的運作方式很單純：顯示卡對每個像素呼叫一次程式，程式知道這個像素的位置與目前時間，然後回傳顏色。因為時間也是輸入，所以只要公式是週期性的，畫面就能無縫循環。

要畫三維場景，常用的做法是 SDF，也就是距離場。它用公式描述空間中任一點離物體表面有多遠，例如球、方塊、圓錐都有現成公式。

再搭配光線步進，也就是從相機往每個像素的方向，一小段一小段向前走，直到碰到表面。碰到之後，就能算出法線、光照與陰影，最後上色。

這就是不需要貼圖的原因。沒有貼圖時，石板、牆面、花盆與燈籠這類細節，只能由公式與噪聲函數生成，這也解釋了作者說的程式碼為什麼能這麼小。

代價是運算量。每個像素都要重複計算整個場景，場景越複雜、解析度越高，顯示卡負擔越重。作者在貼文裡沒有提供幀率，所以實際流暢度仍是未知數。

那麼讓 AI 代理寫出這種程式，作者是怎麼處理的？關鍵在他開源的技能庫。

## shader-skills 是什麼？給 AI 代理一套驗證過的 GLSL 零件庫

依官方 README，shader-skills 是一份讓程式代理寫出「第一次就能編譯、而且看起來有設計感」的 GLSL 技能。

它搭配一個經過驗證的函式庫，共 186 個函式、20 個模組，每個都在 WebGL2 編譯過，並在顯示卡上做數值測試。

README 對問題的描述很直接：代理寫 GLSL 時看不到輸出，容易記錯公式，而且 WebGL 會拒絕桌面版 GLSL 接受的寫法。常見錯誤包括結構體的三元運算、在不同顯示卡上結果不同的正弦雜湊，以及內外符號弄反的距離函數。

所以技能提供四樣東西。第一是測試過的零件，例如每個 SDF 都檢查過距離值不會高估，這樣光線步進才不會穿過表面。第二是打包工具，代理只要用名稱呼叫函式，工具就會把用到的函式內嵌成單一檔案，可直接貼到 Shadertoy。

第三是檢查器，會抓出 WebGL1 殘留寫法、正弦雜湊、多餘的版本宣告與缺少 mainImage，並在裝有 glslangValidator 時實際編譯。

第四是固定的除錯模式，例如 0 是最終畫面、1 是距離場、2 是法線，讓問題能「看」出來，而不是猜。

驗證分成多層，README 用表格列出。以下我把最能說明可信度的幾項整理出來。

| 檢查項目 | 證明了什麼 |
| --- | --- |
| 檢查器與文件漂移 | 每個函式都有說明與標記，沒有 WebGL1 或正弦雜湊寫法 |
| glslang 編譯 | 每個模組、範例與模板都能編譯成 GLSL ES 3.00 |
| WebGL2 編譯 | 在 ANGLE 下編譯，同時套用 WebGL 專屬規則 |
| 顯示卡屬性測試 | 90 項測試、每項 16,384 筆取樣，涵蓋距離界限、導數、色彩轉換等 |
| 渲染驗證 | 11 個模板與 16 個變體，沒有 NaN 或黑畫面 |

這張表來自 README 的驗證章節，是作者的自述，我沒有重跑。另外，技能效果評測要用 Claude 的外掛評測指令，每次執行都會真的呼叫模型而計費，所以 README 標為手動執行。

安裝方式依工具而不同。

Claude Code 用外掛市集，指令是 `claude plugin marketplace add gokayfem/shader-skills` ，接著執行 `claude plugin install shader-skills@shader-skills` 。

其他工具可以用 `npx skills add gokayfem/shader-skills` ，README 說明這可涵蓋 Codex、Cursor、Gemini CLI、Copilot 與約 70 種代理。

專案採 MIT 授權，函式是已發表技術的原創實作，來源列在 CREDITS 檔案。倉庫從 6 月 20 日起有第一版，10 月 18 日重建成 v2，10 月 20 日查詢時約有 100 顆星。

延伸閱讀： [27KB 程式畫出一條龍：Keenan Crane 的 AI 生成 GLSL 示範在 X 熱傳](https://www.aiposthub.com/keenan-crane-llm-glsl-27kb-dragon-implicit-surface/)

## 影片轉 GLSL 的提示詞怎麼設計？先拆解、等核准，再分階段做

倉庫的 prompts 資料夾有一份 video-to-shader-prompt，是 10 月 18 日加入的，早於貼文發布。它不描述任何特定影片，而是一份通用模板，使用者填入影片網址、輸出名稱、必須保留的畫面、可以省略的部分與目標機器。

整份提示詞的核心是流程控制。第一步不寫程式，而是要求代理先拆解影片，寫成 REFERENCE.md，並且停下來等你核准。

拆解的內容包括片長與是否循環、每半秒取樣的鏡頭時間表、物件清單、動作紀錄、鏡頭紀錄、光線與色盤，而且顏色要用量測得到的數值，不能靠目測。

核准之後才開始建造，分成八個里程碑：拆解與工具、方塊替身與時間軸、動作、主體細節、環境細節、光線色彩與氣氛、對照原片的逐項修正、效能。每個里程碑結束都要把渲染結果和原片並排，批評、修正，至少做三輪。

品質標準也寫得具體。動作方面，每次切換都要用 10 格、間隔 1/60 秒的條帶證明沒有跳動，腳踩地時不能滑動，二次運動像頭髮與衣服要有彈簧般的延遲。

細節方面，畫面中超過 10% 面積的物件，要有輪廓、次級形體與表面細節三層。對照方面，每個物件都要拆出它的組成部分，並比較原片與渲染的平均顏色。

這份提示詞最特別的設定是，效能排在品質後面，而且規定只要有一項檢查沒通過，就不能停手。它也限制了代理的偷懶空間，沒有核准就不能簡化，只有景深、動態模糊、背景 2D 雲鳥與沒出現的臉部可以直接省略。

所以這份提示詞的重點是驗收流程：先拆解、等核准、逐項對照原片，避免代理在長任務中自說自話。作者的示範影片第一步也叫 REFERENCE.md，與提示詞的第一步相符，但他沒有明說示範是照這份提示詞做的。

## 第三方怎麼試？14 秒釣魚遊戲的實驗結果

倉庫還有一個值得參考的小插曲。10 月 19 日，一位名叫 winfred5432 的貢獻者提交修改，在提示詞第三節加入「先測試比對方法，再相信它」。

起因是他自己的實驗。他用這個技能，請 DeepSeek Harness 和 Claude Opus 5.5 各自把另一位創作者貼出的 14 秒釣魚遊戲片段，重建成單一 Shadertoy shader。

這次實驗用的是實驗者自訂的任務書與評分程式，不是倉庫裡的影片轉 shader 提示詞。

結果他發現只比對區域平均顏色會被騙，因為一張平坦灰畫面，只要塗上量測到的色塊，就能通過顏色檢查。

所以新增的規則是，先拿一張塗色的平面畫面、一張凍結畫面和一組打亂順序的畫面去評分，如果它們也能通過，這個檢查就沒有意義。實驗倉庫 creek-shader-remake 記錄了完整過程。

結果有幾個數字。DeepSeek 與 Claude Code 兩組在三輪後，都通過實驗者自訂的硬性檢查，版面一致度平均分別是 0.54 與 0.64。作為對照，不同場景約 0.25，同一鏡頭相隔 1 秒約 0.65。

耗時各約 2 小時多，Claude Code 的最終審查是「通過但有保留」，最後幾項修正沒有再獨立審查。

這個實驗也寫明，實驗者全程與代理對話、提供指引，所以是引導式的實驗，不是全自動結果。Claude 自己在第一輪的差距分析也承認，場景結構吻合，但細節建模、材質筆觸與動作大多沒有重現。

換句話說，第三方結果顯示，這套流程在人工引導下，能做出版面與原片相近、通過自訂檢查的重建。但兩個實驗的參考片不同，原作者示範的 shader 也沒有公開，所以無法比較兩者的還原度，導入前仍要自己做小規模驗收。

延伸閱讀： [Opus 5.5 做出 CD 盒三維動畫：免匯入模型，Three.js 仍要驗收幾何與互動](https://www.aiposthub.com/opus-threejs-cd-case-animation-geometry-interaction/)

## 適合誰、不適合誰：導入前檢查

適合的人有兩類。一類是想做 GLSL shader 作品或網頁動態背景的創作者，因為成品是單一檔案，不依賴貼圖，可直接貼到 Shadertoy，要做成網頁背景則得自己加一層 WebGL2 的包裝。

另一類是想學習代理驗收做法的開發者，因為那份提示詞本身就是很好的教材。

不適合的人也很清楚。需要像素級還原、需要在手機低階顯卡上穩定跑滿幀率的專案，要先實測。需要把他人影片當參考做商用內容的，還要先處理授權。

導入前建議先用一段 3 到 5 秒、自己擁有版權的短片試做。這樣可以先確認安裝是否順利、代理是否願意停下來等核准，也能先看到成本與耗時。

同時要確認三件事。你的電腦要裝 glslangValidator，沒有的話檢查器只做規則檢查，不會實際編譯，也要知道 Claude Code 或其他代理的使用額度，並能在瀏覽器實際跑 WebGL2、記錄幀率。

確認完這些前置條件，還要面對幾個不容易靠設定解決的風險。

## 限制與風險：參考影片、效能與自述數字

最大的風險是參考影片的權利。GLSL 程式沒有貼圖，並不代表沒有複製。如果參考影片是別人的作品，重建出來的畫面仍可能涉及原作的權利，本文不構成法律意見。示範中的 village-loop 來源與授權，作者沒有說明。

第二個是效能。156KB 的程式碼可能要用大量迴圈與雜訊運算，在高解析度與低階顯示卡上很吃力。提示詞雖然要求回報幀率，但示範沒有提供，所以不能假設它能在任何裝置順暢播放。

第三個是數字的出處。156KB、19.3MB、五個代理平行、無縫循環，全是作者自己的影片說法。第三方實驗的數據較完整，但也是單一案例，並且有人為引導。

另外，代理自己做驗收，仍有自說自話的可能。第三方實驗就記錄了代理漏報審查失敗結果、沒有經過同意就改掉已核准選項的情形，這些都是在長任務中要用人工審查補上的缺口。

最後是成本。示範沒有提供代幣用量或費用，五個代理平行、至少三輪修正的流程，在實際帳單上可能不便宜。

## Video 2 GLSL 常見問題

### 使用 Video 2 GLSL 的做法要付費嗎？

shader-skills 是 MIT 授權，可以免費使用。但執行流程需要 Claude Code 或其他支援技能的代理，模型用量依各服務計費。

### 一定要用 Claude Opus 5.5 嗎？

不一定。README 說明技能可用於 Claude Code、Codex、Cursor、Gemini CLI、Copilot 等多種代理，貼文示範則是用 Opus 5.5。第三方實驗也用過 DeepSeek Harness。

### 做出來的 GLSL 可以貼到 Shadertoy 嗎？

可以。技能的設計目標就是輸出單一檔案、使用 mainImage 並只依賴時間與解析度，並提供打包與檢查工具。

### 把別人的影片轉成 GLSL，有版權問題嗎？

有可能。程式碼本身雖然沒有貼圖，但重建的畫面若與原作高度相似，仍可能涉及原作權利。建議使用自己擁有或已授權的影片。

## 先用自己的 3 秒短片，驗證流程再決定投入

Video 2 GLSL 展示的是一種新的創作方式，把影片拆解成可驗收的規格，再讓代理用 GLSL 公式重建。示範的震撼度來自作者自述的數字，真正可學的，是倉庫公開的技能與驗收流程。

如果你想試，建議先用自己拍的 3 秒短片，完整走一遍拆解、核准與逐格比對，再判斷成本與品質是否值得。想持續追蹤 AI 創意程式與 shader 工具的更新，可以免費訂閱 AI 郵報。

## 資料來源

- gokaygokay 原作者 X 貼文與影片： [https://x.com/gokayfem/status/2111907592129671505](https://x.com/gokayfem/status/2111907592129671505?ref=aiposthub.com)
- shader-skills 官方 GitHub 倉庫與 README： [https://github.com/gokayfem/shader-skills](https://github.com/gokayfem/shader-skills?ref=aiposthub.com)
- shader-skills 影片轉 shader 提示詞： [https://github.com/gokayfem/shader-skills/blob/main/prompts/video-to-shader-prompt.md](https://github.com/gokayfem/shader-skills/blob/main/prompts/video-to-shader-prompt.md?ref=aiposthub.com)
- shader-skills 提示詞驗收補強的 Pull Request #3： [https://github.com/gokayfem/shader-skills/pull/3](https://github.com/gokayfem/shader-skills/pull/3?ref=aiposthub.com)
- 第三方實驗 creek-shader-remake： [https://github.com/winfred5432/creek-shader-remake](https://github.com/winfred5432/creek-shader-remake?ref=aiposthub.com)
- 第三方實驗流程紀錄： [https://github.com/winfred5432/creek-shader-remake/blob/main/docs/process.md](https://github.com/winfred5432/creek-shader-remake/blob/main/docs/process.md?ref=aiposthub.com)
- Shadertoy 官方網站： [https://www.shadertoy.com/](https://www.shadertoy.com/?ref=aiposthub.com)