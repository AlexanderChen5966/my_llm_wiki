---
title: "Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態"
source: "https://www.kocpc.com.tw/archives/665541"
author:
  - "[[ClaireC]]"
published: 2026-08-18
created: 2026-08-18
description: "許多 Home Assistant 用戶喜歡分享他們打造的智慧家庭控制面板，我一直佩服那些以 3D 模型呈現家中設備狀態的作品，雖然製作過程看起來相當繁瑣，但最近看到一位用戶的作品後，我真的有點心動，甚至開始想自己也做一個。"
tags:
  - "clippings"
status: "processed"
---
許多 Home Assistant 用戶喜歡分享他們打造的智慧家庭控制面板，我一直佩服那些以 3D 模型呈現家中設備狀態的作品，雖然製作過程看起來相當繁瑣，但最近看到一位用戶的作品後，我真的有點心動，甚至開始想自己也做一個。

![Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818120038_0_d63ba9.jpg "Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態 - 電腦王阿達")

一般常見的做法，是利用 Sweet Home 3D 或 Blender 建立自家房屋的 3D 平面圖，將牆壁、窗戶、門與家具一一放入場景，再渲染成圖片。接著，你可以為不同燈光狀態建立多張渲染圖，例如「燈全關」與「燈全開」版本，並在 Home Assistant 儀錶板中依照設備狀態切換圖片。這種 3D 儀錶板視覺效果確實很吸引人。

![Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818120617_0_ff62c6.jpg "Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態 - 電腦王阿達")

但 Reddit 用戶 [u/anonym9173](https://www.reddit.com/r/homeassistant/comments/1v4uk1e/interactive_pokemon_floorplan_dashboard/?tl=zh-hans&utm_source=embedv2&utm_medium=post_embed&utm_content=whitespace&embed_host_url=https%3A%2F%2Fwww.howtogeek.com%2Fpokemon-style-home-assistant-dashboard%2F) 的作品更令人驚喜，他依照 [u/katschung](https://www.reddit.com/r/homeassistant/comments/1ht3ipu/how_to_create_a_floorplan_pokemon_style/) 提供的詳細教學，製作了一套 2D 像素風格的平面圖，從俯視角度重現自家房屋的每個房間、門、家具與燈光，風格宛如經典的俯視角寶可夢遊戲，既復古又可愛，讓人一看就愛上。

作者表示，這套像素平面圖花了大量時間與心力完成。他使用 GIMP 製作，搭配 RPG 圖塊集，並為一些難以找到的物件（例如電視櫃、衣帽架）自行繪製圖塊。我甚至做了個小實驗，發現 AI 影像生成技術或許能加快製作流程，未來可能會讓這類作品更容易上手。

<iframe height="740" src="https://embed.reddit.com/r/homeassistant/comments/1v4uk1e/interactive_pokemon_floorplan_dashboard/?embed=true&amp;ref_source=embed&amp;ref=share&amp;utm_medium=widgets&amp;utm_source=embedv2&amp;utm_term=23&amp;utm_name=post_embed&amp;embed_host_url=https%3A%2F%2Fwww.kocpc.com.tw%2Farchives%2F665541" width="640px" allowfullscreen="true" sandbox="allow-scripts allow-same-origin allow-popups" allow="clipboard-read; clipboard-write"></iframe>

更棒的是，這個寶可夢風格的控制面板不只是好看而已，它是真正能反映智慧家庭狀態的即時儀錶板。在 Reddit 貼文附帶的影片中，作者展示了自己走進走廊、再進入客廳的過程，隨著人體感應器偵測到移動，各房間的燈光會自動亮起，而儀錶板也會幾乎同步更新。每盞燈的亮度都會在平面圖上即時呈現，讓人一眼就能看出哪些燈正在運作。

<iframe frameborder="0" src="https://f944a2813e2f5c861f1e471f80e3178d.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="320" height="480" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>

不只燈光如此。當作者指向電視並打響指開啟電源時，平面圖上的像素電視螢幕也會從黑色切換成亮起的畫面，象徵電視已開啟。甚至同一房間的兩台電腦，也能透過區域感應器分別控制螢幕亮度，確保只有正在使用的那台電腦會亮起，使用者離開後則自動熄滅。

![Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818120949_0_2b27b1.jpg "Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態 - 電腦王阿達")

影片中還展示了白天與夜晚兩種視圖，白天版本明亮清晰，夜晚版本則較暗，只在燈光開啟的區域呈現亮色。儀錶板會依照日出與日落自動切換視圖。這個設計非常實用，因為夜晚模式能更清楚顯示哪些燈還亮著，讓你睡前只需瞥一眼，就能確認家中是否還有設備未關閉。更重要的是，這個儀錶板完全具備互動功能。你可以直接點擊平面圖上的燈或其他設備來開關它們。按鈕的透明度被調整為零，因此不會破壞整體的像素風格。長按設備還能呼叫更多選項。甚至連電視也能透過平面圖控制，點擊電視圖示後會跳出虛擬遙控器視窗，讓你直接在瀏覽器中操作。

![Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818120352_0_1b5006.jpg "Home Assistant 用戶打造 2D 寶可夢風格儀表板，還能即時反映居家狀態 - 電腦王阿達")

老實說，我平常並不熱衷於用控制面板管理智慧家庭，但這個專案真的讓我心癢難耐。我甚至開始想，如果能把家變成薩爾達傳說的像素世界，應該會超級可愛，只是我得小心別真的忍不住把家裡的花瓶都砸了。