---
title: "Claude 在 Blender 建日本車站花六天？19 項說法核對與 Blender MCP 警告"
source: "https://www.aiposthub.com/claude-blender-mcp-japan-station-six-days-fact-check/"
author:
  - "[[Philo]]"
published: 2026-11-13
created: 2026-11-19
description: "荒船泰廣的 Claude × Blender 日本車站影片 31.6 萬次觀看。整理六天與三處修正、IG 貼文 19 項說法核對，以及官方版與社群版 Blender MCP 的警告與安裝步驟。"
tags:
  - "clippings"
status: "processed"
---
11 月 6 日，日本影像導演荒船泰廣在 X 貼出一支 35.7 秒的日本車站動畫，說是用 Claude Opus 5.5 接 Blender 做的；到 11 月 10 日晚間，這則貼文有 31.6 萬次觀看。之後有 IG 貼文把它整理成「Claude 在 Blender 裡自己蓋的車站」，並談到怎麼接 Blender MCP。我們回到一手來源，重讀作者的三則 X 貼文、他參考的 note 文章、GitHub README 和 Blender 官網，再逐項核對。先說結論（資料時間：2026 年 11 月 10 日晚間（台北））：

- **作者自己說花了六天。** 他當天傍晚回覆：作法是參考另一位日本創作者葦沢かもめ的 note 文章，之後針對做出來的東西「大量下達具體的修改指示」，花了 6 天。
- **他自己修正了三件事。** 材質「全部程序化」說得太滿；髒汙用的通用模板，似乎有一部分是 ChatGPT 做的；只有近景的樹用了現成素材。Tripo 幾乎沒用到。
- **Blender MCP 有兩個。** 一個是 Blender 官方（Blender Lab）的，一個是社群做的 MCP for Blender（原名 blender-mcp）。荒船沒寫他接的是哪一個；他參考的文章用的是官方版。
- **兩個版本都會讓 AI 在你的 Blender 裡執行程式，請先存檔。** 社群版 README 寫「使用前務必先存檔」；官方頁面寫得更重：沒有任何防護，建議用虛擬機，或沒有重要資料的電腦。
- **我們沒有安裝或測試。** 下面的步驟照 README 與官方頁面原文整理，不保證你做得出同樣的成果。

先看這個：它能在 Blender 裡跑 Python，先存檔

社群版 README 原文：「The execute\_blender\_code tool allows running arbitrary Python code in Blender… ALWAYS save your work before using it.」（AI郵報自譯：這個工具能在 Blender 裡執行任意 Python 程式碼……使用前，務必先存檔。）

Blender 官方頁面原文：「will execute LLM generated code in Blender without any guards in place」（AI郵報自譯：會在 Blender 裡執行 LLM 產生的程式碼，沒有任何防護）；建議用虛擬機，或沒有敏感資料的系統。

這篇只寫作者自己在 X 上寫的事實與各頁面的原文，不評論作者與作品；每個說法標明出處，作者沒說的就寫「沒說」。

## 荒船泰廣在 X 上寫了什麼？

