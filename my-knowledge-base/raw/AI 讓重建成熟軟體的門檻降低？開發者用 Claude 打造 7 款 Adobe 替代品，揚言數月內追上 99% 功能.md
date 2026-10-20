---
title: "AI 讓重建成熟軟體的門檻降低？開發者用 Claude 打造 7 款 Adobe 替代品，揚言數月內追上 99% 功能"
source: "https://techorange.com/2026/10/08/artcraft-software-ai-developer-adobe/"
author:
  - "[[吳玟錡]]"
published: 2026-10-08
created: 2026-10-13
description: "Adobe 長期以 Photoshop、Illustrator、Premiere 等 Creative Cloud 軟體主導創意工具市場，但現在一名軟體開發者試圖利用 AI 加速逆向工程，打造一套免費、開源的 Adobe 替代方案，甚至喊話要在數個月內達到接近完整的功能相容。"
tags:
  - "clippings"
status: "inbox"
---
![](https://techorange.com/app/uploads/2026/10/164e82bc76f7e194-scaled.png)

Adobe 長期以 Photoshop、Illustrator、Premiere 等 Creative Cloud 軟體主導創意工具市場，但現在一名軟體開發者試圖利用 AI 加速逆向工程，打造一套免費、開源的 Adobe 替代方案，甚至喊話要在數個月內達到接近完整的功能相容。

這項計畫來自 ArtCraft。ArtCraft 起步於 2025 年，原本是一款主打讓藝術家能控制 AI 的工具，讓使用者能對 AI 生成的圖片與影片進行更精細的後製。近日，ArtCraft 轉型發表一套包含 7 款開源應用程式的軟體組合，分別模仿 Adobe Photoshop、Illustrator、Premiere、Lightroom、After Effects、InDesign 與 Acrobat Pro 的介面與工具。

## 用 Claude 打造 7 款 Adobe 替代品，Rust 開發並支援 WebAssembly

ArtCraft 開發者 Brandon Thomas 在 Reddit 發表這項計畫時表示，他使用 Anthropic 的 Claude Opus 5.5，透過無塵室（clean-room）方式打造 Adobe 封閉原始碼軟體的替代版本。這些應用程式以 Rust 開發，同時提供 WebAssembly 版本，因此可以直接在瀏覽器中使用。

不過，這套軟體目前仍處於非常早期的 Alpha 階段。《Ars Technica》指出，Hacker News 等社群中的使用者已點出新軟體目前存在不少缺陷；《Crypto Briefing》也提到，這些功能缺漏與錯誤，讓這套軟體距離它想取代的 Adobe 產品仍有差距。

Thomas 卻對開發速度相當有信心。他在 Reddit 表示，將快速修正 Bug、加入新功能，並借助社群回報問題，在一個月內達到 100% 功能相容。幾天後，他在 Hacker News 上稍微調整說法，表示達到 99% 功能相容需要一段時間，但他確信這個時間尺度會是「幾個月，而不是幾年」。

Thomas 表示，他擁有約 10 年大型 Rust 專案開發經驗，而促使他打造 Adobe 替代方案的原因，是自己多次被「取消費用」這類誘導式設計所害。這些開源應用程式採用 MIT 或 Apache 授權，使用者可以免費使用。

至於開發與維護成本，Thomas 表示，團隊會透過 ArtCraft 原有的視覺 AI 模型與整合開發環境（IDE）提供的 Token 制存取服務獲得收入。這套 AI 影像生成模型也作為應用程式中的選項，與第三方模型並列。

## AI 加速軟體逆向工程，也可能帶來介面模仿的法律風險

Thomas 的做法並非完全創新的軟體開發模式。《Ars Technica》指出，過去已有許多開發者利用 AI 工具，加速長久以來相當耗費人力的逆向工程，也就是在不直接複製原始碼的情況下，重新建立另一套軟體的功能。Thomas 表示，自今年 2 月以來，他完全使用 AI 工具進行程式開發，並認為在不久的將來，一般開發者也能重建熱門付費軟體的開源版本。

《Ars Technica》指出，逆向工程在法律上普遍被認為可以接受，但如果 ArtCraft 應用程式的商業外觀（trade dress，即產品具識別性的整體外觀與設計）過度模仿 Adobe 的外觀與操作介面，仍可能面臨法律問題。

Thomas 對 AI 輔助開發的野心也不只停留在 Adobe。他認為，現在開發者已經可以用無塵室方式，打造功能一比一對應、高效能且跨平台的軟體。他甚至表示，開發者可以取代 Google Search、打造自己的智慧型手機與更好的基礎架構，並移除網路上所有廣告，讓整個科技產業擺脫劣化。

Thomas 也曾在 Hacker News 留言中提出「Software is over」（軟體已死）的說法，並宣稱自己「一次就生成了整個 Photoshop」。不過，《Crypto Briefing》指出，這些應用程式熟悉的介面雖然展現了 AI 輔助開發的速度，但功能仍未完成，Thomas 宣稱能追上 Adobe 的承諾仍有待證實。

＊本文開放合作夥伴轉載，資料來源： [《Ars Technica》](https://arstechnica.com/ai/2026/10/software-is-over-bold-ai-developer-takes-aim-at-adobe-with-open-source-clones/) 、 [《Crypto Briefing》](https://cryptobriefing.com/open-source-creative-suite-launch/) ，圖片來源：ArtCraft。

![](https://sslcode.adgeek.com.tw/public/images/popad_close_button.png)