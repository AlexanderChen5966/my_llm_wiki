---
title: "Ponytail 安裝教學：Codex、Claude Code 和 Gemini CLI 怎麼用"
source: "https://knightli.com/zh-tw/2026/06/24/ponytail-ai-agent-coding-plugin-guide/"
author:
  - "[[KnightLi]]"
published: 2026-06-24
created: 2026-07-09
description: "Ponytail 是一組給 AI 編碼代理使用的規則和外掛，目標是讓 Codex、Claude Code、Copilot CLI、Gemini CLI 等工具在寫程式前先判斷是否真的需要新增程式碼，優先複用現有實作、標準函式庫、平台能力和已安裝依賴。"
tags:
  - "clippings"
status: processed
---
Ponytail 是一個很有意思的 AI 編碼外掛。

它不是讓 AI 更會「炫技」，而是反過來提醒 AI：先別急著寫程式，先看看這件事能不能不寫、能不能複用、能不能用標準函式庫、能不能用瀏覽器或平台自帶能力解決。

專案地址： [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)

如果你經常遇到這種情況：

```text
只是想要一個日期選擇器，AI 卻裝了元件庫、寫了封裝、加了樣式，還開始討論時區。
只是想改一個小邏輯，AI 卻順手抽象出三層 helper。
只是想修一個 bug，AI 卻重構了半個檔案。
```

那 Ponytail 的思路就很對味：讓 AI 像一個懶但可靠的資深開發者一樣，先找最小改動。

## Ponytail 是什麼

Ponytail 可以理解成一組給 AI 編碼代理看的工作規則。

它會提醒代理在寫程式前先按一條「階梯」判斷：

```text
1. 這個東西真的需要存在嗎？
2. 程式庫裡是不是已經有了？
3. 標準函式庫能不能做？
4. 平台原生能力能不能做？
5. 已安裝依賴能不能做？
6. 一行程式碼能不能解決？
7. 最後才寫剛好夠用的新程式碼。
```

這不是 code golf，也不是盲目追求短。它強調的是少做無效工作：能刪就刪，能複用就複用，能用原生能力就別造輪子。

<iframe width="809" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1866383169&amp;adf=954940372&amp;abgtt=6&amp;w=809&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1783483743&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=809x280&amp;url=https%3A%2F%2Fknightli.com%2Fzh-tw%2F2026%2F06%2F24%2Fponytail-ai-agent-coding-plugin-guide%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=808&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783579248467&amp;bpp=4&amp;bdt=1041&amp;idt=4&amp;shv=r20260706&amp;mjsv=m202607010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1782198190%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1782198190%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1782198190%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0&amp;nras=2&amp;correlator=8730560328786&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=1348&amp;biw=1486&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95393484%2C95340252%2C95340254&amp;oid=2&amp;pvsid=6841507982655867&amp;tmod=11738386&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1486%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=31292" title="Advertisement" aria-label="Advertisement"></iframe>

README 裡有一個很典型的例子：日期選擇器。

普通 AI 可能會裝 `flatpickr` ，寫 wrapper，補樣式，再解釋一堆邊界情況。

Ponytail 風格會先想到：

```html
<input type="date">
```

這就是它想訓練 AI 做的判斷。

## 它解決的不是「程式碼短」，而是「別過度設計」

Ponytail 最容易被誤解成「讓 AI 寫一行程式碼」。

其實更準確的說法是：讓 AI 少做沒必要的設計。

它保留安全邊界，不鼓勵刪掉必要的校驗、錯誤處理、安全處理和可訪問性。該做的仍然要做，只是不要為了一個很小的需求引入一堆額外複雜度。

<iframe width="809" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1866383169&amp;adf=1239212730&amp;abgtt=6&amp;w=809&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1783483743&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=809x280&amp;url=https%3A%2F%2Fknightli.com%2Fzh-tw%2F2026%2F06%2F24%2Fponytail-ai-agent-coding-plugin-guide%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=808&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783579248478&amp;bpp=1&amp;bdt=1053&amp;idt=1&amp;shv=r20260706&amp;mjsv=m202607010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1782198190%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1782198190%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1782198190%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C809x280&amp;nras=3&amp;correlator=8730560328786&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=2266&amp;biw=1486&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95393484%2C95340252%2C95340254&amp;oid=2&amp;pvsid=6841507982655867&amp;tmod=11738386&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1486%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=31290" title="Advertisement" aria-label="Advertisement"></iframe>

