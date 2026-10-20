---
title: "CodeGraph - AI 寫程式省80% Token 的好東東"
source: "https://vocus.cc/article/6a0d4512fd897800017e53db"
author:
  - "[[Scott Hsiao]]"
published: 2026-05-20
created: 2026-06-02
description: "問題邁入 Vide Coding 世界後，寫程式的工作輕鬆了，但也帶來新的困擾：Token 耗盡被迫下班或者課金。最近逛的一個開源專案 CodeGraph 號稱可以節省 80%，解決 Token 耗盡的焦慮，那就讓我們實戰導入測試看看！"
tags:
  - "clippings"
status: processed
---
## 問題

邁入 Vide Coding 世界後，寫程式的工作輕鬆了，但也帶來新的困擾：Token 耗盡被迫下班或者課金。

為什麼會看到廣告

<iframe frameborder="0" src="https://fa325e698b9aa69e7321ee7ada000035.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="728" height="90" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>最近逛的一個開源專案 CodeGraph 號稱可以節省 80%，解決 Token 耗盡的焦慮，那就讓我們實戰導入測試看看！

## 推坑影片

![](https://www.youtube.com/watch?v=B0BZZjzuOpM)

### 官方網站

[https://github.com/colbymchenry/codegraph](https://github.com/colbymchenry/codegraph)

## 原理

https://youtu.be/B0BZZjzuOpM?si=OlFYubp3FFjVdQSu&t=186

CodeGraph 會掃描本地的程式碼建立抽象語法樹 （Abstract Syntax Tree，AST）並存在 DB 中，之後要找哪個函式或者變數就能快速查詢

你可能好奇，那後續變動呢？他有檔案系統監測，有變動就會同步更新資料庫，不需要一直手動更新，完全不費力

然後是 LLM 整合，建立.codegraph 的目錄後，會更新 LLM 對應的參考 MD，如 Claude 的 Claude.MD，告訴 LLM 在需要查詢程式碼時自動使用，完全不需要動腦

![zoomable](https://images.vocus.cc/8f437b7d-81ee-4105-9833-a1ccbb8a3e5d.png)

## 安裝設定方法

首先，安裝工具程式

```javascript
npx @colbymchenry/codegraph
```

接著，在專案目錄中初始化，讓他跟 LLM 深入連結

```javascript
cd your-project
codegraph init -i
```

然後就可以射後不理，讓他在背景運作

## 實用心得

以手上的案例為例，請 LLM 做個對比評側，結果如宣傳所示，真的可以節省 86% 的 Token，真是太棒了！

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F932243b3-3cef-455e-9705-c989dba7ff7a.png&width=740&sign=Z4i6ckJAhRCQi3GUsvnmsv1dvflRGTKcHqu3raPJyx0) ![zoomable](https://images.vocus.cc/932243b3-3cef-455e-9705-c989dba7ff7a.png)

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F0ba07dd8-47fe-41db-890d-876ec5d3a33f.png&width=740&sign=qOEqXiKk-y9VdE4auC_9IdKu4L9k5c27H3hwTGHTC9c) ![zoomable](https://images.vocus.cc/0ba07dd8-47fe-41db-890d-876ec5d3a33f.png)