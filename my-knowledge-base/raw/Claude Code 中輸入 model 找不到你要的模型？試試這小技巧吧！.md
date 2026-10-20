---
title: "Claude Code 中輸入 /model 找不到你要的模型？試試這小技巧吧！"
source: "https://israynotarray.com/ai/20260727/3871780834/"
author:
  - "[[Ray]]"
published: 2026-07-27
created: 2026-07-27
description: "Claude Code 的 `/model` 選單找不到想用的模型？這篇示範如何直接輸入完整模型 ID 切換指定版本，並透過 `.claude/settings.json` 固定預設模型，也提醒你到 Anthropic Model Status 確認模型名稱與停用時間。"
tags:
  - "clippings"
status: processed
---
目錄

![Claude Code /model 找不到模型？直接輸入完整模型 ID 切換](https://images.israynotarray.com/blog/ai/claude-code-toggle-model/claude-code-model-switch-1200.jpg)

Claude Code /model 找不到模型？直接輸入完整模型 ID 切換

## 前言

出了新模型但你不想要使用？可是 /model 指令找不到你要的模型？所以這邊就來教你一個小技巧，讓你可以在 Claude Code 中使用你想要的模型。

## Claude Code 中的 /model 指令

有在使用 Claude Code 的讀者大概都知道可以使用 `/model` 指令來切換模型

![切換模型](https://images.israynotarray.com/blog/ai/claude-code-toggle-model/20260727102039.png)

透過這個列表我們通常會有這些可以選：

基本上大多人都會使用 `Default（recommended）： Opus5` ，但是如果你想要使用其他模型的話就會遇到一個問題，那就是 `/model` 指令找不到你想要的模型。

> [!note] Note
> Note
> 
> `/model` 指令只會列出官方推薦的模型，其他模型就不會列出來。

所以為什麼會有這個問題呢？其實很簡單，模型商會希望你直接使用他們推薦的模型，因為這樣可以確保你使用的模型是最穩定、最適合的版本，所以他們就把其他模型隱藏起來了。

但這也延伸一些問題，最常見的大概是...

> 新模型沒有想像中好用。

以之前的案例來講，當時的 Opus 4.8，一直被說 Opus 4.8 不如 Opus 4.6～4.7，大家都會只想用 Opus 4.6～4.7。

## 小技巧：使用 /model 指令切換模型

沒錯，我們還是要使用 `/model` 指令來切換模型，這邊就來教你一個小技巧，讓你可以在 Claude Code 中使用你想要的模型。

假設你還是想要使用 Opus 4.8 的話，你可以先進入 Claude Code 中，然後在對話視窗中輸入...

```bash
/model claude-opus-4-8
```

![切換模型](https://images.israynotarray.com/blog/ai/claude-code-toggle-model/20260727103450.png)

而這些模型名稱你必須正確的去輸入，這些都在官方文件的「 [Model Status](https://platform.claude.com/docs/en/about-claude/model-deprecations#model-status) 」中可以找到。

上面也都有明確寫清楚這些模型什麼時候會停止支援，所以如果你想要使用的模型已經停止支援的話，那就建議找別的使用。

![Model Status](https://images.israynotarray.com/blog/ai/claude-code-toggle-model/20260727103344.png)

Model Status

如果你不希望每次 Claude Code 更新就偷偷把你的模型更新的話，你可以在你的 `.claude/settings.json` 中加入以下設定：

```json
··

{
  "model": "claude-opus-4-8"
}
```

> [!note] Note
> Note
> 
> 需要注意 settings.json 的權重，系統設定 > 使用者設定。

那麼以上就是幫你固定的一個小技巧哩～

## 相關文章[2026-07-14 閱讀時間約 6 分鐘](https://israynotarray.com/ai/20260714/5005075303/)

[AI 明明在雲端，為什麼開越多 Agent 電腦越 Lag？](https://israynotarray.com/ai/20260714/5005075303/)[

閱讀時間約 2 分鐘

新版 Claude Code 終端機攔截滑鼠點擊超煩！用 CLAUDE\_CODE\_DISABLE\_MOUSE\_CLICKS 環境變數把它關掉](https://israynotarray.com/ai/20260629/2995176290/)[

閱讀時間約 7 分鐘

design.md？如果你使用 AI 畫面總是跑掉，那肯定是因為你沒導入 Google 的 design.md](https://israynotarray.com/ai/20260627/1947382651/)