適合它處理的場景包括：

- 前端小功能；
- 表單控制項；
- 簡單工具函式；
- 設定檔調整；
- 小型 bug 修復；
- 程式碼審查時發現過度實作；
- AI 改程式前的約束提醒；
- 讓代理優先查找現有實作。

不適合把它理解成：

- 永遠只寫一行；
- 永遠不加依賴；
- 永遠不重構；
- 為了少程式碼犧牲可維護性；
- 為了速度跳過閱讀程式碼。

Ponytail 的重點是「懶於實作，不懶於閱讀」。

<iframe width="809" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1866383169&amp;adf=2965993031&amp;abgtt=6&amp;w=809&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1783483743&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=809x280&amp;url=https%3A%2F%2Fknightli.com%2Fzh-tw%2F2026%2F06%2F24%2Fponytail-ai-agent-coding-plugin-guide%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=808&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783579248495&amp;bpp=1&amp;bdt=1069&amp;idt=1&amp;shv=r20260706&amp;mjsv=m202607010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1782198190%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1782198190%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1782198190%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C809x280%2C809x280&amp;nras=4&amp;correlator=8730560328786&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=3202&amp;biw=1486&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95393484%2C95340252%2C95340254&amp;oid=2&amp;pvsid=6841507982655867&amp;tmod=11738386&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1486%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=31278" title="Advertisement" aria-label="Advertisement"></iframe>

## 支援哪些 AI 編碼工具

從 README 看，Ponytail 覆蓋的工具很多，包括：

- Claude Code；
- Codex；
- GitHub Copilot CLI；
- Pi agent harness；
- OpenCode；
- Gemini CLI；
- Antigravity CLI；
- CodeWhale；
- Swival；
- OpenClaw；
- Cursor、Windsurf、Cline、Aider、Kiro、Zed 等規則檔方式。

這類專案的好處是：你不一定非要換主力 AI 工具。

如果你現在用的是 Codex，就按 Codex 外掛方式裝；如果你用 Claude Code，就走 Claude Code 外掛市場；如果只是想把規則放進編輯器，也可以複製對應的規則檔。

<iframe width="809" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1866383169&amp;adf=4104435823&amp;abgtt=6&amp;w=809&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1783483743&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=809x280&amp;url=https%3A%2F%2Fknightli.com%2Fzh-tw%2F2026%2F06%2F24%2Fponytail-ai-agent-coding-plugin-guide%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=808&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783579248503&amp;bpp=1&amp;bdt=1078&amp;idt=1&amp;shv=r20260706&amp;mjsv=m202607010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1783579247%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1783579247%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1783579247%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C809x280%2C809x280%2C809x280%2C728x90&amp;nras=6&amp;correlator=8730560328786&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=4149&amp;biw=1486&amp;bih=1003&amp;scr_x=0&amp;scr_y=138&amp;eid=95393484%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsnYGTuFV7E1aSSD_hu9XqF-kqTUCP756Z7iDBglHGTk88Um17sWjLHeqYgwAJ4_bcCVwAEvSxo6gqOZde4cEbyaM1WxVO3jSbd1834jU5IHQlCAu2pqZZLwhvDDXkt6W6A%2CAOrYGsnqO-anFYICHDkTbIFIX9CNLuS_J7-A3JX_vmSYdgfxX-eJYJO2vx1cxjAUkYF7hx5xHy-Czh1qyDGAVhriWL9DP3IAcGDEYXsTHerhoENiFQ%2CAOrYGsn4pOlPqjC0hYYxcxc3KIoOIVc3z00XJw_9eGcOhX7n8zNtQF6ZHiQLlWPnnJmmhKKk3QfTULVEkOfUTr4UVtFXDqSN-AkFm7g-lkRD8XEYLQ%2CAOrYGslKGK-0We4cDM0VlCxd6MLiX1Bf_jh33ygkwsW8WFn2eU9P7kmLvmES5H3g8CC36qN1m5sfuUq5E0mHq2ce0sXJOgkPetiM2PGcAkVRUXMIFOU&amp;pvsid=6841507982655867&amp;tmod=11738386&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1726%2C1050%2C1486%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=5&amp;uci=a!5&amp;btvi=5&amp;fsb=1&amp;dtd=32798" title="Advertisement" aria-label="Advertisement"></iframe>

