---
title: "ComfyUI 安裝指南"
source: "https://tw-blog.reroom.ai/ai-comfyui-installation"
author:
  - "[[Wayne Huang]]"
published: 2025-02-27
created: 2026-07-24
description: "學習如何安裝ComfyUI。Learn how to install ComfyUI for AI interior design workflows, covering hardware requirements and a step-by-step installation guide"
tags:
  - "clippings"
status: processed
---
![AI 室內設計工作流教學（二）：安裝ComfyUI](https://tw-blog.reroom.ai/_next/image?url=https%3A%2F%2Fcdn.hashnode.com%2Fres%2Fhashnode%2Fimage%2Fupload%2Fv1740617871506%2F48b2dc2b-92ea-4571-9ae4-a2b8f022f5c0.png&w=3840&q=75)

大致上暸解了ComfyUI和Krtia是什麼了之後、能做什麼之後，我們就可以開始ComfyUI的安裝。讀者可以再次確認以下ComfyUI的硬體需求：

---

### ComfyUI 硬體需求

要跑動ComfyUI最重要的一個零組件就是 **GPU，也就是顯卡。**

從ComfyUI官方推薦的頁面來看，近10年內的Nvidia顯卡都可以跑（10系～最新的50系），但用低階顯卡跑的話單張圖的生成時間會變的很長。官方建議是 ==Nvidia 30系顯卡的中高階顯卡以上、VRAM越高越好== 。AMD和Intel顯卡的話會有額外的限制和安裝方式，在這邊筆者並不推薦。

關於更詳細的資訊，請參考官方的顯卡天梯：

[**https://github.com/comfyanonymous/ComfyUI/wiki/Which-GPU-should-I-buy-for-ComfyUI**](https://github.com/comfyanonymous/ComfyUI/wiki/Which-GPU-should-I-buy-for-ComfyUI)

---

## 安裝流程

## Git 安裝

### Window中安裝git

首先，我們先安裝git，Windows 使用者請到以下網址下載最新版本: [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win) ，下載後開啟下載檔案，一路點擊下一步即可。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741251547796/2a8c302a-4a44-4cfd-bb21-4500e95b5a2d.jpeg)

### Mac 中安裝Git

可以參考 [https://git-scm.com/downloads/mac](https://git-scm.com/downloads/mac) 的安裝方式，以下做簡單說明：

Ｍac使用者請先安裝 Homebrew，請先到 [https://brew.sh/](https://brew.sh/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741248314804/006cb39b-fa69-4362-9fba-7ef430d32536.png)

複製Install Homebrew 下方的這一行程式碼（按下右側的文件圖標複製）

並貼到Terminal / 終端機，按下Enter執行

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

在過程中會提示輸入密碼

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741251090051/45a0193f-31c5-408a-895e-de4942561567.png)

請輸入mac的密碼（登入時使用的密碼），輸入時候密碼不會顯示，輸入完按下Enter鍵即可，在過程中，需要確認的話就按下enter鍵，耐心等待安裝完成（跳出install complete）。

接下來會跳出這一段訊息：

```bash
==> Next steps:
- Run these two commands in your terminal to add Homebrew to your \`$PATH\`:
    (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> ~/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
```

直接複製這兩行執行(這邊的 ～ 會被你電腦的路徑取代，這邊只是示意)

```bash
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

執行完後用以下程式碼確認，應該會顯示你的版本。

```bash
brew --version
```

安裝完後，請在Terminal/終端機中輸入 `brew install git` ，按下enter確認，即可成功安裝git。

```bash
brew install git
```

---

### 安裝ComfyUI

現在的ComfyUI有很多種的安裝方式，但這邊推薦直接使用最新也最簡單的安裝方法。

**首先我們先到官方的github主頁：** [**https://github.com/comfyanonymous/ComfyUI#installing**](https://github.com/comfyanonymous/ComfyUI#installing) **。**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740380475253/5e035c91-4c47-4c00-aff2-db84f9e3f580.png)

Github 簡單來說就是一個儲存程式碼的空間，許多工程師都會利用這個網站管理存放他們的程式碼，同時也有很多開源的專案會分享到github上，這些含有程式碼的空間被稱為Repo（儲存庫）。

每個Repo底下的說明欄通常都會說明安裝方式，我們可以下滑到Get Started欄位來看一下安裝方式：

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740366947238/af88df4d-33a1-49a1-a08a-c0b9812c20f3.png)

在Get Started 這欄，我們可以看到很多的安裝方式：

- **桌面版 - 一般的安裝程式，使用者只需要依照流程操作即可完成安裝，最方便安裝的版本。**
- Portable 版 - 便攜版，也就是不用安裝的方式，但需要自行設定環境。
- 手動安裝 - 有一些基礎的朋友可以直接手動安裝，直接透過指令下載。

**這邊選擇最簡單的方式，直接點擊Desktop Application，這個連結會連到：** [ComfyUI主頁](https://www.comfy.org/download) 。其他的安裝方式，因為牽涉的步驟較多，所以不另外說明，網路上有其他的教學影片可以參考。

在主頁面直接依據你的作業系統選擇相應的下載，並且依照步驟安裝：

- Windows - 需要Nvidia顯卡
- Mac - 需要Apple silicon，也就是M系列的CPU

### Mac 安裝步驟

筆者手邊的電腦是M4 Macbook pro，因此會拿Mac的版本來做說明，windows版本的安裝程序也基本相同：

點開下載的.dmg，後熟悉的Mac安裝介面，拖拉ComfyUI到你的應用程式即可，接著直接點擊Icon開啟ComfyUI。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740449169138/0a5b2b16-b50b-4982-86cf-bdfd3d597fd6.png)

首次點擊會進入歡迎頁面、安裝頁面，直接按Get Started開始。

這邊是選擇GPU的部分，就按照你有的GPU下去選擇即可，這邊用的是Mac，如果是windows的話通常就選擇Nvidia顯卡，這邊筆者就按照安裝程式的預設選擇。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740449360959/6c63b824-76ac-4f41-b2cd-7bd0ffcab33f.png)

安裝路徑：選擇好你要的安裝路徑即可。

這頁問的是之前是否有安裝過ComfyUI，有的話可以設定好路徑，就可以使用先前下載過的檔案、資料。本篇教學面向的是新手，因此直接按Next跳過即可。

Automatic Updates - 自動安裝 ／ Usage Metrics - 提供使用資訊給ComfyUI來改進產品

這兩個選項是預設選取的，如果不想自動更新、提供的話就直接關掉即可。Mirror Settings裡面是安裝過程中會安裝的其他相關程式的下載路徑，如果有偏好的來源可以自行設定，這邊我們使用預設的設定即可。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740450123536/b50958c5-1131-492c-8080-999540e56d09.png)

ComfyUI的安裝程式會幫你設定好需要的環境。

安裝完後會直接開啟ComfyUI的介面（如下圖），這時候應該會跳出錯誤訊息，提示你Ｍissing Models（找不到模型），這時候必須先下載缺少的模型。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740449067763/6d4269c2-b130-41e5-97d3-4b46313209e3.png)

### 文生圖與大模型

以首次安裝的使用者來說，要運行最基本的 **文生圖功能** ，會需要下載一個 **大模型** 。ComfyUI預設下載的是一個SD1.5的模型，這個大模型的下載不是強制性的，也可以自行下載你需要的模型，之後會介紹如何下載並使用模型。目前就直接下載這個ComfyUI預設使用的模型即可。

### 安裝問題

目前新主頁的內容還很少，所以如果在安裝的途中、之後的使用上遇到任何問題的話，ComfyUI的 **Github Repo的Issues分頁** 是一個很好搜尋解答的地方，當然有問題也可以直接發問。

[https://github.com/comfyanonymous/ComfyUI/issues](https://github.com/comfyanonymous/ComfyUI/issues)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740367272078/4bb3d49f-53d5-4456-b2ee-8cd0b24f747e.png)

---

ComfyUI的安裝完成後，我們就可以開始ComfyUI的介面和更細緻的介紹了，礙於篇幅問題，ComfyUI的其他內容會擺到下一篇教學。在下一篇教學，我們將會學習ComfyUI的 **介面、節點、各種模型的知識，並開始利用提示詞生成圖片。**

**請點擊下方連結到AI 室內設計工作流（三）：**

[https://tw-blog.reroom.ai/ai-comfyui-workflow](https://tw-blog.reroom.ai/ai-comfyui-workflow)

---

**點擊這個連結：** [**https://tw.reroom.ai/**](https://tw.reroom.ai/) **或者直接搜尋ReRoom AI就可以找到我們。**

也歡迎追蹤我們的Threads，我們會將最新的功能和使用心得放到上面，分享一些有趣的使用經驗。

---

## [AI設計工作流: Krita 繪圖軟體＋ComfyUI](https://tw-blog.reroom.ai/series/krita-comfyui-design)

Part 2 of 4

Krita + ComfyUI 應用教學

Up next

### [AI 室內設計工作流教學 （三）：ComfyUI實際應用](https://tw-blog.reroom.ai/ai-comfyui-workflow)

初學者指南：理解ComfyUI的基本功能