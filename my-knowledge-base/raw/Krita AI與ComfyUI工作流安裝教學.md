---
title: "Krita AI與ComfyUI工作流安裝教學"
source: "https://vocus.cc/article/683a6cf9fd897800012ba83c"
author:
  - "[[映嶙YingLin]]"
published: 2025-05-31
created: 2026-07-22
description: "這篇文章提供教學使用Krita AI算圖，透過krita-ai-diffusion讀取ComfyUI製作的工作流。步驟涵蓋軟體安裝、設定、模型下載與ComfyUI插件安裝等，並附上教學工作流下載連結。"
tags:
  - "clippings"
status: processed
---
目標:使用Krita AI算圖，透過krita-ai-diffusion讀取ComfyUI製作的工作流。

以下介紹krita-ai-diffusion及ComfyUI的安裝。

****1.下載krita本體**** [https://krita.org/zh-tw/](https://krita.org/zh-tw/)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fbdfc7f68-f09e-43fd-9f84-5ae99e910786.png&width=740&sign=nbQuOKjg57neC9MP5Ycqfaby0tLi96epbXDYOLBtgFg) ![zoomable](https://images.vocus.cc/bdfc7f68-f09e-43fd-9f84-5ae99e910786.png)

****2.下載krita-ai-diffusion**** [https://github.com/Acly/krita-ai-diffusion/releases/tag/v1.34.0](https://github.com/Acly/krita-ai-diffusion/releases/tag/v1.34.0)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F876717d7-1fae-4f81-bd96-3089086873e2.png&width=740&sign=Ox8Fd4sU0AIV_E-vCL58-enb0OXWMGI9RRcuZOwEyFw) ![zoomable](https://images.vocus.cc/876717d7-1fae-4f81-bd96-3089086873e2.png)

****3.安裝krita-ai-diffusion**** [****https://docs.interstice.cloud/installation/****](https://docs.interstice.cloud/installation/)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F0ec4f9ab-7ce6-4919-a9ff-090060cac5da.png&width=740&sign=88oEwqg_Ubg8D133uZqQyVUbc7hKeMrQcxL1QO6gwco) ![zoomable](https://images.vocus.cc/0ec4f9ab-7ce6-4919-a9ff-090060cac5da.png)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb6ce539b-dd62-4e49-b69c-1678a43a8456.png&width=740&sign=VrW4nntePr4SaeqxBgF7PITED8Q1owqKfyGtdRaO17g) ![zoomable](https://images.vocus.cc/b6ce539b-dd62-4e49-b69c-1678a43a8456.png)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F3063fe0d-c6a9-4c94-9719-3c2396238c93.png&width=740&sign=Kb0lJKJkLcbnX4XzyHO2WxuE1XyvrWAYTmSmuLXXnuI) ![zoomable](https://images.vocus.cc/3063fe0d-c6a9-4c94-9719-3c2396238c93.png)

****4.選擇Local Managed Server**** (例如C:\\krita\_ai\_diffusion\\server) [https://docs.interstice.cloud/installation/](https://docs.interstice.cloud/installation/)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fa640b218-90f6-4a48-bac1-db21f6b1a52f.png&width=740&sign=5MX434i_kkxsqvioEC25QyE67_GNzqhOv22Vhf7pKn4) ![zoomable](https://images.vocus.cc/a640b218-90f6-4a48-bac1-db21f6b1a52f.png)

****5.把\\server\\ComfyUI\\models裡的檔案移到\\server\\ComfyUI\\ComfyUI\\models**** (例如C:\\krita\_ai\_diffusion\\server\\ComfyUI\\models) ****\->**** (C:\\krita\_ai\_diffusion\\server\\ComfyUI\\ComfyUI\\models)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4cc17bc9-43d6-418d-88fd-24c14f82b678.png&width=740&sign=rOJr7vMWY1AEzmqJaCuSoJ71u8CJaS3ggSHuRDAcDtA) ![zoomable](https://images.vocus.cc/4cc17bc9-43d6-418d-88fd-24c14f82b678.png)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fdc07837a-0562-4aa0-82a8-bfd38d4cc094.png&width=740&sign=vuFSfCjFHRQv0SOJJgE1eR7rASk5Yp4huC_1QBaSdsI) ![zoomable](https://images.vocus.cc/dc07837a-0562-4aa0-82a8-bfd38d4cc094.png)

****6.安裝Git**** [https://git-scm.com/downloads](https://git-scm.com/downloads)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fc4e6078c-ca69-4271-a3fb-f0eb919758a9.png&width=740&sign=izSjZAq2PiHrpx9XzPp4hjpLXpSabbLnGVPyFGf2kKo) ![zoomable](https://images.vocus.cc/c4e6078c-ca69-4271-a3fb-f0eb919758a9.png)

****7.安裝ComfyUI**** (例如C:\\ComfyUI) [https://www.comfy.org/zh-cn/](https://www.comfy.org/zh-cn/)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F28ff4be5-84f5-4acf-9590-f1d56e44b274.png&width=740&sign=r26fnLiz9RRAGMFJTuNOEosmmHetxsc9uHdutghpkM8) ![zoomable](https://images.vocus.cc/28ff4be5-84f5-4acf-9590-f1d56e44b274.png)

****8.從現有安裝遷移**** (例如C:\\krita\_ai\_diffusion\\server\\ComfyUI\\ComfyUI)

****9.ComfyUI設定 -> 端口：要监听的端口->8188****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F1d9880e2-6014-465d-9698-d5062bb7bcef.png&width=740&sign=0XZt4WUNcO-1UmFJnCFOUbAYpLuOZJ5qHR0w8Z3QNNI) ![zoomable](https://images.vocus.cc/1d9880e2-6014-465d-9698-d5062bb7bcef.png)

****10.krita-ai-diffusion選擇Custom Server -> Server URL：127.0.0.1:8188****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F9001e419-c5ff-4c3f-b4d3-996fd310e86a.png&width=740&sign=C6rbAxdcrJfmcLBK4_y1foN4hTSYqQrzqu5yXv2eR0M) ![zoomable](https://images.vocus.cc/9001e419-c5ff-4c3f-b4d3-996fd310e86a.png)

****11.下載Stable Diffusion 3.5 -> 放到checkpoints**** [https://huggingface.co/stabilityai/stable-diffusion-3.5-large/blob/main/sd3.5\_large.safetensors](https://huggingface.co/stabilityai/stable-diffusion-3.5-large/blob/main/sd3.5_large.safetensors)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F7d7e4c8f-cc7a-4cc2-aedc-bf8b6f03309c.png&width=740&sign=kFmakLE6mrGjif4Pemyq_A5_vrNLDe7w5MEOSKaURXE) ![zoomable](https://images.vocus.cc/7d7e4c8f-cc7a-4cc2-aedc-bf8b6f03309c.png)

****12.下載clip\_l -> 放到clip**** [****https://huggingface.co/calcuis/sd3.5-large-gguf/blob/main/clip\_l.safetensors****](https://huggingface.co/calcuis/sd3.5-large-gguf/blob/main/clip_l.safetensors)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Faf3bd74e-5f90-47bf-833b-4ce4906d960f.png&width=740&sign=bjLejU9MCsZ_4iRoX3jw1lk43EJK3zZhiEOHbMwOIS8) ![zoomable](https://images.vocus.cc/af3bd74e-5f90-47bf-833b-4ce4906d960f.png)

****13.下載clip\_g -> 放到clip**** [****https://huggingface.co/calcuis/sd3.5-large-gguf/blob/7f72f2a432131bba82ecd1aafb931ac99f0f05f7/clip\_g.safetensors****](https://huggingface.co/calcuis/sd3.5-large-gguf/blob/7f72f2a432131bba82ecd1aafb931ac99f0f05f7/clip_g.safetensors)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F61711cfc-e30a-410e-9e8c-cf5f86622f9f.png&width=740&sign=GUEMqwDVGVUOylFi1R7hLl5tT5WUNC0Rd0W-8o4ObLU) ![zoomable](https://images.vocus.cc/61711cfc-e30a-410e-9e8c-cf5f86622f9f.png)

****14.下載FLUX -> 放到checkpoint**** [****https://civitai.com/models/618692?modelVersionId=691639****](https://civitai.com/models/618692?modelVersionId=691639)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F516f9be6-9543-43e2-9be0-76559be4dd55.png&width=740&sign=s0ryTWorVLyIbvJMBMcqr18zqoh3hBIrl_JNbTG8eY8) ![zoomable](https://images.vocus.cc/516f9be6-9543-43e2-9be0-76559be4dd55.png)

****15.安裝ComfyUI-Manager(推薦方法1)**** [****https://github.com/Comfy-Org/ComfyUI-Manager****](https://github.com/Comfy-Org/ComfyUI-Manager)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4a9959b9-f72f-49d8-9044-997242a37688.png&width=740&sign=bOYEmZ5rlKTlbsXoBqMGT1BYlG_b91jfyJsP6P-8Sb4) ![zoomable](https://images.vocus.cc/4a9959b9-f72f-49d8-9044-997242a37688.png)

****16.打開ComfyUI -> ComfyUI-Manager -> Custom Nodes Manager****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fd524d479-e3c3-4a9d-98f6-d85ffaa00e42.png&width=740&sign=0hZOz435--I9NC0BNEJw681EAHMmuEBXy_Y-HEBa55A) ![zoomable](https://images.vocus.cc/d524d479-e3c3-4a9d-98f6-d85ffaa00e42.png)

****17.安裝(1)comfyui\_controlnet\_aux****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F11b4c254-65c2-41f4-aee6-634617f14706.png&width=740&sign=i1SZt_fz1Zb_ozJGuhb3Kk62w4D1i5TrGnSckwUMdyQ) ![zoomable](https://images.vocus.cc/11b4c254-65c2-41f4-aee6-634617f14706.png)

****18.安裝(2)ComfyUI\_IPAdapter\_plus****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fe0884cd4-5603-4ead-89a7-7bd50c95c24d.png&width=740&sign=fN4IwF2uo0ud5s5fCYA9bugLCkRBrfq68nbuofN_Slg) ![zoomable](https://images.vocus.cc/e0884cd4-5603-4ead-89a7-7bd50c95c24d.png)

****19.安裝(3)comfyui-inpaint-nodes****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fa4ce1e86-0df1-4805-b23d-d800a0825299.png&width=740&sign=y_y-B9vmpYKSduHfTlyVEaqsq3JOoLs2u_V9SCOLC1Y) ![zoomable](https://images.vocus.cc/a4ce1e86-0df1-4805-b23d-d800a0825299.png)

****20.安裝(4)comfyui-tooling-nodes****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Fb408bb14-8a12-4d73-a3c6-0928d1141d5d.png&width=740&sign=_ecKl1KAo-7DxtwDYjL99IvkQUxR4N-Hi66RphkPWG4) ![zoomable](https://images.vocus.cc/b408bb14-8a12-4d73-a3c6-0928d1141d5d.png)

****21.關閉ComfyUI再開啟****

****22.krita-ai-diffusion按下Connect****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Feb896ab6-c622-4240-afc6-604e2e70fb8b.png&width=740&sign=qOQNDVUcqYLfwvGeiorazUOnD227wg68HYrnXPiBzXo) ![zoomable](https://images.vocus.cc/eb896ab6-c622-4240-afc6-604e2e70fb8b.png)

****23.製作工作流****

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F135a03fc-0350-47b0-9aa5-59bb4e365070.png&width=740&sign=g6fUDsBobufWO09XtXfu-izGtEHnZ2p6ye9-RLgAbOc) ![zoomable](https://images.vocus.cc/135a03fc-0350-47b0-9aa5-59bb4e365070.png)

教學工作流下載 [https://drive.google.com/file/d/1xcst6KMrvzE0ULjcvFT-c4JDE0KJGO5j/view?usp=sharing](https://drive.google.com/file/d/1xcst6KMrvzE0ULjcvFT-c4JDE0KJGO5j/view?usp=sharing)