## Codex 裡怎麼安裝 Ponytail

如果你使用的是 Codex CLI，README 給出的入口是：

```bash
codex plugin marketplace add DietrichGebert/ponytail
codex
```

然後在 Codex 裡：

1. 打開 `/plugins` ；
2. 選擇 Ponytail marketplace；
3. 安裝 Ponytail；
4. 打開 `/hooks` ；
5. 檢查並信任它的兩個 lifecycle hooks；
6. 開一個新對話開始使用。

如果你用的是 Codex 桌面版，README 也提到：安裝完成後重新啟動應用程式，外掛會被識別。

<iframe width="809" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1866383169&amp;adf=1432462350&amp;abgtt=6&amp;w=809&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1783483743&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=809x280&amp;url=https%3A%2F%2Fknightli.com%2Fzh-tw%2F2026%2F06%2F24%2Fponytail-ai-agent-coding-plugin-guide%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=808&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783579248520&amp;bpp=1&amp;bdt=1095&amp;idt=1&amp;shv=r20260706&amp;mjsv=m202607010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1783579247%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1783579247%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1783579247%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C809x280%2C809x280%2C809x280%2C728x90%2C809x280&amp;nras=7&amp;correlator=8730560328786&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=5042&amp;biw=1486&amp;bih=1003&amp;scr_x=0&amp;scr_y=1052&amp;eid=95393484%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsnYGTuFV7E1aSSD_hu9XqF-kqTUCP756Z7iDBglHGTk88Um17sWjLHeqYgwAJ4_bcCVwAEvSxo6gqOZde4cEbyaM1WxVO3jSbd1834jU5IHQlCAu2pqZZLwhvDDXkt6W6A%2CAOrYGsnqO-anFYICHDkTbIFIX9CNLuS_J7-A3JX_vmSYdgfxX-eJYJO2vx1cxjAUkYF7hx5xHy-Czh1qyDGAVhriWL9DP3IAcGDEYXsTHerhoENiFQ%2CAOrYGsn4pOlPqjC0hYYxcxc3KIoOIVc3z00XJw_9eGcOhX7n8zNtQF6ZHiQLlWPnnJmmhKKk3QfTULVEkOfUTr4UVtFXDqSN-AkFm7g-lkRD8XEYLQ%2CAOrYGslKGK-0We4cDM0VlCxd6MLiX1Bf_jh33ygkwsW8WFn2eU9P7kmLvmES5H3g8CC36qN1m5sfuUq5E0mHq2ce0sXJOgkPetiM2PGcAkVRUXMIFOU%2CAOrYGsm6oyS1izLBLCQdQ0gzS2lNSmxafIi2xFTjfRrfK5F6kqcwS_u3xjv06RY7PYrtbVFeD74XUxOyRUvRlNG44tHpjzcUNg2JRERhUtSJPVHtRDI&amp;pvsid=6841507982655867&amp;tmod=11738386&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1726%2C1050%2C1486%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=6&amp;uci=a!6&amp;btvi=6&amp;fsb=1&amp;dtd=34483" title="Advertisement" aria-label="Advertisement"></iframe>

注意一點：Ponytail 的 Claude Code 和 Codex 外掛會執行兩個很小的 Node.js lifecycle hooks，所以 `node` 需要在 PATH 裡。README 也說明，如果沒有 `node` ，skills 仍然可以工作，只是 always-on 啟用不會自動生效。

可以先檢查：

```bash
node -v
```

如果命令找不到，先安裝 Node.js，或者確認目前非互動 shell 的 PATH 能找到 `node` 。

## Claude Code 怎麼安裝

Claude Code 的安裝命令是：

```text
/plugin marketplace add DietrichGebert/ponytail
```

然後再發一條：

```text
/plugin install ponytail@ponytail
```

README 特別提醒，這兩條需要分兩次傳送。

