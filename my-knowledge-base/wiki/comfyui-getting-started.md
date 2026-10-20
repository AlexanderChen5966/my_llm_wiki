---
title: ComfyUI 入門：概念、安裝、使用者定位與節點參數詳解
date: 2026-07-22
tags: [ai/tool, type/how-to]
source: https://www.cc.ntu.edu.tw/chinese/epaper/home/20251220_007505.html
type: article
status: processed
asset_value: 55
---

ComfyUI 把 Stable Diffusion 的複雜生成流程「攤開來」成節點式（Node-based）工作流，用線把積木般的節點連起來就是一個 Workflow。彈性極高、完全本地無生成數量限制，但門檻也高。本條目整合四篇來源，從術語、安裝、使用者定位到節點參數逐層說明。

> 整合來源：①臺大計中電子報 陳俊佑〈AI家家酒【Mac】×【ComfyUI】〉（Mac 環境 + 官方模板逐節點參數）② 痞客邦 小黃老師〈一次搞懂全英文介面與節點術語〉（概念/術語白話入門）③ 方格子 映嶙 YingLin〈Krita AI 與 ComfyUI 工作流安裝教學〉（Windows 安裝 + Krita 前端）④ 方格子 M i o〈誰適合使用 ComfyUI？從零開始生成你的第一張圖〉（使用者定位 + 首張圖）

## T（核心論點）

一般生圖工具（Midjourney / WebUI）像「自動販賣機」——把烹調過程藏起來，簡單但彈性低；ComfyUI 像「透明廚房 / 電路板」——你看得到模型怎麼載入、文字怎麼被解析、圖怎麼算出來，熟練後能做出高度客製化的成果。**只要理解「文字 → 模型運算 → 解碼成圖」這條流程，滿滿的英文術語就會變成肌肉記憶**；而**影響外貌最大的其實是模型 checkpoint，而非提示詞**。

## B（適用場景）

**適用（來源④使用者定位）：** 想「大量」產出創意、愛嘗試各種風格、討厭被框架限制、且帶一點「研究精神」（能自己解版本衝突）的創作者。
**不適用 / 注意：** 只是偶爾娛樂——一般生成平台就夠了。硬體門檻：RTX 12G+ 顯卡、大硬碟（每個 checkpoint 約 10G，Mac 16GB RAM 可跑但效能有限）、快網路。警語（來源①）：小心陷入蒐集模型的癖好，變成「數位模型的墳場」。

## R（四個互補角度）

### 角度一：概念與術語（來源②，白話入門）

**文生圖六步驟**：① Load Checkpoint（載入生圖引擎/畫風底模）② CLIP Text Encode（把提示詞翻成模型聽得懂的語言，分 positive/negative）③ Empty Latent Image（準備指定尺寸的隱藏畫布）④ KSampler（心臟：依參數把雜訊「洗」成圖）⑤ VAE Decode（把半成品 Latent 轉成肉眼可見圖片）⑥ Save Image。**新手常誤會 KSampler 直接吐圖**，其實它輸出的是模型才懂的半成品，一定要接 VAE Decode。

**資料型別（線接不上通常是型別不對）**：MODEL（引擎本體）/ CLIP（文字理解，把提示詞轉數值）/ VAE（潛在格式 ↔ JPG/PNG 互轉）/ LATENT（內部運算半成品）/ CONDITIONING（給模型的指令卡）。
**進階術語**：Load Image（圖生圖起點）/ Inpainting（局部重繪）/ LoRA（微調小模型，像特效濾鏡/服裝包）/ ControlNet（精準鎖姿勢/線稿/光影）。

### 角度二：安裝與前端串接（來源③，Windows + Krita）

