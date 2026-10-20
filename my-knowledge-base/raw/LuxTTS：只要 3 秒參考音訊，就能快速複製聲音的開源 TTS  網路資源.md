---
title: "LuxTTS：只要 3 秒參考音訊，就能快速複製聲音的開源 TTS | 網路資源"
source: "https://tools.wingzero.tw/articles/4029"
author:
  - "[[赤紅翼零]]"
published: 2026-08-18
created: 2026-08-18
description: "LuxTTS 是主打高速、輕量與 Voice Cloning 的開源 TTS 模型，支援短音訊聲音複製、48kHz 語音輸出，官方效能超過 150x Realtime，VRAM 可低於 1GB。"
tags:
  - "clippings"
status: "processed"
---
網站連結 [https://github.com/ysharma3501/LuxTTS](https://github.com/ysharma3501/LuxTTS)

LuxTTS 是一套主打高速、輕量與 Voice Cloning 的開源文字轉語音模型（Text-to-Speech，TTS）。它以 ZipVoice 架構為基礎，透過蒸餾與改良的 Sampling 方法，把推論流程縮減至約 4 Steps，同時搭配自訂 48kHz Vocoder。官方公布的效能可超過 150x Realtime，也就是理論上一秒鐘能生成超過 150 秒的語音。

更值得注意的是它對硬體的要求。官方表示 LuxTTS 可以控制在 1GB VRAM 以內，不一定需要高階 NVIDIA GPU；除了 CUDA，也提供 CPU 與 Apple Mac 的 MPS 執行方式，而且 CPU 仍可達到快於即時播放的生成速度。這讓 LuxTTS 和許多追求大型模型、高運算量的 Voice Cloning 專案形成不同路線：它更在意「聲音品質、速度與模型效率」之間的平衡。

LuxTTS 的主要用途是語音複製。使用者準備一段參考語音，例如 WAV 或 MP3，再輸入希望朗讀的文字，模型就會根據參考聲音產生新的語音。官方建議 Voice Cloning 至少準備 3 秒的參考音訊，而範例預設會取約 5 秒進行 Prompt Encoding。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=4116832479&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787036429&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4029&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1787036425177&amp;bpp=2&amp;bdt=1982&amp;idt=3&amp;shv=r20260817&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1787036411%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1787036411%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1787036411%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x908%2C784x280&amp;nras=3&amp;correlator=5681742066441&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=2725&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C42533293&amp;oid=2&amp;pvsid=7776534181590853&amp;tmod=27976224&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=0&amp;ifi=12&amp;uci=a!c&amp;btvi=2&amp;fsb=1&amp;dtd=3938" title="Advertisement" aria-label="Advertisement"></iframe>

這種工作方式很適合需要大量產生同一個角色聲音的情境。例如先錄製幾秒角色語音，之後便可以利用文字批次產生旁白；如果正在建立 AI 影片自動化流程，也能把腳本生成結果直接交給 LuxTTS，再把輸出的 WAV 送進字幕、對嘴或影片剪輯流程。

目前 LuxTTS 比較值得注意的功能與特色包括：

- 支援 Voice Cloning，以短參考音訊複製聲音特徵
- 官方建議至少使用 3 秒參考語音
- 輸出 48kHz 語音，而非只停留在常見的 24kHz
- 官方公布 GPU 速度可超過 150x Realtime
- CPU 也能達到 Faster-than-Realtime 的生成速度
- VRAM 使用量可控制在 1GB 以內
- 支援 CUDA、CPU 與 macOS MPS
- 可控制語音生成速度與 Sampling 參數
- 已有 Gradio、ComfyUI、ONNX 等社群整合
- 模型與程式碼採 Apache 2.0 License。

48kHz 是 LuxTTS 很明確的一項產品差異。官方指出，許多 TTS 模型的輸出仍以 24kHz 為主，而 LuxTTS 使用自訂 48kHz Vocoder，目標是在維持高速推論的同時提供更清晰的語音。實際輸出範例也直接以 48,000Hz 儲存 WAV。

不過，48kHz 並不能直接等同於「聲音一定比其他模型自然」。取樣率只是音訊品質的一部分，語音自然度仍會受到模型、參考音訊、文字內容、發音準確度與 Sampling 設定影響。因此比較合理的理解是：LuxTTS 提供了較高取樣率的輸出管線，而不是單靠 48kHz 就能判定整體 TTS 品質。

它的速度則和模型設計有直接關係。LuxTTS 沿用 ZipVoice 架構，但官方表示模型已蒸餾至 4 Steps，並使用改良的 Sampling Technique。實際 API 中的 `num_steps` 可以自行調整，官方認為 3～4 Steps 是效率與品質之間較好的折衷。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=1429272977&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787036432&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4029&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1787036425221&amp;bpp=3&amp;bdt=2027&amp;idt=3&amp;shv=r20260817&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1787036411%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1787036411%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1787036411%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x908%2C784x280%2C832x280%2C235x600&amp;nras=5&amp;correlator=5681742066441&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=3932&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=337&amp;eid=95397678%2C42533293&amp;oid=2&amp;psts=AOrYGskg3H8LuFjaPaSp4VFCwghmGYy8S5shDyGqXTgIOg1Xq30Q4KTAVqFRcMWXrjnB4_hOOk9EWGvD69ch4x4ZkuSh4_gbekLzTfuKJQ%2CAOrYGsmXm2VZOmcEZ9KLwxb-EusisoMb7XJwAxJk9eFb7Dbwe2KUIJLNqYtwu-QVoZXJ9kd3lGDyy2DlR7aYOIzJjQxHtq38qu8SYZSYU29eNhrO0A&amp;pvsid=7776534181590853&amp;tmod=27976224&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=13&amp;uci=a!d&amp;btvi=4&amp;fsb=1&amp;dtd=7354" title="Advertisement" aria-label="Advertisement"></iframe>

這也代表 LuxTTS 並不是單純追求 Benchmark 上的極限速度，而是讓使用者自行決定 Quality／Speed Trade-off。若需要快速大量生成語音，可以降低 Steps；若比較重視成品品質，也能增加 Steps，只是推論時間會跟著提高。

模型另外提供幾項可以直接調整的參數。例如 `speed` 控制講話速度，數值降低會讓語音變慢； `rms` 可調整參考音訊的音量標準； `t_shift` 則牽涉品質與 Word Error Rate 之間的取捨。官方提醒，較高的 `t_shift` 可能帶來較好的聲音表現，但也可能增加發音錯誤；降低數值則有機會改善 Pronunciation。

另一個實際使用時可能遇到的問題是 Metallic Sound。官方提供 `return_smooth` 選項，如果生成結果出現金屬感，可以啟用 Smooth Processing，不過代價可能是聲音沒有原本那麼清晰。這類參數顯示 LuxTTS 還是比較偏向開發者工具，而不是完全把所有細節隱藏起來的一鍵式 TTS 網站。

硬體支援則是它很有吸引力的一部分。官方範例可以直接指定 `cuda` 在 NVIDIA GPU 上執行，也能切換成 `cpu` ；Apple Mac 則已有 MPS 支援。加上模型可以塞進 1GB VRAM，使它比較有機會放進一般個人電腦、迷你工作站或其他 Local AI 環境，而不必為了 Voice Cloning 準備昂貴的 GPU。

LuxTTS 的社群整合發展也很快。官方 README 已經列出 Lux-TTS-Gradio、OptiSpeech、LuxTTS-ComfyUI 與 ONNX 等社群專案。對不想直接撰寫 Python 的使用者，可以透過 Gradio 類介面操作；正在建立生成式影片工作流程的人，則可以利用 ComfyUI Node，把 TTS 放進圖片、影片與聲音生成節點之間。

從使用情境來看，LuxTTS 很適合 AI 影片旁白、有聲內容、遊戲角色語音、虛擬人物，以及需要大量 TTS 的自動化服務。假設一個 AI Video Pipeline 每天需要產生數百段旁白，模型推論成本與速度就會比單次生成品質更重要。此時不到 1GB VRAM、短步數推論以及 Reference Audio 重複使用，都能降低整體運算負擔。

它也適合本地 AI 應用。與只能呼叫雲端 API 的 Voice Cloning 服務相比，LuxTTS 可以直接在自己的硬體執行，模型與程式碼採 Apache 2.0 License。對需要自行控制部署環境的開發者而言，這種架構會比完全依賴第三方 API 更有彈性。

目前仍有一些發展中的部分。官方 Roadmap 包含 LuxTTS v1.5 與 Float16 Inference；現行版本主要使用 Float32，作者估計改成 Float16 後速度還可能接近再提升一倍。不過 Float16 仍列在 Roadmap，因此現階段不應把這項預期效能當成已經正式提供的功能。

整體而言，LuxTTS 最有辨識度的地方並不是單純「可以複製聲音」，而是試圖把 Voice Cloning 做得夠小、夠快，又維持 48kHz 輸出。對追求最高單次語音品質的人來說，仍值得與其他大型 TTS 模型實際試聽比較；但如果需求是 Local TTS、AI 影片批次旁白、ComfyUI 工作流或大量自動語音生成，LuxTTS 對速度與 VRAM 的取向會相當有吸引力。

<iframe frameborder="0" src="https://df23381787e89d6ffaddfe7f7be18294.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>