<iframe width="809" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1866383169&amp;adf=105872308&amp;abgtt=6&amp;w=809&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1783483743&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=809x280&amp;url=https%3A%2F%2Fknightli.com%2Fzh-tw%2F2026%2F06%2F24%2Fponytail-ai-agent-coding-plugin-guide%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=808&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783579248524&amp;bpp=1&amp;bdt=1099&amp;idt=0&amp;shv=r20260706&amp;mjsv=m202607010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1783579247%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1783579247%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1783579247%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C809x280%2C809x280%2C809x280%2C728x90%2C809x280%2C809x280&amp;nras=8&amp;correlator=8730560328786&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=6154&amp;biw=1486&amp;bih=1003&amp;scr_x=0&amp;scr_y=2179&amp;eid=95393484%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsnYGTuFV7E1aSSD_hu9XqF-kqTUCP756Z7iDBglHGTk88Um17sWjLHeqYgwAJ4_bcCVwAEvSxo6gqOZde4cEbyaM1WxVO3jSbd1834jU5IHQlCAu2pqZZLwhvDDXkt6W6A%2CAOrYGsnqO-anFYICHDkTbIFIX9CNLuS_J7-A3JX_vmSYdgfxX-eJYJO2vx1cxjAUkYF7hx5xHy-Czh1qyDGAVhriWL9DP3IAcGDEYXsTHerhoENiFQ%2CAOrYGsn4pOlPqjC0hYYxcxc3KIoOIVc3z00XJw_9eGcOhX7n8zNtQF6ZHiQLlWPnnJmmhKKk3QfTULVEkOfUTr4UVtFXDqSN-AkFm7g-lkRD8XEYLQ%2CAOrYGslKGK-0We4cDM0VlCxd6MLiX1Bf_jh33ygkwsW8WFn2eU9P7kmLvmES5H3g8CC36qN1m5sfuUq5E0mHq2ce0sXJOgkPetiM2PGcAkVRUXMIFOU%2CAOrYGsm6oyS1izLBLCQdQ0gzS2lNSmxafIi2xFTjfRrfK5F6kqcwS_u3xjv06RY7PYrtbVFeD74XUxOyRUvRlNG44tHpjzcUNg2JRERhUtSJPVHtRDI%2CAOrYGsl2gRmOa5Y_FrlnLJ6GFWzkU6FCtXcjXXfxeXXcKCU1jRdtySm8tkWtUcNfsm4QIoDMnoaBVUinyg9sFkREbuQaF9zFt_PRHN3y7fk8wWdzYA&amp;pvsid=6841507982655867&amp;tmod=11738386&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1726%2C1050%2C1486%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=7&amp;uci=a!7&amp;btvi=7&amp;fsb=1&amp;dtd=35877" title="Advertisement" aria-label="Advertisement"></iframe>

如果你用的是 Claude 桌面應用程式，沒有 `/plugin` 命令，需要從 UI 裡新增個人外掛市場：

1. 打開 Customize；
2. 點擊 personal plugins 旁邊的加號；
3. 建立外掛並新增 marketplace；
4. 選擇 Add from repository；
5. 輸入 Ponytail 的 GitHub 倉庫地址。

## GitHub Copilot CLI 怎麼安裝

Copilot CLI 可以用：

```bash
copilot plugin marketplace add DietrichGebert/ponytail
copilot plugin install ponytail@ponytail
```

在互動式 Copilot CLI 會話裡，也可以用 slash 命令：