用 krita-ai-diffusion 讓繪圖軟體 Krita 讀取 ComfyUI 工作流（23 步教學）重點：
- 裝 Krita 本體 + krita-ai-diffusion 外掛 + Git + ComfyUI（官網 comfy.org）
- 下載模型放對資料夾：SD 3.5 → checkpoints、clip_l / clip_g → clip、FLUX → checkpoint
- ComfyUI 設定監聽 **8188 端口**，Krita 選 Custom Server → `127.0.0.1:8188` 按 Connect
- 用 ComfyUI-Manager 裝四個必備 custom nodes：`controlnet_aux` / `IPAdapter_plus` / `inpaint-nodes` / `tooling-nodes`（裝完關閉再開）

### 角度三：使用者定位與首張圖（來源④，FLUX 實作）

1. 選左側「模板」→ 生成人像建議直接選 **Flux** 模型（人物較少出問題）→ 點 Flux Dev fp8
2. 系統跳出缺少的模型，下載後貼到 `models/checkpoints`，關瀏覽器重開再「執行」
3. 預設會生成一隻獸娘；在正面提示詞（綠框，Positive Prompt）寫 `1 woman, realistic style, Live photography` 執行
4. Flux 系列不需寫負面提示詞（紅色摺疊節點）；`1 woman` 因基礎模型多以歐美外貌訓練會偏歐美，`1 girl` 會偏動漫——再次印證 **checkpoint 比 prompt 更影響外貌**

### 角度四：官方模板逐節點參數（來源①，最深）

從官方 Image Generation 模板起手（首次執行自動下載模型）。KSampler 主要參數：
- `seed`：決定初始雜訊排列（相同模型+seed 產出幾乎一樣的圖）
- `control after generate`：種子變化方式（固定/遞增/遞減/隨機）
- `steps`：去噪計算次數，常見 15–35（太低糊、太高浪費算力，一般 20–30）
- `cfg`：提示詞遵循強度，通常 7–12（一說 5–8）；越高越死板照做、越低 AI 越有創意
- `denoise`：雜訊移除強度，文轉圖固定 1.0；改圖時越高改動越大（0.8）、越低越保留原圖（0.2）
- `sampler_name`：去噪計算方式（Euler / DPM++ 2M / DPM++ SDE 等）
- `scheduler`：去噪速度策略（Karras / Simple / Exponential 等）

Load Checkpoint 輸出 MODEL/CLIP/VAE 供後續節點；Empty Latent 的 width/height 決定尺寸、batch_size 決定一次生成張數；VAE Decode 把潛在表示轉成實際圖像。

## C（關鍵行動）

- [ ] 先判斷自己是不是 ComfyUI 的目標使用者（大量產出+研究精神+夠硬體），否則生成平台就夠
- [ ] 記住六步驟流程與資料型別，線接不上先檢查型別（孔位）是否對應
- [ ] 想省調參時間，優先換 checkpoint 而非只改提示詞（模型對外貌影響最大）
- [ ] Krita 串接記得對齊 8188 端口並裝齊四個必備 custom nodes
- [ ] 節制蒐集模型，避免硬碟變成「數位模型的墳場」

## 相關連結

- [[chatgpt-image-prompts-80]] — 圖像生成提示詞結構公式（雲端工具版），可與本篇本地 ComfyUI 的 CLIP 提示詞互相參照
- [[can-i-run-ai-tool]] — 跑本地模型前先確認 GPU/VRAM 是否足夠，對應本篇的硬體門檻
- [[ideas/ideogram-4-open-source]] — 開源圖像生成模型（可本地部署），與 ComfyUI 生態互補

## 原始摘要

四篇來源合併：① 臺大計中（Mac，官方模板逐節點參數詳解 + 數位模型墳場警語）② 痞客邦小黃老師（純術語/介面白話，資料型別 MODEL/CLIP/VAE/LATENT/CONDITIONING + 文生圖六步驟）③ 方格子映嶙（Windows 安裝 + Krita 前端 + 8188 端口 + 四 custom nodes）④ 方格子 M i o（使用者定位 + Flux Dev fp8 首張真人圖 + checkpoint 比 prompt 更影響外貌）。