| 貼文 | 發文時間（台北） | 內容（AI郵報中譯） | 觀看數（11/10 晚間） |
| --- | --- | --- | --- |
| [主貼文](https://x.com/arafune/status/2118582485412131107?ref=aiposthub.com) | 2026/11/6 13:53 | 「Blender × Claude Opus 5.5 測試」；也允許用 Tripo，但只用了約 5%，幾乎是它自己建模；文字等一部分貼圖用 ChatGPT，著色器也幾乎用程序化方式完成；聲音是 HunyuanVideo-Foley；連製作過程（making-of）也請它做了。附 0:33 影片。 | 316.4K（5K 讚、482 轉發） |
| [回覆一](https://x.com/arafune/status/2118648503312502813?ref=aiposthub.com) | 2026/11/6 18:15 | 說明作法：基本上是參考葦沢かもめ的 note 文章（附連結）；之後針對做出來的東西，大量下達具體的修改指示。「花了 6 天」。 | 22K |
| [回覆二](https://x.com/arafune/status/2118650606592348241?ref=aiposthub.com) | 2026/11/6 18:24 | 修正：說質感「全部程序化」有點說過頭，髒汙等通用模板似乎一部分是 ChatGPT 做的；只有近景的樹用了現成素材；Tripo 真的幾乎沒用，他想是這次的畫面完全沒有小物件。 | 8,719 |

三則的日文原文與 X 內建英譯，我們都對過。作者在回覆二用了「似乎」（ようです）和「我想」（と思う）的語氣，代表髒汙模板與 Tripo 的部分是他的推測，不是逐項查證的結果，這裡照原樣保留。

影片本身（我們下載後逐格看過）：前約 10 秒是完成畫面，約 10.5 到 28.5 秒是製作過程（灰色模型、俯瞰全景、逐步上色與打光），最後約 7 秒回到完成畫面；沒有人物，畫面上有日文站牌。作者沒寫是哪一站，我們不猜。這支影片沒有寫授權條款，所以本文不放影片，只放原貼文連結和一張標名的封面畫面；要看請到原貼文。

## 「花了六天」和三處修正：作者後來補了什麼？

主貼文只列了這支影片怎麼做的幾件事，沒有寫作法。當天傍晚的兩則回覆補上了作法和修正。把前後說法放在一起：

| 項目 | 主貼文（13:53） | 回覆（18:15、18:24） |
| --- | --- | --- |
| 作法 | 沒寫 | 參考葦沢かもめ的 note 文章；之後一直下具體的修改指示 |
| 時間 | 沒寫 | 花了 6 天（沒說是連續六天或累計工時） |
| 材質 | 著色器也幾乎用程序化方式完成 | 「全部程序化」有點說過頭；髒汙等通用模板似乎一部分是 ChatGPT 做的 |
| 樹與素材 | 幾乎是它自己建模 | 只有近景的樹用了現成素材 |
| Tripo | 允許用，但只用了約 5% | 真的幾乎沒用；他想是畫面沒有小物件所以沒地方用 |

### 作者沒說的事

- **用的是哪一個 Blender MCP** ：三則貼文與他的回覆列都沒寫。他只說參考葦沢かもめ的做法，而那篇文章用的是 Blender 官方的 MCP。
- **他的車站有沒有用同樣的規則與分工** （見下一節）：沒寫。
- **六天怎麼算** ：是連續六天，還是累計工時，沒寫。
- **下了多少提示、花了多少費用或 token** ：沒寫。
- **提示詞與操作紀錄** ：三則貼文都沒有附。

## IG 貼文的說法，對得上一手來源嗎？

[一則 IG 貼文](https://www.instagram.com/p/Dfj4e9bgZOG/?ref=aiposthub.com) （@your.engineer.ben，11/10 20:15 台北發）把這支影片整理成教學導流，說了 19 件事。我們把每一項對回一手來源：準確 9、要加註 6、不準 3、查不到 1。這不是評論那個帳號，只是標出哪些地方和來源不同。

| # | 貼文的說法 | 結果 | 一手來源怎麼寫 |
| --- | --- | --- | --- |
| 1 | 「你以為這是實拍的？這個黃昏的日本車站，是 Claude 在 Blender 裡自己蓋的」 | 要加註 | 荒船寫的是「幾乎是它自己建模」；但他在回覆說：參考別人的作法、一直下具體修改指示、花了 6 天，近景的樹用了現成素材。 |
| 2 | 「日本 3D 藝術家荒船泰廣」 | 要加註 | 荒船的 X 簡介自述 I work as a video director（影像導演）；「3D 藝術家」不是他自己寫的。 |
| 3 | 「把 Claude Opus 5.5 接上 Blender」 | 準確 | 主貼文標題「Blender × Claude Opus 5.5テスト」。用什麼 MCP 接，他沒寫。 |
| 4 | 「X 上 30 萬人看過」 | 準確 | 316.4K 次觀看（11/10 22:00）。是觀看次數，不是人數。 |
| 5 | 「影片後半段是製作過程」 | 不準 | 逐格看原片：0–10 秒完成畫面；約 10.5–28.5 秒製作過程；29–35.6 秒回到完成畫面。製作過程在中段。 |
| 6 | 「從灰色模型開始，建築、樹、鐵軌一路長出來，最後才打光」 | 準確 | 逐格確認：灰色月台與俯瞰全景、綠色的樹，後段有光線的全景。 |
| 7 | 「①有開 Tripo，但只用大約 5%，幾乎都是 Claude 自己建模」 | 要加註 | 主貼文原句準確；回覆二又說 Tripo 幾乎沒用、近景的樹用了現成素材。 |
| 8 | 「②看板上的文字貼圖給 ChatGPT 生」 | 要加註 | 主貼文寫「文字等一部分貼圖」用 ChatGPT，沒寫「看板」。 |
| 9 | 「③材質幾乎全用程式算出來」 | 不準 | 作者在回覆二自己修正：說質感全部程序化有點說過頭。 |
| 10 | 「④音效是 HunyuanVideo-Foley」 | 準確 | 主貼文「音はHunyuanVideo-Foley」；這是騰訊混元的開源音效模型。 |
| 11 | 「連製作過程影片都叫它做」 | 要加註 | 日文原句沒有主詞，沒寫是哪個工具做的製作過程影片；放在 ④ 音效底下，容易被讀成 HunyuanVideo-Foley 做的。 |
| 12 | 「作者沒說怎麼接的」 | 不準 | 作者在 11/6 18:15 回覆說了作法與六天；確實沒寫用哪個 MCP。 |
| 13 | 「最多人用的是開源的 Blender MCP」 | 查不到 | 星數不等於使用量，我們沒有找到各 Blender MCP 的使用人數；另外 Blender 官方也有自己的 MCP。 |
| 14 | 「MIT」 | 準確 | GitHub API 與 README：授權 MIT。 |
| 15 | 「3.37 萬星，11/9 查」 | 準確 | 11/10 22:05 是 33,861 星，與 11/9 的 3.37 萬相符；倉庫已改名 mcp-for-blender。 |
| 16 | 「README 的 Visual verification：多角度截圖、渲染，自己檢查成品，做錯自己改」 | 要加註 | README 寫「so the AI checks its own work」（讓 AI 檢查自己的作品）；「做錯自己改」README 沒有這句。 |
| 17 | 「README 警告：能直接在 Blender 跑 Python，先存檔再叫它動手」 | 準確 | README Limitations 原文：ALWAYS save your work before using it。 |
| 18 | 「一行指令裝好」 | 準確 | README Quickstart 確實有一行指令（見下面「安裝步驟」）；它是下載腳本再執行的做法。 |
| 19 | 「留言『建模』」 | 準確 | IG 頁面可見的留言，我們讀到的 7 則都是「建模」。 |

**怎麼讀這張表：** 主幹（車站是 Claude 在 Blender 裡建的、31.6 萬次觀看、Tripo、HunyuanVideo-Foley、MIT、先存檔）都對。三處不準，原因大多一樣：作者當天傍晚補了兩則回覆，那則 IG 貼文用的是補充前的資訊。

## 他參考的那篇 note 文章，怎麼做的？

荒船在回覆一貼的連結，是葦沢かもめ 2026 年 10 月 14 日發在 note 的免費文章： [〈Claude Opus 5.5 と Blender MCP でフォトリアル調の喫茶店をモデリングする方法〉](https://note.com/ashizawakamome/n/na1925903260b?ref=aiposthub.com) 。這是 **另一個案子** ：下雨黃昏小巷裡的虛構復古咖啡店「月灯り」，做外觀與一樓店內。作者自述用的是 Claude Code（Opus 5.5）和「Blender 官方的 Blender MCP」。荒船只說「參考這個做法」，沒說他的車站用了同樣的規則與工序，所以下面是那篇文章的做法，不是荒船車站的數字。

1. **先給範本。** 三張參考圖（雨的傍晚外觀、從入口看店內、從店內深處看）加一份店的設定文；圖和文都是用 ChatGPT 做的。
2. **再放規則與分工。** 一份 AI 開工前會讀的指示書（CLAUDE.md）、分工的子代理（設計書係、建模係、目視檢查係等）、作業手順書與工具。
3. **放手讓 AI 做。** AI 先寫了 4 本設計書，再分 13 個工序做；每個工序都拿渲染圖和參考圖比對，檢查係看過、有問題退回建模係。完成時約 110 萬個面，渲染一張約 7 到 11 秒。
4. **中途看結果，說出理由。** 例如作者要求不用外部的 3D 模型與素材圖（連 CC0 也不用），並寫進規則；AI 就把植物與古道具改成用程式做形狀。
5. **人只在兩處動手。** 作者寫：自己動手的只有準備範本，和中途看結果、說哪裡要改。

### AI 說沒問題，其實有 45 處

文章裡有一個很具體的例子：窗邊桌上的杯子陷進窗台的突出部分（「めり込んでいました」），AI 卻回報沒有問題（「問題なしと報告していました」）。作者請它查原因；原因是小物件的位置只用數字決定，沒有檢查互相重疊。AI 於是補了一個檢查，掃過桌子、櫃台和架子上的 326 個小物件，找出 45 處卡進別的東西（包含這個杯子），都修好了。

> 作者的結論（日文原句：AIの見た目のチェックは信用できません）：AI 的外觀檢查不能信；哪些要修，最後由人決定。

這跟社群版 README 的「Visual verification」有關：README 說多角度截圖與渲染，是為了「讓 AI 檢查自己的作品」；但 note 文章的例子提醒，AI 自己檢查可能漏掉問題，要不要修還是人決定。

## Blender MCP 其實有兩個：官方版和社群版差在哪？

IG 貼文講的是社群版；荒船參考的文章用的是官方版。兩個專案名字很像，下表是我們讀兩邊頁面整理的差異（都以 2026/11/10 晚間讀到的內容為準）：

| 項目 | Blender 官方（Blender Lab） | 社群版 MCP for Blender |
| --- | --- | --- |
| 來源 | [blender.org/lab/mcp-server](https://www.blender.org/lab/mcp-server/?ref=aiposthub.com) （Blender 官網 Blender Lab 的 MCP Server 頁） | [GitHub ahujasid/mcp-for-blender](https://github.com/ahujasid/mcp-for-blender?ref=aiposthub.com) ；倉庫說明寫「Not affiliated with the official Blender Foundation」（與官方的 Blender 基金會無關） |
| 名稱 | MCP Server | 原名 blender-mcp；PyPI 套件現為 mcp-for-blender（ [改名說明](https://github.com/ahujasid/mcp-for-blender/issues/366?ref=aiposthub.com) ） |
| 授權與星數 | 頁面沒有寫授權（頁面連到原始碼，我們沒有再查） | MIT；33,861 顆星（11/10 22:05）；最後一次 push 在 11/4 |
| 需求 | Blender 5.1 以上；外掛、LLM 客戶端、MCP 伺服器三樣都要自己下載安裝（頁面原文：Blender 沒有內建連接 LLM 的功能） | Blender 3.0 以上、Python 3.10 以上、uv 套件管理工具 |
| 警告 | Security Warning：會在 Blender 裡執行 LLM 產生的程式碼，沒有任何防護避免資料被刪除或傳到遠端；建議用虛擬機，或沒有敏感資料的系統 | execute\_blender\_code 能執行任意 Python，可能很危險；使用前務必先存檔 |
| 頁面寫的功能 | 用自然語言操作 Blender 的 Python API、查文件、了解複雜的檔案；頁面範例是場景分析、資料塊改名等（頁面沒有寫整個場景建模的範例） | 物件與材質操作、執行 Python、視覺驗證（多角度截圖與渲染）、Poly Haven／Sketchfab 素材，以及透過 Meshy、Tripo、Hyper3D Rodin、Hunyuan3D 生成 3D 模型 |

![GitHub 倉庫 ahujasid/mcp-for-blender 首頁：倉庫名稱、33.9k 星與「Not affiliated with the official Blender Foundation」說明](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/blendermcp_gh_header.jpg)

圖：GitHub 倉庫 ahujasid/mcp-for-blender 首頁（2026/11/10 擷取）；內容屬倉庫作者 Siddharth，授權 MIT，AI郵報擷取並裁切

**「最多人用」查不到。** 33,861 顆星代表關注度，不是使用量；我們沒有找到官方版與社群版的使用人數，所以不比較誰比較多人用。

### 社群版 README 的「視覺驗證」是什麼？

README 的功能表有一列 Visual verification：多角度、相機、線框、X 光與動畫條視圖，加上渲染，「so the AI checks its own work」（讓 AI 檢查自己的作品）。對應的工具叫 look，可以從視窗、相機、自動取景的多視角圖或動畫條檢視，以實體、材質、渲染、線框或 X 光方式顯示。

![社群版 README 的功能表，Visual verification 那一列說明多角度、相機、線框、X 光與動畫條視圖，加上渲染，讓 AI 檢查自己的作品](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/blendermcp_readme_features.jpg)

圖：GitHub README「Features」表（2026/11/10 擷取，英文原文；中文為本站翻譯：視覺驗證＝多角度、相機、線框、X 光與動畫條視圖，加上渲染，讓 AI 檢查自己的作品）；內容屬倉庫作者，MIT，AI郵報擷取並裁切

## 為什麼一定要先存檔？兩邊的警告原文

兩個版本都讓 AI 在你的 Blender 裡直接執行它寫的程式，所以出錯的後果可能是檔案被改壞或刪掉。兩邊的警告強度不一樣：

![社群版 README 的警告：execute\_blender\_code 能在 Blender 裡執行任意 Python，使用前務必先存檔](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/blendermcp_readme_warning.jpg)

圖：GitHub README「Limitations」警告（2026/11/10 擷取，英文原文；中文為本站翻譯：execute\_blender\_code 工具能在 Blender 裡執行任意 Python 程式碼，可能很危險；在正式環境使用要小心；使用前，務必先存檔）；內容屬倉庫作者，MIT，AI郵報擷取

![Blender 官方 MCP Server 頁面的 Security Warning：會執行 LLM 產生的程式碼、沒有任何防護，建議用虛擬機或沒有敏感資料的系統](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/blendermcp_blender_warning.jpg)

圖：blender.org Blender Lab「MCP Server」頁（2026/11/10 擷取，英文原文；中文為本站翻譯：MCP 伺服器會在 Blender 裡執行 LLM 產生的程式碼，沒有任何防護避免你的資料被刪除或傳到遠端；為了安全，建議使用虛擬機，或沒有敏感資料的系統）；內容屬 Blender 官網，AI郵報擷取並裁切

### 兩份說明還提到的幾件事

- **存檔只能對付檔案被改壞。** 官方頁面提到的風險還包括資料被傳到遠端，所以另外建議用虛擬機，或沒有敏感資料的電腦。（官方頁面）
- **社群版有安全模式，但預設不開。** README：預設 AI 可以執行任何 Python；設定 BLENDER\_MCP\_SAFE\_MODE=1 會在執行前檢查每段程式，擋下直接讀寫檔案、執行其他程式、連網路、安裝常駐程式這類風險程式碼；一般建模、材質、渲染、存檔、匯入匯出仍可用；被擋的程式會連同原因退回給 AI 重寫。
- **社群版的 socket 伺服器沒有驗證也沒有加密。** README：能連到那個連接埠的人，都能在 Blender 裡執行 Python，所以請維持在 localhost，除非你在受信任的網路上。
- **社群版預設會送一份最小的匿名使用紀錄。** README：包含隨機產生的安裝代號、工作階段代號、工具名稱、是否成功、花多久、版本與作業系統等；你的提示、產生的程式、視窗截圖、場景資料，要自己選擇加入才會收；想完全關閉，README 寫設定環境變數 DISABLE\_TELEMETRY=true。

## 安裝步驟（照 README 與官方頁面原文）

IG 貼文原本承諾留言送「安裝步驟」。下面照兩邊的原文整理：指令一字不改，只翻說明；我們沒有執行，也沒有加自己的指令。 **動手前請先存檔，並先看過上一節的警告。**

### 社群版 MCP for Blender（來源：GitHub README）

需求：Blender 3.0 以上、Python 3.10 以上、uv 套件管理工具。

**README 的一行指令（Quickstart）** ：它會在需要時安裝 uv、找出你電腦上的 MCP 客戶端（Claude Desktop、Claude Code、Codex、Cursor、VS Code 等）讓你挑要設定哪一個，再安裝並啟用 Blender 外掛；既有設定會保留，只新增一個 blender 項目，並替每個被改的檔案存一份.bak。macOS／Linux：

```
curl -LsSf https://www.mcp-for-blender.com/install.sh | sh
```

Windows（PowerShell）：

```
irm https://www.mcp-for-blender.com/install.ps1 | iex
```

這兩個都是「下載腳本再執行」的做法。README 也寫了不想跑腳本時的替代：Windows 先用 winget 裝 uv（`winget install --id=astral-sh.uv -e`），再執行 `uvx mcp-for-blender setup`；已經有 uv 的人直接執行 `uvx mcp-for-blender setup`。做完後要完全關閉再重開你的 AI 應用程式（Windows 要從系統匣結束），再開 Blender。

**手動安裝（README 的 Manual setup，四步）** ：

1. 安裝 uv：macOS 用 `brew install uv`；Linux 用 `curl -LsSf https://astral.sh/uv/install.sh | sh`；Windows 用 `winget install --id=astral-sh.uv -e`。README 提醒：要用官方安裝程式，不要用 `pip install uv`。
2. 把 MCP 伺服器加進你的客戶端。Claude Code：`claude mcp add blender uvx mcp-for-blender`；Claude Desktop：Settings → Developer → Edit Config，加入下面這段設定。
3. 安裝 Blender 外掛：`uvx mcp-for-blender install-addon`，然後在 Blender 的 Edit → Preferences → Add-ons 勾選「Interface: MCP for Blender」。
4. 連線：外掛會在 Blender 開啟時啟動伺服器；在 3D 視窗按 N 鍵，打開「MCP for Blender」分頁，沒在跑就按 Start MCP Server。
```
{
    "mcpServers": {
        "blender": {
            "command": "uvx",
            "args": ["mcp-for-blender"]
        }
    }
}
```

README 另外提醒：同時只能跑 **一個** MCP 伺服器實例（Cursor 和 Claude Desktop 擇一）。

![README 附的 Blender 畫面：按 N 鍵開啟側欄，Blender MCP 分頁裡有 Port 與 Start MCP Server 按鈕](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/blendermcp_addon_ui.jpg)

圖：GitHub README 附圖（Blender 3D 視窗側欄的 MCP 分頁與 Start MCP Server 按鈕；圖是舊版介面，分頁名稱還是「Blender MCP」，現在的外掛名稱是 MCP for Blender）；圖屬倉庫作者，MIT，AI郵報縮小使用

### Blender 官方版（來源：blender.org Blender Lab 頁面）

- 需求：Blender 5.1 以上。頁面寫，Blender 沒有內建連接 LLM 的功能，要把三樣東西手動下載、安裝並執行：外掛、LLM 客戶端、MCP 伺服器。
- **外掛** ：把外掛拖放進 Blender；頁面說要拖放兩次，第一次加入 Blender Lab 儲存庫，第二次才安裝外掛本身（這樣之後有新版會收到通知）；也可以下載後從磁碟安裝。
- **LLM 客戶端** ：MCP 伺服器是通用標準，相容很多客戶端；頁面的示範用的是 llama.cpp，請依你選的客戶端文件安裝。
- **MCP 伺服器** ：支援.mcpb 套件的新客戶端，可以從 release 頁下載最新套件（頁面說 llama.cpp 還不支援）；也可以依原始碼的說明從原始碼安裝。
- 頁面還給了可以貼上的範例提示（資料塊改名、查詢哪個材質被哪些物件用、場景除錯、幾何節點加註解）；頁面註明成功與否取決於所用的模型。

## 常見問題

### 荒船用的是官方版還是社群版？

沒說。他只說參考葦沢かもめ的作法，那篇文章用的是 Blender 官方的 MCP；他自己的車站有沒有用同樣設定，沒寫。

### 這支影片能用一句話做出來嗎？

作者自己說的是：參考別人的作法、針對做出來的東西一直下具體的修改指示、花了 6 天；他參考的文章也是準備範本、放規則與分工、中途看結果再說哪裡要改。三則貼文都沒附提示詞或操作紀錄，我們沒有證據顯示「一句話」就能做出來。

### Tripo 是什麼？

一個用文字或參考圖生成有貼圖的 3D 模型的服務。社群版 README 有列它：可以透過 Premium（不用自備金鑰）或自己的 Tripo API 金鑰使用。荒船說他允許 Claude 用 Tripo，但實際幾乎沒用。

### HunyuanVideo-Foley 是什麼？

騰訊混元開源的音效生成模型（ [GitHub Tencent-Hunyuan/HunyuanVideo-Foley](https://github.com/Tencent-Hunyuan/HunyuanVideo-Foley?ref=aiposthub.com) ，2025 年 8 月建立，說明寫「Multimodal Diffusion with Representation Alignment for High-Fidelity Foley Audio Generation」）。荒船說影片的聲音用的是它。

### 社群版要付費嗎？

專案本身是 MIT 開源。README 另有「Premium」：不用自備金鑰、直接在 Blender 裡用 Meshy、Hunyuan3D、Tripo、Hyper3D Rodin 生成模型，細節在 [官網 Premium 頁](https://www.mcp-for-blender.com/premium?ref=aiposthub.com) ；我們沒有查價格。官方版頁面沒有提到付費項目。

### 先存檔就夠安全了嗎？

不夠。存檔只能保住你的檔案。官方頁面提到的風險還有資料被傳到遠端，所以另外建議用虛擬機，或沒有敏感資料的電腦；社群版則有預設不開的安全模式。

### 我可以轉載荒船的影片嗎？

影片沒有寫授權條款，是作者自己的作品，請不要自行轉載；要看請到 [原貼文](https://x.com/arafune/status/2118582485412131107?ref=aiposthub.com) 。本文只引用貼文標題與連結，以及一張標名的封面畫面。

## 資料來源

以下都在 2026 年 11 月 10 日晚間（台北） 讀取；觀看數與星數會隨時間變動，只代表關注度。

1. 荒船泰廣 X 主貼文，2026/11/6 13:53 台北：https://x.com/arafune/status/2118582485412131107（日文原文與 X 內建英譯；316.4K 觀看）
2. 荒船泰廣 X 回覆一，2026/11/6 18:15：https://x.com/arafune/status/2118648503312502813（22K 觀看）
3. 荒船泰廣 X 回覆二，2026/11/6 18:24：https://x.com/arafune/status/2118650606592348241（8,719 觀看）
4. 葦沢かもめ，note〈Claude Opus 5.5とBlender MCPでフォトリアル調の喫茶店をモデリングする方法〉，2026/10/14：https://note.com/ashizawakamome/n/na1925903260b
5. IG @your.engineer.ben 貼文，2026/11/10 20:15 台北：https://www.instagram.com/p/Dfj4e9bgZOG/
6. GitHub ahujasid/mcp-for-blender（原 blender-mcp）倉庫、README 與 GitHub API：https://github.com/ahujasid/mcp-for-blender（33,861 星、MIT）
7. Blender Lab「MCP Server」頁：https://www.blender.org/lab/mcp-server/
8. GitHub Tencent-Hunyuan/HunyuanVideo-Foley：https://github.com/Tencent-Hunyuan/HunyuanVideo-Foley

**製作說明：** 本文由 AI郵報整理；英文與日文原句都是短片段並標「AI郵報自譯／中譯」，圖片為 GitHub 與 Blender 官網頁面的擷取（標明出處與版權屬誰）；我們沒有安裝或測試任何 Blender MCP，也沒有聯絡作者。若原作者或專案方認為使用不妥，請聯絡 AI郵報，我們會先撤下。