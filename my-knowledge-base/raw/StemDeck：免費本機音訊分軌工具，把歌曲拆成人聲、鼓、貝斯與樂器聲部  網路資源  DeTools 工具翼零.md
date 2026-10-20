---
title: "StemDeck：免費本機音訊分軌工具，把歌曲拆成人聲、鼓、貝斯與樂器聲部 | 網路資源 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3958"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-25
created: 2026-06-25
description: "StemDeck 是免費開源的本機音訊分軌工具，可將 MP3、WAV、FLAC 分離成人聲、鼓、貝斯等六軌，支援 YouTube 音訊處理、多軌混音與匯出，適合音樂創作、Remix、扒歌與練習使用。"
tags:
  - "clippings"
status: processed
---
StemDeck 是一款免費、開源、主打本機運算的音訊分軌工具，主要用途是將 MP3、WAV、FLAC 等音訊檔案分離成最多六個 stem，包括人聲、鼓、貝斯、吉他、鋼琴與其他聲部。使用者也可以貼上 YouTube URL 進行處理，但官方明確說明，它不是下載器，而是用來處理使用者有權處理的音訊內容。所有流程都在自己的電腦上完成，不需要帳號、不需上傳檔案，也沒有訂閱或額度限制。

它的定位比較接近 Moises、LALAL.AI 這類雲端分軌服務的開源替代方案，但方向更偏向「免費、本機、隱私優先」。如果使用者只是想為個人練習、扒歌、轉錄、Remix 或創作工作流取得分軌，StemDeck 提供了相當直接的解法；若需要更精緻的模型品質、手機 App、批次處理或更完整的音樂人工具，商業服務仍可能更適合。

從功能來看，StemDeck 不只是把音訊拆開而已。它提供類似 DAW 的多軌混音介面，使用者可以在分離後直接播放各個 stem，並進行靜音、獨奏、音量平衡、波形縮放、區段循環播放，也能匯出單一 stem 或自訂混音檔。專案使用 Demucs `htdemucs_6s` 模型進行六軌分離，並會自動偵測可用運算裝置，例如 NVIDIA CUDA、Apple Silicon MPS 或 CPU fallback。

整理來看，StemDeck 的核心特色包括：

- 支援 MP3、WAV、FLAC 與 YouTube URL 匯入
- 可分離人聲、鼓、貝斯、吉他、鋼琴與其他聲部
- 使用 Demucs `htdemucs_6s` 開源模型
- 提供 DAW 風格多軌播放與波形編輯介面
- 支援 mute、solo、音量推桿、VU meter 與區段循環
- 可匯出單一 stem 或自訂 `mix.wav`
- 具備 BPM、key、scale、LUFS 與 sample peak 分析
- 所有音訊處理都在本機完成，不上傳雲端

這些功能讓 StemDeck 特別適合音樂學習者、樂手、製作人與創作者。例如吉他手可以把歌曲中的吉他聲部分離出來練習，也可以移除人聲製作伴奏；製作人則能快速取得鼓、貝斯或人聲 stem，用於分析編曲、做 remix 草稿或研究混音結構。

StemDeck 的另一個重要特色是隱私與自主性。許多雲端分軌工具需要上傳音訊到第三方伺服器，並依照方案限制處理次數、檔案長度或輸出品質；StemDeck 則將處理工作放在使用者自己的電腦上，因此音訊不會離開本機，也不受帳號、訂閱或雲端額度限制。代價是處理速度會受到硬體影響，有 GPU 會比較快，只有 CPU 時可能較慢。

<iframe width="825" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=3666733072&amp;adf=828129217&amp;pi=t.aa~a.2909957178~i.15~rp.4&amp;abgtt=13&amp;w=825&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1782349762&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=825x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fsn%2F3958&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=825&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782348370898&amp;bpp=1&amp;bdt=18966&amp;idt=0&amp;shv=r20260624&amp;mjsv=m202606180101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1782349666%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1782349666%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1782349666%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=825x280%2C0x0%2C1680x938%2C255x600%2C255x600&amp;nras=3&amp;correlator=4906287957079&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=285&amp;ady=2010&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95393485%2C95340253%2C95340255&amp;oid=2&amp;psts=AOrYGslJoiqO2PiSbPASMN7hTUlJ1z55Fydd23rZUrd2h1z2ig68gru_G1bRsQME4vdfxTyJMoOcwIi3IyP10EVsL5xFXyychD5bV_nXBQ&amp;pvsid=360193056968716&amp;tmod=801988014&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Fweb%2Fpage%2F1&amp;fc=384&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=247x742_l&amp;bz=0&amp;pgls=CAs.&amp;ifi=16&amp;uci=a!g&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

整體而言，StemDeck 是一款針對音樂練習與創作流程設計的本機分軌工具。它不追求成為最完整的商業音樂平台，而是把「音訊分離、多軌播放、簡單混音與匯出」這幾個核心需求做好。對於想免費、私密地處理自己音訊素材的人來說，StemDeck 是一個相當實用的開源選擇。