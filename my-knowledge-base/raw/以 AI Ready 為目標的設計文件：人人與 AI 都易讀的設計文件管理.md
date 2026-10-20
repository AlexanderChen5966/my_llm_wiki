---
title: "以 AI Ready 為目標的設計文件：人人與 AI 都易讀的設計文件管理"
source: "https://codelove.tw/@tony/post/q5n80x"
author:
published:
created: 2026-06-15
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言=============================================之前在〈[把設計文件用 Markdown 管理，並自動轉換成 PDF 吧！](https://qiita.com/gr..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月14日](https://codelove.tw/@tony/post/q5n80x "2026-06-14 17:06:53")

## 以 AI Ready 為目標的設計文件：人人與 AI 都易讀的設計文件管理

## 前言

之前在〈 [把設計文件用 Markdown 管理，並自動轉換成 PDF 吧！](https://qiita.com/grhg/items/eb2935ba815db16a16a4) 〉這篇文章中，我介紹了以 Markdown 管理設計文件的做法。

這裡則是作為延伸，進一步具體整理一個構成：將以 Markdown 管理的設計文件以 HTML 形式公開，並同時考慮在 GitHub 上的審查，以及 AI 的活用。

## 動機

我們團隊目前是用 Confluence 來管理設計文件。

編輯與瀏覽都很方便，日常運作上也沒有遇到太大的困擾。不過，當想要透過 AI 來參照設計文件、產生修改提案，或是上傳後更新時，就會碰到幾個不便之處。

- 透過 MCP 取得與更新的成本較高
- 設計文件本文與圖表不容易一起處理
- 差異不容易以程式碼審查的流程來檢視

因此，我嘗試規劃一個以 GitHub 上好處理的 Markdown 為基礎，同時盡量不要降低人類閱讀時可讀性的架構。

## 實作內容

1. 將設計文件交由 GitHub 管理
2. 透過 GitHub Actions 自動產生 HTML 頁面，並在 GitHub Pages 上公開
3. 以 PR 為單位也生成 HTML 頁面，方便確認外觀

## 建立的架構

為了驗證，我做了一個把 Markdown 設計文件網站化的範例，使用的是 **MkDocs Material** 。

拿來驗證的範例也已經公開在 GitHub 上。

- [tgrhg/md-to-design-doc](https://github.com/tgrhg/md-to-design-doc)

大致上的架構如下。

```
mkdocs.yml
requirements.txt
package.json
.github/workflows/pages.yml
docs/
  index.md
  architecture/sample-ec-service.md
  assets/images/sample-system-context.svg
scripts/
  write-version.mjs
```

整理一下各自的角色如下。

檔案角色 `mkdocs.yml` 主題、導覽、Markdown 擴充設定 `docs/**/*.md` 設計文件本文 `docs/assets/images/*` 圖片資源`.github/workflows/pages.yml` 發佈到 GitHub Pages 的流程如下圖所示。

## MkDocs 是什麼

[MkDocs](https://www.mkdocs.org/) 是一個將 Markdown 文件轉換成靜態 HTML 網站的工具。

這裡會使用它來將設計文件轉成 HTML。它和 GitHub Pages 這類靜態託管服務也很搭，整體架構相對簡單。

MkDocs 本身是用來從 Markdown 產生網站的基礎。在它上面套用的主題則使用 [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) 。

Material for MkDocs 的特色不只是外觀，也很容易強化搜尋、導覽、目錄、日文支援等功能。因為它很容易整理成對人類友善的文件網站，所以我選了它。

## 本文管理

設計文件本文就直接以 Markdown 放在 `docs/` 底下。

用 Markdown 管理的話，本文的變更會直接成為 Pull Request 的差異。

例如，非功能需求的表格，用一般的 Markdown 就已經足夠了。

```
| 分類 | 要求 | 補充 |
| --- | --- | --- |
| 可用性 | 每月可用率 99.9% | 將 CDN 與 Backend API 做冗餘設計 |
| 效能 | 商品搜尋 P95 低於 500ms | 善用快取 |
| 安全性 | 管理介面必須使用 SSO | 保存稽核紀錄 |
```

## 圖與圖片的處理

設計文件中圖表也很重要。過去我們常用 PlantUML，但因為無法直接在 GitHub 上顯示，所以改採 Mermaid。

雖然也有考慮過把 PlantUML 動態轉成 SVG 再顯示，但那樣會變成要轉成 HTML 之後才能確認，比較不方便。因為我希望在 GitHub 上就能直接看見，也比較容易追蹤差異，所以優先選擇 Mermaid。

種類適合用途管理方式Mermaid架構圖、流程圖、循序圖直接寫在 Markdown 內圖片檔畫面截圖、既有資料放在 `docs/assets/images/` Mermaid 或圖片都能直接在 GitHub 上顯示，因此在倉庫中閱讀設計文件時也很方便。

## 本機確認

在本機可以用以下方式建置。

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
npm test
python3 -m http.server 8000 -d site
```

## 發佈到 GitHub Pages

當合併到 main 分支後，會透過 GitHub Actions 建置網站並發佈到 GitHub Pages。

workflow 也支援 push / PR / 手動執行。

```
on:
  push:
    branches: [main]
  pull_request:
    types: [opened, reopened, synchronize, closed]
  workflow_dispatch:
```

對 main 的 push 會更新 production，PR 則會公開 preview。

## PR 時的預覽顯示

光看 Markdown 的差異也能審查，但像下面這些排版問題，不看 HTML 很難注意到。

- Mermaid 或圖片顯示異常
- 圖片連結失效
- 表格寬度
- 在導覽中的呈現方式

因此，我改成每個 PR 都公開一個 preview。PR 會把建置好的網站放到 `gh-pages` 分支的 `/previews/pr-<PR編號>/` 底下，讓差異審查和外觀確認可以分開進行。

實作上，是透過 GitHub Actions 將建置好的網站部署到 PR 對應的路徑，再由 workflow 組出 preview URL，自動寫進 PR 留言裡。同時，如果能從側邊欄連到該 preview 頁面，就更容易和 production 對照查看。

```
production:
  https://<owner>.github.io/<repo>/

PR preview:
  https://<owner>.github.io/<repo>/previews/pr-<PR編號>/
```

PR 的流程如下。

preview 的公開是使用 `gh-pages` 分支的子目錄，並且也會在 PR 留言中顯示 URL。如果有需要，也可以同時更新 preview 清單與側邊欄，讓導引更完整。

這一段有些地方是用 workflow 和 script 比較偏土法煉鋼的方式處理，所以直接看實際檔案會最快。

- [pages.yml](https://github.com/tgrhg/md-to-design-doc/blob/main/.github/workflows/pages.yml)

組合 Preview 頁面 URL 的 workflow\`\`\`

- name: Compute preview URL  
	id: preview\_url  
	env:  
	OWNER: ${{ github.repository\_owner }}  
	REPO: ${{ [github.event.repository.name](http://github.event.repository.name/) }}  
	PR\_NUMBER: ${{ github.event.pull\_request.number }}  
	run: |  
	if \[ "$REPO" = "$ [OWNER.github.io](http://owner.github.io/) " \]; then  
	BASE\_URL=" [https://$OWNER.github.io](https://$owner.github.io/) "  
	else  
	BASE\_URL=" [https://$OWNER.github.io/$REPO](https://$owner.github.io/$REPO) "  
	fi  
	echo "url=$BASE\_URL/previews/pr-$PR\_NUMBER/" >> "$GITHUB\_OUTPUT"

將 preview URL 寫入 PR 留言的 workflow\`\`\`

- name: Comment preview URL  
	uses: actions/github-script@v7  
	env:  
	PREVIEW\_URL: ${{ steps.preview\_url.outputs.url }}  
	with:  
	script: |  
	const body = \[  
	'### Design Docs Preview',  
	`🔎 **Preview:** ${process.env.PREVIEW_URL}`,  
	\].join('\\n');

實際上，我是讓 workflow 自動在 PR 留言中貼上 preview URL 與 version index 的連結。

[![pr-preview-comment.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3294382%2F16e97914-3d67-4ddd-a338-f59b4227a78b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=59cfd803667e461b2047a30a89cababf)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3294382%2F16e97914-3d67-4ddd-a338-f59b4227a78b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=59cfd803667e461b2047a30a89cababf)

從留言打開 preview 頁面後，就能直接確認產生出來的 HTML。

[![pr-preview-animation.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3294382%2F11832419-4dd9-4f8a-afb9-7d0683c421f0.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6ca28d91ef1c92362fca31e72685bea9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3294382%2F11832419-4dd9-4f8a-afb9-7d0683c421f0.gif?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6ca28d91ef1c92362fca31e72685bea9)

## 注意事項

## GitHub Pages 的設定

若要透過 URL 顯示 PR preview，就需要先將 GitHub Pages 設為以 branch-based 方式啟用。

```
Settings > Pages > Build and deployment
Source: Deploy from a branch
Branch: gh-pages
Folder: / (root)
```

## 來自 fork 的 PR

同一個倉庫內的 PR 處理起來比較簡單，但來自 fork 的 PR 就需要注意權限與 secret 的使用。

這種情況下，也可以考慮不公開 preview，只保留 artifact 的構成，我認為這也是可行的。

## 結語

如果你希望設計文件同時容易被 AI 處理，又能維持人類閱讀時的可讀性，我認為 Markdown + MkDocs Material + GitHub Pages 是很值得嘗試的組合。

如果你有興趣，不妨先從一份小型設計文件開始試試看。

---

原文出處： [https://qiita.com/grhg/items/eee10528b403baf89631](https://qiita.com/grhg/items/eee10528b403baf89631)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/q5n80x) [8瀏覽](https://codelove.tw/@tony/post/q5n80x)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/q5n80x)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝14 ❤️1

445

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1781493246&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fq5n80x&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuNTQiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE0OS4wLjc4MjcuNTQiXSxbIkNocm9taXVtIiwiMTQ5LjAuNzgyNy41NCJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1781493246045&amp;bpp=2&amp;bdt=184&amp;idt=54&amp;shv=r20260611&amp;mjsv=m202606090101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1781493177%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1781493177%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1781493177%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=8152961706290&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099008%2C95390667%2C95390787&amp;oid=2&amp;pvsid=6931475489181882&amp;tmod=1085849897&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=103" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260611/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>