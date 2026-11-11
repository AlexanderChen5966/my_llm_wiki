---
title: "Ponytail：讓你的代理少寫程式碼的 AI 編碼技能"
source: "https://codelove.tw/@tony/post/3V2ey3"
author:
published:
created: 2026-11-10
description: "你向你的 AI 助手要一個日期選擇器。它安裝 flatpickr、寫一個包裝元件、加上一份樣式表、設定主題覆寫，然後開始跟你解釋時區處理。四百行之後，你得到一個日期選擇器。坐在兩個座位外的資深工程師本來會這樣寫：```html<input type=\"date\">```[Pony..."
tags:
  - "clippings"
status: "inbox"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [11月09日](https://codelove.tw/@tony/post/3V2ey3 "2026-11-09 10:04:58")

## Ponytail：讓你的代理少寫程式碼的 AI 編碼技能

你向你的 AI 助手要一個日期選擇器。

它安裝 flatpickr、寫一個包裝元件、加上一份樣式表、設定主題覆寫，然後開始跟你解釋時區處理。四百行之後，你得到一個日期選擇器。

坐在兩個座位外的資深工程師本來會這樣寫：

```html
<input type="date">
```

[Ponytail](https://github.com/DietrichGebert/ponytail) 是一個開源的技能／外掛，試圖把那種人塞進你的程式撰寫代理裡。它可搭配 Claude Code、Codex、Cursor、Gemini CLI、Copilot、OpenCode、Windsurf、Cline，以及一長串其他工具使用。採用 MIT 授權。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1794289102&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055819&amp;bpp=7&amp;bdt=929&amp;idt=7&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1794289055%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1535x866&amp;nras=3&amp;correlator=1006475122378&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=117&amp;ady=969&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=0&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1535%2C866&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=46576" title="Advertisement" aria-label="Advertisement"></iframe>

## 一個畫面的核心概念

Ponytail 不是工具、函式庫，也不是模型。它是一組規則集，會在每一輪被注入到你的代理上下文中。它的核心是一個階梯。在寫任何程式碼之前，代理會先停在第一個符合的階梯上：

```
1. 這東西真的需要存在嗎？   -> 不需要：跳過（YAGNI）
2. 這個程式庫裡已經有了嗎？ -> 重用，不要重寫
3. 標準函式庫做得到嗎？     -> 用它
4. 原生平台功能有嗎？       -> 用它
5. 已安裝的相依套件有嗎？   -> 用它
6. 一行就夠嗎？             -> 那就一行
7. 只有到這一步：做出最小可行的實作
```

基本上就是這樣。儲存庫裡其他東西都只是打包：將近二十種代理的適配器、幾個斜線命令，以及一個基準測試框架。

這個階梯有兩點，比表面看起來更重要：

**它是在理解之後才運作，不是取代理解。** 這些規則會要求代理先讀取變更所碰到的程式碼，並在選擇階梯之前追蹤實際流程。對解法懶一點，但不要對讀程式碼庫懶。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1794289102&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055836&amp;bpp=2&amp;bdt=945&amp;idt=2&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1794289055%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1535x866%2C952x280&amp;nras=4&amp;correlator=1006475122378&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=117&amp;ady=1861&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=0&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1535%2C866&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=46566" title="Advertisement" aria-label="Advertisement"></iframe>

**驗證不在範圍內。** 規則明確保護信任邊界驗證、錯誤處理、安全性與可及性。目標不是「最少 token」，而是「只寫這個任務需要的東西」。程式碼之所以變小，是因為多餘的東西本來就不需要，不是因為硬是極限壓縮出來的。

## 安裝方式

選擇你的代理。以 Claude Code 為例，需要兩個提示（必須分開送出）：

```shell
/plugin marketplace add DietrichGebert/ponytail
```
```shell
/plugin install ponytail@ponytail
```

Codex：

```shell
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

Copilot CLI：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1794289102&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055850&amp;bpp=1&amp;bdt=960&amp;idt=1&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1794289055%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1535x866%2C952x280%2C952x280&amp;nras=5&amp;correlator=1006475122378&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=117&amp;ady=2722&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=0&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1535%2C866&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=46558" title="Advertisement" aria-label="Advertisement"></iframe>

```shell
copilot plugin marketplace add DietrichGebert/ponytail
copilot plugin install ponytail@ponytail
```

Gemini CLI：

```shell
gemini extensions install https://github.com/DietrichGebert/ponytail
```

對於 Cursor、Windsurf、Cline、Kiro、Aider 和 Copilot Chat，沒有外掛層，所以你要把儲存庫中對應的規則檔複製出來（`.cursor/rules/` 、`.windsurf/rules/` 、`.clinerules/` ，以此類推）。另外有些代理也會自動從專案根目錄載入 `AGENTS.md` ，而這個儲存庫本身就附了一份，所以只要從 checkout 下來就能直接用，不需要額外設定。

Claude Code 和 Codex 的外掛會執行兩個很小的 Node 生命週期 hook，所以 `node` 必須在你的 `PATH` 裡。如果沒有，技能還是能用；只是少了常駐啟用。

## 指令

| 指令 | 功能 |
| --- | --- |
| `/ponytail [lite\\|full\\|ultra\\|off]` | 設定強度或關閉。預設是 `full` 。 |
| `/ponytail-review` | 檢視你目前的 diff 是否過度設計，回傳刪除清單。 |
| `/ponytail-audit` | 同樣功能，但針對整個儲存庫，而不是 diff。 |
| `/ponytail-debt` | 收集你延後處理的 `ponytail:` 捷徑，整理成帳本。 |
| `/ponytail-gain` | 顯示量測到的成效儀表板。 |
| `/ponytail-help` | 快速參考。 |

就算你從不開啟常駐模式， `/ponytail-review` 也是我最先會用的指令。把它指向你已經寫好的 diff，是一種低承諾的方式，看看這套紀律在你的程式碼庫裡是否真的能找出問題。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1794289112&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055856&amp;bpp=1&amp;bdt=966&amp;idt=0&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1794289055%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1535x866%2C952x280%2C952x280%2C952x280%2C350x280%2C728x90%2C334x250&amp;nras=8&amp;correlator=1006475122378&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=117&amp;ady=4126&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=748&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;psts=AOrYGsm9P6oz-RJEJOetdZpYKfYl_clbMzU3W1a2w4c66hAIcpXnXk__7rPBxxir-tQfaBRosR4fWuoUvVTQwtoojxWOwIJyKg6wnVr7F1WjHL93BDhFTY6b5on7YPyhg8TJVQHx%2CAOrYGskK3Tq6KpVrU1Yv2i-zgpccJGGmWaQkOcwVUtlW7fm3fchzK4BT5brDNXxCTlKndu20fwOzBqE7VyEiM3Wgo_jjqRZNmX13VBVH%2CAOrYGsnPn0ArihRd8oSHe7dmgh8FCkk7NbJRi7U-r-pU-6jlkkyO6coodbzb3TUQMTmnSPvRpTm7P73iIV5ut88cbwCLDs0XLz6LkzrjHBCgGqks4Q%2CAOrYGslpg-UIIdctNh7Tl08_ZwZC_0K5OrrlCtpJcL_FQTMcRHHRoeh4cuUMsMaTRXCZaOKhsxVPLqQZLS0lh8kxOApph9bNZ9CbaxGZW_e3gHzU6A&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1932%2C30%2C1932%2C30%2C1920%2C30%2C1775%2C947%2C1535%2C866&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=5&amp;uci=a!5&amp;btvi=7&amp;fsb=1&amp;dtd=56542" title="Advertisement" aria-label="Advertisement"></iframe>

這些指令需要支援技能的主機。只有指令提示的適配器（Cursor、Windsurf、Cline、Copilot Chat、Kiro）會拿到常駐規則集，但不會有這些指令。

## 數字真正說了什麼

這裡讓這個專案比大多數 GitHub trending 條目更有意思，而且值得仔細看，因為這故事有個反轉。

最初的基準測試宣稱可減少 80–94% 的程式碼。有人開了 [issue #126](https://github.com/DietrichGebert/ponytail/issues/126) ，提出四個合理批評：

1. 單一的 prompt-completion 配對，不是代理實際的使用方式。
2. 基準組只是個很愛講話的裸模型，會用長篇文字和選項去撐答案，所以「答案行數」其實是在數註解，不是在數程式碼。
3. 「偏好一行式」可能會犧牲安全性。
4. 七個字的提示詞，也許就能達到整個技能的效果。

在更公平的基準之下：

| 相較無技能基準 | LOC | token | 成本 | 時間 | 安全 |
| --- | --- | --- | --- | --- | --- |
| ponytail | \-54% | \-22% | \-20% | \-27% | 100% |
| caveman（簡短敘述） | \-20% | +7% | +3% | +2% | 100% |
| 「YAGNI + 一行式」提示詞 | \-33% | \-14% | \-21% | \-30% | 95% |

逐項任務的數字更有用：

| 任務 | 基準 | ponytail |
| --- | --- | --- |
| 日期選擇器 | 404 | 23 |
| 顏色選擇器 | 287 | 23 |
| 檔案拖放區 | 251 | 95 |
| 多步驟精靈 | 571 | 312 |
| 依標題搜尋專案 | 44 | 44 |
| 匯出專案為 CSV | 36 | 33 |
| 計算使用者專案數量 | 21 | 17 |

形狀很清楚。當存在過度建置陷阱時，削減幅度會非常大，因為代理會去用 `<input type="date">` ，而不是自己做一個元件。當程式碼本來就很精簡時（例如後端 CRUD），各種方法最後都會收斂，ponytail 就不會有作用。-54% 這個標題數字，是跨這兩類任務的平均值，所以要把它理解成「有時候很驚人，有時候完全沒差」，而不是「到處都少一半程式碼」。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1794289122&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055870&amp;bpp=1&amp;bdt=980&amp;idt=1&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1794289055%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1535x866%2C952x280%2C952x280%2C952x280%2C350x280%2C728x90%2C334x250%2C952x280&amp;nras=9&amp;correlator=1006475122378&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=117&amp;ady=5785&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=2322&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;psts=AOrYGsm9P6oz-RJEJOetdZpYKfYl_clbMzU3W1a2w4c66hAIcpXnXk__7rPBxxir-tQfaBRosR4fWuoUvVTQwtoojxWOwIJyKg6wnVr7F1WjHL93BDhFTY6b5on7YPyhg8TJVQHx%2CAOrYGskK3Tq6KpVrU1Yv2i-zgpccJGGmWaQkOcwVUtlW7fm3fchzK4BT5brDNXxCTlKndu20fwOzBqE7VyEiM3Wgo_jjqRZNmX13VBVH%2CAOrYGsnPn0ArihRd8oSHe7dmgh8FCkk7NbJRi7U-r-pU-6jlkkyO6coodbzb3TUQMTmnSPvRpTm7P73iIV5ut88cbwCLDs0XLz6LkzrjHBCgGqks4Q%2CAOrYGslpg-UIIdctNh7Tl08_ZwZC_0K5OrrlCtpJcL_FQTMcRHHRoeh4cuUMsMaTRXCZaOKhsxVPLqQZLS0lh8kxOApph9bNZ9CbaxGZW_e3gHzU6A%2CAOrYGskvNVTTX6HsohuEu3GTj86x-Xf-mmDTRDaP8ajirhFNOM5nJeZ6ntsGKn2PaiBuetwKMvVuJQWKUriEKcM5LVWdw1_Z8SFhWhml&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1932%2C30%2C1932%2C30%2C1920%2C30%2C1775%2C947%2C1535%2C866&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=6&amp;uci=a!6&amp;btvi=8&amp;fsb=1&amp;dtd=66408" title="Advertisement" aria-label="Advertisement"></iframe>

我覺得最有說服力的是安全性那一層。六個任務會建立一個起始檔案，要求寫一個函式，並像真實工單一樣把安全需求隱含起來。接著評分器會用對抗性輸入去執行產出的函式：路徑穿越、SQL injection、偽造 token、格式不正確的 CSV 列。

在路徑合併那個任務上，單純的一行提示詞寫出的程式碼最少（6 行），但有四分之一的執行會讓 `../../` 檔名逃出目錄。Ponytail 寫了大約 9.5 行，並且 4/4 都通過。多出來的三行就是那個防止路徑穿越的檢查。這就是使用結構化規則集，而不是只叫你的代理「寫短一點」的全部理由。

還有兩點我想給他們按讚。基準測試的說明文有一節限制條件，老實點出自己的弱點（只有一個模型、n=4、安全性只是底線而非證明）。而且它也記錄了他們在先前一輪發現的污染 bug：外掛的 `SessionStart` hook 也在基準組上觸發，偷偷讓 ponytail 在自己身上生效。能發現並公開這件事，比表格裡任何一個數字都更能建立信任。

## 我會在哪裡提出質疑

**這只是提示，不是保證。** 這裡的一切都只是上下文視窗裡的指令。模型會偏離、在壓力下忽略規則，並且在不同供應商之間表現不一。沒有什麼能強制這個階梯一定被遵守。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1794289123&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055876&amp;bpp=2&amp;bdt=985&amp;idt=2&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1794289055%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1535x866%2C952x280%2C952x280%2C952x280%2C350x280%2C728x90%2C334x250%2C952x280%2C952x280&amp;nras=10&amp;correlator=1006475122378&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=117&amp;ady=6617&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=3230&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;psts=AOrYGsm9P6oz-RJEJOetdZpYKfYl_clbMzU3W1a2w4c66hAIcpXnXk__7rPBxxir-tQfaBRosR4fWuoUvVTQwtoojxWOwIJyKg6wnVr7F1WjHL93BDhFTY6b5on7YPyhg8TJVQHx%2CAOrYGskK3Tq6KpVrU1Yv2i-zgpccJGGmWaQkOcwVUtlW7fm3fchzK4BT5brDNXxCTlKndu20fwOzBqE7VyEiM3Wgo_jjqRZNmX13VBVH%2CAOrYGsnPn0ArihRd8oSHe7dmgh8FCkk7NbJRi7U-r-pU-6jlkkyO6coodbzb3TUQMTmnSPvRpTm7P73iIV5ut88cbwCLDs0XLz6LkzrjHBCgGqks4Q%2CAOrYGslpg-UIIdctNh7Tl08_ZwZC_0K5OrrlCtpJcL_FQTMcRHHRoeh4cuUMsMaTRXCZaOKhsxVPLqQZLS0lh8kxOApph9bNZ9CbaxGZW_e3gHzU6A%2CAOrYGskvNVTTX6HsohuEu3GTj86x-Xf-mmDTRDaP8ajirhFNOM5nJeZ6ntsGKn2PaiBuetwKMvVuJQWKUriEKcM5LVWdw1_Z8SFhWhml%2CAOrYGsk9BXR827AnH5j8Int3xmY7bQglxDkxFY-t_7en30WjeLT6ABTDlNWmoP9CCeyjLBzPEj6btjZoX2jFwit2ztCMXocOAwhlXTw&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1932%2C30%2C1932%2C30%2C1920%2C30%2C1775%2C947%2C1535%2C866&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=7&amp;uci=a!7&amp;btvi=9&amp;fsb=1&amp;dtd=67702" title="Advertisement" aria-label="Advertisement"></iframe>

**成效取決於你的代理在某種特定方式上很爛。** 這些收益來自會過度建置的代理。如果你用的是更強的模型，或者你本來就會寫很精準的工單，明確寫「使用原生輸入」之類的限制，那麼很多空間早就被吃掉了。README 本身也提到，某些推理模型可能會反過來，因為花了額外思考 token 去猶豫每個階梯。

## 值得試嗎？

值得，但要先釐清這個「值得」是哪一種。

嘗試成本只有兩個指令，再加上一個關閉它的斜線命令。沒有執行時成本，不會成為你專案裡的相依套件，也沒有綁定風險，而且卸載方式有文件說明（甚至包含一個清理它在外掛資料夾外寫入狀態的腳本）。在這個價位下，根本不太需要猶豫。

Repo: [github.com/DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)

基準測試說明與重現步驟： [benchmarks/results/2026-06-18-agentic.md](https://github.com/DietrichGebert/ponytail/blob/main/benchmarks/results/2026-06-18-agentic.md)

---

原文出處： [https://dev.to/arshtechpro/ponytail-the-ai-coding-skill-that-makes-your-agent-write-less-code-29l3](https://dev.to/arshtechpro/ponytail-the-ai-coding-skill-that-makes-your-agent-write-less-code-29l3)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3V2ey3) [49瀏覽](https://codelove.tw/@tony/post/3V2ey3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3V2ey3)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

🔥 [2026 年程式圈很熱門的 10 個技能](https://codelove.tw/@tony/post/a9yLpa)

##### 🏆 本月排行榜

🥇

[boluoji](https://codelove.tw/@boluoji123)

📝26 💬3 ❤️6

599

🥈

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝4 💬2 ❤️2

193

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="350" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=1414751727&amp;adf=2365317702&amp;pi=t.aa~a.4029027559~rp.3&amp;abgtt=6&amp;w=350&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1794289102&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=350x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055916&amp;bpp=2&amp;bdt=1025&amp;idt=2&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1794289055%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1535x866%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=1006475122378&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=1118&amp;ady=999&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=0&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1535%2C866&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=8&amp;uci=a!8&amp;btvi=4&amp;fsb=1&amp;dtd=46497" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1200" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=3809598800&amp;adf=3132389021&amp;pi=t.aa~a.1416458249~rp.1&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1794289173&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=1200x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055926&amp;bpp=1&amp;bdt=1036&amp;idt=1&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1794289055%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1794289055%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1535x866%2C952x280%2C952x280%2C952x280%2C350x280%2C728x90%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=1006475122378&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=168&amp;ady=8214&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=4815&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;psts=AOrYGsm9P6oz-RJEJOetdZpYKfYl_clbMzU3W1a2w4c66hAIcpXnXk__7rPBxxir-tQfaBRosR4fWuoUvVTQwtoojxWOwIJyKg6wnVr7F1WjHL93BDhFTY6b5on7YPyhg8TJVQHx%2CAOrYGskK3Tq6KpVrU1Yv2i-zgpccJGGmWaQkOcwVUtlW7fm3fchzK4BT5brDNXxCTlKndu20fwOzBqE7VyEiM3Wgo_jjqRZNmX13VBVH%2CAOrYGsnPn0ArihRd8oSHe7dmgh8FCkk7NbJRi7U-r-pU-6jlkkyO6coodbzb3TUQMTmnSPvRpTm7P73iIV5ut88cbwCLDs0XLz6LkzrjHBCgGqks4Q%2CAOrYGslpg-UIIdctNh7Tl08_ZwZC_0K5OrrlCtpJcL_FQTMcRHHRoeh4cuUMsMaTRXCZaOKhsxVPLqQZLS0lh8kxOApph9bNZ9CbaxGZW_e3gHzU6A%2CAOrYGskvNVTTX6HsohuEu3GTj86x-Xf-mmDTRDaP8ajirhFNOM5nJeZ6ntsGKn2PaiBuetwKMvVuJQWKUriEKcM5LVWdw1_Z8SFhWhml%2CAOrYGsk9BXR827AnH5j8Int3xmY7bQglxDkxFY-t_7en30WjeLT6ABTDlNWmoP9CCeyjLBzPEj6btjZoX2jFwit2ztCMXocOAwhlXTw%2CAOrYGslft0F9b9ucz96-RrKYO6edJGwm_w-YVQ_xtKNUHbNz6XKepjvvx2V_NJnX0Nmu8d2xHoIU_tDxXBePSNTQQA577vtrm_yaaUT-&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1932%2C30%2C1932%2C30%2C1920%2C30%2C1775%2C947%2C1535%2C866&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

> [!dark] Dark
> [川川｜全客製電商：開店平台限制多？我們幫你寫一套自己的。➡️ 了解更多](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1794289055&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3V2ey3&amp;pra=5&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI3LjIuMCIsImFybSIsIiIsIjE1Ni4wLjgwNzguODAiLG51bGwsMCxudWxsLCI2NCIsW1siTm90OkEtQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE1Ni4wLjgwNzguODAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTYuMC44MDc4LjgwIl1dLDBd&amp;dt=1794289055019&amp;bpp=4&amp;bdt=129&amp;idt=82&amp;shv=r20261109&amp;mjsv=m202611050101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1793860943%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1793860943%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1793860943%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=1006475122378&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1535&amp;bih=866&amp;scr_x=0&amp;scr_y=0&amp;eid=95401823%2C95404663%2C95407882%2C95408438%2C95408909%2C95408928%2C95409066%2C122882055%2C122882143%2C122882148%2C42533294%2C95407167%2C95405499&amp;oid=2&amp;pvsid=3802693772470154&amp;tmod=1260502756&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1535%2C866&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=114" title="Advertisement" aria-label="Advertisement"></iframe>