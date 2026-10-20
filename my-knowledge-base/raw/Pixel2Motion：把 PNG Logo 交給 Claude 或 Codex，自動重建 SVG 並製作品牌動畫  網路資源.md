---
title: "Pixel2Motion：把 PNG Logo 交給 Claude 或 Codex，自動重建 SVG 並製作品牌動畫 | 網路資源"
source: "https://tools.wingzero.tw/articles/4038"
author:
  - "[[赤紅翼零]]"
published: 2026-08-27
created: 2026-08-27
description: "Pixel2Motion 是專為 Claude、Codex 打造的開源 AI Logo Animation Skill，可將 PNG、JPG 等點陣 Logo 重建為乾淨 SVG，再製作品牌動畫、互動式 HTML 與 GIF／影片預覽。"
tags:
  - "clippings"
status: processed
---
網站連結 [https://github.com/nolangz/pixel2motion](https://github.com/nolangz/pixel2motion)

Pixel2Motion 是一套專門為 Claude 與 Codex 設計的開源 AI Logo Animation Skill，可以將 PNG、JPG、WebP 或 Screenshot 中的點陣 Logo，先重新擬合成乾淨、平滑且適合動畫的 SVG，再進一步製作 Logo Reveal、品牌動態效果、互動式 HTML Demo，以及 GIF／影片預覽。它的完整流程可以概括成「Raster Logo → Smooth SVG → SVG Animation → Interactive HTML」。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=199514577&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787813461&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4038&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1787813417377&amp;bpp=1&amp;bdt=1546&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1787813397%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1787813397%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1787813397%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280&amp;nras=3&amp;correlator=2917912105767&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=2666&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95400150%2C95400776%2C122880597%2C122880333&amp;oid=2&amp;pvsid=2054429066061998&amp;tmod=102446034&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=0&amp;ifi=12&amp;uci=a!c&amp;btvi=2&amp;fsb=1&amp;dtd=44443" title="Advertisement" aria-label="Advertisement"></iframe>

Pixel2Motion 和一般「圖片轉 SVG」工具最大的差異，在於它並不把向量化視為最終目的。專案真正要解決的是 Brand Motion：SVG 必須先被整理成具有語意、容易控制的結構，後續才能讓 Logo 的不同部分按照設計好的節奏移動。換句話說，它不是把 PNG 自動 Trace 成數百個節點就算完成，而是要求 AI 找出盡可能簡潔、平滑，而且適合動畫控制的幾何結構。

這也是 Pixel2Motion 相當有辨識度的地方。許多 Raster-to-SVG 工具追求的是「像不像原圖」，Pixel2Motion 則同時考慮「像不像」與「這個 SVG 接下來好不好做動畫」。官方甚至明確指出，一個由 3 個具有語意的部分構成的 Logo 很容易設計動畫；如果 Logo 是由 400 個沿著像素邊緣 Trace 出來的節點組成，即使靜態看起來接近原圖，也很難做出專業且乾淨的 Motion Design。

因此，Pixel2Motion 的工作不是直接從圖片跳到動畫，而是先完成一輪相當嚴格的靜態 SVG 擬合與 QA。

它會反覆將 Vector Candidate 與原始 Raster Logo 疊合比較，檢查 Logo Mark 尺寸、文字基線、點的位置與筆畫粗細等細節。專案會使用 IoU（Intersection over Union）作為診斷指標，但官方並沒有設定一個所有 Logo 都必須達到的固定 IoU 門檻。真正的 Hard Gate 是 Smoothness 與 Structure：如果一個高 IoU 結果包含大量鋸齒節點，而另一個較簡潔的曲線結構視覺上更合理，Pixel2Motion 會偏向後者。

目前 Pixel2Motion 的主要輸出內容包括：

- `logo.svg` ：完成 QA、並針對 Motion 整理過結構的靜態 SVG
- `motion.css` ：控制各個 SVG Semantic ID 的動畫編排
- `logo_motion.html` ：不依賴額外套件的動畫展示頁
- `motion_spec.md` ：記錄 Motion Brief、動畫原則、Timeline、Easing 與 QA
- `fit_iterations` ：保存不同階段的幾何擬合比較
- `motion_frames` ：在固定時間點擷取動畫畫面
- `motion_strip.png` ：將動畫不同時間點排列成視覺 QA Strip
- `final_render.png` 與 `html_render.png` ：靜態 Rendering 檢查結果。

其中 `logo_motion.html` 不只是單純播放一次 Logo Animation。官方設計的 Showcase HTML 還包含 Replay、Slow Motion、Speed Control、QA Hooks，以及不同 Atomic Motion Studies，方便設計者檢查每個動畫元素的實際表現。

Pixel2Motion 對動畫本身也不是採取「隨便讓 Logo 飛進來」的方式。Skill 文件明確把 Disney's 12 Principles of Animation 納入 Motion Discipline，並要求先建立 Motion Brief，定義品牌個性、使用情境、Logo Parts 與 Choreography，再開始製作動畫。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=1429272977&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787813463&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4038&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1787813417383&amp;bpp=2&amp;bdt=1552&amp;idt=2&amp;shv=r20260825&amp;mjsv=m202608210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1787813397%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1787813397%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1787813397%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280%2C832x280&amp;nras=4&amp;correlator=2917912105767&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=4058&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=445&amp;eid=95400150%2C95400776%2C122880597%2C122880333&amp;oid=2&amp;psts=AOrYGsk15lChYKynvbBSaabOyONVwRXtIMjPl-eJ520MJegQEvQsfVnOe8rGDtc7IGodjmTDzdbpdT0yaD-VhU2Oj5WnPsWu6XNPiQW08R0K8ZaY%2CAOrYGsl8o2oF3XXdM7fStqBZGDuHtcvHTlfROaFi44UMuyi_POMkrpW79RRe8WdVbZtHAv7EARqIzx_xewKgBH5222R9ej98DJ5ZwWoQSxERsRs&amp;pvsid=2054429066061998&amp;tmod=102446034&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1990%2C106%2C1990%2C106%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=13&amp;uci=a!d&amp;btvi=3&amp;fsb=1&amp;dtd=45931" title="Advertisement" aria-label="Advertisement"></iframe>

這種方式比較接近真正的 Motion Designer 工作流程。例如科技品牌與兒童品牌即使使用相似的 Logo 結構，動畫節奏也不應完全相同。前者可能適合快速、精準、克制的 Movement，後者則可能允許更多 Squash、Overshoot 或較活潑的 Timing。Pixel2Motion 的 Skill 架構試圖讓 Coding Agent 先思考品牌 Motion Personality，再決定動畫，而不是只套用固定 Template。

另一個重要概念是官方所稱的「Final Frame Contract」。

所有動畫最後都必須準確回到已經通過 QA 的靜態 SVG。也就是說，Motion 可以在過程中使用 Reveal、Draw-on、Mask、Scale 或其他動態效果，但動畫結束後，Logo 必須回到原本已確認的品牌造型。這可以避免動畫製作過程為了視覺效果而無意間改變 Logo 最終比例或幾何。

Pixel2Motion v2 也開始處理更複雜的動畫問題，包括 Self-crossing Draw-on Choreography、Split-fill、Exact Easing Subdivision、Tip Glint，以及 Closed Variable-width Ribbon Fitting。這些能力比較適合筆畫型、Ribbon 型或具有交叉線條的 Logo，而不是只做簡單的 Fade In。

更值得注意的是，它把 Motion QA 做成可以量化與重現的流程。

一般 Logo Animation 很常靠設計師播放幾次後憑感覺判斷是否順暢；Pixel2Motion 則提供 Script，在指定的毫秒位置擷取動畫 Frame。例如可以在 0、300、700、1000、1250、1500 ms 擷取畫面，再排列成 `motion_strip.png` ，檢查 Logo 在不同時間點的狀態。

如果動畫涉及 Draw-on、Path Crossing、Mask 或 Motion Handoff，還能使用 `probe_motion_continuity.py` 針對特定時間點檢查 `stroke-dashoffset` 、 `offset-distance` 等屬性。這類工具主要是避免動畫在某個瞬間突然跳動、斷線或出現不自然的位置變化。

因此，Pixel2Motion 的完整工作方式更接近：

`Raster Logo → Vector Fitting → Geometry QA → Motion Brief → SVG Choreography → Motion Frames → Continuity QA → HTML / GIF / Video`

而不是：

`圖片 → AI → 動畫`

這個差異也決定了它比較適合對 Logo 品質有要求的設計與開發工作。

從技術環境來看，Pixel2Motion 本身需要 Python 3.10 以上版本，並使用 Pillow、NumPy 處理部分影像分析；Chrome 或 Chromium 負責 Geometry 與 HTML Rendering，Playwright 則用來進行可重現的 Frame Capture 與 Motion Continuity QA。

Repository 本身也不只放一份 Prompt。 `SKILL.md` 定義 Pixel-to-Vector-to-Motion 的主要工作流程， `references/` 保存動畫原則、Motion Personality、Reveal Pattern、HTML Template 與 Vector Fitting 參考資料； `scripts/` 則包含 Tracing、Rendering、Overlay、SVG Path Audit、Showcase HTML Generation、Frame Capture 與 Continuity Probe 等實際工具。

這表示 Pixel2Motion 比較適合理解成一套「Agent Skill + Deterministic Tools」的工作流。Claude 或 Codex 負責需要設計判斷的部分，例如理解 Logo 結構、規劃動畫個性與編排；可以用程式確定執行的 Rendering、Overlay 與 Frame Capture，則交給 Script 處理。

從實際使用情境來看，它很適合網站開場 Logo Reveal、App Splash Screen、品牌影片片頭、Loading Animation、Idle Motion 或 Hover Motion。尤其對本來就使用 Claude、Codex 進行 Vibe Coding 的開發者，可以直接把品牌 Logo 圖片交給 Agent，再取得可以嵌入網站的 SVG 與 HTML，而不一定需要另外進入 After Effects 製作動畫。

它也很適合 Designer 與 Developer 之間的交付。傳統 Motion Designer 可能交付 MP4、MOV 或 GIF，但這些格式放進網站後通常缺乏互動性；Pixel2Motion 產生的是具有語意結構的 SVG、CSS 與 HTML，因此前端開發者可以繼續控制播放時機、速度、Hover 或其他互動。

如果和一般 Image-to-SVG 工具比較，差異則更直接。Vectorizer 的工作通常在產生 SVG 時就結束；Pixel2Motion 認為 SVG 只是中間產物，而且必須為動畫重新建立具有語意的結構。這也是為什麼專案會把 Smoothness 與 Low-complexity Geometry 看得比單純提高 IoU 更重要。

目前 Pixel2Motion 採 MIT License 開源，GitHub Repository 也提供實際 Motion Gallery 與互動式 GitHub Pages Demo。官方另外正在建立較完整的商業 Pixel2Motion 服務，方向包含更成熟的 Logo-to-Motion Workflow、Project-ready Preview 與 Production Support；不過開源 Skill 本身仍可獨立使用。

整體而言，Pixel2Motion 最有意思的地方並不是「AI 可以讓 Logo 動起來」，而是把 Logo Motion 拆成一套可檢查的工程流程。它先要求 Agent 建立真正乾淨、可編輯、可動畫的 SVG，再進行品牌 Motion 設計，最後透過固定 Frame、Overlay 與 Continuity Probe 驗證結果。對正在使用 Claude、Codex 做網站與品牌視覺，希望把靜態 Logo 進一步轉成 Web-ready Motion 的設計師與開發者而言，這是一套定位相當清楚的開源 Skill。

<iframe frameborder="0" src="https://bebcdf6ebc114be90489cce8ae36202f.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>