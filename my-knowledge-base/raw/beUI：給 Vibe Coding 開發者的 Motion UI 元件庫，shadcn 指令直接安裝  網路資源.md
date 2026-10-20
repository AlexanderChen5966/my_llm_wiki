---
title: "beUI：給 Vibe Coding 開發者的 Motion UI 元件庫，shadcn 指令直接安裝 | 網路資源"
source: "https://tools.wingzero.tw/articles/4045"
author:
  - "[[赤紅翼零]]"
published: 2026-09-01
created: 2026-09-01
description: "beUI 是專為 React、Next.js 打造的開源 Animated Component Library，結合 Motion、Tailwind CSS 與 shadcn Registry，提供 Modal、Tabs、Toast、Table、Text Animation 等動態 UI 元件。"
tags:
  - "clippings"
status: "processed"
---
網站連結 [https://beui.dev/](https://beui.dev/)

beUI 是一套專為 React 與 Next.js 開發者打造的 Animated Component Library，主打可以直接 Copy-paste 到專案中的動態 UI 元件。它使用 Motion（Framer Motion）處理動畫、Tailwind CSS 負責樣式，並透過 shadcn registry 發布，因此開發者不是安裝一個封閉的黑盒 UI Package，而是直接把 TypeScript Component Source 拉進自己的專案，再依產品需求修改外觀與互動。官方目前將免費版本定位為 Free、Open Source、Fully Customizable 的動態元件庫。

它和一般 React UI Library 最大的差異，在於 beUI 把「Motion」放在元件設計的核心。Button、Modal、Tabs、Toast、Tooltip、Drawer、Slider 這些東西本身並不新鮮，但 beUI 會進一步處理 Spring Animation、Morphing、Blur Cross-fade、Shared Layout、Swipe、3D Tilt、Glare、View Transition 等互動效果。對正在製作 Landing Page、SaaS、Dashboard 或希望產品介面更有動態質感的前端開發者來說，可以省下大量自己研究動畫 Timing 與 Interaction Detail 的時間。

beUI 的使用方式也很接近 shadcn/ui 所帶動的「Source Code Ownership」模式。網站每個元件都提供實際 Preview、安裝方式與 TypeScript 程式碼，開發者可以透過 shadcn CLI 把指定 Component 加進專案。安裝完成後，程式碼就在自己的 Codebase 裡，而不是只能透過 Package API 使用。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=4116832479&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228612&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4045&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224756128&amp;bpp=1&amp;bdt=1109&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788228448%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280&amp;nras=3&amp;correlator=4983681045654&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=2834&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95395999%2C95399244%2C95399630%2C95400780%2C122880598%2C122880743%2C122880333&amp;oid=2&amp;pvsid=3609428527295729&amp;tmod=684280329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=0&amp;ifi=12&amp;uci=a!c&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這種模式對需要高度客製 UI 的專案特別實用。傳統 Component Package 如果動畫不符合需求，可能需要覆寫大量 CSS 或研究 Library API；Copy-paste Component 則可以直接修改 Animation Variant、Spring Physics、Layout、Class Name 與 Interaction Logic。代價是元件後續維護也會更偏向專案自己負責，而不是只等待 npm Package 升級。

目前 beUI 首頁列出的 Motion Primitives 已涵蓋相當多互動情境，例如 Morphing Modal、Animated Toast Stack、Action Swap、Dock、Tabs、Dynamic Island、Command Palette、Expandable Action Bar、Expandable Tabs、Tilt Card、Bottom Sheet、Switch、Tooltip、Text Animation、Number Animation、Bouncy Accordion、Range Slider、Theme Toggle、Drawer、Multi-chain Swap、OTP Input、Swipeable List 與 Bloom Menu。官方元件目錄目前顯示 41 個 Components，另外還有 22 個 Blocks。

如果整理 beUI 比較有代表性的功能與特色，可以看到：

- **Motion-first Components** ：元件不是做好靜態外觀後才附加 Transition，而是從互動與動畫開始設計。
- **React／Next.js 導向** ：適合現代 React 前端專案直接整合。
- **Tailwind CSS** ：樣式可以延續既有 Tailwind Design Token 與 Utility Workflow。
- **shadcn Registry** ：透過 CLI 將 Component Source 直接加入專案。
- **可直接修改 Source Code** ：不需要被封裝好的 Component API 完全限制。
- **大量 Spring 與 Morphing 動畫** ：特別強調具有物理感、連續性的 UI Transition。
- **Reduced Motion 考量** ：部分元件明確提供 reduced-motion-safe 的替代呈現。
- **Components + Blocks** ：除了單一 Primitive，也開始提供由多個元件組成的完整介面區塊。
- **OpenUI 整合** ：可以把 beUI 註冊成 Generative UI Framework 可使用的元件集合。
- **免費版 + Pro** ：免費元件開源，同時提供進階商業元件與 Blocks。

實際看看元件，就比較容易理解 beUI 所謂的 Animated Components 並不只是「加個 Fade In」。

例如 Morphing Modal 會在 Modal 內切換不同 View 時，讓同一個 Panel 自動 Morph Height，內容則使用 Blur Cross-fade 過渡。這種動畫如果自己從零實作，需要處理 Layout Measurement、Exit／Enter Animation 與內容高度變化；beUI 已經把這些互動整理成可以直接使用的 Component。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=1429272977&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228624&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4045&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224756132&amp;bpp=1&amp;bdt=1114&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788228448%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280%2C832x280%2C235x600&amp;nras=5&amp;correlator=4983681045654&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=4605&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1132&amp;eid=95395999%2C95399244%2C95399630%2C95400780%2C122880598%2C122880743%2C122880333&amp;oid=2&amp;psts=AOrYGsmVND9m6R3x_O8fOiJVRwEwpnaTNQJITDPFztPwftLMNn9h1nMHMr4ZEOtvavWf0srLUf5WInmn1RqI3AXZswtYrr799gWPGZBXeelyoo0KnA%2CAOrYGsm1dIQKcfOM3Tb2xzg5p4pIVWdWSYq-sf2pO19-MANY6WBSlULp2te-HD7a3LImugqNzslmXAxxvh75B9Oi40aDfckudcEHB54UU7Ta04bly2Y&amp;pvsid=3609428527295729&amp;tmod=684280329&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=13&amp;uci=a!d&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Dynamic Island 則借用了 iOS Dynamic Island 的互動概念，讓 Pill 在不同 Live Activity View 之間改變尺寸，Shell 使用帶彈性的 Resize，內容再搭配 Blur Crossfade。這類元件不一定適合每一種企業系統，但用在音樂播放器、Upload Progress、AI Generation Status 或即時任務提示，就能快速建立相當明顯的動態效果。

Tilt Card 則加入 3D Perspective Hover 與跟隨游標的 Glare；Dock 模擬 macOS Dock，並使用滑動式 Active Pill；Bottom Sheet 提供 Drag、Snap Points 與 Inertia；Swipeable List 則採 Mobile-style Swipe Interaction，左右滑動 Row 可以露出 Contextual Actions。這些都屬於「功能不複雜，但自己做好非常花時間」的 Micro-interaction。

文字與數字動畫也是 beUI 很適合 Landing Page 的一部分。Text Animation 包含 Spring Reveal、Chromatic Sweep、Shimmer Loading、Letter Cascade 與 Character Scramble；Number Animation 則處理 Count-up 與 Rolling Digit Ticker。Text Animation 的獨立頁面目前還提供 Text Scramble、Dia Text Animation、Text Reveal、Text Shimmer 與 Text Cascade 等不同形式。

Theme Toggle 的做法也比一般 Sun／Moon Icon 切換更完整。它利用 View Transition API，讓整個頁面在切換 Light／Dark Theme 時透過 Rectangle、Circle Clip-path 或類似百葉窗的 Slats 動畫重新展開。這種效果很適合 Portfolio、設計導向產品或 Landing Page，用一個很小的 Interaction 提升整體完成度。

beUI 也不只有展示型元件。近期推出的 Multi Select 提供可搜尋 Options、可以移除的 Animated Tokens，以及會依碰撞情況調整的 Morphing Panel；File Tree 則提供 File／Folder Primitives、Spring Branch Animation、滑動 Selection Indicator 與完整 Keyboard Navigation。這表示它正在從「漂亮動畫收藏」逐漸往真正可以用在 Product UI 的元件發展。

Table 就是一個很明顯的例子。除了 Data Table，還提供 Editable Table 與 Async Table；API 包含 Cell Editing、Column Rename、Insert／Delete Row、Insert／Delete Column、Virtualization、Overscan、Infinite Loading 與 Skeleton Rows 等能力。這已經不是單純拿來展示動畫的 Demo，而是開始處理 Dashboard 與 Data-heavy Application 的需求。

除了 Components，beUI 也提供 Blocks。首頁目前列出的類型包括 404／Not Found、Feedback Widget、Bloom Menu、Sign Up Form、OTP Input、Wallet Card、Prediction Market 與 File Upload 等。

Block 和 Primitive 的差異，在於它已經替開發者組好一段更完整的 Product Experience。例如 File Upload 不只是 Dropzone，而是包含檔案列表、Upload Progress、Success、Failure、Retry、Removal，以及圖片 Preview 與 Audio Waveform 等狀態；另一個 Queue Variant 則可以呈現多檔上傳、進度與連線失敗。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=3975518427&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228625&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4045&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224756139&amp;bpp=1&amp;bdt=1121&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788228448%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280%2C832x280%2C235x600%2C832x280&amp;nras=6&amp;correlator=4983681045654&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=5913&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2337&amp;eid=95395999%2C95399244%2C95399630%2C95400780%2C122880598%2C122880743%2C122880333&amp;oid=2&amp;psts=AOrYGsmVND9m6R3x_O8fOiJVRwEwpnaTNQJITDPFztPwftLMNn9h1nMHMr4ZEOtvavWf0srLUf5WInmn1RqI3AXZswtYrr799gWPGZBXeelyoo0KnA%2CAOrYGsm1dIQKcfOM3Tb2xzg5p4pIVWdWSYq-sf2pO19-MANY6WBSlULp2te-HD7a3LImugqNzslmXAxxvh75B9Oi40aDfckudcEHB54UU7Ta04bly2Y%2CAOrYGsm0FyhyPwqhf65s42XC6U612GL0aXRQULrGYxY6cGstQ26sAh_t4sLk6abLh2zTvNlz4DqTF-EfkPajQAwSjBxaYhxSq4fUrXGbIjUbs2pEpQ&amp;pvsid=3609428527295729&amp;tmod=684280329&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=14&amp;uci=a!e&amp;btvi=5&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Sign Up Form 同樣不是單純 Input + Button。範例會在使用者離開欄位後才顯示 Validation Error，修正內容後立即清除錯誤，同時加入 Password Strength、Password Reveal 與 Animated Submit Lifecycle。這類 Block 的價值，是直接提供一段已經考慮 Interaction State 的介面，而不只是視覺 Layout。

beUI 另一個值得注意的方向是 AI 與 Generative UI。

官方提供 OpenUI Integration Guide。OpenUI 的概念不是讓模型輸出 Markdown，而是讓 AI 產生抽象 UI Tree，再由 React Runtime 將每個 Node 對應到開發者允許的 Component。beUI 可以在這個架構下被註冊成 Custom OpenUI Component Library，讓模型產生的介面真正使用 beUI 的 Animated Components。

這和單純要求 AI「幫我寫一個漂亮的 React 頁面」有明顯不同。Generative UI 如果沒有 Component Constraint，模型每次可能寫出完全不同的 Button、Card 與 Animation；把 beUI 註冊成可用元件後，AI 只能從團隊允許的 Component Set 組合 UI，更容易維持一致的 Design Language。

因此對現在使用 AI Coding、Vibe Coding 或 Generative UI 的開發者而言，beUI 不只是「找動畫 Copy Code」的網站，也可以成為 Agent 能直接取用的 UI Building Blocks。

如果與 shadcn/ui 比較，兩者比較適合互補而不是互相取代。shadcn/ui 已經建立非常成熟的基礎 UI 工作方式，而 beUI 延續類似的 Copy-paste 與 Registry 思維，把重點放到更有表現力的 Motion Interaction。可以把前者理解成產品介面的基礎骨架，而 beUI 更適合補上動畫、轉場與較具特色的互動。

如果和 Framer Motion 本身比較，差異就更清楚。Motion 提供的是動畫能力與 API；beUI 提供的是已經設計完成的 Interaction Pattern。前者讓開發者「可以做動畫」，後者則幫忙回答「這個 Modal、Toast、Dock 或 Tabs 到底應該怎麼動」。

這對不熟 Motion Design 的工程師特別有幫助。UI 動畫真正困難的地方通常不是讓元素從 `opacity: 0` 變成 `opacity: 1` ，而是 Timing、Spring、Hierarchy、Exit Transition、Layout Continuity 與 Interaction Feedback。beUI 把這些細節封裝在可以直接閱讀和修改的 Component Source 裡。

網站目前採免費開源與 beUI Pro 並行的模式。免費版首頁標示 MIT License；Pro 則透過 Private shadcn Registry 發布 Premium Components 與 Blocks。Pro 文件目前要求 React 19、Next.js 與 Tailwind CSS v4，使用者取得 License Token 後，可以將私人 Registry 加入 `components.json` ，再透過 shadcn CLI 安裝商業元件。

這種商業模式也沒有改變免費版的核心使用方式：開發者仍然可以先從公開元件庫挑選需要的 Motion Component，直接把 Source 加進專案；只有需要更多 Premium Blocks 與進階內容時才需要考慮 Pro。

從實際使用情境來看，beUI 特別適合三種專案。第一種是 Landing Page 與 Portfolio，需要快速建立有記憶點的 Hero Interaction、Text Animation、CTA 與 Scroll Effect；第二種是 SaaS／Web App，希望 Modal、Toast、Tabs、Table、Upload、Command Palette 等日常元件具有更細緻的 Motion；第三種則是使用 Claude Code、Codex、Cursor 等 AI Coding Tool 快速開發產品，希望直接指定成熟 Component，而不是每次讓 AI 從零發明一套動畫。

當然，動畫越多不代表 UX 越好。Dashboard、企業後台或高頻操作工具如果每個 Button 都彈跳、每個 Panel 都 Morph，反而可能降低效率。beUI 真正適合的使用方式，是挑選能表達狀態轉換、操作結果或視覺層級的 Motion，而不是把所有元件全部套進同一個產品。網站部分元件已經考慮 Reduced Motion，也顯示這套 Library 並非只追求視覺效果。

整體而言，beUI 的市場定位相當清楚：它不是重新發明 React 的基礎元件，而是提供一批「已經把 Motion Design 做好」的 React／Next.js Components。Motion、Tailwind CSS、shadcn Registry，加上直接擁有 Source Code 的使用方式，使它很適合現在強調快速開發與高度客製的前端工作流。尤其對已經習慣 shadcn/ui、Tailwind 與 AI Coding 的開發者來說，beUI 可以扮演一個很實際的 Motion Layer——需要更有質感的 Modal、Toast、Tabs、Loader、Table 或 Upload Interaction 時，不必每次從 `motion.div` 開始重新設計。