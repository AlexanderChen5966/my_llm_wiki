---
title: "在 colab 上顯示: Hugging Face API Token 沒有設定 – Max的程式語言筆記"
source: "https://stackoverflow.max-everyday.com/2025/03/colab-hugging-face-api-token/"
author:
  - "[[max-stackoverflow]]"
published: 2025-03-01
created: 2026-05-18
description:
tags:
  - "clippings"
---
你需要上 Hugging Face 官網註冊一個帳號, 在帳號的設定的地方, 可以建立 access token, 註冊(sign up) 傳送門:  
[https://huggingface.co/](https://huggingface.co/)

---

## 解決方法

### 方法 1：忽略警告（如果模型是公開的）

`dreamlike-art/dreamlike-photoreal-2.0` 是 **公開模型** ，所以你可以 **忽略這個警告** ，直接執行下載：

```
from diffusers import StableDiffusionPipeline
import torch

MODEL_NAME = "dreamlike-art/dreamlike-photoreal-2.0"

pipe = StableDiffusionPipeline.from_pretrained(
    MODEL_NAME, 
    torch_dtype=torch.float16
).to("cuda")

print(" 模型加載完成！")
```

**如果執行成功，那麼你不需要 API Token！**

---

### 方法 2：手動輸入 Hugging Face API Token

如果你要存取 **受限模型** 或 **加快 Hugging Face 下載速度** ，可以手動輸入 API Token：

```
from huggingface_hub import login

#  這裡手動輸入 API Token
login(token="你的 Hugging Face API Token")
```

**API Token 取得方式：**

1. 前往 [Hugging Face 設定頁面](https://huggingface.co/settings/tokens)
2. 生成 **Access Token** （建議選擇 `write` 權限）
3. **複製 Token** ，然後貼到 `login(token="你的 Token")`

**注意**

- 這樣做每次啟動 Colab 都要重新輸入 Token
- 但它可以加快 Hugging Face 下載速度

---

### 方法 3：在 Colab Secrets 儲存 Token（避免手動輸入）

如果你不想每次輸入，可以 **將 API Token 存到 Colab Secrets** ：

```
import os
from huggingface_hub import login

# 設定 Token
os.environ["HF_TOKEN"] = "你的 Hugging Face API Token"

# 直接從環境變數登入
login(token=os.environ["HF_TOKEN"])
```

這樣，你可以在 **多個 Colab Notebook 中重複使用這個 Token** 。

---

## 結論

- **公開模型** → **可以直接忽略這個警告**
- **受限模型或加快 Hugging Face 下載** → **使用 `login(token="你的 Token")`**
- **在 Colab 儲存 Token，避免重複輸入** → **使用 `os.environ["HF_TOKEN"]`**

這樣，你就能 **順利加載 `dreamlike-art/dreamlike-photoreal-2.0`** 並開始推理