```text
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

安裝後，Copilot CLI 裡可以透過 Ponytail 命令使用不同能力，例如 README 中提到的：

```text
/ponytail:ponytail ultra
/ponytail:ponytail-review
```

## Gemini CLI 和 Antigravity CLI

Gemini CLI 的安裝方式是：

```bash
gemini extensions install https://github.com/DietrichGebert/ponytail
```

它會把規則集作為每次會話的上下文載入，並註冊 `/ponytail` 相關命令。

Antigravity CLI 可以用：

```bash
agy plugin install https://github.com/DietrichGebert/ponytail
```

README 裡還提到，Google 正在把 Gemini CLI 改名為 Antigravity CLI，所以一段時間內兩種路徑可能會並存。實際使用時，按你目前安裝的 CLI 名稱來。

## OpenCode 怎麼用

OpenCode 可以在 `opencode.json` 中加入：

```json
{ "plugin": ["@dietrichgebert/ponytail"] }
```

如果你從本地 checkout 執行，也可以指向本地外掛檔：

```json
{ "plugin": ["./.opencode/plugins/ponytail.mjs"] }
```

這種方式適合已經在 OpenCode 裡管理專案設定的人。

## 只想用規則，不想裝外掛怎麼辦

Ponytail 倉庫裡還放了很多不同工具的規則檔。

例如：

- `AGENTS.md` ；
- `.cursor/rules/` ；
- `.windsurf/rules/` ；
- `.clinerules` ；
- `.github/copilot-instructions.md` ；
- `.kiro/steering/` 。

如果你的工具支援讀取專案級規則，可以直接複製對應檔案。

比如 VS Code 的 Codex 擴充會讀取 `AGENTS.md` 。如果你把 Ponytail 的 `AGENTS.md` 放到專案根目錄，或者放到全域 Codex 設定位置，就可以用規則方式讓 Codex 遵循這套思路。

外掛方式更完整，規則檔方式更輕。

## Ponytail 適合怎麼提示

裝完以後，不要只期待它「自動變聰明」。你也可以在任務裡主動配合它。

比如：

```text
請先檢查現有程式碼裡是否已經有類似實作，優先複用；只有確實沒有時再新增最小實作。
```

或者：

```text
這個需求不要過度設計。先判斷是否可以用標準函式庫、原生 HTML 控制項或已有依賴解決。
```

再比如做 code review：

```text
請按 Ponytail 思路審查這次改動，重點找過度抽象、重複實作、不必要依賴和可以用原生能力替代的地方。
```

這類提示和 Ponytail 的規則方向一致，效果會更穩定。

## 適合用 Ponytail 的典型任務

### 1\. 前端控制項

比如日期、顏色、檔案上傳、數字輸入、開關、下拉選擇。

AI 很容易一上來裝元件庫，但很多場景原生 HTML 控制項已經夠用。

### 2\. 工具函式

比如去重、排序、格式化、路徑處理、日期處理。

先看語言標準函式庫和專案已有 helper，再決定要不要寫新函式。

### 3\. 設定改動

比如 ESLint、TypeScript、Hugo、Vite、Docker、CI 設定。

很多設定問題只需要改一行，不需要寫腳本或新增工具鏈。

### 4\. 小 bug 修復

Ponytail 思路適合先找最小修復點，避免 AI 為了修一個 bug 引出大重構。

### 5\. 程式碼審查

它很適合當作「過度設計檢查器」。

你可以讓 AI 看 diff，專門回答：

- 有沒有重複造輪子；
- 有沒有沒必要的新依賴；
- 有沒有可以直接刪除的程式碼；
- 有沒有複雜度超過需求的實作；
- 有沒有可以複用現有模組的地方。

## 使用時要注意什麼

Ponytail 的方向很好，但不要把它變成新的教條。

下面這些場景不能為了少程式碼而硬省：

- 輸入校驗；
- 權限檢查；
- 資料丟失保護；
- 並發和交易安全；
- 使用者隱私；
- 可訪問性；
- 錯誤處理；
- 關鍵日誌；
- 測試覆蓋。

簡單不是粗糙。

如果一個功能涉及安全邊界、資金、使用者資料、生產環境操作，最小實作也必須把邊界處理清楚。

## 我的建議用法

如果你第一次用 Ponytail，可以按這個順序試：

1. 先在一個小專案裡安裝；
2. 用一個很容易過度實作的需求測試，比如日期選擇器或顏色選擇器；
3. 對比沒有 Ponytail 時 AI 會寫什麼；
4. 再讓它審查一次已有 diff；
5. 最後再放到日常專案裡。

不要一上來就拿大重構測試。

Ponytail 最能體現價值的場景，是那些「本來很小，但 AI 特別容易寫大」的任務。

## 一句話總結

Ponytail 不是讓 AI 變成只會寫短程式碼的工具，而是讓 AI 在動手前多問一句：

```text
這段程式碼真的需要寫嗎？
```

如果你用 Codex、Claude Code、Copilot CLI、Gemini CLI 這類 AI 編碼代理，經常被它們的過度實作煩到，可以試試 Ponytail。它不能替你判斷所有架構問題，但很適合給 AI 加上一層「少造輪子、少繞路、先複用」的約束。